---
name: output-proofread
description: AI-driven exhaustive quality audit on a generated artifact (blog / deck / LT). Performs (1) technical accuracy fact-check against the state file, source code, generated artifacts, and public documentation, and (2) language-specific proofreading for typos, unnatural expressions, and style consistency — in Japanese for a Japanese article, English for an English one. Produces a structured finding report; the author makes the actual edits. Triggers on phrases like "校正して", "ファクトチェック", "徹底的にレビュー", "誤字脱字チェック", "技術的に正しいか確認", "proofread the article", "fact-check", "accuracy audit", "language polish".
---

# Output Proofread

AI-driven exhaustive quality audit on the artifact produced by `output-crafting`.

## Purpose

`output-review` is **reactive**: the author leads, the AI answers questions and traces grounds. This skill is **proactive**: the AI performs an exhaustive audit and produces a finding list along two complementary axes:

1. **Technical accuracy** — every factual / technical claim is cross-checked against verifiable grounds (state file, code, generated artifacts, public docs).
2. **Language quality** — typos, unnatural expressions, style inconsistencies, checked in the article's own language (Japanese rules for a Japanese article, English rules for an English one).

The deliverable is a finding report. **This skill never edits the artifact silently.** The author decides which findings to act on and either edits directly or asks for specific edits.

## Prerequisites

- `./.cadenza/state.md` must contain Phase 5 (Output Crafting) — i.e. the artifact under audit must already exist.
- Determine the artifact path:
  - Default: the path recorded in Phase 5's "Generated outputs" section (typically `./.cadenza/output.md`, or an author-relocated path such as `~/work/workshop/zenn/articles/<file>.md`).
  - If the author specifies a different path, use that.
- If a voice/style profile exists at `./.cadenza/voice-style-notes.md`, fold "voice profile compliance" into the audit.

## Execution flow

### Step 1: Detect article language

Read the artifact and classify its primary language:

- **Japanese** if Hiragana / Katakana / CJK characters dominate the body.
- **English** otherwise.
- **Mixed** is rare in this workflow; treat as the dominant language but flag any inconsistent switching as a finding.

The proofreading rules differ per language (Step 3).

### Step 2: Technical accuracy audit

For every factual / technical claim, ask "where is the ground for this?". Treat the following as claims requiring grounds:

- Numbers, percentages, latencies, costs, sample sizes.
- Code snippets (existence, accuracy, runnability).
- Product / service / API names, version numbers, region names.
- Architectural claims ("X uses Y under the hood", "Z is implemented as W").
- Quoted official documentation or behavior descriptions.
- Terminology that may be AI-coined or paraphrased (verify it matches the official name).

Cross-checking sources, in order of authority:

1. `./.cadenza/state.md` — Phase 1〜4 records, Side observations, verification matrices.
2. Generated artifacts in the working repo — code files, scripts, log outputs, JSON results.
3. Public documentation — fetch with WebFetch when grounded confirmation requires it. Cite the URL.

Classify each claim:

| Label | Meaning |
|---|---|
| `[WRONG]` | Claim contradicts a checkable source. |
| `[UNVERIFIED]` | Claim has no traceable ground in available sources. (Not necessarily wrong — just unsupported.) |
| `[OK]` | Claim aligns with sources. (Do not list these in the report; they are the silent majority.) |

Be honest about the difference between `[WRONG]` and `[UNVERIFIED]`. If you cannot ground the *correction*, the finding is `[UNVERIFIED]`, not `[WRONG]`.

### Step 3: Language-specific proofreading

Apply only the rule set matching the article's language.

#### If Japanese

Look for:

- **誤字脱字 / 変換ミス** — missing or wrong characters, kanji conversion errors.
- **表記ゆれ** — inconsistent spelling within the same article (例: 「プロンプト」と「prompt」が混在、「バリアント」と「variant」が混在、「サブエージェント」と「sub-agent」が混在).
- **不自然な日本語** — AI-translation flavor, redundant 「することができる」「することによって」, 不要な「〜という」, 二重否定の濫用.
- **文末の不整合** — 「です・ます」と「だ・である」の混在 (voice profile があれば profile に従う).
- **助詞の誤用** — が / は / を / に / で の取り違え.
- **半角・全角の混在** — 英数字の前後スペース有無 (voice profile が「スペースを入れない」を指定している場合は厳守).
- **句読点** — 「、」「。」の置き位置、連続使用、英文記号 (,) (.) との混在.
- **カタカナ語の濫用** — 自然な日本語表現があるのにカタカナを優先しているケース.
- **AI 生成の癖** — 不用意な太字、「〜と言えるでしょう」「〜することが重要です」「まず最初に」のような箇条書き常套句、不要な「ですね」「と思います」.

#### If English

Look for:

- **Typos and spelling errors.**
- **Subject-verb agreement.**
- **Article misuse** (a / an / the).
- **Inconsistent terminology** within the same article (e.g. "AgentCore" vs "Agent Core", "config bundle" vs "configuration bundle").
- **Tense consistency** within paragraphs / sections.
- **Awkward or wordy phrasing**, including unnecessary nominalization.
- **Punctuation consistency** (Oxford comma policy, hyphenation, em-dash style — match voice profile if one exists).
- **Heading capitalization style** (sentence case vs title case — match voice profile).
- **AI-generated tells**: "It is worth noting that", "In conclusion", over-hedging chains ("might possibly perhaps"), excessive bolding, em-dash overuse, "delve / leverage / unlock" cliches.

#### Voice / style profile compliance (when a profile exists)

For each substantive paragraph, spot-check against the profile dimensions:

- Sentence ending pattern
- Hedging / certainty level
- Emphasis density (bold, code spans)
- Code block convention (language tag, filename, preface, after-code reaction line)
- Frontmatter and platform-specific syntax (Zenn `:::message` / Qiita `:::note`, etc.)

Flag any deviations as `[VOICE]` findings.

### Step 4: Produce the finding report

Write a single Markdown report. Default path: `./.cadenza/proofread-findings-YYYYMMDD.md` (use date suffix when running multiple audits on the same day, append `-N`).

Use this format:

```markdown
# Proofread findings: <artifact path>

- Audit date: YYYY-MM-DD
- Article language: <ja | en>
- Voice profile in use: <yes (path) | no>
- Sources consulted: <state.md sections | repo files | fetched URLs>

## Summary

- Technical: N total ([WRONG]: a / [UNVERIFIED]: b)
- Language: M
- Voice profile: K

## Technical accuracy

### [WRONG] L<line> — <one-line summary>

- Claim: "<verbatim quote from artifact>"
- Ground checked: <state.md section | repo file:line | doc URL>
- Discrepancy: <what differs from the ground>
- Suggested fix: <minimal correction; author decides>

### [UNVERIFIED] L<line> — <summary>

- Claim: "<quote>"
- Searched: <where you looked and why you couldn't ground it>
- Suggested action: <ask author to verify | remove | soften with hedging>

## Language quality

### L<line> — <category, e.g. 表記ゆれ / typo / awkward phrasing>

- Quote: "<the problematic span>"
- Issue: <what's wrong>
- Suggested fix: "<corrected text>"

## Voice profile

### [VOICE] L<line> — <dimension> mismatch

- Quote: "<span>"
- Profile rule: <relevant line from voice-style-notes.md>
- Suggested fix: "<rephrasing matching the profile>"
```

Ordering inside the report:

1. `[WRONG]` findings (highest priority)
2. `[UNVERIFIED]` findings
3. Language quality findings (by line number)
4. `[VOICE]` findings (by line number)

Always include line numbers so the author can jump to each location quickly.

### Step 5: Hand off, do not edit

Tell the author the report path. Do not touch the artifact.

If the author then asks "fix L42" or "apply suggestions for L42 and L78", perform those minimal edits — consistent with `output-review`'s edit principles:

- Confirm scope (which findings, which sections).
- Make the minimum change needed.
- Preserve all other author edits verbatim.
- Do not bundle in unrequested "improvements".

## Anti-patterns (always avoid)

- **Silently editing the artifact**: this skill outputs a report. Never edit unless explicitly asked.
- **Confidently flagging style preferences as errors**: if it isn't actually wrong, inconsistent, or unverifiable, it isn't a finding. "Could be more concise" is not a finding unless the voice profile demands it.
- **Confident corrections without grounding**: if the proposed correction itself can't be grounded, mark the finding as `[UNVERIFIED]`, not `[WRONG]`.
- **Cross-language rule application**: do not apply English style rules to Japanese text or vice versa.
- **Re-flagging settled author choices**: if the author already accepted a phrasing during conversation, don't list it again.
- **Padding the report with `[OK]` items**: only list things needing attention. The silent majority should stay silent.
- **Single-pass shallowness**: actually open and read the cited grounds. Don't claim "this is verified by state.md" without quoting the relevant line.

## Relationship to other skills

| Skill | Role | Direction |
|---|---|---|
| `output-crafting` | Produce the artifact | AI writes |
| `output-proofread` (this) | Audit the artifact, produce findings | AI audits, author edits |
| `output-review` | Q&A support during author's read-through | Author leads, AI answers |

Typical sequence:

1. `output-crafting` → draft.
2. `output-proofread` → finding report.
3. Author edits in response to findings.
4. `output-review` cycle for any remaining questions / discussions.
5. Author declares review complete → `output-review` writes Phase 6 to `state.md`.

This skill does **not** write to `state.md`. The audit is an artifact, not a phase.

## Upstream-return signals

If the audit reveals issues too deep to fix by editing alone:

- **Numerous `[WRONG]` findings tracing back to incorrect verification** → return to `cadenza:analysis-execution` for additional / corrected verification.
- **`[WRONG]` findings revealing the storyline doesn't match the verified facts** → return to `cadenza:issue-decomposition` to redesign the storyline.
- **Pattern of `[UNVERIFIED]` claims that originate from a hand-wavy main message** → return to `cadenza:issue-finding` to redefine the issue.

In these cases, surface the upstream-return recommendation in the finding report's Summary section.
