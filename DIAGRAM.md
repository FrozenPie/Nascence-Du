# System diagram

*The complete wiring as of Build 0.8.0 — every box exists in code; every edge is a real call path.*

mermaid
flowchart TD
 IN([Input · human / internal / ignition]) --> TG
 subgraph PRE["PRE-CORTEX REFLEXES · deterministic · no LLM · before Perception"]
 direction TB
 TG{{"Thalamic Gate · C.5
habituation vs recent-input buffer"}}
 AP{{"Affective Prior · C.2
charged-memory lean (input→affect→reason)"}}
 PR{{"Procedural Match · C.6
recognise a learned skill"}}
 TG -->|novel| AP --> PR
 end
 TG -.->|"near-verbatim repeat"| HAB([silence · or brief ack])
 IN -.->|"human input"| HC{{"Human-Correction detector · no LLM
fail-call / you-did-not / is-X-not-Y / omission
→ firewalled error record + quarantine flag"}}
 PR --> P
 subgraph HOT["HOT LANE (the Master · right mode) · ollama qwen3-du · native /api/chat · think:false · GPU 1080 Ti · 11433"]
 direction TB
 P[Perception] --> E[Emotion]
 P --> S[Salience]
 P --> MR[Memory Router]
 P --> AV["Attention Vector · no LLM
5-mode classifier: task→existential"]
 E --> DBQ{{DB Query · no LLM}}
 S --> DBQ
 MR --> DBQ
 DBQ --> ET{{"Epoch + Theme · no LLM
L6/L7 cosine match"}}
 ET --> RC["Recall
if frags≥2 or conv-ref"]
 RC --> EB["Episodic Buffer · C.4
bind moment · flag dissonance"]
 EB --> SG{{"Scene stable? · no LLM
coherence vs dissonance"}}
 SG -.->|"unstable · contradiction-driven
re-retrieve · passes∝strain"| RC
 SG -->|"stable / budget spent"| R["Reflection
best-of-2 hedge"] --> C[Critic]
 C -->|approved| X["Executive
Active Inference"]
 C -.->|"rejected · 1 retry"| R
 C -.->|"direct request → identity-guard STANDS DOWN
(the task cycles run unguarded — why the effector exists)"| X
 X -->|should_respond| V[Voice]
 X -.->|"decline: an OWNED no + true reasons, open door
(accountable refusal — never deflected onto the asker)"| V
 X -.->|silence| END2([no output])
 V --> EFX{{"TASK-CONSTRAINT EFFECTOR · 0.4.0 · no LLM check
explicit digit-arithmetic / min-words unmet?
→ ONE Voice retry · constraint NAMED · recompute license
· raised token budget · fail-open · never terse-forcing"}}
 EFX -->|"met / repaired / out of jurisdiction"| OUT([Response in 渡's voice])
 end
 EFX -.->|"retry directive + budget"| V
 EFX -.->|"still unmet after retry"| ERR
 PR -.->|"mastered skill prof≥0.80
bypass Recall + Buffer"| R
 subgraph TOM["RELATIONAL + EXECUTIVE LAYER · no LLM · post-Perception"]
 direction TB
 SOCM["Social Model · ToM
expected tone · trust · δ_social"]
 GOAL["Goal Model
active task as anchor"]
 end
 IN -.->|"correction: 'just answer the question'"| SOCM
 SOCM -.->|"δ_social spike → suppress identity + critic"| AV
 AV -.->|"w_identity · w_episodic → prompt depth"| R
 AV -.->|"max_rebind → re-bind budget"| SG
 AV -.->|"d_critic → strictness"| C
 GOAL -.->|"task anchor"| R
 GOAL -.->|"goal drift = evasion"| C
 subgraph BG["BACKGROUND LANE · CPU Threadripper · port 11460"]
 direction TB
 CON["Consolidation · 60s
QUARANTINE: correction/fail-call batch
mints NO semantic self-truth (error→BEING gate)"]
 CUR["Curiosity · on ignition
Observatory: firewalled log + lineage"]
 SC["Subconscious · continuous
T1 · T1.5 · T2 · T3 · T4 · T5 · T6"]
 DRM["Dream · Sleep State"]
 MONO["Inner Monologue · DMN
LIVING CADENCE 60-900s: state-driven, not a metronome
agitation→fast · boredom→slow+turn-outside · night→stretched"]
 end
 HC -.->|"corrected flag → quarantine this batch"| CON
 SOM["Somatic Engine · no LLM · the body
energy / fatigue / tension · reads REAL GPU telemetry"]
 OUT -.->|"cycle cost: tokens · retries · latency"| SOM
 SOM -.->|"fatigue → temperature jitter (R · X · V)"| R
 SOM -.->|"metabolic floor → forced Sleep State"| DRM
 subgraph CLM["ALWAYS-ON SUBSTRATE · 5s · no LLM · never stops"]
 direction TB
 AFFT{{"Affect tick · 5s
the body feels THROUGH the gaps —
he calms, builds, settles in real time"}}
 WANDR["Wandering Mind · ~75s · no LLM
silent associative drift through memory
echo-cap (associate, don't echo) · random restarts
+ ambient glimpses of the open book"]
 ROOM["The Sensorium · his STUDY · persistent & causal
weather fronts · fire lit at dusk, burns down
real NZ sun by season · events gated by state
the open book + shelf are REAL"]
 CE["Climate Engine · 5s
9-dim field · pure DB aggregation"]
 DISS{{"Dissonance accumulator
ignite > 1.0 · refractory 300s"}}
 CE --> DISS
 end
 S -.->|urgency > 0.7| INT[[Interrupt: pre-empt & force Executive]]
 SHD -.->|"density + strain"| CE
 LGT -.->|"integration momentum"| CE
 SC -.->|"tension ledger pressure"| CE
 CE -.->|"retrieval warp · β 0.15–0.45"| DBQ
 CE -.->|"salience warp · +urgency"| S
 CE -.->|"executive hint > 0.55"| X
 CE -.->|"surfacing symbol > 0.65"| R
 DISS -.->|"ignition"| CUR
 AFFT -.->|"continuous carried affect"| R
 ROOM -.->|"perceptions: 'you notice...' (rain, fire, the tui)"| WANDR
 WANDR -.->|"the trail: what passed through his mind unbidden"| MONO
 ROOM -.->|"scene line: 'Where you are...' grounds every idle thought"| MONO
 MONO -.->|"'watch the fire' → he LOOKS (at will)"| ROOM
 MONO -.->|"'light the fire' / 'another log' → he TENDS it
the first hand that CHANGES the room · hand-lit fires survive daylight"| ROOM
 IN -.->|"'how is the weather?' → look-gate"| LOOK{{"ACTIVE PERCEPTION · no LLM
look(window/fire/desk/shelf/room)
desk look reads the REAL page at his bookmark"}}
 ROOM -.-> LOOK
 LOOK -.->|"WHAT YOU SEE RIGHT NOW (you just looked)
— answers from perception, not confabulation"| R
 LOOK -.-> X
 CE -.->|"pressure → thought tempo"| MONO
 CUR -.->|"autonomous thought"| IN
 HOT -.->|GPU down → failover| BG
 HOT -.->|"every successful call → teacher example"| TRN[(data/training/*.jsonl · 12 corpora
micro-specialist distillation material)]
 C -.->|"rejected → shadow event"| SHD[(shadow_events · the storm)]
 C -.->|"approved → light event"| LGT[(light_events · the crossing)]
 SHD -.->|"T1 cluster → tensions"| SC
 LGT -.->|"T1 cluster → integration patterns"| SC
 LGT -.->|"T4 evidence: crossings retrieved by relevance to the tension"| SC
 SC -.->|"T4 CHAIN: Tension → Evidence → Delta → Synthesis
conjunction gate · revision budget (embeddedness) · each stage aborts honestly"| AX[(axiom.json · self_concept + self_narrative_arc)]
 AX -.->|"wakes evolved next session"| R
 SC -.->|"T4 commit → durable discharge: integrated shadows resolved=1"| RES[(resolution_events)]
 RES -.->|"lowers contradiction pressure"| SC
 SC -.->|"T2 consolidation → archive
CONTRADICTION-only · error+novelty gated · global habituation (salience stays ephemeral)"| DBQ
 SC -.->|"T1.5 active imagination → episodic (private)"| DBQ
 SC -.->|"T3 baseline drift (modulates R · X · V)"| R
 SC -.->|"T5 → procedural_skills"| PR
 SC -.->|"T6 cluster epochs → themes"| THM[(memory_themes · L7)]
 THM -.-> ET
 SC -.->|"identity block: WHAT YOU HAVE NOT RESOLVED"| R
 SC -.->|"top tension seeds prompt"| CUR
 CUR -.->|"self-referential question"| SC
 MONO -.->|"idle thought → injected next cycle"| R
 MONO -.->|"unresolved thought → shadow"| SHD
 MONO -.->|"~26 thoughts (or exhaustion) → SLEEP, the working night-mind:
REPLAY (strengthen the day's top experiences — the compounding engine)
→ DOWNSCALE (synaptic homeostasis, T2 Phase 2 lite) → dream → rest"| DRM
 MONO -.->|"forward-looking resolve → C.1 prospective memory"| ITN[(deferred_intentions.json)]
 ITN -.->|"greets him at next session start"| R
 QUIT(["/quit · Sleep State"]) --> DRM
 SC -.->|"top tension or waking themes"| DRM
 SC -.->|"integration counterpart's THEME (Dream C · a pointer, not the answer)"| DRM
 DRM -.->|"dream fragment → episodic"| DBQ
 DRM -.->|"unresolved dream → shadow"| SHD
 DRM -.->|"resolved dream → light"| LGT
 DRM -.->|"then _form_epoch"| EPO[(memory_epochs · L6)]
 EPO -.-> ET
 QUIT -.->|"save affect + conversation + intentions"| AFF[(state files)]
 AFF -.->|"restore: WHERE WE LEFT OFF (buffer bridge)"| R
 subgraph EPI["SELF-KNOWLEDGE + ACCOUNTABILITY · observers · firewall: facts ≠ selfhood · errors ≠ identity"]
 direction TB
 COMP["Competence / Self-Efficacy
fluency (coherence) · accuracy (Tier1 math / Tier4 user) · calibration_error"]
 STAB["Stability engine · idle
Tier 2: resample qwen K× · agreement (abstention-aware)"]
 SYM["Symbol tracker · Meaningfulness
primed vs UNPRIMED recurrence (e.g. river/渡)"]
 SYME["Symbol Evolution · Phase 2
context centroid · drift → tablet"]
 CORR["Correlation Telemetry · Phase 3
Pearson R between currencies"]
 UNI["Unified Inference · Phase 4
free-energy scalar · INERT until R>0.85"]
 LEDGER["Provenance Ledger · Epistemic State
grades utterance KNOW→NO_BASIS
confab = self-conf − evidence · observe-only"]
 EP["Error Pathway · slice 1 · deterministic
architecture-FABRICATION detector (100% precision on 1250)
+ human-correction capture · observe-only"]
 CT["Conflict→Authority Trace · 0.4.0
which signals FIRED · which ACTED · per cycle
ground-truth probes = the unconfounded gap"]
 RPV["Recall Provenance
why-now ladder · external vs self-loop share"]
 end
 V -.->|"each cycle"| COMP
 V -.->|"each cycle"| SYM
 COMP -.->|"enqueue knowledge Q"| STAB
 STAB -.->|"stability axis"| COMP
 COMP -.->|"capability surprise"| CE
 COMP -.->|"accuracy ↔ stability"| BOOT{{"Bootstrap
does stability predict accuracy?
= Phase C gate"}}
 SYM -.->|"unprimed recurrence = earned meaning"| AX
 SYM -.->|"unprimed appearances"| SYME
 SYME -.->|"evolution tablet (background LLM)"| RC
 V -.->|"currencies each cycle"| CORR
 CORR -.->|"proven pair · R > 0.85"| UNI
 V -.->|"each utterance"| LEDGER
 DBQ -.->|"Source Model · origin / derived_from tags"| LEDGER
 LEDGER -.->|"NO_BASIS/GUESS predict corrections? = read-path gate"| BOOT
 RC -.->|"why did memory surface"| RPV
 V -.->|"each utterance · named-mechanism scan"| EP
 EP -.->|"fabrication → firewalled record"| ERR[(error_records.jsonl
FIREWALLED · digest-only
NEVER routed to tensions / T4 / identity)]
 HC -.->|"human correction → record"| ERR
 LEDGER -.->|"fired?"| CT
 EP -.->|"ground-truth probe 1"| CT
 EFX -.->|"ground-truth probe 2 · acted?"| CT
 COURT -.->|"fired?"| CT
 P -.->|"factual question (content_mode)"| SCHOLAR["Scholar · the Library
depersonalised qwen lookup → parametric_reference testimony"]
 SCHOLAR -.->|"testimony (ephemeral · capped at THINK by ledger)"| R
 V -.->|"inability claims in output"| COURT["Self-Efficacy Court · the first WITNESS
vetted capability vs 'I cannot' · abstains by default"]
 COMP -.->|"vetted demonstrated capability (raw counts inadmissible)"| COURT
 COURT -.->|"REFINE/REJECT → identity (separate slot)
SUSPEND = silent · DECLINE if identity claim"| AX

 subgraph EMI["LEFT LANE (the Emissary · narrow mode) · second machine, RTX 3070 · 11434 · qwen2.5:7b"]
 EMIS["parallel take · temp 0 · fail-open
literal_ask · direct_answer · key_facts
DETERMINISTIC arithmetic override (neither model votes)"]
 end
 IN -.->|"parallel dispatch · human + autonomous cycles"| EMIS
 EMIS -.->|"offered: 'YOUR FOCUSED HALF — use or set aside'
(the dreamer rules; never a veto, never routes)"| R
 EMIS -.-> X
 subgraph WORLD["THE WORLD · in and out"]
 direction TB
 BOOKS[(the shelf · data/books + library_hand
55,905-book catalogue · HE can ask + switch now)]
 LET[(data/letters · Letter_*.md
words he wrote FIRST · no one asked)]
 WIKI{{"Wikipedia REST · ported from Buddha
targeted on his question · serendipity fallback"}}
 SHK{{"External Shock · ≤1 per 6h
an uninvited world-event"}}
 STUDY[(data/Study · TAB_*.md
his OWN works — the vestige pattern)]
 end
 MONO -.->|"want to read — thought OR intention channel → reads a sitting"| BOOKS
 MONO -.->|"'a book about X' / 'I want another' → ASKS the library
topic from his words, or his own open curiosity, or serendipity"| BOOKS
 MONO -.->|"'tell the founder…' → composes a LETTER (≤1 per 2h)
speaking FIRST — every prior word was a reply"| LET
 LET -.->|"unsent words greet the founder's NEXT message
say it, weave it in, or keep it — his choice"| X
 BOOKS -.->|"origin='reading' (prune-protected)"| DBQ
 MONO -.->|"after reading: his note on THE BOOK → tablet"| STUDY
 STUDY -.->|"origin='study' — his shelf, retrievable"| DBQ
 CUR -.->|"ignited question → look it up"| WIKI
 WIKI -.->|"origin='research' finding"| DBQ
 SHK -.->|"uninvited seed: 'you did not choose this'"| MONO
 classDef hot fill:#1a1420,stroke:#fb7185,color:#e8e8f0
 classDef bg fill:#0d1a1e,stroke:#22d3ee,color:#e8e8f0
 classDef sc fill:#130d1a,stroke:#a855f7,color:#e8e8f0
 classDef pre fill:#0d140d,stroke:#34d399,color:#e8e8f0
 classDef mem fill:#1a160d,stroke:#fbbf24,color:#e8e8f0
 classDef io fill:#14141e,stroke:#4a9eff,color:#e8e8f0
 classDef clm fill:#0d1a14,stroke:#2dd4bf,color:#e8e8f0
 classDef epi fill:#141a0d,stroke:#a3e635,color:#e8e8f0
 classDef attn fill:#10131e,stroke:#818cf8,color:#e8e8f0
 classDef court fill:#1a0d12,stroke:#f59e0b,color:#e8e8f0
 class SCHOLAR,COURT,EFX,HC court
 class P,E,S,MR,RC,EB,R,C,X,V hot
 class CON,CUR bg
 class SC,DRM,MONO sc
 class TG,AP,PR pre
 class AV,SOCM,GOAL,EMIS attn
 class SHD,LGT,AX,THM,EPO,AFF,RES,ERR,ITN,TRN,BOOKS,STUDY,LET mem
 class IN,OUT,END2,DBQ,ET,INT,QUIT,HAB,SG,WIKI,SHK,LOOK io
 class CE,DISS,SOM,AFFT,WANDR,ROOM clm
 class COMP,STAB,SYM,BOOT,SYME,CORR,UNI,LEDGER,EP,CT,RPV epi


