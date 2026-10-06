# Coding conventions

*Stub ví dụ. Ghi luật thật của bạn; agent coi đây là luật và dừng lại khi có xung đột.*

- **Naming:** `camelCase` cho member, `PascalCase` cho type/composable, resource theo chuẩn nền tảng (`kebab`/`snake`).
- **State:** một nguồn sự thật trong ViewModel; Composable là hàm thuần của state.
- **Enum/decoding:** decode API payload phòng thủ — một giá trị lạ không được làm crash app.
- **Strings:** app có localize thì không hardcode copy người dùng thấy — thêm key cho mọi ngôn ngữ.
- **Verification:** build pass chỉ chứng minh compile được; behavior lúc chạy phải xác nhận trên device/emulator.
