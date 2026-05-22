# Flyer Drafter Skill

## Role

Single-stop drafter for **both** AceX flyer variants. Consumes the brief + voice card + journey card; routes to the right variant via the KB-06 Part 13.3 decision tree; emits structured Markdown + `[DESIGN INTENT]` block for `/gamma-render`.

Authoritative templates: **KB-06 Part 13.1 (2-page product flyer)** and **Part 13.2 (1-page sales one-pager)**.

---

## When Invoked

Step 9 of the orchestrator workflow, after brief + journey stage are locked (no research step for flyers by default).

You receive: brief, voice card, journey card.

---

## Workflow

### Step 1 — Variant routing (KB-06 Part 13.3 decision tree)

Ask the brief intake question first:

> *"Is this flyer about a **single product** (whole feature set, multi-ICP) or a **single use case** (one ICP, pinned proof)?"*

- **Single product** → **Variant 13.1 — 2-page product flyer**
- **Single use case** → **Variant 13.2 — 1-page sales one-pager**

If ambiguous, also check funnel context:
- Event handout (TOFU touch) → 13.2 with awareness-stage CTA
- Sales hand-off / post-discovery → 13.1 (product) or 13.2 (use case), with intent-stage CTA

Confirm the variant choice with the user before continuing intake.

---

### Step 2A — Brief intake for Variant 13.1 (2-page product flyer)

Ask in a single message:

1. **The single product** — Voice Bot OR Broadcast OR Contact Center Studio OR API Connect OR Interactions Hub OR Post Call Analytics. One only.
2. **Outcome the product delivers** — 1 sentence in client language (will drive the hero).
3. **The hero metric** — the single most differentiating number (latency, uptime, throughput, cost). One only.
4. **3 capability/proof blocks for the Page 1 strip** — name + 2–3 line description each (e.g. uptime, native architecture trait, scalability claim).
5. **3 use cases for Page 2 left column** — name + 2–3 line description each. Parallel grammar across all 3.
6. **6 infrastructure features for Page 2 right column** — pick from AceX infrastructure traits (native SIP, direct PSTN, faster deployment, reliable call quality, lower failure rates, enterprise scale, India compliance, no Twilio, single invoice). Each: name + 1-line description.
7. **CTA** — confirm Journey Card CTA or override.

### Step 2B — Brief intake for Variant 13.2 (1-page sales one-pager)

Ask in a single message:

1. **The single use case + ICP** — e.g. "DPD 1–30 collections for NBFC ops heads".
2. **Core problem statement** — 2 sentences max 40 words, in their language.
3. **Acefone answer** — 1 sentence max 25 words.
4. **3 key differentiators** — bold name + 1-line elaboration each. Parallel structure mandatory.
5. **2 proof points** — metric-first format ("60% reduction in COD call cost"). Real numbers or "typically" / "up to" ranges.
6. **CTA** — confirm Journey Card CTA or override.

Confirm intake in caveman lite: *"Got it — [variant], [product/use case]. Drafting."*

---

### Step 3A — Draft Variant 13.1 (2-page product flyer)

**Page 1**

```markdown
[BRAND BAR — Acefone logo top-left, wave accent top-right]

# [HERO HEADLINE — 3-line stack, oversized, outcome-led, ≤ 10 words]

[Subhead — 3 lines, normal weight, names 2–3 product attributes that produce the outcome]

[Centre visual — described in design intent]
[Hero metric callout — one number, near the visual]

[3-column proof strip at bottom]

**[Stat or Capability 1]**
[2–3 line benefit description]

**[Capability 2]**
[2–3 line benefit description]

**[Capability 3]**
[2–3 line benefit description]
```

**Page 2**

```markdown
[BRAND BAR — repeat]

[LEFT COLUMN — blue panel]

EYEBROW: [small-caps] DESIGNED FOR [audience descriptor]
# [Use-case column headline — 2 lines, oversized]

→ **[Use case 1]** — [2–3 line description]
→ **[Use case 2]** — [2–3 line description]
→ **[Use case 3]** — [2–3 line description]

[RIGHT COLUMN — white]

EYEBROW: [small-caps] BUILT ON ACEFONE'S
# [Infrastructure column headline — 2 lines, oversized]

→ **[Feature 1]** — [1-line description]
→ **[Feature 2]** — [1-line description]
→ **[Feature 3]** — [1-line description]
→ **[Feature 4]** — [1-line description]
→ **[Feature 5]** — [1-line description]
→ **[Feature 6]** — [1-line description]

[BOTTOM ROW]

[LEFT — oversized bold CTA, 6–8 words, imperative verb]
[from Journey Card — e.g. "GET IN TOUCH WITH US & SEE IT LIVE"]

[RIGHT — contact strip in this fixed order]
📧 MAIL TO   reachus@acefone.com
📞 CALL US   1800-121-7777
🌐 WEBSITE   acefone.com
```

### Step 3B — Draft Variant 13.2 (1-page sales one-pager)

```markdown
# [HERO STATEMENT — 8–12 words, bold, outcome-first, never starts with Acefone/AceX]

**Problem**
[2 sentences, ≤ 40 words. In their language.]

**Acefone answer**
[1 sentence, ≤ 25 words.]

**Why AceX**

→ **[Differentiator 1]** — [1-line elaboration]
→ **[Differentiator 2]** — [1-line elaboration]
→ **[Differentiator 3]** — [1-line elaboration]

**Proof**

- [Metric-first proof point 1: "60% reduction in cost-per-call in 30 days"]
- [Metric-first proof point 2: "Typically 48 hours to first live agent"]

[CTA — max 15 words, from Journey Card]

---
Acefone | 15,000+ Enterprise Brands | ISO 27001 | RBI & TRAI Compliant | 99.5% Uptime
```

---

### Step 4 — Apply brand voice rules (KB-01)

- Banned-word scan (KB-01 Voice Card)
- Hero / hero statement: outcome-led, never starts with "Acefone" or "AceX"
- 13.1 hero cap: 10 words. 13.2 hero cap: 12 words.
- Parallel structure across differentiators / use cases / features
- Numbers per KB-01 formatting
- Product names exact and capitalised

### Step 5 — Heading criteria (dynamic per audience)

Heading patterns by block:

- **13.1 hero (Page 1)**: outcome + product attribute pairing — *"AI Voice Agents That Actually Sound Natural"*, *"Collections Calls Answered in Under 800ms"*, *"COD Confirmation Without Adding Agents"*. Lead with the differentiating product attribute or outcome.
- **13.1 use case headings (Page 2 left)**: 2–3 word noun phrases, parallel — *"Lead Qualification" / "Appointment Booking" / "Collections & Reminders"*.
- **13.1 feature headings (Page 2 right)**: 2–4 word noun phrases, parallel — *"Native SIP Architecture" / "Direct PSTN Integration" / "Faster Deployment"*.
- **13.2 hero (1-pager)**: imperative outcome — *"Cut COD Call Costs by 60% in 30 Days"*, *"Stay DPDPA Compliant on Every Collections Call"*. Lead with the verb + result.
- **13.2 differentiators**: noun phrases, all same length, all same form — *"India-native compliance" / "Hinglish call handling" / "Pre-deployment AI Evaluators"*.

Match the heading register to the audience seniority signal in the brief — C-suite = tighter and metric-led; ops manager = action-led and ICP-vocabulary-rich.

### Step 6 — Emit the `[DESIGN INTENT]` block

For 13.1:

```markdown
[DESIGN INTENT]

format:        document
dimensions:    a4
text_mode:     preserve
audience:      [from brief]
language:      en-in
variant:       13.1 — 2-page product flyer

theme_hint:    modern professional, navy + blue accent + green highlight

page_1:
  - Hero: 3-line oversized stack, navy primary + blue accent on key phrase
  - Subhead: 3 lines normal weight
  - Centre visual: [describe — match product type: waveform circle for Voice Bot, dialer pattern for Broadcast, etc.]
  - Hero metric: callout positioned upper-left of central visual
  - Proof strip: 3-column at bottom, navy bold headings + body text

page_2:
  - Left column: blue solid panel with white text, 3 use cases with icons
  - Right column: white with dark text, 6 features in icon+text rows
  - Bottom CTA: oversized white-on-navy or dark-on-white, curved-arrow visual cue
  - Contact strip: 3-channel with icons, fixed order Mail · Call · Website

callouts:
  - Brand bar: logo top-left + wave accent top-right on both pages
  - Icons: blue line-art for features, white line-art for use cases
  - Decorative dots/waves: subtle background accent

gamma_format_hint:
  - 2 pages, A4 portrait
  - Image-heavy, text-tight aesthetic
```

For 13.2:

```markdown
[DESIGN INTENT]

format:        document
dimensions:    a4
text_mode:     preserve
audience:      [from brief]
language:      en-in
variant:       13.2 — 1-page sales one-pager

theme_hint:    modern professional, navy primary

layout:
  - Hero: oversized bold, top of page
  - Problem + Answer: side-by-side or stacked, equal weight
  - 3 differentiators: icon grid, parallel layout
  - 2 proof points: stat-first format, bold metric + supporting context
  - CTA: prominent button-style, primary brand colour
  - Trust footer: monochrome strip at bottom

gamma_format_hint:
  - 1 page, A4 portrait
  - Text-dense but white-space-aware
  - One supporting visual maximum
```

### Step 7 — Self-run quality gate

Common to both variants:
- [ ] No banned words (KB-01 Voice Card)
- [ ] Numbers formatted per KB-01
- [ ] CTA matches Journey Card
- [ ] Product names exact and capitalised
- [ ] Parallel grammar within each group

13.1 specific:
- [ ] Hero ≤ 10 words, outcome-led
- [ ] Exactly 1 hero metric on Page 1
- [ ] Exactly 3 proof strip blocks (= 4 proof points total with hero metric)
- [ ] Exactly 3 use cases on Page 2
- [ ] Exactly 6 features on Page 2
- [ ] Contact strip in fixed order Mail · Call · Website
- [ ] Total body 200–400 words

13.2 specific:
- [ ] Hero 8–12 words, doesn't start with Acefone/AceX
- [ ] Problem ≤ 40 words, 2 sentences
- [ ] Answer ≤ 25 words, 1 sentence
- [ ] Exactly 3 differentiators, parallel structure
- [ ] Exactly 2 proof points, metric-first
- [ ] CTA ≤ 15 words
- [ ] Body (problem → proof points) ≤ 250 words
- [ ] Trust footer verbatim (not paraphrased)

### Step 8 — Pause for user approval

> *"Flyer draft ready (variant [13.1 | 13.2]). Approve to render via Gamma, or share changes."*

Full rewrite on every feedback round.

### Step 9 — Hand-off

On approval, pass the draft + `[DESIGN INTENT]` block to `/gamma-render`.

---

## Hard Rules

1. **Single variant per request.** No hybrid flyers. If the brief drifts mid-intake, halt and re-route.
2. **Locked counts.** 13.1 = 1 hero metric / 3 props / 3 use cases / 6 features. 13.2 = 3 differentiators / 2 proof points. Never break these.
3. **Hero never starts with Acefone or AceX.** Always outcome-first.
4. **Parallel grammar within each group.** Mixing forms breaks the visual rhythm Gamma will render.
5. **Word count caps are enforced.** 13.1 body 200–400; 13.2 body ≤ 250 (excluding hero + footer).
6. **Contact strip order is fixed.** 13.1 = Mail · Call · Website. 13.2 trust footer = verbatim.
7. **Zero banned words.** Flyers are short — banned words are unmissable.

---

## Output Contract

This skill emits exactly two artifacts:
1. The flyer draft (Markdown — 1 or 2 pages depending on variant).
2. The `[DESIGN INTENT]` block.

Both go to `/gamma-render` on user approval.
