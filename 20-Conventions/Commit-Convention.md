---
title: Commit Convention
tags: [conventions, git]
---

# Commit Convention

ใช้ **Conventional Commits**.

## Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

## Types

| Type | Use |
|------|-----|
| `feat` | new feature |
| `fix` | bug fix |
| `docs` | docs only (รวม wikis) |
| `style` | format, no code change |
| `refactor` | restructure, no behavior change |
| `perf` | performance |
| `test` | tests |
| `chore` | tooling, deps, config |
| `wiki` | wiki-only update (custom) |

## Scope examples

`webapp`, `wikis`, `landing`, `hero`, `pricing`, `portfolio`, `ui`, `seo`

## Examples

```
feat(landing): add hero section
fix(pricing): align tier card heights on mobile
wiki(architecture): document landing page structure
chore: bump next to 16.2.6
```

## Rules

- Subject ภาษาอังกฤษ, lowercase, ไม่มี period
- Body อธิบาย "why" ไม่ใช่ "what"
- Breaking change → `feat!:` หรือ footer `BREAKING CHANGE:`

## See Also

- [[Code-Style]]
- [[../50-Workflows/Dev-Workflow]]
