---
title: "ADR-0006: Brand pivot — B&W premium with single accent (v2.0) [DEFERRED]"
tags: [adr, brand, design, deferred]
status: Deferred
date: 2026-05-23
deferred_at: 2026-05-26
---

# ADR-0006 — Brand pivot: B&W premium with single accent (v2.0)

> ⚠️ **STATUS: DEFERRED (2026-05-26).** This ADR proposed pivot to
> B&W light base + sage/lime single accent. **The pivot was not
> implemented in code.** Shipped site uses **dark zinc-950 + neon lime
> `#CCFF00`**.
>
> Treat as **future direction proposal**, not a current brand spec.
> Do not brief brand assets from this document.
>
> **Canonical brand spec:** [[../10-Architecture/Brand-Truth|Brand-Truth.md]]

## Status

Deferred — proposal only, not implemented in code as of 2026-05-26.

## Context

WarpClip launched 2026-05-23 with brand v1.0:
- Dark theme (zinc-950 base)
- Tri-color gradient signature: indigo → violet → fuchsia
- Emerald CTA (LINE button)
- Mono W mark on gradient tile

Logo v1.0 shipped via [`WarpClip-design#7`](https://github.com/PASAKON/WarpClip-design/pull/7) +
[`WarpClip-webapp#1`](https://github.com/PASAKON/WarpClip-webapp/pull/1) on 2026-05-23 morning.

ในวันเดียวกัน CEO ตัดสินใจ pivot:
- Differentiation vs. video-editing competitors ที่ default ใช้ dark + gradient signatures
- Timeless brand position vs. trend-following
- Apple-minimal / Linear / Vercel posture — align กับ premium B2B + brand/agency/corporate audience
- Reduce visual cliché — "tech AI tool" gradient ดูเหมือนกันหมด

## Decision

Pivot ไป **light B&W premium + single accent** (v2.0):

- **Base:** light/white theme (replacing zinc-950 dark)
- **Wordmark:** mixed-type "Warp**Clip**" — Geist Bold (sans) "Warp" + Newsreader Italic 500 "Clip"
- **Mark:** `Wc` monogram (Geist Bold W + Newsreader Italic c) on black tile, replacing W stroke on gradient
- **Accent:** single accent color (final hex TBD — Brand-Brief v2.0 says sage `#6B8E6F`,
  initial code uses `#CCFF00` neon lime; designer คือคน decide canonical)
- **No tri-color gradients** — section gradients removed
- **Em-dash ban** in generated copy (user-typed character only — Brand-Brief v2.0 rule)
- **Two-tracks asset note**: v1.0 + v2.0 assets coexist in design repo เพื่อ rollback

## Implementation phases

**Phase 1 — Header/Footer + brand assets (in progress):**
- `WarpClip-design#8` — full logo v2.0 system (49 PNGs + 14 SVGs + tokens/colors.json)
- `WarpClip-webapp#2` — Header + Footer integration only (mark.svg, mixed wordmark, layout font load)

**Phase 2 — Body sections (pending — issue #9):**
- Hero, Services, Portfolio, Pricing, Contact ยังเป็น v1.0 dark+gradient
- Visual mismatch ใน preview deployment ตอนนี้
- ทำผ่าน claudesign web_designer agent (task-e9f4af31)

## Consequences

**Positive**
- Brand differentiation จาก competitors (Opus Clip, Submagic, etc. ที่ใช้ dark+gradient)
- Premium positioning เปิดทาง B2B/agency tier ราคาสูง
- Single-accent system maintenance ง่าย (vs tri-color gradient ที่ต้อง coordinate ทุก surface)
- Mixed-type wordmark = distinctive memorable signature
- B&W neutral base = portfolio thumbnails (full color) ไม่ clash กับ site chrome

**Negative**
- Re-shoot/redesign portfolio thumbnails อาจต้อง (v1.0 placeholders ใช้ gradient ที่ clash)
- เสีย "tech/AI tool" cue ที่ gradient ส่ง (อาจถูกตีความว่าเป็น traditional agency)
- Em-dash ban = constraint บน generated copy (พึ่ง LLM-generated text ต้อง strip ก่อน paste)
- Asset migration cost: webapp 5 sections + email templates + social ทั้งหมด ต้อง update
- v1.0 user mental model ของ Mooniex ecosystem (ถ้ามี) อาจขัดกับ v2.0 minimal

## Rollback Plan

ถ้า v2.0 ไม่ตอบโจทย์หลัง customer test:

1. Design repo มี v1.0 assets อยู่ (PR #7 merged, files ใน `assets/logo/png/*color*.png` + gradient SVGs)
2. Revert webapp commits ระหว่าง 2026-05-23 04:00 ถึง 06:30 UTC
3. Restore `globals.css` selection color + Header/Footer pre-v2 versions
4. Write supersede ADR (e.g., ADR-0007 "revert to v1.0")

## Alternatives

- **Stick กับ v1.0 (dark+gradient)** — already shipped, easier; ตัดออกเพราะ brand differentiation argument
- **Hybrid (dark base + B&W accents)** — confusing visual hierarchy; reject
- **Light theme + colorful accents (multiple)** — defeats "single accent" simplicity gain
- **Light theme + no accent (pure B&W)** — visually flat, no CTA emphasis
- **Pivot to brand color from MoonieX parent** — defeats House of Brands strategy ([[0005-mooniex-house-of-brands|ADR-0005]])

## Open Questions

- **Final accent hex:** sage `#6B8E6F` (Brand-Brief) vs `#CCFF00` neon lime (initial code) — designer decides
- **Headline gradient:** keep indigo→violet→fuchsia ใน Hero headline? หรือ swap ไป B&W solid? (decision in Phase 2)
- **Dark inverted blocks:** Brand-Brief Sec.10 allows dark inverted blocks (Header chrome) — extent?

## See Also

- [[0001-stack-nextjs-typescript-tailwind]]
- [[0005-mooniex-house-of-brands]]
- [[../10-Architecture/Overview]]
- [WarpClip-design#8 — Logo v2.0 PR](https://github.com/PASAKON/WarpClip-design/pull/8)
- [WarpClip-webapp#2 — Header/Footer integration PR](https://github.com/PASAKON/WarpClip-webapp/pull/2)
- [WarpClip-design#9 — Phase 2 full landing redesign](https://github.com/PASAKON/WarpClip-design/issues/9)
