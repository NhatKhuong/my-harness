# Architecture — overview

*Stub ví dụ. Mô tả client này thật sự được build thế nào; agent sở hữu đọc file này trước tiên.*

- **Shape:** component tree → view theo route → data layer gọi service `api`.
- **State:** mặc định là state cục bộ của component; chỉ dùng shared store ở chỗ thật sự xuyên suốt.
- **Styling:** Tailwind utility; ưu tiên design token dùng chung hơn giá trị ad-hoc.
- **Data:** typed client cho API; không hardcode response shape mà API làm chủ.

*Cập nhật lần cuối: 2026-01-01 — đổi nội dung thì cập nhật luôn dòng này.*
