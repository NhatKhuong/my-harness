# ADR — Hồ sơ quyết định kiến trúc

Mỗi quyết định được lưu trong một file riêng, có đánh số theo định dạng `NNNN-kebab-case-title.md`.

Các ADR ghi lại **lý do tại sao** workspace và mô hình vận hành được thiết kế như hiện tại. Đây là những lý do mang tính lâu dài, giải thích tại sao lựa chọn phương án này thay vì các phương án khác — những thông tin nếu không được ghi lại sẽ dễ bị thất lạc trong các cuộc hội thoại.

**Quyền quản lý:** Owner chịu trách nhiệm quản lý ADR.

Mọi quyết định có ảnh hưởng đến nhiều thành phần (cross-cutting) đều phải được ghi nhận tại đây, bao gồm:

- Mô hình vận hành.
- Quy ước về agent và workflow.
- Quyền hạn và phân quyền.
- Chính sách sử dụng công cụ.

Các công việc mang tính triển khai cụ thể (tactical work items) được quản lý trong `../backlog/`. Thư mục này chỉ dành cho các quyết định.

**Quy tắc đánh số:**

- `0000` là ADR ghi nhận hồi tố các nguyên tắc nền tảng đã tồn tại trước đó.
- Các quyết định mới được đánh số tăng dần, bắt đầu từ `0001`.
- Khi một quyết định bị đảo ngược hoặc thay thế, hãy cập nhật trạng thái ADR cũ thành `Superseded by NNNN` thay vì xóa file.

**Các quyết định đang chờ xử lý hoặc chưa được đưa ra** được ghi nhận trong [CANDIDATES.md](CANDIDATES.md).

## Mẫu ADR (Template)

```markdown
# ADR NNNN: <Tiêu đề>

**Trạng thái:** Proposed | Accepted | Superseded by <ADR-NNNN>
**Ngày:** YYYY-MM-DD
**Người phụ trách:** <Owner>

## Bối cảnh
Tình huống nào dẫn đến quyết định này?
Điểm nào không hiển nhiên?
Những ràng buộc nào cần được xem xét?

## Quyết định
Chúng ta sẽ thực hiện điều gì?
Trình bày trong một hoặc hai câu, không diễn đạt mơ hồ hoặc thiếu dứt khoát.

## Các phương án đã cân nhắc
- **<Phương án>:** Lý do không lựa chọn.

## Hệ quả
Điều gì sẽ trở nên dễ dàng hơn?
Điều gì sẽ trở nên khó khăn hơn?
Những vấn đề nào cần tiếp tục theo dõi?
```

## Danh sách ADR (Index)

| ADR | Quyết định | Ngày |
|---|---|---|
| [0000](0000-baseline-operating-principles.md) | Nguyên tắc vận hành nền tảng — PM không viết code, quản lý công việc bằng file, quy tắc đặt tên `kebab-case`/`ALL_CAPS` | 2026-01-01 |