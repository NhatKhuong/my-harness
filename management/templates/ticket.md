# Ticket templates

Copy một trong hai bản dưới vào `backlog/NNNN-slug.md` hoặc `bugs/NNNN-slug.md`. Bản **light** cho việc nhỏ; bản **PRD** cho việc nặng, nhiều phase.

---

## Bản light (việc nhỏ)

```markdown
# Backlog/Bug NNNN: <tiêu đề một dòng>

**Status:** Open | In Progress | Awaiting Owner | Done
**Priority:** Low | Medium | High | Critical
**Surfaces:** api / app / android
**Opened:** YYYY-MM-DD
**Reported by:** Owner | user | monitoring

## Context

<1–3 câu: vì sao việc này quan trọng, phát hiện ra thế nào>
**Auto-persisted:** Yes — rubric (1 surface / correction / no call / internal) · hoặc · No — Owner approved

## Plan

- <agent>: <làm gì>
- Out of scope: <thứ cố ý KHÔNG chạm>

## Outcome

<!-- điền sau khi thực thi -->
- Files changed: `<path:line>`
- Verified via: <build / test kèm số lượng / manual smoke>
- Evidence: <thứ chứng minh nó chạy>
- Harness delta: <ticket này dạy hệ thống điều gì, hoặc "None">
```

---

## Bản PRD (việc nặng)

```markdown
# Backlog NNNN: <title>

**Status:** …  ·  **Priority:** …  ·  **Surfaces:** …  ·  **Opened:** YYYY-MM-DD
**Epic:** <tên, nếu thuộc một Epic>

## Context / problem
Giải quyết cái gì, vì sao là bây giờ. Constraint nào không hiển nhiên.

## Goal & non-goals
- Goal: …
- Non-goal (cố ý ngoài scope): …

## Plan (theo phase)
1. **Phase 1 — <agent>:** …
2. **Phase 2 — <agent>:** …

## Contract
Mọi shape consumer phụ thuộc — tên field/param, permission, URL — chốt ở đây TRƯỚC khi chạy song song.

## Outcome
<!-- điền dần theo từng phase -->
- Phase 1: <file, evidence>
- Harness delta: …
```
