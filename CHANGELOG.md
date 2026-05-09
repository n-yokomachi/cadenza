# Changelog

## [0.3.1] - 2026-05-07

### Changed

- **`issue-finding` SKILL.md**: 「Critical protocol — do not bypass」セクションを冒頭に追加し、Step 1 / Step 2 / Step 5 が **explicit user articulation** を要求することを明記。Auto Mode や continuous-execution flag では override できない旨を記載。
- **Step 2** の文言を「Have the user articulate」→「**Stop and explicitly ask the user**」に強化。fabricate / infer from prior conversation / fill in defaults を明示的に禁止。
- **Step 5** の文言を「get the user's agreement」→「**wait for the user's explicit verbal agreement** (e.g. "OK" / "進めて")」に強化。implicit agreement では不十分である旨を明示。

### Background

cadenza:issue-finding を Auto Mode で実行した際、AI が Step 2 (target reader / problem hypothesis / post-read change) をユーザーに尋ねず勝手に fabricate して進めてしまう protocol 違反が発生。これに気づいたユーザーからの指摘で、SKILL.md 側で明示的に止めるよう改修。Auto Mode は「Make reasonable assumptions and proceed on low-risk work」だが、Phase 1 のユーザー言語化は low-risk ではなく workflow の根幹であるため、Auto Mode の判断対象外であることを skill 側で明文化した。

## [0.3.0] - 2026-05-07

### Changed

- **`output-crafting` now produces a single, format-agnostic Markdown file** (`./.cadenza/output.md`) instead of multiple format-specific files. Platform-specific finishing (Zenn frontmatter, SpeakerDeck/Marp directives, LT-specific compression) is now downstream of the cadenza workflow.
- **`issue-finding`**: removed Step 6 (intended output format selection) — the workflow is now format-agnostic at the planning phase.
- **`issue-decomposition`**: storyline pattern recommendation no longer references intended output format.
- **`storyboarding`**: Step 3 reframed from blog/deck/lt to dominant output style (long-form prose / slide presentation / short-form). The platform-specific finishing notes have been removed.
- **`output-proofread`** / **`output-review`**: path references updated from `./.cadenza/output-blog.md` etc. to `./.cadenza/output.md`.
- This repository is now self-contained as a Claude Code marketplace. A top-level `.claude-plugin/marketplace.json` allows direct install via `claude plugin marketplace add github.com/n-yokomachi/cadenza`.
- Added top-level `LICENSE` (MIT) and OSS-facing `README.md`.

### Removed

- `skills/output-crafting/references/{blog,deck,lt}.md`: format-specific finishing references. Equivalent functionality moved out of the OSS plugin.

## [0.2.0] - 2026-05-06

### Changed

- **Renamed plugin from `issue-driven` to `cadenza`** to make the project's identity independent of any specific book or framework, and to reframe the README around "inspiration from issue-first thinking traditions" rather than direct attribution.
- Workflow state and output artifacts now live under `./.cadenza/` (previously `./.issue-driven/`). The directory rename matches the new plugin name.
- Skill descriptions no longer reference any specific author or book; they describe each phase's role in the cadenza workflow directly.
- README rewritten with an `Inspiration` section that frames the phased structure as a reinterpretation of issue-first thinking, with no claim of implementation or endorsement.
- License clarified as MIT at the repository root.

### Added

- `output-proofread` skill (AI-driven exhaustive accuracy + language audit).
- `output-review` skill (author-led review-cycle support).

## [0.1.0] - 2026-05-02

### Added

- Initial release.
- Five skills covering the full pipeline: `issue-finding`, `issue-decomposition`, `storyboarding`, `analysis-execution`, `output-crafting`.
- Shared state file convention for cross-skill handoff.
- Format-specific references for `output-crafting` (`blog.md`, `deck.md`, `lt.md`).
- Auto-chained transition: each skill prompts the user to proceed to the next phase and invokes the next skill via the Skill tool on confirmation.
- "Upstream-return signal" section in every skill for principled retreat to upstream phases.
- All skill content authored in English (mixed-language descriptions retain Japanese trigger phrases for natural Japanese-speaking user invocation).
