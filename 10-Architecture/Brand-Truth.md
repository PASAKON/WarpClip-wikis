---
title: "Brand Truth — canonical visual identity (sourced from code)"
tags: [brand, design, canonical, agents-must-read]
status: Active
date: 2026-05-26
priority: P0
---

# Brand Truth — WarpClip

> **This file overrides every other brand document.** Wiki ADRs, design briefs,
> external decks are advisory. The **running code** is the source of truth.
> When wiki text and code disagree → code wins. Update this file before
> changing code; never the other way around.

## Why this exists

CMO briefed FB ad creative 2026-05-26 trusting `wikis/40-Decisions/0006-brand-v2-bw-premium.md`
(B&W light + sage accent) — but live site uses **dark + neon lime**. 3 generated
images rejected. Root cause: brand sources scattered, drift between aspirational
ADR and shipped code went undetected.

This file is the single canonical reference for any agent (CMO, web_designer,
content_strategist, copywriter) producing assets in WarpClip's name.

## Verification protocol (every agent MUST do before producing brand asset)

1. **Read this file.**
2. **Open `webapp/src/app/globals.css`** — confirm hex tokens match § "Color".
3. **Open `webapp/src/app/layout.tsx`** — confirm body bg + text colors.
4. **Open `webapp/src/components/sections/Hero.tsx`** — confirm signature visual.
5. **Open `webapp/public/brand/mark.svg`** — confirm mark form.
6. If any code disagrees with this file → CODE WINS → update this file in same PR.

Wikis that disagree with this file are **stale** until updated:
- `wikis/40-Decisions/0006-brand-v2-bw-premium.md` (B&W light pivot) → aspirational/deferred
- `design/WarpClip-Brand-Brief.md` § 4 Color Palette (v1.0 tri-color) → stale
- `webapp/public/brand/mark-512.png` (v1.0 indigo gradient W) → stale, do not use

## Color (canonical hex)

| Role | Hex | Tailwind | Notes |
|------|-----|----------|-------|
| Base | `#09090b` | `zinc-950` | **DARK** body bg |
| Text primary | `#fafafa` | `zinc-100` | on dark |
| Text muted | `#a1a1aa` | `zinc-400` | secondary copy |
| Border subtle | `rgba(255,255,255,0.10)` | `white/10` | card edges |
| Surface chip | `rgba(255,255,255,0.05)` | `white/5` | pill bg |
| **Accent** | **`#CCFF00`** | (custom) | **neon lime** — primary brand color |
| Accent hover | `#A8D400` | (custom) | darker hover state |
| Selection | bg `#CCFF00`, fg `#09090b` | — | ::selection in globals.css |

**Banned colors** (auto-reject any asset using these):
- Sage / mint `#6B8E6F` (aspirational only, never shipped)
- B&W only / pure greyscale (aspirational pivot, deferred)
- Cyan / turquoise (common LLM hallucination of lime)
- Tri-color gradient indigo→violet→fuchsia (v1.0 superseded)
- Emerald `#10b981` (v1.0 CTA, replaced by lime)
- Pastels of any kind
- Multiple accent colors per asset (single accent rule)

## Mark

Source: `webapp/public/brand/mark.svg`

```svg
<svg viewBox="0 0 100 100">
  <rect width="100" height="100" rx="22" fill="#CCFF00"/>
  <path d="M18 26 L32 76 L50 44 L68 76 L82 26"
        fill="none" stroke="#09090b" stroke-width="13"
        stroke-linecap="round" stroke-linejoin="round"/>
</svg>
```
- Tile: 100×100, `rx="22"` rounded corners
- Fill: **lime** `#CCFF00`
- W stroke: **dark** `#09090b`, stroke-width 13, rounded caps + joins
- Mark always reads as "dark W on lime tile" (never reversed)

Companion lockups: `design/assets/logo/png/lockup-horizontal-white-*.png` (mark + wordmark side by side).

## Typography

- Latin: **Geist Sans** — Google Fonts, weights 400 / 600 / 700
- Thai: system fallback (Apple SD Gothic Neo / Noto Sans Thai)
- No second font family allowed in MVP (no Newsreader Italic, no Inter — ignore stale ADR-0006 wording)
- Tracking: `tracking-tight` on display sizes
- Headline: ~64-96px display, weight 700
- Body: 16-18px, weight 400
- Section label: 12px uppercase `tracking-widest text-zinc-400`

## Signature visual: Lime Skewed Marker

The "Warp Speed" / "Prosonal" / emphasis-word treatment. Defined in Hero.tsx:

```tsx
<span className="relative inline-block px-3 py-1">
  <span className="absolute inset-0 -z-10 -skew-x-3
                   rounded-[8px_4px_12px_4px]
                   bg-[#CCFF00]" />
  <span className="relative text-zinc-950">EMPHASIS WORD</span>
</span>
```
- Background: lime `#CCFF00`
- Transform: `skewX(-3deg)`
- Border radius: `8px 4px 12px 4px` (asymmetric)
- Text inside: dark `text-zinc-950` (dark on lime)
- Used on **one phrase per surface** as emphasis. Not decorative.

This is the strongest brand signature. Every brand surface should use it on its emphasis word.

## CTA pattern

- Primary: lime pill `bg-[#CCFF00] text-zinc-950`, `rounded-full px-6 py-3`, weight 600
- Secondary: outline `border border-white/10 bg-white/5 text-white`, `rounded-full`
- Hover primary: `bg-[#A8D400]`
- Default text in primary CTA always dark zinc-950 on lime (never white on lime — fails contrast)

## Chip / status pill

- `inline-flex gap-2 rounded-full border border-white/10 bg-white/5 px-3 py-1 text-xs text-zinc-400`
- Dot: `h-1.5 w-1.5 rounded-full bg-[#CCFF00] animate-pulse`
- Use for "เปิดรับงานใหม่" status, "limited promo" markers, etc.

## Stats counter pattern

Triplet display, count-up on inView:
```
24-48      100+        98%
ชม. ส่งงาน  คลิป/เดือน  ลูกค้ากลับมา
```
- Number: large, bold, white
- Label: small, zinc-400
- Animated count from 0 to target on intersection

## Voice (link to ground truth)

- Brand voice: see `mooniex-llms-wiki/mooniex-voice-guide.md` (parent) + future
  `wikis/30-Domain/Voice.md` (WarpClip-specific, TBD)
- Hard voice rules from current site copy:
  - ใช้ "เรา" ไม่ใช้ "ผม/ฉัน"
  - ตัวเลขชัด: "24-48 ชม.", "990 บาท", "แก้ฟรี 3 ครั้ง"
  - **ห้าม em-dash** ใน generated copy (user-typed only)
  - ห้าม clichés: "เปลี่ยน clip ของคุณ", "สู่ระดับใหม่", "ครบจบในที่เดียว"
  - ห้าม AI tropes: unlock / elevate / delve / in today's fast-paced world

## Pricing canonical (from `Pricing.tsx`)

| Tier | Price | Notes |
|------|-------|-------|
| Basic | ฿990/คลิป | ≤60s, sub-title basic, 48ชม., แก้ฟรี 1 |
| **Pro** (default) | ฿1,990/คลิป | + sound FX, color grading, 24ชม., แก้ฟรี 3 |
| Premium | ติดต่อ | + B-roll, thumbnail, priority, แก้ไม่จำกัด |

Promo campaign override: ฿900/คลิป regular ฿1,590 (Promo SKU, ≤3 นาที, แก้ฟรี 3, 3 วัน). See `wikis/50-Workflows/Marketing-Workflow.md`.

## CTA channel state

- Landing primary: **LINE OA** (`คุยกับเราใน LINE` lime pill)
- FB ad campaign 2026-05+: **Messenger** (Click-to-Messenger ad, CTA "ทักผ่าน Messenger")
- LINE remains canonical for landing + retargeting; Messenger for cold paid traffic on Meta

## How to produce brand asset (rule for AI agents)

1. **NO image gen API for brand-strict creative.** No fal.ai, no gpt-image-2,
   no midjourney. These hallucinate hex codes + reverse contrast.
2. Build by **manual composition**:
   - HTML/CSS → Playwright screenshot → PNG
   - Python PIL/cairo composing existing SVG + font
   - SVG → rsvg-convert / Inkscape CLI
3. Reuse existing SVG mark — never recreate.
4. Hex codes must be exact (`#CCFF00`, `#09090b`) — never approximation.
5. Commit composition source (`.html` / `.py`) alongside output PNG.
6. Inline `<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Geist:wght@400;600;700&display=swap">` for Geist.

## Source of truth ranking (highest = wins)

| Rank | Source | Status |
|------|--------|--------|
| 1 | This file | canonical |
| 2 | `webapp/src/app/globals.css` | live code |
| 3 | `webapp/src/components/sections/*.tsx` | live code |
| 4 | `webapp/public/brand/mark.svg` | live asset |
| 5 | `wikis/50-Workflows/Marketing-Workflow.md` | operations |
| 99 | `wikis/40-Decisions/0006-brand-v2-bw-premium.md` | aspirational (deferred) |
| 99 | `design/WarpClip-Brand-Brief.md` § 4 | stale v1.0 |
| 99 | `webapp/public/brand/mark-512.png` | stale v1.0 |

## See Also

- [[../50-Workflows/Marketing-Workflow|Marketing Workflow]]
- [[../40-Decisions/0006-brand-v2-bw-premium|ADR-0006 (deferred)]]
- [Webapp Hero source](https://github.com/PASAKON/WarpClip-webapp/blob/main/src/components/sections/Hero.tsx)
- [Webapp globals.css](https://github.com/PASAKON/WarpClip-webapp/blob/main/src/app/globals.css)
- [Mark SVG](https://github.com/PASAKON/WarpClip-webapp/blob/main/public/brand/mark.svg)
