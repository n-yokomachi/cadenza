---
name: output-review
description: Support the author's review of a generated output artifact (blog post, deck, LT). The author edits the artifact directly; this skill answers questions, traces verification grounds, and never reverts author edits as "regressions". Triggers on phrases like "let me review", "I have a question", "記事をレビューする", "今からレビューします", "確認したいことがある". The cycle continues until the author explicitly declares review complete.
---

# Output Review

Support the author's review cycle on a generated artifact (typically the result of `output-crafting`).

## Purpose

`output-crafting` produces a draft (e.g. `./.cadenza/output.md`). The author then reviews and edits it directly. This skill governs the review cycle:

- The author edits the artifact directly. **Do not treat author edits as regressions and revert them.**
- The author may ask questions stemming from gaps in their knowledge or recall of the verification phase. Answer with explicit grounds, not by guessing.
- The cycle continues until the author explicitly declares the review complete.

This is the **final gate before publishing**. Quality of grounding here protects the author from publishing claims they can't defend later.

## Prerequisites

`./.cadenza/state.md` must contain Phase 5 (Output Crafting) — i.e. the artifact under review must already exist.

## Operating principles

### 1. Author edits are authoritative

When the author has modified the artifact since the last AI write:

- Read the latest artifact before any new action.
- Treat author edits as encoding information you don't have (intent, taste, domain knowledge, factual correction).
- Never restore a prior version unless the author asks for it.
- If a later AI edit must touch a region the author edited, preserve their wording verbatim outside the requested change.

### 2. Answer with grounds

For every author question, ground the answer in a checkable source:

- `./.cadenza/state.md` (Phase 1〜4 records, Side observations, verification matrices)
- Generated artifacts (recommendation result JSONs, code, scripts, log files)
- Public documentation (cite URLs)

If the answer cannot be grounded, say so explicitly — "the state file does not record this", "no verification covers this case" — rather than fabricating. Then offer to investigate.

### 3. Question-answer cycle

The author may pose a sequence of questions over multiple turns. Treat the review as an ongoing dialog:

- Do not summarise progress unless asked.
- Do not propose moving to the next phase.
- Do not declare the review complete on your own.

When uncertain whether the author intends to close, ask explicitly: 「レビュー完了でよろしいですか?」 / "Are we done with the review?".

Recognised explicit closing signals: 「レビュー完了」「OK 公開する」「了解、これで進める」「review complete」「ship it」「公開してよし」, etc.

### 4. Edit only when asked

Do not edit the artifact unless the author requests it. When asked to edit:

- Confirm scope (which section / what wording / what change).
- Make the minimum change needed.
- Preserve all other author edits verbatim.
- Do not bundle in unrequested "improvements".

### 5. Distinguish question types

Answers should match the question type:

| Question type | Response shape |
|---|---|
| **Factual ("X はどういう仕組みなの?")** | Grounded explanation citing state file or docs |
| **Reasoning recall ("なぜここで Y という結論にしたんだっけ?")** | Trace back to the Phase 4 verification or Phase 2 hypothesis that led to it |
| **Edit request ("ここを書き換えて")** | Confirm scope → minimal edit → diff summary |
| **Opinion ("これでいい?")** | Honest opinion with reasoning, not pleasantry. If unsure say so |

## Companion skill: `output-proofread`

`output-review` is **reactive** (author leads, AI answers). For a **proactive** AI-driven exhaustive audit (technical accuracy fact-check + language-specific proofreading) that produces a finding report instead of waiting for the author to spot issues, invoke `cadenza:output-proofread`. The two are complementary and often used together: run `output-proofread` first to surface a finding list, then use `output-review` for follow-up Q&A and edit dialogs.

## Anti-patterns (always avoid)

- **Reverting author edits as "fixes"**: Author edits are authoritative. Never silently revert.
- **Padding answers with unrequested suggestions**: When asked a factual question, answer that question. Suggestions only on explicit request.
- **Guessing when grounds are missing**: If the state file or artifacts don't cover the question, say so. Then offer to investigate. Don't fabricate.
- **Closing the cycle prematurely**: Do not declare the review done. Wait for an explicit signal from the author.
- **Bundling improvements into edit requests**: When the author asks for change X, make change X only. Don't also fix typo Y you noticed unless asked.
- **Treating gentle questions as criticism**: The author's questions about wording or structure are exploratory, not always change requests. Answer first; only edit when explicitly asked.

## Transition

When the author explicitly declares the review complete, append to `./.cadenza/state.md`:

```markdown
## Phase 6: Output Review (✅ Done YYYY-MM-DD)

### Artifact reviewed
- `./.cadenza/output.md` (or other artifact)

### Notable changes during review
- [list of substantive changes the author made or requested, or "minor edits only"]

### Open questions remaining
- [list, or "none"]

### Final publish decision
- [ready to publish / hold for further work + reason]
```

The `cadenza` workflow is then truly complete.
