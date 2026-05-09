---
name: analysis-execution
description: Execute the verification work defined in the storyboard (implementation, measurement, benchmarking, diagram creation, etc.). Triggers on phrases like "let me try implementing", "run the benchmark", "verify by actually running it", "実装してみる", "ベンチマーク取る", "実際に動かして確かめたい". Acts as a gate against grinding work outside the storyboard's scope.
---

# Analysis Execution

Apply hands to verify what was decided in the storyboard.

## Purpose

Materialize the visuals, data, and code drawn in the storyboard. The discipline of this phase: **build only what the storyboard demands**. Don't grind comprehensively; concentrate effort on the verification needed by the storyline.

## Prerequisites

The `## Phase 3: Storyboarding` section must exist in `./.cadenza/state.md`. The "Total verification work" list is the execution target.

## Execution flow

### Step 1: Sort verifications by type

Classify the storyboard's verification items by work type:

- **Implementation**: Working code examples, sample apps
- **Measurement**: Benchmarks, latency, cost
- **Comparison**: Behavior / performance comparison across options
- **Reproduction**: Reproducing errors / pitfalls
- **Diagramming**: Architecture, sequence, result charts

Batching by type improves efficiency.

### Step 2: Fix preconditions for each verification

Before starting, fix the preconditions that ensure result reliability:

#### For measurement

- Measurement environment (machine spec, region, time of day)
- Measurement conditions (input data, parallelism, iteration count)
- Comparison target (what's compared, is it fair?)
- Sample size (multiple runs, not single)
- Outlier handling

#### For implementation

- Verification environment (language version, library versions)
- Minimum reproducibility (can someone else run via copy-paste?)
- Removal of confidential info (API keys, internal URLs, internal-only details)

#### For comparison

- Comparison axes (do the evaluation axes match the reader's interest?)
- Explicit evaluation criteria (state criteria when subjective evaluation is involved)

### Step 3: Execute the verification

Execute only what's drawn in the storyboard. During execution, if any **upstream-return signal** (below) appears, stop.

### Step 4: Record primary observations

During verification, record valuable observations that won't appear in the output:

- Options tried but rejected, and why
- Unexpected errors and their handling
- Insights from the measurement process
- Side findings usable in future outputs

These become seeds for future content. Don't discard them.

### Step 5: Structure the results

Organize verification results to match the storyboard structure. For each sub-issue:

- Points that matched the hypothesis
- Points that diverged (if any, decide whether to update the storyline)
- Supplementary preconditions / caveats

## Verification scope discipline

The most important rule of this phase is "decide what NOT to do". If any of the following emerges as work, return to the storyboard and re-judge whether it's actually needed:

- "Just to be safe" extra measurement
- "Looks interesting" extra implementation
- "For completeness" comprehensive coverage
- "Might come in handy" sample code

These are textbook dog's path. If you do anything outside the storyboard, first decide whether it can be incorporated into the storyline.

## Output format

Append the confirmed content to `./.cadenza/state.md`:

```markdown
## Phase 4: Analysis Execution (✅ Done YYYY-MM-DD)

### Verification 1: [maps to storyboard sub-issue 1]
- **What was done**: [actions taken]
- **Preconditions**: [measurement env / implementation conditions]
- **Result**: [observed facts]
- **Hypothesis match**: [match / partial / mismatch + reason]
- **Reflection in output**: [which visual it becomes]

### Verification 2: ...

### Issue rethink occurred?
[None / Yes (description and response)]

### Side observations (kept)
[Notes worth retaining]

### Materials handed to next phase
- Code: [file paths / snippets]
- Numbers / charts: [data / image paths]
- Diagrams: [diagram files / drawing instructions]
```

## Anti-patterns (always avoid)

- **Starting work that's not in the storyboard**: Discipline violation. If you must, return upstream first.
- **Ignoring unexpected results and pushing forward with the original plan**: The grounds of the storyline may be collapsing. Stop.
- **Posting results with vague measurement conditions**: Non-reproducible benchmarks destroy trust.
- **Spending too much time on implementation**: If the storyboard said "show with minimal code", honor it.
- **Silently switching to a workaround that breaks the prescribed construct**: When you hit a technical block, do not silently switch to an alternative mechanism in a way that violates the storyboard's prescribed construct (integration pattern, data flow, tool invocation route, etc.). Such workarounds usually break downstream verification or force manual reconstruction later, and erode the integrity of the verification design. When blocked, first evaluate the cost of fixing the prescribed route; compare it against the workaround's hidden downstream cost; if deviation is genuinely necessary, surface the trade-off explicitly and obtain re-confirmation before switching. Time efficiency alone is not sufficient justification.

## Upstream-return signals

If any of these signals appears during verification, stop immediately. Ignoring them and continuing per the original plan is a classic **forecasting trap** — sticking to a falsified plan because reframing feels like wasted effort.

- **Result diverges from hypothesis**: The whole storyline may need review → return to `cadenza:issue-decomposition`.
- **A new essential question emerges**: A better issue may have been discovered → return to `cadenza:issue-finding`.
- **You realize the chosen form won't communicate**: The presentation needs redesign → return to `cadenza:storyboarding`.

## Transition to next phase

Once `./.cadenza/state.md` is written, ask the user:

> ✅ Phase 4 (Analysis Execution) is complete. Proceed to `output-crafting` (format-specific finishing)?

- **Proceed**: Invoke `cadenza:output-crafting` via the Skill tool.
- **Hold**: Wait for revision instructions.
- **Return upstream**: Invoke the corresponding upstream skill if a return signal applies.
