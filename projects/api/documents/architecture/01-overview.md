# Architecture — overview

*Stub ví dụ. Mô tả service này thật sự được build thế nào; agent sở hữu đọc file này trước tiên.*

- **Shape:** REST API — routes → controllers → services → data access.
- **Data:** Postgres, mỗi bounded context một schema; migration có version và chỉ đi tiến (forward-only).
- **Auth:** token-based; permission kiểm tại ranh giới route.
- **Config:** biến môi trường, ghi trong `.env.example`; không bao giờ commit secret.

*Cập nhật lần cuối: 2026-01-01 — đổi nội dung thì cập nhật luôn dòng này.*
