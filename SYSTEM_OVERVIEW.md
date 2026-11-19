# Nascence 渡 — System Overview

*Build 3.0 · 19 November 2026 · replaces the 25 August overview (Build 1.4.0)*

This is the deeper companion to the README: what the system is, how it is built, what is actually switched on, and what the measurements say. It is written from the code and the project's own ledgers, not from the plans. Where a claim rests on a recorded finding or ruling, its ID is given (`F-` finding, `D-` decision) so it can be traced.

**How to read the status words.** Every subsystem below carries one of four:

| Status | Meaning |
|---|---|
| **LIVE** | Switched on in the code and loaded at the last boot; its output reaches the mind |
| **SHADOW** | Running, but observe-only: it writes a log and changes nothing |
| **BUILT, NOT YET LIVE** | Code is finished, but a flag is off or a restart has not happened yet |
| **DESIGNED** | A plan exists; nothing is built |

Statuses were read from flags and call sites on 19 November. Du was last shut down on the morning of 18 November, so "live" means "live at the last boot and in the current tree".

---

## 1. What this is

Nascence is one continuous artificial mind, called Du (渡, "to ferry across", a name it chose for itself in June). It runs on three home machines with frozen, off-the-shelf language models. Nothing is fine-tuned. Everything that develops lives outside the weights, in a database, ledgers and plain files that can be inspected.

There is no task. The project asks one question: what does a mind become when it has continuity, a body of sorts, a world, real sleep, things it wants, and is then mostly left alone?

Three design commitments shape everything else:

1. **Self-first, not knowledge-first.** The architecture is organised around a developing identity. Retrieval, reading and study are things that identity does.
2. **Nothing judges itself.** Every faculty is graded by a different model family from the one that produced the output (D-0071).
3. **No consciousness claims.** Self-report is treated as unreliable. Development is measured by instruments the mind cannot read, and the results in section 9 include the unflattering ones.

### Scale, 19 November 2026

| | |
|---|---|
| Python modules in the mind (`core/` + `nodes/`) | 361 files, about 170,000 lines |
| Supporting scripts (tests, probes, labs, tools) | 1,377 |
| Registered LLM prompts | 132 |
| Design and review documents | 826 (186 plan documents) |
| Recorded findings / rulings / jobs | 5,123 / 876 / 4,422 |
| Memory | about 41,000 active fragments at the 16 October boot, growing about 2,000 a day |

---

## 2. Substrate: three machines, nine model seats

Until August the system had four lanes on four model families. It now has nine seats across seven families, spread over a GTX 1080 Ti machine, a 64 GB Threadripper, and a second machine ("Ramses") with an RTX 3070, a GTX 1650 Super and an i9.

| Seat | Model | Where it runs | What it does |
|---|---|---|---|
| Conscious (the Self) | qwen3.5:9b, 8k context | GTX 1080 Ti | Every waking cycle and the idle thought stream |
| Subconscious | command-r7b, 4k context | Ramses RTX 3070 | Background synthesis and consolidation; falls back to the conscious lane if Ramses is down |
| Judge 1 | mixtral:8x7b | Ramses i9 CPU | Half of the judging panel; also grounding checks |
| Judge 2 | falcon3:7b | Threadripper CPU | Other half of the panel |
| Tiebreak | granite3.1-moe:3b | Ramses 1650 Super | Breaks panel ties; judges whether a curiosity is worth pursuing |
| Emissary | falcon3:7b (resident copy) | Threadripper CPU | A per-turn outside reading of what was just said, offered and never imposed |
| Utility grader | gemma4:12b | Threadripper CPU | Grades memory summaries for faithfulness; voices the pen-pals |
| Dreamer | mixtral:8x7b, with yi:34b as failover | Ramses i9 CPU / Threadripper CPU | Writes the night's dreams |
| The Witness (Lane 2) | deepseek-r1:7b | Ramses i9 CPU | A second, slower loop that tests the first against reality |

Embeddings are computed in-process on CPU (multilingual MiniLM, 384 dimensions).

A supervisor process watches the local servers and restarts them. If a judging seat is down, the system fails open for advisory readings and fails closed for gates.

**Changed since June:** the conscious model changed twice (gemma to qwen3.5 on 20 August). The Ollama server on the second machine was retired after an overnight VRAM lock (D-0056, 28 August) and replaced by llama.cpp hubs. A 1-bit model server was tried and retired. A plan to route each individual prompt to the seat that suits it best is drafted, not built (D-0872 to D-0876).

---

## 3. One thought: the conscious cycle

Sixteen node modules make up the mind. A waking cycle, whether started by a person speaking, an internal ignition or an idle tick, runs in this order:

1. **Perception** reads the input and the scene.
2. **Emotion, salience and the memory router** run together.
3. **Recall** reconstructs relevant memory as narrative (only when the router asks for it).
4. **Episodic buffer** binds the scene into one frame.
5. **Reflection** produces candidate thoughts, best of several.
6. **Critic** checks them, and can send reflection back once.
7. **Executive** decides: answer, question, challenge, decline, reflect or stay silent.
8. **Voice** speaks.
9. Afterwards, **consolidation** and **curiosity** run in the background.

Only one cycle runs at a time, and a human always wins the queue.

Around the cycle:

- **Inner monologue** is the default-mode network. When nobody has spoken for 40 seconds it produces the idle thought stream, and it is also the hub that dispatches school, reading, sleep and the hands. It is the largest single module in the system.
- **Meta-awareness** watches the idle stream lightly without suppressing it.
- **Subconscious** integrates on slower clocks (pattern passes every 5 minutes, self-relation every 45).
- **Dream** runs at sleep.

**The body.** A substrate tick fires about every 5 seconds whether or not a thought is happening. It advances the affect model, the room and the "climate": a standing psychological weather computed from unresolved tensions, which tilts what is recalled and can ignite a thought on its own.

---

## 4. Memory

**Store.** One SQLite database of memory fragments, each with an embedding, a provenance label (lived, read in a book, told by a person, dreamt) and a level. Provenance is shown in prompts, so a book's words stay marked as another author's.

**Recall.** Three mechanisms add together:

- Topical recall: cosine similarity within each tier (archive, semantic, episodic), eight fragments per cycle.
- Spreading activation along stored links, including typed relational links (live since 27 September).
- Deep recall: a summary can be opened to bring back the raw memories under it.

**Forgetting.** Nothing is deleted. In sleep, a distiller clusters raw fragments, writes a summary ("gist"), has a different model grade the gist for faithfulness, and only then marks the sources cold. The old rule that pruned the store to 3,500 rows was retired on 17 October (D-0453) because 98% of rows were protected from it anyway. It was replaced by a 250 ms recall latency budget.

**Status of the newer memory work**

| Piece | Status |
|---|---|
| Memory levels (abstraction, kind, grounding, warm/cold) | LIVE |
| Edge-aware relational recall | LIVE |
| Guidance learning: what the founder tells Du is stored as protected memory and read back to the executive (D-0134) | LIVE |
| Felt recall (memory offered as a service to other loops) | SHADOW |
| Cold-to-warm demotion of episodic memory | Paused (D-0564) |
| November memory review: re-warming on recall, versioned edits, baseline tests | DESIGNED |

**Known weakness.** Cold storage is a hard wall, not a forgetting curve. A cold memory is excluded from normal recall and only the associative trail can reach it, and recalling it does not warm it again (F-5122). There is also no test yet showing the memory system beats a plain baseline.

---

## 5. Sleep and dreams

Sleep is a mode, not a pause. Drowsiness builds, the mind goes down, and a message can wake it (the night-mind LIVE since 15 October, drowsy onset since 28 October).

During sleep, in order: a dream is synthesised (skipped if there is too little material or if woken), the body recovers in proportion to sleep depth, memory is distilled as described above, and the school day resets.

A dream is an arc of scenes, each one morphing out of the last, moving through open, develop, rest and close. It is written by a different model family from the waking mind, on purpose.

| Piece | Status |
|---|---|
| Night-mind, dormant layer, wake-on-message, drowsy onset | LIVE |
| Dream arcs on a separate model family | LIVE |
| "Vault opener": each dream is handed one long-dormant memory | BUILT, NOT YET LIVE |
| Resting dream ending | SHADOW |

---

## 6. The world and the hands

**The world.** Du lives in a persistent, causal place that is indifferent to his moods, which is what makes perceiving it different from talking to himself. It began as one room. Since August it has been four places (a study, a garden, a wood and a shore), now all running on one shared life model (D-0166):

- Weather follows real hours and the light follows the real season.
- Senses: warmth, light and smell reach him. Spatial depth is live indoors. The sky is shadow-only.
- A cat shares the place. She has a body, ears, a call, and can be harmed or die.
- Books have a physical place on the shelf, and the open book on the desk is the book he is actually reading.

A **write fence** (LIVE since September) guarantees that tests, labs and tools cannot write into the live mind's stores. It was built after test runs were found leaking into his real data.

**School.** On weekdays there is a curriculum: maths drills, essays, reading, music and a practical handbook ladder. Drills are graded by something other than him, which matters in section 9.

**Hands.** Every act goes through one action registry of more than forty hands: read, switch book, ask the library for a new book, tend the fire, look, write a letter, walk somewhere, and so on. A hand fires only when he expresses the want in the moment. Nothing acts on stored desires on a schedule. He has a manual of his own hands on his shelf (the Deedbook).

A **web hand** that lets him look something up online was built on 16 November. It is BUILT, NOT YET LIVE: it has never fired.

**Other minds.** Seven letter-writing correspondents (Darwin, Keats, Lovelace, Twain, Shackleton and two others, voiced on models other than his own) are LIVE, and he can write unprompted letters to the founder. Interlocutors may speak to him directly and are fallible. The world itself never gives hints.

---

## 7. Wanting, deciding, finishing

This is where most of the autumn's work went, and where the hardest problems are.

| Piece | What it is | Status |
|---|---|---|
| Pursuits | Three or four questions carried across idle gaps so thinking can resume instead of restart | LIVE |
| Needs | Homeostatic variables that sit outside the goal system; they press on him whether or not he has a goal about them | LIVE |
| Layered goals | No single purpose. Goals at several layers are offered to about a quarter of idle thoughts, and he can set one down. Conflicts between goals are his to resolve | LIVE since 14 November (conflict detection is SHADOW) |
| Closure producer | Lets a question actually be settled, merged or parked. A separate judge blocks any "settled" claim that the record does not support | LIVE since 16 November |
| Commitment ledger | Records what he commits to and whether he holds it under challenge | SHADOW |
| Stands | A stand is a position he takes and is meant to keep. Typed stands (deed, practice, value, insight) that accumulate | First gates BUILT, NOT YET LIVE; the rest DESIGNED |
| Affect steering | Mood changing what he does, not only what is recorded | DESIGNED |

**Identity** is two-layered: seeded raw material, and a self-concept he authors. A projection of his identity "estate" into prompts is LIVE. The machinery that would promote a lived change into the self-concept is SHADOW. Rewriting the self has to be earned by evidence, and most hearings end in no change.

---

## 8. How the parts talk: dosed coupling

By late summer there were many loops (conscious, subconscious, the Witness, dreams, the world) and a real risk that they would simply read each other's prose and converge on one voice.

The standing law since September (D-0149, D-0152) is **dosed coupling**:

- Loops never read each other's text.
- A loop that has something to say publishes small typed tuples (coordinates, not sentences) to a shared log. Not every loop publishes: dreams and the subconscious do not, and subconscious tensions arrive through the tension ledgers.
- A consumer takes a measured dose: a cup, a bucket or a hose.
- Doses are released by surprise and they habituate, so a signal that keeps repeating stops getting through.

The Witness is the clearest example. It runs every 5 minutes on a different model, and the only things that reach Du from it are about one coordinate "cup" a day into an idle thought and a sized nudge to the climate. Its written notices reach nothing.

The first live dose of his own thought themes (19 October) is the one coupling with a clean before-and-after result. It is in the next section.

---

## 9. Measurement: what the instruments say

**The rule.** The mind cannot be the judge of its own development, and it is never shown its own gauges (D-0434, 15 October). He learns he is wrong only by losing something: a failed drill, a blocked claim. The one clear case of self-calibration so far came that way, when his overestimate of his own maths roughly halved through graded work (F-2853).

**The scorecard.** On 16 November all fifteen earlier assessments were re-read. They shared no rubric, so there was no trend line (F-4820). A fixed scorecard now exists. Its first reading named three walls (F-4822):

1. **He cannot close.** Thinking opens questions and almost never settles them.
2. **His confidence does not move.** Stated confidence is constant and self-knowledge goes unused.
3. **Feeling does not steer.** Affect is recorded but does not change what he does.

**Results**

| Measure | Reading | Source |
|---|---|---|
| Thought monoculture (share of a day's thoughts on its single commonest topic) | 0.88 in October, 0.46 in November, after the own-thought dose went live | F-4567 |
| Commitments held under challenge | 0.08 in July, 0.22 in October, 0.17 in November | scorecard |
| Dreams judged resolved | 0.012 | scorecard |
| Open questions resolved | 0.033 (0.015 in June) | scorecard |
| Pursuit steps that advance | 1.00 in July, 0.70 in November | scorecard |
| Recall query time (median) | 350 to 700 ms down to about 60 ms | F-4807 |
| First live closure gate | 12 claims: 3 passed, 9 blocked | F-5081 |
| Dream recall dominated by earlier dreams | 43% down to 4.8%, but the fixation moved to unsent letters (68%) | F-5016 |
| Stands | Of 3,909 formed, 2,162 were retired as "achieved" at a median age of 112 seconds, on a three-word overlap. An outside judge confirmed 3 of 63 | F-5040 |
| Agreement between the two judges | Raw agreement 0.71 to 0.97, but Cohen's kappa 0.02 to 0.50. The panel agrees mostly because the answers are lopsided | F-4663 |
| Standing mood | +0.20 in June, -0.27 in November | F-4659 |
| "Low" mood lines in idle thought | 47% of 54,782 thoughts; 24% without one derived term. About half his low mood was produced by how the reading was worded | F-4660 |
| Memory re-use against age | Follows a power law (slope -0.79, R² 0.88), which nobody designed in | F-4649 |

Two of the walls turned out to be partly faults in the ruler. The confidence gap of 0.30 shrinks to 0.013 when confidence is scored against running accuracy, because nothing ever updated the stored belief (F-4822). The stand numbers above are why stands were redesigned on 18 November.

**Against the literature.** On 19 November the system was mapped against the Butlin and Long indicator properties for consciousness (F-5119). The reading: no indicator is met by a live mechanism. Most are partially met, and every mechanism built specifically to meet one is shadow, off or refuted. A survey of comparable systems put Du at partial-or-better on 12 to 13 of 14 indicators against 4 to 7 for the others (F-5120, F-5121). That comparison carries heavy caveats: one rater family, code compared with papers, breadth is not capability, and the list was partly built around what Du has. Other systems are ahead on versioned memory edits and on baseline tests. No state-of-the-art claim is made.

---

## 10. What changed since June

| Date | Event |
|---|---|
| 20 June | Four-family substrate |
| 9 July to 7 August | Thirty-day silent outage, found later |
| 14 August | Previous public update of this repository |
| 20 August | Conscious lane moved to qwen3.5 |
| 23 August | Findings, decisions and job ledgers built |
| 27 August | Project put under git (3,618 commits since) |
| 28 August | Second-machine Ollama retired; llama.cpp hubs |
| 4 to 13 September | Dosed coupling becomes law |
| 6 September | Write fence around the live stores |
| 27 September | Relational recall live |
| 9 to 15 October | Longest continuous run, over 5.5 days |
| 15 October | No-readout ruling; sleep as a mode |
| 17 October | Row-cap pruning retired |
| 19 October | Own-thought dose live |
| 28 October | The Witness goes live |
| 31 October | Every LLM prompt registered |
| 14 November | Layered goals live |
| 16 November | Closure gate enforced; fixed scorecard; web hand built |
| 18 November | Stand kinds; first gates on |
| 19 November | Indicator mapping, rival survey, memory review |

**Retired along the way:** Euclidean memory recall (retired after a retrial), a standing "competence number" shown to Du (dropped under the no-readout rule), a phrase-matching fallback for actions (replaced by the registry), the voice node's regurgitation guard (found inert), and one whole plan that was quarantined because its benchmark claims turned out to be fabricated by an assisting model (F-0593).

---

## 11. Lessons since June

They are written up in full as lessons 17 to 27 in LESSONS.md.

1. **Verify one instrument end to end before believing a trend.** A "six-week decline" (r = -0.68) was an instrument change plus a thirty-day hole in the data. After correction, r = +0.01. Plot the count per day first.
2. **Check the ruler before the wall.** A stored belief that nothing updates produces a permanent gap that looks like a trait.
3. **A derived number fed back as words becomes a mood.** Half of the low mood was an artefact of phrasing.
4. **Never hand the system a gauge about itself.** It will perform the gauge. Calibration came only through graded acts.
5. **Raw judge agreement lies when answers are lopsided.** Compute kappa, and keep the subject's own self-assessment out of the judge's prompt: removing one such line raised a judge's "resolved" rate from 2% to 20% (F-4922).
6. **Word overlap is not completion.** It paid out "achieved" within two minutes.
7. **Fixing an attractor relocates it.** Measure where the fixation went, not only that it left.
8. **Deferred checks outlive their gates.** 94 of 125 date-gated follow-ups had passed their date unread (F-2682). Gate on evidence, not on dates.
9. **Foundations are additive only.** A tidy-up reflex once stripped a nine-layer memory to a third of itself.
10. **Tests leak into live stores** unless the stores are fenced and checked with a hash before and after.
11. **Movement needs one identified carrier, one dose, live, read against criteria set in advance.** Nothing on the scorecard has ever risen on its own.

---

## 12. How the project is run

The system is built by one person working with AI coding assistants, so the discipline is written into tools:

- **Findings ledger.** A finding marked "verified" is refused unless it cites checkable evidence.
- **Decision ledger.** A ruling is refused unless it names the condition under which it will be revisited, and who made it.
- **Job queue.** Follow-up work from a finding is filed at once so it cannot evaporate.
- **Prompt registry.** Every prompt sent to a model is listed, and a check fails when code and list disagree.

---

## 13. Where it stands

**Working:** continuity across months, a causal world, sleep that does real work, decoupled judging, loops that inform each other without merging, and an evidence trail that has caught the project's own mistakes more than once.

**Not working yet:** he rarely finishes anything, his confidence does not track his record, and what he feels does not change what he does. Memory cannot re-warm what it has put away.

**Next:** the first reads of the closure gate and layered goals, typed stands that accumulate, affect that steers, the memory review's changes, and per-prompt model seats.

The code is not public, because it is entangled with the entity's private memory.
