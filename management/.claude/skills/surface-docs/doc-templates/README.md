# {{surface}} — documents

<!-- Index của bộ luật surface. Agent sở hữu đọc file này đầu tiên để biết đọc gì, khi nào. -->

**{{surface}}** — {{một câu: surface làm gì, cho ai}}.
Stack: {{ngôn ngữ · framework · datastore}} · Agent sở hữu: `{{surface}}` (`management/.claude/agents/{{surface}}.md`).

Đây là **luật** của surface. Ticket mâu thuẫn với bất kỳ file nào dưới đây → **dừng và flag cho PM**, không tự hoà giải.

## Đọc gì, khi nào

| Khi bạn chuẩn bị… | Đọc trước |
| --- | --- |
| Bắt đầu bất kỳ ticket nào | [architecture/01-overview.md](architecture/01-overview.md) |
| Viết hoặc sửa code | [coding-conventions.md](coding-conventions.md) |
| Đổi data model / thêm migration | [architecture/02-data-model.md](architecture/02-data-model.md) |
| Chạm vào bất cứ thứ gì consumer phụ thuộc | [architecture/03-contracts.md](architecture/03-contracts.md) — **contract thì phải hỏi** |
| Build, chạy, test, smoke | [commands.md](commands.md) |
| Viết test | [testing.md](testing.md) |
| Chạm auth, secret, dữ liệu nhạy cảm | [security.md](security.md) |
| Gặp thuật ngữ domain lạ | [glossary.md](glossary.md) |
| Viết report "done" | [response-format.md](response-format.md) |

## Quyết định liên quan

<!-- Link ADR trong management/decisions/ ảnh hưởng surface này. Không có → "None yet". -->
{{- [NNNN — tiêu đề](../../../management/decisions/NNNN-slug.md)}}

*Cập nhật lần cuối: {{YYYY-MM-DD}} — đổi nội dung thì cập nhật luôn dòng này.*
