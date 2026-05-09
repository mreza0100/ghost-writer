# Ghostwriter

A Claude Code skill that captures the mechanical fingerprint of how someone writes — vocabulary, sentence rhythm, punctuation density, formatting quirks — from a corpus of their writing, and generates new text that reproduces those patterns.

This is not persona imitation. It doesn't try to capture worldview, opinions, or vibe. It reads observable, measurable patterns and reproduces them at the documented densities. The result is auditable: every rule in a profile has a quoted example and a frequency figure from the source corpus.

## Why this exists

LLMs have a default voice. You've read it: balanced paragraphs, confident verbs, "moreover," neat tricolons, tidy transitions. When you ask an LLM to "write in someone's style," it usually produces its own default prose wearing a costume — a few surface-level quirks layered on top of the same underlying rhythm.

Ghostwriter takes a different approach. It builds a structured profile from a corpus of real writing, capturing density-based rules (not just presence/absence), classifying patterns as VOICE vs PLATFORM vs BORDERLINE, and maintaining a banned-words list of LLM-isms the writer demonstrably avoids. Generated text is then checked against a 29-pattern LLM-ism catalog before delivery.

## How it works

### Five modes

| Mode | Input | Output |
|------|-------|--------|
| **A — Extract** | Corpus of writing samples | Structured style profile |
| **A.5 — Calibrate** | Profile + user feedback on samples | Refined profile |
| **B — Generate** | Profile + writing prompt | New text in that style |
| **C — Audit** | Profile + recent writing | Drift report (4 buckets) |
| **D — Update** | Profile + audit/feedback | Revised profile + changelog |

Modes chain naturally: A then A.5 for first-time profiling. C then D for maintenance. A then B for one-shot "write like this" requests.

### The profile lifecycle

Profiles are living documents, not one-shot artifacts. They compound with iteration:

1. **Extract** from a corpus (10+ documents, 2+ formats recommended)
2. **Calibrate** with tagged feedback (WRONG, OVERSTATED, UNDERSTATED, MISSING, NEEDS_NUANCE, LLM_ISM, NOT_ME)
3. **Generate** using the profile
4. **Audit** against new writing every 6-8 weeks
5. **Update** to close drift gaps

### What a profile captures

- **Banned words and phrases** — patterns the writer demonstrably avoids, placed first because position affects generation
- **Anti-performative rules** — guards against exaggerating one-time patterns into theatrical signature moves
- **Quantitative layer** — sentence length, burstiness, punctuation rates per 1000 words, contraction rate, hedge-word rate
- **Vocabulary fingerprint** — pet phrases with frequency counts, register, domain jargon handling
- **Sentence structure and rhythm** — paragraph shape, clause-joining patterns, opener variety
- **Quirks** — idiosyncrasies with density figures
- **Negative rules** — patterns that are strikingly absent
- **Format-specific modes** — same voice, different registers (blog vs email vs Slack)

Every rule is tagged: VOICE (travels with the writer) / PLATFORM (convention of the medium) / BORDERLINE (ambiguous). Every rule has a quoted example and a confidence level.

### What a profile does NOT capture

- Opinions, beliefs, or persona
- Subject matter or topics
- Subjective tone descriptors ("warm," "snarky," "earnest")
- Generic good-writing advice
- Platform conventions confused as personal voice

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
```

Then use it in Claude Code:

```
/gwriter profile     — extract a style profile from a corpus
/gwriter calibrate   — calibrate a profile with feedback
/gwriter generate    — generate text in a profiled style
/gwriter audit       — audit a profile against recent writing
/gwriter update      — update a profile from audit findings
/gwriter list        — list available profiles
```

### As a standalone reference

The files work as a methodology guide even without Claude Code. `SKILL.md` contains the complete extraction and generation methodology. The `references/` directory has the LLM-ism catalog and extraction checklist.

## Key design decisions

**Density, not presence.** "Em-dashes ~7.6/1000w" is a rule. "Uses em-dashes" is not. Presence-only rules produce caricature — every quirk gets cranked to maximum.

**Banned words go first.** Position in the profile matters. Constraints encountered early have stronger influence on generation. The never-say list sits at the top.

**Two-pass self-review.** Pass 1 scans for LLM-isms (the 29-pattern catalog). Pass 2 scans for performative exaggeration. These are different failure modes — a single pass catches one and misses the other.

**VOICE vs PLATFORM classification.** Without this, a profile built from Slack messages will produce Slack-style text in every format. The classification prevents platform conventions from being encoded as personal voice.

**Corpus is the source of truth.** Every rule must have a quoted example. If you can't find a quote that demonstrates the rule, the rule isn't there. Under-claiming beats over-claiming.

## Files

```
SKILL.md                           — Complete skill specification (modes, principles, templates)
references/extraction-checklist.md — Corpus vetting rules and 8-dimension extraction grid
references/llm-isms.md             — 29-pattern catalog of LLM-tells with detection cues
profiles/                          — Where extracted profiles are stored
```

## The LLM-ism catalog

The `references/llm-isms.md` file catalogs 29 patterns commonly found in LLM-generated text, grouped by surface area:

- **Content patterns** — significance inflation, promotional language, vague attributions
- **Language patterns** — AI vocabulary ("moreover," "leverage," "robust"), copula avoidance, rule of three
- **Style patterns** — em-dash overuse, boldface overuse, signposting announcements
- **Communication patterns** — chatbot closers, sycophantic tone
- **Filler and hedging** — filler phrases, excessive hedging, generic conclusions

This catalog is used during extraction (to verify corpus authenticity), calibration (to identify LLM-ism tags), and generation (pre-delivery self-check).

## Credits

- Extraction methodology adapted from [Sam Dumont's voice-skill work](https://www.linkedin.com/in/samdumont/)
- LLM-ism catalog adapted from [blader/humanizer](https://github.com/blader/humanizer) (MIT) and [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)
- Audit framework (4-bucket drift report) from McFarland's voice plugin methodology

## License

MIT
