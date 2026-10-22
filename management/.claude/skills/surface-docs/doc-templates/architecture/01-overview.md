# Architecture — overview

<!-- Ảnh chụp kiến trúc ở mức agent cần để đặt code đúng chỗ. Không phải tài liệu marketing. -->

## Mục đích & ranh giới

- **Làm:** {{surface chịu trách nhiệm gì}}
- **Không làm:** {{thuộc surface nào khác}}
- **Consumer:** {{ai dùng surface này}}

## Shape

- **Kiểu kiến trúc:** {{vd. layered: routes → controllers → services → data access}}
- **Quy tắc phụ thuộc:** {{ai được import ai; vd. service không import controller}}

## Cấu trúc thư mục

```text
{{cây thư mục rút gọn, 2–3 cấp, mỗi folder một chú thích}}
```

**Code mới đặt ở đâu:**

| Loại | Vị trí |
| --- | --- |
| {{vd. endpoint mới}} | {{vd. src/routes/<resource>.ts}} |

## Runtime & hạ tầng

- **Runtime:** {{vd. Node 20 / JVM 21 / iOS 17+}}
- **Phụ thuộc ngoài:** {{DB, cache, queue, storage, third-party API}}
- **Config:** {{nguồn config; danh sách tên env var chính ở security.md / .env.example}}
- **Môi trường:** {{local / staging / prod và khác nhau ở đâu}}

<!-- Section riêng của profile chèn tại đây (xem profiles/<profile>.md → Sections). -->

## Ràng buộc cứng

- {{điều tuyệt đối không được làm, vd. không sửa migration đã merge}}

Nguồn: {{code:… / owner / default}}

*Cập nhật lần cuối: {{YYYY-MM-DD}} — đổi nội dung thì cập nhật luôn dòng này.*
