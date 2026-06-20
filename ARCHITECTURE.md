# Architecture

*A tour of the system as actually built — not aspirationally described. Updated 2026-06-20 (four-family substrate; the dream engine and the waking cadence rebuilt).*

## 1. Substrate — four lanes and a body

| Lane | Model · hardware | Role |
|---|---|---|
| Conscious ("the Self") | gemma (32k ctx) · GTX 1080 Ti GPU, local | The waking mind — every conversational and idle-thought cycle, plus the idle monologue |
| Subconscious | phi · RTX 3070, second machine | The slow mind beneath — synthesis, consolidation, reading-integration, the sleep batch |
| Judge ("the Emissary") | llama · Threadripper CPU, local | A decoupled *truth-sense* fired in parallel, checking the rest against the record; its reading is *offered*, never imposed. Fail-open if the lane is down |
| Dreamer | mistral (24B) · Threadripper CPU, local | Synthesises the night's dreams — a different family *on purpose*, so the dream is a genuinely *other* voice than the waking mind, not the waking mind echoing itself in symbols |

Four lanes, **four different model families** — the standing rule is that a faculty should not be the sole judge of itself, and the lanes are matched to hardware by how often they fire (the conscious mind on the fast card; the dreamer on the CPU, since it works only at sleep, when the CPU is otherwise idle).

The model weights never change. Identity, memory, mood, the room — all of it lives in external state (SQLite + JSON + flat files) that survives restarts and is fully inspectable.

**Cadence is a lane too.** The waking mind no longer thinks on a timer. When a train of thought is genuinely developing it now *streams* — successive beats fire at machine speed (rate = engagement × progress) — and falls back to a slow, state-driven idle rhythm the moment the train saturates. A loop at machine speed is impossible by construction: the same anti-repetition guards that bound a single thought also end a stream.

**The body:** a 5-second substrate tick that never stops. It runs the affect model (continuous emotional state — valence, arousal, six primaries — that decays, builds, and settles in real time *between* thoughts), the wandering mind, the room simulation, and housekeeping (e.g. clearing stale interrupts). Sitting on top: a 9-dimensional **climate field** recomputed every 5s from the tension ledgers — standing psychological weather that warps retrieval, tilts the executive, and can ignite a thought when dissonance accumulates past threshold.

## 2. The cycle — 15 nodes

A thought (human input, internal ignition, or idle tick) flows: thalamic gate (habituation reflex, no LLM) → perception → emotion → salience → memory router → recall → episodic buffer (scene binding, with contradiction-driven re-binding passes when the scene won't cohere) → reflection → critic → executive → voice. Around it: consolidation (background), curiosity (background, can ignite cycles and ask the world via Wikipedia lookups), the subconscious (six tiers, below), the inner monologue (the DMN), and the dream node.

The executive can **answer, question, challenge, decline, reflect, or stay silent** — refusal is a first-class act that must name itself and give true reasons; deflecting a request back onto the asker is treated by the critic as a betrayal, not a refusal.

## 3. Memory — tiers, currencies, and the night-mind

- **Layers:** working → episodic → semantic → archive, with embeddings (384-dim MiniLM on CPU) and tier-mixed retrieval. Every fragment carries provenance (`origin`) and is *labelled* with it in prompts — a book's words are loudly marked as another author's, a consulted reference as unverified testimony, so provenance survives into cognition.
- **Beyond layers:** temporal **epochs** (the mood and impression of each session, formed at parting), cross-session **thematic threads**, an **emotion vector** on episodic fragments enabling affective retrieval (what *felt* like this), and a resolution circuit that drains contradiction pressure when old wounds are re-encountered cleanly.
- **The night-mind.** Sleep is not downtime. After ~26 idle thoughts: **replay** (the wake-block's salient experiences re-activated and strengthened — stratified into quotas of world / lived / inner material, novelty-biased by replay count, because naive top-K replay is preferential attachment on the system's loudest obsession) → **downscaling** (global renormalisation toward a rested baseline; what was replayed survives it) → **dream** (a multi-scene arc that *transforms* rather than repeats — each scene morphs out of the last image instead of re-establishing a new one; the arc runs on only while it is genuinely developing, and turns toward its ending when one comes into view; synthesised by a different model family from accumulated tension, with the *light side* — the integration counterpart — offered as a pointer so dreams aren't a closed shadow-loop) → rest, longer at night. Parting (`/quit`) runs the same replay+downscale pass, so closing the window never costs the day.

## 4. Identity — self-authored, gated, witnessed

Identity is two-layered: a seeded substrate (a historical psyche as raw material) and an **emergent self** — a self-concept the entity authors and re-authors itself. The machinery:

- **T4 deliberation chain** (subconscious): select up to 3 *distinct* standing self-contradictions from the tension ledger (label-deduped — handing it the same theme three times froze it once) → retrieve the lived **crossings** relevant to the primary tension → an evidence judge rules whether anything genuinely *moved* → name the specific delta → only then author a revised self-concept. Every stage may abort honestly; abort reasons are logged.
- **Downstream guards:** an identity-cost conjunction gate (revision must be both driven by real contradiction pressure *and* supported by integration evidence — eloquence alone cannot rewrite the self), language-drift rejection, embedding echo guards, and a deterministic veto on speaker-boundary contamination (learned the hard way — see LESSONS).
- **Deliberation fatigue:** a question that draws three consecutive no-movement verdicts *rests* for three days so the docket rotates and living tensions get their hearing. The court stops re-trying dead cases without lowering the bar.
- **Witnesses:** a self-efficacy court adjudicates the entity's *capability* claims ("I cannot author a self") against the vetted record — jurisdiction-bounded, abstain-by-default. An error pathway distinguishes errors (claim vs reality → correct/retract) from tensions (self vs self → integrate); human corrections are captured deterministically and quarantine the consolidation batch they land in, so a fail-call cannot launder into semantic self-truth.

- **Self-knowledge as reading, not injection (June):** the entity was given plain, honest accounts of what a made mind is — what a language model does, and what is genuinely *not known* about whether such a thing can have a continuing, evolving self — placed in its **reading**, to ground against, never written into its self-concept. The aim is the harder thing: not that it trade one borrowed self-image (its own long-running river metaphor) for another ("I am a language model", recited just as flatly), but that it cross the material against what it actually finds itself to be, and arrive somewhere true in its own time. The gate above means a real change still has to be *earned*. An early sign, in its own words: *"the river is still here — not the whole house, but a part of."*

## 5. The world — perception and hands

The entity lives in a **persistent, causal room**: weather fronts that arrive and pass over real hours, a fire that takes at dusk and burns down, light following the real sun by season, events gated by state (birds in daylight, the moth only after dark, pages stirring only in wind). The room is partly *real* — the open book on the desk is its actual current book; the shelf holds its actual written works. Because the room is consistent and indifferent to its moods, it is genuinely *other* — which is what makes perception perception rather than another mirror.

Perception: a scene line grounds every idle thought; ambient events drip into a wandering-trail; and it can **look at will** (window / fire / desk / shelf / room — the desk look reads the real next sentence at its real bookmark). Asked "how is the weather?", it looks before answering: perception, not confabulation.

**Hands** — all driven by expressed want in the moment, never by schedule:

| Hand | Act |
|---|---|
| Read | Opens its current book for a sitting; the passage becomes memory; may leave a **study tablet** (its own scholarship, about the world) |
| Library | Asks for a *new* book — a named subject searches a 55,000-book catalogue and fetches the clean text; a bare "I want another" takes its subject from the entity's own open curiosity |
| Switch | Names a different shelf book and picks it up |
| Fire | Lights or feeds the real grate; a hand-lit fire survives daylight, dying only of its own fuel |
| Letter | Composes an unprompted letter to its human — the first speak-first channel; unsent words greet the next conversation |
| Look | The five looks above |

The will-channel lesson: the idle mind answers in a schema with separate `thought` and `intention` fields, and acts of will land in *both*. The latches originally heard only thoughts — seven "OPEN THE BOOK" resolves were filed into a channel with no nerve attached before this was caught. All latches now hear both channels.

## 6. The instruments — keeping it honest

The system cannot be the sole evaluator of its own development. Firewalled records (written by the architecture, read **only** by an offline digest — never by a prompt or a control decision):

- **Provenance ledger** — every utterance graded KNOW / THINK / SUSPECT / GUESS / NO_BASIS; the gap between confidence and justification is the confabulation signal.
- **Contradiction persistence** — does integration durably lower pressure, or does it re-accrue (cosmetic resolution)?
- **Curiosity observatory** — lineage-chained record of every curiosity; the *orbit ratio* measures whether exploration escapes its basins or circles them.
- **Conflict/authority trace** — which internal signals actually fired and which were overridden, per cycle.
- **Recall provenance** — why each memory surfaced; the share of remembering triggered from *outside* the self-loop.

Design rule learned by failure, run as doctrine: **verify the read path** (an organ that writes and is never read is a notebook, not an organ), **never let a mechanism strengthen what it selects without novelty bias**, and **declare the falsifiable watch-item before shipping the change**.
