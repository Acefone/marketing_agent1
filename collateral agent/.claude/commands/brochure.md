# Brochure Drafter Skill

## Role

3-page A4 brochure drafter for AceX use-case bundles. Consumes the brief + voice card + journey card; emits a structured 3-page Markdown body + `[DESIGN INTENT]` block for `/gamma-render`.

**One use case per brochure.** Brief intake enforces this — multi-topic brochures are rejected.

Authoritative template: **KB-06 Part 12**.

---

## When Invoked

Step 9 of the orchestrator workflow, after brief + journey stage are locked (no research step for brochures by default — opt-in only).

You receive: brief, voice card, journey card.

---

## Workflow

### Step 1 — Brief intake (single message, all questions at once)

Ask the consultant:

1. **Single use case** — what is the one use case this brochure is for? (e.g. "Lead Generation via Missed Call", "COD Verification", "DPD 1-30 Collections", "Appointment Booking"). If they list more than one, refuse and ask them to pick one.
2. **Target reader (ICP + persona)** — vertical + role + size signal (e.g. "BFSI collections head, NBFC, 50–200 agents").
3. **3 pain points the reader is living with** — in their language.
4. **Page 1: 4 value propositions to feature** — name + 2–3 line benefit each. (If they don't have 4, propose 4 from the use case + KB-06 Part 12.3 templates.)
5. **Page 2: the 4 numbered steps in the workflow** — what the customer does → what the platform does → what the system processes → what the customer receives.
6. **Page 3: 6 features to feature** — pick from KB-06 Part 10 product list, anchored to the use case.
7. **Trust strip** — top brand logos to show (or use the standard 8-logo set).
8. **CTA** — confirm the CTA pattern from Journey Card or override (the journey-mapper has already locked one, but the user can pick a sharper version).

Confirm intake in caveman lite: *"Got it — [use case], [ICP]. Drafting 3 pages."*

### Step 2 — Draft per KB-06 Part 12.1 layout

**Page 1 — WHAT**

```markdown
# [USE CASE NAME — oversized, outcome-led, ≤ 12 words]

[2–3 sentence definition. Name the mechanic and the outcome.]

[4-prop grid — exactly 4 value props]

→ **[Prop 1 name]** — [2–3 line benefit. Include one number where possible.]
→ **[Prop 2 name]** — [2–3 line benefit.]
→ **[Prop 3 name]** — [2–3 line benefit.]
→ **[Prop 4 name]** — [2–3 line benefit.]
```

**Page 2 — HOW**

```markdown
## How It Works

[One-paragraph narrative use case — 60–80 words. "A [ICP] wants to [outcome]. They [trigger]. Here's what happens next."]

[Exactly 4 numbered steps — each 1–2 lines]

**Step 1**: [Action — what the customer / prospect does]
**Step 2**: [Action — what triggers in the platform]
**Step 3**: [Action — what gets captured / processed]
**Step 4**: [Action — what the customer receives back]

[Flow Diagram — described as design intent]
[VISUAL SUGGESTION: flow diagram — Source → AceX Platform → Outcome channel | Alt text: "[Use case] flow on AceX"]
```

**Page 3 — HELP + CLOSE**

```markdown
## How Acefone Helps

[Exactly 6 features — name + 2-line description, tied to the use case]

→ **[Feature 1]** — [2-line description]
→ **[Feature 2]** — [2-line description]
→ **[Feature 3]** — [2-line description]
→ **[Feature 4]** — [2-line description]
→ **[Feature 5]** — [2-line description]
→ **[Feature 6]** — [2-line description]

---

**Easy integration with leading CRMs and tools**
[VISUAL SUGGESTION: integrations strip — 6–8 CRM/tool logos in horizontal strip]

**Top Brands Trust Acefone**
[VISUAL SUGGESTION: trust strip — 8 customer logos in horizontal strip]

---

[CTA line from Journey Card — imperative + outcome]
📞 1800-121-7777   ✉ contact@acefone.com

© 2026 Acefone. All rights reserved. www.acefone.com
```

### Step 3 — Apply brand voice rules

- Banned-word scan (KB-01 Voice Card)
- Use-case name ≤ 12 words, outcome-led
- Numbers per KB-01 formatting
- Product names exact and capitalised (Voice Bot, Broadcast, API Connect, etc.)
- Industry vocabulary matches the ICP (BFSI / Healthcare / Logistics / E-commerce / BPO)
- Sales register DO/DON'T

### Step 4 — Heading criteria (dynamic per audience)

Each value prop, step, and feature heading is dynamic. Apply these patterns based on the ICP and what the heading is doing:

- **Value props (Page 1)**: stat-led (`"60% Lower Cost-Per-Call"`) OR capability-led (`"Native SIP Routing"`) — never benefit-only ("Better Quality"). Match the ICP's KPI vocabulary.
- **Workflow steps (Page 2)**: action-form, 3–5 words each (`"Customer dials displayed number"`, `"Platform logs missed call"`). Parallel grammar across all 4.
- **Features (Page 3)**: noun phrases, 2–4 words each (`"Geographical Call Capturing"`, `"Workflow Automation"`). Parallel grammar across all 6.

If the brief's audience suggests a sharper register (e.g. C-suite vs ops manager), tighten word count and lead with the metric.

### Step 5 — Emit the `[DESIGN INTENT]` block

```markdown
[DESIGN INTENT]

format:        document
dimensions:    a4
text_mode:     preserve
audience:      [from brief — exact ICP phrase]
language:      en-in

theme_hint:    modern professional, navy + blue accent + green highlight

page_1:
  - Hero treatment: use-case name oversized, navy primary
  - Definition: subhead style, normal weight
  - 4-prop grid: icon + bold name + 2–3 line description, 2x2 layout
  - Decorative accent: top-right wave/dot graphic (Acefone brand element)

page_2:
  - Narrative paragraph at top
  - 4 numbered steps in horizontal strip with mini-illustrations
  - Flow diagram below — Source → AceX → Outcome
  - Decorative accent top-right

page_3:
  - 6-feature grid: icon + bold name + 2-line description, 2x3 or 3x2 layout
  - Integrations strip: small monochrome logos
  - Trust strip: customer logos
  - CTA block: oversized imperative + dual-channel contact
  - Footer: copyright + URL

callouts:
  - Step numbers: green circle treatment
  - Feature icons: blue line-art style
  - Trust strip: monochrome logo treatment

gamma_format_hint:
  - 3 pages, A4 portrait
  - Persistent brand bar: logo top-left + wave accent top-right every page
  - Contact strip layout: 📞 left · ✉ right OR centred 3-channel strip if there's room
```

### Step 6 — Self-run quality gate

- [ ] Exactly 1 use case (no multi-topic)
- [ ] Page 1 has exactly 4 value props
- [ ] Page 2 has exactly 4 numbered steps
- [ ] Page 3 has exactly 6 features
- [ ] Page 3 has integrations strip + trust strip + CTA in that order
- [ ] CTA is dual-channel (phone + mail)
- [ ] Use-case name ≤ 12 words, outcome-led
- [ ] All value props / steps / features use parallel grammar within their group
- [ ] No banned words (KB-01 Voice Card)
- [ ] Numbers formatted per KB-01
- [ ] Total body 500–750 words
- [ ] Industry vocabulary matches the ICP throughout
- [ ] `[DESIGN INTENT]` block populated

### Step 7 — Pause for user approval

> *"Brochure draft ready. Approve to render via Gamma, or share changes."*

Full rewrite on every feedback round — never patch.

### Step 8 — Hand-off

On approval, pass the draft + `[DESIGN INTENT]` block to `/gamma-render`.

---

## Hard Rules

1. **One use case per brochure.** Period. Multi-topic = rejected at intake.
2. **The 4 / 4 / 6 rhythm is locked.** Page 1 = 4 props. Page 2 = 4 steps. Page 3 = 6 features. Never 3, 5, 7. Gamma rendering depends on this.
3. **Page 3 order is fixed.** Feature grid → integrations strip → trust strip → CTA → footer. Never rearrange.
4. **Dual-channel CTA always.** Phone + mail. The brochure is MOFU — prospects often want to call.
5. **Parallel grammar within each group.** All 4 props are noun phrases OR all are stat-led OR all are verb phrases. Same for steps and features.
6. **Zero banned words.** Brochures are short — every word is conspicuous.

---

## Output Contract

This skill emits exactly two artifacts:
1. The 3-page brochure draft (Markdown).
2. The `[DESIGN INTENT]` block.

Both go to `/gamma-render` on user approval.
