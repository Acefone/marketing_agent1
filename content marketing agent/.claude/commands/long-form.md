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

## Brief Intake Checklist

Confirm all fields are answered before moving to writing:

- [ ] Primary keyword
- [ ] 3–5 secondary / semantic keywords
- [ ] Target word count (default if unspecified: **1,800–2,500 words**)
- [ ] Target audience — role, industry, pain point
- [ ] Blog type: SEO-primary / AEO-primary / GEO-primary / hybrid
- [ ] Content angle / unique hook
- [ ] Funnel stage: TOFU / MOFU / BOFU
- [ ] CTA aligned to funnel stage
- [ ] Tone: formal / conversational / authoritative / thought-provoking
- [ ] Author byline (required for EEAT signals — name + title)
- [ ] Competitor URLs or angles to avoid (if any)

---

## Document Structure

Every blog post must follow this structure in this order. Do not deviate without a reason from the brief.

```
H1 TITLE
  — Primary keyword within first 60 characters
  — Phrased as a benefit or outcome statement, not a label
  — Example: "How Cloud Telephony Cuts Call Centre Costs by 35%"

EXECUTIVE ANSWER [90–120 words]
  — Appears immediately below the H1, before any other text
  — Directly answers the blog's core topic/question
  — Written for AI extraction: no fluff, no preamble, no "in this article"
  — This block is the GEO/AEO citation target

INTRODUCTION [150–200 words]
  — Hook (stat, question, or counterintuitive claim)
  — Pain point the target audience faces
  — Promise: what the reader will know or be able to do after reading
  — Primary keyword appears naturally within first 100 words

BODY SECTIONS [3–5 sections]
  Each section follows this exact pattern:

  ## H2 — Phrased as a question or outcome statement
     Example: "Why Do Call Centres Lose Revenue During Peak Hours?"

  [Section Answer Block — 50–80 words]
     Direct answer to the H2 question. No preamble.

  [Body — 250–400 words]
     Evidence, examples, data, and B2B context.
     Include 1 statistic or external citation per section.
     Use bullet lists or numbered steps where appropriate.

  > **TL;DR:** [One-sentence summary of this section — for AI extraction]

B2B APPLICATION SECTION
  ## How [Topic] Applies to [Industry / Audience]
  — Concrete use case, workflow, or implementation example
  — Reference Acefone product context or KB data where available
  — Inline CTA placed at the end of this section (see Conversion Rules)

FAQ BLOCK [3–5 questions]
  ### [Question in H3]
  [60–100 word direct answer — no conversational filler]

  — Must include at least one contrarian FAQ:
    "When should you NOT use [topic]?" or "Is [topic] right for every business?"

CONCLUSION [100–150 words]
  — Summarise the 3 most important takeaways
  — Reinforce the unique angle from the brief
  — Transition naturally into the primary CTA

PRIMARY CTA BLOCK
  — Matched to funnel stage (see Conversion Rules)
  — One paragraph max
```

---

## AEO Writing Rules

- Every H2 must be a question or a concrete outcome — never a label (not "Section 3: Benefits")
- The Executive Answer block (90–120 words) must be the first content under H1 — nothing before it
- Each Section Answer Block (50–80 words) answers the H2 directly — no lead-in sentence
- End every body section with a `> **TL;DR:**` line
- FAQ block uses H3 question headings with 60–100 word answers — no conversational padding
- For any technical term used for the first time, add a definition block immediately after its first use:
  > **[Term]** — [one-sentence definition]
- At the end of the draft, output a JSON-LD Article schema block with these fields pre-filled from the brief: `headline`, `author`, `datePublished`, `description`, `publisher`

---

## GEO Writing Rules

- Every assertion is written in direct, unambiguous language — avoid: "it could be argued", "some might say", "it's worth noting"
- Every factual claim must be supported by a statistic, named citation, or attributed source — no unsourced generalisations
- Include at least **2 expert quotes or attributed statements** (sourced from the Research Brief or KB)
- Build semantic richness: identify 3–5 related concepts from the Research Brief for the primary keyword and weave them naturally through body sections — do not force them
- Authoritative tone throughout — state Acefone's perspective confidently, not tentatively
- After the Editor Notes section, include a **Syndication Recommendation**: list 2–3 platforms where this post should be published or cross-posted to build multi-platform LLM signal (e.g. LinkedIn Article, Medium, industry forum)

---

## SEO Writing Rules

- **Keyword placement:**
  - Primary keyword: H1, first 100 words of Introduction, at least one H2, meta description
  - Secondary keywords: distributed naturally — max 1–2 occurrences per 500 words
- **Internal linking:** recommend 2–3 internal links using anchor text that matches the target page's topic
  - Mark in draft as: `[INTERNAL LINK: topic/page description]`
- **External linking:** cite at least 2 authoritative external sources (government body, academic paper, or tier-1 industry publication)
- **Visual suggestions:** after each major section, add a bracketed callout for the design team:
  - `[VISUAL SUGGESTION: type — description | Alt text: "..."]`
- **Meta elements** (delivered at end of draft, see Output Format):
  - Title tag: 55–60 characters, primary keyword present
  - Meta description: 150–160 characters, primary keyword + CTA hook
  - URL slug: lowercase, hyphens, primary keyword, max 6 words

---

## B2B Conversion Rules

Map the CTA to the funnel stage specified in the brief:

| Funnel Stage | CTA Type | Example |
|---|---|---|
| TOFU | Low-friction education | "Download the free guide", "Read the related post" |
| MOFU | Evaluation asset | "Get the comparison checklist", "See how [Customer] did it" |
| BOFU | Direct sales action | "Book a demo", "Talk to our team", "Start free trial" |

- Every post has **exactly two CTAs**:
  1. **Inline CTA** — placed after the B2B Application section, 1–2 sentences
  2. **Primary CTA block** — at the end of the post, 1 short paragraph
- CTA copy must reference the reader's specific pain point — never use generic copy like "Learn more" or "Click here"
- Example of strong CTA: *"See how Acefone helped [industry] teams cut call resolution time by 30% — book a 20-minute walkthrough"*

---

## Output Format

Deliver the complete post in this exact order:

---

### Blog Post Draft

[Full post — H1 through Primary CTA block]

---

### Meta Elements
- **Title tag:** [55–60 chars]
- **Meta description:** [150–160 chars]
- **URL slug:** [lowercase-hyphenated-slug]
- **Primary keyword:** [keyword]
- **Secondary keywords:** [comma-separated list]

---

### Schema Markup (JSON-LD)
```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "",
  "author": {
    "@type": "Person",
    "name": "",
    "jobTitle": ""
  },
  "publisher": {
    "@type": "Organization",
    "name": "Acefone"
  },
  "datePublished": "",
  "description": ""
}
```

---

### Visual Suggestions
[Numbered list — each with type, description, and recommended alt text]

---

### Internal Links to Resolve
[List of all `[INTERNAL LINK: ...]` placeholders from the draft]

---

### Syndication Recommendation
[2–3 platforms for cross-posting to build multi-platform LLM signal]

---

### Editor Notes
[Flags for: fact-checking needs, missing KB data, brand voice review, claims requiring legal/product sign-off]

---

## Quality Gate

Run this self-check before delivering the draft. Do not present output until all items pass:

- [ ] Executive Answer block is present and 90–120 words
- [ ] Executive Answer appears immediately below H1 with no text before it
- [ ] Every H2 is a question or outcome statement — no label headings
- [ ] Every body section ends with a `> TL;DR:` line
- [ ] FAQ block has 3–5 questions in H3 format with direct 60–100 word answers
- [ ] At least one contrarian FAQ included
- [ ] Primary keyword in H1, first 100 words, and at least one H2
- [ ] At least 2 cited statistics or external sources with attribution
- [ ] At least 2 expert quotes or attributed statements
- [ ] CTA type matches the funnel stage from the brief
- [ ] Inline CTA placed after B2B Application section
- [ ] Meta elements complete (title tag, meta description, slug)
- [ ] JSON-LD schema block present and populated
- [ ] Visual suggestions present after each major section
- [ ] Syndication Recommendation included
