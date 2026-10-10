# Output Publish Reference: LT (Lightning Talk)

Finishing flow for LT output. Input: `./.cadenza/output.md`. Output: `./.cadenza/output-lt.md` — pared-down slide-structured Markdown ready to be fed to an AI slide-generation tool.

## Design intent

LT is **impact, not coverage**. The audience forgets most things after the talk ends. Design assuming they remember exactly one thing.

Like `deck.md`, the output is **NOT a Marp/Reveal file** — it's structured Markdown that an AI slide tool can render visually. But the structure is far more compressed.

## Step L-1: Time budget → slide count

- **5-min LT**: 5–8 slides
- **10-min LT**: 10–15 slides
- **3-min LT (Ignite-style)**: 5 slides exactly

Rule of thumb: 30s–1min per slide. Confirm time budget with the author and **decide the slide count cap up front**. Cut to fit.

## Step L-2: Aggressive trimming

LT is impact, not coverage. Cut:

- Detailed background → problem statement in **one slide**
- Full comparison tables → narrow to **2–3 main axes**
- Detailed code → image-only for atmosphere, or only the most important snippet
- Detailed verification steps → overall picture and the **decisive moment only**
- Self-intro → one line on the title slide, not a separate slide

Rule: **if it doesn't move the audience toward the single takeaway, cut it.**

## Step L-3: Skeleton

```
1. Title slide (with one-line self-intro and contact)
2. Hook (1 slide — why should the audience care, in 30 seconds)
3. Body (3–8 slides — minimum viable evidence for the takeaway)
4. Main message (1 standalone slide — physically large, single sentence)
5. Closing (1 slide — restate main message + 1 next-action prompt for audience)
```

Optional last slide: SNS / link to the corresponding blog post (so the audience can dig deeper after the talk).

## Step L-4: Slide block format

Same structure as `deck.md` but more compressed:

```markdown
## Slide N: <Slide Title>

**Visual**: <what the slide should show — keep it simple, one focal point>

**Talking points** (max 2):
- <point 1, one short sentence>
- <point 2 if absolutely needed>
```

For LT, **drop the speaker-notes section entirely** unless the author specifically requests it. The author memorizes the talk; speaker notes during a 5-min slot are unread anyway.

## Step L-5: Main-message slide design

The main-message slide is the moment everything builds toward. Make it physically dominant:

```markdown
## Slide N: <Main Message Slide>

**Visual**: ONE sentence in very large type. Optional: a single supporting icon or number. Background should be high-contrast.

**Talking points**:
- <speak the message word-for-word, then pause>

<!-- decisive: yes -->
```

Repeat the main-message wording at opening (Hook slide) and closing — three exposures cement memory.

## Step L-6: Promo / SNS post drafts

LTs depend heavily on post-talk distribution. After the slide blocks, append a `## Promo` section:

```markdown
## Promo

### SNS post (Twitter/X, ~140 chars Japanese)
<draft tweet announcing slide release, with main-message hook>

### SNS post (longer version, threaded or for Mastodon/Bluesky)
<2–3 sentence version with more context>

### Slack announcement (internal community, if applicable)
<3–4 sentence version with link>

### Link to the corresponding article (if one exists)
<URL or "TODO: write blog post">
```

The author edits these before posting; this skill provides the draft.

## Step L-7: Output file structure

```markdown
# <LT Title>

- Speaker: <name>
- Time budget: <minutes>
- Slide count: <count>
- Main message (one sentence): <main message>

---

[Slide blocks follow]

---

## Promo

[promo drafts follow]
```

## Step L-8: Final checks

- Can the audience get the main message even if they only catch 1 slide of the talk?
- Is the slide count within the time-budget cap?
- Does the title slide contain the speaker's name + contact?
- Is the main-message slide marked `<!-- decisive: yes -->`?
- Are the promo drafts ready (or marked TODO)?
