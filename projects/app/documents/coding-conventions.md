# Coding conventions

*Stub ví dụ. Ghi luật thật của bạn; agent coi đây là luật và dừng lại khi có xung đột.*

- **Components:** mỗi file một component, `PascalCase`; giữ nhỏ và tập trung một việc.
- **State:** chỉ lift lên khi thật sự dùng chung; đừng nhét chuyện cục bộ vào global store.
- **Styling:** ưu tiên design token hơn giá trị literal; đã có token thì không dùng magic number inline.
- **Strings:** project có localize thì không hardcode copy người dùng thấy — thêm key cho mọi ngôn ngữ.
- **Tests:** cover view logic và data wiring; báo số lượng. Đang đỏ thì không merge.
