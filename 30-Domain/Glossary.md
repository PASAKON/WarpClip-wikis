---
title: Domain Glossary
tags: [domain, glossary]
---

# Domain Glossary

Term ทั้งหมดของ WarpClip — product, business, technical. 1 term = 1 row.

ดูกฎเพิ่ม/แก้ที่ [[../20-Conventions/Wiki-Style#8-Glossary|Wiki-Style §8]].

---

## Product

| Term | ความหมาย |
|------|----------|
| **Short-form video** | คลิปวิดีโอความยาว ≤ 60 วินาที — TikTok, IG Reels, YouTube Shorts |
| **Tier** | ระดับบริการ (Basic / Pro / Premium) ในหน้า Pricing |
| **Portfolio item** | ผลงานตัดต่อ 1 ชิ้น — มี thumbnail + before/after + tier ที่ใช้ |
| **Turnaround time** | ระยะเวลาตั้งแต่รับงาน → ส่งงาน (per tier) |
| **Revision** | จำนวนครั้งที่ลูกค้าขอแก้ไขได้ฟรี (per tier) |

## Business

| Term | ความหมาย |
|------|----------|
| **MoonieX** | บริษัทแม่ (parent company) จดทะเบียนแล้ว, เจ้าของ trading product line |
| **WarpClip** | sub-brand ของ MoonieX สำหรับบริการตัดต่อ short-form video |
| **House of Brands** | brand architecture model ที่บริษัทแม่มีหลาย brand ลูกอิสระ (ดู [[../40-Decisions/0005-mooniex-house-of-brands\|ADR-0005]]) |
| **LINE OA** | LINE Official Account — ช่องทาง lead capture หลักของ MVP |
| **Lead** | ผู้สนใจที่ติดต่อผ่าน CTA (LINE OA, contact form ในอนาคต) |
| **Editor** | คนตัดต่อ (in-house, ตอนนี้มีแล้ว) |

## Technical

| Term | ความหมาย |
|------|----------|
| **App Router** | Next.js routing system (`app/` dir) — ใช้แทน Pages Router |
| **RSC** | React Server Component — render ที่ server, ส่ง HTML ลง client |
| **CTA** | Call-to-action button — ปุ่มหลักที่นำไปสู่ LINE OA |
| **OG image** | Open Graph image — รูป preview เวลา share link |
| **Design token** | ค่า design (color, spacing, font) ที่ shared ระหว่าง webapp ↔ design |
| **Polyrepo** | architecture ที่แยก code/wiki/design เป็น 3 repo (ดู [[../40-Decisions/0004-polyrepo-split\|ADR-0004]]) |
| **MVP** | Minimum Viable Product — landing page เฟสแรก, no DB |

## Technical Alias

| Alias | Full term |
|-------|-----------|
| `cn` | classnames utility (`@/lib/utils`) |
| `tw` | Tailwind CSS |
| `pnpm` | performant npm — chosen package manager |
| `ADR` | Architecture Decision Record |
