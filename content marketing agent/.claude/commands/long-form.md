# Long-Form Content Skill
**Covers:** SEO blogs, AEO blogs, GEO blogs, off-site blogs

---

## Identity and Scope

This skill writes AEO/SEO/GEO-optimised long-form blog posts for B2B audiences on behalf of Acefone.

**Inputs required before writing begins:**
1. Completed brief (collected by the agent per CLAUDE.md Step 4)
2. Research Brief output from `/deep-research`

If either input is missing or incomplete, halt and ask for it before proceeding.

---

## Voice, Authenticity & Source Integrity

These rules override any structural rule below if they ever conflict. A draft that fails any one of them is not shippable, regardless of how well it scores on SEO/AEO.

### Write as an Acefone subject-matter expert (not a detached observer)
- **First person.** Write as Acefone: "we", "our team", "in our experience", "we've seen customers". Never write about cloud telephony as a neutral encyclopaedia. You are an operator who runs this stack, sharing what you know.
- **Active voice, named actors.** "Our routing engine reroutes the call" — not "the call is rerouted". Say who does what.
- **Sound like a person talking to a peer.** Use the natural cadence of spoken expert language: contractions ("you're", "it's", "we've"), varied sentence length, the occasional short punchy line. Read a paragraph aloud in your head; if it sounds like a whitepaper or a press release, rewrite it.
- **Earn every sentence.** Cut hedging ("it's worth noting", "in today's fast-paced world", "businesses of all sizes"), throat-clearing, and filler transitions ("furthermore", "moreover", "in conclusion").

### Flow, not a stack of blocks
- The piece must read as one continuous argument, not a list of disconnected sections. The AEO extraction blocks (Section Answer, TL;DR) stay tight and factual, but the **body prose around them must connect**: each section should pick up where the last left off and lead into the next. Reference what you just said. Set up what's coming.
- Open body sections with a real thought, not a definition dump. A reader should feel a human walking them through a problem, not a machine populating a template.

### Dashes
- **Never use em dashes (—) anywhere.** Not in the body, headings, meta, FAQ, callouts, or notes. This is a hard brand rule.
- **En dashes (–) are permitted only for numeric ranges** (e.g. "25–35%", "500–600ms"). Never use an en dash as a sentence connector or in prose.
- For any non-range pause, recast the sentence. Use a full stop, a comma, a colon, parentheses, or split into two sentences.

### Mechanics: length and spelling
- **Headings:** every heading (H1, H2, H3) is under 9 words. Eight words is the hard ceiling.   
- **Sentences:** every sentence is under 20 words. This applies everywhere, including the Section Answer blocks, TL;DR lines, FAQ answers, meta description, and CTA. Split long sentences in two.
- **Indian / British English** spelling and grammar in all generated output: optimise, centre, behaviour, analyse, organisation, prioritise, defence, programme, enquiry, colour, favour, licence (noun) / license (verb), fulfil, fulfilment, catalogue. Avoid US spellings (optimize, center, behavior, analyze). Also avoid *whilst, amongst, whom* in web copy — use *while, among, who*.

### Citation integrity (zero fabrication)
- **Every statistic, percentage, dated figure, study reference, and quote must trace to a real source URL that was actually fetched into the Research Brief.** No exceptions.
- **Do not invent** statistics, study names, report titles, author names, quotes, or URLs. If you cannot point to a fetched source for a number, you may not publish that number.
- If a useful point has no source, you have two honest options: (1) drop it, or (2) frame it explicitly as Acefone's own operational experience ("In our deployments we typically see...") with **no fabricated figure attached**.
- **Expert quotes must be real** and attributed to a real, named source from the Research Brief. If the research surfaced no genuine quote, replace the quote requirement with an attributed data point from a fetched source. Never manufacture a quote.
- When you cite, name the source inline (e.g. "according to [Source]") and keep the URL with it so the editor can verify.
- **Citation placement:** keep the opening clean. The first 2-3 paragraphs of the post carry no citations at all. Introduce your first citation only after that, once the argument is set up.

---

## Brief Intake Checklist

Confirm all fields are answered before moving to writing:

- [ ] Primary keyword
- [ ] 3–5 secondary / semantic keywords
- [ ] Target word count (default if unspecified: **1,550–1,800 words** for long-form; **500–600 words** for web copy)
- [ ] Target audience — role, industry, pain point
- [ ] Blog type: SEO-primary / AEO-primary / GEO-primary / hybrid
- [ ] Content angle / unique hook
- [ ] Funnel stage: TOFU / MOFU / BOFU
- [ ] CTA aligned to funnel stage
- [ ] Tone: formal / conversational / authoritative / thought-provoking
- [ ] Author byline (required for EEAT signals — name + title)
- [ ] Competitor URLs or angles to avoid (if any)

---

## AEO Writing Rules

- Every H2 must be a question or a concrete outcome — never a label (not "Section 3: Benefits")
- The Introduction is the first content under H1. Do not add any summary or answer block before it
- Each Section Answer Block (50–80 words) answers the H2 directly — no lead-in sentence
- End every body section with a `> **TL;DR:**` line
- FAQ block uses H3 question headings with 40–60 word answers (2–3 sentences) — no conversational padding. Draw the questions from the real People Also Ask queries and SERP features in the Research Brief wherever available
- For any technical term used for the first time, add a definition block immediately after its first use:
  > **[Term]** — [one-sentence definition]

---

## GEO Writing Rules

- Every assertion is written in direct, unambiguous language — avoid: "it could be argued", "some might say", "it's worth noting"
- Every factual claim must be supported by a statistic, named citation, or attributed source. No unsourced generalisations, and per Citation Integrity, no fabricated ones either.
- **Structure around real answer-engine demand.** Use the Google "People Also Ask" questions and SERP features (featured-snippet phrasing, related searches) captured in the Research Brief for the primary keyword. Mirror those exact queries in your H2s and FAQ questions so the post is built to win answer-engine extraction. Use only the questions surfaced during research. Do not invent them.
- Build semantic richness: identify 3–5 related concepts from the Research Brief for the primary keyword and weave them naturally through body sections — do not force them
- Authoritative tone throughout — state Acefone's perspective confidently, not tentatively

---

## SEO Writing Rules

- **Keyword placement:**
  - Primary keyword: H1, first 100 words of Introduction, at least one H2, meta description
  - Secondary keywords: distributed naturally — max 2-3 occurrences per 200 words
- **Internal linking:** recommend 1 internal link every 200-300 words using anchor text that matches the target page's topic
  - Mark in draft as: `[INTERNAL LINK: topic/page description]`
  - Never place an internal link in the Introduction
- **External linking:** cite at least 2 authoritative external sources (government body, academic paper, or tier-1 industry publication)
- **Visual suggestions:** after each major section, add a bracketed callout for the design team:
  - `[VISUAL SUGGESTION: type — description | Alt text: "..."]`
- **Meta elements** (delivered at end of draft, see Output Format):
  - Title tag: 55–60 characters, primary keyword present
  - Meta description: 150–160 characters, primary keyword + CTA hook
  - URL slug: lowercase, hyphens, primary keyword, max 6 words

---

## Outline Approval Gate

Before drafting the full blog, produce a structured outline and get explicit user approval. The outline itself must already conform to the AEO, GEO, and SEO rules above — H2s phrased as PAA-mirroring questions or outcomes, primary keyword mapped to H1 / intro / at least one H2, PAA-sourced FAQs, semantic concepts and citations planned per section. Do not begin writing the draft until the user approves the outline.

**Outline must include:**
- Proposed H1 title (under 9 words, primary keyword in first 60 chars)
- Introduction angle: the hook, pain point, and promise in 1-2 lines each
- Each proposed H2 (as a question or outcome statement, under 9 words) with a one-line description of what that section will cover, the key stat or source it will lean on, and a planned word count (must be under 300 words; if the topic needs more, split it into stub sub-sections at the outline stage itself, each with its own H3 and its own word budget)
- B2B Application section: the specific use case, industry, or workflow angle
- Conclusion direction: the 3 takeaways the post will land on
- Primary CTA: funnel stage and the exact CTA action
- FAQ list: the 5-6 questions (drawn from PAA / SERP features in the Research Brief), with the contrarian FAQ flagged

**Then:**
1. Present the outline to the user.
2. Ask explicitly: *"Approve this outline as-is, or share edits before I draft?"*
3. Apply any requested changes and re-confirm if the structure shifts materially.
4. Only after explicit approval, proceed to the full draft.

---

## Document Structure

Every blog post must follow this structure in this order. Do not deviate without a reason from the brief.

**Enforce word budgets while drafting, not after.** Before writing each section, check the planned word count from the approved outline. Stop at the cap. If mid-draft you realise a section needs more than 300 words, pause, split it into stub sub-sections (each with its own H3 and its own budget), and only then continue. Never let a section run past 300 words and try to fix it at the Quality Gate.

```
H1 TITLE
  — Primary keyword within first 60 characters
  — Phrased as a benefit or outcome statement, not a label
  — Keep every heading under 9 words (8 words maximum)
  — Example: "How Cloud Telephony Cuts Call Centre Costs"

INTRODUCTION [100–120 words]
  — Hook (a question or a counterintuitive claim, NOT a statistic)
  — Pain point the target audience faces
  — Promise: what the reader will know or be able to do after reading
  — Primary keyword appears naturally within first 100 words
  — Do NOT use any statistic or any internal link in the Introduction

BODY SECTIONS [3–5 sections]
  Each section follows this exact pattern:

  ## H2 — Phrased as a question or outcome statement (under 9 words)
     Example: "Why Do Call Centres Lose Peak-Hour Revenue?"

  [Section Answer Block — 50–80 words]
     Direct answer to the H2 question. No preamble.

  [Body — under 300 words]
     Evidence, examples, data, and B2B context.
     Include 1 statistic or external citation per section.
     Use bullet lists or numbered steps where appropriate.
     If the section demands more than 300 words, split it into
     stub sub-sections (each with its own H3 and its own body),
     rather than letting any single section run past 300 words.

  > **TL;DR:** [One-sentence summary of this section — for AI extraction]

B2B APPLICATION SECTION
  ## How [Topic] Applies to [Industry / Audience]
  — Concrete use case, workflow, or implementation example
  — Reference Acefone product context or KB data where available
  — Do NOT place any CTA inside this section or anywhere in the body

CONCLUSION [100–150 words]
  — Summarise the 3 most important takeaways
  — Reinforce the unique angle from the brief
  — Transition naturally into the primary CTA

PRIMARY CTA BLOCK
  — The post's ONLY CTA. Matched to funnel stage (see Conversion Rules)
  — One paragraph max

FAQ BLOCK [5-6 questions] — placed last, after the Primary CTA
  ### [Question in H3]
  [40–60 word direct answer, 2–3 sentences — no conversational filler]

  — Must include at least one contrarian FAQ:
    "When should you NOT use [topic]?" or "Is [topic] right for every business?"
```

---

## B2B Conversion Rules

Map the CTA to the funnel stage specified in the brief:

| Funnel Stage | CTA Type | Example |
|---|---|---|
| TOFU | Low-friction education | "Download the free guide", "Read the related post" |
| MOFU | Evaluation asset | "Get the comparison checklist", "See how [Customer] did it" |
| BOFU | Direct sales action | "Book a demo", "Talk to our team", "Start free trial" |

- Every post has **exactly one CTA**: a single **Primary CTA block** at the end of the post, one short paragraph.
- **Do not place any CTA in the body of the post.** No inline CTA after the B2B Application section, no mid-article prompts. The reader meets a CTA only once, at the end.
- CTA copy must reference the reader's specific pain point. Never use generic copy like "Learn more" or "Click here".
- Example of strong CTA: *"See how Acefone helped [industry] teams cut call resolution time by 30%. Book a 20-minute walkthrough."*

---

## Output Format

Deliver the complete post in this exact order:

---

### Blog Post Draft

[Full post: H1, Introduction, Body Sections, B2B Application, Conclusion, Primary CTA, then the FAQ block last]

---

### Meta Elements
- **Title tag:** [55–60 chars]
- **Meta description:** [150–160 chars]
- **URL slug:** [lowercase-hyphenated-slug]
- **Primary keyword:** [keyword]
- **Secondary keywords:** [comma-separated list]

---

### Visual Suggestions
[Numbered list — each with type, description, and recommended alt text]

---

### Internal Links to Resolve
[List of all `[INTERNAL LINK: ...]` placeholders from the draft]

---

### Editor Notes
[Flags for: fact-checking needs, missing KB data, brand voice review, claims requiring legal/product sign-off]

---

## Quality Gate

Run this self-check before delivering the draft. Every item must pass. If any item fails, fix in place — do not present the draft.

### Structure & Long-Form Craft
- [ ] Introduction is the first content under H1, with no summary/answer block before it
- [ ] Introduction is 100–120 words; opens with a hook (not a statistic); states pain and promise
- [ ] Every body section is under 300 words; longer material is split into stub sub-sections with their own H3
- [ ] Every body section has: H2 → Section Answer Block (50–80w) → Body → `> TL;DR:` line, in that order
- [ ] B2B Application section presents a concrete use case, workflow, or implementation example
- [ ] Conclusion is 100–150 words and lands on exactly 3 takeaways
- [ ] FAQ block has 5–6 H3 questions with 40–60 word direct answers (2–3 sentences), placed last after the Primary CTA
- [ ] At least one contrarian FAQ ("when NOT to use", "is this right for everyone")
- [ ] Total word count matches the target from the brief (default 1,550–1,800 for long-form; 500–600 for web copy)
- [ ] Every H2 is a question or outcome statement — no label headings
- [ ] Every heading (H1/H2/H3) is under 9 words
- [ ] Every sentence is under 20 words
- [ ] No em dashes (—) anywhere; en dashes (–) used only for numeric ranges (25–35%), never as sentence connectors

### Voice, Language & Craft
- [ ] Written in first person as Acefone ("we", "our team"), active voice, named actors
- [ ] Reads like an expert talking to a peer — contractions, varied cadence, not whitepaper prose
- [ ] No hedging or filler ("it's worth noting", "in today's fast-paced world", "furthermore", "moreover")
- [ ] No em dashes anywhere; en dashes limited to numeric ranges only
- [ ] Indian / British English spelling and grammar throughout (optimise, centre, behaviour); no US spellings and no *whilst / amongst / whom*
- [ ] Sections connect into one continuous argument — each picks up from the previous and sets up the next

### AEO (Answer Engine Optimization)
- [ ] Each Section Answer Block (50–80w) answers its H2 directly, with no preamble
- [ ] Every body section ends with a `> **TL;DR:**` line
- [ ] Every technical term used for the first time has a `> **Term** — definition` block immediately after
- [ ] FAQ questions and at least one H2 mirror real People Also Ask queries or SERP features from the Research Brief (no invented questions)
- [ ] FAQ answers are direct (40–60 words, 2–3 sentences) with no conversational padding

### GEO (Generative Engine Optimization)
- [ ] Every factual claim is supported by a statistic, named citation, or attributed source
- [ ] Language is direct and unambiguous — no "it could be argued", "some might say"
- [ ] 3–5 related semantic concepts from the Research Brief are woven naturally through the body
- [ ] Authoritative tone — Acefone's perspective stated confidently, not tentatively

### AIO (AI / LLM Optimization — ChatGPT, Perplexity, Claude, Gemini)
- [ ] Key facts are written as self-contained sentences an LLM can extract without surrounding context
- [ ] Acefone product and feature names are used consistently across the post (same spelling, capitalisation, phrasing each time)
- [ ] Entities (products, industries, roles, integrations) are named clearly on first mention, not just referred to by pronoun or category
- [ ] Numeric answers (percentages, timeframes, prices) are stated with the unit and the source in the same sentence
- [ ] Claims are attributable to a named actor (Acefone, a cited study, a named expert) — not floating assertions

### SEO
- [ ] Primary keyword appears in: H1, first 100 words of Introduction, at least one H2, and the meta description
- [ ] Secondary keywords are distributed naturally, max 2–3 occurrences per 200 words, no stuffing
- [ ] At least 2 cited external sources from tier-1 outlets (government, academic, or top industry publication), each with attribution and URL
- [ ] 1 internal link recommended every 200–300 words, marked as `[INTERNAL LINK: ...]`, none in the Introduction
- [ ] Author byline present with name + title (EEAT signal)
- [ ] Title tag 55–60 chars with primary keyword; meta description 150–160 chars with keyword + CTA hook; URL slug lowercase-hyphenated, ≤6 words, contains primary keyword

### Citation Integrity
- [ ] Every statistic, figure, study, and quote traces to a real source URL from the Research Brief — zero invented numbers, sources, or quotes
- [ ] Every quote is real and attributed to a named person from a fetched source
- [ ] No citation appears in the first 2–3 paragraphs of the post

### User Experience & Readability
- [ ] Paragraphs are short (max 3–4 sentences) for scannability
- [ ] Bullet lists or numbered steps used where content is enumerable
- [ ] Above-the-fold: H1 + Introduction make the promise and payoff clear without scrolling
- [ ] Visual suggestions present after each major section, each with type, description, and alt text
- [ ] Alt text on every visual suggestion is descriptive, not decorative filler

### Conversion
- [ ] Exactly one CTA — the Primary CTA at the end — matched to the funnel stage from the brief
- [ ] No CTA anywhere in the body of the post
- [ ] CTA copy references the reader's specific pain point; no generic "Learn more" or "Click here"

### Deliverables
- [ ] Meta elements block complete (title tag, meta description, slug, primary and secondary keywords)
- [ ] Visual Suggestions list included
- [ ] Internal Links to Resolve list included
- [ ] Editor Notes cover fact-checking, missing KB data, brand voice, and any legal/product sign-off flags
