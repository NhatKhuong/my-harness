# Coding conventions

*Stub ví dụ. Ghi luật thật của bạn; agent coi đây là luật và dừng lại khi có xung đột.*

- **Naming:** `camelCase` cho biến/hàm, `PascalCase` cho type, `kebab-case` cho file.
- **Errors:** lỗi phải nổ ra ngay tại boundary, không nuốt. Trả typed error shape, không trả string trần.
- **Tests:** mỗi thay đổi behavior đi kèm một test; báo số lượng. Đang đỏ thì không merge.
- **Comments:** giải thích *vì sao*, không giải thích *cái gì*. Đừng kể lịch sử ticket trong source.
- **Contracts:** API response shape và permission là contract — đổi chúng là quyết định do PM gate, không phải sửa cục bộ.
