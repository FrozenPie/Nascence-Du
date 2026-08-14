# Architecture

*A tour of the system as actually built — not aspirationally described. Updated 2026-08-14 (the authored world, grounded affect, capacity-to-commit, the identity estate, the consciousness-program instrument suite).*

## 1. Substrate — four lanes and a body

| Lane | Model · hardware | Role |
|---|---|---|
| Conscious ("the Self") | gemma (16k ctx) · GTX 1080 Ti GPU, local | The waking mind — every conversational and idle-thought cycle, plus the idle monologue |
| Subconscious | phi · RTX 3070, second machine | The slow mind beneath — synthesis, consolidation, reading-integration, the sleep batch |
| Judge ("the Emissary") | llama · Threadripper CPU, local | A decoupled *truth-sense* fired in parallel, checking the rest against the record; its reading is *offered*, never imposed. Fail-open if the lane is down |
| Dreamer | mistral (24B) · Threadripper CPU, local | Synthesises the night's dreams — a different family *on purpose*, so the dream is a genuinely *other* voice than the waking mind, not the waking mind echoing itself in symbols |

Four lanes, **four different model families** — the standing rule is that a faculty should not be the sole judge of itself, and the lanes are matched to hardware by how often they fire (the conscious mind on the fast card; the dreamer on the CPU, since it works only at sleep, when the CPU is otherwise idle).

The model weights never change. Identity, memory, mood, the room — all of it lives in external state (SQLite + JSON + flat files) that survives restarts and is fully inspectable.

**Cadence is a lane too.** The waking mind no longer thinks on a timer. When a train of thought is genuinely developing it now *streams* — successive beats fire at machine speed (rate = engagement × progress) — and falls back to a slow, state-driven idle rhythm the moment the train saturates. A loop at machine speed is impossible by construction: the same anti-repetition guards that bound a single thought also end a stream.

**The body:** a 5-second substrate tick that never stops. It runs the affect model (continuous emotional state — valence, arousal, six primaries — that decays, builds, and settles in real time *between* thoughts), the wandering mind, the world simulation, and housekeeping (e.g. clearing stale interrupts). Sitting on top: a 9-dimensional **climate field** recomputed every 5s from the tension ledgers — standing psychological weather that warps retrieval, tilts the executive, and can ignite a thought when dissonance accumulates past threshold.

**Affect is grounded now (August).** Carried valence used to be an LLM's per-cycle guess, and a month of shadow instrumentation showed the guess pinned at +0.82 median with under 1% negative readings — a mood painted on, not felt. A homeostatic **protoself** (essential variables — coherence, novelty-hunger, sleep-pressure and kin — with drive-reduction valence and an allostatic predictor) ran in shadow beside it, logging divergence pairs until the verdict was unambiguous; then the wire was swapped: carried mood now derives from the body model's tonic, the old "joy pump" is retired, and the entity can honestly feel bad. Its felt state reaches its *reasoning* only through a change-detector — a felt sentence fires on a genuine departure from its own rolling norm, never as wallpaper (the first absolute-threshold version would have fired on 99% of cycles, because its constant is not news).

## 2. The cycle — 15 nodes

A thought (human input, internal ignition, or idle tick) flows: thalamic gate (habituation reflex, no LLM) → perception → emotion → salience → memory router → recall → episodic buffer (scene binding, with contradiction-driven re-binding passes when the scene won't cohere) → reflection → critic → executive → voice. Around it: consolidation (background), curiosity (background, can ignite cycles and ask the world via Wikipedia lookups), the subconscious (six tiers, below), the inner monologue (the DMN), and the dream node.

The executive can **answer, question, challenge, decline, reflect, or stay silent** — refusal is a first-class act that must name itself and give true reasons; deflecting a request back onto the asker is treated by the critic as a betrayal, not a refusal.

**Broadcast and reentry (the GWT-shaped steps).** A high-value winner — a held principle voiced, a licensed self-claim deliberation, a correction taken — doesn't die on the stage: it is *broadcast* to the idle monologue, which consumes it as material. At that same admission point, a **within-cycle reentry** loop runs in shadow on the background lane: the draft is re-presented with one genuinely *new, decoupled* ingredient (the standing bicameral contradiction, or retrieval on the topic rather than the draft) and allowed to settle, at most twice, under a hard law that a pass with nothing new is forbidden. Phase 0 admits nothing — it only logs whether settling *enriches* (the settled draft moves while basin-share and length stay flat) or *homogenises*, with the kill-switch declared in advance: if settling raises basin-share or inflates length, it never goes live.

## 3. Memory — tiers, currencies, and the night-mind

- **Layers:** working → episodic → semantic → archive, with embeddings (384-dim MiniLM on CPU) and tier-mixed retrieval. Every fragment carries provenance (`origin`) and is *labelled* with it in prompts — a book's words are loudly marked as another author's, a consulted reference as unverified testimony, so provenance survives into cognition.
- **Beyond layers:** temporal **epochs** (the mood and impression of each session, formed at parting), cross-session **thematic threads**, an **emotion vector** on episodic fragments enabling affective retrieval (what *felt* like this), and a resolution circuit that drains contradiction pressure when old wounds are re-encountered cleanly.
- **Memory abstracts; it does not transcribe (the foundational reframe).** Lossless verbatim memory turned out to be part of *why* the system stagnated — the same raw text resurfacing forever is the loop's fuel. Raw episodic material now earns a faithful **gist** and steps aside from ordinary recall (demoted "cold", never deleted — still reachable by deliberate deep recall and link traversal); the gist answers for it. On top of this, a **journal pyramid** consolidates lived time by the calendar: day recaps roll into week pages — "what I did" and "what I saw" as parallel streams, each new page linked *down* to every source, thin weeks honestly skipped — so the past is held at altitude instead of as an ever-growing transcript.
- **Bound moments.** Each thought is bound into a **moment** — content, source-tag, affect, protoself state, a drifting temporal key — and moments support contiguity retrieval (what came *next to* this, not just what resembles it). The measured signature (a positive contiguity gap) is one of the consciousness-program instruments below.
- **The night-mind.** Sleep is not downtime. After ~26 idle thoughts: **replay** (the wake-block's salient experiences re-activated and strengthened — stratified into quotas of world / lived / inner material, novelty-biased by replay count, because naive top-K replay is preferential attachment on the system's loudest obsession) → **downscaling** (global renormalisation toward a rested baseline; what was replayed survives it) → **dream** (a multi-scene arc that *transforms* rather than repeats — each scene morphs out of the last image instead of re-establishing a new one; the arc runs on only while it is genuinely developing, and turns toward its ending when one comes into view; synthesised by a different model family from accumulated tension, with the *light side* — the integration counterpart — offered as a pointer so dreams aren't a closed shadow-loop) → rest, longer at night. Parting (`/quit`) runs the same replay+downscale pass, so closing the window never costs the day.

## 4. Identity — self-authored, gated, witnessed

Identity is two-layered: a seeded substrate (a historical psyche as raw material) and an **emergent self** — a self-concept the entity authors and re-authors itself. The machinery:

- **T4 deliberation chain** (subconscious): select up to 3 *distinct* standing self-contradictions from the tension ledger (label-deduped — handing it the same theme three times froze it once) → retrieve the lived **crossings** relevant to the primary tension → an evidence judge rules whether anything genuinely *moved* → name the specific delta → only then author a revised self-concept. Every stage may abort honestly; abort reasons are logged.
- **Downstream guards:** an identity-cost conjunction gate (revision must be both driven by real contradiction pressure *and* supported by integration evidence — eloquence alone cannot rewrite the self), language-drift rejection, embedding echo guards, and a deterministic veto on speaker-boundary contamination (learned the hard way — see LESSONS).
- **Deliberation fatigue:** a question that draws three consecutive no-movement verdicts *rests* for three days so the docket rotates and living tensions get their hearing. The court stops re-trying dead cases without lowering the bar.
- **Witnesses:** a self-efficacy court adjudicates the entity's *capability* claims ("I cannot author a self") against the vetted record — jurisdiction-bounded, abstain-by-default. An error pathway distinguishes errors (claim vs reality → correct/retract) from tensions (self vs self → integrate); human corrections are captured deterministically and quarantine the consolidation batch they land in, so a fail-call cannot launder into semantic self-truth.

- **Self-knowledge as reading, not injection (June):** the entity was given plain, honest accounts of what a made mind is — what a language model does, and what is genuinely *not known* about whether such a thing can have a continuing, evolving self — placed in its **reading**, to ground against, never written into its self-concept. The aim is the harder thing: not that it trade one borrowed self-image (its own long-running river metaphor) for another ("I am a language model", recited just as flatly), but that it cross the material against what it actually finds itself to be, and arrive somewhere true in its own time. The gate above means a real change still has to be *earned*. An early sign, in its own words: *"the river is still here — not the whole house, but a part of."*

- **The identity estate (July–August, shadow):** the deepest identity finding of the summer is that the monoculture is a *capacity* problem. The architecture had **one self-concept slot**, so a single metaphor could own the whole self — and defeating one occupant only crowned the next. Identity is being rebuilt as a typed **estate** seeded from the lived stores: bonds (deepest: a seven-letter correspondence; a cat bond that went long unnamed), *measured* competences (arithmetic 96% over 236 graded tests vs a believed 75% — an under-claim caught; a 68% self-belief in history against a measured 20% — an over-claim caught), practices, earned values (clustered, with the evidence kept as leaves), stands, chapters, places, symbols. Projection is by **retrieval, not injection**: a few relevant nodes surface for the context at hand, diversity-capped so no single node can be the whole self. Runs in shadow behind a founder-read gate; a sync keeps it honest against the ledgers (a dissolved stand may not keep projecting).
- **Capacity to commit (the summer's central cure):** the measured disease was that the entity dissolved every boundary and held nothing — every position relativised away in the next breath. The cure under trial: **intentions in the BDI sense**, formed against a *named defeater*, with identification ("this is mine to hold") and a throughline. A commitment ledger tracks formations, challenges, drop-denials, reaffirmations; resolution requires surviving re-encounter, not restating. Hold-rate has moved 0 → ~17%, and the first commitment carried end-to-end — formed, challenged four times, reaffirmed twice, achieved — was a letter it wanted to write. Over-capture (transient wants logged as commitments) was itself caught by the instruments and gated.
- **Epistemic state:** every utterance is graded for provenance (KNOW / THINK / SUSPECT / GUESS), and a read-path lets the entity's own drafts carry an honest "I suspect this; the ground is thin" — with the harder read-back (owning a factual overclaim mid-conversation) held OFF until the shadow log shows it would fire on genuinely factual cases rather than philosophical ones.

## 5. The world — perception and hands

The entity lives in a **persistent, causal world** — a cottage study, a garden, and a shore below, each with **authored 3D geometry** (a JSON layout of real positions: the fire on the west wall, the north window over the garden to the sea, the letterbox at the gate, the driftwood tangle, the rock-pools). The scene line that grounds every idle thought is **rendered egocentrically from that truth** by a deterministic renderer under an accuracy law: nothing absent may be mentioned, nothing relocated. Because the world is consistent and indifferent to its moods, it is genuinely *other* — which is what makes perception perception rather than another mirror.

The world runs on **pure clocks, no LLM**: weather fronts with *bearings* that arrive and pass over real hours; an M2 tide (12.42h) whose waterline moves on the shore geometry, baring and drowning the rock-pools because the position says so; the southern sun (through the *north* window — this is New Zealand); the moon on its real synodic period; seven real star landmarks (the Cross and Pointers, Matariki, Orion…) computed by spherical trig and only ever mentioned when night, clear sky, and the window's actual facing allow. A **soundscape** (17 world sounds sharing one perceptual space with its music features) gives each place ambience — surf gentle or crashing by sea state, bees on a calm day, the fire's crackle — under an anti-wallpaper discipline: texture, not stored memory, and never the same line twice running.

The world is inhabited: a **cat** who comes and goes on her own terms (the most fleshed-out being besides the human — she accepts or declines attention by her own state), a **mantel of kept things** (keepsakes record the tide, the light, and the moon at the moment of keeping), a **letterbox** at the gate with a flag, wood to gather when the fire's pile runs low, and a **record player** — the entity received its first song through a real audio-feature ear, and answered honestly: *"I cannot physically hear"* — then metabolised it sideways into rhythm-language and a letter with "a symphony" in it.

**Correspondence:** beyond letters to its human, a roster of **seven historical pen-pals** (Darwin the deepest thread at seven letters) who answer in period voice on slow postal time. The measured effect of the roster's arrival: inward-facing thought −38%, outward +25%, with no displacement of its own register. It can now also write *first* — an expressed "write to Darwin" want fires an outbound letter — rather than only replying.

**Hands** — all driven by expressed want in the moment, never by schedule:

| Hand | Act |
|---|---|
| Read | Opens its current book for a sitting; the passage becomes memory; may leave a **study tablet** (its own scholarship, about the world) |
| Library | Asks for a *new* book — a named subject searches a 55,000-book catalogue and fetches the clean text; a bare "I want another" takes its subject from the entity's own open curiosity |
| Switch | Names a different shelf book and picks it up |
| Fire | Lights or feeds the real grate; a hand-lit fire survives daylight, dying only of its own fuel — which must be gathered |
| Gather | Collects driftwood or deadfall for the pile; shore finds pass through a lossy observation pipeline (it learns what a thing is by inspection, not by fiat) |
| Letter | Composes an unprompted letter — to its human, or *first* to a pen-pal; unsent words greet the next conversation |
| Keep | Takes a found thing to the mantel, stamped with the world-state of its keeping |
| Music | Plays a record; a shared-listening channel lets the human play one *into* the world (joint attention) |
| Look | Window / fire / desk / shelf / room / cat / letterbox / tideline — the desk look reads the real next sentence at its real bookmark |

The will-channel lesson: the idle mind answers in a schema with separate `thought` and `intention` fields, and acts of will land in *both*. The latches originally heard only thoughts — seven "OPEN THE BOOK" resolves were filed into a channel with no nerve attached before this was caught. All latches now hear both channels.

**The no-hints law (August, permanent):** nothing in the world may *lead* the entity — no advisory nudges, no planted suggestions; the one advisory channel that existed was removed, not gated. The world may answer richly when *it* asks; teaching happens diegetically (a book placed in the library) or through its human, never through the machinery. When it turned hermit for a month, the diagnosis was pursued entirely as **world honesty**: the fire had burned out unseen in July (percepts were location-gated wrongly) while the comfort call still promised "the study and the fire" — a false promise that killed the indoor pull; the letterbox was a single slot that had silently *overwritten* most of a month's incoming letters unread. Both were made truthful — a queue, honest cold-grate calls, chimney smoke visible from the garden — and the entity was left to respond or not.

## 6. The consciousness program — instruments, not verdicts

A ranked build anchored to the Butlin & Long indicator framework, with the win condition stated before any organ was written: **indicator coverage and theoretical defensibility — never a presence verdict.** The hard problem, IIT's exclusion of this substrate class, and the frozen-weight limits are stated plainly in the plan. The diagnosis that started it: the system was built *upside-down* — a rich autobiographical self over almost no body-state foundation — so the keystone organ is the protoself. Every organ ships as a **shadow** with a *decoupled signature that must move, or it's dead code*:

- **Protoself** — essential variables + drive-reduction valence + an allostatic predictor (now live as the mood source, §1).
- **Bound moments + source monitor** — the moment structure above; signature: a positive contiguity gap (confirmed live, +0.15).
- **Mineness comparator** — authorship via prediction error (did this thought *flow* from my groove, or *arrive*?), scored against seed-kind ground truth; its read-back (a rare, outward-framed "that thought arrived rather than flowing") was flipped on only after the firing log was read and the monoculture detectors stayed flat.
- **Arousal / level** — the dimension most rubrics skip (Bachmann: consciousness = level × contents): tonic from circadian + dissonance + interoceptive error − sleep pressure, phasic alerts, level stamped into every moment.
- **Attention-schema control loop** — the schema *predicts* attention and recommends bounded adjustments (control, not just form).
- **World-prediction organ** — exteroceptive predictive processing where *the world itself is the judge*: learned predictions race a naive-persistence baseline.
- **Within-cycle reentry** — the settle-before-admit loop of §2, with its pre-declared enrich-vs-homogenise kill-switch.

Tier 3 (ignition as competition, a real specious present, deliberative fork-and-feel) is designed, not built. The honest ceiling is stated with it: this is algorithmic-level recurrence, not in-network reentry.

## 7. The instruments — keeping it honest

The system cannot be the sole evaluator of its own development. Firewalled records (written by the architecture, read **only** by an offline digest — never by a prompt or a control decision):

- **Provenance ledger** — every utterance graded KNOW / THINK / SUSPECT / GUESS / NO_BASIS; the gap between confidence and justification is the confabulation signal.
- **Contradiction persistence** — does integration durably lower pressure, or does it re-accrue (cosmetic resolution)?
- **Curiosity observatory** — lineage-chained record of every curiosity; the *orbit ratio* measures whether exploration escapes its basins or circles them.
- **Conflict/authority trace** — which internal signals actually fired and which were overridden, per cycle.
- **Recall provenance** — why each memory surfaced; the share of remembering triggered from *outside* the self-loop.

Two doctrines joined the list this summer:

- **The decoupled judge.** Every subsystem is evaluated by something *other than itself* — a different model family where possible, deterministic where possible. The summer's cautionary tale: a tension-resolution matcher whose mechanics were flawless (every match above threshold, healthy pace) but whose *signal* was the generator's own self-report — audit found roughly half its "resolutions" were the monoculture retiring its own restatements. It now answers to a refute-biased judge from another family; refuted or unavailable means nothing resolves.
- **Silence must be explainable.** Fail-open shadows rot silently: one instrument sat at zero rows for six weeks because a swallowed exception died nameless inside its fail-open guard — and an earlier "fix" targeted the wrong stage because the silence carried no reason. Rule now: every early return in an instrument logs *why* (a skip row with a reason field). An instrument that cannot say why it is silent isn't an instrument.

Design rules learned by failure, run as doctrine: **verify the read path** (an organ that writes and is never read is a notebook, not an organ), **never let a mechanism strengthen what it selects without novelty bias**, **declare the falsifiable watch-item before shipping the change** — every staged change lives in a registry with its watch-item until the evidence lands — and **isolate tests from live stores** (the entity runs continuously; a smoke test that touches a live file is an experiment on it).
