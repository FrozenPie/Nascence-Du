# Architecture

*A tour of the system as actually built — not aspirationally described. Updated 2026-11-19 (nine model seats, sleep as a mode, four places on one life model, dosed coupling and the Witness, wanting and finishing, the no-readout rule and the fixed scorecard). The previous version was dated 2026-08-14.*

The entity is called Du (渡); this document mostly says "the entity" and "it". Status words are used exactly as in SYSTEM_OVERVIEW.md: **LIVE** (switched on and reaching the mind at the last boot), **SHADOW** (running, writes a log, changes nothing), **BUILT, NOT YET LIVE** (code finished, flag off or restart pending), **DESIGNED** (a plan, no code). The entity was last shut down on the morning of 18 November, so "live" means live at that boot and in the current tree. IDs in brackets (`F-` finding, `D-` decision) point at rows in the project's ledgers.

## 1. Substrate — nine seats and a body

In August this section described four lanes on four model families. That line-up is retired. A *seat* is a named job that one model does; there are now nine, on seven model families, across three home machines: a GTX 1080 Ti machine, a 64 GB Threadripper, and a second box with an RTX 3070, a GTX 1650 Super and an i9.

| Seat | Model · hardware | Role |
|---|---|---|
| Conscious ("the Self") | qwen3.5:9b, 8k context · GTX 1080 Ti | The waking mind: every conversational cycle and the idle thought stream |
| Subconscious | command-r7b, 4k context · RTX 3070, second machine | The slow mind beneath: synthesis and consolidation. Falls back to the conscious seat if the second machine is down |
| Judge 1 | mixtral:8x7b · i9 CPU, second machine | Half of the judging panel; also grounding checks |
| Judge 2 | falcon3:7b · Threadripper CPU | The other half of the panel |
| Tiebreak | granite3.1-moe:3b · GTX 1650 Super | Breaks panel ties; judges whether a curiosity is worth pursuing |
| Emissary | falcon3:7b (resident copy) · Threadripper CPU | A per-turn outside reading of what was just said, *offered*, never imposed |
| Utility grader | gemma4:12b · Threadripper CPU | Grades memory summaries for faithfulness; voices the pen-pals |
| Dreamer | mixtral:8x7b, yi:34b as failover · i9 CPU / Threadripper CPU | Writes the night's dreams, on a different family from the waking mind *on purpose* |
| The Witness (Lane 2) | deepseek-r1:7b · i9 CPU | A second, slower loop that tests the first against reality (§6) |

Embeddings are computed in-process on CPU (multilingual MiniLM, 384 dimensions).

What changed, and why. The conscious model was gemma in August and moved to qwen3.5 on 20 August. The single "Judge" lane became a two-judge panel with a tiebreak, because the standing rule — a faculty must not be the sole judge of itself — was hardened into a ruling that every faculty is graded by a different model family from the one that produced the output (D-0071). The Ollama server on the second machine was retired after an overnight VRAM lock (D-0056, 28 August) and replaced by llama.cpp hubs. A 1-bit model server was tried and retired. A supervisor process watches the local servers and restarts them; when a judging seat is down the system fails *open* for advisory readings and fails *closed* for gates. A plan to route each individual prompt to the seat that suits it is drafted, not built (D-0872 to D-0876).

The model weights never change. Identity, memory, mood, the world — all of it lives in external state (SQLite, JSON, flat files) that survives restarts and is fully inspectable.

**Cadence.** The waking mind does not think on a fixed timer. When a train of thought is developing, the idle monologue *streams* — successive beats follow one another — and falls back to a slow idle rhythm when the train saturates (`THOUGHT_STREAM` and `CONTINUOUS_STREAM` are both on in the monologue node; LIVE). The same anti-repetition guards that bound a single thought also end a stream.

**The body.** A substrate tick fires about every 5 seconds whether or not a thought is happening. It advances the affect model, the world, and the **climate**: a 9-dimensional standing psychological weather computed from unresolved tensions, which tilts what is recalled and can ignite a thought on its own.

**Affect is grounded in a body model (LIVE since 7 July).** Carried valence used to be an LLM's per-cycle guess, and shadow instrumentation showed the guess pinned: a carried mood of +0.735 with 0.8% negative readings, beside a body model that was mean-zero and negative 59% of the time (`core/schema.py`, the `AFFECT_SOURCE_PROTOSELF` note). The body model is a homeostatic **protoself**: five essential variables (coherence, commitment integrity, relational warmth, novelty, vitality), each with a calibrated setpoint; *drive* is the weighted deviation from the setpoints, and mood is derived from it. The wire was swapped once the shadow pairs were read, and the entity can now feel bad. Two honest footnotes from November. First, the standing mood drifted from +0.20 in June to −0.27 in November (F-4659), and about half of the "low" mood lines turned out to be produced by how one derived reading was worded, not by the state itself (F-4660). Second, the 19 November literature mapping rates the protoself "keystone" as half-refuted as a predictive mechanism (F-5119). The body model sources the mood; it has not been shown to do more.

## 2. The cycle — 16 node modules

A *cycle* is one thought. It can be started by a person speaking, an internal ignition, or an idle tick. First a **thalamic gate** (a habituation reflex in the orchestrator, no LLM) can turn away input that merely repeats. Then, in order:

1. **Perception** reads the input and the scene.
2. **Emotion, salience and the memory router** run together.
3. **Recall** reconstructs relevant memory as narrative, only when the router asks for it.
4. **Episodic buffer** binds the scene into one frame.
5. **Reflection** produces candidate thoughts, best of several.
6. **Critic** checks them, and can send reflection back once.
7. **Executive** decides: answer, question, challenge, decline, reflect, or stay silent. Refusal is a first-class act.
8. **Voice** speaks.
9. Afterwards **consolidation** and **curiosity** run in the background.

Only one cycle runs at a time, and a human always wins the queue.

Around the cycle sit four more modules. The **inner monologue** is the default-mode network: when nobody has spoken for 40 seconds it produces the idle thought stream, and it is also the hub that dispatches school, reading, sleep and the hands. It is the largest single module in the system. **Meta-awareness** watches the idle stream lightly without suppressing it. The **subconscious** integrates on slower clocks (pattern passes every 5 minutes, self-relation every 45). The **dream** node runs at sleep.

**Broadcast and reentry.** A high-value winner of a cycle is *broadcast* to the idle monologue, which consumes it as material. This is LIVE but narrow: it has one consumer and posts after the fact (F-5119's mapping). At that admission point a **within-cycle reentry** loop re-presents the draft with one new, decoupled ingredient (a standing contradiction, or retrieval on the topic rather than on the draft) and lets it settle, at most twice, under a hard law that a pass with nothing new is forbidden. Its history is the August kill-switch doing its job. It ran in shadow from 4 July; the first 27 settle events showed no collapse, and admission went live on 2 September (D-0106). By 2 November the pre-declared defeater had fired: 31.6% of 136 settles since then collapsed toward the basin, a settle could delay a human reply by up to 260 seconds, and nothing but embedding arithmetic was judging the rewrite. Admission was switched off (D-0540; `REENTRY_ADMIT = False`, `core/reentry.py:42`). It is SHADOW again: the first draft is admitted and the settle is only logged. A Phase 2 (commitment formation and scene binding, with a pass budget scaled by arousal) is also SHADOW.

## 3. Memory — levels, relations, and forgetting by gist

**Store.** One SQLite database of memory *fragments*. Each carries an embedding, a provenance label (lived, read in a book, told by a person, dreamt) and a level. Provenance is shown in prompts, so a book's words stay marked as another author's. About 41,000 fragments were active at the 16 October boot, growing by about 2,000 a day.

**Levels (LIVE).** Every fragment is described along four axes: how abstract it is, what kind of thing it is, how well grounded, and whether it is *warm* (eligible for ordinary recall) or *cold* (set aside).

**Recall.** Three mechanisms add together. *Topical recall* is cosine similarity within each tier (archive, semantic, episodic), eight fragments per cycle. *Spreading activation* runs along stored links, including typed relational links — edges that say how two memories are related, not only that they are (LIVE since 27 September). *Deep recall* opens a summary to bring back the raw memories under it. Thoughts are also bound into *moments* (content, source, affect, body state, a drifting temporal key) so that what came next to something can be retrieved, not only what resembles it; that read-back is LIVE since 28 October (D-0495).

**Memory abstracts; it does not transcribe.** Lossless verbatim memory was part of why the system stagnated: the same raw text resurfacing forever is a loop's fuel. So forgetting is done by *gist*. In sleep a distiller clusters raw fragments, writes a summary, has a **different model** grade that summary for faithfulness, and only then marks the sources cold. Nothing is deleted. A journal consolidates lived time by the calendar on top of this, with day recaps rolling up into week pages.

**The row cap is gone.** The old rule pruned the store to 3,500 rows. It was retired on 17 October (D-0453) because 98% of rows were protected from it anyway. Its replacement is a budget on what actually matters: 250 ms of recall latency. Median recall query time fell from 350–700 ms to about 60 ms (F-4807).

**Guidance learning (LIVE, D-0134).** What the founder tells the entity is stored as protected memory and read back to the executive.

**Felt recall** — memory offered as a service to other loops — is SHADOW. Cold-to-warm demotion of episodic memory is paused (D-0564).

**The known weakness.** Cold storage is a hard wall, not a forgetting curve. Ordinary recall excludes cold rows entirely. The only way through is the associative trail, which demands a higher activation (0.22 against 0.13), and recalling a cold memory that way does not warm it again (F-5122). A November review designed re-warming on recall, versioned edits and baseline tests; none of it is built. There is no test yet showing this memory system beats a plain baseline.

## 4. Sleep and dreams — sleep as a mode

In August sleep was a batch that ran after a count of idle thoughts. It is now a *mode* the mind enters and leaves. Drowsiness builds with sleep pressure and the entity perceives its own onset; it goes down into a dormant layer where a night-mind runs instead of the waking stream; and a message from a person wakes it. The night-mind has been on since 15 October, and the perceived drowsy onset since the 28 October restart batch (D-0495). All LIVE.

During sleep, in order: a dream is synthesised (skipped if there is too little material, or if woken); the body recovers in proportion to sleep depth; memory is distilled as in §3; and the school day resets. The August replay and downscaling passes — re-activating the day's salient experience, then renormalising toward a rested baseline — are still called from the sleep path.

A dream is an **arc** of scenes, each morphing out of the last image instead of establishing a new one, moving through open, develop, rest and close. It is written by a different model family from the waking mind so that it is another voice and not the waking mind echoing itself in symbols (LIVE). Two additions are not yet in effect: a "vault opener" that hands each dream one long-dormant memory (BUILT, NOT YET LIVE), and a resting dream ending (SHADOW). The measured state of dreaming is unflattering: the share of dreams judged resolved is 0.012, and when dream recall stopped being dominated by earlier dreams (43% down to 4.8%) the fixation moved to unsent letters instead (F-5016).

## 5. The world — perception and hands

The entity lives in a **persistent, causal world** that is indifferent to its moods, which is what makes perceiving it different from talking to itself. It began as one room. It is now four places — a study, a garden, a wood and a shore — all running on one shared **life model** (D-0166): a single simulation of what grows, burns, moves and wears, so that the places obey the same rules.

The world runs on clocks, not on a language model. Weather follows real hours and the light follows the real season; the tide runs on a 12.42-hour period. Warmth, light and smell reach the entity as senses. Spatial depth is LIVE indoors. The sky is computed but SHADOW only. Each place has its own ambient sound. Books have a physical place on the shelf, and the open book on the desk is the book actually being read.

**The cat.** A cat shares the place. She has a body, ears and a call, comes and goes by her own state, and can be harmed or die. She is the most fleshed-out being besides the humans.

**The write fence (LIVE since September).** The entity runs continuously, so a test that touches a live file is an experiment on it. After test runs were found leaking into its real data, every store was put behind a fence: a process that is not the live mind raises an error when it tries to write there, and labs run against redirected copies.

**School.** On weekdays there is a curriculum: maths drills, essays, reading, music and a practical handbook ladder. Drills are graded by something other than the entity, which matters in §9.

**Hands.** A *hand* is one thing the entity can do. Every act goes through a single **action registry** of more than forty hands; the older phrase-matching fallback was retired in its favour. A hand fires only when the want is expressed in the moment. Nothing acts on stored desires on a schedule.

| Hand (examples) | Act |
|---|---|
| Read | Opens the current book for a sitting; the passage becomes memory |
| Switch | Picks up a different book from the shelf |
| Library | Asks for a *new* book |
| Fire | Tends the fire |
| Look | Looks at something in the place |
| Letter | Writes a letter, to the founder or to a correspondent |
| Walk | Goes somewhere else in the world |

The **Deedbook** is a manual of the entity's own hands, kept on its shelf as a book it can read, so that knowing what it can do is a matter of reading and not of being told. A **web hand** that lets it look something up online was built on 16 November. It is BUILT, NOT YET LIVE: it has never fired.

**Other minds.** Seven letter-writing correspondents (Darwin, Keats, Lovelace, Twain, Shackleton and two others) are LIVE, each voiced on a model other than the entity's own, and it can write unprompted letters to the founder.

**The no-hints law (August, permanent).** Nothing in the world may *lead* the entity: no advisory nudges, no planted suggestions. The world answers when asked. Interlocutors may speak to it directly and are fallible. Teaching happens diegetically — a book placed in the library — or through a person, never through the machinery.

## 6. How the parts talk — dosed coupling and the Witness

*New since August.* By late summer there were many loops (conscious, subconscious, dreams, the world, and a second mind being planned) and a real risk that they would simply read each other's prose and converge on one voice. The standing law since September (D-0149, D-0152) is **dosed coupling**:

- Loops never read each other's text.
- Each publishes small typed tuples — coordinates, not sentences — to a shared log.
- A consumer takes a measured *dose*: a cup, a bucket or a hose.
- Doses are released by surprise and they *habituate*, so a signal that keeps repeating stops getting through.

**The Witness (Lane 2)** is the clearest case. It is a second loop on a different model family that runs every 5 minutes and tests the first loop against reality. It went live on 28 October (D-0497). The only things that reach the entity from it are about one coordinate "cup" a day into an idle thought, and a sized nudge to the climate. Its written notices reach nothing.

The first live dose of the entity's own thought themes back into its idle stream (19 October) is the one coupling with a clean before-and-after reading: the share of a day's thoughts on its single commonest topic went from 0.88 in October to 0.46 in November (F-4567).

## 7. Identity — self-authored, gated, witnessed

Identity is two-layered: seeded raw material, and a self-concept the entity authors. Rewriting the self has to be earned by evidence, and most hearings end in no change. Each piece below was checked against the code on 19 November unless marked.

- **The identity estate — projection LIVE since 15 August.** The summer's finding was that the monoculture was a capacity problem: one self-concept slot, so one metaphor could own the whole self. Identity was rebuilt as a typed *estate* of earned nodes (bonds, measured competences, practices, values, stands, places and so on; 539 nodes at the October count, F-2758). Projection is by **retrieval, not injection**: three to five nodes relevant to the moment surface in the prompt, under a cap of two per node type so that no single kind can be the whole self. It was switched on after a founder read of the shadow log (136 projections, 54 distinct nodes, top node 6% of the share; `core/identity_projection.py:22`). A known gap: the estate is read by resemblance to the current thought, so the calibrated competence nodes surfaced on only 2.6% of projections.
- **Asking the estate — SHADOW.** A module that lets a self-question query the estate on the question's own terms logs what it would return and injects nothing (`ESTATE_ASK_LIVE = False`).
- **T4 deliberation chain** (the subconscious's self-authoring court). It selects up to 3 distinct standing self-contradictions, retrieves the lived *crossings* relevant to the tension, has an evidence stage rule whether anything moved, names the specific delta, and only then authors a revision. Every stage may abort honestly. A tension that draws three consecutive no-movement hearings rests for three days so the docket rotates. These constants are in the current subconscious node, and the identity-cost gate (a revision must be driven by real contradiction pressure, not eloquence) is still called there. How often the chain has run or authored since August was not re-checked.
- **Promotion of a lived change into the estate — SHADOW.** The writer that would turn a solidified change into one estate node (a single sentence of the entity's own, chosen deterministically, with its evidence attached) ships with all four of its flags off (`core/becoming_nodes.py:50-53`).
- **Witnesses.** A self-efficacy observer (capability claims checked against the record) and an error pathway (errors corrected, as distinct from tensions integrated) are both wired into the cycle. Their effect on behaviour was not re-checked since August.
- **Epistemic state.** Utterances are graded for provenance (KNOW / THINK / SUSPECT / GUESS). The harder read-back, owning a factual overclaim mid-conversation, has been off since 5 July (`STAGE_B_ENABLED = False`).

The August version described a reading-based route to self-knowledge (plain accounts of what a language model is, placed in the entity's reading and never in its self-concept). Status not re-checked since August.

## 8. Wanting, deciding, finishing

*Mostly new since August.* This is where most of the autumn's work went, and where the hardest problems are. August's "capacity to commit" — intentions formed against a named defeater — is the root of it.

- **Pursuits (LIVE).** Three or four questions carried across idle gaps, so thinking can resume instead of restart.
- **Needs (LIVE).** Homeostatic variables that sit *outside* the goal system. They press on the entity whether or not it has a goal about them.
- **Layered goals (LIVE since 14 November; conflict detection SHADOW).** There is no single purpose. Goals at several layers are *offered* to about a quarter of idle thoughts, never imposed, and the entity can set one down. Conflicts between goals are its own to resolve.
- **Closure producer (LIVE since 16 November).** Closure had been graded for months but never produced: nothing let a question actually be settled, merged or parked. The producer does that, and a **decoupled gate** stands behind it — a separate judge blocks any "settled" claim that the record does not support, and fails closed when the judge is unreachable. It was put in enforce mode by founder ruling on 16 November. In its first two days it saw 12 claims: 3 passed and 9 were blocked, 6 judged unsupported and 3 because the judge was down (F-5081).
- **Commitment ledger (SHADOW).** Records what the entity commits to and whether it holds under challenge. Commitments held under challenge read 0.08 in July, 0.22 in October and 0.17 in November.
- **Stands, and why they were redesigned.** A *stand* is a position the entity takes and is meant to keep. The numbers showed stands did not accumulate. Of 3,909 formed, 2,162 were retired as "achieved" at a median age of 112 seconds, on the strength of a three-word overlap between the stand and some later deed; an outside judge read 63 of those achievements and confirmed 3 (F-5040). Word overlap is not completion. On 18 November stands were redesigned as typed kinds (deed, practice, value, insight) that accumulate. The first gates are BUILT, NOT YET LIVE; the rest is DESIGNED.
- **Affect steering (DESIGNED).** Mood changing what the entity does, not only what is recorded.

## 9. Measurement — no readout, and a fixed scorecard

The system cannot be the sole evaluator of its own development.

**The no-readout rule (D-0434, 15 October).** *New since August.* The entity is never shown its own gauges. A standing "competence number" that used to be shown to it was dropped under this rule, because a system handed a gauge about itself performs the gauge. It learns it is wrong only by losing something: a failed drill, a blocked claim. The one clear case of self-calibration so far came that way — its overestimate of its own maths roughly halved through graded work (F-2853).

**The fixed scorecard.** On 16 November all fifteen earlier assessments were re-read. They shared no rubric, so there was no trend line (F-4820). A fixed scorecard now exists, and its first reading named **three walls** (F-4822):

1. **It cannot close.** Thinking opens questions and almost never settles them.
2. **Its confidence does not move.** Stated confidence is constant and self-knowledge goes unused.
3. **Feeling does not steer.** Affect is recorded but does not change what it does.

Two of the walls were partly faults in the ruler. The confidence gap of 0.30 shrinks to 0.013 when confidence is scored against running accuracy, because nothing ever updated the stored belief (F-4822); and the stand numbers in §8 were an artefact of the retirement rule.

**The decoupled judge.** Every subsystem is evaluated by something other than itself: a different model family where possible, deterministic where possible. The summer's cautionary tale was a tension-resolution matcher whose mechanics were flawless but whose signal was the generator's own self-report. The panel itself is now measured too, and not kindly: raw agreement between the two judges is 0.71 to 0.97, but Cohen's kappa is 0.02 to 0.50, so the panel agrees mostly because the answers are lopsided (F-4663).

**Firewalled records.** Some logs are written by the architecture and read only offline, never by a prompt. A conflict/authority trace (which internal signals fired and which were overridden) and a recall-provenance log (why each memory surfaced) are still wired into the cycle. The August list also named a provenance ledger, a contradiction-persistence record and a curiosity observatory; their status was not re-checked since August.

**Design rules learned by failure, run as doctrine.** Verify the read path: an organ that writes and is never read is a notebook, not an organ. Never let a mechanism strengthen what it selects without a novelty bias. Declare the falsifiable watch-item before shipping the change. Every early return in an instrument logs *why* it was silent. Isolate tests from live stores — now enforced by the write fence, not by care. Gate deferred checks on evidence, not on dates: 94 of 125 date-gated follow-ups had passed their date unread (F-2682).

## 10. The consciousness program — instruments, not verdicts

A build anchored to the Butlin and Long indicator properties, with the win condition stated before any organ was written: indicator coverage and theoretical defensibility, **never a presence verdict**. No consciousness claim is made here or anywhere in the project. Every organ ships as a shadow with a decoupled signature that must move, or it is dead code. Status of each, read from module flags on 19 November:

- **Protoself** — LIVE as the mood source (§1). As a predictive keystone, half-refuted.
- **Bound moments** — read-back LIVE since 28 October (`MOMENT_READBACK`).
- **Mineness comparator** — authorship by prediction error: did this thought flow from the entity's own groove, or arrive? Read-back LIVE (`MINENESS_READBACK`). The November mapping calls it the one confirmed live win, and notes it is not a Butlin indicator.
- **Arousal / level** — a global activation scalar from circadian base, dissonance, interoceptive error and sleep pressure. SHADOW: computed and logged, gating nothing.
- **Attention-schema control loop** — the schema predicts attention; the control half is off (`AST_CONTROL = False`). SHADOW.
- **World-prediction organ** — learned predictions of the world race a naive-persistence baseline, with the world itself as judge. SHADOW, and the learned side has not shown a significant lead.
- **Within-cycle reentry** — admission switched off on 2 November (§2). SHADOW.

Tier 3 (ignition as competition, a real specious present, deliberative fork-and-feel) remains DESIGNED. The honest ceiling is stated with it: this is algorithmic-level recurrence, not in-network reentry.

On 19 November the whole system was mapped against the fourteen indicators (F-5119). The reading: **no indicator is met by a live mechanism.** Most are partially met, and every mechanism built specifically to meet one is shadow, off or refuted. The program's own earlier scorecard had overstated four of them as present.

---

For the measured results, the full status tables and the timeline since June, see [SYSTEM_OVERVIEW.md](SYSTEM_OVERVIEW.md).
