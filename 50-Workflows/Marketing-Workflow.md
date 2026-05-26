---
title: "Marketing Workflow (WarpClip)"
tags: [workflow, marketing, cmo, ads]
status: Active
date: 2026-05-25
---

# Marketing Workflow — WarpClip

WarpClip = sub-brand ของ MoonieX. CMO + CMO team บริหารงาน
brand / campaign / creative ที่ระดับ org (mooniex). หน้านี้ =
WarpClip-specific reference + ลิงก์ไป mooniex playbook กลาง.

## เรียก CMO เมื่อไหร่

**ต้องเรียก:**
- launch campaign บน paid channel (Meta / TikTok / Google / LINE OA paid)
- เปลี่ยน positioning / value prop / pricing message
- เขียน landing page copy / hero / pricing / CTA
- ทำ creative ใหม่ (image / video ad / static)
- สร้าง content calendar / blog / organic social
- ปรับ brand voice / asset

**ไม่ต้องเรียก:**
- typo fix ใน copy ที่ approved แล้ว
- รูปภาพ portfolio thumbnail (อยู่ใน design repo workflow)
- microcopy ใน Dev-Workflow scope

## CMO Team (org-level, shared across brands)

| Role | งานใน WarpClip context |
|------|------------------------|
| `content_strategist` | brand voice, copy, SEO, content calendar |
| `web_designer` | ad creative + landing visuals (ใช้ WarpClip-design tokens) |
| `ads_manager` | Meta + Google campaign execution (FB + IG primary) |

Coordinate (shared):
- `data_analyst` (under CGO) — performance / CAC / ROAS
- `developer` (under CTO) — Meta Pixel install, Conversions API,
  landing page integration

## Budget Authority (WarpClip override)

- **WarpClip per-campaign cap:** $300 lifetime (CEO-approved 2026-05-25)
- $0-50 = CMO standalone
- $51-200 = CMO + CFO notification (async)
- $201-300 = CFO explicit approval before launch
- > $300 = ต้อง supersede ADR

ดูเต็ม:
[`mooniex-llms-wiki / decisions/2026-05-25-marketing-budget-authority.md`](https://github.com/PASAKON/mooniex-llms-wiki/blob/main/decisions/2026-05-25-marketing-budget-authority.md)

## Brand Constraint (binding)

> 🔥 **ALWAYS read [[../10-Architecture/Brand-Truth|Brand-Truth.md]] first.**
> มันคือ canonical visual spec ที่ sourced จาก running code.
> ห้าม brief asset โดยไม่ผ่าน Brand-Truth verification protocol.

ทุก asset / copy ที่ออกในนาม WarpClip ต้องผ่าน:

1. **Brand v2.0 visual check** — B&W premium, single accent
   (sage `#6B8E6F` หรือ neon lime `#CCFF00`, designer ตัดสิน — ใช้สี
   เดียวต่อ asset set), ห้าม tri-color gradient, wordmark mixed-type
   (Geist Bold + Newsreader Italic). ดู [[../40-Decisions/0006-brand-v2-bw-premium|ADR-0006]].
2. **Voice check** — ห้าม em-dash ใน generated copy, ห้าม AI tropes
   (unlock / elevate / delve / in today's fast-paced world / in conclusion).
3. **Claim check** — ห้าม claim product feature ที่ไม่อยู่ใน wiki / glossary.
4. **CTA target** — CTA หลัก = LINE OA (Phase 1 MVP).

## Campaign Pipeline (WarpClip-specific)

```
CEO brief
  └─ CMO
      ├─ อ่าน wiki: ADR-0006 brand + Glossary tiers + CFO-Budget
      ├─ check ad account / Page / Pixel readiness
      ├─ ping CFO ถ้า spend > $50
      ├─ ping CTO ถ้าต้อง Pixel / new landing / new tracking
      ├─ delegate parallel:
      │    ├─ content_strategist → ad copy 3-5 variants ต่อ asset
      │    └─ web_designer → image/video creative 3-5 variants
      ├─ review brand fidelity (ADR-0006) + voice (em-dash, AI tropes)
      ├─ delegate ads_manager → launch test phase ($20/day x 5 days)
      ├─ handoff CGO → A/B + scale call (ต้องมี pre-registered metric)
      └─ campaign retro ADR ใน wiki/40-Decisions/
```

## Current FB Campaign State (2026-05-25)

| Item | Status |
|------|--------|
| Ad account `1374787969502492` | ACTIVE, USD, MCP-enabled |
| WarpClip FB Page | **❌ NOT created / NOT linked** (blocker) |
| Meta Pixel on warpclip.com | **❌ NOT installed** (CTO task pending) |
| Conversions API | ❌ pending (depends on Pixel) |
| Business Manager | ❌ pending (CTO task) |
| $300 budget approval | ⏳ pending CFO reply |
| web_designer task | `task-ed2d6dc1` ready to delegate after blockers clear |
| Meta MCP — official | ✓ Connected (~29 tools) |
| Meta MCP — mikusnuz/meta-ads-mcp | ✓ Installed (135 tools, token placeholder) |
| Marketing skills in `~/.claude/skills/` | 20 installed (voice-extractor, copywriting, page-cro, facebook-ads, etc.) |

## Out of Scope (Phase 1 MVP)

- DB-backed user accounts (per [[../40-Decisions/0002-no-db-mvp|ADR-0002]])
- Native LINE Messaging integration (env reserved, unused)
- AI-assisted editor demo (Phase 3 vision)
- Multi-language ad copy (EN-only ads ตอนนี้; TH-only landing)

## See Also

- [[../40-Decisions/0005-mooniex-house-of-brands|ADR-0005 House of Brands]]
- [[../40-Decisions/0006-brand-v2-bw-premium|ADR-0006 Brand v2.0]]
- [[CFO-Budget|CFO Budget Workflow]]
- [`mooniex-llms-wiki / playbooks/marketing.md`](https://github.com/PASAKON/mooniex-llms-wiki/blob/main/playbooks/marketing.md)
- [`mooniex-llms-wiki / playbooks/ads.md`](https://github.com/PASAKON/mooniex-llms-wiki/blob/main/playbooks/ads.md)
- [`mooniex-llms-wiki / decisions/2026-05-25-meta-ads-mcp-adoption.md`](https://github.com/PASAKON/mooniex-llms-wiki/blob/main/decisions/2026-05-25-meta-ads-mcp-adoption.md)
