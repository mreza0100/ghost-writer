# Rhetorical Structure & Essay Arc

A reference used during Mode A (extract) to capture the layer between cognitive moves (idea-level) and the mechanical/vocabulary layers (sentence- and word-level): how the writer assembles paragraphs into pieces. This is the **essay-arc layer**, sometimes called the macro layer.

Read this file alongside `extraction-checklist.md`, `cognitive-moves.md`, and `vocabulary-fingerprint.md`. The mechanical layer captures *how sentences are shaped*; the vocabulary layer captures *which words get reached for*; the cognitive-moves layer captures *what operations the writer performs on an idea*; this layer captures *what shape the piece takes as it unfolds*.

## Why this layer exists

A writer can be identified, mechanically and lexically, by sentence-level fingerprints. They can be identified, cognitively, by the moves they reach for. But two writers with identical mechanics, vocabulary, and cognitive moves can still write completely different essays — because they assemble their material into different *shapes*.

Paul Graham is the test case for this. Profile him on mechanics: 17-word sentences, σ 11.5, em-dash density 1.0/1000w, colon density 3.7/1000w. Profile him on vocabulary: synonym binaries crystalline (use/utilize 1184/0, but/however 4829/94). Profile him on cognitive moves: reframe-first, mechanism-then-consequence, evidence-from-named-cases. All correct. But none of that captures what makes a PG essay a PG essay at the piece scale: the paradox opener, the personal-to-civilizational scale shift, the term coined in passing, the footnote drier than the main text, the negation-as-thesis structure ("the way to get ideas is *not* to try to think of ideas").

These are essay-scale patterns. They show up across paragraphs, not within sentences. They need their own extraction.

## What this layer is

Observable, recurring structural patterns across the corpus at the **paragraph-and-above** scale. Each rule must be supported by at least two quoted instances or, where appropriate, density figures across multiple pieces.

The diagnostic test for "is this in scope": can you point to it across two or more pieces, with a quote or a structural description that another reader would recognize? If yes, in scope. If you'd have to argue it's there, out of scope.

## What this layer is NOT

- **Topic / subject matter.** "Writes about startups" is not a rhetorical pattern. "Argues by mapping startups onto historical cases" is.
- **Persona / archetype.** "Engineer mindset", "journalist mindset", "essayist". Too coarse. Quote specific structural moves.
- **Vague compliments about structure.** "Well-organized." "Builds arguments carefully." Not rules; just praise.
- **Single-piece quirks.** A move that appears once in a corpus of 50 pieces is not a structural pattern. Two minimum, three+ for high confidence.
- **Format conventions that aren't the writer's.** A blog template's section headers aren't structure; the writer's preference to ignore or embrace them is.

## Categories to scan for

When reading the corpus, scan each dimension independently. Capture: the pattern, two quoted instances or a density figure, classification (VOICE / PLATFORM / BORDERLINE), confidence.

### 1. Opening shape

The first paragraph (sometimes first sentence) of a piece. Sample 10–20 piece openings from the corpus and classify by shape:

- **Paradox / counterintuitive claim.** "The way to get startup ideas is not to try to think of startup ideas." Opens with a claim that initially sounds wrong.
- **Anecdote.** Opens with a personal story or observed moment.
- **Question.** Opens with the question the piece will answer.
- **Scene-setting.** Opens with a concrete situation or environment.
- **Direct thesis.** Opens by stating the conclusion.
- **Historical hook.** Opens with an event or figure from the past.
- **Definition.** Opens by defining the key term.
- **Negation.** Opens by saying what the piece will *not* argue.
- **Quoted material.** Opens with someone else's words.
- **Numerical anchor.** Opens with a striking statistic.

A writer with a dominant opening shape is highly identifying — many writers cycle one or two shapes for 60%+ of openings. Capture the dominant shape and the alternates with frequencies.

### 2. Argument arc (full essay shape)

Beyond conclusion shape (already in cognitive moves): the *full* structural arc of a piece, from opening to close. Sample 5–10 pieces and trace the arc.

Common arcs:

- **Problem → reframe → mechanism → examples → derived advice.** (PG's dominant.)
- **Anecdote → generalization → return-to-anecdote.** (Personal-essay shape.)
- **Claim → defense → counterargument → response → conclusion.** (Argumentative shape.)
- **Question → exploration → tentative answer.** (Inquiry shape.)
- **List of cases → synthesis.** (Empirical shape.)
- **Definition → consequences → applications.** (Analytic shape.)
- **History → present moment → forecast.** (Temporal shape.)
- **Steelman → attack → replacement.** (Adversarial shape.)
- **Personal story → moral.** (Parable shape.)

A consistent arc across pieces is one of the most identifying patterns in this layer. Some writers have one arc; some cycle through 2–3.

### 3. Scale-shifts

How often does the writer zoom between scales (concrete↔abstract, personal↔civilizational, momentary↔historical), and in which direction?

Sample 5 pieces, count scale shifts per 1000 words (a shift is any move between concrete-particular and abstract-general). Note the dominant direction:

- **Concrete-to-abstract dominant.** Starts with a moment, generalizes.
- **Abstract-to-concrete dominant.** Starts with a principle, illustrates.
- **Balanced.** Roughly equal in both directions.
- **Sustained-one-scale.** Stays at one scale for whole pieces.

A writer who shifts 8+ times per 1000 words has a fundamentally different texture from one who shifts 1–2 times.

### 4. Example texture mix

When the writer concretizes (which the cognitive-moves layer already says they do), *what kind of example*? Sample 30–50 examples across the corpus and tag each:

- **Personal anecdote.** ("When I was running YC…")
- **Historical figure or event.** (Patek, Wozniak, Renaissance Florence.)
- **Named company / product.** (Microsoft, Apple, Google.)
- **Dated anchor.** ("By 1978…", "In the 1960s…")
- **Hypothetical scenario.** ("Imagine you had to ship by Friday.")
- **Numerical / data point.** (.625 USD / yen, 17% margin.)
- **Domain transfer.** (Programming compared to painting, startups compared to evolution.)
- **Quoted source.** (Someone else's words used as evidence.)
- **Imagined dialogue.** (Constructed conversation as illustration.)

A writer's example-texture-mix is a distinctive fingerprint. Some lean entirely on personal anecdote (memoirists); some on dated historical anchors (essayists); some on named-company lists (business writers); some on hypothetical scenarios (philosophers). PG mixes all heavily; many writers don't.

### 5. Reference horizon

The temporal and domain span of the writer's references.

**Temporal span:** in pieces with multiple dated references, what's the range? "Last week to last month" is one horizon; "1450 to 2026" is another. Sample 5 pieces and note the range.

**Domain span:** within a single piece, how many distinct domains does the writer reference? Programming, biology, economics, history, art — count distinct domains per piece. Some writers stay in one; PG roams across four or five in a single essay.

Capture: dominant temporal horizon, dominant domain count, and whether they're correlated (some writers stay narrow on both; some go wide on both).

### 6. Self-reference pattern

When "I" appears in the corpus, what's it doing? Sample 30+ instances of "I" and tag each:

- **Confident assertion.** "I think the real reason is…" — the writer using "I" to claim authority.
- **Personal anecdote.** "When I was at Y Combinator…" — the writer drawing on experience.
- **Admitted uncertainty.** "I'm not sure, but…" — the writer flagging doubt.
- **Procedural.** "In this essay I'll argue…" — the writer narrating the piece.
- **Opinion qualifier.** "I prefer X to Y." — the writer marking preference.

A writer who uses "I" 4,846 times across a corpus (PG) is texturally different from one who avoids it. *But* — the more interesting fingerprint is the *distribution*. PG uses "I" for confident assertion *and* anecdote; many writers reserve it for one. Capture the mix percentages.

### 7. Term-coining propensity

Does the writer invent terms, and at what rate?

A coined term has three signatures:
- It's introduced with a definition or naming move ("call this X", "I'll call it X", "X is when…")
- It's used multiple times in the piece (sometimes across multiple pieces)
- It compresses a concept the writer is arguing for

Examples: "Schlep blindness", "maker's schedule", "default alive", "ramen profitable" (PG). "Disruptive innovation" (Christensen). "Adjacent possible" (Johnson). "Stock vs. flow" (Forte).

Count coined terms across the corpus, normalize per 10,000 words. Note which structural slot the coining tends to occupy: the title, the opening, midway, the closing punchline. Some writers coin at the title and never elsewhere; some coin mid-paragraph.

For most writers this number is near zero — coining is rare. The writers who coin are highly identifying because of it.

### 8. Meta-commentary on the writing itself

Does the writer step outside the argument to discuss the writing as writing? Sample for moments like:

- "This is the kind of thing essays are for."
- "I'm not sure how to say this except…"
- "Bear with me — this is going to be a digression."
- "If you're skimming, the answer is X; if you want the reasoning, read on."
- Reflections on the medium ("blog posts can do what books can't")

Count instances across the corpus. For most writers this is zero. For writers who do it, it's often a small but very identifying pattern — meta-commentary is rare in default prose and shows up only when the writer is self-conscious about the form.

### 9. Negation-as-thesis

Some writers structure their thesis as a *negation* of conventional wisdom rather than a positive claim. The negation is more prominent than what's affirmed.

Examples:
- "The way to get ideas is *not* to try to think of ideas." (Negation thesis.)
- "Wealth is *not* the same thing as money." (Negation thesis.)
- Compare to: "Wealth comes from creating value." (Positive thesis.)

Both versions might appear in the same piece, but the *opening* and the *thesis sentence* can lean either way. Sample piece openings: what fraction start with negation-of-conventional-view vs. positive-claim? PG leans heavily negation-first. Some writers never do.

### 10. Definition-by-compression

The move of substituting a surprising or compressed claim for a familiar term. The compressed definition often becomes the piece's central tool.

Examples:
- "Brand is what's left when the substantive differences between products disappear." (PG)
- "Branding is centrifugal; design is centripetal." (PG)
- "Code is just the side effect of thinking." (Programmer aphorism.)

Sample the corpus for "X is Y" constructions where Y is a surprising substitution. Count density. Note: this overlaps with the cognitive-moves *reframe* category but is a sharper, more lexicalized move — the writer creates a substitution that travels.

### 11. Aphorism placement

When the writer drops a compressed principle (an aphorism), where in the piece structure does it tend to land?

- **Opening.** The piece opens with the aphorism, then unpacks it.
- **Midway.** The aphorism arrives as the result of a setup.
- **Closing.** The aphorism is the final line.
- **Mixed.** No consistent slot.

Sample 5–10 pieces. The slot is often consistent within a writer. Some writers open with aphorisms; some save them for closes. The placement is structural.

### 12. Footnote habit (long-form writers)

For writers whose corpus includes long-form essays: do they use footnotes? If yes:

- **Density** — footnotes per 1000 words.
- **Function** — citation / digression / joke / counter-claim / correction / shoutout.
- **Tonal contrast** — is the footnote register the same as the main text, or shifted (more casual, more honest, drier)?
- **Length** — short pointer notes vs. multi-paragraph digressions.

PG's footnotes are characteristically funnier and more digressive than his main text. Some writers use footnotes only for citations. The footnote habit, when present, is one of the most identifying long-form patterns.

For writers with no long-form in the corpus, mark as N/A.

## Extraction prompts

When reading the corpus, ask these questions explicitly:

1. Sample 10 piece openings. What's the most common shape (paradox / anecdote / question / scene-setting / direct thesis / historical / definition / negation / quoted / numerical)? Note the runner-up.
2. Trace the full structural arc of 5 representative pieces. What sequence of moves does the writer cycle through? Is the arc the same across pieces?
3. In 5 pieces, count scale shifts (concrete↔abstract). What's the rate per 1000 words? What's the dominant direction?
4. Across 30+ examples in the corpus, what fraction is personal anecdote vs. historical vs. named-company vs. dated-anchor vs. hypothetical vs. data vs. domain-transfer?
5. In 5 pieces, what's the temporal span of references (range from earliest to latest dated reference)? How many distinct domains per piece?
6. Across 30+ instances of "I" in the corpus, what fraction is confident-assertion vs. anecdote vs. uncertainty vs. procedural vs. opinion?
7. Count coined terms across the corpus. Normalize per 10,000 words. What slot do they typically occupy?
8. Sample for meta-commentary on the writing itself. Count instances.
9. Sample piece thesis sentences. What fraction lead with negation-of-conventional vs. positive-claim?
10. Sample the corpus for "X is Y" surprising-substitution definitions. Count density.
11. In 5–10 pieces, where do aphorisms land — opening, midway, closing, or no consistent slot?
12. For long-form writers: footnote density, function, tonal contrast to main text.

## Profile section format

Add as Section 4 of the profile template (between Cognitive moves and Quantitative layer). Position matters: rhetorical structure rules influence what gets assembled at the essay scale, so they need to be in mind during planning, not bolted on after drafting.

```markdown
## 4. Rhetorical structure & essay arc

The essay-scale shape — how the writer assembles paragraphs into pieces. See `references/rhetorical-structure.md` for methodology. Each rule has at least two quoted instances or a density figure across multiple pieces.

### 4.1 Opening shape
- Dominant opening shape: <e.g., "paradox / counterintuitive claim — 12 of 20 sampled openings">
- Runner-up: <e.g., "scene-setting — 4 of 20">
- Instances:
  - "<piece A opening, with brief context>"
  - "<piece B opening>"

### 4.2 Argument arc (full essay shape)
- Dominant arc: <named arc, e.g., "problem → reframe → mechanism → examples → derived advice">
- Alternates: <list any secondary arcs the writer also uses>
- Instances:
  - <piece 1 arc traced>
  - <piece 2 arc traced>

### 4.3 Scale-shifts
- Rate per 1000w: <N>
- Dominant direction: <concrete→abstract / abstract→concrete / balanced / sustained-one-scale>
- Sample shifts from corpus:
  - "<example of a shift>"
  - "<example>"

### 4.4 Example-texture mix
- Personal anecdote: <%>
- Historical figure / event: <%>
- Named company / product: <%>
- Dated anchor: <%>
- Hypothetical scenario: <%>
- Numerical / data point: <%>
- Domain transfer: <%>
- Quoted source: <%>
- Imagined dialogue: <%>
- (Sum to ~100%. Note dominant categories.)

### 4.5 Reference horizon
- Temporal span: <typical range, e.g., "recent decade to ~2000 years, with frequent classical references">
- Domain span: <e.g., "3–5 distinct domains per piece on average">
- Domains observed in corpus: <list>

### 4.6 Self-reference pattern
- "I" frequency: <Nx in corpus, or per 1000w>
- Distribution:
  - Confident assertion: <%>
  - Personal anecdote: <%>
  - Admitted uncertainty: <%>
  - Procedural ("In this essay I'll…"): <%>
  - Opinion qualifier: <%>

### 4.7 Term-coining propensity
- Coined terms per 10,000 words: <N>
- Examples from corpus: "<term 1>", "<term 2>", "<term 3>"
- Typical structural slot: <opening / mid-paragraph / closing / title / scattered>
- (Or: "no coining observed.")

### 4.8 Meta-commentary on the writing itself
- Instances per 10,000 words: <N>
- Examples:
  - "<quote 1>"
  - "<quote 2>"
- (Or: "no meta-commentary observed.")

### 4.9 Negation-as-thesis frequency
- % of piece openings or thesis sentences that lead with negation-of-conventional: <%>
- Examples:
  - "<thesis 1>"
  - "<thesis 2>"

### 4.10 Definition-by-compression
- Density per 10,000 words: <N>
- Examples:
  - "<compressed definition 1>"
  - "<compressed definition 2>"

### 4.11 Aphorism placement
- Dominant slot: <opening / midway / closing / mixed>
- Sample placements:
  - <piece 1: where the aphorism lands>
  - <piece 2: where the aphorism lands>

### 4.12 Footnote habit (long-form only)
- Density per 1000 words: <N>
- Function: <citation / digression / joke / counter-claim / correction>
- Tonal contrast to main text: <same register / drier / funnier / more honest>
- (Or: N/A — no long-form in corpus.)
```

For thin corpora (<5 pieces or <3000 words), use an abridged version capturing 4.1 (opening shape), 4.2 (arc), 4.4 (example-texture mix in summary form), and 4.7 (coining). Skip the density-based items where the sample is too small to be reliable.

## How this layer gets used in Mode B

The rhetorical-structure layer is consulted *before* drafting begins, in the planning phase. Read order:

1. Read banned-words and anti-performative (Sections 1, 2) — what cannot appear.
2. Read cognitive moves (Section 3) — what operations to perform on the topic.
3. **Read rhetorical structure (Section 4) — what shape the piece will take.** Before writing a single paragraph, decide: opening shape, argument arc, example-texture mix to aim for, whether to coin a term, where aphorisms will land. The piece's macro shape gets planned here.
4. Then quantitative (Section 5), vocabulary (Section 6), sentence structure (Section 7) — for the drafting.
5. Draft.
6. Self-review includes a rhetorical-structure sub-check (Pass 3) — does the draft follow the planned arc? Did the opening shape land? Was the example texture mix maintained?

## Common failure modes

- **Manufacturing a coined term to seem original.** If the writer's coining rate is near zero, don't invent a term to feel PG-like. Match the rate.
- **Importing PG's arc when the writer doesn't use it.** Problem→reframe→mechanism is PG's; don't force it onto a writer with a different arc.
- **Scale-shift overuse.** A writer with 2 shifts per 1000w can read AI'd if generation hits 8. Match the rate.
- **Reference-horizon contamination.** Don't import PG's roaming-across-centuries habit into a writer who stays in one decade.
- **Meta-commentary cranking.** Most writers don't comment on their writing. Don't add it unless the corpus does.
- **Aphorism placement inversion.** A writer who closes with aphorisms reads wrong if you open with them.
- **Opening shape default.** Claude's default opening is a scene-setting hook ("In today's fast-paced world…"). If the writer opens with paradox or negation, you have to actively swap. Don't drift to the default.
- **Footnote cosplay.** Don't add footnotes to mimic long-form when the writing task is short-form. Footnote habit is format-specific.
