---
title: Decision Log (ADR)
tags: [adr, index]
---

# Decision Log

Architecture Decision Records ของ WarpClip. 1 ADR = 1 ไฟล์ `NNNN-<slug>.md`.

กฎ ADR: ดู [[../20-Conventions/Wiki-Style#7-ADR-Rules|Wiki-Style §7]] + [[../50-Workflows/Wiki-Workflow#scenario-a|Wiki-Workflow Scenario A]].

## Index

| # | Title | Status | Date |
|---|-------|--------|------|
| [0001](0001-stack-nextjs-typescript-tailwind.md) | Stack: Next.js + TypeScript + Tailwind | Accepted | 2026-05-23 |
| [0002](0002-no-db-mvp.md) | No database in MVP phase | Accepted | 2026-05-23 |
| [0003](0003-deploy-vercel.md) | Deploy to Vercel | Accepted | 2026-05-23 |
| [0004](0004-polyrepo-split.md) | Polyrepo split: webapp + wikis + design | Accepted | 2026-05-23 |
| [0005](0005-mooniex-house-of-brands.md) | Brand architecture: House of Brands under MoonieX | Accepted | 2026-05-23 |
| [0006](0006-brand-v2-bw-premium.md) | Brand pivot: B&W premium with single accent (v2.0) | Accepted | 2026-05-23 |

## Numbering

- 4-digit, monotonic, ไม่ reuse แม้ deprecated/superseded
- ดูเลขถัดไปจากแถวสุดท้ายของตารางด้านบน

## Status

- **Proposed** — ยังไม่ตกลง, PR เปิด
- **Accepted** — ตกลงแล้ว, ใช้งานจริง
- **Deprecated** — ไม่ใช้แล้ว, ไม่มี ADR ใหม่มาแทน
- **Superseded by NNNN** — ADR ใหม่มาแทน

## Workflow

```bash
cp ../90-Templates/ADR-Template.md NNNN-<slug>.md
# กรอก Status: Proposed, Context, Decision, Consequences, Alternatives
# update index ด้านบน
# PR → review → merge → flip Status: Accepted
```
