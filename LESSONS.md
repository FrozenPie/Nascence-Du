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
