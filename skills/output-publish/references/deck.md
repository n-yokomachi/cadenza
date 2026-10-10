# Output Publish Reference: Deck (long-form talk)

Finishing flow for conference-deck output. Input: `./.cadenza/output.md`. Output: `./.cadenza/output-deck.md` — slide-structured Markdown ready to be fed to an AI slide-generation tool (Claude Design, Gamma, etc.) for visual rendering.

## Design intent

The output is **NOT a Marp/Reveal/PowerPoint file**. It is structured Markdown describing each slide's:

- Title
- Visual specification (what should be shown)
- Talking points (what the speaker says)
- Optional: speaker notes, data references

AI slide tools consume this structure and produce the actual visual deck. Keep the Markdown semantic, not stylistic.

## Step D-1: Decide slide count and time budget

For a typical conference talk:

- **30 min**: ~25–35 slides (excluding appendix)
- **45 min**: ~35–50 slides
- **60 min**: ~50–70 slides

Confirm the time budget with the author before structuring.

## Step D-2: Assemble the deck skeleton

```
1. Title slide
2. Self-intro (1 slide)
3. Agenda (1 slide)
4. Background / problem statement (2–4 slides)
5. Body — one chapter per sub-issue from cadenza Phase 2
   - Chapter divider slide
   - Slides supporting that sub-issue (built from Phase 3 storyboard)
6. Main message (1 standalone slide, prominently designed)
7. Conclusion (recap + takeaway, 1–2 slides)
8. Q&A prompt slide
9. Appendix (backup slides for likely Q&A topics)
```

Map sub-issues → chapters using the storyline from cadenza Phase 2 (`./.cadenza/state.md`).

## Step D-3: Write each slide as a structured block

Use this format for every slide (the AI slide tool consumes this structure):

```markdown
## Slide N: <Slide Title>

**Visual**: <what the slide should show — diagram type, code snippet, comparison table, screenshot, etc.>

**Talking points** (what the speaker says):
- <point 1, one sentence>
- <point 2, one sentence>
- <point 3 if needed>

**Speaker notes** (optional, for the speaker only):
> <longer context, personal anecdote, fallback explanation>

**Data ref** (optional, when the slide cites a verification result):
- See `./.cadenza/state.md` Phase 4 → <specific verification entry>
```

Rules:

- **One slide, one message**: the slide title states the single claim it carries.
- **Visual primary, text secondary**: bullet points are speaker prompts, not paragraphs to read.
- **Verbal narration is the supplement**: the slide itself shouldn't be self-contained; the speaker fills in.

## Step D-4: Chapter divider slides

Insert chapter divider slides between sub-issues. They look like:

```markdown
## Slide N: <Chapter Number>. <Sub-issue Title>

**Visual**: large chapter number + sub-issue title in big type. Optional: a 1-line summary of what's coming.

**Talking points**:
- <one-sentence framing of why this chapter matters to the main message>
```

## Step D-5: Polish the decisive slides

Mark slides that deserve extra polish in the AI tool prompt:

- The **main-message slide** — physically prominent, single-message, high-contrast design.
- The **slide for the most important verification result** — clear data viz, source cited.
- The **opening slide** — strong hook for audience attention.

In the output file, mark these with a `<!-- decisive: yes -->` comment so the AI slide tool can give them visual weight.

## Step D-6: Appendix prep

Prepare 3–8 appendix slides for likely Q&A topics. Cut from main flow:

- Detailed verification results too dense for main flow
- Edge cases the author wants to be ready to discuss
- Deeper-dive diagrams

Mark them with `## Slide A.N: <Title>` (use `A.` prefix).

## Step D-7: Voice / persona alignment

For talking points and speaker notes, match the author's spoken-presentation style if known:

- Formal academic vs casual storytelling
- Density of personal anecdotes
- Joke / aside frequency
- Acknowledgment patterns (audience, collaborators)

If `./.cadenza/voice-style-notes.md` exists from a previous Zenn run, use it as a hint — but spoken style differs from written style, so adapt.

## Step D-8: Output structure

Write to `./.cadenza/output-deck.md`. Top of file:

```markdown
# <Talk Title>

- Speaker: <name>
- Time budget: <minutes>
- Slide count: <count> + <appendix count> appendix
- Main message: <one-sentence main message from cadenza Phase 2>
- Source: derived from `./.cadenza/output.md` and `./.cadenza/state.md`

---

[Slide blocks follow in order]
```

The header gives the AI slide tool global context before it processes individual slide blocks.

## Step D-9: Final checks

- Does each slide title state a single claim?
- Lining up the slide titles, can you trace the storyline?
- Are the decisive slides marked?
- Are visualization specs concrete enough that the AI tool can produce something faithful?
- Any leaked confidential info in talking points or speaker notes?
