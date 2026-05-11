---
name: ghostwriter
version: "1.0.0"
repo: "https://github.com/mreza0100/ghost-writer"
description: Use when the user wants to extract a reusable writing-style profile from a corpus, generate text in a specific person's style, audit or update an existing voice profile, or humanize AI-sounding text via the bundled human profile. Trigger on phrases like "match my writing style", "write like this", "make it sound like me", "voice profile", "voice DNA", "audit/update my style profile", or when the user pastes a substantial sample and asks for new text in the same voice. Do not use for generic copyediting, grammar cleanup, or broad tone shifts; this skill is for evidence-grounded reproduction of a writer's mechanical fingerprint, cognitive moves, rhetorical structure, and vocabulary.
---

# Ghostwriter

Capture the mechanical fingerprint of how someone writes — vocabulary, sentence rhythm, punctuation density, formatting quirks — from a corpus, generate new text that reproduces those patterns, and maintain the profile as the writer's habits drift.

The corpus is the **source of truth**. Every rule comes from the corpus, not from priors about what good writing looks like.

## Bundled references

Read these when the situation calls for them. Don't try to keep them all in working memory.

- `references/llm-isms.md` — 29-pattern catalog of LLM-tells. Used during Mode A (corpus sanity-check), Mode A.5 (canonical definition of the `LLM_ISM` tag), and Mode B (pre-delivery self-check).
- `references/extraction-checklist.md` — corpus preparation rules and the 8-dimension mechanical extraction grid. Read before starting Mode A.
- `references/cognitive-moves.md` — extraction prompts and categories for the idea-level layer (framing, reasoning, concretization, rejections, conclusion shape, audience assumptions, argument shape). Read before extracting any non-trivial profile; read again when generating in Mode B.
- `references/rhetorical-structure.md` — the essay-arc layer that sits between cognitive moves (idea-level) and the mechanical/vocabulary layers (word/sentence-level): opening shape, full argument arc, scale-shifts, example-texture mix, reference horizon, self-reference patterns, term-coining propensity, meta-commentary, negation-as-thesis, definition-by-compression, aphorism placement, footnote habit. The layer that captures *what shape the piece takes* — easy to miss and central to writers like Paul Graham whose recognizability is largely macro-scale.
- `references/vocabulary-fingerprint.md` — the lexical layer: verb preferences, hedge vocabulary, intensifiers, synonym binaries, casualism markers, sentence-final and topic-shift vocabulary. Read during Mode A vocabulary extraction; read again during Mode B drafting. This is where classical stylometry's identifying signal lives.

## What this skill is, and isn't

This skill captures four layers from a corpus and reproduces them in new text:

1. **The mechanical fingerprint** — pet phrases, sentence shapes, punctuation rates, formatting quirks. Observable, quotable, density-tracked.
2. **The cognitive moves** — the repeatable operations the writer performs on an idea before assembling words: how they frame problems, how they test claims, where they concretize, what they refuse, how they shape conclusions.
3. **The rhetorical structure** — the essay-scale shape: opening pattern, full argument arc, scale-shifts, example-texture mix, reference horizon, self-reference patterns, term-coining, meta-commentary, negation-as-thesis, aphorism placement, footnote habit. This is the layer that captures what shape the piece takes as it unfolds — the macro layer above sentences and below ideas.
4. **The vocabulary fingerprint** — the specific words the writer reaches for when alternatives exist: verb preferences, hedges, intensifiers, synonym binaries, casualisms.

All four layers must be evidence-grounded. Every rule requires at least two quoted instances from the corpus.

This skill is *not* a persona-direction skill. It does not capture worldview, opinions, values, beliefs, or vibe descriptors ("warm", "snarky", "earnest"). Those are downstream of cognitive moves and they read fake when ported into a different context. The distinction matters: a cognitive move is "reflexively asks 'compared to what?' before accepting a comparison" — observable in two quoted moments. A vibe descriptor is "is skeptical" — a label, not a move. The move is in scope; the label is not.

This split — mechanics + moves but no persona — is a deliberate position. McFarland's voice plugin argues that pure mechanics produce monotonous output and prefers persona; Dumont's argues for pure mechanics. The cognitive-moves layer is the bridge: the upstream layer that shapes word assembly, while staying as auditable as the mechanics. If output still feels off after both layers are dialed in, the gap is genuinely in persona, and the user should pair this skill with a separate persona prompt.

Voice doesn't operate alone, either. In a content-creator setting, it's one axis of a triple: voice (how it sounds and thinks) + audience (who it's for) + business context (what it's selling or building toward). This skill owns voice. If the user needs the other two, point them at separate ICP and business-profile work.

## Modes

Pick the mode based on what the user asked for. Modes can chain in one response.

- **Mode A — Extract.** Corpus → structured profile (saved as markdown).
- **Mode A.5 — Calibrate.** Profile → calibration samples → tagged feedback → revised profile.
- **Mode B — Generate.** Profile (or sample + prompt) → new text in that style + audit note.
- **Mode C — Audit.** Existing profile + N recent pieces → drift report (4 buckets: strong / thin / missing / fix).
- **Mode D — Update.** Existing profile + audit findings or feedback → revised profile + changelog.

If the user gives a sample and asks for new text in one go, do A then B. If they bring a profile that's been around for weeks and want it tuned to recent work, do C then D. **Profiles compound with iteration** — every audit-update cycle makes the profile sharper. Treat it as a living document, not a one-shot artifact.

## Built-in profiles

One profile ships with the skill. It lives at `profiles/human.md` and is the default fallback.

- **`human`** — the **negative profile**. Instead of capturing one writer's fingerprint, it bans the full 29-pattern LLM-ism catalog (see `references/llm-isms.md`) and sets human-typical density ranges (burstiness σ ≥ 7, em-dash ≤ 3/1000w, no rule-of-three lists, etc.). Use it when:
  - The user asks to "humanize" some AI-sounding text — no specific writer involved, just remove the tells.
  - The user wants generic-but-human writing and hasn't profiled anyone yet.
  - A specific person's profile is too thin to use confidently — fall back to `human` and prompt for more samples.

This is a different *kind* of profile from a person profile — it has no pet phrases, no fingerprint to reproduce, no signature moves. The whole point is the absence of identifiable patterns plus the LLM-tell ban list. Treat it as the floor: any person profile should at least clear what `human` clears, and add a fingerprint on top.

Don't run Mode A.5 (calibration) or Mode C (audit) on `human` — there's no source corpus to drift from. Mode B is the only mode that meaningfully applies.

If the user asks to write something and doesn't specify a profile, default to `human` rather than asking. Quietly mention which profile was used in the "Rules applied" note so they can swap if they wanted a specific person.

## Core principles

### The corpus is the source of truth

Every rule must be grounded in evidence. Attach a short quoted example AND a frequency to each rule — not "uses em-dashes" but "em-dash ~3 per 1000 words; e.g., 'and then — without warning — it stopped'". If you can't find a quote that demonstrates the rule, the rule isn't really there. Drop it. **Under-claiming beats over-claiming.**

The alternative — a confident-sounding profile of generic "good writing" rules — is the most common failure mode. It will make generated text sound like default-Claude prose with a costume on, not the actual person.

### Density, not presence

For every recurring quirk, capture **the rate**. "Em-dashes ~3/1000w", not "uses em-dashes". "Sentence fragments ~1 in 12 sentences", not "uses fragments". When generating, match the *rate* the writer actually uses — caricature is the failure mode of presence rules.

### VOICE vs PLATFORM vs BORDERLINE

Before recording any rule, classify it. Mark the bucket in the profile.

- **VOICE** — a personal pattern that travels with the writer across formats. ("Starts paragraphs with 'so'.", "ends emails with 'cheers,'.")
- **PLATFORM** — a convention of the medium the corpus came from, not the writer. (Slack: short single-sentence messages; LinkedIn: line breaks every sentence; academic: passive voice and hedging.)
- **BORDERLINE** — could be either; flag and ask the user, or note as low-confidence.

If you encode platform conventions as personal voice, the imitation will read fine in the original medium and feel wrong everywhere else. With a single-medium corpus, default ambiguous patterns to PLATFORM unless the pattern is unusual enough that it's clearly the writer's own.

### What NOT to capture

- **Opinions, beliefs, values.** "Believes in transparency." "Pro-open-source." These are downstream of cognitive moves and read fake when ported.
- **Subject matter / topics.** A corpus about cooking → "writes about food" is not a style rule.
- **Voice / tone / personality descriptors** ("warm", "snarky", "earnest", "thoughtful", "contrarian"). Subjective labels. The cognitive-moves layer captures the *moves* that produce these impressions, with quotes; the labels themselves are out.
- **Coarse cognitive archetypes** ("thinks like an engineer", "approaches things like a journalist"). Too broad to be useful; quote specific moves instead.
- **Generic good-writing or good-thinking virtues** ("uses active voice", "considers counterarguments"). Only include if the corpus shows them as *distinctively* present, with numbers or two quoted instances.
- **Platform conventions** confused as personal voice.
- **LLM-isms in the corpus.** If the corpus has clusters of patterns from `references/llm-isms.md`, verify with the user before encoding any of them.

The line between an in-scope cognitive move and an out-of-scope vibe descriptor is the evidence rule: a move is something you can quote two distinct instances of. A vibe descriptor is something you'd have to *argue* the writer demonstrates. If you're arguing, it's not in scope.

---

## Mode A: Extract a style profile

### Step 1: Vet the corpus

Read `references/extraction-checklist.md` for the full corpus rules. The short version:

- **10+ documents, 2+ formats** is the floor. Single-format corpora encode the format's conventions as voice.
- **No AI-generated content.** AI in the corpus poisons extraction.
- **Recency:** prefer the last 2 years.
- **Length variety:** include short and long pieces.
- **Single author:** all samples written by the person being profiled.

If the corpus fails any rule, say so explicitly in the profile's *Confidence notes*. A tentative profile from a thin corpus is honest; a confident one is a lie.

### Step 2: Read along the 8 mechanical dimensions

Run through the dimensions in `references/extraction-checklist.md`: sentence patterns, opening patterns, vocabulary fingerprint, structural patterns, tone markers, formatting habits, language-specific patterns, LLM-ism scan. Treat each independently — patterns in one don't predict patterns in another.

### Step 2b: Read along the cognitive-moves dimensions

Run through the categories in `references/cognitive-moves.md`: framing moves, reasoning moves, concretization tendencies, reflexive rejections, conclusion shape, audience assumptions, argument shape. Use the extraction prompts at the end of that file — they surface moves that a one-pass mechanical read misses.

Apply the same evidence discipline as the mechanical layer: a cognitive move is only a rule if you can quote two distinct moments from the corpus where the writer demonstrably uses it. If you'd have to argue the move is present, it isn't a rule.

### Step 2c: Extract the rhetorical structure

Run through the 12 categories in `references/rhetorical-structure.md`: opening shape, argument arc, scale-shifts, example-texture mix, reference horizon, self-reference pattern, term-coining propensity, meta-commentary, negation-as-thesis, definition-by-compression, aphorism placement, footnote habit.

Use the 12 extraction prompts at the end of that file. Two are especially high-leverage:

- **Sample 10+ piece openings and classify the dominant shape** (paradox / anecdote / question / scene-setting / direct thesis / historical / negation). Most writers have one dominant opening shape across 60%+ of their pieces. Catching this captures a huge amount of macro-scale fingerprint in one move.
- **Trace the full structural arc of 5 representative pieces.** The arc — "problem → reframe → mechanism → examples → derived advice", or "anecdote → generalization → return-to-anecdote", or whatever shape applies — is one of the most identifying patterns in the corpus and is invisible at the sentence level.

This layer is the most-often-missed layer in style imitation. Writers like Paul Graham are recognizable largely at the macro-scale (paradox openings, scale-shifts from coffee-maker to civilizational, terms coined in passing, footnotes drier than the main text). A profile that nails mechanics and vocabulary but skips this layer will produce sentences that sound right inside paragraphs that don't.

For thinner corpora (<5 pieces or <3000 words), use the abridged extraction in that file — keep 4.1 (opening shape), 4.2 (arc), 4.4 (example-texture mix), 4.7 (coining); skip the density-based items.

### Step 2d: Extract the vocabulary fingerprint

Run through the 12 categories in `references/vocabulary-fingerprint.md`: top content lexicon, function-word patterns, verb preferences, hedge vocabulary, intensifiers, synonym binaries, casualism markers, profanity, sentence-final vocabulary, topic-shift vocabulary, question vocabulary, and lexical banned-by-omission.

Use the 12 extraction prompts at the end of that file. The single highest-leverage section is **6.6 Synonym binaries** — when the writer had a choice between two common alternatives (use/utilize, help/assist, but/however, weird/strange), which side did they pick consistently? Aim for 8–15 binaries from a substantive corpus. Applying these during generation is one of the fastest ways to make output sound like the writer.

For thinner corpora (<5000 words OR <10 pieces), do the abridged extraction described in that file — skip top-N frequency tables, keep verb preferences, hedges, synonym binaries, casualisms, topic-shift, and pet phrases.

### Step 3: Compute the quantitative layer

Numbers Claude can compute directly:

- Avg sentence length and **burstiness** (= standard deviation of sentence length). High burstiness — the writer alternates 5- and 35-word sentences — is a recognizable rhythm. Low burstiness — uniform 18 ± 4 — is another. Both are voice.
- Avg paragraph length in sentences.
- Type-token ratio (first 500 words of long-form samples).
- Em-dash, semicolon, colon, ellipsis rates per 1000 words.
- Contraction rate (actual / eligible).
- Hedge-word rate per 1000 words.
- Top 5 sentence-initial connectors with counts.
- Exclamation rate per 1000 words.

Don't fake precision. Small corpus → say so and round.

### Step 4: Classify and rate every rule

For each rule: quoted example, density figure, VOICE / PLATFORM / BORDERLINE, confidence (high / medium / tentative).

### Step 5: Save the profile

Default path: `.claude/skills/ghostwriter/profiles/<name>.md`. If not writeable, fall back to the user's outputs folder. Show the rules in chat as well.

### Profile template

Position matters. **Banned-words and never-say lists go first.** Claude reads top-down, and earlier constraints have stronger influence on generation. Format-specific modes go last so they don't bleed across formats.

```markdown
# Style Profile: <name or context>

Source corpus: ~<word count> words across <N> documents in <list formats>. Date range: <YYYY-MM> to <YYYY-MM>.
Profile created: <YYYY-MM-DD>. Last audit: —.

## 1. Banned words & phrases (never-say list)

Words and phrases the writer demonstrably avoids. These go first because position matters: a banned-word rule encountered early in this profile is far less likely to be generated.

- **Banned words:** "<word>" (0 occurrences in <N> words; the writer uses "<alternative>" instead)
- **Banned phrases:** "<phrase>"
- **Banned constructions:** <pattern, e.g., "rule-of-three lists with parallel structure">

Source: explicit absences in the corpus + LLM-ism patterns from `llm-isms.md` confirmed absent.

## 2. Anti-performative rules

When imitating a writer's style, the failure mode is exaggerating one-time patterns into theatrical signature moves. Explicit guards against this:

- One-time usages in the corpus are NOT signature moves. Don't manufacture catchphrases from a single occurrence.
- Don't announce the writing ("Let's dive in," "Here's the thing"). The corpus shows the writer just starting with content.
- Don't perform casualness. If the writer is casual, they're casually casual, not theatrically casual.
- Match densities; don't crank.

## 3. Cognitive moves & frames

The repeatable operations the writer performs on an idea before assembling words. Position matters: these rules sit above the mechanical sections because they shape *what* gets assembled, not just how. See `references/cognitive-moves.md` for the extraction methodology. Each rule below has two quoted instances and a move-type classification.

### Framing moves
- [VOICE | high] <Move>. Type: <reframe / reject / steelman / concretize / zoom-out / reduce>. Instances:
  - "<quote 1 with brief context>"
  - "<quote 2 with brief context>"

### Reasoning moves
- [VOICE | medium] <Move>. Type: <counterexample / comparison / inversion / incentives / falsification / data / mechanism / constraint>. Instances:
  - "<quote 1>"
  - "<quote 2>"

### Concretization tendencies
- [VOICE | high] <Pattern, with density e.g., "every abstract claim paired with a numbered example, 4 of 4 pieces">. Instances:
  - "<quote 1>"
  - "<quote 2>"

### Reflexive rejections (things the writer demonstrably refuses)
- [VOICE | high] <Refusal>. Two cases where the writer had the option and didn't take it:
  - "<context 1>"
  - "<context 2>"

### Shape of conclusion
- [VOICE | high] <Shape>. Type: <action / open-question / compression / hedge / cut-off>. Closing sentences from two pieces:
  - "<closing 1>"
  - "<closing 2>"

### Audience assumptions
- [VOICE | medium] <What the writer doesn't explain that they could>. Evidence: <a technical term used without definition, a reference dropped without context, etc.>

### Argument shape
- [VOICE | high] <Shape>. Type: <stair-step / story-moral / thesis-first / inquiry / empirical / argumentative>. Two pieces showing it:
  - <how piece 1 is structured>
  - <how piece 2 is structured>

## 4. Rhetorical structure & essay arc

The essay-scale shape — how the writer assembles paragraphs into pieces. Position matters: these rules influence what gets assembled at the macro scale, so they need to be in mind during the planning phase of generation. See `references/rhetorical-structure.md` for methodology. Each rule has at least two quoted instances or a density figure across multiple pieces.

### 4.1 Opening shape
- Dominant: <e.g., "paradox / counterintuitive claim — 12 of 20 sampled openings">
- Runner-up: <e.g., "scene-setting — 4 of 20">
- Instances:
  - "<piece A opening, with brief context>"
  - "<piece B opening>"

### 4.2 Argument arc (full essay shape)
- Dominant arc: <e.g., "problem → reframe → mechanism → examples → derived advice">
- Alternates: <list any secondary arcs>
- Instances:
  - <piece 1 arc traced>
  - <piece 2 arc traced>

### 4.3 Scale-shifts
- Rate per 1000w: <N>
- Dominant direction: <concrete→abstract / abstract→concrete / balanced / sustained-one-scale>
- Sample shifts:
  - "<example>"
  - "<example>"

### 4.4 Example-texture mix
Percentages roughly summing to 100% across 30+ examples sampled.
- Personal anecdote: <%>
- Historical figure / event: <%>
- Named company / product: <%>
- Dated anchor: <%>
- Hypothetical scenario: <%>
- Numerical / data point: <%>
- Domain transfer: <%>
- Quoted source: <%>
- Imagined dialogue: <%>
- Dominant categories: <list top 2–3>

### 4.5 Reference horizon
- Temporal span: <typical range, e.g., "last decade to ~500 years; frequent classical references">
- Domain span: <e.g., "3–5 distinct domains per piece on average">
- Domains observed: <list>

### 4.6 Self-reference pattern
- "I" frequency: <Nx in corpus, or per 1000w>
- Distribution across 30+ sampled instances:
  - Confident assertion: <%>
  - Personal anecdote: <%>
  - Admitted uncertainty: <%>
  - Procedural: <%>
  - Opinion qualifier: <%>

### 4.7 Term-coining propensity
- Coined terms per 10,000 words: <N>
- Examples: "<term>", "<term>", "<term>"
- Typical structural slot: <opening / mid-paragraph / closing / title / scattered>
- (Or: "no coining observed.")

### 4.8 Meta-commentary on the writing itself
- Instances per 10,000 words: <N>
- Examples:
  - "<quote>"
  - "<quote>"
- (Or: "no meta-commentary observed.")

### 4.9 Negation-as-thesis frequency
- % of piece openings or thesis sentences leading with negation-of-conventional: <%>
- Examples:
  - "<thesis>"
  - "<thesis>"

### 4.10 Definition-by-compression
- Density per 10,000 words: <N>
- Examples:
  - "<surprising-substitution definition>"
  - "<surprising-substitution definition>"

### 4.11 Aphorism placement
- Dominant slot: <opening / midway / closing / mixed>
- Sample placements:
  - <piece 1: where the aphorism lands>
  - <piece 2: where the aphorism lands>

### 4.12 Footnote habit (long-form only)
- Density per 1000 words: <N>
- Function: <citation / digression / joke / counter-claim / correction>
- Tonal contrast to main text: <same / drier / funnier / more honest>
- (Or: N/A — no long-form in corpus.)

## 5. Quantitative layer

- Avg sentence length: <N> words; **burstiness (σ)**: <N>
- Avg paragraph length: <N> sentences
- Type-token ratio (first 500 words): <N>
- Em-dashes / 1000w: <N>; semicolons / 1000w: <N>; colons / 1000w: <N>; ellipses / 1000w: <N>
- Contraction rate: <%>
- Hedge-word rate / 1000w: <N>
- Exclamation rate / 1000w: <N>
- Top sentence-initial connectors: "<x>" (Nx), "<y>" (Nx), "<z>" (Nx)

## 6. Vocabulary fingerprint

The lexical layer — the specific words the writer reaches for when alternatives exist. See `references/vocabulary-fingerprint.md` for extraction methodology. Use the full structure below for substantive corpora (≥5000 words AND ≥10 pieces); for thinner corpora keep sections 6.3, 6.4, 6.6, 6.7, 6.10, and pet phrases, and skip the frequency tables in 6.1 and 6.2.

### 6.1 Top content lexicon
- Top content words (excluding stopwords; 30 for substantive corpora, 15 for thinner):
  "<word>" (Nx), "<word>" (Nx), …
- Words that travel across pieces (voice, not topic): "<word>", "<word>"

### 6.2 Function-word patterns
- Standout function-word patterns: "<observation, e.g., 'but' outranks 'however' 12:1>"
- Pronoun preferences: I/we/you ratios if distinctive.

### 6.3 Verb preferences
- Top 20 verbs by frequency.
- Plain-to-elevated ratio: <e.g., "85% plain (use, make, do); 1 'utilize' in 8000 words">
- Identifying verb picks: "<verb>" over "<alternative>" — Nx in corpus.

### 6.4 Hedge vocabulary
- Top hedges with counts: "<hedge>" (Nx), "<hedge>" (Nx)
- Hedges the writer demonstrably avoids: "<hedge>"

### 6.5 Intensifier vocabulary
- Top intensifiers with counts: "<word>" (Nx)
- Intensifiers avoided: "<word>"

### 6.6 Synonym binaries (the diagnostic table)
- <plain> / <elevated> → uses "<plain>" (Nx vs Nx)
- [list 8–15 binaries observed in the corpus]

### 6.7 Casualism / internet markers
- (If present) "<marker>" at <density>
- (If absent — consistently formal register) Note explicitly: no casualism markers observed.

### 6.8 Profanity
- Rate and specific words, OR explicit "no profanity in corpus."

### 6.9 Sentence-final vocabulary
- Dominant shape: <hedge-end / concrete-end / intensifier-end / mixed>
- Sample endings from corpus.

### 6.10 Topic-shift vocabulary
- Most-used topic-shift word: "<word>" (Nx)
- Other shifts observed, or: no consistent topic-shift word.

### 6.11 Question vocabulary
- Dominant question shape: "<shape>"
- Sample questions from corpus.

### 6.12 Banned-by-omission (lexical)
- Words the writer demonstrably avoids at the lexical level: "<word>", "<word>"

### Pet phrases (multi-word recurring units)
- "<phrase>" (Nx), "<phrase>" (Nx)

## 7. Sentence structure & rhythm

- [VOICE | high] <Rule>. Example: "<short quote>"
- [BORDERLINE] <Rule, flagged>. Example: "<short quote>"
- Paragraph shape: <description>.

## 8. Quirks & idiosyncrasies

- [VOICE | high] <Rule, with rate>. Example: "<short quote>"
- [VOICE | tentative] <Rule>. Example: "<short quote>"

## 9. Negative rules (patterns demonstrably absent)

- <Pattern absent — only if absence is striking. e.g., "0 semicolons in 1200 words.">
- <e.g., "Never uses 'utilize'; uses 'use'.">

## 10. Default mode

The writer's default register when context isn't specified. Pick one:
- **Informational** — direct, factual, no narrative arc. Shorter, punchier. Set this as default if Slack/email/chat dominates the corpus.
- **Narrative** — story arcs, scene-setting, build-up. Longer paragraphs. Set this as default if blog posts / long-form dominates.
- **Mixed** — switches by format. Note which format gets which.

Document the default explicitly. Without this, generation applies one register uniformly and gets it wrong half the time.

## 11. Format-specific modes

Same voice, different registers. Without this, every output reads like the writer's most-represented format.

- **Blog / long-form:** <register notes, e.g., "narrative, 3–5 sentence paragraphs, occasional headers">
- **Email:** <e.g., "no greeting on internal team threads; brief greeting + one-line context on external">
- **Slack / chat:** <e.g., "lowercase sentence starts, no closing, often single-sentence messages">
- **Social / public posts:** <e.g., "first sentence carries the hook; no hashtags">

## 12. Voice in action (prompt-ready examples)

Three short examples that demonstrate the voice across the formats above. These compress all the rules into something the user can read and recognize.

- **Long-form opening (~50 words):** <generated example>
- **Email (~3 sentences):** <generated example>
- **Slack message (~1–2 sentences):** <generated example>

## 13. Confidence notes

- <Rules tentative because evidence was thin.>
- <Aspects that couldn't be read confidently from this corpus's coverage.>
- <Open questions for the user.>

## Changelog

- <YYYY-MM-DD> Created from <source>. <N> docs, <list formats>.
```

---

## Mode A.5: Calibration round (run after first extraction)

Extraction misses things humans only notice in fresh output. Run this every time after Mode A.

1. Generate **3 short calibration samples** in the new style — one per most-likely-use format (e.g., blog opener + email reply + Slack message). Show them in chat.
2. Ask the user to mark each issue with a tag:
   - `WRONG` — this rule is wrong; the writer doesn't actually do this
   - `OVERSTATED` — direction right, density too high
   - `UNDERSTATED` — present but weaker than reality; bump it up
   - `MISSING` — a real pattern the profile doesn't capture
   - `NEEDS_NUANCE` — true sometimes, not always; condition it
   - `LLM_ISM` — sounds like default-Claude voice leaking through (cross-reference `references/llm-isms.md` to identify which pattern)
   - `NOT_ME` — whole sample feels off; can't articulate why
3. Apply the tagged feedback. Mapping:
   - `WRONG / OVERSTATED / UNDERSTATED / NEEDS_NUANCE` → revise the rule's density or condition.
   - `MISSING` → add a new rule, with the example the user provides as evidence.
   - `LLM_ISM` → identify the catalog pattern in `llm-isms.md`, add it to Section 1 (banned).
   - `NOT_ME` → re-run extraction. Likely a structural miss in the 8 dimensions.
4. Log the change in the changelog. Re-render calibration samples once. Stop after one revision unless the user wants another pass — convergence, not perfection.

Dumont reports his profile grew 333 → 510 lines through three calibration rounds. Most of the growth was rules the automated extraction missed because they only become visible in fresh output.

---

## Mode B: Generate (or humanize) text in the style

Mode B covers two related operations that use the same machinery:

- **Generate** — input is a writing prompt; produce new text in the profile's style.
- **Humanize** — input is existing AI-sounding text; rewrite it through the profile (typically `human`) to remove LLM-tells.

Both follow the same steps below; the only difference is whether you're starting from a prompt or from an existing draft to revise.

### Choosing a profile

1. If the user names a profile, use it.
2. If the user provides a corpus and asks for new text in one go, do Mode A first (quick), then come back here.
3. If neither, default to `profiles/human.md` and note which profile was used in the Rules applied note. Don't ask which profile to use for routine requests — defaulting to `human` is the right call and trivial to override.

### Steps

1. **Read the profile top-down.** Order matters.
   - Banned words and anti-performative rules go first (Sections 1, 2) because they shape every following generation choice.
   - The **cognitive moves** section (Section 3) comes next because it shapes *how the writer would approach this specific topic*.
   - The **rhetorical structure** section (Section 4) shapes *what the piece will look like as it unfolds*: pick the opening shape, the argument arc, the example-texture mix to aim for, whether to coin a term, where aphorisms will land. Plan the macro shape *before* drafting any sentences.
   - Then quantitative (Section 5), vocabulary (Section 6) — especially Section 6.6 (synonym binaries) and 6.4 (hedges). Internalize the writer's specific verb and hedge picks before drafting, because Claude's defaults will otherwise win every micro-choice.
   - Then sentence structure (Section 7) and the remaining sections for the drafting itself.
2. If the input is existing text (humanize), read it once for content, once for which LLM-isms it contains (cross-reference `references/llm-isms.md`). Plan the rewrite at the paragraph level, not the sentence level — wholesale restructuring usually beats find-and-replace.
3. **Apply the cognitive moves to the topic before drafting words.** This is the step most likely to be skipped — don't skip it. Ask: how would this writer frame this question? What would they refuse to accept about it? Where would they concretize? What shape would their argument take? What would the conclusion look like? Do those operations first, mentally. *Then* draft.
4. Check the **priority hierarchy** (next section). Conventions of the writing context (legal, academic, regulated) can override the profile.
5. Draft (or rewrite). Reproduce profile rules at documented densities. Match densities; don't crank. For the `human` profile especially, **vary sentence length aggressively** — burstiness σ ≥ 7 is the single most important target.
6. Run the **three-pass self-review** before delivering.
7. Append the **Rules applied** note (with profile name).

### Priority hierarchy (when style and context conflict)

Personal style does not always win.

1. **Hard contextual norms** — legal briefs, academic abstracts, regulatory filings, medical chart notes, formal business letters. Form conventions override. Apply style at the margins (vocabulary, paragraph shape) where it doesn't violate the form.
2. **Audience norms** — when the audience won't tolerate the writer's quirks (rambling exec summary for a CEO who reads on a phone). Compress; keep voice cues; downscale density.
3. **Personal style** — default for blog posts, emails, social posts, casual writing.
4. **Platform conventions** — apply on top of style, only for the platform being written to. Don't import another platform's conventions.

When the trade-off kicks in, say so in the Rules applied note.

### Three-pass self-review (do this before delivering)

Three different failure modes. A single pass catches one and misses the others — they need separate attention.

**Pass 1 — LLM-ism scan.** Skim the draft against `references/llm-isms.md`. Cues to look for: stacked em-dashes, "moreover / furthermore / actually", neat tricolons, balanced paragraph lengths, "It's not just X, it's Y" constructions, "navigate the complexities", "in today's fast-paced world", chatbot closers, "great question" sycophancy. For every catalog match: is it in the corpus at this density? If yes, keep. If no, revise.

**Pass 2 — Performative scan.** Look at every place the draft has a "signature move" — a quirk applied loudly. Check the profile's anti-performative rules in Section 2: is this move actually high-density in the corpus, or did you crank a one-time tic into a catchphrase? If cranked, dial it down. The smell test: would the writer's friend roll their eyes reading this?

**Pass 3 — Moves, structure, and vocabulary pass.** Three checks in one read, each a different lens on the same draft.

*Moves sub-pass.* Read the draft *for the thinking*, ignoring the words. Did it apply the writer's characteristic cognitive moves (Section 3 of the profile)? Or did default-Claude reasoning sneak in — survey 5 angles neutrally, balance both sides, "while X has its merits, Y also has its strengths", land on a synthesis no one asked for, conclude with generic uplift? Common leaks: the writer always concretizes but your draft stayed abstract; the writer reflexively asks "compared to what?" but your draft accepted a comparison at face value; the writer's conclusion shape is "specific action item" but your draft ended in "exciting times ahead." If the mechanics are right but the thinking reads like a committee, fix the thinking.

*Structure sub-pass.* Step back and look at the *macro shape* of the draft. Did the opening shape match the writer's dominant one (Section 4.1) — did you open with paradox if they open with paradox, with anecdote if they open with anecdote? Did the full argument arc match (Section 4.2)? Is the example-texture mix in roughly the right ratios (Section 4.4) — not all personal anecdote when the writer balances anecdote with historical figures and named companies? If the writer coins terms (Section 4.7), is there a coined term in the draft (or a noted absence)? Did the aphorism land in the slot the writer uses (Section 4.11)? The structure pass is the easiest to skip and the easiest to spot in retrospect — generated text that has the right sentences in the wrong arc reads more wrong than vice versa.

*Vocabulary sub-pass.* Now scan the *words*. Walk the draft and check: does each verb match the writer's plain-vs-elevated profile (Section 6.3)? Did the hedges and intensifiers come from the writer's lists (Sections 6.4–6.5), or did Claude defaults ("perhaps", "potentially", "arguably", "quite") sneak in? Are the synonym binaries (Section 6.6) being respected — "use" not "utilize", "but" not "however" if those were the writer's picks? Is casualism density right? Did the writer's topic-shift word appear at paragraph breaks, or default-LLM transitions? The single most common vocabulary failure: Claude reaches for the elevated synonym when the writer always picks the plain one. Fix any binary inversions before delivering.

Combined, this pass is the one most often skipped and the one that, when skipped, produces "mechanically right but conceptually a stranger" prose — or "thinking right but in the wrong shape" — or "thinking and shape right but worded by someone else."

### Common failure modes

- **Topic drift bias.** Corpus about hiking, prompt about taxes — don't import hiking metaphors. Style ≠ subject.
- **Caricature.** Match densities, not just presence.
- **Default-Claude leak.** Well-balanced, varied prose with confident verbs and tidy transitions ("moreover", "furthermore", neat tricolons) is your default voice, not theirs. Find the messier and weirder spots in the corpus and lean into them.
- **Persona invention.** Don't invent personality, opinions, or biographical hooks the corpus didn't show.
- **Length mismatch.** Infer length from the writing task itself, not from corpus length.
- **Platform contamination.** Don't import LinkedIn line breaks into an email; don't import Slack's lowercase casualness into a memo.
- **Anti-leakage.** Voice doesn't leak into places it doesn't belong (citation in academic prose, defined terms in legal prose, etc.). The hierarchy guards this.

### "Rules applied" note format

End the response with:

```markdown
---
**Profile used:** `<profile name>` (e.g., `human` default, or `<person>`)
**Operation:** generate | humanize

**Rules applied (top hits):**
- <Rule>: <how reproduced, with target density if relevant>
- <Rule>: <how reproduced>
- ...

**Self-review:**
- LLM-ism pass: <patterns found and removed, OR "none flagged">
- Performative pass: <quirks dialed back, OR "no cranking detected">
- Moves, structure, and vocabulary pass:
  - Moves: <how the writer's cognitive moves shaped this draft — e.g., "applied reframe + concretization + action-item conclusion" — OR "no specific cognitive moves applied (human profile)">
  - Structure: <opening shape used, argument arc followed, example-texture mix approximated, term coined or not — OR "structure layer not yet captured / using human defaults">
  - Vocabulary: <synonym binaries applied, hedges/intensifiers matched, OR "lexical fingerprint not yet captured / using human profile defaults">

**Trade-offs:** <if priority hierarchy kicked in, mention briefly. Otherwise omit.>
```

Keep it short — audit trail, not essay. The profile-name line matters even when defaulting to `human` — the user needs to see which profile ran so they can swap if they meant a specific person.

---

## Mode C: Audit a profile against recent writing

Use when the user has an existing profile and wants to know if it still fits how they currently write. Recommended cadence: every 6–8 weeks of active writing, or after a major shift (new job, new platform, new audience).

1. Read the existing profile.
2. Read N recent pieces (≥3 ideally; more is better). Treat as a fresh corpus.
3. Re-compute the quantitative layer on the new corpus.
4. For each rule in the profile, check: does it still hold? Sample a quote.
5. Look for new patterns in the new corpus that aren't in the profile.
6. Produce a drift report using the **4-bucket structure**:

```markdown
# Drift Report: <profile name>
Compared profile dated <YYYY-MM-DD> against <N> recent pieces (~<word count>).

## Numbers, then vs. now
| Metric | Profile | Recent | Δ |
|---|---|---|---|
| Avg sentence length | ... | ... | ... |
| Burstiness (σ) | ... | ... | ... |
| Em-dashes / 1000w | ... | ... | ... |
| ... | | | |

## Strong (rules that still hold confidently)
- <Rule>. Recent example: "<quote>"

## Thin (rules with weakening evidence)
- <Rule>. Profile rated [high]; recent corpus shows it [medium] or absent. Recent counter-example: "<quote>"

## Missing (new patterns in recent corpus, not yet in profile)
- <Rule>. Recent example: "<quote>"

## Fix (rules that have decayed or shifted, with proposed edit)
- <Rule>. Profile says X; recent corpus shows Y. Proposed: <revised rule>.

## Recommendation
- <Update / no update needed / partial update with these specific edits>
```

The 4 buckets — strong, thin, missing, fix — come from McFarland's audit framework. They make it obvious what's actionable: thin rules need evidence-or-removal; missing rules need adding; fix rules need editing; strong rules need nothing. Don't auto-update the profile in Mode C. Surfacing is the audit's job; updating is Mode D.

## Mode D: Update an existing profile

Triggered by: a Mode C drift report, Mode A.5 calibration feedback, or fresh user notes ("I've been writing more casually lately").

1. Open the profile.
2. Apply the changes — adjust densities, update or remove decayed rules, add new patterns with evidence.
3. **Append a changelog entry** with the date and a one-line summary. The changelog is how profiles compound — every cycle you can see what shifted, and you can revert if an update overshoots.
4. Show the diff in chat (additions, removals, changed densities). Let the user sanity-check.
5. Optionally re-run a Mode A.5 calibration pass to verify the updated profile generates correctly.

---

## Output format (always)

- **Mode A:** the profile (saved to file + shown in chat). Optionally followed by Mode A.5.
- **Mode A.5:** calibration samples + tag-request prompt; after feedback, revised profile + diff.
- **Mode B:** the new text + the "Rules applied" note (with three-pass self-review).
- **Mode C:** the 4-bucket drift report (saved + shown).
- **Mode D:** the diff + updated profile (saved).
- **Combined:** chain in order, separated by `---`.

## Edge cases

- **Corpus under ~150 words or single document.** Profile will be tentative; imitation may default toward generic. Offer to proceed with caveats, or ask for more.
- **Single-format corpus.** Mark all platform-conditioned patterns as PLATFORM. Warn the user that the profile will need a calibration pass when applied to other media.
- **Multilingual writer.** Extract per language; some patterns (rhythm, punctuation conventions) carry across; vocabulary mostly doesn't. Note language coverage.
- **User asks "write more like this" mid-draft.** Treat the existing fragment as a tiny corpus; do a quick extract; continue.
- **Profile passed in but parts conflict with the new sample.** Flag the conflict; ask which to honor. Don't silently override.
- **Sample contains AI-generated text.** Detection cues in `references/llm-isms.md`. Verify with the user before encoding any of them as voice. Encoding LLM tells as personal voice is the worst possible failure of this skill.
- **Writer's voice in conflict / pushback is missing from corpus.** Most corpora bias toward neutral writing; the writer in disagreement is often invisible. If the user will need the profile for adversarial writing (debates, complaints, refutations), ask for at least one piece of that kind — or flag the gap explicitly in Confidence notes.

---

## Version & Updates

**Current version:** 1.0.0
**Repository:** https://github.com/mreza0100/ghost-writer

To check for updates, compare the `version` field in your installed SKILL.md frontmatter against the latest in the repository.

**To update:**

```bash
cd /path/to/ghost-writer-repo && git pull

# Copy core skill file
cp SKILL.md /your/project/.claude/skills/ghostwriter/SKILL.md

# Copy reference files (check for new ones)
cp references/*.md /your/project/.claude/skills/ghostwriter/references/

# Copy updated profiles
cp profiles/human.md /your/project/.claude/skills/ghostwriter/profiles/
```

**Changelog:**
- **1.0.0** — First versioned release. Four-layer architecture (mechanical + cognitive moves + rhetorical structure + vocabulary fingerprint). Five modes (A/A.5/B/C/D). Built-in human profile. Paul Graham profile included.
