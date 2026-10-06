---

name: ticket-new

description: Dùng khi tạo ticket mới cho backlog hoặc bug. Skill này bắt buộc search-before-open, lấy ID từ folder (không lấy từ STATUS), chạy 4-signal autonomy rubric để quyết định auto hay ask, tạo file từ template và cập nhật STATUS.md.

---

# ticket-new — Tạo ticket với kỷ luật scope được kiểm soát

Các quy tắc về scope trong playbook chính là những thứ dễ bị bỏ qua khi đang làm việc theo đà.

Skill này biến chúng thành **các bước bắt buộc và phải thực hiện đúng thứ tự**.

## Procedure

### 1. Search before you open

Kiểm tra:

```text
backlog/STATUS.md
```

hoặc:

```text
bugs/STATUS.md
```

nếu là bug.

Tìm trong các section:

* **Open**
* **Awaiting Owner**
* **Epics**

để xem đã có ticket nào liên quan đến cùng surface / feature / bug chưa.

Sau đó grep cả hai folder:

```bash
grep -ril <keyword> backlog/ bugs/
```

* Nếu đã có ticket liên quan → **extend ticket đó hoặc thêm phase. Không tạo sibling ticket mới.**

  Báo cáo ticket đã tìm thấy và dừng để Owner quyết định, trừ khi đây rõ ràng chỉ là một phase tiếp theo.

* Nếu công việc thuộc một **Epic** → gắn ticket vào Epic đó, không tạo một sibling ticket độc lập.

### 2. Lấy ID tiếp theo từ folder, không bao giờ lấy từ STATUS

Chạy:

```bash
ls backlog/ | grep -E '^[0-9]{4}' | sort | tail -1
```

Sau đó tăng ID lên 1.

`grep` bỏ qua `STATUS.md`. Nếu không có output nghĩa là chưa có ticket nào → bắt đầu từ:

```text
0001
```

Không dùng `STATUS.md` để lấy ID vì file này được maintain thủ công và có thể bị lag.

Nếu dựa vào `STATUS.md`, có nguy cơ **ID collision**.

Với bug, thực hiện tương tự trong `bugs/`.

### 3. Chọn template

Chọn từ:

```text
templates/ticket.md
```

* **light template** → item nhỏ.
* **PRD-style template** → item lớn / nhiều phase / phức tạp.

### 4. Chạy 4-signal Autonomy Rubric

Theo `pm-playbook → "Autonomy rubric"`:

* **Blast radius**

   * 1 surface → **Auto**
   * cross-surface → **Ask**

* **Change type**

   * correction (fix / typo / revert) → **Auto**
   * addition (feature / endpoint / screen mới) → **Ask**

* **Product decision**

   * không có decision / yêu cầu đã rõ → **Auto**
   * có câu hỏi kiểu "A hay B?" → **Ask**

* **Contract**

   * internal → **Auto**
   * bất kỳ thứ gì consumer phụ thuộc vào:

      * API shape
      * permission
      * CLI flags
      * schema
      * URL

     → **Ask**

#### Nếu cả 4 đều là Auto

→ persist và delegate ngay.

Trong phần **Context** của ticket, ghi rõ rule nào đã cho phép auto:

```text
Auto-persisted: rubric (1 surface / correction / no call / internal)
```

#### Nếu có bất kỳ signal nào là Ask

→ draft ticket → trình bày plan → **chờ Owner approve trước khi persist**.

### 5. Write

Tạo:

```text
backlog/NNNN-slug.md
```

hoặc:

```text
bugs/NNNN-slug.md
```

Một ticket = **một work item có tính nhất quán**.

Nếu title cần dùng từ **"and"**, thường nên tách thành các ticket riêng.

Nếu vẫn là một unit công việc nhưng có nhiều bước → giữ trong một ticket và chia thành **phases** trong body.

### 6. Thêm row vào `STATUS.md`

Thêm ticket vào đúng lane.

Một ticket mới có thể action ngay → **Open**.

## Report back

Báo cáo:

* Path của file vừa tạo.
* Kết quả của autonomy rubric.
* Nếu Auto-persist → ghi rõ rule nào đã authorize việc auto-persist.
* Nếu bước 1 tìm thấy ticket liên quan → báo cáo ticket nào đã được **extend** thay vì tạo ticket mới.
