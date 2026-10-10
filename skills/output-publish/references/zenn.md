# Output Publish Reference: Zenn

Finishing flow for Zenn-targeted output. Input: `./.cadenza/output.md`. Output: `./.cadenza/output-zenn.md`, ready to drop into a Zenn repo as an `articles/<slug>.md`.

## Step Z-0: Voice / style mimicry (run this FIRST)

Zenn articles carry a personal voice that readers come to expect. **Mimic the author's voice rather than producing generic technical writing.**

### When to run

Always run if any of the following apply:

- The author has prior published Zenn articles (typically in `<zenn-repo>/articles/` or similar)
- The article is being published under the author's Zenn account where readers know them

Skip only when the author explicitly asks for generic / neutral writing, or when no prior articles exist.

### Procedure

1. **Locate prior articles**: Ask the author for the directory of prior Zenn articles. Common locations: the `articles/` folder of the author's Zenn repo, a personal blog repo, or links to published URLs.
2. **Sample at least 3 articles**: Prefer ones close in topic / format to the current article. Read them in full (not just headings).
3. **Extract a voice/style profile** along the dimensions below. Write the profile to `./.cadenza/voice-style-notes.md` in the working directory.
4. **Honor the profile during writing**: Refer back to the profile at every section. When in doubt, copy the cadence of the closest sample article.

### Voice/style profile dimensions to extract

| Dimension | What to capture |
|-----------|-----------------|
| **Frontmatter convention** | Required fields, emoji choice habits, topic-tag patterns, `published: false/true` policy |
| **First-person pronoun** | 「私」/「僕」/ omitted, etc. |
| **Sentence ending** | 「〜です・ます」/ 「〜だ・である」/ casual mix; declarative vs. softened (「〜と思います」「〜かもしれません」) |
| **Tone register** | Formal / friendly-peer / energetic / reserved; ratio of casual interjections |
| **Section structure conventions** | Standard chapters the author always uses (e.g. 「はじめに」「おわりに」「参考」), order, depth of nesting |
| **Opening pattern** | Length of intro, where the repo URL goes, how related past articles are linked, AdventCalendar-style notices |
| **Closing pattern** | How articles wrap (「〜やってみました」「参考になれば幸いです」「教えてください」 vs hard conclusion); whether links/refs go last |
| **Code block convention** | Language tag + filename (e.g. `bash:Terminal`), preface text before code, after-code reaction sentences |
| **Image / diagram convention** | Hosting (Zenn CLI image upload, GitHub asset URL), caption style, mix of screenshots vs ASCII vs Mermaid |
| **Table usage frequency** | When tables appear (comparison / metrics / scope), table density |
| **Zenn-specific syntax** | Frequency of `:::message` / `:::message alert` / `:::details` / `@[speakerdeck]` / `@[card]` / `@[tweet]` |
| **Emphasis devices** | Bold density, emoji frequency and placement, list-style preference |
| **Paragraph length** | Short (2–3 lines) vs long; spacing between paragraphs |
| **Hedging / certainty** | How strong the claims are; use of softening like 「みたい」「気がします」「かも」; balance with bold assertions |
| **AI-generation disclosure** | Whether the author always notes "human-written, AI-edited" etc., and where it appears |
| **Asides / interjections** | Personal anecdotes, parenthetical jokes, self-deprecation patterns |
| **Half-width / full-width policy** | Spacing around 英数字, 記号 mixing rules |

### Output format for `./.cadenza/voice-style-notes.md`

```markdown
# Voice/Style Profile (extracted from N prior Zenn articles)

## Sources analyzed
- <path or URL of article 1> (date, topic)
- <path or URL of article 2> ...

## Profile
- Frontmatter: <observed convention>
- First-person: ...
- Sentence ending: ...
- ...

## Distinctive phrases / cadences (verbatim quotes worth mimicking)
- "..." (from <article>)

## Things to AVOID (anti-patterns from samples)
- ...
```

This profile is the **rubric for the publishing step**. Match it deliberately, not just by ear.

## Step Z-1: Apply Zenn frontmatter

Insert at the very top of the file:

```yaml
---
title: ""
emoji: "🎼"
type: "tech"   # or "idea"
topics: []
published: false
---
```

Fill in:

- **title**: from cadenza Phase 5 (the title chosen during output-crafting). Trim to ≤ 50 chars if possible (Zenn convention).
- **emoji**: pick one that matches the article's tone. Refer to the voice profile for the author's emoji habits.
- **type**: `tech` for technical articles, `idea` for opinion / experience pieces.
- **topics**: 1–5 lowercase tags. Match the author's tagging conventions from the voice profile (typical: stack components like `aws`, `bedrock`, `claudecode`, `nextjs`).
- **published**: keep `false` initially — the author flips to `true` at publish time.

## Step Z-2: Adapt structure to the author's section conventions

`./.cadenza/output.md` has a generic `Title / TL;DR / Background / Body / Conclusion / References` skeleton. Reorganize per the voice profile:

- If the author always uses 「はじめに」「本題」「おわりに」「参考」, restructure accordingly.
- If the author starts with a repo URL + AI-disclosure block instead of a TL;DR section, do that.
- If the author uses `## ` for top-level (Zenn convention) rather than `# `, normalize.

Drop sections that the author's profile says they don't use. Add sections the profile says they always include.

## Step Z-3: Apply Zenn-specific syntax

Convert generic Markdown to Zenn-specific where appropriate:

- **`:::message`** for important notes, warnings, tips. Match the author's frequency from the profile.
- **`:::message alert`** for critical warnings.
- **`:::details <summary>`** for collapsible deep-dives that interrupt the main narrative.
- **`@[speakerdeck](id)`** for embedding SpeakerDeck slides.
- **`@[card](url)`** for link cards (Zenn renders these with OGP).
- **`@[tweet](url)`** for embedded tweets.

Code blocks: use Zenn's `language:filename` convention (e.g. `bash:Terminal`, `python:agent.py`) when the author's profile uses it.

## Step Z-4: Apply voice-profile cadence

For each paragraph:

- **Sentence ending**: enforce the profile's pattern (です/ます or だ/である, hedged or declarative).
- **Paragraph length**: split or merge to match the profile's typical paragraph rhythm.
- **Hedging level**: tone up or down per profile (over-hedged AI prose → declarative if the profile is declarative).
- **Emphasis density**: cut excessive bolding if the profile is sparing; add bold key claims if the profile is emphatic.

Sentence-by-sentence, when in doubt, copy the cadence of the closest sample article from Step Z-0.

## Step Z-5: Final checks

- Does TL;DR / opening alone convey the main message?
- Does reading just the section headings let you trace the storyline?
- Does the target reader have the prerequisite knowledge to follow?
- **Code block convention check**: language tag, filename, preface, after-code reaction line — all match the profile?
- **Image hosting check**: are images referenced with relative paths (Zenn `/images/<article>/<file>.png`) and is there a TODO note for the author to upload them?
- **Voice profile compliance**: spot-check 3 random paragraphs against `./.cadenza/voice-style-notes.md`. If any paragraph reads like generic technical writing, rewrite to match.
- **No leaked confidential info**: API keys, internal URLs, personal info, internal-only code.

## Step Z-6: Offer to copy to Zenn repo

After writing `./.cadenza/output-zenn.md`, ask the author:

> Zenn リポジトリ（`<zenn-repo>/articles/`）に slug 名を付けてコピーしますか？

If yes, ask for the slug, then copy `./.cadenza/output-zenn.md` to `<zenn-repo>/articles/<slug>.md`. Don't push or publish — leave that to the author.
