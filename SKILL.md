---
name: social-media-carousel
description: "Generate professional social media carousel posts directly in Canva via MCP. Full pipeline: parse content → build 7-slide outline → review with user → generate 4 design variants in Canva → user picks one or all → deliver edit links + optional PNG/PDF export. Platform-aware (LinkedIn, Instagram, Twitter/X, Facebook). Uses 7-slide framework: hook → context → 4 value slides → CTA. Trigger words: carousel, instagram carousel, linkedin carousel, slide post, carousel design, swipe post, multi-image post, carousel template, educational carousel, carousel content, instagram slides, linkedin slides."
---

# Social Media Carousel Skill

Build high-engagement carousels and publish them directly to Canva — fully editable, export-ready.

---

## Trigger

Activate when the user types `/carousel` or asks to create a carousel, slide post, or swipe post for LinkedIn, Instagram, Twitter/X, or Facebook.

---

## Pipeline Overview

```
Input content/topic
    ↓
STEP 1 — Gather requirements (platform, tone, handle)
    ↓
STEP 2 — Write 7-slide outline, show for approval
    ↓
STEP 3 — Check brand kits (list-brand-kits)
    ↓
STEP 4 — Submit to Canva (request-outline-review → generate-design-structured)
    ↓
STEP 5 — Show 4 design candidates with slide thumbnails
    ↓
STEP 6 — User selects one or all → create-design-from-candidate
    ↓
STEP 7 — Return Canva edit URLs
    ↓
STEP 8 — Optional: export as PNG/PDF
```

---

## STEP 1 — Gather Requirements

If the user provided topic/content in the trigger, extract:
- **Platform** (default: LinkedIn)
- **Carousel type** (Educational / Storytelling / Before-After / Listicle / Case Study)
- **Tone** (professional / casual / technical)
- **Handle** (for the CTA slide — ask if not provided)

If no content was provided, ask:
> "What's the topic, and which platform — LinkedIn or Instagram?"

---

## STEP 2 — Build the 7-Slide Outline

Map the content to this framework before sending to Canva:

| Slide | Role | Rule |
|-------|------|------|
| 1 | **Hook** | Bold claim, question, or number + promise. Max 15 words. Include "Swipe →" |
| 2–6 | **Value** | One point per slide. Max 30–40 words. Numbered (01, 02…) |
| 7 | **CTA** | Save / Follow / Comment. Include handle. |

**Hook types to choose from:**
- Bold claim: "90% of [audience] make this mistake"
- Question: "Why does [X] keep failing?"
- Number + promise: "5 things we do before [Y]"
- Contrarian: "Stop doing X (do this instead)"

**Compress the source content:** extract the sharpest version of each point. Cut filler. One idea per slide, no cramming.

Show the outline as a table before proceeding. Wait for approval or edits.

---

## STEP 3 — Check Brand Kits

Call `list-brand-kits` in parallel while waiting for outline approval (or immediately after approval).

Show available brand kits by name. Ask the user which to apply, or if they want the default Canva style.

---

## STEP 4 — Generate in Canva

**4a. Submit outline for review:**
Call `request-outline-review` with:
- `topic`: the hook headline (max 150 chars)
- `audience`: "professional" (default) or match platform
- `style`: "digital" for tech/B2B, "modular" for structured content, "elegant" for thought leadership
- `length`: always "short" (7 slides)
- `pages`: the 7-slide array, descriptions as `"- Point\n- Point\n- Point"` (hyphen bullets, no Unicode •)
- `brand_kit_id` + `brand_kit_name`: if user selected one

**4b. Generate design:**
Call `generate-design-structured` with the same parameters plus `design_type: "presentation"`.

This returns 4 design candidates, each with 7 slide thumbnail URLs.

---

## STEP 5 — Present Candidates

Show all 4 options as:

```
**Option A**
[Open in Canva](edit_url)
![Cover](thumbnail_url)
Slides: [1](url) · [2](url) · [3](url) · [4](url) · [5](url) · [6](url) · [7](url)
```

Ask: "Which do you want — A, B, C, D, or all?"

---

## STEP 6 — Create Editable Designs

For each selected option, call `create-design-from-candidate` with:
- `job_id`: from the generate step
- `candidate_id`: the selected candidate's ID

Run all selections **in parallel**.

Return the edit URLs grouped by option label.

---

## STEP 7 — Deliver Edit Links

Present results as:

```
**Option [X]** — "[title]" (N slides)
Edit: https://www.canva.com/d/...
```

Remind the user:
- Apply brand kit directly inside Canva
- Resize to 1080×1350 (4:5) for feed real estate on Instagram/LinkedIn
- Export as PNG for upload

---

## STEP 8 — Optional Export

If the user asks to export, call `export-design` with:
- `design_id`: from the create-design response (format: `D[a-zA-Z0-9_-]{10}`)
- `format.type`: "png" (default) or "pdf"
- `format.export_quality`: "pro" for max resolution
- `format.pages`: omit to export all slides

Always display the download URL returned.

---

## Platform Specs

| Platform | Dimensions | Max Slides | Best Ratio |
|----------|-----------|------------|------------|
| Instagram | 1080×1080 | 20 | **4:5 (1080×1350)** ← use this |
| LinkedIn | 1080×1080 | 20 | **4:5 (1080×1350)** ← use this |
| Twitter/X | 1080×1080 | 4 | 1:1 |
| Facebook | 1080×1080 | 10 | 1:1 or 4:5 |

---

## Design Style Guide

### Text hierarchy (at 1080px)
| Element | Size | Weight |
|---------|------|--------|
| Slide number | 96–120px | 900 |
| Heading | 48–64px | 700–800 |
| Body | 24–28px | 400 |
| Tag/caption | 18–22px | 500 |

### Readability rules
- Max 30–40 words per slide
- Line height: 1.5–1.6
- Font: sans-serif (Inter, Montserrat, Poppins)
- Text contrast: 4.5:1 minimum (WCAG AA)

### Consistency rules (all slides)
- Same background palette
- Same font family
- Same margin/padding
- Same accent color
- Same number format (01, 02… or 1., 2.)

---

## Carousel Types

**Educational / Tips**
Hook → Why this matters → Tip 1 → Tip 2 → Tip 3 → Tip 4 → Save/Follow

**Storytelling / Case Study**
Hook → The beginning → The challenge → What failed → What worked → Result (numbers) → Key takeaway + CTA

**Before / After**
Hook → Before state → Problem annotated → After state → What changed → Results → CTA

**Listicle / Tools**
Hook → Items 1–2 → Items 3–4 → Items 5–6 → Items 7–8 → Items 9–10 → Save CTA

**Process / How-We-Do-It**
Hook → Context (why this matters) → Step 1 → Step 2 → Step 3 → Step 4–5 combined → CTA

---

## Swipe Psychology

- **Curiosity gap**: Hook promises value that requires swiping
- **Numbered progress**: "3/7" creates completion drive — include in slide descriptions
- **Increasing value**: Best point last — rewards completing the carousel
- **Swipe cue**: "Swipe →" always on slide 1
- **Visual continuity**: Consistent design signals "there's more"

---

## Common Mistakes to Avoid

| Mistake | Fix |
|---------|-----|
| Weak hook | Use bold claim, question, or number + promise — never a generic title |
| Too much text per slide | Hard cap at 40 words. Cut ruthlessly. |
| No swipe indicator | "Swipe →" on slide 1, always |
| No CTA on last slide | Ask to save, follow, share, or comment |
| Cramming 2+ ideas per slide | One point per slide, no exceptions |
| Square format on Instagram/LinkedIn | Use 1080×1350 (4:5) — more feed real estate |
| No brand consistency | Same colors, fonts, spacing throughout |

---

## Canva MCP Tool Reference

| Tool | When to Use |
|------|------------|
| `list-brand-kits` | Fetch available brand kits before generation |
| `request-outline-review` | Required first step before generate-design-structured |
| `generate-design-structured` | Generate 4 design variants from approved outline |
| `create-design-from-candidate` | Convert selected candidate(s) into editable designs |
| `export-design` | Export finished design as PNG/PDF with download URL |
| `start-editing-transaction` + `perform-editing-operations` + `commit-editing-transaction` | Make inline edits to an existing design |

---

## Output Location

Save exported files to: `outputs/carousels/[topic-slug]-[YYYY-MM-DD]/`

Name files: `slide-01.png`, `slide-02.png`... or `carousel-[slug].pdf`
