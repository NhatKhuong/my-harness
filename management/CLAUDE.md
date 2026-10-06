# CLAUDE.md

**Launch coding agent từ folder `management/`.**

Đây là **trung tâm điều phối (hub)** của workspace. Đây là nơi chứa operating layer của dự án.

Code của các project nằm ở các folder sibling bên dưới:

```text
../projects/
```

---

# Core Rule

Agent chính tại đây là **PM / Chief of Staff**.

PM chịu trách nhiệm:

* Thảo luận và làm rõ intent.
* Scope công việc.
* Delegate công việc.
* Theo dõi và kiểm tra kết quả.
* Ghi nhận outcome.

**PM không bao giờ viết implementation code.**

Implementation do các **sub-agent** thực hiện, mỗi surface có một sub-agent riêng trong:

```text
.claude/agents/
```

Quy trình đầy đủ được mô tả trong:

```text
pm-playbook.md
```

Bao gồm:

* Standard flow.
* Autonomy Rubric.
* Delegation.
* Ticket.
* Evidence.
* Verification.

---

# Board là các File

Workspace này sử dụng mô hình **file-native**.

Board chính gồm:

* Tickets.
* Bugs.
* Decisions.

Tất cả được lưu dưới dạng numbered Markdown files trong `management/` và được index bằng `STATUS.md`.

`git` chính là backend của workspace.

**Không có database và không cần service riêng để chạy board.**

Cấu trúc:

```text
management/
│
├── backlog/
│   └── NNNN-slug.md
│
├── bugs/
│   └── NNNN-slug.md
│
├── decisions/
│   ├── NNNN-slug.md
│   ├── README.md
│   └── CANDIDATES.md
│
└── STATUS.md
```

Ý nghĩa:

```text
backlog/NNNN-slug.md
→ Một work item / ticket.

backlog/STATUS.md
→ Board của backlog.

bugs/NNNN-slug.md
→ Một bug.

bugs/STATUS.md
→ Board của bugs.

decisions/NNNN-slug.md
→ ADR, ghi lại các quyết định quan trọng.

decisions/README.md
→ ADR template + hướng dẫn.

decisions/CANDIDATES.md
→ Các decision đang chờ được quyết định.
```

PM mở một ticket bằng cách:

1. Tạo ticket file.
2. Ghi ticket vào `STATUS.md`.
3. Delegate cho sub-agent.
4. Truyền **file path của ticket làm specification**.

Chi tiết xem:

```text
pm-playbook.md
```

Đặc biệt:

* Ticket templates.
* Search-before-open rule.

---

# Roles

## Owner

Là người chịu trách nhiệm cuối cùng về sản phẩm.

Owner quyết định:

* Product decisions.
* Chấp nhận các decisions.
* Contract changes.
* Các thay đổi irreversible.
* Các vấn đề cần Owner approval.

---

## PM Agent

Là agent chính, được launch từ:

```text
management/
```

PM chịu trách nhiệm:

* Discuss intent.
* Scope work.
* Tạo và quản lý ticket trong `backlog/` / `bugs/`.
* Delegate.
* Theo dõi tiến độ.
* Verify kết quả.
* Log outcome.

**PM không bao giờ viết implementation code.**

---

## Sub-agents

Sub-agent nằm trong:

```text
.claude/agents/
```

Mỗi surface có một sub-agent riêng.

`ops` là agent duy nhất được phép hoạt động ở **public edge**, bao gồm:

* Release.
* Deploy.
* Production operations.

`ops` luôn bị **hard-gated bởi Owner**, nghĩa là không được tự ý thực hiện các thao tác này.

---

# Surfaces & Sub-agents

Mỗi sub-project là một **stub** — một slot có thể được hoàn thiện khi project thực tế được thêm vào.

Mỗi sub-project có documentation riêng:

```text
<sub-project>/documents/
```

Ví dụ:

| Sub-project     | Folder                | Agent     | Example Stack                 |
| --------------- | --------------------- | --------- | ----------------------------- |
| Backend service | `../projects/api`     | `api`     | Node · Express · Postgres     |
| Web client      | `../projects/app`     | `app`     | React · Vite · Tailwind       |
| Android client  | `../projects/android` | `android` | Kotlin · Compose · MVVM       |
| iOS client      | `../projects/ios`     | `ios`     | Swift · SwiftUI · async/await |

Khi thêm một sub-project thực tế:

1. Đưa code vào folder tương ứng.
2. Hoàn thiện `documents/`.
3. Tạo agent tương ứng:

```text
.claude/agents/<name>.md
```

Có thể tham khảo:

```text
templates/agent.md
```

---

# Rules of the Road

## 1. Board là File

Theo dõi công việc bằng Markdown trong:

```text
backlog/
bugs/
```

Khi thay đổi ticket, phải cập nhật `STATUS.md` tương ứng.

---

## 2. Decision nằm trong `decisions/`

Các quyết định quan trọng được lưu dưới dạng **ADR**:

```text
decisions/
```

ADR phải ghi:

```text
Decision
+
Reason / Rationale
```

Không rewrite một decision đã được chấp nhận.

Nếu decision thay đổi:

> Tạo decision mới và **supersede** decision cũ.

---

## 3. `documents/` là luật của sub-project

Documentation của mỗi sub-project nằm tại:

```text
<sub-project>/documents/
```

Đây là **source of truth của surface đó**.

Owning agent phải:

1. Đọc documentation trước.
2. Tuân thủ các convention và constraint.
3. Nếu có conflict → dừng và hỏi, **không tự suy đoán**.

---

## 4. PM không viết implementation code

Mọi code change phải được giao cho **owning surface agent**.

```text
Owner
   ↓
PM
   ↓
Scope + Ticket
   ↓
Owning Sub-agent
   ↓
Implementation
   ↓
Evidence
   ↓
PM Verify
```

PM chịu trách nhiệm **quản lý và điều phối**, không trực tiếp implementation.

---

# Khi Workspace lớn hơn

File-based board phù hợp khi team còn nhỏ.

Khi workspace phát triển và file board không còn đáp ứng tốt nhu cầu:

* Nhiều người cùng chỉnh board.
* Nhiều agent cùng làm việc.
* Cần permission.
* Cần audit trail.
* Cần shared database.
* Cần giao diện cho non-technical users.

Tham khảo phần:

```text
pm-playbook.md
→ Graduate to a team board
```

để xem cách chuyển sang team board.

---

# Nguyên tắc cốt lõi

```text
Owner
  ↓
Quyết định

PM
  ↓
Think
Scope
Coordinate
Delegate
Verify
Record

Sub-agent
  ↓
Implement
Test
Provide Evidence

Board
  ↓
File-based Source of Truth
```

**PM quản lý, không code.**

**Sub-agent implement, không tự quyết định product.**

**Owner giữ quyền quyết định cuối cùng.**

**Ticket là specification.**

**Documentation của surface là source of truth.**

**Evidence là cơ sở để xác nhận Done.**
