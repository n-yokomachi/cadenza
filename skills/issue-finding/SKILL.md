---
name: issue-finding
description: Use during the planning phase of a technical output (blog post, conference talk, LT slides) to narrow down what to write about. Triggers on phrases like "I want to write a blog", "looking for an LT topic", "ブログを書きたい", "登壇ネタを考えたい", "Qiita/Zenn/SpeakerDeck 記事を作りたい" — any conversation about topic selection or planning. Acts as a gate before writing begins.
---

# Issue Finding

The most upstream phase of technical output creation. Identify "what question is worth answering" and avoid the dog's path (sinking effort into low-issue-quality topics).

## Purpose

Maximize **issue quality** in the formula `Value = Issue Quality × Solution Quality`. Before any downstream work (writing, analysis, slide design), guarantee that the topic itself is worth pursuing.

## Critical protocol — do not bypass

Steps 1, 2, and 5 require **explicit user articulation**. Specifically:

- **Step 1** (primary-source basis): ask the user about their primary-source material; do not assume from prior conversation context.
- **Step 2** (target reader / reader's problem hypothesis / post-read change): stop and ask the three questions; wait for the user's actual answers; **do not fabricate, infer from prior conversation, or fill in defaults silently**.
- **Step 5** (final issue 1-sentence): present the candidate sentence and require explicit agreement (verbal "yes" / "OK" / "進めて" or similar) before recording — implicit agreement does not count.

**Auto Mode and continuous-execution flags do not override these stops.** The whole downstream workflow rests on these three pieces of user-supplied truth; substituting your own guesses silently corrupts every phase that follows.

Steps 3 (existing-content survey) and 4 (three-conditions check) can be AI-driven and don't require live user input.

## State file and artifact location

All workflow artifacts (state file, voice/style notes, output files) live under `./.cadenza/` in the working directory. This skill, being the most upstream phase, creates that directory if it does not exist and writes the state file as `./.cadenza/state.md`. If the file already exists, ask the user whether to overwrite or append.

## Execution flow

When the user comes with a planning request, proceed in order. Don't advance until each step is settled.

### Step 1: Verify primary-source basis

Ask the user:

- Do they have **primary-source material** for this topic? (They implemented it themselves, hit the bug themselves, measured it themselves, deployed it internally, etc.)
- Is this just a rehash of secondary sources (summarizing other articles, translating official docs)?
- Can user-specific experience or context be added?

If there is no primary-source basis, point it out clearly and propose either pivoting to a different topic or first acquiring the primary-source material (build/measure).

### Step 2: Articulate target reader and reader hypothesis

**Stop and explicitly ask the user the following three questions.** Wait for the user's actual answers — do not fabricate, infer from prior conversation, or fill in defaults, even under Auto Mode (see "Critical protocol" above).

- **Target reader**: What tech stack, what role, what situation? (1–2 sentences)
- **Reader's problem hypothesis**: What concrete pain point or open question is this reader currently facing? (1–2 sentences)
- **Post-read change**: After reading, what can the reader do or decide that they couldn't before? (1–2 sentences)

If the user delegates ("you decide" / "適当に") for one of these, still propose a concrete draft and require explicit confirmation before recording. Do not record any of the three based purely on your own inference. If these three are vague, the result will be a generic article that resonates with no one.

### Step 3: Existing-content survey

For the candidate topic, check the following. Use WebSearch as needed:

- Do high-quality articles or talks already exist on this topic?
- If yes, what's your differentiator? (Angle, depth, target audience, newer version, novel combination, failure case, etc.)
- Is the content something where reading the official docs would suffice?

If no clear differentiator emerges, propose narrowing the topic or shifting the angle.

### Step 4: "Three conditions of a good issue" check

The core judgment criteria of this workflow. Verify all three are met:

1. **It's an essential choice**: Answering it changes subsequent action, provides a decision criterion, resolves a binary, etc.
2. **It contains a deep hypothesis**: Structural insight like "overturns common belief", "explains via shared pattern", "explains via opposing relationship".
3. **It can be answered**: You can reach a conclusion using information, implementations, and measurements available to you.

**If even one condition fails, stop here.** Do not let the user advance to the next phase. Push them to redefine the topic.

### Step 5: Articulate the issue

Once all three conditions are met, fix the issue as a single sentence in this form:

> "How should [target reader] think about [judgment/question] in [situation]?"

**Present this sentence explicitly in the conversation and wait for the user's explicit verbal agreement before recording.** Implicit / silent agreement is not enough — explicitly ask for confirmation (e.g. "この issue で進めてよろしいですか？" / "Proceed with this issue as written?") and wait for "yes" / "OK" / "進めて" or similar. If the user requests changes, refine and re-confirm. Subsequent phases use this sentence as the basis, so an unchecked recording silently corrupts the rest of the workflow.

## Output format

Write the confirmed content to `./.cadenza/state.md` using this structure (update only this phase's block if the section already exists):

```markdown
## Phase 1: Issue Finding (✅ Done YYYY-MM-DD)

### Confirmed issue
[The issue, as a single sentence]

### Target reader
[Reader profile]

### Reader's problem hypothesis
[Concrete pain point the reader has]

### Post-read change
[What changes for the reader after reading]

### Primary-source basis
[User-specific experience / implementation / measurement]

### Differentiator
[How this differs from existing content]
```

The state file content itself may be written in the user's working language (Japanese is fine if the user writes in Japanese).

## Anti-patterns (always avoid)

- **Allowing comprehensive-survey issues**: Topics like "Intro to X", "X summary", "Everything about X" fail the issue-quality bar. Always force narrowing.
- **Letting topics through without primary-source basis**: Articles that "could be written from official docs alone" waste downstream time if not blocked at the planning phase.
- **Letting vague issues slip into downstream phases**: Even if the user says "let me figure it out as I write", at minimum complete Step 5 (the single-sentence issue).
- **Accepting the user's first idea as-is**: Always run the three-conditions check.

## Upstream-return signals

This skill is the most upstream — there's nowhere to return to. However, if any of the following apply, discard the topic entirely and start over with a different one:

- The three-conditions check fails on at least one condition no matter how the topic is reframed.
- The cost of acquiring primary-source material is excessive (the topic is too early for the user's current state).
- The survey shows that comparable or better content already exists in abundance.

## Transition to next phase

Once `./.cadenza/state.md` is written, ask the user:

> ✅ Phase 1 (Issue Finding) is complete. Proceed to `issue-decomposition` (structure design)?

- **Proceed**: Invoke `cadenza:issue-decomposition` via the Skill tool.
- **Hold**: Wait for revision instructions. Do not advance.
