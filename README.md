**English** | [日本語](README.ja.md)

# cadenza

A Claude Code plugin that turns "I want to write a blog post / talk / LT" into a disciplined, gated pipeline. Each phase — *find the issue → decompose → storyboard → verify → craft* — acts as a gate, blocking premature writing and forcing the author to clarify the question before producing the answer.

The name *cadenza* comes from the classical-music term for the structured-yet-expressive solo passage in a concerto: each phase is a disciplined step, but the final output is the author's own voice.

## Install

This repository is itself a Claude Code marketplace. Add it and install the `cadenza` plugin:

```bash
claude plugin marketplace add github.com/n-yokomachi/cadenza
claude plugin install cadenza@cadenza
```

Then restart Claude Code.

## What it does

Once installed, the plugin exposes 7 skills, callable as slash commands:

| Phase | Skill | Role |
|-------|-------|------|
| 1. Plan | `/cadenza:issue-finding` | Identify "what question is worth answering" |
| 2. Design | `/cadenza:issue-decomposition` | Decompose into sub-issues, design the storyline |
| 3. Show | `/cadenza:storyboarding` | Design how each sub-issue is shown |
| 4. Verify | `/cadenza:analysis-execution` | Implement / measure / draw per the storyboard |
| 5. Finish | `/cadenza:output-crafting` | Produce the final Markdown artifact |
| Review | `/cadenza:output-proofread` | AI-driven exhaustive accuracy + language audit |
| Review | `/cadenza:output-review` | Author-led review-cycle support |

Each skill, on completion, asks "proceed to next phase?" and chains to the next skill on confirmation. State is shared via `./.cadenza/state.md` in the working directory. The final artifact is written to `./.cadenza/output.md`.

## What you do with `output.md`

cadenza deliberately produces a **single, format-agnostic Markdown file**. Platform-specific finishing — Zenn frontmatter, SpeakerDeck Marp directives, LT-specific compression — is left to downstream tooling so this plugin stays useful regardless of where you publish.

Some common downstream paths:

- **Blog posts (Qiita / Zenn / dev.to / personal blog)**: copy `output.md` into your blog repo, add platform-specific frontmatter, adjust syntax (`:::message` for Zenn, etc.).
- **Conference decks / LT slides**: feed `output.md` to an AI slide-generation tool (Claude Design, Gamma, etc.) which can consume the structured Markdown and produce visual slides.
- **Documentation**: drop `output.md` into your docs folder as-is.

## Why "gated"

Without this kind of plugin, Claude Code is happy to start writing the moment you ask. That's helpful for short tasks, but for technical content — blog posts, conference talks — it produces drafts that look fine and read fluently, but rest on questions that were never sharpened. *cadenza* deliberately slows that down: each phase has explicit anti-patterns and "upstream-return signals" that force you to revisit earlier decisions when something downstream reveals the foundation was weak.

## Inspiration

The phased structure draws on the issue-first thinking tradition popularized in Japanese knowledge-work literature — most notably in Kazuto Ataka's *『イシューからはじめよ』* (Eiji Press, 2010). That tradition established the now-common idea that the quality of an output is gated more by the quality of the question being answered than by the effort spent on the answer.

This plugin is **not an implementation of any specific book or framework**, and is not affiliated with or endorsed by any author or publisher. It is an independent reinterpretation that takes the general principle — *narrow the question before writing the answer* — and adapts it to the specific context of technical content creation, expressed as a Claude Code skill pipeline.

For readers interested in the broader thinking-method literature, the Ataka book is one well-known starting point, alongside English-language traditions such as Minto's pyramid principle and the McKinsey "Sky-Rain-Umbrella" framing.

## Repository structure

```
cadenza/
├── .claude-plugin/
│   ├── marketplace.json     ← this repo as a Claude Code marketplace
│   └── plugin.json          ← the plugin manifest
├── README.md                 ← you are here
├── README.ja.md              ← 日本語版
├── CHANGELOG.md
├── LICENSE                   ← MIT
└── skills/
    ├── issue-finding/SKILL.md
    ├── issue-decomposition/SKILL.md
    ├── storyboarding/SKILL.md
    ├── analysis-execution/SKILL.md
    ├── output-crafting/SKILL.md
    ├── output-proofread/SKILL.md
    └── output-review/SKILL.md
```

## License

MIT. See [`LICENSE`](LICENSE).

## Author

Naoki Yokomachi — [@n-yokomachi](https://github.com/n-yokomachi)
