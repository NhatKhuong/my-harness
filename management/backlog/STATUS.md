# Backlog Tracker

**Lane:** `Open` = PM/agent làm được ngay · `Awaiting Owner` = đã build/chốt xong, chờ Owner commit / deploy / review / quyết (KHÔNG phải việc đang mở) · `Epics` = initiative nhiều ticket, kiểm trước khi mở ticket anh em · `Closed`.

> Trước khi mở ticket mới: quét **Open + Awaiting Owner + Epics** tìm cùng surface/feature. Có cái liên quan thì mở rộng nó hoặc thêm phase — đừng mở ticket anh em. (pm-playbook → "Kỷ luật scoping")
>
> ID kế tiếp lấy từ folder (`ls backlog/ | grep -E '^[0-9]{4}' | sort | tail -1` — grep loại file này ra), không lấy từ file này: nó làm tay nên lag.

## Open — cần làm

| ID | Title | Priority | Status | Agents |
|----|-------|----------|--------|--------|
| [0001](0001-example-settings-screen.md) | Thêm settings screen cho web app *(ticket ví dụ — xoá khi bắt đầu thật)* | Medium | Open — ví dụ minh hoạ template | app |

## Awaiting Owner — commit / deploy / review / quyết định

*Đã build hoặc đã chốt; PM/agent không làm tiếp được.*

| ID | Title | Waiting on | Detail |
|----|-------|-----------|--------|
| — | — | — | — |

## Epics

Initiative nhiều ticket. **Trước khi mở ticket liên quan mới, gắn nó vào đây như một phase thay vì sinh ticket anh em lẻ.**

*(chưa có)*

## Closed

*(chưa có)*
