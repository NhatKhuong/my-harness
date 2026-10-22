# Testing

## Chính sách

- Mỗi thay đổi behavior đi kèm test; report ghi **số lượng**. Đang đỏ thì không report Done.
- Thứ thật sự không unit-test được (I/O glue, wiring) → ghi rõ lý do trong report, không âm thầm bỏ qua.
- {{ngưỡng coverage nếu có}}

## Loại test

| Loại | Framework | Vị trí | Khi nào bắt buộc |
| --- | --- | --- | --- |
| Unit | {{vitest}} | {{src/**/*.test.ts}} | {{logic thuần mới}} |
| Integration | {{…}} | {{…}} | {{đụng DB / API}} |
| E2E / UI | {{…}} | {{…}} | {{…}} |

## Dữ liệu test

- {{fixture / factory / seed / testcontainers / mock server}}

## Behavior quan sát ở nơi nó chạy

- {{surface này "quan sát được" nghĩa là gì: request thật, render trên dev server, boot simulator…}}

Lệnh chạy: xem [commands.md](commands.md).

Nguồn: {{code:… / owner / default}}

*Cập nhật lần cuối: {{YYYY-MM-DD}} — đổi nội dung thì cập nhật luôn dòng này.*
