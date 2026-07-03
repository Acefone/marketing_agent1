---
name: acefone-page-copyreviewer
description: >
  Reviews and rewrites Acefone landing page copy section by section. Fetches a live URL,
  pulls brand voice/positioning/approved stats from Notion knowledge base, runs a severity-flagged
  audit, then produces a current-vs-suggested copy table with SEO/AEO recommendations baked in.
  Trigger when user provides an Acefone page URL and asks for: copy review, copy rewrite,
  page audit, content audit, landing page suggestions, "fix this page", "rewrite this page",
  or similar. Always asks user to confirm framework + audience + competitor URLs before drafting.
---

# Acefone Page Copywriter

## Trigger Phrases
- "review the copy on [URL]"
- "rewrite this landing page"
- "audit [URL]"
- "give me copy suggestions for [page]"
- "content review for [URL]"
- "fix the copy on [page]"

## Required Inputs (from user)
- **Page URL** — the live Acefone page to review
- Optional: competitor URLs for benchmarking
- Optional: page brief / specific objective (e.g. "align with AceX voice bot")

## Execution Flow

### Step 1 — Confirm scope with user (ALWAYS ASK BEFORE DRAFTING)
Ask three questions in a single `ask_user_input_v0` call:

1. **Target audience for THIS page** (single_select):
   - Pre-sales / lead qualification (decision-stage SMB)
   - Enterprise IT/operations leaders (RFP-stage)
   - Mid-market business owners (awareness/consideration)
   - Other (let me specify)
2. **Copywriting framework to lock in for this page** (single_select):
   - AIDA (Attention, Interest, Desire, Action)
   - PAS (Problem, Agitate, Solve)
   - BAB (Before, After, Bridge)
   - Mix — pick best per section
3. **Competitor URLs for benchmarking** (text input OR skip):
   - "Paste competitor URLs, type 'KB-04' to use the default competitor list from your knowledge base, or 'skip' to skip benchmarking"

Do not proceed to Step 2 until all three are answered.

### Step 2 — Fetch the page
- Use `web_fetch` with `html_extraction_method: "markdown"` on the user's URL.
- Map the page's actual section structure (Hero, sub-sections, features, CTA, etc.) — do NOT impose a template.

### Step 3 — Pull brand context from Notion

Use the Notion MCP `fetch` tool to retrieve the four core knowledge base pages by their exact page IDs (these are pinned — never search for them):

| Doc | Page ID | URL |
|---|---|---|
| KB-01 Brand Identity | `d5063300-59da-83e4-aafa-013e3b7a9503` | https://www.notion.so/d506330059da83e4aafa013e3b7a9503 |
| KB-03 Target Audience & ICP | `82463300-59da-8217-971d-01894d6b8e32` | https://www.notion.so/8246330059da8217971d01894d6b8e32 |
| KB-04 Market & Competitive Strategy | `a2163300-59da-83eb-89d3-015d3ebf2720` | https://www.notion.so/a216330059da83eb89d3015d3ebf2720 |
| KB-06 Content Templates | `6d663300-59da-82a1-9d40-81e89e5e118c` | https://www.notion.so/6d66330059da82a19d4081e89e5e118c |
| KB-07 Messaging & Copy Guidelines | `43e63300-59da-83d5-8dee-81f638260c89` | https://www.notion.so/43e6330059da83d58dee81f638260c89 |

**What to extract from each:**

- **KB-01 Brand Identity** → voice, tone, personality attributes, what the brand sounds like and what it does NOT sound like
- **KB-03 Target Audience & ICP** → match against the audience selected in Step 1; pull pain points, jobs-to-be-done, objections, decision criteria for that specific persona only
- **KB-04 Market & Competitive Strategy** → official competitor list, positioning vs each, differentiators. **This is the canonical competitor source — use these competitors for Step 4 benchmarking unless the user explicitly provides different URLs.**
- **KB-06 Content Templates** → check if there's a template that matches the page type (product, solution, comparison, etc.) and follow its structure where relevant
- **KB-07 Messaging & Copy Guidelines** → banned words list, flowery language list, approved CTAs, messaging do's and don'ts, approved stats and proof points

**Additionally, search for product-specific positioning** (only if the page is about a specific product like AceX, Contact Center Studio, etc.):
Use `notion-search` with `query_type: "internal"`, `page_url` set to the parent Knowledge Base, and query: `"[PRODUCT NAME] positioning"` — e.g. `"AceX positioning"`.

If any KB page fetch fails or returns empty, note it in the final output ("⚠️ Couldn't access KB-XX — applied general B2B SaaS conventions for [section]") and proceed with what's available.

### Step 4 — Fetch competitor pages

**Default behavior**: Use the competitors listed in KB-04 (pulled in Step 3). Pick the 2–3 most relevant competitors based on the page's product/positioning.

**Override behavior**: If the user provided specific competitor URLs in Step 1, use those instead and ignore KB-04 for this run.

**Never** search the open web for random competitors — only use KB-04 or user-provided URLs.

For each competitor, `web_fetch` the equivalent product page and extract:
- Hero headline
- Primary value prop
- CTA copy
- Any messaging hook worth benchmarking against

Use this only as a benchmark — never copy phrasing.

### Step 5 — Run the audit
Identify issues across the page. For each issue assign a severity:
- **🔴 Critical** — message-level breaks (wrong audience, missing value prop, off-brand claim, factually wrong)
- **🟡 Medium** — clarity/flow issues (jargon, weak CTA, structural problems)
- **🔵 Low** — polish (grammar, minor word choice, spacing)

**Special rule for banned words**: Only flag in the audit if the same banned word/phrase appears **3+ times** across the page (signals systemic issue, not isolated slip). Single occurrences get silently fixed in the rewrite table.

### Step 6 — Produce the Markdown output file
Save to `/mnt/user-data/outputs/` with filename: `[page-slug]-copy-review-[YYYY-MM-DD].md`

Use this exact structure:

```markdown
# Copy Review — [Page Title]

**URL**: [page URL]
**Reviewed**: [date]
**Target audience (locked for this review)**: [from Step 1]
**Framework applied**: [from Step 1]
**Competitor benchmarks**: [list URLs or "none"]

---

## 🔍 Audit Summary

### 🔴 Critical
- [issue] — [1-line explanation]
- [issue] — [1-line explanation]

### 🟡 Medium
- [issue] — [1-line explanation]

### 🔵 Low
- [issue] — [1-line explanation]

> [Only include this block IF banned words appeared 3+ times]
> **⚠️ Systemic voice issue**: "[banned word]" used [N] times across the page. This is part of Acefone's banned vocabulary — see brand voice doc.

---

## ✍️ Section-by-Section Rewrite

### [Section name from live page, e.g. "Hero"]

| Current | Suggested | Why |
|---|---|---|
| [exact current copy] | [rewritten copy] | [1-line rationale — tie to framework or audience] |

[For Hero headline ONLY — include 2 A/B variants:]
**A/B variants for headline:**
- **Variant A**: [headline] — angle: [outcome-led / problem-led / curiosity]
- **Variant B**: [headline] — angle: [different angle]

[Repeat the table block for every section that exists on the live page]

### [Primary CTA section]

| Current | Suggested | Why |
|---|---|---|
| [current CTA] | [suggested CTA] | [rationale] |

**A/B variants for primary CTA:**
- **Variant A**: [CTA] — psychology: [low-friction / value-led / urgency]
- **Variant B**: [CTA] — psychology: [different angle]

---

## 🔎 SEO + AEO Recommendations

### Meta tags
- **Title tag** (under 60 chars): [recommendation]
- **Meta description** (under 155 chars): [recommendation]
- **H1**: [recommendation — should be the page's primary keyword + value prop]

### Schema markup (recommend adding)
- [e.g. Product schema, FAQPage schema, BreadcrumbList — list 2–3 relevant types]

### Citable passages (for AI engines like ChatGPT, Perplexity, Google AI Overviews)
Add these 40–60 word standalone answers under each H2 — engineered to be cited by LLM-based search:

**Under "[H2 name]":**
> [40–60 word citable passage answering the most likely question for this H2]

[Repeat for 2–3 key H2s on the page]

### FAQ block (recommend adding to end of page)
5–8 questions a buyer would actually ask, with 50–80 word answers:

1. **[Question]?**
   [Answer]

2. **[Question]?**
   [Answer]

---

## 📌 Notes
- [Anything user should know — e.g. "Couldn't find AceX positioning doc in Notion, applied general voice bot positioning"]
- [Suggestions outside copy scope — e.g. "Hero image should show contact center UI not a generic person on headset"]
```

### Step 7 — Present the file
Use `present_files` to share the Markdown file with the user. Keep the chat response under 4 sentences — point them to the file, flag any Notion gaps, mention the 2 A/B variants are inside.

## Rules
- **Always confirm** Step 1 questions before drafting. Never skip and assume.
- **Match the live page's structure** — do not impose Hero/Problem/Solution/etc. if the page doesn't have it
- **One quote per source max** if quoting competitor copy. Paraphrase otherwise.
- **No flowery rewrites** — short sentences, B2B tone, concrete outcomes
- **CTAs**: action verb + outcome (e.g. "Book a 15-min demo" not "Learn more")
- **No filler in chat response** — just file + 1–2 key callouts

### Heading Rules (apply to every rewritten heading)

**H2:**
- Title Case (capitalize all major words)
- 7–8 words maximum
- No punctuation (no periods, commas, colons, em-dashes, question marks)
- Never generic. Bad: "Our Features", "Why Choose Us", "Benefits". Good: "Cut Call Drop Rate by 40 Percent"
- Must carry a concrete message — outcome, proof, or specific claim

**H3:**
- Sentence case (only first word + proper nouns capitalized)
- 10–12 words maximum
- Punctuation allowed (periods, commas, colons fine)
- Should expand on the H2 above it, not repeat it

**Narrative flow across all headings (H1 → H2 → H2 → H2):**
- Read every H1/H2 in sequence. They must form a connected story arc.
- Each H2 should pick up where the previous one left off — hook → problem → solution → proof → action.
- If two adjacent H2s could swap positions without losing meaning, the flow is broken — rewrite.
- The page should make sense if a reader only scans the headings.

**Validation step before delivering output**: List all H2s in order and confirm they read as a narrative. If they don't, revise before saving the file.

### Body Copy Rules (apply to every rewritten paragraph and supporting text block)

**Funnel stage (default to MOFU/BOFU):**
- Audience is almost always product-aware or problem-aware. Skip TOFU-style education ("What is a voice bot?", "Why customer support matters").
- **MOFU** copy: comparison-led, evaluation-led, decision criteria, feature-to-outcome mapping
- **BOFU** copy: proof, specifics, integration depth, pricing transparency, implementation, switching cost
- Calibrate based on the audience selected in Step 1. Enterprise IT = deeper BOFU. SMB pre-sales = MOFU with clear next step.

**Sentence rules:**
- Maximum 20 words per sentence. Break anything longer.
- No em dashes. Use commas, periods, or restructure the sentence.
- Active voice only. No "is being", "was made", "can be achieved".
- One idea per sentence. Don't stack clauses.

**Paragraph rules:**
- Prefer no paragraphs at all. Use bullets, tables, or single-line statements.
- If a paragraph is necessary, hard break after 2–3 sentences.
- Never write a block of 4+ sentences. Split or convert to a list.

**Banned patterns:**
- No "In today's fast-paced world" / "In the age of AI" openers
- No "seamlessly", "robust", "cutting-edge", "best-in-class", "unlock", "leverage" (check KB-07 for full list)
- No throat-clearing phrases ("It's important to note that", "Worth mentioning")
- No statements without specifics. "Reduces costs" → "Cuts agent cost per call by 60 percent"
