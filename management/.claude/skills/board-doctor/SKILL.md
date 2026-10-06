---

name: board-doctor

## description: Dùng để kiểm tra và đồng bộ file board. Phát hiện ID collision, ticket bị thiếu khỏi STATUS, STATUS lệch với Status trong ticket và các ticket Awaiting Owner bị tồn đọng. Sau đó rebuild STATUS.md từ folder — nơi chứa source of truth. Chạy skill này khi STATUS có vẻ không đồng bộ với các ticket.

# board-doctor — Đồng bộ File Board với STATUS

Điểm yếu chính của file-based board:

> `STATUS.md` được cập nhật thủ công nên có thể bị lệch so với các ticket thực tế trong folder.

Điều này có thể dẫn đến:

* Hai ticket dùng cùng một ID.
* Ticket đã `Done` nhưng vẫn xuất hiện ở `Open`.
* Ticket tồn tại nhưng không có trong `STATUS.md`.
* `STATUS.md` chứa ticket không còn tồn tại.

Skill này dùng để **audit và reconcile** sự khác biệt đó.

---

# Source of Truth

Thứ tự ưu tiên của source of truth:

```text
Folder
  ↓
Quyết định ticket nào thực sự tồn tại

Ticket body
  ↓
Quyết định ticket đang ở lane nào
  thông qua field Status:

STATUS.md
  ↓
Derived view / index của board
```

Cụ thể:

* **Folder** quyết định ticket nào tồn tại.
* Field `Status:` trong ticket quyết định lane.
* `STATUS.md` là view được tạo từ hai nguồn trên.

Ví dụ:

```text
backlog/0012-login.md

Status: Done
```

thì ticket phải thuộc lane `Done`, ngay cả khi `STATUS.md` vẫn đang để nó ở `Open`.

**Không sửa ticket body chỉ để làm cho nó khớp với STATUS.md.**

---

## Các field chỉ tồn tại trong STATUS.md

Một số thông tin không nằm trong ticket body mà chỉ có trong `STATUS.md`.

Ví dụ:

### Awaiting Owner

```text
Waiting on
Detail
```

### Open

```text
Agents
```

Khi rebuild `STATUS.md`:

* Các field có thể derive → rebuild từ folder + ticket body.
* Các field không thể derive → giữ nguyên từ `STATUS.md` hiện tại.
* Không được làm mất thông tin Owner đang theo dõi.

---

# Checks

Chạy các check sau trên cả:

```text
backlog/
bugs/
```

## 1. ID Collision

Kiểm tra xem có hai file cùng sử dụng một `NNNN` hay không.

Ví dụ:

```text
backlog/0012-login.md
backlog/0012-payment.md
```

→ ID collision.

Phải report cả hai file.

**Owner quyết định ticket nào được renumber.**

Renumber nghĩa là:

```text
rename file
+
update STATUS.md
```

Không tự ý quyết định ticket nào phải đổi ID.

---

## 2. Orphan Ticket

Kiểm tra theo cả hai chiều.

### Ticket file không có trong STATUS

```text
backlog/0012-login.md
```

tồn tại nhưng không có row tương ứng trong `STATUS.md`.

### STATUS trỏ tới ticket không tồn tại

```text
STATUS.md
→ 0013-payment
```

nhưng:

```text
backlog/0013-payment.md
```

không tồn tại.

Cả hai đều phải được report.

---

## 3. Lane Drift

Kiểm tra sự khác nhau giữa:

```text
Ticket body
vs
STATUS.md lane
```

Ví dụ:

```text
Ticket:
Status: Done
```

nhưng:

```text
STATUS.md:
Open
```

→ Lane drift.

**Ticket body là source of truth.**

Không sửa ticket body để khớp STATUS.

---

## 4. Stale Awaiting Owner

Kiểm tra các ticket đang nằm trong:

```text
Awaiting Owner
```

Những ticket này cần được **flag để Owner được nhắc**.

Không tự động chuyển chúng sang lane khác.

> Chỉ Owner mới có quyền clear `Awaiting Owner`.

---

## 5. Next-ID Safety

Đảm bảo ID lớn nhất trong folder không nhỏ hơn ID lớn nhất trong `STATUS.md`.

Ví dụ kiểm tra:

```bash
ls backlog/ | grep -E '^[0-9]{4}' | sort | tail -1
```

Mục đích:

> Đảm bảo `ticket-new` không tạo ra ID đã tồn tại.

Folder là nguồn chính để xác định next ID.

---

# Procedure

## Bước 1 — Liệt kê Ticket

Liệt kê ticket trong `backlog/`:

```bash
ls backlog/ | grep -E '^[0-9]{4}'
```

và tương tự với:

```bash
ls bugs/ | grep -E '^[0-9]{4}'
```

`grep` giúp bỏ qua các file như:

```text
STATUS.md
```

Sau đó đọc các field chính trong mỗi ticket:

```text
Status:
Priority:
Title
```

Ticket sử dụng **bold headers**, không sử dụng YAML frontmatter.

Vì vậy:

**Không tìm các field bằng `---` fence.**

---

## Bước 2 — Chạy 5 Checks

Thực hiện:

```text
1. ID collisions
2. Orphans
3. Lane drift
4. Stale Awaiting Owner
5. Next-ID safety
```

Sau đó tạo findings list.

Ví dụ:

```text
Findings:

[Collision]
0012 → login.md
0012 → payment.md

[Orphan]
0015-payment.md không có trong STATUS.md

[Lane drift]
0018-login.md:
Ticket = Done
STATUS = Open

[Stale]
0020-payment.md đang Awaiting Owner

[Next-ID]
Folder max = 0020
STATUS max = 0019
→ Safe
```

---

# Bước 3 — Rebuild STATUS.md

Tạo phiên bản `STATUS.md` đã được reconcile.

Các field có thể derive phải lấy từ:

```text
Folder
+
Ticket body
```

Bao gồm:

```text
ID
Title
Priority
Lane / Status
```

Các field không thể derive phải giữ nguyên từ `STATUS.md` hiện tại:

```text
Waiting on
Detail
Agents
```

Chỉ giữ chúng cho những row mà ticket vẫn tồn tại.

**Không được làm mất các thông tin Owner đang theo dõi.**

---

## Giữ đúng cấu trúc từng Lane

### Open

```text
ID | Title | Priority | Status | Agents
```

### Awaiting Owner

```text
ID | Title | Waiting on | Detail
```

Không được gộp hai lane thành một cấu trúc chung nếu làm mất thông tin riêng của từng lane.

---

# Bước 4 — Show Diff

Trước khi ghi `STATUS.md`, phải xem sự khác biệt giữa:

```text
Current STATUS.md
vs
Reconciled STATUS.md
```

Nếu việc rebuild:

* Di chuyển ticket.
* Xóa row.
* Thay đổi thông tin Owner đang theo dõi.
* Có thay đổi mà Owner có thể không mong đợi.

→ **Phải confirm với Owner trước khi ghi.**

Nếu chỉ là refresh thuần túy, không thay đổi ý nghĩa:

> Có thể apply trực tiếp.

---

# Bước 5 — Write STATUS.md

Sau khi được phép:

```text
Write reconciled STATUS.md
```

**Không chỉnh sửa ticket body.**

Nếu ticket body và STATUS khác nhau:

> Rebuild STATUS theo ticket body.

---

# Report Back

Sau khi chạy `board-doctor`, report phải bao gồm:

## Findings

```text
- ID collisions
- Orphan tickets
- Lane drift
- Stale Awaiting Owner
- Next-ID safety
```

## STATUS Changes

Nêu những gì đã thay đổi trong `STATUS.md`.

## Owner Decisions

Nêu những việc cần Owner quyết định, ví dụ:

```text
- Chọn ticket nào được renumber khi có ID collision.
- Có cần nudge Owner cho ticket Awaiting Owner hay không.
```

---

# Nguyên tắc cốt lõi

```text
Folder
  ↓
Source of Truth về ticket tồn tại

Ticket body
  ↓
Source of Truth về Status / Lane

STATUS.md
  ↓
Derived Index
```

**Không sửa ticket body để khớp STATUS.**

**Rebuild STATUS từ folder + ticket body.**

**Giữ lại các field chỉ có trong STATUS.**

**ID collision cần Owner quyết định.**

**Awaiting Owner chỉ Owner mới được clear.**
