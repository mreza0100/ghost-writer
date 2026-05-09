---
name: gwriter
description: Use this skill whenever the user wants to capture how a specific person writes from a sample of their text and/or generate new text in that exact style, audit a profile against recent writing for drift, or update a profile to close gaps. Trigger phrases include "match my writing style", "write like this", "mimic this writer", "in the style of [person/text]", "make it sound like me", "extract the style from this", "write the rest of this in the same voice", "study how I write", "learn my writing", "style profile", "voice profile", "voice DNA", "audit my voice profile", "update my style profile", or any time the user pastes a long sample of someone's writing and asks for new text that should sound the same. Also trigger when the user wants a reusable style profile they can apply later. Do NOT use this skill for generic copyediting, grammar fixes, or simple tone shifts (formal/casual) — this skill is specifically for reproducing one specific person's mechanical writing fingerprint (vocabulary, sentence rhythm, punctuation quirks) grounded in evidence from a corpus.
---

# Ghostwriter

Capture the mechanical fingerprint of how someone writes — vocabulary, sentence rhythm, punctuation density, formatting quirks — from a corpus, generate new text that reproduces those patterns, and maintain the profile as the writer's habits drift.

The corpus is the **source of truth**. Every rule comes from the corpus, not from priors about what good writing looks like.

## Bundled references

Read these when the situation calls for them. Don't try to keep them all in working memory.

- `references/llm-isms.md` — 29-pattern catalog of LLM-tells. Used during Mode A (corpus sanity-check), Mode A.5 (canonical definition of the `LLM_ISM` tag), and Mode B (pre-delivery self-check).
- `references/extraction-checklist.md` — corpus preparation rules and the 8-dimension extraction grid. Read before starting Mode A.

## What this skill is, and isn't

This is a **mechanical-fingerprint** skill. It reads observable patterns — pet phrases, sentence shapes, punctuation rates — and reproduces them. It is _not_ a persona-direction skill: it doesn't try to capture worldview, opinions, or vibe. Persona-driven imitation often reads livelier but is harder to verify; mechanics are auditable but can read flat alone. If the user wants both, pair this skill with a separate persona/voice-direction prompt; flag the trade-off when it matters (e.g., the user says "this still doesn't _sound_ like me" after the mechanics are right). This split is real — McFarland's voice plugin argues against pure mechanics; Dumont's argues for them. Both have a point.

Voice also doesn't operate alone. In a content-creator setting, voice is one axis of a triple: voice (how it sounds) + audience (who it's for) + business context (what it's selling or building toward). This skill owns voice. If the user needs the other two, point them at separate ICP and business-profile work.

## Modes

Pick the mode based on what the user asked for. Modes can chain in one response.

- **Mode A — Extract.** Corpus → structured profile (saved as markdown).
- **Mode A.5 — Calibrate.** Profile → calibration samples → tagged feedback → revised profile.
- **Mode B — Generate.** Profile (or sample + prompt) → new text in that style + audit note.
- **Mode C — Audit.** Existing profile + N recent pieces → drift report (4 buckets: strong / thin / missing / fix).
- **Mode D — Update.** Existing profile + audit findings or feedback → revised profile + changelog.

If the user gives a sample and asks for new text in one go, do A then B. If they bring a profile that's been around for weeks and want it tuned to recent work, do C then D. **Profiles compound with iteration** — every audit-update cycle makes the profile sharper. Treat it as a living document, not a one-shot artifact.

## Core principles

### The corpus is the source of truth

Every rule must be grounded in evidence. Attach a short quoted example AND a frequency to each rule — not "uses em-dashes" but "em-dash ~3 per 1000 words; e.g., 'and then — without warning — it stopped'". If you can't find a quote that demonstrates the rule, the rule isn't really there. Drop it. **Under-claiming beats over-claiming.**

The alternative — a confident-sounding profile of generic "good writing" rules — is the most common failure mode. It will make generated text sound like default-Claude prose with a costume on, not the actual person.

### Density, not presence

For every recurring quirk, capture **the rate**. "Em-dashes ~3/1000w", not "uses em-dashes". "Sentence fragments ~1 in 12 sentences", not "uses fragments". When generating, match the _rate_ the writer actually uses — caricature is the failure mode of presence rules.

### VOICE vs PLATFORM vs BORDERLINE

Before recording any rule, classify it. Mark the bucket in the profile.

- **VOICE** — a personal pattern that travels with the writer across formats. ("Starts paragraphs with 'so'.", "ends emails with 'cheers,'.")
- **PLATFORM** — a convention of the medium the corpus came from, not the writer. (Slack: short single-sentence messages; LinkedIn: line breaks every sentence; academic: passive voice and hedging.)
- **BORDERLINE** — could be either; flag and ask the user, or note as low-confidence.

If you encode platform conventions as personal voice, the imitation will read fine in the original medium and feel wrong everywhere else. With a single-medium corpus, default ambiguous patterns to PLATFORM unless the pattern is unusual enough that it's clearly the writer's own.

### What NOT to capture

- **Opinions, beliefs, or persona.** Style ≠ identity.
- **Subject matter / topics.** A corpus about cooking → "writes about food" is not a style rule.
- **Voice / tone descriptors** ("warm", "snarky", "earnest"). Subjective, hard to verify, push you toward imitating a vibe instead of mechanics.
- **Generic good-writing virtues** ("uses active voice", "varies sentence length"). Only include if the corpus demonstrates them as a _distinctive_ pattern with numbers.
- **Platform conventions** confused as personal voice.
- **LLM-isms in the corpus.** If the corpus has clusters of patterns from `references/llm-isms.md`, verify with the user before encoding any of them.

---

## Mode A: Extract a style profile

### Step 1: Vet the corpus

Read `references/extraction-checklist.md` for the full corpus rules. The short version:

- **10+ documents, 2+ formats** is the floor. Single-format corpora encode the format's conventions as voice.
- **No AI-generated content.** AI in the corpus poisons extraction.
- **Recency:** prefer the last 2 years.
- **Length variety:** include short and long pieces.
- **Single author:** all samples written by the person being profiled.

If the corpus fails any rule, say so explicitly in the profile's _Confidence notes_. A tentative profile from a thin corpus is honest; a confident one is a lie.

### Step 2: Read along the 8 dimensions

Run through the dimensions in `references/extraction-checklist.md`: sentence patterns, opening patterns, vocabulary fingerprint, structural patterns, tone markers, formatting habits, language-specific patterns, LLM-ism scan. Treat each independently — patterns in one don't predict patterns in another.

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

## 3. Quantitative layer

- Avg sentence length: <N> words; **burstiness (σ)**: <N>
- Avg paragraph length: <N> sentences
- Type-token ratio (first 500 words): <N>
- Em-dashes / 1000w: <N>; semicolons / 1000w: <N>; colons / 1000w: <N>; ellipses / 1000w: <N>
- Contraction rate: <%>
- Hedge-word rate / 1000w: <N>
- Exclamation rate / 1000w: <N>
- Top sentence-initial connectors: "<x>" (Nx), "<y>" (Nx), "<z>" (Nx)

## 4. Vocabulary & word choice

- [VOICE | high] <Rule, with rate>. Example: "<short quote>"
- [VOICE | medium] <Rule>. Example: "<short quote>"
- [PLATFORM] <Medium-driven rule>. Example: "<short quote>"
- Pet phrases recurring verbatim: "<phrase>" (Nx), "<phrase>" (Nx)

## 5. Sentence structure & rhythm

- [VOICE | high] <Rule>. Example: "<short quote>"
- [BORDERLINE] <Rule, flagged>. Example: "<short quote>"
- Paragraph shape: <description>.

## 6. Quirks & idiosyncrasies

- [VOICE | high] <Rule, with rate>. Example: "<short quote>"
- [VOICE | tentative] <Rule>. Example: "<short quote>"

## 7. Negative rules (patterns demonstrably absent)

- <Pattern absent — only if absence is striking. e.g., "0 semicolons in 1200 words.">
- <e.g., "Never uses 'utilize'; uses 'use'.">

## 8. Default mode

The writer's default register when context isn't specified. Pick one:

- **Informational** — direct, factual, no narrative arc. Shorter, punchier. Set this as default if Slack/email/chat dominates the corpus.
- **Narrative** — story arcs, scene-setting, build-up. Longer paragraphs. Set this as default if blog posts / long-form dominates.
- **Mixed** — switches by format. Note which format gets which.

Document the default explicitly. Without this, generation applies one register uniformly and gets it wrong half the time.

## 9. Format-specific modes

Same voice, different registers. Without this, every output reads like the writer's most-represented format.

- **Blog / long-form:** <register notes, e.g., "narrative, 3–5 sentence paragraphs, occasional headers">
- **Email:** <e.g., "no greeting on internal team threads; brief greeting + one-line context on external">
- **Slack / chat:** <e.g., "lowercase sentence starts, no closing, often single-sentence messages">
- **Social / public posts:** <e.g., "first sentence carries the hook; no hashtags">

## 10. Voice in action (prompt-ready examples)

Three short examples that demonstrate the voice across the formats above. These compress all the rules into something the user can read and recognize.

- **Long-form opening (~50 words):** <generated example>
- **Email (~3 sentences):** <generated example>
- **Slack message (~1–2 sentences):** <generated example>

## 11. Confidence notes

- <Rules tentative because evidence was thin.>
- <Aspects that couldn't be read confidently from this corpus's coverage.>
- <Open questions for the user.>

## Generation instructions

These instructions are embedded in the profile so that generation (`/ghostwriter generate`) can read THIS FILE ONLY, without loading SKILL.md. Follow them exactly when generating text in this voice.

### How to generate

1. Read every section of this profile top-down before drafting. The order matters — banned words and anti-performative rules shape every following choice.
2. Check the priority hierarchy: does the writing task have conventions that override personal style?
   - **Hard contextual norms** (legal, academic, regulatory) override personal style. Apply voice only at margins.
   - **Audience norms** (exec summary for a CEO) → compress; keep voice cues; downscale density.
   - **Personal style** → default for blog posts, emails, social posts, casual writing.
   - **Platform conventions** → apply on top of style, only for the target platform.
3. Draft. Reproduce rules at documented densities. Match densities; don't crank.
4. Run the two-pass self-review before delivering.
5. Append the "Rules applied" note.

### Two-pass self-review

**Pass 1 — LLM-ism scan.** Skim the draft for: stacked em-dashes, "moreover / furthermore / actually", neat tricolons, balanced paragraph lengths, "It's not just X, it's Y" constructions, "navigate the complexities", "in today's fast-paced world", chatbot closers, "great question" sycophancy. For every match: is it in the corpus at this density? If yes, keep. If no, revise.

**Pass 2 — Performative scan.** Look for "signature moves" applied loudly. Check Section 2 (anti-performative rules): is this move high-density in the corpus, or did you crank a one-time tic into a catchphrase? If cranked, dial down. Smell test: would the writer's friend roll their eyes reading this?

### Failure modes to avoid

- **Topic drift bias.** Match the style, not the subject matter of the corpus.
- **Caricature.** Match densities, not just presence.
- **Default-Claude leak.** Well-balanced, varied prose with confident verbs and tidy transitions is your default voice, not theirs. Lean into the messier and weirder spots.
- **Persona invention.** Don't invent personality, opinions, or biographical hooks.
- **Length mismatch.** Infer length from the writing task, not the corpus.
- **Platform contamination.** Don't import another platform's conventions.

### "Rules applied" note

End every generated text with:

```
---
**Rules applied (top hits):**
- <Rule>: <how reproduced, with target density if relevant>
- <Rule>: <how reproduced>

**Self-review:**
- LLM-ism pass: <patterns found and removed, OR "none flagged">
- Performative pass: <quirks dialed back, OR "no cranking detected">

**Trade-offs:** <if priority hierarchy kicked in, mention briefly. Otherwise omit.>
```

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

## Mode B: Generate text in the style

1. If the user hands you a profile, treat it as authoritative. **Read every section, top-down.** The order matters — banned words and anti-performative rules go first because they shape every following generation choice.
2. If the user gives raw corpus + prompt without first asking for a profile, do a quick mental extract. Stay evidence-grounded. Optionally offer to save the profile.
3. Check the **priority hierarchy** (next section).
4. Draft. Reproduce rules at documented densities. Match densities; don't crank.
5. Run the **two-pass self-review** before delivering.
6. Append the **Rules applied** note.

### Priority hierarchy (when style and context conflict)

Personal style does not always win.

1. **Hard contextual norms** — legal briefs, academic abstracts, regulatory filings, medical chart notes, formal business letters. Form conventions override. Apply style at the margins (vocabulary, paragraph shape) where it doesn't violate the form.
2. **Audience norms** — when the audience won't tolerate the writer's quirks (rambling exec summary for a CEO who reads on a phone). Compress; keep voice cues; downscale density.
3. **Personal style** — default for blog posts, emails, social posts, casual writing.
4. **Platform conventions** — apply on top of style, only for the platform being written to. Don't import another platform's conventions.

When the trade-off kicks in, say so in the Rules applied note.

### Two-pass self-review (do this before delivering)

These are different failure modes. A single pass tends to catch one and miss the other.

**Pass 1 — LLM-ism scan.** Skim the draft against `references/llm-isms.md`. Cues to look for: stacked em-dashes, "moreover / furthermore / actually", neat tricolons, balanced paragraph lengths, "It's not just X, it's Y" constructions, "navigate the complexities", "in today's fast-paced world", chatbot closers, "great question" sycophancy. For every catalog match: is it in the corpus at this density? If yes, keep. If no, revise.

**Pass 2 — Performative scan.** Look at every place the draft has a "signature move" — a quirk applied loudly. Check the profile's anti-performative rules in Section 2: is this move actually high-density in the corpus, or did you crank a one-time tic into a catchphrase? If cranked, dial it down. The smell test: would the writer's friend roll their eyes reading this?

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

**Rules applied (top hits):**

- <Rule>: <how reproduced, with target density if relevant>
- <Rule>: <how reproduced>
- ...

**Self-review:**

- LLM-ism pass: <patterns found and removed, OR "none flagged">
- Performative pass: <quirks dialed back, OR "no cranking detected">

**Trade-offs:** <if priority hierarchy kicked in, mention briefly. Otherwise omit.>
```

Keep it short — audit trail, not essay.

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

| Metric              | Profile | Recent | Δ   |
| ------------------- | ------- | ------ | --- |
| Avg sentence length | ...     | ...    | ... |
| Burstiness (σ)      | ...     | ...    | ... |
| Em-dashes / 1000w   | ...     | ...    | ... |
| ...                 |         |        |     |

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
- **Mode B:** the new text + the "Rules applied" note (with two-pass self-review).
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
