# Security

<!-- KHÔNG BAO GIỜ ghi giá trị secret vào file này. Chỉ tên biến + mục đích + nơi quản lý. -->

## Secret & config

| Biến | Mục đích | Quản lý ở |
| --- | --- | --- |
| {{DATABASE_URL}} | {{kết nối DB}} | {{.env local / CI secret / vault}} |

- Không commit secret. Mẫu ở `{{.env.example}}`.

## Auth

- {{tóm tắt; chi tiết contract ở [architecture/03-contracts.md](architecture/03-contracts.md#auth--permission)}}

## Dữ liệu nhạy cảm

- **Loại:** {{PII, payment, token, health…}}
- **Quy tắc:** {{không log; mã hoá at rest; mask khi hiển thị}}

## Việc cần hỏi trước

- Đổi cơ chế auth, permission, CORS, mã hoá, thư viện crypto → flag cho PM.

Nguồn: {{code:… / owner / default}}

*Cập nhật lần cuối: {{YYYY-MM-DD}} — đổi nội dung thì cập nhật luôn dòng này.*
