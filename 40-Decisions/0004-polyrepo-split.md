---
title: "ADR-0004: Polyrepo split — webapp + wikis + design"
tags: [adr, repo, architecture]
status: Accepted
date: 2026-05-23
---

# ADR-0004 — Polyrepo split: webapp + wikis + design

## Status

Accepted

## Context

WarpClip มี 3 artefact หลักที่ lifecycle ต่างกัน:

| Artefact | เปลี่ยนบ่อยแค่ไหน | Trigger Vercel deploy? | คนแก้ |
|----------|-------------------|------------------------|-------|
| Code (Next.js app) | ทุก feature | ใช่ | dev |
| Wiki (Obsidian docs + ADR) | ทุก decision | ไม่ใช่ | dev + lead |
| Design (mockups, assets) | per design sprint | ไม่ใช่ | designer |

ถ้ารวมใน monorepo:
- Wiki edit → Vercel build trigger (waste)
- Design asset commit → push lock files / dependencies (noise)
- Git history เละ — code/doc/design ผสมกัน
- Branch protection rule แยกไม่ได้

LungNote ใช้ pattern นี้สำเร็จ ([ADR-0002](https://github.com/PASAKON/LungNote-wikis/blob/main/40-Decisions/0002-split-repos-webapp-wikis-design.md)).

## Decision

แยกเป็น **3 GitHub repos**:

| Repo | Purpose | Vercel hooked? |
|------|---------|----------------|
| `PASAKON/WarpClip-webapp` | Next.js app | ใช่ — auto deploy |
| `PASAKON/WarpClip-wikis` | Obsidian vault, ADR | ไม่ |
| `PASAKON/WarpClip-design` | Mockups, assets, brief | ไม่ |

Local layout = parent dir + 3 git repos:

```
/Users/gob/WarpClip Projects/
├── CLAUDE.md       (parent, not in any git)
├── README.md
├── .gitignore
├── webapp/         ← git repo
├── wikis/          ← git repo
└── design/         ← git repo
```

Parent dir `WarpClip Projects/` ไม่ใช่ git repo — เพียง folder hub.

## Consequences

**Positive**
- Wiki/design edit ไม่ trigger Vercel rebuild
- Git history สะอาด — แต่ละ repo single concern
- Branch protection + access control แยก (designer ไม่ต้อง write code repo)
- ลด lock file conflict (pnpm-lock อยู่ใน webapp เท่านั้น)

**Negative**
- Cross-repo PR coordination → ต้อง manual link
- Wiki update ตามไม่ทัน code change ได้ → discipline ต้องแข็ง (workflow §4.3)
- Clone 3 ครั้ง = onboarding ช้าลง (1 นาที)

## Coordination Rules

ดู [[../50-Workflows/Multi-Repo-Workflow#4-conflict-prevention|Multi-Repo §4]] + [[../20-Conventions/Wiki-Style#10-Cross-Repo-Discipline|Wiki-Style §10]]:

1. Change กระทบ ≥ 2 repo → PR wiki ก่อน, link cross-repo ใน PR description
2. Merge wiki ก่อน → code merge ทีหลัง (กัน doc lag)
3. ห้ามใส่ `// TODO: wiki` ทิ้งไว้นาน — ภายใน 1 working day ต้อง update

## Alternatives

- **Single monorepo** — ง่ายต่อ atomic commit แต่ Vercel trigger ผิด, history เละ
- **Monorepo + Turborepo** — overkill สำหรับ 3 artefact ที่ไม่ share dependency
- **Submodule** — fragile, สร้าง confusion เวลา onboard

## See Also

- [[../50-Workflows/Multi-Repo-Workflow]]
- [[../20-Conventions/Wiki-Style#10-Cross-Repo-Discipline]]
