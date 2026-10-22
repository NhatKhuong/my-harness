# Architecture — data model

<!-- Surface không sở hữu persistence → giữ heading, ghi "N/A — <lý do>" ở từng mục. -->

## Datastore

- **Loại:** {{vd. Postgres 16 / Room / Core Data / N/A}}
- **Truy cập qua:** {{ORM / query builder / repository}}
- **Tổ chức:** {{vd. mỗi bounded context một schema}}

## Entity chính

| Entity | Mô tả | Khoá / quan hệ chính |
| --- | --- | --- |
| {{User}} | {{…}} | {{id (uuid); 1–n Session}} |

<!-- Chỉ entity agent hay đụng tới. Schema đầy đủ → link file schema/migration, không chép lại. -->
Schema đầy đủ: {{đường dẫn schema / migrations}}

## Migration policy

- **Tool:** {{vd. Prisma Migrate / Flyway / Alembic}}
- **Quy tắc:** {{vd. forward-only; không sửa migration đã merge; mỗi migration một thay đổi}}
- **DB schema là contract** → mọi thay đổi schema phải qua PM (xem [03-contracts.md](03-contracts.md)).

## Transaction & tính nhất quán

- {{vd. transaction mở ở service layer; không gọi API ngoài trong transaction}}

Nguồn: {{code:… / owner / default}}

*Cập nhật lần cuối: {{YYYY-MM-DD}} — đổi nội dung thì cập nhật luôn dòng này.*
