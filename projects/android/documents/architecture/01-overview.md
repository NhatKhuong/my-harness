# Architecture — overview

*Stub ví dụ. Mô tả client này thật sự được build thế nào; agent sở hữu đọc file này trước tiên.*

- **Shape:** MVVM — Compose UI → ViewModel → repository → service `api`.
- **State:** một chiều; UI observe immutable state do ViewModel expose.
- **DI:** constructor injection; một graph duy nhất wire ở ranh giới app.
- **Data:** typed model phản chiếu API contract; decode phòng thủ.

*Cập nhật lần cuối: 2026-01-01 — đổi nội dung thì cập nhật luôn dòng này.*
