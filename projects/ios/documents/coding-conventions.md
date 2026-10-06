# Coding conventions

*Stub ví dụ. Ghi luật thật của bạn; agent coi đây là luật và dừng lại khi có xung đột.*

- **Naming:** `camelCase` cho member, `PascalCase` cho type/view; mỗi file một primary type.
- **State:** mỗi screen một nguồn sự thật trong view model của nó; view là hàm thuần của state.
- **Decoding:** decode API payload phòng thủ — enum case lạ thì fallback, không crash.
- **Strings:** app có localize thì không hardcode copy người dùng thấy — thêm key cho mọi ngôn ngữ.
- **Verification:** build pass chỉ chứng minh compile được; runtime (thứ tự init, navigation) phải xác nhận trên device/simulator.
