---
title: "ADR-0002: No database in MVP phase"
tags: [adr, scope, mvp]
status: Accepted
date: 2026-05-23
---

# ADR-0002 — No database in MVP phase

## Status

Accepted

## Context

WarpClip MVP = landing page เพื่อคุยลูกค้า. ยังไม่มี:
- User account / login
- Project tracking
- Order management
- Content upload

Lead capture ทำผ่าน **LINE OA** (external, manual handoff). ไม่มี data ที่ webapp ต้องเก็บ.

การเพิ่ม DB ใน MVP จะ:
- เพิ่ม latency (cold start, connection pool)
- เพิ่ม cost (Supabase/Vercel Postgres tier)
- เพิ่ม attack surface (RLS, auth, secrets)
- ทำให้ออก MVP ช้าลง 2-3 วัน

## Decision

**MVP phase ไม่มี database.** Content + portfolio + pricing = hardcoded JSON/TypeScript ใน repo.

- Lead capture → LINE OA URL (CTA button)
- Portfolio items → static array in `app/portfolio/data.ts`
- Pricing tiers → static array in `app/pricing/data.ts`
- Analytics → Vercel Web Analytics (no schema needed)

## Consequences

**Positive**
- Deploy ภายในวัน
- Zero secrets ที่ต้อง manage
- Zero attack surface สำหรับ injection / RLS bugs
- Content update = code commit (git history = audit log)

**Negative**
- Non-dev update content ไม่ได้ — ต้องผ่าน PR
- ไม่มี analytics เชิง business (เช่น lead conversion funnel)
- ต้อง migrate ตอนเพิ่ม user accounts (Phase 2)

## Migration Path

ถ้าต้องเพิ่ม DB → เขียน ADR ใหม่ที่ supersede อันนี้:
- ระบุเทคโนโลยี (Supabase / Vercel Postgres / Turso / etc.)
- ระบุ schema initial
- ระบุ migration tool (Drizzle / Prisma / SQL files)
- Update [[../10-Architecture/Overview]]

## Alternatives

- **Supabase from day 1** — overkill, ไม่มี data จะเก็บ
- **Headless CMS (Sanity/Contentful)** — เพิ่ม dep ที่ไม่จำเป็นใน MVP, ค่าใช้จ่ายฟรี-tier มี limit
- **Notion API as CMS** — slow, rate-limited, ลูกค้า/editor ไม่ได้ใช้ Notion อยู่แล้ว

## See Also

- [[../10-Architecture/Overview]]
- [[0001-stack-nextjs-typescript-tailwind]]
