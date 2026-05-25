---
title: "CFO Budget Workflow (WarpClip)"
tags: [workflow, finance, cfo, budget]
status: Active
date: 2026-05-25
---

# CFO Budget Workflow — WarpClip

WarpClip ปัจจุบัน = landing page เปล่า ([[../40-Decisions/0002-no-db-mvp|ADR-0002]]
no DB MVP). cost ≈ 0. เมื่อเริ่ม build v2 (video editing service)
จะมี cost surface ใหม่ → CFO เข้ามา.

## เรียก CFO เมื่อไหร่ (WarpClip-specific)

**ต้องเรียก ก่อน supersede ADR-0002**
- เพิ่ม DB (Supabase / อื่น)
- เปิด LINE Messaging integration จริง (ตอนนี้ env reserved แต่ unused)
- เพิ่ม video processing API (FAL / Replicate / Modal / own GPU)
- เพิ่ม payment gateway (Omise / Stripe)
- launch ad ผ่าน Meta / TikTok / YouTube

**ต้องเรียกตลอดอายุ project**
- ต่อ / เปลี่ยน custom domain `warpclip.com`
- เปลี่ยน Vercel plan tier
- เพิ่ม brand asset / video gen ผ่าน paid API

**ไม่ต้องเรียก**
- copy / design tweak บน landing
- เพิ่ม section / animation (ใช้ motion lib ฟรี)
- เปลี่ยน meta tag / SEO

## CFO ทำอะไรให้ WarpClip

- monitor (ปัจจุบัน ≈ $0/เดือน — แค่ Vercel + domain)
- ก่อน v2 launch: ขอ unit economics จาก CGO (cost per video / pricing tier)
- review เลือก video API provider (FAL vs Replicate vs Modal vs self-host)
- ต่อสัญญา / ยกเลิก subscription รายเดือน
- monthly close ของ WarpClip line item

## วิธีเรียก

- ส่ง message: `to: cfo, subject: [SPEND][WarpClip] <ขออะไร>`

## Dashboard

CFO API monitor `/admin/cfo` จะ rollup ของ WarpClip ภายใต้
project=warpclip-webapp.

## House of Brands constraint

WarpClip เป็น sub-brand ของ MoonieX ([[../40-Decisions/0005-mooniex-house-of-brands|ADR-0005]]).
- billing entity ยังอยู่ใต้ MoonieX
- separate ledger line ใน CFO books → ตอน spin-off / sell ใช้ตัวเลขแยกได้
- WarpClip budget envelope ตั้งต่างหากจาก mooniex-* projects
