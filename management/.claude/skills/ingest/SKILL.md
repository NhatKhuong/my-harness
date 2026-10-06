---

name: ingest

description: Dùng khi đưa một tập tài liệu, ticket hoặc ghi chú có sẵn (folder markdown, wiki export, cây legacy docs/) vào file board của workspace. Phân loại từng item vào backlog / bugs / decisions / project-docs, đề xuất mapping để Owner duyệt rồi mới ghi. Không bao giờ import hàng loạt một cách mù quáng.

---

# ingest — Đưa hệ thống tài liệu hiện có vào board

Đưa một tập tài liệu có sẵn về đúng cấu trúc của workspace:

`backlog/`, `bugs/`, `decisions/`, `projects/<name>/documents/`.

Phần khó nhất là **phân loại**, vì đây là công việc **mang tính phán đoán cao và có thể làm mất thông tin**. Vì vậy, sản phẩm thực sự của skill này là **mapping đề xuất để Owner duyệt**, không phải việc ghi hàng loạt file.

**Iron rule: classify → confirm → write. Không bao giờ ghi file trước khi Owner duyệt mapping.**

## Inputs

* **Source path** — folder markdown, wiki export, cây `docs/` cũ hoặc danh sách file cụ thể. Nếu chưa được cung cấp thì hỏi Owner.
* (Tùy chọn) Gợi ý về nguồn tài liệu là gì, ví dụ: `"old Notion export"` hoặc `"các design note rời của một repo"`.

## Procedure

### 1. Inventory

Liệt kê tất cả file có khả năng được đưa vào từ source.

Đọc đủ nội dung của từng file để phân loại — title, các đoạn đầu và cấu trúc. Không được âm thầm bỏ qua file nào; mỗi file phải được **map** hoặc được ghi rõ là **Drop**.

### 2. Classify

Phân loại mỗi item vào **đúng một bucket**:

* `backlog/` — công việc còn mở / việc ai đó vẫn cần thực hiện.
* `bugs/` — defect / thứ đang bị lỗi.
* `decisions/` — **"vì sao chọn X thay vì Y"** — rationale có giá trị lâu dài. Đây là một ADR.
* `projects/<name>/documents/` — specification / architecture / convention có tính lâu dài và thuộc về một surface cụ thể, nên nằm cạnh source code của surface đó.
* **Drop** — noise, duplicate, obsolete hoặc file rỗng. Drop là một kết quả hợp lệ; một tập tài liệu gần như luôn có một số file thuộc nhóm này.

### 3. Propose mapping và STOP

Xuất ra một bảng:

`source file → target path → bucket → one-line rationale`

Đồng thời liệt kê ngắn gọn:

* Những file bị **Drop** và lý do.
* Những file **ambiguous** mà Owner cần xem xét.

**Chưa được ghi bất kỳ file nào.**

Dừng lại và chờ Owner xác nhận mapping.

### 4. Sau khi Owner approve, tiến hành write

* Cấp ID dựa trên **folder**, không dựa trên `STATUS`:

```bash
ls backlog/ | grep -E '^[0-9]{4}' | sort | tail -1
```

→ lấy `NNNN` tiếp theo.

Tương tự với `bugs/`.

`grep` sẽ bỏ qua `STATUS.md`.

* Tạo ticket/ADR mới dựa trên template:

   * `templates/ticket.md`
   * `decisions/README.md`

Giữ nguyên **ngày tháng gốc** của source và thêm dòng provenance:

```text
Ingested from: <original path>
```

để lịch sử không bị âm thầm thay đổi.

* Không tự đoán `Status` hoặc `Priority` nếu không thể suy ra từ source.

Thay vào đó ghi:

```text
UNKNOWN — Owner to set
```

* Sau khi ghi xong toàn bộ file, rebuild:

```text
backlog/STATUS.md
bugs/STATUS.md
```

dựa trên folder. Có thể reuse bước reconcile của `board-doctor`.

### 5. Report back

Báo cáo dưới dạng **data, không phải prose dài**, gồm:

* Số lượng item trong từng bucket.
* Danh sách item bị Drop.
* Danh sách item được đánh dấu ambiguous.
* File nào không thể phân loại.
* Những nội dung Owner cần kiểm tra thủ công.

## Guardrails

### Ambiguity phải được đưa ra, không được tự giải quyết

Ví dụ:

> "Đây là decision hay chỉ là một note?"

→ Flag trong bước 3 và để Owner quyết định.

Không được âm thầm tự chọn.

### No scope creep

Ingest chỉ **đưa những gì đang có về đúng vị trí**.

Không được:

* cải thiện nội dung,
* merge nội dung,
* rewrite nội dung.

Nếu một document sai, hãy tạo follow-up ticket thay vì chỉnh sửa document trong quá trình ingest.

### Idempotent-ish

Nếu skill được chạy lần thứ hai, phải phát hiện những file đã được ingest thông qua dòng:

```text
Ingested from: <original path>
```

và **skip** chúng thay vì tạo bản duplicate.
