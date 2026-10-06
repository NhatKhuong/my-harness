# Skills

**Skills** là các quy trình được đóng gói để PM Agent có thể gọi và thực hiện theo tên.

Skills **không phải convenience tools**. Mỗi skill tồn tại để đảm bảo một rule trong [pm-playbook.md](../../pm-playbook.md) — đặc biệt là những rule con người và agent dễ bỏ qua khi làm nhanh.

## Nguyên tắc tạo Skill

Một skill chỉ nên được tạo khi nó giúp **enforce một rule mà con người hoặc agent thường xuyên làm sai hoặc bỏ quên**.

Nếu skill chỉ đơn giản bọc lại một việc mà `pm-playbook.md` đã mô tả rõ ràng → **không cần tạo skill**.

> Workspace này là một **operating model**, không phải một danh sách các tool/parts.

Khi thêm một skill, phải chỉ rõ:

> **Skill này bảo vệ rule nào trong PM Playbook?**

Nếu không trả lời được câu hỏi này → skill không nên tồn tại.

---

# Tier 1 — Operating Loop

Đây là các skill quan trọng nhất, giúp các rule cốt lõi của PM được thực thi một cách nhất quán.

| Skill                                   | Rule được bảo vệ                                                                                                        |
| --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| [`ingest`](ingest/SKILL.md)             | Đưa documentation hiện có vào board → **classify trước, confirm sau**; không được bulk-import một cách mù quáng         |
| [`ticket-new`](ticket-new/SKILL.md)     | **Search-before-open** · lấy ID từ folder, không lấy từ `STATUS.md` · chạy Autonomy Rubric 4 tiêu chí trước khi persist |
| [`ticket-close`](ticket-close/SKILL.md) | **Evidence bar**, phải có evidence và counts · thực hiện bước **Harness Delta**                                         |
| [`board-doctor`](board-doctor/SKILL.md) | Xử lý điểm yếu của file-based board: `STATUS.md` được cập nhật thủ công và có thể bị lệch so với ticket                 |

---

# Cách sử dụng Skill

PM chạy một skill khi tình huống thực tế phù hợp với mô tả của skill đó.

Skill là một **checklist mà agent phải thực thi**, không phải tài liệu chỉ để đọc.

Ví dụ:

```text
Có yêu cầu tạo ticket
        ↓
ticket-new
        ↓
Search existing tickets
        ↓
Check ID
        ↓
Check Autonomy Rubric
        ↓
Persist ticket
        ↓
Report result
```

Skill phải kết thúc bằng việc báo cáo:

* Đã thực hiện những gì.
* Kết quả là gì.
* Nếu không thể tiếp tục → dừng ở đâu và lý do gì.

Kết quả được trả về dưới dạng **data cho PM**, theo đúng contract của delegation brief, thay vì một đoạn prose dài để gửi trực tiếp cho Owner.
