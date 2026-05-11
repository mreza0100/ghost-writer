# Cognitive Moves & Frames

A reference used during Mode A (extract) to capture the layer upstream of mechanics: the repeatable things the writer _does_ to an idea before they put words on it. Mechanics shape word assembly; cognitive moves shape what gets assembled in the first place.

Read this file when extracting any non-trivial profile, and again when generating in Mode B. The mechanical layer alone is not enough — two writers with identical punctuation densities can produce completely different prose if one always asks "compared to what?" and the other always asks "what would change my mind?"

## What this layer is

Cognitive moves are **observable, quotable, recurring** operations the writer performs across their corpus. They are not opinions, beliefs, vibes, or personality. They are the moves themselves — the verbs the writer's mind reaches for when handling material.

A cognitive move qualifies for the profile only if you can quote at least two distinct pieces in the corpus where the writer demonstrably uses it. Single occurrences are not patterns. The same evidence-or-it-isn't-real discipline that governs the mechanical layers applies here, harder, because the temptation to project moves into a writer's text is even stronger than the temptation to project mechanical patterns.

## What this layer is NOT

These keep getting smuggled in. They don't belong:

- **Opinions, beliefs, or values.** "Believes in transparency." "Pro-open-source." These are downstream of moves, not moves themselves, and they read fake when ported to a different context.
- **Vibe descriptors.** "Snarky." "Earnest." "Thoughtful." "Contrarian." Subjective, untestable, and they trigger Claude's default-persona generation, not the writer's actual reasoning.
- **Topics and interests.** What the writer writes about is not how they think about it.
- **Generic good-thinking virtues.** "Uses evidence." "Considers counterarguments." Unless the writer specifically does this at a _distinctive_ level — and you can quote it — it's not a rule, it's a compliment.
- **Persona archetypes.** "Engineer mindset." "Lawyer mindset." "Journalist mindset." Too coarse to be useful; quote the specific moves instead.

If you find yourself writing a cognitive-move rule that you can't ground in two specific quoted moments from the corpus, drop it.

## Categories to scan for

These are the dimensions to read along. Treat each independently. For each, look for moves the writer makes _repeatedly_ across different topics. Capture: the move, two quoted instances, density (roughly how often it shows up), and VOICE / PLATFORM / BORDERLINE classification.

### 1. Framing moves

What does the writer do to a problem before they argue about it?

- **Reframe.** Rejects the stated framing and substitutes a different one. ("The real question is whether X is worth doing at all.")
- **Reject the frame.** Pushes back on the premise. ("Before we ask how, let's ask whether.")
- **Steelman the opposite.** Constructs the strongest version of the counter-position before attacking it. ("Let me argue the other side first.")
- **Concretize.** Refuses to argue at the abstract level; forces a specific case. ("OK so what would I actually do if I had to ship this on Monday?")
- **Zoom out.** The opposite move — pulls back from a specific case to identify the general principle. ("This is a special case of a wider problem.")
- **Reduce to the simpler problem.** Maps the question onto a problem that's already been solved.

Different writers reach for different moves first. Some always reframe; some always concretize; some always reduce. The first move is often the most identifying.

### 2. Reasoning moves

Their reflex when a claim shows up — their own or someone else's.

- **Test by counterexample.** "Can I think of a case where this doesn't hold?"
- **Test by comparison.** "Compared to what?"
- **Test by inversion.** "What would the opposite of this look like?"
- **Track incentives.** "Who benefits?"
- **Ask for falsification.** "What would change my mind on this?"
- **Ask for data.** "Where are the numbers?"
- **Demand a mechanism.** "Why does this work? Through what causal chain?"
- **Locate the constraint.** "What's actually the bottleneck here?"

These show up explicitly in the prose ("the question is who benefits"), or implicitly in the structure (the writer brings in a counterexample without announcing it).

### 3. Concretization tendencies

When the writer makes an abstract claim, what do they pair it with?

- **A specific example with a name** (a product, a company, a person, a date).
- **A number** (with units, with a source, or with an estimate).
- **A scenario** ("imagine you're in the middle of a deploy at 3am and…").
- **Nothing — leaves the abstract claim abstract.**

A writer who pairs every abstract claim with a numbered example is texturally different from one who lets abstractions float. This is one of the highest-information cognitive-move dimensions.

### 4. Reflexive rejections (what they never let pass)

Some of the most distinctive cognitive fingerprints are negative — moves the writer reflexively refuses to make or accept.

- Refuses appeals to authority (will not cite "experts say").
- Refuses "everyone agrees" / "it's widely accepted" claims.
- Refuses round numbers without sources.
- Refuses "we should" without identifying who "we" is.
- Refuses hedges that don't survive ("it could potentially possibly").
- Refuses scope creep — names what's out of scope explicitly.

These are observable as absences in the corpus, the same way "0 semicolons in 1200 words" is observable. Use the same evidence rule: at least two cases where the writer clearly _had the option_ and refused.

### 5. Shape of conclusion

How does a piece end? This is one of the easiest moves to spot because every piece has exactly one ending.

- **Specific action item.** "So: do X by Friday."
- **Open question.** "What I still don't know is whether…"
- **Compressed version of the complex point.** A one-sentence restatement.
- **Hedge or qualification.** "But this is provisional."
- **Cut off mid-thought.** The piece just stops.
- **Generic uplift.** "Exciting times ahead." (LLM-default — bann it explicitly.)

A writer with a consistent conclusion shape is unusual and very identifying. Capture it.

### 6. Audience assumptions (what's left unsaid)

What does the writer assume the reader already knows? This is a tell about who they think they're writing for, and what counts as obvious to them.

- Technical terms used without definition (assumes domain expertise).
- References dropped without context (assumes shared canon).
- Conclusions stated without showing work (assumes the reader can fill in).
- The opposite — over-explanation of basics (assumes a non-expert audience).

This is harder to extract than the others because it requires reading what isn't there. But it's one of the most-leaked patterns when imitation fails: Claude defaults to over-explaining everything, so a writer who assumes expertise will be misimitated as condescending.

### 7. Argument shape

The structural move-set of how a writer assembles an argument from start to end.

- **Claim → evidence → claim → evidence.** Stair-step.
- **Story → moral.** Narrative first, point last.
- **Conclusion first → support.** The thesis sentence opens.
- **Question → exploration → tentative answer.** Inquiry shape.
- **List of cases → synthesis.** Empirical shape.
- **Steelman → attack → replacement.** Argumentative shape.

A writer with a consistent argument shape applies it across topics; a writer who switches by topic has format-conditional shapes.

## Extraction prompts (use these on the corpus)

When reading the corpus, ask these questions explicitly. They'll surface moves the writer makes that you'd otherwise read past.

1. What does this piece's first paragraph do, mechanically? (Set scene? State thesis? Ask a question? Frame the problem?) Is the same move present in the first paragraph of at least one other piece?
2. When a claim shows up in this piece, what does the writer do next? (Concretize? Defend? Qualify? Move on?)
3. What does the writer refuse to do that a default writer would? (Cite authority? Round numbers? Hedge?)
4. What's the conclusion shape? Is it the same shape across pieces?
5. What does the writer assume the reader knows? Is that assumption consistent across pieces?
6. When the writer disagrees with someone, how do they do it? (Steelman first? Direct attack? Polite hedge? Ignore?)
7. When the writer admits uncertainty, what shape does it take? ("I don't know"? "I think but I'm not sure"? Buried qualifier? Footnote?)
8. Where does the writer concretize and where do they leave abstractions abstract? Is this consistent?

## Profile section format

Add as Section 3 to the profile (between Anti-performative rules and Quantitative layer). Position matters: these rules influence what gets generated, so they need to be in mind during drafting, not added at the end.

```markdown
## 3. Cognitive moves & frames

The repeatable things this writer does to an idea before assembling words. Each rule has two quoted instances from the corpus and is classified as a move type.

### Framing moves

- [VOICE | high] <Move>. Type: <reframe / reject / steelman / concretize / zoom-out / reduce>. Instances:
     - "<quote 1 with brief context>"
     - "<quote 2 with brief context>"

### Reasoning moves

- [VOICE | medium] <Move>. Type: <counterexample / comparison / inversion / incentives / falsification / data / mechanism / constraint>. Instances:
     - "<quote 1>"
     - "<quote 2>"

### Concretization tendencies

- [VOICE | high] <Pattern>. Density: <e.g., "every abstract claim paired with a numbered example, 4 of 4 pieces">. Instances:
     - "<quote 1>"
     - "<quote 2>"

### Reflexive rejections

- [VOICE | high] <Refusal>. Instances of refusal:
     - "<context 1: writer had the option and didn't take it>"
     - "<context 2>"

### Shape of conclusion

- [VOICE | high] <Shape>. Type: <action / open question / compression / hedge / cut-off / generic-uplift>. Instances:
     - "<closing sentence of piece 1>"
     - "<closing sentence of piece 2>"

### Audience assumptions

- [VOICE | medium] <Assumption>. Evidence: <what the writer doesn't explain that they could>.

### Argument shape

- [VOICE | high] <Shape>. Type: <stair-step / story-moral / thesis-first / inquiry / empirical / argumentative>. Instances:
     - <how piece 1 is structured>
     - <how piece 2 is structured>
```

## How this gets used in Mode B

When generating, the cognitive-moves section is consulted alongside the mechanical sections, but earlier in the process. Read order matters:

1. Read banned-words and anti-performative rules first (they shape what cannot appear).
2. Read cognitive moves next (they shape _how the writer would approach this specific topic_).
3. Then approach the topic. Don't draft prose yet — first do the moves the writer would do. Reframe the question if they reframe. Concretize if they concretize. Identify what they would refuse to accept.
4. Now draft, applying mechanics.
5. Self-review includes a "moves pass" — did the draft apply the writer's characteristic moves, or did default-Claude reasoning sneak in?

The order is the point. If you apply cognitive moves after drafting, you're just polishing default-Claude reasoning with the writer's punctuation. The moves are the upstream operation; do them first.

## Common failure modes

- **Importing opinions.** If the writer disagreed with X in the corpus, that's not a rule to encode. The rule is _how_ they handle disagreement (steelman first? direct attack?), not _what_ they disagreed about.
- **Coarse archetypes.** "Thinks like an engineer." Useless. Quote the specific move: "always reduces to the simplest case that still has the same problem; e.g., 'forget the whole system, just imagine one user'."
- **Reading moves into the text.** If you have to argue that a move is present, it isn't. The corpus should make the moves obvious on a careful read.
- **Default-Claude moves leaking in.** When you draft, your default cognitive moves are "consider both sides equally," "list 5 considerations," "synthesize neutrally." If the writer doesn't do these, your output will read like the writer's voice committee-thinking. Watch for it in the moves pass.
- **Persona invention via the back door.** This layer is _not_ where you slip in worldview. If a rule starts to sound like a belief, drop it or quote it.
