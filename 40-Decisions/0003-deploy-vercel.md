---
title: "ADR-0003: Deploy to Vercel"
tags: [adr, deploy, hosting]
status: Accepted
date: 2026-05-23
---

# ADR-0003 — Deploy to Vercel

## Status

Accepted

## Context

WarpClip ต้องการ hosting ที่:
- Auto-deploy จาก GitHub push (no manual ops)
- Custom domain (`warpclip.com`)
- Preview deploy ต่อ PR (สำหรับ design review)
- CDN edge global (ลูกค้าไทย + future SE Asia)
- รองรับ Next.js 16 App Router native
- มี analytics tier ฟรี

## Decision

ใช้ **Vercel** เป็น hosting + CI/CD ของ `webapp/`.

- GitHub integration: push to `main` → auto deploy production
- PR branch → preview URL (auto-comment ใน PR)
- Custom domain `warpclip.com` ผ่าน Vercel DNS หรือ external A/CNAME
- Vercel Web Analytics enabled (built-in)
- Vercel Speed Insights enabled (Core Web Vitals)

## Consequences

**Positive**
- Zero CI/CD config — push = deploy
- Edge network = fast global delivery
- Next.js native = ไม่มี config drift ระหว่าง local กับ prod
- Preview URLs = ทำให้ design review ง่าย
- Free tier เพียงพอ MVP traffic

**Negative**
- Vendor lock-in (เปลี่ยน host = ปรับ deploy config)
- ค่าใช้จ่าย scale ตาม traffic (อาจ pricey ถ้า viral)
- Vercel Web Analytics ไม่ลึกเท่า GA / PostHog

## Alternatives

- **Cloudflare Pages** — ดี, แต่ Next.js App Router support ไม่ smooth เท่า Vercel
- **Self-host (VPS + nginx)** — overhead ops สูง, ไม่คุ้ม MVP phase
- **Netlify** — Next.js support ดี แต่ edge network ครอบคลุมน้อยกว่า Vercel
- **GitHub Pages** — ไม่รองรับ Next.js App Router (static-only export มี limit)

## See Also

- [[../10-Architecture/Overview]]
- [[0001-stack-nextjs-typescript-tailwind]]
- [[../50-Workflows/Multi-Repo-Workflow#6-deploy]]
