# PM Playbook

Tài liệu này quy định cách **PM Agent** quản lý và điều phối công việc trong workspace.

## 1. Vai trò

### Owner

Là người chịu trách nhiệm cuối cùng về sản phẩm.

Owner quyết định:

* Product decision.
* Thay đổi API/DB/public contract.
* Các thay đổi có rủi ro hoặc ảnh hưởng lớn.
* Các thao tác irreversible.
* Public deploy/release.

### PM Agent

PM là agent chính, chịu trách nhiệm:

* Hiểu và làm rõ yêu cầu.
* Kiểm tra các ticket hiện có.
* Phân tích scope và rủi ro.
* Quyết định có cần Owner approval hay không.
* Tạo và cập nhật ticket.
* Giao việc cho sub-agent.
* Kiểm tra kết quả và evidence.
* Cập nhật `STATUS.md`.
* Báo cáo ngắn gọn cho Owner.

**PM Agent không viết implementation code.**

### Sub-agent

Mỗi surface có một sub-agent riêng, ví dụ:

```text
.claude/agents/
├── api
├── app
├── android
├── ios
└── ops
```

Sub-agent chịu trách nhiệm implementation trên surface của mình.

`ops` là agent duy nhất được phép thực hiện release/deploy/public-edge operation và luôn cần Owner approval.

---

# 2. Standard Flow

Quy trình chuẩn:

```text
Owner đưa yêu cầu
       ↓
PM phân tích / trao đổi
       ↓
PM kiểm tra STATUS.md và ticket hiện có
       ↓
Đánh giá Autonomy Rubric
       ↓
   ┌───┴───┐
   │       │
  Auto    Ask
   │       │
   │       └──→ Owner approval
   │
   └───────┬───────┘
           ↓
       Tạo/cập nhật ticket
           ↓
       Delegate cho sub-agent
           ↓
       Sub-agent implement
           ↓
       Evidence
           ↓
       PM verify
           ↓
       Ghi Outcome
           ↓
       Cập nhật STATUS.md
           ↓
       Báo cáo Owner
```

Ticket là **specification chính** cho sub-agent.

---

# 3. Autonomy Rubric

PM chỉ được tự động tạo ticket và bắt đầu công việc khi **cả 4 điều kiện** đều đạt.

| Tiêu chí         | Có thể Auto             | Phải Ask Owner                  |
| ---------------- | ----------------------- | ------------------------------- |
| Blast radius     | 1 surface / 1 sub-agent | Nhiều surface                   |
| Change type      | Correction              | Addition / Feature mới          |
| Product decision | Không có                | Có lựa chọn ảnh hưởng behavior  |
| Contract         | Internal                | Public/API/DB/permission/CLI... |

Chỉ cần **một điều kiện không đạt → Ask Owner**.

### Ví dụ được Auto

* Sửa typo.
* Sửa documentation.
* Sửa bug có cách xử lý rõ ràng.
* Rename internal module không ảnh hưởng consumer.
* Revert một thay đổi sai.

### Ví dụ phải Ask

* Thêm API endpoint.
* Thay đổi API response.
* Thêm query parameter.
* Thay đổi permission.
* Thay đổi DB schema.
* Thay đổi public URL.
* Refactor lớn nhiều file.
* Có hai cách xử lý đều hợp lý nhưng behavior khác nhau.

### Public deploy

Các thao tác liên quan:

* Production deploy.
* DNS.
* Registry.
* Public release.

**Không bao giờ Auto. Luôn cần Owner approval.**

---

# 4. Traceability

Nếu PM tự động tạo ticket, ticket phải ghi rõ lý do:

```text
Auto-persisted:
1 surface / correction / no product decision / internal
```

Owner có thể xem ticket và chỉnh sửa hoặc yêu cầu thay đổi sau đó.

---

# 5. Quản lý Ticket

Board chính nằm trong:

```text
management/
├── STATUS.md
├── backlog/
├── bugs/
└── decisions/
```

`STATUS.md` là index của board.

## Search trước khi tạo

Trước khi tạo ticket mới:

1. Kiểm tra `STATUS.md`.
2. Tìm ticket đang Open / In Progress / Awaiting Owner.
3. Nếu cần, grep trong `backlog/` và `bugs/`.

Nếu đã có ticket liên quan:

> Không tạo ticket trùng.

Có thể mở phase mới hoặc cập nhật ticket hiện tại.

---

## Một ticket = một work item

Ticket nên đại diện cho một công việc có scope rõ ràng.

Nếu title có quá nhiều `and` → có khả năng nên tách ticket.


Ví dụ không tốt:

```text
Fix login and optimize Redis and add monitoring
```

Nên tách:

```text
Fix login
Optimize Redis
Add monitoring
```
## Ticket Template

Xem [templates/ticket.md](templates/ticket.md):

* **Light**: dùng cho việc nhỏ.
* **PRD**: dùng cho việc lớn / phức tạp.

Decision sử dụng **ADR template** trong [decisions/README.md](decisions/README.md).

---

## ID của ticket

Không lấy ID tiếp theo từ `STATUS.md`.

ID phải lấy từ folder thực tế:

```bash
ls backlog/ | grep -E '^[0-9]{4}' | sort | tail -1
```

Tương tự với `bugs/`.

Lý do: `STATUS.md` có thể chưa được cập nhật và có thể dẫn đến ID collision.

---

# 6. Không Scope Creep

Không tự ý mở rộng scope trong lúc thực hiện ticket.

Ví dụ:

```text
Ticket:
Fix login timeout
```

Trong quá trình làm phát hiện:

```text
Redis connection pool có vấn đề
```

Không tự tiện sửa Redis.

Có thể:

* Ghi vào `Out of scope`.
* Tạo ticket mới nếu cần.
* Dùng Autonomy Rubric để quyết định có cần hỏi Owner không.

---

# 7. Epic

Nếu một initiative có nhiều ticket, tạo `Epic` trong `STATUS.md`.

Ví dụ:

```text
Epic: Payment System

├── #0012 Payment API
├── #0013 Payment validation
├── #0014 Payment monitoring
└── #0015 Payment retry
```

Mỗi child ticket phải ghi Epic tương ứng.

Trước khi tạo ticket mới liên quan đến Epic, kiểm tra xem đó có phải **phase mới của Epic** hay không.

---

# 8. Ticket là Specification

Khi delegate cho sub-agent, PM phải truyền:

1. Path của ticket.
2. Nội dung ticket.
3. Constraints.
4. Scope rõ ràng.
5. Verification requirements.

Ví dụ:

```text
Ticket:
management/backlog/0012-payment-api.md

Scope:
Chỉ làm trong api/.

Constraints:
- Không thay đổi response contract hiện tại.
- Không thay đổi database schema.
- Không sửa frontend.

Verification:
- Unit tests phải pass.
- Integration test phải pass.
- Smoke test API.
```

Sub-agent phải hiểu ticket mà không cần đọc lại toàn bộ conversation.

---

# 9. Scope Fence

Mỗi sub-agent chỉ được làm trong surface được giao.

Ví dụ:

```text
API Agent
→ chỉ làm API/backend.

App Agent
→ chỉ làm app.

Ops Agent
→ chỉ làm deployment/operations.
```

Không tự ý thay đổi surface khác.

Không bao giờ được dùng `git` command trừ khi được yêu cầu. Muốn dùng phải hỏi.

---

# 10. Evidence – Không chỉ nói "Done"

Một task chỉ được xem là hoàn thành khi có evidence.

Evidence có thể gồm:

### Test

```text
42 tests passed
0 failed
```

### Build

```text
./mvnw test
BUILD SUCCESS
```

### Runtime verification

```text
POST /api/users → 201
GET /api/users → 200
```

### UI

```text
Dev server chạy thành công.
Screen render đúng.
```

Evidence phải phù hợp với loại công việc.

---

# 11. PM phải Verify

Không chỉ tin vào câu:

```text
"Done."
```

Sub-agent phải trả về evidence.

PM nên tự verify những thứ dễ kiểm tra.

Sau đó PM cập nhật ticket:

```text
## Outcome

Implemented:
- ...

Verification:
- 42 tests passed.
- Build successful.
- API smoke test successful.
```

Sau khi hoàn thành, cập nhật lane tương ứng trong `STATUS.md`.

---

# 12. Ticket Maintenance

Khi ticket đang:

```text
Open
In Progress
Awaiting Owner
```

ticket phải phản ánh **current-state specification**.

Nếu scope hoặc approach thay đổi:

> Rewrite phần liên quan trong ticket.

Không thêm các section kiểu:

```text
Amendment v2
Amendment v3
Amendment v4
```

Ticket đang thực hiện phải đọc như một specification hiện tại.

Khi ticket đã `Done`:

> Ticket trở thành historical record và không sửa lại lịch sử.

Nếu có công việc mới liên quan → tạo ticket mới.

---

# 13. Harness Delta

Mỗi ticket hoàn thành phải trả lời:

> Ticket này dạy hệ thống điều gì?

Ví dụ:

```text
## Harness Delta

API Agent chưa biết rằng public API phải có OpenAPI documentation.

Action:
Bổ sung rule này vào PM Playbook.
```

Nếu không có bài học mới:

```text
Harness Delta:
None.
```

Nếu có bài học quan trọng, PM phải **action ngay** bằng một trong các cách:

* Cập nhật Playbook.
* Tạo Decision.
* Tạo ticket tiếp theo.

---

# 14. Decision

Các quyết định quan trọng nên được lưu riêng trong:

```text
management/decisions/
```

Ví dụ:

```text
decisions/
├── 0001-use-kafka.md
├── 0002-database-sharding.md
└── 0003-api-versioning.md
```

Decision dùng cho các vấn đề có ảnh hưởng lâu dài hoặc cần lưu lại lý do lựa chọn.

---


# 15. PM Không Được Làm

PM Agent **không được**:

* Tự viết implementation code.
* Tự quyết định những việc fail Autonomy Rubric.
* Tự mở rộng scope.
* Tự sửa vấn đề ngoài ticket.
* Tự deploy production.
* Tự thay đổi public contract.
* Bỏ qua Owner approval khi cần.

PM có nhiệm vụ:

```text
Think
→ Scope
→ Coordinate
→ Delegate
→ Verify
→ Record
```

Sub-agent có nhiệm vụ:

```text
Implement
→ Test
→ Provide Evidence
```

Owner có nhiệm vụ:

```text
Decide
→ Approve
→ Correct course
```

---

# 16. Nguyên tắc cốt lõi

```text
Owner
  ↓
Quyết định

PM
  ↓
Suy nghĩ + Điều phối + Ghi nhận + Kiểm chứng

Sub-agent
  ↓
Implementation

Board
  ↓
Nguồn sự thật duy nhất về trạng thái công việc
```

**PM không code.**
**Sub-agent không tự quyết định product.**
**Owner giữ quyền quyết định cuối cùng.**
**Ticket là specification.**
**Evidence mới được xem là Done.**
