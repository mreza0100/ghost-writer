# Ghostwriter

A Claude Code skill that captures how someone writes — the mechanical fingerprint (sentence rhythm, punctuation density, formatting quirks), the vocabulary fingerprint (the specific words they reach for when alternatives exist), and the cognitive moves underneath it (how they frame problems, what they refuse, where they concretize, how they shape conclusions) — from a corpus of their writing, and generates new text that reproduces all three layers.

This is not persona imitation. It doesn't try to capture worldview, opinions, or vibe. It reads observable, measurable, quotable patterns at three levels and reproduces them at the documented densities. The result is auditable: every rule in a profile has at least one quoted example from the source corpus, classified by type and rated by confidence.

## Why this exists

LLMs have a default voice. You've read it: balanced paragraphs, confident verbs, "moreover," neat tricolons, tidy transitions. When you ask an LLM to "write in someone's style," it usually produces its own default prose wearing a costume — a few surface-level quirks layered on top of the same underlying rhythm, the same default vocabulary, and the same default reasoning shape.

Ghostwriter takes a different approach. It builds a structured profile from a corpus of real writing across three layers:

- **Mechanical fingerprint** — density-based rules (not just presence/absence), patterns classified as VOICE / PLATFORM / BORDERLINE, a banned-words list of LLM-isms the writer demonstrably avoids.
- **Vocabulary fingerprint** — the specific lexical choices: verb preferences, hedge vocabulary, intensifiers, synonym binaries, casualism markers, sentence-final and topic-shift vocabulary. This is the layer classical stylometry (the Federalist Papers method) leans on most.
- **Cognitive moves** — the repeatable operations the writer performs on an idea _before_ assembling words: framing moves, reasoning moves, concretization tendencies, reflexive rejections, conclusion shape, audience assumptions, argument shape.

Generated text is then checked against a 29-pattern LLM-ism catalog (surface), the writer's vocabulary fingerprint (words), and the writer's cognitive moves (thinking) before delivery.

## How it works

### Five modes

| Mode                | Input                              | Output                                 |
| ------------------- | ---------------------------------- | -------------------------------------- |
| **A — Extract**     | Corpus of writing samples          | Structured style profile (both layers) |
| **A.5 — Calibrate** | Profile + user feedback on samples | Refined profile                        |
| **B — Generate**    | Profile + writing prompt           | New text in that style + audit note    |
| **C — Audit**       | Profile + recent writing           | Drift report (4 buckets)               |
| **D — Update**      | Profile + audit/feedback           | Revised profile + changelog            |

Modes chain naturally: A then A.5 for first-time profiling. C then D for maintenance. A then B for one-shot "write like this" requests.

### The profile lifecycle

Profiles are living documents, not one-shot artifacts. They compound with iteration:

1. **Extract** from a corpus (10+ documents, 2+ formats recommended)
2. **Calibrate** with tagged feedback (WRONG, OVERSTATED, UNDERSTATED, MISSING, NEEDS_NUANCE, LLM_ISM, NOT_ME)
3. **Generate** using the profile
4. **Audit** against new writing every 6-8 weeks
5. **Update** to close drift gaps

### What a profile captures

The profile is organized into three layers, with each rule evidence-grounded by quoted examples from the corpus.

**Mechanical layer:**

- **Banned words and phrases** — patterns the writer demonstrably avoids, placed first because position affects generation
- **Anti-performative rules** — guards against exaggerating one-time patterns into theatrical signature moves
- **Quantitative layer** — sentence length, burstiness, punctuation rates per 1000 words, contraction rate, hedge-word rate
- **Sentence structure and rhythm** — paragraph shape, clause-joining patterns, opener variety
- **Quirks** — idiosyncrasies with density figures
- **Negative rules** — patterns that are strikingly absent
- **Format-specific modes** — same voice, different registers (blog vs email vs Slack)

**Vocabulary-fingerprint layer:**

- **Top content lexicon** — the words that travel across pieces (voice), not the ones tied to single pieces (topic)
- **Function-word patterns** — standout pronoun and connector ratios (classical stylometry's strongest signal)
- **Verb preferences** — plain-to-elevated ratio; identifying picks ("use" not "utilize")
- **Hedge vocabulary** — which specific hedges the writer reaches for ("I think" vs "perhaps")
- **Intensifier vocabulary** — "really / very / super" vs "quite / considerably"
- **Synonym binaries** — the diagnostic table: when alternatives exist, which side did the writer pick consistently
- **Casualism / internet markers** — "lol", emoji, lowercase starts, letter-stretching, with density
- **Profanity** — frequency, words, contexts
- **Sentence-final vocabulary** — what shape sentences land on (hedge / concrete / intensifier)
- **Topic-shift vocabulary** — how paragraph transitions get signaled
- **Question vocabulary** — how questions are phrased
- **Banned-by-omission (lexical)** — words the writer demonstrably avoids
- **Pet phrases** — multi-word recurring units with counts

**Cognitive-moves layer:**

- **Framing moves** — reframe, reject, steelman, concretize, zoom-out, reduce
- **Reasoning moves** — test by counterexample, comparison, inversion, incentives, falsification, mechanism, constraint-finding
- **Concretization tendencies** — what the writer pairs abstract claims with
- **Reflexive rejections** — what the writer refuses to accept (appeals to authority, round numbers, "everyone agrees", etc.)
- **Shape of conclusion** — action item, open question, compression, hedge, cut-off
- **Audience assumptions** — what's left unsaid because it's assumed
- **Argument shape** — the structural move-set of how a piece is built

Every rule at all three layers is tagged: VOICE (travels with the writer) / PLATFORM (convention of the medium) / BORDERLINE (ambiguous). Cognitive-move rules and vocabulary-binary rules additionally require at least two quoted instances from the corpus — single occurrences are not patterns.

### What a profile does NOT capture

- Opinions, beliefs, values
- Subject matter or topics
- Subjective vibe descriptors ("warm," "snarky," "earnest," "contrarian")
- Coarse cognitive archetypes ("thinks like an engineer")
- Generic good-writing or good-thinking virtues (unless distinctively present in the corpus)
- Platform conventions confused as personal voice

The line between an in-scope cognitive move and an out-of-scope vibe descriptor is the evidence rule: a move is something you can quote two distinct instances of. A vibe descriptor is something you'd have to argue the writer demonstrates.

## Built-in profile: `human`

The repo ships with one default profile at `profiles/human.md`. It's the **negative profile** — instead of capturing one writer's fingerprint, it bans the full 29-pattern LLM-ism catalog at the mechanical layer and the default-LLM reasoning moves (both-sides-ism, 5-angle topic surveys, reflexive synthesis, "it depends" without follow-through, false consensus framing) at the cognitive layer.

Use `human` when:

- You want to "humanize" some AI-sounding text — no specific writer, just remove the tells.
- You want generic-but-human writing and haven't profiled anyone yet.
- A specific person's profile is too thin to use confidently — fall back to `human`.

If you don't name a profile, `human` is the default.

## Installation

### As a Claude Code skill

Copy the skill files into your project's Claude Code skills directory:

```bash
# From your project root
mkdir -p .claude/skills/gwriter/references
mkdir -p .claude/skills/gwriter/profiles

cp SKILL.md .claude/skills/gwriter/SKILL.md
cp references/extraction-checklist.md .claude/skills/gwriter/references/
cp references/llm-isms.md .claude/skills/gwriter/references/
cp references/cognitive-moves.md .claude/skills/gwriter/references/
cp references/vocabulary-fingerprint.md .claude/skills/gwriter/references/
cp profiles/human.md .claude/skills/gwriter/profiles/
```

Then use it in Claude Code:

```
/gwriter profile     — extract a style profile from a corpus
/gwriter calibrate   — calibrate a profile with feedback
/gwriter generate    — generate text in a profiled style (defaults to `human`)
/gwriter audit       — audit a profile against recent writing
/gwriter update      — update a profile from audit findings
/gwriter list        — list available profiles
```

### As a standalone reference

The files work as a methodology guide even without Claude Code. `SKILL.md` contains the complete extraction and generation methodology. The `references/` directory has the LLM-ism catalog, extraction checklist, cognitive-moves methodology, and vocabulary-fingerprint methodology.

## Key design decisions

**Three layers, all evidence-grounded.** Mechanics alone produce texturally correct prose that argues like a stranger. Vocabulary alone produces the right words assembled by someone with a different mind. Cognitive moves alone produce the right thinking in default-Claude prose. The profile captures all three, with the same quote-it-or-it-isn't-real discipline.

**Density, not presence.** "Em-dashes ~7.6/1000w" is a rule. "Uses em-dashes" is not. Presence-only rules produce caricature — every quirk gets cranked to maximum.

**Banned words go first.** Position in the profile matters. Constraints encountered early have stronger influence on generation. The never-say list sits at the top. The cognitive moves come next because they shape what gets assembled, not just how. Vocabulary fingerprint comes after, with the synonym-binaries table loaded before drafting starts.

**Three-pass self-review.** Pass 1 scans for LLM-isms (the 29-pattern catalog). Pass 2 scans for performative exaggeration. Pass 3 reads the draft for the _thinking_ (did the writer's cognitive moves apply) and the _words_ (did the synonym binaries get respected; did the writer's hedges and intensifiers show up rather than Claude defaults). These are different failure modes — a single pass catches one and misses the others.

**VOICE vs PLATFORM classification.** Without this, a profile built from Slack messages will produce Slack-style text in every format. The classification prevents platform conventions from being encoded as personal voice.

**Corpus is the source of truth.** Every rule must have a quoted example. If you can't find a quote that demonstrates the rule, the rule isn't there. Under-claiming beats over-claiming.

## Files

```
SKILL.md                             — Complete skill specification (modes, principles, templates)
references/extraction-checklist.md   — Corpus vetting rules and 8-dimension mechanical extraction grid
references/llm-isms.md               — 29-pattern catalog of LLM-tells with detection cues
references/cognitive-moves.md        — Cognitive-moves layer extraction (7 categories + 8 prompts)
references/vocabulary-fingerprint.md — Vocabulary-fingerprint layer extraction (12 categories + 12 prompts)
profiles/human.md                    — Built-in default profile (negative profile / humanizer)
profiles/                            — Where extracted profiles are stored
```

## The LLM-ism catalog

The `references/llm-isms.md` file catalogs 29 patterns commonly found in LLM-generated text, grouped by surface area:

- **Content patterns** — significance inflation, promotional language, vague attributions
- **Language patterns** — AI vocabulary ("moreover," "leverage," "robust"), copula avoidance, rule of three
- **Style patterns** — em-dash overuse, boldface overuse, signposting announcements
- **Communication patterns** — chatbot closers, sycophantic tone
- **Filler and hedging** — filler phrases, excessive hedging, generic conclusions

This catalog is used during extraction (to verify corpus authenticity), calibration (to identify LLM-ism tags), and generation (Pass 1 of the pre-delivery self-check).

## The cognitive-moves catalog

The `references/cognitive-moves.md` file captures the layer upstream of words — the moves the writer performs on an idea before assembling sentences. Seven categories with extraction prompts:

- **Framing moves** — what the writer does to a problem before arguing about it
- **Reasoning moves** — their reflex when a claim shows up
- **Concretization tendencies** — what they pair abstract claims with (example, number, scenario, or nothing)
- **Reflexive rejections** — what they refuse to accept (the most distinctive cognitive fingerprints are often negative)
- **Shape of conclusion** — how they end a piece
- **Audience assumptions** — what they leave unsaid because it's assumed
- **Argument shape** — the structural shape of how an argument is built

The discipline is the same as the mechanical layer: each rule requires two quoted instances. If you'd have to argue a move is present, it isn't a rule.

## The vocabulary-fingerprint catalog

The `references/vocabulary-fingerprint.md` file captures the lexical layer — the specific words the writer reaches for when alternatives exist. This is where classical stylometry's identifying signal lives (the Federalist Papers attribution method works on this layer). Twelve categories with twelve extraction prompts:

- **Top content lexicon** — words that travel across pieces (voice), not those tied to single pieces (topic)
- **Function-word patterns** — pronoun and connector ratios — classical stylometry's strongest signal
- **Verb preferences** — plain vs. elevated; the writer's top 20 verbs
- **Hedge vocabulary** — which specific hedges, not just hedge rate
- **Intensifier vocabulary** — specific intensifiers, not generic "uses intensifiers"
- **Synonym binaries** — the diagnostic table: when alternatives exist, the writer's pick (use/utilize, but/however, weird/strange, etc.)
- **Casualism / internet markers** — "lol", emoji, lowercase, letter-stretching, with density
- **Profanity** — frequency, words, contexts
- **Sentence-final vocabulary** — what shape sentences land on
- **Topic-shift vocabulary** — how paragraph transitions get signaled
- **Question vocabulary** — how questions are phrased
- **Banned-by-omission (lexical)** — words the writer demonstrably avoids

The single highest-leverage section is the synonym-binaries table. Applying those binaries during generation is the fastest way to make output sound like the writer.

## Credits

- Extraction methodology adapted from [Sam Dumont's voice-skill work](https://www.linkedin.com/in/samdumont/)
- LLM-ism catalog adapted from [blader/humanizer](https://github.com/blader/humanizer) (MIT) and [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)
- Vocabulary-fingerprint methodology draws on classical stylometry (function-word frequency, type-token ratio, lexical attribution)
- Audit framework (4-bucket drift report) from McFarland's voice plugin methodology
- Three-layer framing (mechanics + vocabulary + cognitive moves) — own synthesis, bridging Dumont's pure-mechanics approach and McFarland's pure-persona approach

## License

MIT
