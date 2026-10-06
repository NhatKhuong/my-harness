# ADR 0000: Baseline operating principles

**Status:** Accepted
**Date:** 2026-01-01
**Owner:** <the Owner>

## Context

Đây là baseline hồi tố — những nguyên tắc workspace vốn đã chạy theo, được ghi lại để các decision sau có cái mà xây tiếp hoặc supersede.

## Decision

1. **PM không viết implementation code.** Nó điều phối và delegate; sub-agent build. PM tự code là bỏ qua convention của từng surface.
2. **Board là file.** Ticket, bug, decision là markdown đánh số trong `management/`, index bằng `STATUS.md`. `git` là backend.
3. **Decision được ghi lại, không phải được nhớ.** Lựa chọn xuyên suốt thành ADR ở đây, kèm lý do; đảo ngược thì supersede, không viết lại.
4. **`documents/` của sub-project là luật.** Agent sở hữu đọc trước; gặp xung đột thì dừng, không tự xoay.
5. **Đặt tên:** `kebab-case` cho file và slug, `ALL_CAPS` cho file index/status (`STATUS.md`, `CANDIDATES.md`).

## Alternatives considered

- **Board dạng database ngay từ đầu:** mạnh hơn (RBAC, cửa web, không lệch index), nhưng phải dựng hạ tầng thật và dựng tường tài khoản cho một workspace chỉ có một người. Hoãn tới khi có *team* — xem pm-playbook → "Nâng cấp lên team board".
- **PM cũng code:** nhanh hơn tại thời điểm đó, nhưng xoá mất ranh giới surface-convention, thứ giữ cho việc multi-agent còn nhất quán.

## Consequences

- Không cần setup, board có đủ lịch sử `git` — nhưng `STATUS.md` làm tay nên lag được, vì vậy ID lấy từ folder chứ không từ index.
- Operating model portable: lên database dùng chung chỉ đổi *chỗ board ở*, không đổi cách workspace chạy.
