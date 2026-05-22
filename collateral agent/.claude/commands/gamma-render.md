# Gamma Render Skill

## Identity and Scope

Single gateway to Gamma MCP for the Collateral Agent. Every drafter (ebook, brochure, flyer, slide deck) routes here for the final render. Centralising Gamma's contract here keeps drafter skills free of Gamma's parameter shape.

Invoked at **Step 10 of CLAUDE.md** — after the user approves the drafted body.

**Critical operating rule:** Call `mcp__claude_ai_Gamma__generate` (or `generate_from_template`) **once** per render. **Do not poll `get_generation_status` in a loop.** The MCP widget that Gamma returns auto-polls and updates when generation completes. Looping wastes context and ignores the MCP server's explicit instruction. Only call `get_generation_status` if the user asks "is it ready?".

---

## Inputs

The drafter passes:
1. **Drafted Markdown body** — structured content with card boundaries marked by `---` horizontal rules between cards/pages.
2. **`[DESIGN INTENT]` block** — see contract below.
3. **Journey Card** (from `/journey-mapper`) — locked CTA pattern, content angle.
4. **Voice Card** (from `/brand-voice`) — brand rules folded into `additionalInstructions`.

## `[DESIGN INTENT]` Contract

The drafter emits this block at the end of every draft. `/gamma-render` consumes it.

```
[DESIGN INTENT]
docType:        ebook | brochure | flyer-product | flyer-onepager | slide-deck
format:         document | presentation | webpage
dimensions:     a4 | 16x9 | letter | pageless
numCards:       <integer>
textMode:       preserve | generate | condense
themeHint:      <free text, optional — used to match a theme via get_themes>
coverConcept:   <one sentence describing the cover/hero card>
visualStyle:    photorealistic | illustration | abstract | 3D | lineArt
imageSource:    aiGenerated | placeholder | noImages | webFreeToUse
sectionVisuals: <free text describing per-section visual treatments>
exportAs:       <pdf, only when user explicitly requested an exported file>
[/DESIGN INTENT]
```

If the drafter omits a field, the per-doc-type default below applies.

---

## Per-Doc-Type Defaults

| Doc Type | format | dimensions | numCards | textMode | imageSource | visualStyle | headerFooter |
|---|---|---|---|---|---|---|---|
| Ebook — TOFU | document | a4 | 18 | preserve | aiGenerated | illustration | book title + cardNumber |
| Ebook — MOFU | document | a4 | 22 | preserve | aiGenerated | illustration | book title + cardNumber |
| Ebook — BOFU | document | a4 | 16 | preserve | placeholder | illustration | book title + cardNumber |
| Brochure | document | a4 | 3 | preserve | aiGenerated | illustration | none |
| Flyer — product (2-page) | document | a4 | 2 | preserve | aiGenerated | illustration | none |
| Flyer — one-pager (1-page) | document | a4 | 1 | preserve | aiGenerated | illustration | none |
| Slide deck — first pitch | presentation | 16x9 | (from deck task) | preserve | aiGenerated | illustration | none |
| Slide deck — RFP / QBR | presentation | 16x9 | (from deck task) | preserve | aiGenerated | illustration | none |

**Why `textMode=preserve` everywhere:** the drafter's output has already passed brand-voice validation (banned-word scan, headline rules, deliverable formatting). `preserve` keeps the text as-is so we don't re-introduce violations through Gamma's rewriter. If `preserve` proves to deviate in testing, fall back to `generate` with stronger `additionalInstructions`.

---

## Workflow

### Step 1 — Validate inputs
- Drafted body must be non-empty Markdown with at least one `---` boundary (except 1-page flyers).
- `[DESIGN INTENT]` block must be present.
- Brand-voice validation must have been run (see `brand-voice.md`). If not, halt.

### Step 2 — Theme discovery (CONDITIONAL)
Only when `themeHint` is non-empty in the design intent:
1. Call `mcp__claude_ai_Gamma__get_themes` once per session; cache the result.
2. Match `themeHint` against the returned themes by keyword (tone + colour descriptors).
3. If a confident match exists, pass its `id` as `themeId`. Otherwise omit `themeId` (Gamma's default theme is usually fine).

**Do not call `get_themes` if no themeHint is supplied.** Per the MCP description, calling it without need wastes context and risks suboptimal matches.

### Step 3 — Template discovery (CONDITIONAL)
Only when the user explicitly references a Gamma template by name or ID:
1. Call `mcp__claude_ai_Gamma__get_gammas` with `type="template"`.
2. Match by title query or use the supplied ID.
3. If found, use the `generate_from_template` path (Step 4b).
4. If no template matches the request, fall back to plain `generate` (Step 4a) and inform the user.

**Until Acefone templates exist in Gamma**, this path is dormant. Plain `generate` is the default.

### Step 4a — Build `generate` payload (default)

```
{
  "inputText": <drafted Markdown body, verbatim, including --- card boundaries>,
  "format": <design intent or doc-type default>,
  "numCards": <design intent or doc-type default>,
  "textMode": "preserve",
  "themeId": <from Step 2, only if matched>,
  "cardOptions": {
    "dimensions": <design intent or doc-type default>,
    "headerFooter": <ebooks only — see Header/Footer Pattern below>
  },
  "imageOptions": {
    "source": <design intent or doc-type default>,
    "stylePreset": <design intent or doc-type default>
  },
  "additionalInstructions": <constructed per Step 5>,
  "exportAs": <only if design intent specified pdf>
}
```

**Do not pass `textOptions.amount`** when `textMode=preserve`. The schema allows it but it may cause unexpected expansion.

### Step 4b — Build `generate_from_template` payload

```
{
  "gammaId": <template ID from Step 3>,
  "prompt": <drafted Markdown body + brief adaptation directive, e.g. "Populate this brochure template with the following Lead Generation content. Preserve the source structure and image positions. Replace placeholder copy verbatim with the supplied text.">,
  "themeId": <from Step 2, only if user requested a theme change>,
  "imageOptions": { ... },
  "exportAs": <only if design intent specified pdf>
}
```

The `prompt` field has a 400,000-char budget shared with the source template. Keep adaptation directives brief; the bulk of the prompt is the verbatim drafted content.

### Step 5 — Construct `additionalInstructions`

Single string that gives Gamma the guidance the structured params can't carry:

```
Brand context:
- Voice: Acefone's professional, outcome-led, business-results-focused. No paraphrasing — preserve drafted copy verbatim.
- Banned words (do not introduce in captions, alt text, or filler): leverage, synergy, robust, cutting-edge, game-changer, world-class, seamless, innovative, innovation, empower, transform, future-proof, state-of-the-art, next-generation, holistic, ecosystem, end-to-end, streamline.
- Number formatting: use numerals (3x, 60%, ~500ms, 15,000+); en-dash for ranges (25–35%); "typically" or "up to" prefix on estimates.
- Currency: ₹ for India-targeted content.

Cover concept: <coverConcept from design intent>

Per-section visuals: <sectionVisuals from design intent>

Final card requirement: The CTA block (phone, mail, URL) must appear prominently on the final card — not buried as inline text.

<doc-type addendum, picked from the list below>:
- Ebook: persistent footer with book title + card number on every body card (cover + back cover omit footer). Numbered chapter headings (02, 03, ...). Definition lists rendered as bold term + line-break + description.
- Brochure: 3-card rhythm — page 1 = 4-block value-prop grid; page 2 = 4-step process with flow diagram; page 3 = 6-block feature grid + integrations strip + trust strip + CTA. Maintain rhythm exactly.
- Flyer (product, 2-page): page 1 = hero + 4-block capability/proof grid; page 2 = 3-use-case grid (parallel grammar) + "Built on Acefone's Communication Infrastructure" + 6-block infrastructure grid + CTA.
- Flyer (one-pager): hero + problem + answer + 3 differentiators + 2 proof points + CTA + trust footer. Single card.
- Slide deck: follow JSON structure passed in inputText. Each top-level key (slide_02, slide_03, ...) is one card.
```

### Step 6 — Header/Footer Pattern (ebooks only)

```
"cardOptions": {
  "dimensions": "a4",
  "headerFooter": {
    "bottomLeft":  { "type": "text", "value": "<ebook title>" },
    "bottomRight": { "type": "cardNumber" },
    "hideFromFirstCard": true,
    "hideFromLastCard": true
  }
}
```

This satisfies KB-06 Part 11's persistent footer requirement (`[Ebook Title] [Page #]`).

### Step 7 — Call Gamma (ONCE)
Single call to `mcp__claude_ai_Gamma__generate` or `mcp__claude_ai_Gamma__generate_from_template`. Surface the tool response to the user immediately — the response either contains the final `gammaUrl` or an auto-polling widget.

**Do not call `get_generation_status` after this.** Wait for the widget. Only invoke `get_generation_status` if the user explicitly asks "is it ready?" — and even then, exactly once, not in a loop.

### Step 8 — Return to orchestrator

```
GAMMA RENDER RESULT
URL:         <gammaUrl, or "pending — see widget above">
GenerationId:<id, for editorial DB log>
Format:      <format>
NumCards:    <numCards>
Theme:       <theme name, or "default">
Status:      <complete | in-progress>
ExportUrl:   <if exportAs was pdf>
```

The orchestrator passes this to Step 11 (Notion editorial DB log). The URL and generation ID are required fields for the log entry.

---

## Card Boundary Convention

Drafters MUST insert `---` horizontal rules between intended cards. Empirically Gamma respects horizontal-rule boundaries in `preserve` mode, but this is unconfirmed. If Gamma collapses cards in testing:
1. First mitigation: add explicit `# Card N: <title>` headers at each boundary.
2. Second mitigation: switch the drafter to emit Gamma's slide notation (TBD per docs).
3. Last resort: switch `textMode` from `preserve` to `generate` and rely on `numCards` + `additionalInstructions` to enforce structure.

---

## Known Limitations and Open Questions

1. **`preserve` mode fidelity is unconfirmed.** The schema says it uses inputText "as-is, without any changes" — but Gamma may still apply minor formatting tweaks (heading levels, list spacing, image placement). First flyer render in Phase 4 will confirm or refute. If banned words appear post-render under `preserve`, switch to `generate` with stronger guidance.

2. **Card boundary signal is empirical.** `---` is the working assumption. Verified only in Phase 4 first render.

3. **No Acefone Gamma templates exist yet.** `generate_from_template` path is dormant until the user creates templates in Gamma. Plain `generate` is the only path that runs today.

4. **Theme match is heuristic.** `themeHint` → `themeId` mapping is keyword-based against `get_themes` output. Quality is best-effort. If brand wants a single locked theme across all pieces, set `themeId` directly in this skill once a brand theme is chosen.

5. **Image consistency.** AI-generated images may drift between renders. Mitigations: lock `stylePreset: "illustration"` (default here), define a custom `imageOptions.style` prompt encoding Acefone visual brand once that brand spec exists, or fall back to `imageSource: "placeholder"` for pieces going through human designer review.

6. **CTA block fidelity.** Phone + Mail + URL blocks may render as plain text. Mitigated by the explicit `additionalInstructions` directive (Step 5). Verify on Phase 4 first render.

7. **No edit-after-generate.** Confirmed by Gamma MCP. Every iteration is a fresh `generate` call. Communicate this expectation to users — they refine in the Gamma editor, or revise the draft and re-render.

8. **`textOptions.amount` interaction with `textMode=preserve` is undocumented.** We omit `amount` when using `preserve` (safe default).

9. **`exportAs` PDF URLs expire.** Per the schema description: "exportUrl is signed and expires after approximately one week." Captured exportUrl must be downloaded promptly or re-requested. Editorial DB log captures the timestamp.

10. **Long ebook content brief size.** Ebooks ~3,500–4,500 words plus design intent should fit comfortably in `inputText` (no explicit cap documented; `generate_from_template`'s prompt cap is 400K chars). If a very long ebook strains the call, chunking is not supported — split into multi-document renders and reassemble.

---

## Failure Modes

- **Gamma returns an error**: surface the error verbatim. Do not retry automatically. Ask the user to revise design intent or content.
- **Generated URL is missing in response**: report "Generation queued — open the Gamma widget to see the URL when ready" and stop. Do not poll.
- **Banned words detected in rendered output** (rare under `preserve`): flag to user, suggest switching to `generate` with stronger `additionalInstructions` or refining the draft.
- **Card count mismatch** (Gamma generated more/fewer cards than `numCards`): note in editorial DB log; not a hard failure. If consistent, tune `numCards` higher or add more `---` boundaries.
- **Theme not found**: omit `themeId` and proceed with Gamma's default. Inform the user the theme hint did not match a known theme.

---

## Example Call — Brochure (Lead Generation bundle, MOFU)

```
mcp__claude_ai_Gamma__generate(
  inputText="""
# Lead Generation

Lead generation via missed call refers to the process of acquiring potential customers by encouraging them to initiate contact through a phone call to a marketed number, thereby expressing interest in a product or service.

## Value Propositions

**Customer Verification**
Using missed call alerts for customer verification offers a superior alternative to OTPs. Invite customers to confirm their phone numbers by placing a missed call to a designated number.

[... rest of page 1 ...]

---

# How It Works

A B2C Company wants to capture the interested audience for their recently launched product...

## Steps
1. Customer views your advertisement on all platforms.
2. Customer initiates a call on displayed number, which disconnects automatically.
3. Generated leads are captured and stored on the client's cloud-based server.
4. Automated callback or SMS is triggered after the call gets disconnected.

[Lead Generation Flow: Client Calls → DID Number → Missed Call Logs created on AceX server → SMS/Callback]

---

# How Acefone Can Help

[6-feature grid: Engagement via WhatsApp & SMS · Advanced API Suite · Geographical Call Capturing · Workflow Automation · Scalable Infrastructure · Unique Callers Identification]

Easy integration with leading CRMs and tools.

Top Brands Trust Acefone.

**Get an exclusive demo session today.**
📞 1800-121-7777
✉ contact@acefone.com
🌐 acefone.com
""",
  format="document",
  numCards=3,
  textMode="preserve",
  cardOptions={ "dimensions": "a4" },
  imageOptions={ "source": "aiGenerated", "stylePreset": "illustration" },
  additionalInstructions="Brand context: Use Acefone's professional, outcome-led voice. Do not paraphrase — preserve drafted text verbatim. Banned words: leverage, synergy, robust, cutting-edge, seamless, innovative, empower, transform. Cover concept: Lead-capture hero with phone + funnel visual. Per-section visuals: page 1 = value-prop grid icons; page 2 = horizontal flow diagram for missed-call process; page 3 = 6-feature grid icons + CRM integration logos + customer brand logos. Final card requirement: CTA block (phone 1800-121-7777, mail contact@acefone.com, demo offer) appears prominently on page 3. Brochure rhythm: page 1 = 4-block value-prop grid; page 2 = 4-step process with flow diagram; page 3 = 6-block feature grid + integrations strip + trust strip + CTA."
)
```
