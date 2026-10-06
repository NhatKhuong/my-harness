---

name: ticket-close

description: Dùng khi hoàn thành một ticket. Skill này kiểm tra evidence bar (test/build kèm số lượng, behavior đã được quan sát), điền phần Outcome, bắt buộc ghi harness delta và chuyển lane trong STATUS.md. Không cho phép đóng ticket chỉ dựa trên cảm giác "đã xong".

---

# ticket-close — Đóng ticket với bằng chứng, không dựa vào cảm tính

"Done" phải có **evidence được ghi lại trong ticket**.

Skill này sẽ **từ chối đóng ticket** nếu chưa đạt evidence bar, đồng thời bắt buộc thực hiện một bước mọi người thường quên: **harness delta**.

## Procedure

### 1. Check Evidence Bar

Theo `pm-playbook → "Evidence bar"`.

Để đóng ticket, report của sub-agent phải có:

* **Tests green kèm số lượng**, hoặc **build green kèm tên tool** đã sử dụng.

* **Behavior đã được quan sát tại nơi nó thực sự chạy** — ví dụ:

   * dev-server smoke,
   * compiled-binary boot,
   * DOM render,
   * real request,

  và phải phù hợp với surface đang được kiểm tra.

* Nếu thiếu bất kỳ phần nào → **không được đóng ticket**.

Phải ghi rõ chính xác phần evidence nào còn thiếu và giữ ticket ở trạng thái **In Progress**.

Với những claim dễ kiểm chứng, PM phải **tự smoke-test lại độc lập** thay vì chỉ truyền đạt lại lời của sub-agent.

### 2. Điền phần Outcome

Trong file ticket, điền:

```text id="u3y0r4"
Files changed: <path:line>

Verified via: build / tests-with-counts / manual smoke

Evidence: những gì thực tế đã chứng minh ticket hoạt động
```

### 3. Ghi Harness Delta — bắt buộc

Trả lời câu hỏi:

> **"Ticket này đã dạy hệ thống điều gì?"**

Ví dụ:

* Một rule trong playbook trước đây chưa đúng.
* Có khoảng trống trong specification.
* Có edge case trong autonomy rubric.

Sau đó **phải xử lý ngay** bằng cách đưa insight đó vào một trong các nơi phù hợp:

* `pm-playbook`
* một `decision`
* một ticket mới.

`None` là câu trả lời hợp lệ, nhưng phải ghi rõ:

```text
Harness delta: None
```

Không ghi gì không được xem là câu trả lời.

### 4. Chuyển lane trong `STATUS.md`

* Nếu **hoàn toàn Done** và không cần Owner làm gì thêm → **Closed**.
* Nếu đã **build/decided** nhưng vẫn cần Owner:

   * commit,
   * deploy,
   * review,
   * decision

  → **Awaiting Owner**.

`Awaiting Owner` **không phải** một open-work lane.

### 5. Freeze body

Khi ticket đã Done, body của ticket trở thành **immutable record**.

Nếu phát sinh công việc liên quan mới → tạo **ticket mới** bằng `ticket-new`, **không chỉnh sửa lịch sử của ticket đã đóng**.

## Report back

Báo cáo:

* Ticket đã chuyển sang lane nào.
* Evidence nào đã đạt evidence bar, **kèm số lượng**.
* Harness delta là gì.
* Insight đó đã được đưa vào đâu.
