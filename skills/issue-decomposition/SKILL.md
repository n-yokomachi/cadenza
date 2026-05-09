---
name: issue-decomposition
description: Decompose a confirmed issue into MECE sub-issues and build a storyline. Triggers on phrases like "help me design the article structure", "decide the table of contents", "build the outline", "記事の構成を考えたい", "目次を決めたい", "アウトラインを作りたい" — any conversation about structural design. Acts as a gate before writing begins.
---

# Issue Decomposition

Decompose the confirmed issue into the sub-issues needed to answer it, and assemble the logical flow that leads to the conclusion.

## Purpose

Skeletal design for raising solution quality. Complete the logical structure itself before any writing or slide work begins. If this phase is weak, no amount of downstream effort will produce a persuasive output.

## Prerequisites

The `## Phase 1: Issue Finding` section must exist in `./.cadenza/state.md`. If it doesn't, instruct the user to run `cadenza:issue-finding` first.

## Execution flow

### Step 1: Decompose the issue

Decompose the confirmed issue into a set of sub-issues (questions) needed to answer it. Each set must satisfy:

- **MECE**: Mutually exclusive, collectively exhaustive
- **No duplication**: No sub-issue restates another in different words
- **Granularity alignment**: Sub-issues sit at comparable levels of abstraction
- **3–5 sub-issues**: Re-aggregate under higher concepts if more

Typical decomposition patterns for technical output:

| Pattern | Example structure | Best for |
|---------|------------------|----------|
| Why-What-How | Why needed → What it is → How to use | Introducing new tech |
| Before-After | Problem → Solution → Effect | Improvement / adoption case |
| Compare-Select | Options → Criteria → Decision | Tech selection |
| Failure-Lesson | Expectation → Reality → Lesson | Sharing pitfalls |
| Steps-Implementation | Overview → Components → Behavior → Pitfalls | Implementation deep-dive |

Propose the most fitting pattern for the user's issue and get agreement.

### Step 2: Hypothesis for each sub-issue

For each sub-issue, have the user articulate their **current hypothetical answer** as a single sentence. Sub-issues without a hypothesis must be explicitly marked as unresolved territory to be filled in during the analysis phase.

When the hypotheses are filled in:

- The conclusion becomes visible upfront (storyline can be built)
- It becomes clear where primary-source verification is needed
- The overall message of the output is fixed in advance

### Step 3: Assemble the storyline

Reorder sub-issues and hypotheses into a sequence that lands with the reader. Two main patterns for technical output:

#### A. Sky-Rain-Umbrella (situation → interpretation → action)

- Sky: Observed facts, current situation
- Rain: Their meaning / interpretation / why
- Umbrella: Action / recommendation / conclusion

Best for technical blogs and explanatory articles.

#### B. Parallel reinforcement (multiple grounds supporting one conclusion)

- Conclusion
- Ground 1
- Ground 2
- Ground 3
- Restate conclusion

Best for LTs and conference talks (time-constrained, designed to be remembered).

Pick the pattern that best matches your storyline's purpose. **Sky-Rain-Umbrella** is the default for explanatory writing; **Parallel reinforcement** for time-constrained or memorability-first delivery. The choice doesn't have to be exclusive — adopt the dominant frame and use the other locally as needed.

### Step 4: Fix the main message

Fix the **single message** that runs through the entire storyline. This is:

- The one sentence the reader should remember after reading
- The text on the final slide for a deck
- The title or conclusion text for a blog post

Inability to fix the main message is a signal that the storyline is weak. If it can't be fixed, return to Steps 1–3.

### Step 5: Storyline validity check

Self-review the assembled storyline:

- Does every sub-issue contribute to the main message?
- Are there logical jumps between sub-issues (hidden assumptions)?
- Does the sequence work for the target reader's prerequisite knowledge?
- For each sub-issue, can you answer "So what?"
- If a reader who heard the conclusion asked "Why is that?", can the sub-issues answer?

If any check fails, repeat Steps 1–3.

## Output format

Append the confirmed content to `./.cadenza/state.md` (update the block if it exists):

```markdown
## Phase 2: Issue Decomposition (✅ Done YYYY-MM-DD)

### Main message
[The conclusion as a single sentence]

### Adopted pattern
[Why-What-How / Before-After / Compare-Select / Failure-Lesson / Steps-Implementation / Sky-Rain-Umbrella / Parallel reinforcement / etc.]

### Sub-issues and hypotheses
1. [Sub-issue 1] → [Hypothesis 1]
2. [Sub-issue 2] → [Hypothesis 2]
3. [Sub-issue 3] → [Hypothesis 3]

### Presentation order (storyline)
[Sub-issues reordered for delivery]

### Unverified territory
[Hypothetical gaps to be filled in the analysis phase]
```

## Anti-patterns (always avoid)

- **Storyline that is just chronological**: Listing "what was done" in time order is a record, not a storyline. Design around the reader's flow of understanding.
- **Too many sub-issues**: Six or more imposes too much cognitive load on the reader. Consolidate.
- **Advancing without a fixed main message**: Always complete Step 4.
- **"Just figure it out while writing"**: Writing without a storyline incurs heavy structural rework cost later.

## Upstream-return signals

Return to `cadenza:issue-finding` if any of the following apply:

- No matter how you decompose, you can't get to MECE / sub-issues overlap → the issue itself is vaguely articulated.
- The main message comes out weak or generic → the issue lacks a "deep hypothesis".
- Lining up the sub-issues makes the conclusion feel "obvious" → the issue quality is low.

## Transition to next phase

Once `./.cadenza/state.md` is written, ask the user:

> ✅ Phase 2 (Issue Decomposition) is complete. Proceed to `storyboarding` (visualization design)?

- **Proceed**: Invoke `cadenza:storyboarding` via the Skill tool.
- **Hold**: Wait for revision instructions.
- **Return upstream**: If a return signal applies, invoke `cadenza:issue-finding`.
