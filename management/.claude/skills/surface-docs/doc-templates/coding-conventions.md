# Coding conventions

Đây là luật; agent tuân thủ và **dừng lại khi có xung đột** với ticket.

<!-- Rule đã được lint/formatter cưỡng chế → chỉ ghi "do <tool> cưỡng chế (config: <path>)", không chép lại từng rule. -->

## Tool cưỡng chế

- **Formatter:** {{vd. Prettier — .prettierrc}}
- **Linter:** {{vd. ESLint — eslint.config.js}}
- **Type check:** {{vd. tsc strict}}

## Naming

- {{biến/hàm, type/class, file, constant, test}}

## Cấu trúc code

- {{một file một …; giới hạn kích thước; barrel file; import alias}}

## Error handling

- {{vd. lỗi nổ tại boundary, không nuốt; trả typed error, không string trần}}

## Logging

- {{thư viện; level; không log secret / PII}}

## Dependency

- {{vd. thêm dependency mới = hỏi PM trước}}

## Comment

- Giải thích *vì sao*, không giải thích *cái gì*. Đừng kể lịch sử ticket trong source.

## Contract

- Response shape, status code, permission, schema là contract — xem [architecture/03-contracts.md](architecture/03-contracts.md). Đổi chúng do PM gate.

<!-- Section riêng của profile chèn tại đây. -->

Nguồn: {{code:… / owner / default}}

*Cập nhật lần cuối: {{YYYY-MM-DD}} — đổi nội dung thì cập nhật luôn dòng này.*
