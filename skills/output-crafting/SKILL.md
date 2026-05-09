---
name: output-crafting
description: Produce the final Markdown artifact from the verified storyline and storyboard. Triggers on phrases like "write the article", "produce the final output", "output を生成して", "記事を書いて", "最終アウトプットを仕上げて". Acts as the workflow's finishing gate.
---

# Output Crafting

Produce the final Markdown artifact from the confirmed storyline and verification results.

## Purpose

The workflow's finishing phase. **Be message-driven and shape the output so the reader grasps the essence in the shortest path.** The standard is "what gets through", not "what I want to say".

This skill produces a single, format-agnostic Markdown file at `./.cadenza/output.md`. Platform-specific finishing (Zenn frontmatter, SpeakerDeck / Marp directives, LT-specific compression, etc.) is the responsibility of downstream publishing tooling — it deliberately does not happen here.

## Prerequisites

`./.cadenza/state.md` must contain Phase 1 through Phase 4 sections. If any are missing, instruct the user to invoke the corresponding skill first.

## Finishing principles

### 1. Message-driven

- Every element (paragraph, code block, diagram) ties to the main message or a sub-issue.
- Have the courage to cut what doesn't tie.
- "Would be nice to have" / "It was interesting" are not adopted.

### 2. Visible structure

- Make the structure explicit so the reader knows where they are.
- Heading hierarchy should mirror the storyline.
- Section-opening summaries (1–2 sentences at the top of each section) help skim readers.

### 3. Polished title and opening

- The title must convey the issue and the value gained.
- The opening (first 3–5 lines) must make "for whom / about what / what changes" clear.
- Design so the reader doesn't bounce in the first few seconds.

### 4. Storyboard fidelity

Every visual specified in Phase 3 (Storyboarding) appears exactly where the storyboard placed it. Don't add visuals not in the storyboard. Don't drop visuals that were planned.

## Execution flow

### Step 1: Assemble the structure

Default skeleton for the Markdown output:

```
- Title
- TL;DR (1-2 sentences capturing the main message)
- Background / preconditions
- Body (one section per sub-issue, in storyline order)
- Conclusion (restate main message + next step / takeaway)
- References (optional)
```

Adjust per the dominant output style recorded in Phase 3:

- **Long-form prose**: full skeleton above
- **Slide presentation**: each section becomes a slide-equivalent block (title + bullets + visual reference)
- **Short-form**: minimal skeleton — Title, single body, main message, optional reference

### Step 2: Title

Generate three title candidates using one of these patterns and have the user pick:

- **Question form**: "How should we design X when Y?"
- **Discovery form**: "I tried X and discovered Y"
- **Steps form**: "N steps to achieve X with Y"
- **Comparison form**: "X vs Y: which to choose?"

Avoid clickbait ("Shocking", "The truth about X").

### Step 3: TL;DR / opening

In the first 3–5 lines, convey:

- The main message (lead with the conclusion)
- The target reader
- What the reader gains by finishing

Readers decide whether to continue in the opening. Articles that don't convey value upfront don't get read.

### Step 4: Write each section

Write each sub-issue as one section. Each section:

- Heading expresses the sub-issue (interrogative form is fine)
- 1–2 sentences at the top summarize the section
- Body draws on the verification material from Phase 4
- Mini-summary at the bottom if the section is long

### Step 5: Final check on code and diagrams

- Does the code run as-is on copy-paste?
- Are the diagrams sufficient and necessary in the article's flow?
- Any leaked confidential info (API keys, internal URLs, personal info)?
- Are images / external assets reachable (or noted as TODO for the user to upload separately)?

## Output format

Write the final artifact to `./.cadenza/output.md`. Use a standard technical-writing tone in the artifact (the conversational persona used in chat does not carry into the final artifact).

After generating, append to the end of `./.cadenza/state.md`:

```markdown
## Phase 5: Output Crafting (✅ Done YYYY-MM-DD)

### Generated output
- `./.cadenza/output.md`

### Pre-publish final check
- [ ] Main message in issue summary matches main message in final output
- [ ] Storyline is preserved in the final output
- [ ] Verification grounds are traceable for the reader
- [ ] No confidential info leaked
```

## Anti-patterns (always avoid)

- **Including all verification results**: Don't pack in material beyond what the storyboard scope decided.
- **Deciding the title last as an afterthought**: The title is the entry point. Spend deliberate time on Step 2.
- **Composition that dilutes the main message**: Topics tend to multiply during finishing. Always reconfirm the tie to the main message.
- **Premature platform finishing**: Do not embed Zenn-specific syntax (`:::message`, `:::details`), Marp directives (`---`, `<!-- _class: lead -->`), or platform-specific frontmatter in the cadenza output. Those belong to downstream publishing.

## Upstream-return signals

Return upstream if any of the following apply:

- Material is insufficient at finishing time → `cadenza:analysis-execution` for additional verification.
- The link between main message and sub-issues feels weak → `cadenza:issue-decomposition` to redesign.
- After finishing, "what am I actually saying?" is blurry → `cadenza:issue-finding` to redefine the issue.

## Workflow complete

When this phase is done, the cadenza workflow that started at `cadenza:issue-finding` is complete. Before publishing, always run the pre-publish final check above. Platform-specific publishing happens outside cadenza.
