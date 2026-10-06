# ADR — Architecture Decision Records

Mỗi decision một file, đánh số `NNNN-kebab-case-title.md`. Chúng ghi *vì sao* workspace và operating model của nó như hiện tại — cái "vì sao không làm cách khác" mà nếu không ghi sẽ mất trong chat.

Owner quản. Thứ gì xuyên suốt — operating model, convention của agent/workflow, permission, policy về tooling — thuộc về đây. Việc chiến thuật thì vào `../backlog/`; thư mục này dành cho decision.

**Đánh số:** `0000` là baseline hồi tố, ghi các nguyên tắc có sẵn. Decision về sau tăng dần từ `0001`. Khi một decision bị đảo ngược, đánh dấu ADR cũ `Superseded by NNNN` chứ không xoá.

**Decision chưa chốt** nằm trong [CANDIDATES.md](CANDIDATES.md).

## Template

```markdown
# ADR NNNN: <title>

**Status:** Proposed | Accepted | Superseded by <ADR-NNNN>
**Date:** YYYY-MM-DD
**Owner:** <the Owner>

## Context
Tình huống nào buộc phải quyết. Điều gì không hiển nhiên. Constraint nào quan trọng.

## Decision
Làm gì. Một hai câu, không úp mở.

## Alternatives considered
- **<alt>:** vì sao không

## Consequences
Cái gì dễ hơn. Cái gì khó hơn. Cái gì cần theo dõi.
```

## Index

| ADR | Decision | Date |
|---|---|---|
| [0000](0000-baseline-operating-principles.md) | Baseline — PM không code, board là file, đặt tên kebab/ALL_CAPS | 2026-01-01 |
