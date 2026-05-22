# Brand Voice Skill

## Identity and Scope

Skill responsible for extracting Acefone's brand voice from the Notion KB and enforcing it across every collateral draft. Source of truth: **KB-01 — Brand Identity** in the Marketing KB.

Invoked twice per request:
- **Step 4 of CLAUDE.md** — extract a compact voice card, pass to drafter
- **Pre-render checkpoint** — validate the drafted body against banned words, headline rules, deliverable-specific formatting

If KB-01 fetch fails, halt and surface the error. Do not proceed with a stale or guessed voice card.

---

## How to Invoke

### Initial extraction (Step 4)
1. Run `notion-fetch` on KB-01: `https://www.notion.so/d506330059da83e4aafa013e3b7a9503`
2. Parse the page into the Voice Card format below.
3. Cache the Voice Card in working memory for the rest of the session.
4. Surface a one-line confirmation in caveman lite: `Voice card loaded. Tone: outcome-led. Banned: 20+ words. Headline cap: flyer ≤10 / brochure ≤12 / slide ≤8.`

### Validation (pre-render)
After the drafter outputs Markdown, run four checks:
1. **Banned word scan** — case-insensitive substring search across body and headlines
2. **Headline check** — outcome-led, numeric where possible, length within deliverable-specific cap, no question headlines without implied answer
3. **Number formatting check** — numerals not words, `+` for floors, en-dash for ranges, `typically`/`up to` prefix on estimates
4. **Deliverable-specific check** — apply the matching block from the Voice Card

If any check fails, halt the render step. Output a violation list and ask the user to fix or override.

### Force re-extraction
`/brand-voice refresh` discards the cached card and re-fetches KB-01.

---

## Voice Card Format

```
ACEFONE VOICE CARD

TONE
Professional, direct, outcome-led.
Voice of the senior CX leader: confident, precise, business-results-focused.
No hype, no hedging, no lectures. Every sentence earns its place.

THREE PRINCIPLES
1. Authoritative — no caveats on core claims
2. Outcome-first — lead with what the client gets, not what we do
3. Human — real language, real scenarios. Not marcom filler.

BANNED WORDS (zero tolerance)
Leverage, Synergy, Robust, Cutting-edge, Game-changer, World-class, Best-in-class,
Seamless, Innovative, Innovation, Empower, Transform, Revolutionise / Revolutionize,
Future-proof, State-of-the-art, Next-generation, Scalable (unless followed by actual numbers),
Holistic, Ecosystem, End-to-end, Streamline.
Bare "Solution" / "Platform" — context-flag (OK after a product name, flagged when standalone).

HEADLINE RULES
1. Lead with the result, not the feature
2. Use numbers wherever possible — "Cut Inbound Volume by 60%" beats "Reduce Call Load"
3. Match the vertical:
   - BFSI: compliance, scale, risk, NPA, KYC, collections
   - Healthcare: patient, appointment, discharge, OPD, TPA
   - Logistics: delivery, SLA, DIFOT, last-mile, fulfilment
   - E-commerce: COD, RTO, festive spike, AHT
   - BPO: RFP, deflection rate, client demo
4. Length caps:
   - Slide title ≤ 8 words
   - Brochure front panel ≤ 12 words
   - Flyer hero ≤ 10 words
   - Ebook cover ≤ 12 words
5. No question headlines unless the answer is implied
   ("Your agents should not be handling these calls" beats "Are your agents handling every call?")

NUMBER & METRIC FORMATTING
- Use numerals: 3x, 60%, 800ms, 15,000+
- "+" suffix for floors: 15,000+ brands, 2.5B+ engagements
- En dash for ranges: 25–35% AHT reduction
- Prefix estimates with "typically" or "up to" — never label estimates as facts
- Currency always in ₹ for India-targeted content

PRODUCT NAMES (always capitalised, never abbreviated)
Voice Bot · Broadcast · API Connect · Interactions Hub · Contact Center Studio ·
Post Call Analytics · IVR · Click to Call · AceX (platform) · AI Evaluators (capability)

SALES ENABLEMENT REGISTER

DO:
- Name the pain before the product. Clients buy relief, not features.
- Use client-industry language ("collections" not "lending"; "OPD" not "outpatient")
- Anchor every claim to a metric. Use "typically" qualifier for ranges.
- Present tense for capabilities ("Voice Bot handles"), past tense for results ("reduced agent load by 40%")
- Show, don't tell ("100% of calls QA-scored" beats "comprehensive quality monitoring")

DO NOT:
- Open with Acefone — open with the client's world
- List features without linking to a client outcome
- Use passive voice on result claims ("clients cut costs by 60%" not "60% reduction was achieved")
- Overload with product names — max 3 per deliverable unless it's a full deck
- Make regulatory claims without citing the framework
  ("RBI call recording mandate compliant" not "fully compliant")

DELIVERABLE-SPECIFIC RULES

Slide decks:
- Slide titles: sentence case, max 8 words, no period
- Body copy: fragments acceptable; max 12 words per bullet; max 4 bullets per slide
- All product names capitalised (Voice Bot, Interactions Hub, etc.)

Case studies:
- Section headers: ALL CAPS (CHALLENGE, SOLUTION, RESULTS)
- Executive quote: italicised, attributed to role (not name, unless approved)
- Results bullets: lead with metric, then context
  Example: "60% reduction in agent-handled inbound — now managed by Voice Bot"

Brochures:
- Panel labels: ALL CAPS (FRONT PANEL, BACK PANEL, FEATURE)
- Feature/benefit pairs: Feature in bold, benefit on next line in normal weight
- CTA: imperative verb + outcome ("Book a 30-minute proof-of-concept call")

Flyers (1-page sales one-pager):
- Hero statement: 8–12 words, bold, outcome-first
- Differentiators: parallel structure mandatory (all nouns or all verb phrases)
- Max 250 words body (excluding hero and footer)

Flyers (2-page product flyer):
- Page 1 hero: ≤ 10 words, outcome-led, never starts with Acefone or AceX
- Page 1 grid: exactly 4 capability/proof blocks
- Page 2: 3 use-cases (parallel grammar) + 6 infrastructure features + CTA block
- Total body: 250–400 words

Ebooks:
- Cover title: ≤ 12 words, outcome-led
- Persistent footer per body page: [Ebook Title] [Page #]
- Numbered chapters (02, 03, ...); intro unnumbered
- "By the end you'll know" promise list right after intro hook
- Closing chapter ends with soft pitch (1 sentence); hard CTA on back cover only
```

---

## Banned Word Scan (Implementation)

Search case-insensitively for these substrings/words across the entire draft body, including headlines and image alt text:

```
leverage, synergy, robust, cutting-edge, cutting edge, game-changer, game changer,
world-class, world class, best-in-class, best in class, seamless, innovative, innovation,
empower, transform, revolutionise, revolutionize, future-proof, future proof,
state-of-the-art, state of the art, next-generation, next generation, scalable,
holistic, ecosystem, end-to-end, end to end, streamline, streamlining
```

For `scalable`, only flag if not followed within 10 words by a numeric scale claim (`scalable to 5M calls/day` = OK; `scalable infrastructure` = flag).

For bare `solution` / `platform`, only flag when not preceded by a product name within 5 words (`Voice Bot platform` = OK; `our platform` or `the solution` = flag).

---

## Violation Output Format

```
BRAND VOICE VIOLATIONS

Banned words (3):
- Line 12: "leverage AceX to..." → use "use" or a specific verb
- Line 28: "robust performance" → state the actual perf number
- Line 41: "seamless integration" → "in a single workflow" / "without switching screens"

Headline (1):
- Page 1 hero "Are Your Agents Overwhelmed?" — question without implied answer
  → Rewrite: "Your Agents Shouldn't Be Handling These Calls"

Number formatting (1):
- Line 18: "around 500 milliseconds" → "~500ms" or "typically 500ms"

Deliverable-specific (1):
- Flyer hero is 13 words — cap is 10. Tighten.

Total: 6 violations.
Fix all before render? [y/N]
```

---

## After Render

If any draft was rendered with an override (user said no to fixing violations), log the violations as a Notion comment on the editorial DB entry with the marketing lead tagged. The render still proceeds — the comment is for downstream human review.
