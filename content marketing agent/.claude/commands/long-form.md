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

### No dashes
- **Never use em dashes (—) or en dashes (–) anywhere.** Not in the body, headings, meta, FAQ, callouts, or notes. This is a hard brand rule.
- Recast the sentence instead. Use a full stop, a comma, a colon, parentheses, or split into two sentences. Do not reach for a dash as a default connector.

### Mechanics: length and spelling
- **Headings:** every heading (H1, H2, H3) is under 9 words. Eight words is the hard ceiling.
- **Sentences:** every sentence is under 20 words. This applies everywhere, including the Section Answer blocks, TL;DR lines, FAQ answers, meta description, and CTA. Split long sentences in two.
- **US English** spelling and grammar in all generated output: optimize, center, behavior, analyze, organization, prioritize, defense. (These skill instructions are written in British English; the output is not.)

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
  — Keep every heading under 9 words (8 words maximum)
  — Example: "How Cloud Telephony Cuts Call Centre Costs"

INTRODUCTION [150–200 words]
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

  [Body — 250–400 words]
     Evidence, examples, data, and B2B context.
     Include 1 statistic or external citation per section.
     Use bullet lists or numbered steps where appropriate.

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

FAQ BLOCK [3–5 questions] — placed last, after the Primary CTA
  ### [Question in H3]
  [60–100 word direct answer — no conversational filler]

  — Must include at least one contrarian FAQ:
    "When should you NOT use [topic]?" or "Is [topic] right for every business?"
```

---

## AEO Writing Rules

- Every H2 must be a question or a concrete outcome — never a label (not "Section 3: Benefits")
- The Introduction is the first content under H1. Do not add any summary or answer block before it
- Each Section Answer Block (50–80 words) answers the H2 directly — no lead-in sentence
- End every body section with a `> **TL;DR:**` line
- FAQ block uses H3 question headings with 60–100 word answers — no conversational padding. Draw the questions from the real People Also Ask queries and SERP features in the Research Brief wherever available
- For any technical term used for the first time, add a definition block immediately after its first use:
  > **[Term]** — [one-sentence definition]
- At the end of the draft, output a JSON-LD Article schema block with these fields pre-filled from the brief: `headline`, `author`, `datePublished`, `description`, `publisher`. **If the calling workflow explicitly disables schema output**, skip this block and the matching Quality Gate item.

---

## GEO Writing Rules

- Every assertion is written in direct, unambiguous language — avoid: "it could be argued", "some might say", "it's worth noting"
- Every factual claim must be supported by a statistic, named citation, or attributed source. No unsourced generalisations, and per Citation Integrity, no fabricated ones either.
- **Structure around real answer-engine demand.** Use the Google "People Also Ask" questions and SERP features (featured-snippet phrasing, related searches) captured in the Research Brief for the primary keyword. Mirror those exact queries in your H2s and FAQ questions so the post is built to win answer-engine extraction. Use only the questions surfaced during research. Do not invent them.
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
  - Never place an internal link in the Introduction
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

- [ ] Introduction is the first content under H1, with no summary/answer block before it
- [ ] No statistic and no internal link anywhere in the Introduction
- [ ] Every H2 is a question or outcome statement — no label headings
- [ ] Every heading (H1/H2/H3) is under 9 words
- [ ] Every sentence is under 20 words
- [ ] US English spelling and grammar throughout
- [ ] Every body section ends with a `> TL;DR:` line
- [ ] FAQ block has 3–5 questions in H3 format with direct 60–100 word answers
- [ ] FAQ block is placed last, after the Conclusion and the Primary CTA
- [ ] At least one contrarian FAQ included
- [ ] Primary keyword in H1, first 100 words, and at least one H2
- [ ] At least 2 cited statistics or external sources with attribution, all from fetched sources
- [ ] No citation appears in the first 2-3 paragraphs of the post
- [ ] H2s and/or FAQ questions mirror real People Also Ask questions or SERP features from the Research Brief
- [ ] **Citation integrity:** every statistic, figure, study, and quote traces to a real source URL from the Research Brief. Zero invented numbers, sources, or quotes
- [ ] **No em dashes or en dashes anywhere** in draft, headings, meta, FAQ, callouts, or notes
- [ ] **Voice:** written in first person as Acefone, active voice, reads like an expert talking to a peer (not a whitepaper)
- [ ] **Flow:** sections connect into one continuous argument, not a stack of standalone blocks
- [ ] Exactly one CTA, the Primary CTA at the end, matched to the funnel stage. No CTA anywhere in the body
- [ ] Meta elements complete (title tag, meta description, slug)
- [ ] JSON-LD schema block present and populated (skip if the calling workflow disabled schema output)
- [ ] Visual suggestions present after each major section
- [ ] Syndication Recommendation included
