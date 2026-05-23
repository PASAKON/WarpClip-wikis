---
title: Multi-Repo Workflow
tags: [workflow, multi-repo, multi-dev]
---

# Multi-Repo Workflow

WarpClip = 3 GitHub repos แยก ([[../40-Decisions/0004-polyrepo-split|ADR-0004]]):

| Repo | Local dir | Purpose |
|------|-----------|---------|
| `PASAKON/WarpClip-webapp` | `webapp/` | Next.js app (deploys to Vercel) |
| `PASAKON/WarpClip-wikis` | `wikis/` | Obsidian vault — docs + ADR |
| `PASAKON/WarpClip-design` | `design/` | Design briefs, assets, tokens |

หน้านี้ = workflow สำหรับ dev คนเดียว/หลายคนทำงานข้าม repo ลื่นไหล + กัน conflict.

---

## 1. Workspace Layout

```
/Users/gob/WarpClip Projects/   # parent dir (no git, just folder)
├── CLAUDE.md                   # root entry (parent-folder file)
├── README.md
├── webapp/                     ← clone PASAKON/WarpClip-webapp
├── wikis/                      ← clone PASAKON/WarpClip-wikis
└── design/                     ← clone PASAKON/WarpClip-design
```

แต่ละ subfolder = git repo แยก. `cd webapp && git status` ไม่เห็นไฟล์ของ wikis/design.

---

## 2. First-time Setup

### 2.1 Tools

```bash
brew install pnpm gh
gh auth login
pnpm add -g vercel  # ตอน deploy เท่านั้น
```

### 2.2 Access

ขอ org admin (PASAKON) ให้:
- Add เป็น collaborator ทั้ง 3 repo
- Add เป็น Vercel team member ตอนเริ่ม deploy

(MVP phase: ไม่มี DB / secrets — ดู [[../40-Decisions/0002-no-db-mvp|ADR-0002]])

### 2.3 Bootstrap

```bash
mkdir -p "/Users/gob/WarpClip Projects" && cd "/Users/gob/WarpClip Projects"
git clone https://github.com/PASAKON/WarpClip-webapp.git webapp
git clone https://github.com/PASAKON/WarpClip-wikis.git wikis
git clone https://github.com/PASAKON/WarpClip-design.git design
cd webapp && pnpm install
```

### 2.4 Verify

```bash
cd webapp
pnpm dev          # http://localhost:3000
pnpm build        # lint + typecheck + build
```

---

## 3. Daily Loop

### 3.1 Start — pull all

```bash
for d in webapp wikis design; do
  (cd "$d" && git checkout main && git pull --rebase --autostash)
done
```

### 3.2 Branch (ห้าม commit ตรง main)

```bash
cd <repo>
git checkout -b <type>/<scope>-<short>
# เช่น
git checkout -b feat/hero-section
git checkout -b wiki/adr-line-integration
git checkout -b design/pricing-mockup
```

ดู [[../20-Conventions/Commit-Convention]].

### 3.3 Commit

```bash
git add <files>
git commit -m "feat(hero): add WarpClip headline + CTA"
```

แก้ doc คู่กัน → commit ที่ wiki repo แยก (section 4.3).

### 3.4 Push + PR

```bash
git push -u origin HEAD
gh pr create --fill --base main
```

ทุก PR target → `main`. Branch protection: linear history, no force-push.

### 3.5 Merge

```bash
gh pr merge --rebase --delete-branch
```

### 3.6 After merge — pull back

```bash
git checkout main && git pull --rebase --autostash
```

---

## 4. Conflict Prevention

### 4.1 Repo-level (GitHub branch protection)

ทั้ง 3 repos:
- `required_linear_history: true`
- `allow_force_pushes: false`
- `allow_deletions: false`

### 4.2 Dev-level discipline

- ห้าม commit ตรง main — branch + PR เสมอ
- ก่อน push → rebase บน latest main: `git fetch origin main && git rebase origin/main`
- PR เปิดนาน → rebase ทุก 1-2 วัน กัน drift
- Long-running branch ห้ามเกิน 1 สัปดาห์ — split เป็น multiple PR
- ใช้ `git push --force-with-lease` เท่านั้น (ห้าม `--force`)

### 4.3 Cross-repo coordination

ถ้า change กระทบ ≥ 2 repo:

1. เปิด PR ใน wiki repo **ก่อน** code merge
2. ใส่ link ใน PR description ของ webapp PR ไป wiki PR และกลับ
3. Merge wiki ก่อน → code merge ทีหลัง (กัน doc lag)

```markdown
## Related
- Wiki: [WarpClip-wikis#42](https://github.com/PASAKON/WarpClip-wikis/pull/42)
```

### 4.4 Lock files

`pnpm-lock.yaml` (webapp) = critical file ที่ conflict บ่อยสุด.

- ห้ามแก้มือ
- ถ้า conflict → ลบ `pnpm-lock.yaml`, รัน `pnpm install` ใหม่, commit
- ห้ามใช้ `npm` หรือ `yarn` ใน repo นี้

---

## 5. Env / Secrets

**MVP phase:** ไม่มี secrets — landing page เป็น static + LINE OA link (public URL).

เพิ่ม secret → ต้องผ่าน ADR (supersede [[../40-Decisions/0002-no-db-mvp|ADR-0002]] หรือ add new ADR สำหรับ secret store).

ถ้าถึงจุดนั้น:
- Source of truth = Vercel project env vars
- `.env.local` ห้าม commit (อยู่ใน `.gitignore`)
- `.env.example` keep updated เป็น checklist

---

## 6. Deploy

### 6.1 Auto-deploy (default)

`PASAKON/WarpClip-webapp:main` → push → Vercel webhook → build + deploy → live `warpclip.com`.

PR branch → push → Vercel preview deploy → URL ใส่ใน PR comment auto.

### 6.2 Manual deploy (fallback)

```bash
cd webapp
vercel --prod   # production
vercel          # preview
```

### 6.3 Rollback

```bash
vercel rollback <previous-deployment-url>
```

หรือ Vercel dashboard → Deployments → ⋯ → Promote.

---

## 7. Wiki-only / Design-only changes

### Wiki

```bash
cd wikis
git checkout -b wiki/<topic>
# แก้ใน Obsidian
git add . && git commit -m "wiki(<scope>): <subject>"
git push -u origin HEAD && gh pr create --fill
```

ไม่ trigger Vercel redeploy ✓.

### Design

```bash
cd design
git checkout -b design/<topic>
git add . && git commit -m "design(<scope>): <subject>"
git push -u origin HEAD && gh pr create --fill
```

ไม่ trigger Vercel redeploy ✓.

Design change → webapp ต้อง implement → cross-repo PR (section 4.3).

---

## 8. Troubleshooting

| ปัญหา | ทำยังไง |
|-------|---------|
| Branch protection reject merge ("not linear") | rebase แล้ว force-push-with-lease ก่อน merge |
| `pnpm-lock.yaml` conflict | ลบ lock + `pnpm install` + commit |
| Vercel build failed | ตรวจ Vercel dashboard → Deployments → log |
| Wiki refer path ใน webapp ที่ลบไปแล้ว | grep หา reference ใน wikis แก้ทันที |
| 3 repo ไม่ sync | `for d in webapp wikis design; do (cd $d && git pull --rebase); done` |

---

## See Also

- [[Dev-Workflow]] — code workflow ทั่วไป
- [[Wiki-Workflow]] — wiki edit workflow
- [[../20-Conventions/Commit-Convention]]
- [[../40-Decisions/0004-polyrepo-split]]
- [[../40-Decisions/0003-deploy-vercel]]
