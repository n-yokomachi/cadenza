---
name: output-publish
description: Take the format-agnostic ./.cadenza/output.md and finish it for a specific publishing target (Zenn blog / deck-style markdown for AI slide tools / LT). Triggers on phrases like "Zenn 用に整える", "スライドにする", "LT 形式にする", "publish to Zenn", "make slide markdown", "make LT markdown". Runs after cadenza:output-crafting, downstream of the cadenza workflow.
---

# Output Publish

Format-specific finishing for the cadenza output. This skill is the **publishing layer** that takes the generic `./.cadenza/output.md` and turns it into the specific shape needed for the target publishing platform.

## Purpose

`cadenza:output-crafting` produces a single format-agnostic Markdown file. This skill applies platform-specific publishing conventions:

- For Zenn: voice profile mimicry + Zenn frontmatter + Zenn-specific syntax (`:::message`, `:::details`, etc.)
- For deck-style output: structured Markdown with explicit slide blocks, designed to be fed to an AI slide-generation tool (Claude Design, Gamma, etc.) for visual rendering
- For LT-style output: aggressively pared-down version of the deck-style output with a single takeaway

The skill never publishes directly. It produces a file the author then publishes manually.

## Prerequisites

`./.cadenza/state.md` must contain `## Phase 5: Output Crafting` and `./.cadenza/output.md` must exist. If not, instruct the user to run `cadenza:output-crafting` first.

## Execution flow

### Step 1: Confirm the target(s)

Ask the user which target(s) to produce:

- `zenn` — full-length Zenn article ready to drop into the user's Zenn repo
- `deck` — slide-structured Markdown for AI slide tools (long-form, 30–60 min talk)
- `lt` — pared-down slide-structured Markdown (5–10 min LT)

Multiple targets can be selected — produce one output file per target.

### Step 2: Read the corresponding reference

| Target | Reference |
|--------|-----------|
| `zenn` | [`references/zenn.md`](./references/zenn.md) |
| `deck` | [`references/deck.md`](./references/deck.md) |
| `lt` | [`references/lt.md`](./references/lt.md) |

Each reference defines the finishing flow for that target. Follow it in order.

### Step 3: Generate the output

Read `./.cadenza/output.md`, apply the reference's finishing rules, and write the result.

| Target | Output path |
|--------|-------------|
| `zenn` | `./.cadenza/output-zenn.md` |
| `deck` | `./.cadenza/output-deck.md` |
| `lt` | `./.cadenza/output-lt.md` |

When the user has a specific destination repo (e.g. `<zenn-repo>/articles/`), offer to copy the file there as well — but always write the working copy to `./.cadenza/` first.

### Step 4: Update state file

After generating, append to `./.cadenza/state.md`:

```markdown
## Phase 5b: Output Publish (✅ Done YYYY-MM-DD)

### Generated outputs
- [ ] zenn: ./.cadenza/output-zenn.md
- [ ] deck: ./.cadenza/output-deck.md
- [ ] lt: ./.cadenza/output-lt.md

### Voice profile in use
[ path of ./.cadenza/voice-style-notes.md, or "none" ]
```

(Use the `Phase 5b` heading to distinguish from the format-agnostic `Phase 5: Output Crafting` that produced `output.md`.)

## Anti-patterns (always avoid)

- **Skipping voice profile for Zenn**: When the author has prior Zenn articles, always run the voice-profile extraction (Step Z-0 in `references/zenn.md`) before writing. Producing generic technical writing under the author's name on a platform where readers know them dilutes the personal brand.
- **Embedding presentation-tool-specific directives**: Don't embed Marp, Reveal.js, or PowerPoint-specific directives. The deck/lt output is structured Markdown for AI slide tools to consume — let those tools handle the visual rendering.
- **Re-running the cadenza workflow**: This skill takes `output.md` as-given. If the user wants to revise the storyline or main message, return them to the appropriate cadenza phase (`cadenza:issue-finding` / `issue-decomposition` / etc.) — don't try to fix structural issues here.
- **Confidently rewriting the author's prose**: Apply the platform-specific syntax and frontmatter, but preserve the author's wording from `output.md` unless the reference explicitly requires rewriting (e.g. for slide compression).

## Upstream-return signals

If the user reports that the published artifact reveals deeper issues:

- The voice profile mimicry felt off → re-run the voice-profile extraction with more samples (re-do Step Z-0 in `references/zenn.md`).
- The slide compression cut something essential → return to `cadenza:storyboarding` to re-think what the visual decisive moments are.
- After publishing, "what am I actually saying?" is blurry → return to `cadenza:issue-finding`.

## Workflow position

```
cadenza:output-crafting   →   ./.cadenza/output.md (generic)
        ↓
cadenza:output-publish (this skill)
        ↓
./.cadenza/output-{zenn,deck,lt}.md (target-specific)
        ↓
[Manual] copy to Zenn repo / feed deck/lt to AI slide tool
```

The cadenza-side review skills (`cadenza:output-proofread` / `cadenza:output-review`) operate on `output.md`. For target-specific review (e.g., voice-profile compliance check on `output-zenn.md`), invoke `cadenza:output-proofread` with the target file path explicitly — it will use the voice profile if `voice-style-notes.md` exists.
