---
title: "ADR-0005: Brand architecture — House of Brands under MoonieX"
tags: [adr, brand, business]
status: Accepted
date: 2026-05-23
---

# ADR-0005 — Brand architecture: House of Brands under MoonieX

## Status

Accepted

## Context

MoonieX (บริษัทแม่, จดทะเบียนแล้ว) มี product line หลักด้าน trading. การเพิ่ม WarpClip
(บริการตัดต่อ short-form video) ภายใต้ MoonieX มี 2 ทางออกแบบ:

1. **Branded House** (Apple → Apple TV, Apple Music)
   - ชื่อนำหน้าด้วย MoonieX → "MoonieX Clip" / "MoonieX Studio"
   - ใช้พลังแบรนด์แม่ในการ launch
   - แต่ผูกติดกับ trading audience

2. **House of Brands** (P&G → Tide, Pampers; Meta → Instagram, WhatsApp)
   - WarpClip = แบรนด์ลูกอิสระ
   - มี positioning + marketing ของตัวเอง
   - Spin-off / acquisition exit ง่าย

WarpClip ลูกค้าเป้าหมาย = content creator / SMB ทั่วไป (ไม่ใช่ trader).
ผูกชื่อ MoonieX อาจ confuse audience + ผูก destiny แบรนด์.

## Decision

ใช้ **House of Brands** — WarpClip เป็นแบรนด์อิสระ.

- ชื่อแบรนด์: `WarpClip` (ไม่มี "MoonieX" นำหน้า)
- Domain: `warpclip.com` (ไม่ใช่ subdomain ของ MoonieX)
- Marketing voice / target audience: independent
- Legal entity: ภายใต้ MoonieX (parent company), ไม่ต้องตั้งบริษัทใหม่ในเฟส MVP
- Footer ของ webapp: ระบุ "a brand of MoonieX" subtle (กฎหมาย + credibility)

## Visual Cohesion

ถึงแม้แบรนด์อิสระ — ใช้ design language ร่วมกับ MoonieX ecosystem ได้:

- Dark theme + tech vibe (เหมือน MoonieX trading dashboard)
- Sci-fi / cosmic accents (โทน "warp" สื่อความเร็ว + space)
- Type system อาจ share (TBD ตอน design phase)

## Consequences

**Positive**
- WarpClip ไม่ผูก trading audience — เปิดตลาด content creator ได้กว้าง
- Spin-off / sale ง่าย (ไม่ rebrand)
- MoonieX brand ไม่ contaminate ถ้า WarpClip มีปัญหาคุณภาพ
- Cross-promotion ได้ทั้งคู่ (footer mention + landing page CTA)

**Negative**
- ขาด "halo effect" ของ MoonieX ตอน launch — ต้อง build trust จากศูนย์
- Marketing budget แยก (ไม่ share assets, copy)
- Domain ต้อง register แยก (`warpclip.com` $10/yr)

## Operational Rules

1. **`warpclip.com` deploy แยกจาก `mooniex.com`** — Vercel project ต่างกัน
2. **Footer ของ webapp:** `© 2026 MoonieX Company · WarpClip is a service of MoonieX` (subtle)
3. **Social media account:** WarpClip-branded (IG, TikTok) — ไม่ใช้ MoonieX accounts
4. **Email domain:** `@warpclip.com` (TBD) หรือ shared `@mooniex.com` ใน MVP
5. **เปลี่ยน brand strategy → ต้อง ADR ใหม่** (supersede อันนี้)

## Alternatives

- **Branded House (MoonieX Clip)** — ตัดออกเพราะ audience mismatch
- **Hybrid (Powered by MoonieX)** — กลาง, แต่ commit ไม่ครบ
- **Independent + no MoonieX mention เลย** — เสีย credibility ของ parent company

## See Also

- [[../10-Architecture/Overview#brand-architecture]]
- [[../30-Domain/Glossary]]
