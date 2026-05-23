---
title: Architecture Overview
tags: [architecture, overview]
---

# Architecture Overview

ภาพรวม WarpClip — system shape, boundaries, data flow.

## Phase 1: MVP Landing Page

Single static-ish Next.js app. ไม่มี backend logic. ไม่มี database.

```
┌─────────────────────────────────────────┐
│  PASAKON/WarpClip-webapp (Next.js 16)   │
│  ├─ app/page.tsx       (Hero)           │
│  ├─ app/services       (Services)       │
│  ├─ app/portfolio      (Portfolio grid) │
│  ├─ app/pricing        (Pricing tiers)  │
│  └─ components/        (UI primitives)  │
└──────────────┬──────────────────────────┘
               │ deploy
               ▼
        ┌──────────────┐
        │   Vercel     │ ← warpclip.com (custom domain)
        │  CDN + Edge  │
        └──────────────┘
               │
               ▼
        ┌──────────────┐
        │  End user    │
        │  (mobile/web)│
        └──────┬───────┘
               │ CTA click
               ▼
        ┌──────────────┐
        │  LINE OA     │ ← lead capture (manual handoff)
        │  (external)  │
        └──────────────┘
```

## Boundaries

| Boundary | Owner | Notes |
|----------|-------|-------|
| Frontend rendering | Next.js App Router | static + RSC; no server actions ใน MVP |
| Lead capture | LINE Official Account | external; ไม่มี API call จาก webapp |
| Content / Portfolio | hardcoded JSON ใน repo | TODO: move to CMS เมื่อ scale |
| Analytics | Vercel Web Analytics | built-in, no client-side scripts |
| SEO | Next.js `metadata` API | static OG image, sitemap, robots |

## Tech Stack

- **Framework:** Next.js 16 (App Router) — [[../40-Decisions/0001-stack-nextjs-typescript-tailwind|ADR-0001]]
- **Language:** TypeScript strict mode
- **Styling:** Tailwind CSS 4
- **Package manager:** pnpm
- **Hosting:** Vercel — [[../40-Decisions/0003-deploy-vercel|ADR-0003]]
- **Repos:** 3-repo polyrepo split — [[../40-Decisions/0004-polyrepo-split|ADR-0004]]
- **No DB ใน MVP** — [[../40-Decisions/0002-no-db-mvp|ADR-0002]]

## Brand Architecture

WarpClip = sub-brand ของ MoonieX (House of Brands strategy).
ดู [[../40-Decisions/0005-mooniex-house-of-brands|ADR-0005]].

Visual cohesion ผ่าน design tokens shared (ดู `design/` repo).
Brand independence: WarpClip มี marketing/positioning ของตัวเอง — ไม่ tied กับ MoonieX trading audience.

## Data Flow

**Phase 1 = uni-directional:**

1. User เปิด `warpclip.com` → Vercel CDN serves static HTML+RSC
2. User scroll → Tailwind animation, Vercel Web Analytics tracks
3. User click "ติดต่อ" CTA → redirect to LINE OA URL (external)
4. Lead managed มือใน LINE OA (no backend integration)

**ห้ามทำใน Phase 1:**
- Server actions ที่เก็บข้อมูล
- API routes ที่ proxy ไป external service (ยกเว้น OG image generation)
- Cookie / localStorage สำหรับ user state

ถ้าต้องการเก็บ lead data → ต้องเขียน ADR ใหม่ (supersede [[../40-Decisions/0002-no-db-mvp|ADR-0002]]).

## SEO Strategy

- Static metadata + structured data (JSON-LD `Service` + `LocalBusiness`)
- Sitemap + robots ผ่าน Next.js convention files
- OG image generation ด้วย `next/og`
- Thai-first content, English secondary

## Performance Budget

| Metric | Target | Tool |
|--------|--------|------|
| LCP | < 2.5s mobile | PageSpeed Insights |
| CLS | < 0.1 | Vercel Speed Insights |
| Bundle (initial) | < 100kb gzipped | `next build` analyzer |
| Lighthouse score | ≥ 95 (all categories) | Vercel auto-check |

## See Also

- [[../40-Decisions/README|ADR Index]]
- [[../50-Workflows/Dev-Workflow]]
- [[../30-Domain/Glossary]]
