# Backlog 0001: Thêm settings screen cho web app

> **Ticket ví dụ** đi kèm starter để minh hoạ định dạng. Xoá nó khi bạn mở ticket thật đầu tiên.

**Status:** Open
**Priority:** Medium
**Surfaces:** app
**Opened:** 2026-01-01
**Reported by:** Owner

## Context

Web app chưa có chỗ nào để người dùng đổi account preference (theme, notification). Đây là một screen mới, người dùng thấy.

**Auto-persisted:** No — Owner đã duyệt. (Fail rubric: đây là *Addition*, không phải correction, nên cần được gật trước khi persist.)

## Plan

- `app`: thêm route `/settings` với form preference (toggle theme, opt-in notification). Lưu qua preferences endpoint có sẵn; không thêm gì ở backend.
- Out of scope: thêm *field* preference mới vào API — đó là đổi contract, cần thì mở ticket riêng.

## Outcome

<!-- PM điền sau khi thực thi, dựa trên evidence của sub-agent. -->

- Files changed: `<path:line>`
- Verified via: `<build / test kèm số lượng / manual smoke>`
- Evidence: `<thứ chứng minh nó chạy>`
- Harness delta: `<ticket này dạy hệ thống điều gì, hoặc "None">`
