---
title: "ADR-0001: Stack — Next.js + TypeScript + Tailwind"
tags: [adr, stack]
status: Accepted
date: 2026-05-23
---

# ADR-0001 — Stack: Next.js + TypeScript + Tailwind

## Status

Accepted

## Context

WarpClip ต้องการ landing page MVP ที่:
- เร็วต่อ first paint (mobile-first audience คนไทย)
- SEO ดี (ผ่าน static + metadata API)
- พัฒนาเร็ว (1-2 วันถึง live)
- พร้อมรับ feature เพิ่ม (LINE OA integration, contact form, AI editor ใน Phase 2/3)
- พร้อม integrate กับ ecosystem MoonieX (อาจ share design tokens)

## Decision

ใช้ **Next.js 16 + TypeScript strict + Tailwind CSS 4** บน **pnpm**.

- App Router (ไม่ใช้ Pages Router)
- TypeScript strict — ห้าม `any`
- Tailwind utility-first, mobile-first
- pnpm สำหรับ workspace + lockfile

ไม่รวม:
- Database / ORM (ดู [[0002-no-db-mvp|ADR-0002]])
- Auth library (ไม่มี user account ใน MVP)
- State management library (RSC + URL state พอ)
- Component library (shadcn-style copy-paste ตามต้องการ)

## Consequences

**Positive**
- เร็วต่อ static + Edge serving (Vercel native)
- TypeScript strict catch bug แต่เนิ่นๆ
- Tailwind = ไม่มี CSS-in-JS overhead, design tokens ผ่าน config
- App Router รองรับ RSC → bundle เล็ก
- เปิด path สู่ server actions / API routes ตอน Phase 2 โดยไม่ต้อง migrate framework

**Negative**
- Next.js 16 = framework ใหม่, ecosystem ยังไม่ stable เท่า Next 14
- App Router learning curve ถ้าเคยใช้ Pages Router
- Tailwind 4 = breaking changes from 3 (new engine, new config format)

## Alternatives

- **Astro** — เร็วกว่าสำหรับ pure static, แต่ migration ไป interactive features ยากกว่า. ไม่เลือกเพราะ Phase 2/3 ต้องการ server actions
- **Remix** — Good for forms, but Vercel native ของ Next.js ดีกว่า
- **Vite + React SPA** — SEO weak (CSR-by-default), ต้องเพิ่ม SSR ทีหลังยาก
- **Plain HTML + CSS** — เร็วสุด แต่ scale ไม่ได้, ต้อง migrate ภายในเดือน

## See Also

- [[../10-Architecture/Overview]]
- [[0002-no-db-mvp]]
- [[0003-deploy-vercel]]
