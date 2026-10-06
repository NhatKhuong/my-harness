# Architecture — overview

*Stub ví dụ. Mô tả client này thật sự được build thế nào; agent sở hữu đọc file này trước tiên.*

- **Shape:** SwiftUI view → observable view model → service layer gọi service `api`.
- **State:** một chiều; view observe immutable state; chỗ nào scoped model đủ dùng thì đừng rải singleton.
- **Concurrency:** `async/await`; giữ việc UI trên main actor.
- **Data:** `Codable` model phản chiếu API contract; decode phòng thủ để một giá trị lạ không làm crash app.

*Cập nhật lần cuối: 2026-01-01 — đổi nội dung thì cập nhật luôn dòng này.*
