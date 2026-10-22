# Architecture — contracts

**Contract = bất cứ thứ gì consumer phụ thuộc vào.** Đổi bất kỳ mục nào dưới đây là **quyết định do PM gate và Owner duyệt**, không phải sửa cục bộ. Cần đổi → dừng, flag cho PM.

<!-- Mọi mục trong file này phải có nguồn owner hoặc code: đã được Owner xác nhận. Không dùng default. -->

## Contract surface này cung cấp

| Loại | Mục | Consumer | Ghi chú |
| --- | --- | --- | --- |
| {{API / URL / event / CLI flag / deep link / schema}} | {{vd. GET /v1/users/:id}} | {{app, ios}} | {{…}} |

<!-- Có spec formal (OpenAPI / proto / GraphQL) → spec đó là source of truth; link ở đây, bảng chỉ cần liệt kê. -->
Spec formal: {{đường dẫn hoặc "None"}}

## Shape chung

- **Success:** {{vd. trả resource trực tiếp, không envelope}}
- **Error:** {{vd. `{ "error": { "code": "string", "message": "string", "details"?: {} } }`}}
- **Status code:** {{quy ước dùng mã nào cho trường hợp nào}}
- **Pagination / filter / sort:** {{…}}

## Auth & permission

- **Cơ chế:** {{vd. Bearer JWT; session cookie}}
- **Kiểm tại:** {{vd. ranh giới route / guard}}
- **Ma trận quyền:** {{role → được làm gì, hoặc link}}

## Versioning & breaking change

- **Cách version:** {{vd. prefix /v1}}
- **Breaking change là:** xoá / đổi tên field, đổi kiểu, đổi status code, siết validation, đổi quyền.
- **Additive (vẫn phải báo PM):** thêm field optional, thêm endpoint.

## Contract surface này phụ thuộc

| Nhà cung cấp | Mục | Docs |
| --- | --- | --- |
| {{api}} | {{POST /sessions}} | {{../../api/documents/architecture/03-contracts.md}} |

Nguồn: {{code:… / owner}}

*Cập nhật lần cuối: {{YYYY-MM-DD}} — đổi nội dung thì cập nhật luôn dòng này.*
