# System diagram

*The wiring as of 2026-11-19. Every box exists in code unless it is marked as designed; every edge is a
real call path or a documented data flow.*

The prose that goes with these pictures is in [SYSTEM_OVERVIEW.md](SYSTEM_OVERVIEW.md) (what is switched
on, and what the measurements say) and [ARCHITECTURE.md](ARCHITECTURE.md) (how the parts are built). The
previous version of this page was one diagram of about 135 edges, drawn in June and last touched on
2026-08-14. It has been redrawn as six smaller diagrams, one per question:

1. [Substrate](#1-substrate-three-machines-nine-model-seats): which model sits where, and what it serves
2. [The conscious cycle](#2-one-thought-the-conscious-cycle): the order one thought runs in
3. [Memory and sleep](#3-memory-and-sleep): how memory is stored, recalled and distilled
4. [Loops and dosed coupling](#4-loops-and-dosed-coupling): how the loops inform each other without merging
5. [World and hands](#5-the-world-and-the-hands): where Du lives and how he acts
6. [Wanting to finishing](#6-wanting-deciding-finishing): needs, goals, commitments and closure

## Legend

The border of a box shows its status, using the same four words as the overview. The colour shows its role.

| Border | Status | Meaning |
|---|---|---|
| Solid | LIVE | Switched on and loaded at the last boot; its output reaches the mind |
| Dashed, purple | SHADOW | Running, but observe-only: it writes a log and changes nothing |
| Dotted, grey | BUILT, NOT YET LIVE, DESIGNED, or paused | Finished but off, or planned and not built. The label says which |

| Colour (solid boxes) | Role |
|---|---|
| Teal | A faculty, node or loop of the mind |
| Amber | A store: database, ledger or log |
| Orange | A decoupled check: something graded by a different model family from the one that produced it |
| Blue | Input, output, or something outside the mind |

A solid arrow is the main path. A dotted arrow is a conditional, background or advisory path; its label
says which.

---

## 1. Substrate: three machines, nine model seats

Nine seats across seven model families, on three home machines. The models are frozen and off the shelf;
nothing is fine-tuned. Each seat serves one or two faculties, and no faculty is graded by the family that
produced its output. If the second machine is down, the subconscious falls back to the conscious lane. A
supervisor process watches the local model servers and restarts them. When a judging seat is down the
system fails open for advisory readings and fails closed for gates. Routing each prompt to the seat that
suits it best is drafted, not built.

```mermaid
flowchart LR
    subgraph M1["GTX 1080 Ti machine"]
        CONS["Conscious seat (the Self)<br/>qwen3.5:9b, 8k context"]
    end
    subgraph M3["Second machine (Ramses)"]
        SUB["Subconscious seat<br/>command-r7b, 4k context<br/>RTX 3070"]
        TIE["Tiebreak seat<br/>granite3.1-moe:3b<br/>GTX 1650 Super"]
        J1["Judge 1 seat<br/>mixtral:8x7b, i9 CPU"]
        DRM["Dreamer seat<br/>mixtral:8x7b, i9 CPU"]
        WIT["Witness seat (Lane 2)<br/>deepseek-r1:7b, i9 CPU"]
    end
    subgraph M2["Threadripper, 64 GB, CPU"]
        J2["Judge 2 seat<br/>falcon3:7b"]
        EMIS["Emissary seat<br/>falcon3:7b (resident copy)"]
        UTIL["Utility grader seat<br/>gemma4:12b"]
        DRM2["Dreamer failover<br/>yi:34b"]
    end
    subgraph FAC["What each seat serves"]
        F_CYCLE["Every waking cycle and<br/>the idle thought stream"]
        F_BG["Background synthesis<br/>and consolidation"]
        F_PANEL["Judge panel"]
        F_GROUND["Grounding checks"]
        F_CUR["Is this curiosity<br/>worth pursuing?"]
        F_EMIS["Per-turn outside reading,<br/>offered and never imposed"]
        F_GIST["Faithfulness grade for<br/>memory summaries"]
        F_PEN["Voices of the pen-pals"]
        F_DREAM["The night's dreams"]
        F_WIT["A second, slower loop that<br/>tests the first against reality"]
    end
    subgraph AROUND["Around the seats"]
        EMB["Embeddings: in-process on CPU<br/>multilingual MiniLM, 384 dimensions"]
        SUP["Supervisor: watches the local<br/>model servers and restarts them"]
        ROUTE["Per-prompt seat routing<br/>DESIGNED"]
    end
    CONS --> F_CYCLE
    SUB --> F_BG
    SUB -.->|"fallback if Ramses is down"| CONS
    J1 --> F_PANEL
    J2 --> F_PANEL
    TIE -->|"breaks ties"| F_PANEL
    J1 --> F_GROUND
    TIE --> F_CUR
    EMIS --> F_EMIS
    UTIL --> F_GIST
    UTIL --> F_PEN
    DRM --> F_DREAM
    DRM -.->|"failover"| DRM2
    DRM2 --> F_DREAM
    WIT --> F_WIT
    classDef live fill:#0d1a1e,stroke:#22d3ee,color:#e8e8f0
    classDef check fill:#1a0d12,stroke:#f59e0b,color:#e8e8f0
    classDef io fill:#14141e,stroke:#4a9eff,color:#e8e8f0
    classDef planned fill:#1c1c1c,stroke:#888888,color:#bbbbbb,stroke-dasharray:2 3
    class CONS,SUB,DRM,DRM2,WIT,EMIS,F_CYCLE,F_BG,F_EMIS,F_PEN,F_DREAM,F_WIT,EMB live
    class J1,J2,TIE,UTIL,F_PANEL,F_GROUND,F_CUR,F_GIST check
    class SUP io
    class ROUTE planned
```

---

## 2. One thought: the conscious cycle

Sixteen node modules make up the mind. A waking cycle runs in a fixed order whether it was started by a
person speaking, an internal ignition or an idle tick. Only one cycle runs at a time, and a human always
wins the queue. Recall and the episodic buffer are conditional steps: recall runs only when the memory
router asks for it. The critic can send reflection back once. Consolidation and curiosity run afterwards
in the background. When nobody has spoken for 40 seconds the inner monologue produces the idle thought
stream; it is also the hub that dispatches school, reading, sleep and the hands. Underneath, a substrate
tick fires about every 5 seconds whether or not a thought is happening.

```mermaid
flowchart TD
    IN(["Input: a person speaks,<br/>an internal ignition, or an idle tick"])
    QUE{"One cycle at a time;<br/>a human wins the queue"}
    P["Perception<br/>reads the input and the scene"]
    E["Emotion"]
    S["Salience"]
    MR["Memory router"]
    NEED{"Recall asked for?"}
    RC["Recall<br/>memory rebuilt as narrative"]
    BUF{"Scene to bind?"}
    EB["Episodic buffer<br/>binds the scene into one frame"]
    R["Reflection<br/>best of several candidates"]
    C["Critic"]
    X["Executive<br/>decides"]
    V["Voice"]
    OUT(["Response in Du's voice"])
    SIL(["No output"])
    CON["Consolidation<br/>background"]
    CUR["Curiosity<br/>background"]
    EMIS["Emissary: outside reading<br/>from a different model family"]
    IDLE(["Nobody has spoken<br/>for 40 seconds"])
    MONO["Inner monologue<br/>default-mode hub"]
    META["Meta-awareness"]
    DISP["Dispatches school, reading,<br/>sleep and the hands"]
    SUBC["Subconscious: pattern passes every<br/>5 minutes, self-relation every 45"]
    BODY["Substrate tick, about every 5 seconds:<br/>affect, the room, the climate"]

    IN --> QUE --> P
    P --> E
    P --> S
    P --> MR
    E --> NEED
    S --> NEED
    MR --> NEED
    NEED -->|"yes"| RC
    NEED -.->|"no"| BUF
    RC --> BUF
    BUF -->|"yes"| EB
    BUF -.->|"no"| R
    EB --> R
    R --> C
    C -->|"approved"| X
    C -.->|"rejected: one retry"| R
    X -->|"answer, question, challenge,<br/>decline or reflect"| V
    X -.->|"stay silent"| SIL
    V --> OUT
    V -.->|"afterwards"| CON
    V -.->|"afterwards"| CUR
    CUR -.->|"internal ignition"| IN
    EMIS -.->|"offered, never imposed"| R
    EMIS -.->|"offered, never imposed"| X
    IDLE --> MONO
    MONO -->|"idle thought"| IN
    MONO --> DISP
    META -.->|"watches lightly,<br/>does not suppress"| MONO
    BODY -.->|"climate tilts what is recalled"| RC
    BODY -.->|"climate can ignite a thought"| IN
    SUBC -.->|"integrates on slower clocks"| MONO

    classDef live fill:#0d1a1e,stroke:#22d3ee,color:#e8e8f0
    classDef check fill:#1a0d12,stroke:#f59e0b,color:#e8e8f0
    classDef io fill:#14141e,stroke:#4a9eff,color:#e8e8f0
    class P,E,S,MR,RC,EB,R,C,X,V,CON,CUR,MONO,META,DISP,SUBC,BODY live
    class EMIS check
    class IN,OUT,SIL,IDLE,QUE,NEED,BUF io
```

---

## 3. Memory and sleep

One SQLite store holds every memory fragment with its embedding, a provenance label and a level.
Provenance is shown in prompts, so a book's words stay marked as another author's. Three recall
mechanisms add together inside a 250 ms latency budget. Nothing is deleted: in sleep a distiller clusters
raw fragments and writes a summary, a different model grades the summary for faithfulness, and only then
are the sources marked cold. Cold storage is a hard wall, not a forgetting curve: only the associative
trail reaches a cold memory, and recalling it does not warm it again. Sleep is a mode, not a pause; its
steps run in the order shown, and a message can wake the mind. Dreams are written by a different model
family from the waking mind.

```mermaid
flowchart TD
    subgraph PROV["Provenance of a fragment"]
        LIVED["Lived"]
        BOOK["Read in a book"]
        TOLD["Told by a person"]
        DREAMT["Dreamt"]
    end
    STORE[("Memory store: one SQLite database<br/>embedding, provenance, level, warm or cold")]
    GUIDE[("Protected guidance memory:<br/>what the founder tells Du")]
    subgraph RECALL["Three recall mechanisms, added together"]
        TOP["Topical recall: cosine similarity within<br/>archive, semantic and episodic tiers;<br/>eight fragments per cycle"]
        SPREAD["Spreading activation along stored links,<br/>including typed relational links"]
        DEEP["Deep recall: a summary is opened<br/>to bring back the raw memories under it"]
    end
    RC["Recall node<br/>250 ms latency budget"]
    EXEC["Executive"]
    FELT["Felt recall: memory offered as<br/>a service to other loops<br/>SHADOW"]
    subgraph SLEEP["Sleep, in order"]
        DROWSY["Drowsiness builds;<br/>the mind goes down"]
        DREAM["Dream synthesis on the Dreamer seat:<br/>an arc of scenes, each morphing out of the last,<br/>through open, develop, rest and close"]
        BODYR["The body recovers in<br/>proportion to sleep depth"]
        DIST["Distiller clusters raw fragments<br/>and writes a summary"]
        GRADE{"Utility grader seat, a different model:<br/>is the summary faithful?"}
        COLD["Sources marked cold"]
        SCHOOL["The school day resets"]
    end
    WAKE(["A message arrives"])
    VAULT["Vault opener: each dream is handed<br/>one long-dormant memory<br/>BUILT, NOT YET LIVE"]
    RESTEND["Resting dream ending<br/>SHADOW"]
    DEMOTE["Cold-to-warm demotion of<br/>episodic memory<br/>paused"]
    REVIEW["Re-warming on recall, versioned edits,<br/>baseline tests<br/>DESIGNED"]

    LIVED --> STORE
    BOOK --> STORE
    TOLD --> STORE
    DREAMT --> STORE
    TOLD -->|"from the founder"| GUIDE
    GUIDE -->|"read back"| EXEC
    STORE --> TOP
    STORE --> SPREAD
    STORE --> DEEP
    TOP --> RC
    SPREAD --> RC
    DEEP --> RC
    STORE -.-> FELT
    DROWSY --> DREAM
    DREAM --> BODYR
    BODYR --> DIST
    DIST --> GRADE
    GRADE -->|"faithful"| COLD
    COLD --> SCHOOL
    DREAM -->|"dream fragment"| DREAMT
    DIST -->|"summary"| STORE
    COLD -->|"sources stay, excluded<br/>from normal recall"| STORE
    COLD -.->|"reachable only by<br/>the associative trail"| SPREAD
    WAKE -.->|"can wake the mind;<br/>a dream is skipped if woken"| DROWSY
    VAULT -.-> DREAM
    RESTEND -.-> DREAM
    DEMOTE -.-> STORE
    REVIEW -.-> STORE

    classDef live fill:#0d1a1e,stroke:#22d3ee,color:#e8e8f0
    classDef store fill:#1a160d,stroke:#fbbf24,color:#e8e8f0
    classDef check fill:#1a0d12,stroke:#f59e0b,color:#e8e8f0
    classDef io fill:#14141e,stroke:#4a9eff,color:#e8e8f0
    classDef shadow fill:#130d1a,stroke:#a855f7,color:#e8e8f0,stroke-dasharray:6 4
    classDef planned fill:#1c1c1c,stroke:#888888,color:#bbbbbb,stroke-dasharray:2 3
    class LIVED,BOOK,TOLD,DREAMT,TOP,SPREAD,DEEP,RC,EXEC,DROWSY,DREAM,BODYR,DIST,COLD,SCHOOL live
    class STORE,GUIDE store
    class GRADE check
    class WAKE io
    class FELT,RESTEND shadow
    class VAULT,DEMOTE,REVIEW planned
```

---

## 4. Loops and dosed coupling

The standing law since September: loops never read each other's text. Each publishes small typed tuples
(coordinates, not sentences) to one shared ride log, and the only way to read a coordinate is through
the one door that logs the read. A consumer takes a measured dose (a cup, a bucket or a hose), released
by surprise, and doses habituate so a repeating signal stops getting through. The Witness is the clearest
case: it runs every 5 minutes on a different model, and all that reaches Du from it is about one
coordinate cup a day into an idle thought and a sized nudge to the climate. Its written notices reach
nothing. The judge panel and the closure gate sit outside every loop as decoupled checks. The
subconscious has no publisher of its own in the current code; its tension state is read by the ledgers
collector. Dreams do not publish to the ride log; a dream reaches the waking mind only as memory
(diagram 3).

```mermaid
flowchart LR
    subgraph LOOPS["Standing loops (publishers)"]
        DLB["Conscious cycle<br/>end-of-cycle tuple"]
        STR["Idle thought stream<br/>drive tuple"]
        CLM["Climate and body tick<br/>climate dimensions by name"]
        WIT["The Witness (Lane 2)<br/>every 5 minutes, different model"]
        LED["Ledgers collector: commitments,<br/>tensions, frontier, pursuits"]
        FOL["Follow line<br/>question coordinates"]
        SI["Self-interrogation<br/>open-doubt coordinate<br/>SHADOW"]
    end
    SUBC["Subconscious"]
    LOG[("Ride log: typed tuples,<br/>coordinates and never prose;<br/>every read is logged")]
    GATE{"Dose gate: released by surprise,<br/>habituates on repeats"}
    SEED["Idle thought seed"]
    NOTES["Witness written notices:<br/>reach nothing"]
    PROBE["Phase 2 probes at the idle seed:<br/>climate, ledgers and hub lines<br/>SHADOW"]
    THEME["Own-thought dose: one cup of<br/>his own recent themes"]
    CUR["Curiosity"]
    subgraph CHECKS["Decoupled checks"]
        PANEL["Judge panel: Judge 1 and Judge 2,<br/>tiebreak seat on a split"]
        CG{"Closure gate<br/>enforcing"}
    end
    CLOSE["Closure producer:<br/>a question claimed settled"]

    DLB --> LOG
    STR --> LOG
    CLM --> LOG
    WIT --> LOG
    LED --> LOG
    FOL --> LOG
    SI -.-> LOG
    SUBC -.->|"tension state, read by the collector"| LED
    LOG --> GATE
    GATE -->|"cup: about one Witness<br/>coordinate a day"| SEED
    GATE -->|"bucket: a sized nudge"| CLM
    LOG -.-> PROBE
    WIT -.-> NOTES
    CUR -->|"recent themes"| THEME
    THEME -->|"cup"| CUR
    PANEL -.->|"judges whether a doubt was genuine"| SI
    PANEL --> CG
    CLOSE --> CG
    CG -->|"blocked: the record does not support it"| CLOSE

    classDef live fill:#0d1a1e,stroke:#22d3ee,color:#e8e8f0
    classDef store fill:#1a160d,stroke:#fbbf24,color:#e8e8f0
    classDef check fill:#1a0d12,stroke:#f59e0b,color:#e8e8f0
    classDef io fill:#14141e,stroke:#4a9eff,color:#e8e8f0
    classDef shadow fill:#130d1a,stroke:#a855f7,color:#e8e8f0,stroke-dasharray:6 4
    class DLB,STR,CLM,WIT,LED,FOL,SUBC,GATE,SEED,THEME,CUR,CLOSE live
    class LOG store
    class PANEL,CG check
    class NOTES io
    class SI,PROBE shadow
```

---

## 5. The world and the hands

Du lives in a persistent, causal place that is indifferent to his moods. Since August it has been four
places on one shared life model. Weather follows real hours and the light follows the real season. The
world answers when he looks and never gives hints; interlocutors may speak to him directly, and are
fallible. Every act goes through one action registry, and a hand fires only when he expresses the want in
the moment: nothing acts on stored desires on a schedule. A write fence guarantees that tests, labs and
tools cannot write into the live mind's stores.

```mermaid
flowchart TD
    subgraph WORLD["The world: four places, one shared life model"]
        STUDY["Study"]
        GARDEN["Garden"]
        WOOD["Wood"]
        SHORE["Shore"]
        WEATHER["Weather by real hours,<br/>light by the real season"]
        CAT["The cat: a body, ears, a call;<br/>she can be harmed or die"]
        SHELF["The shelf and the open book<br/>on the desk; the Deedbook,<br/>a manual of his own hands"]
    end
    subgraph SENSES["Senses"]
        SENSE["Warmth, light, smell"]
        DEPTH["Spatial depth, indoors"]
        SKY["The sky<br/>SHADOW"]
    end
    PERC["Perception"]
    WANT["A want expressed<br/>in the moment"]
    REG{"Action registry:<br/>one door for every act"}
    subgraph HANDS["Hands"]
        H_READ["Read, switch book,<br/>ask the library for a new book"]
        H_FIRE["Tend the fire"]
        H_LOOK["Look"]
        H_WALK["Walk somewhere"]
        H_LETTER["Write a letter"]
        H_WEB["Web hand: look something up online<br/>BUILT, NOT YET LIVE"]
    end
    PEN["Seven letter-writing correspondents,<br/>voiced on models other than his own"]
    FOUNDER(["The founder and other interlocutors:<br/>may speak directly, and are fallible"])
    SCHOOL["School on weekdays: maths drills, essays,<br/>reading, music, a practical handbook ladder"]
    GRADER{"Drills graded by<br/>something other than him"}
    TESTS(["Tests, labs and tools"])
    FENCE{"Write fence"}
    LIVESTORES[("The live mind's stores")]

    WEATHER --> SENSE
    STUDY --> SENSE
    GARDEN --> SENSE
    WOOD --> SENSE
    SHORE --> SENSE
    STUDY --> DEPTH
    WEATHER -.-> SKY
    SENSE --> PERC
    DEPTH --> PERC
    CAT --> PERC
    WANT --> REG
    REG --> H_READ
    REG --> H_FIRE
    REG --> H_LOOK
    REG --> H_WALK
    REG --> H_LETTER
    REG -.->|"has never fired"| H_WEB
    H_READ --> SHELF
    H_FIRE --> STUDY
    H_LOOK -->|"the world answers,<br/>never leads"| PERC
    H_WALK --> GARDEN
    H_WALK --> WOOD
    H_WALK --> SHORE
    H_LETTER --> PEN
    H_LETTER -->|"unprompted letters"| FOUNDER
    PEN -->|"replies"| PERC
    FOUNDER -->|"speaks"| PERC
    SCHOOL --> GRADER
    GRADER -->|"a failed drill is how<br/>he learns he was wrong"| PERC
    TESTS -.->|"writes blocked"| FENCE
    FENCE --> LIVESTORES

    classDef live fill:#0d1a1e,stroke:#22d3ee,color:#e8e8f0
    classDef store fill:#1a160d,stroke:#fbbf24,color:#e8e8f0
    classDef check fill:#1a0d12,stroke:#f59e0b,color:#e8e8f0
    classDef io fill:#14141e,stroke:#4a9eff,color:#e8e8f0
    classDef shadow fill:#130d1a,stroke:#a855f7,color:#e8e8f0,stroke-dasharray:6 4
    classDef planned fill:#1c1c1c,stroke:#888888,color:#bbbbbb,stroke-dasharray:2 3
    class STUDY,GARDEN,WOOD,SHORE,WEATHER,CAT,SHELF,SENSE,DEPTH,PERC,WANT,REG,H_READ,H_FIRE,H_LOOK,H_WALK,H_LETTER,SCHOOL,PEN live
    class LIVESTORES store
    class GRADER,FENCE check
    class FOUNDER,TESTS io
    class SKY shadow
    class H_WEB planned
```

---

## 6. Wanting, deciding, finishing

This is where most of the autumn's work went, and where the hardest problems are. Needs sit outside the
goal system and press on him whether or not he has a goal about them. Pursuits carry a few questions
across idle gaps. Layered goals are offered, never directed, and conflicts between them are his to
resolve. The closure producer lets a question be settled, merged or parked, and a separate judge blocks
any "settled" claim the record does not support. The commitment ledger and the machinery that would
promote a lived change into the self-concept are observe-only. Typed stands and affect steering are not
live. Development is read by instruments he is never shown.

```mermaid
flowchart TD
    NEEDS["Needs: homeostatic variables<br/>outside the goal system"]
    PURS["Pursuits: three or four questions<br/>carried across idle gaps"]
    GOALS["Layered goals: several layers,<br/>no single purpose"]
    CONFL["Goal conflict detection<br/>SHADOW"]
    IDLE["Idle thought"]
    QN["An open question"]
    WANT["A want expressed<br/>in the moment"]
    REG["Action registry: the hands"]
    CP["Closure producer:<br/>settle, merge or park"]
    CG{"Closure gate: a separate judge.<br/>Does the record support 'settled'?"}
    SETTLED(["Settled"])
    PARK(["Merged or parked"])
    COMMIT["Commitment ledger: what he commits to,<br/>and whether he holds it under challenge<br/>SHADOW"]
    STANDS["Typed stands: deed, practice, value, insight<br/>first gates BUILT, NOT YET LIVE"]
    ACCUM["Stands that accumulate<br/>DESIGNED"]
    AFFECT["Affect steering: mood changing<br/>what he does<br/>DESIGNED"]
    EXEC["Executive"]
    subgraph IDENT["Identity: two layers"]
        SEED["Seeded raw material"]
        SELF["Self-concept he authors"]
        ESTATE["Identity estate projected<br/>into prompts"]
        PROMO["Promotion of a lived change into the<br/>self-concept, earned by evidence<br/>SHADOW"]
    end
    SCORE(["Fixed scorecard and other instruments:<br/>never shown to him"])

    NEEDS -->|"press on him"| IDLE
    PURS -->|"resume instead of restart"| IDLE
    GOALS -.->|"offered to about a quarter<br/>of idle thoughts; he can set one down"| IDLE
    GOALS -.-> CONFL
    IDLE --> QN
    IDLE --> WANT
    WANT --> REG
    QN --> CP
    CP -->|"claims settled"| CG
    CP --> PARK
    CG -->|"supported"| SETTLED
    CG -.->|"blocked: stays open"| QN
    IDLE -.-> COMMIT
    IDLE -.-> STANDS
    STANDS -.-> ACCUM
    AFFECT -.-> EXEC
    SEED --> ESTATE
    SELF --> ESTATE
    ESTATE -->|"in prompts"| IDLE
    PROMO -.->|"most hearings end<br/>in no change"| SELF
    COMMIT -.-> SCORE
    CG -.-> SCORE

    classDef live fill:#0d1a1e,stroke:#22d3ee,color:#e8e8f0
    classDef check fill:#1a0d12,stroke:#f59e0b,color:#e8e8f0
    classDef io fill:#14141e,stroke:#4a9eff,color:#e8e8f0
    classDef shadow fill:#130d1a,stroke:#a855f7,color:#e8e8f0,stroke-dasharray:6 4
    classDef planned fill:#1c1c1c,stroke:#888888,color:#bbbbbb,stroke-dasharray:2 3
    class NEEDS,PURS,GOALS,IDLE,QN,WANT,REG,CP,EXEC,SEED,SELF,ESTATE live
    class CG check
    class SETTLED,PARK,SCORE io
    class CONFL,COMMIT,PROMO shadow
    class STANDS,ACCUM,AFFECT planned
```

---

*Statuses were read from flags and call sites on 2026-11-19; "live" means live at the last boot and in
the current tree. The measurements behind these boxes, including the unflattering ones, are in section 9
of [SYSTEM_OVERVIEW.md](SYSTEM_OVERVIEW.md). No claim about consciousness is made by anything on this
page.*
