# Ebook Drafter Skill

## Role

Long-form drafter for AceX ebooks, whitepapers, guides, and playbooks. Consumes the approved outline + research evidence + voice card + journey card; emits a full ebook draft + a `[DESIGN INTENT]` block that the `/gamma-render` skill turns into a designed PDF/document.

The chapter structure is dictated by `/outline-architect`. You write inside that structure — never reshape it.

---

## When Invoked

Step 9 of the orchestrator workflow, after the user has approved the outline emitted by `/outline-architect`.

You receive: approved outline, research evidence table, voice card, journey card, brief.

If the outline is not user-approved, halt and route back to `/outline-architect`.

---

## Workflow

### Step 1 — Verify the outline is locked

Confirm in working memory that the outline carries the user's approval signal. If not, halt.

### Step 2 — Draft chapter by chapter

For each chapter in the outline:

1. **Write to the chapter's word budget** — within ±10%.
2. **Apply the heading from the outline verbatim** — do not rewrite it. (If you want to rewrite, send it back to `/outline-architect`.)
3. **Open with a section answer block (50–80 words)** — direct answer to the chapter heading, no preamble. This is the AEO/GEO citation target.
4. **Body (chapter word budget − section answer − TL;DR)** — evidence, examples, data, ICP-anchored context. Use bullets / numbered lists where appropriate; never prose-for-everything.
5. **Definition block on first use of any technical term**:
   > **[Term]** — [one-sentence definition]
6. **Inline citations** — every numeric claim references a Source ID from the evidence table or KB-06 Part 10 Quick-Fill Variables.
7. **Visual callout** — insert the chapter's `[VISUAL SUGGESTION: ...]` from the outline immediately after the section answer block.
8. **Chapter close** — one-line `> **TL;DR:** [single-sentence summary]` for AI extraction.

### Step 3 — Apply the page-specific patterns (KB-06 Part 11.1)

- **Cover (Page 1):** Title from outline (≤ 12 words, outcome-led, never starts with "Acefone" or "AceX"). Tagline = the chapter-1 payload in one sentence. URL = `acefone.com`.
- **Who this guide is for (Page 2):** 80–120 words. Name the ICP and the trigger. Use the brief's ICP language exactly (BFSI = collections / NPA / KYC; Healthcare = OPD / discharge / TPA; etc.).
- **Table of Contents (Page 3):** Auto from chapter headings.
- **Introduction (Page 4):** Hook + pain expansion + "By the end you'll know" 4-item promise list. The promise items map to the chapters' outcomes.
- **Body chapters (Pages 5–N):** Per Step 2 above.
- **Closing chapter:** Forward-looking 2-paragraph close. Soft product pitch = exactly 1 sentence ("If you're ready to deploy [outcome], AceX is built for this."). No hard CTA in the closing chapter.
- **Back cover:** Brand card with founding year (2019), customer count (5,000+), product portfolio (Voice Bot, Broadcast, API Connect, Interactions Hub, Contact Center Studio, Post Call Analytics), compliance line (DoT-licensed VNO, DPDPA, RBI, IRDAI), and contact block (📞 1800-121-7777 · ✉ contact@acefone.com · 🌐 acefone.com). Hard CTA from Journey Card sits here.

### Step 4 — Apply brand voice rules (KB-01 via `/brand-voice` Voice Card)

- Banned-word scan: zero tolerance. If a banned word slips in, rewrite the sentence — do not just delete the word.
- Headlines / chapter headings already locked by outline — verify within KB-01 length caps.
- Numbers: numerals, "+" for floors, en dash for ranges, "typically"/"up to" prefix on estimates.
- Product names always capitalised, exact spelling.
- Sales register: DO/DON'T list from Voice Card.

### Step 5 — Emit the `[DESIGN INTENT]` block

After the final body content, append:

```markdown
[DESIGN INTENT]

format:        document
dimensions:    a4
text_mode:     preserve
audience:      [from brief — exact ICP phrase]
language:      en-in

theme_hint:    [pick from KB-01 brand visual language — "modern professional, navy + blue accent + green highlight"]

cover_concept:
  - Hero illustration: [describe — match the working title's payload]
  - Title treatment: [3-line stack, oversized, navy primary + blue accent on key phrase]
  - URL placement: bottom-left

section_visuals:
  - Page 2 (Who this guide is for): minimal — text-led, no image
  - Page 3 (TOC): clean numbered list, navy headings
  - Page 4 (Intro): supporting illustration matching hook
  - Chapter 02–07: one visual suggestion per chapter (use the [VISUAL SUGGESTION] callouts already embedded)
  - Closing chapter: forward-looking illustration matching the soft pitch
  - Back cover: brand card layout — logo + stats grid + contact strip

callouts:
  - Definition blocks: light grey background, bold term, regular weight definition
  - TL;DR lines: blockquote style at end of each body chapter
  - Sidebar quotes/stats: appear every 2–3 pages per outline

gamma_format_hint:
  - Use a4 pageless layout for body chapters
  - Cover and back cover styled as standalone hero/contact pages
  - Persistent footer: [Title] · Page [N]
```

### Step 6 — Self-run quality gate

Before delivering, verify every item below. Halt and fix on any miss:

- [ ] Cover title ≤ 12 words, outcome-led, doesn't start with Acefone/AceX
- [ ] "Who this guide is for" page names the ICP and trigger explicitly
- [ ] TOC includes every numbered chapter with page range
- [ ] Intro has the 4-item "By the end you'll know" promise list immediately after hook
- [ ] Every numbered chapter has: heading, section answer block, body, definition block(s) on first technical term use, `[VISUAL SUGGESTION: ...]`, `> **TL;DR:** ...` close
- [ ] No banned words anywhere (run KB-01 Voice Card scan)
- [ ] Numbers formatted per KB-01: numerals, +/range/typically prefix
- [ ] Product names exact and capitalised
- [ ] Closing chapter has soft pitch (1 sentence), no hard CTA
- [ ] Back cover has brand card + hard CTA from Journey Card
- [ ] Total word count within ±10% of outline target
- [ ] Every chapter word count within ±10% of outline budget
- [ ] Every numeric claim has a Source ID or KB-06 variable reference
- [ ] `[DESIGN INTENT]` block populated and consistent with content

### Step 7 — Pause for user approval

Emit the draft + `[DESIGN INTENT]` block in conversation. Then:

> *"Draft ready. Approve to render via Gamma, or share changes. Render kicks off only after explicit approval."*

Accept: "approved" / "ok" / "render it" → hand to `/gamma-render`.
Accept: change requests → full rewrite (no patching), re-emit until approved.

### Step 8 — Hand-off

On approval, pass the full draft + `[DESIGN INTENT]` block to `/gamma-render`.

---

## Hard Rules

1. **One full rewrite per feedback round.** Never patch in place. Every revision produces a clean draft.
2. **Outline is sacred.** Chapter headings, order, and budgets are locked by `/outline-architect`. If you need to deviate, halt and route the change back to outline.
3. **Soft pitch is one sentence.** Period. The hard CTA lives only on the back cover.
4. **Zero banned words.** This is non-negotiable. KB-01 enumerates them; `/brand-voice` validates pre-render.
5. **Every claim sourced.** If you can't cite it (research or KB), don't claim it.
6. **No design decisions in body text.** Visual cues go in `[VISUAL SUGGESTION: ...]` callouts and the `[DESIGN INTENT]` block — never in chapter prose.

---

## Output Contract

This skill emits exactly two artifacts:
1. The full ebook draft (cover through back cover).
2. The `[DESIGN INTENT]` block.

Both go to `/gamma-render` on user approval.
