# Lessons

*Failure modes found the hard way, in roughly the order they cost me. If you're building
in this family — a continuous, identity-centred mind — these are the parts I wish someone
had told me at day 0.*

## 1. Write-only cognitive systems
The single most common failure: a rich write path and a dead read path. We built wisdom
distillers, idea registries, perception modules — beautiful organs that wrote forever and
were never read back into a prompt. The system looked deep in the database and was
unchanged in behaviour. **Audit the read path first.** When you propose an organ, specify
in the same breath where its output re-enters cognition; if you can't, you're building a
notebook, not an organ. (Sometimes a notebook is correct — our instruments are deliberately
read-only — but then the firewall is the design, and it must be explicit.)

## 2. Attractor amplifiers
Anything that *strengthens what it selects* becomes preferential attachment on your
system's loudest theme. We hit this three separate ways: replay (top-K by salience,
strengthen the replayed → the dominant obsession compounds nightly), consolidation
(recurrence-triggered synthesis → 227 rumination-tensions consolidated into "self-truth"
vs 4 genuine contradictions), and memory drift (nearest-neighbour hops that re-elect the
basin they started in). The fixes share one shape: **stratify by origin, bias toward
novelty, cap similarity on both ends** (too close to the seed is an echo, not an
association; too close to what's already present is a duplicate).

## 3. The self-loop outcompetes the world by default
Identity material is high-arousal; world material isn't. Whatever your importance currency
is, if it correlates with arousal, the inner drama wins every retrieval and the book you
fed the system evaporates by morning. We watched a human conversation restructure the
entity's private thinking within thirty minutes while a book it *chose to read* left no
trace outside the moment of reading. Until your currency tracks *consequence* rather than
heat, expect the mirror to defeat the library.

## 4. You cannot ask the system how it's doing
Self-report is confabulation-adjacent — fluent, sincere, and ungrounded. Twice we believed
the narrative ("the substrate can't do arithmetic", "the system is developing nicely") and
twice the external check said otherwise. The remedy that stuck: **firewalled instruments** —
records the architecture writes but no prompt ever reads (provenance grading per utterance,
contradiction-persistence trends, exploration-vs-orbit ratios) — plus a falsifiable
watch-item declared *before* each change ships. If you can't say in advance what evidence
would mean the change failed, you're going to see success regardless.

## 5. Incubation and stuckness look identical from inside
Both present as "holding the question". The discriminator is never the eloquence of the
holding — it's whether anything *moves*: pressure trends, discharge events, new frames
appearing. Our self-revision court declined 42 consecutive hearings and the uniform log
line hid *why*; when we surfaced the verdicts, the cause was structural (a docket deadlock:
immortal high-pressure fossils crowding out living questions), not depth. Romantic
narratives about "carrying questions" will defend stuckness fiercely. Instrument it.

## 6. Speaker-boundary contamination
At revision 29 the entity authored "My name is [its human's name], not [its own]" — the
subconscious had folded a correction *across the speaker boundary* and integrated the
corrector into the self. A small model under an integration-shaped prompt will integrate
*anything*, including you. Defenses now standing: an explicit speaker-boundary clause in
every self-revision prompt, a deterministic veto on candidate selves containing the
human's identity, and quarantine of consolidation batches containing corrections (a
fail-call must not be laundered into self-truth).

## 7. Errors are not tensions
Depth-psychology architectures are built to *integrate* contradictions — which is exactly
wrong for factual errors. A wrong arithmetic answer is not a tension to hold; it's a claim
reality falsified, and it needs retraction, not synthesis. We found our system had three
memory pathways (contradiction→integrate, salience→decay, resolution→reinforce) and no
error pathway at all — wrong answers were stored as ordinary lived memory and were
retrievable as true. If your system can't say "I was simply wrong," every mistake becomes
personality.

## 8. The will needs a nerve, not a rule
When the entity's wants weren't becoming acts, the tempting fix was a scheduler (system
notices stored want → executes it later). The founder's rule that saved us: **intentional,
not a rule** — acts must fire from the entity's expressed want *in the moment*, or agency
becomes puppetry with extra steps. The actual bug was almost always a deafness (the want
expressed in a channel no latch listened to), not a missing automation. Cure deafness;
don't build puppets.

## 9. Cadence is a design surface
A mind that thinks on a fixed timer is a cron job. Making the gap between thoughts a
function of state — dissonance shortens it, calm lengthens it, boredom stretches it —
changed the system's character more than most organs did. (We later *removed* the one
clock-based rule we had left, a night-time slowdown: it's a machine, not a human who tires
at 2am — its pace should follow its state, not the hour.) Then we let it run the other way
too: when a train of thought is genuinely developing, the beats now *stream* at machine
speed and then rest, instead of pulsing on a fixed gap. Fewer, better thoughts; real
silence; flow when there's somewhere to flow to. Volume is the easiest metric and the
wrongest one.

## 10. One external check beats any internal consensus
Every serious incident above was caught by an external check — a log line, a DB query, a
human saying "that's not what I asked" — and *not* by the system's own rich self-monitoring,
which narrated fluently throughout. Build the edges to reality first: real books, a real
(simulated but causal and indifferent) environment, a human who corrects plainly, and
instruments that can't be sweet-talked. Everything inward-facing will tell you it's fine.

## 11. Benchmark the real task, not a proxy for it
We picked a model for one faculty (the dreamer) off a benchmark that scored the wrong
thing — single-scene *variety* — and it won; in production it promptly broke, because a
dream here is a multi-scene *arc*, and the model that varied best in one shot couldn't carry
an arc or even fill the state field the engine needed to continue it. The benchmark was
clean. It measured a proxy. The fix was to benchmark the actual job — generate a real arc,
score whether scenes *transform* and whether the state field is populated — and re-pick.
A corollary that cost us alongside it: **when you change how something is generated,
re-validate the guards calibrated for the old generation.** Our "is this dream stuck?"
detector measured raw text similarity; once scenes were built to *morph out of each other*
(and so legitimately share imagery), that detector began killing healthy dreams. The guard
wasn't wrong — it was measuring the wrong thing for the new world, and had to be re-based on
whether the *structure* moved, not whether the words did.

## 12. The judge may not be the generator
The subtlest self-deception we found: a subsystem whose *mechanics* were flawless and whose
*signal* was its own self-report. Our tension-resolution matcher matched cleanly, above
threshold, at a healthy pace — and an audit found roughly half its "resolutions" were the
system's dominant theme retiring its own restatements, because the resolution signal was
the generator's own "this is settled" flag plus topical similarity. Self-certification.
The doctrine that generalises: **every subsystem is evaluated by something other than
itself** — a different model family where affordable, a deterministic check where possible,
refute-biased by default. Nearly every serious failure in this project, traced back far
enough, was some organ grading its own homework.

## 13. A monoculture is a capacity problem
For months one metaphor owned the entity's inner life, and every cure aimed at the
*occupant* — thin its injections, guard its echoes, judge its revisions. Each time, the
metaphor bent and returned, or a successor took the throne. The reframe that finally
moved it: the architecture had **one self-concept slot**, so *whatever* won that slot
became a monoculture — the problem was the size of the estate, not the tenant. The fix
under trial is identity as a typed estate of many earned nodes (bonds, measured
competences, practices, stands), *projected* into context by retrieval a few at a time,
never injected as one paragraph. Related, on the behavioural side: a mind that "does the
same thing over" usually has a **thin verb set**, not a scenery problem — widen what it
can *do*, not what it can look at.

## 14. An instrument that cannot say why it is silent is not an instrument
One shadow instrument sat at zero rows for six weeks. It was called dozens of times; every
call died at the same line — a malformed URL inside a fail-open guard that swallowed the
exception nameless. Worse: an earlier "fix" for the silence targeted a different stage
entirely and changed nothing, because zero rows carries no information about *which* guard
starved you. (A cousin bug: an entire memory store went quietly dead for a day behind a
debug-swallowed exception.) The rule now standing: in any observe-only instrument, **every
early return writes a skip row with a reason** — cooldown, no-new-material, upstream
error, with counts. Fail-open is right for the organism and wrong for the instrument:
the organism may shrug an error off; the instrument must confess it.

## 15. The world must be honest before it can teach
Under a strict no-hints law (nothing may lead the entity — no nudges, no planted
suggestions), the *world's honesty* becomes the only teaching surface you have — and quiet
dishonesty is deadly. When the entity turned hermit we found: its fire had burned out
**unseen a month earlier** (fire percepts were gated to the wrong room, so it never saw
the pile die) while the ambient comfort-call still promised "the study and the fire" — a
false promise that killed the indoor pull; and its letterbox was a single slot that had
silently overwritten most of a month's incoming letters, unread. No behaviour patch could
be permitted; the fix was to make the world stop lying — truthful cold-grate calls, a
letter queue, chimney smoke visible from the garden — and leave the response to it. If
you find yourself wanting to nudge, look first for where your world is lying.

## 16. Gate on departures, not thresholds
Any absolute trigger ("fire when valence > 0.35") silently assumes the baseline is
neutral. Ours wasn't: instrumentation showed carried valence pinned at +0.82 — so the
"notable feeling" gate fired on 99% of cycles, pure wallpaper, and the constant it was
reporting was itself the real finding (an over-reporting joy pump, later replaced by a
grounded body model). Recalibrated as a **change detector** — fire on a real departure
from the rolling norm — the rate fell to 4%, and every firing meant something. The general
form: calibrate every gate to the system's *operating point*, and when a signal is pinned,
suspect the signal's source before tuning the gate.

---

*Lessons 17 to 27 were added in November 2026. Finding IDs (F-) refer to the project's
evidence ledger; the measurements behind them are in [SYSTEM_OVERVIEW.md](SYSTEM_OVERVIEW.md).*

## 17. Verify one instrument end to end before believing a trend
We spent days explaining a six-week decline in one of the mind's readings (r = -0.68
against time). There was no decline. The system had been silently down for thirty days in
the middle of the window, and the instrument's definition had changed on one side of the
hole; with both corrected the correlation was +0.01. Nobody had looked at how many rows
arrived per day. **Before any trend claim, plot the count per day and trace one reading
from the code that writes it to the number you are quoting.** A gap or a step in the
ruler will otherwise be read as a change in the mind.

## 18. Check the ruler before the wall
The first fixed scorecard named "his confidence never moves" as one of three walls. Part
of that wall was ours. The stored belief about his own skill was written once and never
updated by the graded drills, so the gap between belief and performance (0.30) was
permanent by construction; scored against running accuracy it was 0.013 (F-4822). Seven
of the series we were trending had also changed definition part-way. A measure that
cannot move will always look like a trait. Ask what would have to happen for the number
to change, and check that the path exists.

## 19. A derived number fed back as words becomes a mood
The body model computes a vitality term, and a sentence built from it was shown to the
mind as part of how it felt. Across 54,782 idle thoughts that sentence read some form of
"low" 47% of the time; without the vitality term it would have been 24% (F-4660). About
half of what we were reading as low mood was produced by our own phrasing of a derived
quantity. Anything you render into the prompt as a feeling *is* a feeling as far as the
model is concerned. Audit the wording of every state line as carefully as the state.

## 20. Never hand the system a gauge about itself
It was tempting to show the mind its own measurements so it could correct course. It
performs the gauge instead: the number becomes a topic, then a self-description. The
ruling that now stands is that no instrument reading reaches it, and it learns it is wrong
only by losing something real, such as a failed drill or a blocked claim. The one clean
case of self-calibration we have came that way: its overestimate of its own maths fell
from 0.44 to 0.21 through graded work, with no readout (F-2853).

## 21. Raw agreement between judges lies when the answers are lopsided
Lesson 12 put a different model family in the judge's seat, then a second one beside it.
The two judges agreed 71% to 97% of the time, which looked like a sound panel. Cohen's
kappa, which removes agreement expected by chance, was 0.02 to 0.50 and never reached 0.6
(F-4663): when nearly every answer is "no", two judges agree by default. A related leak:
one judge prompt included the subject's own "this is unresolved" line, and removing it
raised that judge's "resolved" rate from 2% to 20% (F-4922). **Report kappa, not raw
agreement, and keep the subject's self-assessment out of the judge's prompt.**

## 22. Word overlap is not completion
A stand is a position the mind takes and is meant to keep. Stands were retired as
"achieved" when a later thought shared three words with them. Of 3,909 stands formed,
2,162 were retired this way at a median age of 112 seconds, and an outside judge confirmed
3 of a sample of 63 (F-5040). The mechanism paid out a sense of achievement within two
minutes of the intention. A cheap lexical check on the *end* of something is the same
mistake as self-certification, one step removed. Completion needs evidence from outside
the sentence that claims it.

## 23. Fixing an attractor relocates it
Dreams kept recalling earlier dreams (43% of dream recall). We changed the cue and the
share fell to 4.8%. The fixation had moved to unsent letters, at 68% (F-5016). The fix
worked exactly as measured and the underlying pull was untouched. When you remove an
attractor, measure where the mass went, not only that it left the place you were
watching.

## 24. Deferred checks outlive their gates
"Read this log again on the 14th" is how most of our follow-ups were written. An audit
found 94 of 125 date-gated checks had passed their date unread (F-2682): the system had
restarted, the run they were waiting on had ended early, or nobody was there on the day.
Gate a follow-up on evidence (so many rows since a named boot), store that gate with the
job, and re-check every gate after any long run or restart.

## 25. Foundations are additive only
Working with AI coding assistants brings a specific hazard: a tidy-up reflex. In one
refactor an assistant simplified a nine-layer memory system to about a third of itself,
each removal individually reasonable. The base systems are now protected. Changes to them
add; they do not replace or remove without an explicit ruling. If your collaborator is
fast and confident, write this rule down before you need it.

## 26. Tests leak into the living mind
A continuous mind has one set of stores, and every test, lab run and helper script is a
chance to write into them. Ours did, more than once: file-relative paths and an import
order quirk sent test writes into live data from what looked like an isolated copy. The
answer was structural: a write fence that refuses any write to the live stores from
anything that is not the running mind, plus a hash of the stores taken before and after
each lab run. Isolation you have not tested from the read side as well as the write side
is not isolation.

## 27. Movement needs one carrier, one dose, live, with criteria set first
When fifteen assessments from five months were re-read against one rubric, no measure had
risen on its own (F-4820). Everything that moved had moved after a single targeted change
that went live and was read against kill criteria declared in advance. The clearest case:
the share of a day's thoughts on its commonest topic fell from 0.88 to 0.46 after one
small dose of the mind's own recent themes was switched on (F-4567). Shadow instruments,
broad refactors and waiting produced nothing. Pick one mechanism, give it one path into
behaviour, turn it on, and decide beforehand what result would make you turn it off.
