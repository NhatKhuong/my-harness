---

name: surface-docs

description: Dùng khi start một dự án / surface mới hoặc khi `projects/<surface>/documents/` còn là stub hay đã lệch so với code. Tự detect greenfield (phỏng vấn Owner) hoặc brownfield (quét code read-only), lập fact sheet có nguồn để Owner duyệt, rồi sinh bộ tài liệu chuẩn cho sub-agent + cập nhật agent file và README của surface. Không bao giờ bịa convention, không bao giờ ghi trước khi Owner duyệt.

---

# surface-docs — Dựng "luật của surface" từ sự thật có nguồn

`documents/` của sub-project là **luật** mà agent sở hữu đọc trước khi code. Khi nó còn là stub, luật chỉ tồn tại trên giấy: agent không có gì để tuân thủ và không biết khi nào phải dừng để flag.

Skill này biến việc viết bộ tài liệu đó thành một quy trình lặp lại được cho mọi dự án, mọi surface.

**Rule được bảo vệ:**

* *"`documents/` của sub-project là luật"*: luật phải được viết ra từ **fact có nguồn** (code hoặc lời Owner), không phải từ trí tưởng tượng của agent.
* *"Contract thì phải hỏi"*: mọi contract (response shape, status code, permission, schema, URL, event, CLI flag) phải được **Owner xác nhận** và ghi tường minh.

**Iron rule: gather → fact sheet → Owner duyệt → write. Không ghi file nào trước khi Owner duyệt.**

## Bộ file của skill

| File | Dùng ở bước |
| --- | --- |
| [reference/scan-signals.md](reference/scan-signals.md) | 1, 3 — nhận diện stack / profile / lệnh từ manifest |
| [reference/interview.md](reference/interview.md) | 3 — bộ câu hỏi chung, chia round |
| [profiles/backend.md](profiles/backend.md) · [profiles/web.md](profiles/web.md) · [profiles/mobile.md](profiles/mobile.md) | 3, 5 — câu hỏi + tín hiệu + section riêng theo loại surface |
| [doc-templates/](doc-templates/) | 5 — skeleton của từng tài liệu |

## Inputs

* **Surface**, ví dụ `api`. Đường dẫn: `../projects/<surface>`. Chưa có thì hỏi Owner.
* (Tùy chọn) **Profile**: `backend` | `web` | `mobile`. Không truyền thì suy ra ở bước 1.
* (Tùy chọn) **Mode**: `bootstrap` (mặc định) hoặc `refresh` (docs đã có, code đã đổi; xem cuối file).

## Procedure

### 1. Detect mode & profile

Liệt kê `../projects/<surface>`, **bỏ qua** `documents/`, `README.md`, `.git/`.

* Còn source / manifest → **brownfield**.
* Không còn gì → **greenfield**.

Suy profile từ manifest theo [scan-signals.md](reference/scan-signals.md). Có tín hiệu lẫn lộn (vd. monorepo có cả server lẫn web) → hỏi Owner, không tự chọn. Một folder chứa hai surface là dấu hiệu nên tách surface: flag lên.

### 2. Inventory `documents/` hiện có

Với mỗi file trong `documents/`, phân loại:

| Trạng thái | Nhận diện | Được làm gì |
| --- | --- | --- |
| `stub` | Có câu *"Stub ví dụ"* hoặc chỉ toàn placeholder | Thay thế hoàn toàn |
| `owner` | Nội dung thật do người viết | **Không ghi đè.** Chỉ đề xuất diff từng mục |
| `missing` | Có trong bộ chuẩn nhưng chưa tồn tại | Tạo mới |

Bộ chuẩn = các file trong [doc-templates/](doc-templates/). File `owner` nằm ngoài bộ chuẩn → giữ nguyên, chỉ thêm link vào index.

### 3. Gather facts

**Brownfield:** quét **read-only** theo [scan-signals.md](reference/scan-signals.md) và mục "Scan" của profile.

* Mỗi fact ghi kèm nguồn `code:<path>` (thêm `:line` nếu cụ thể).
* Convention suy ra từ code (naming, layering, error shape…) chỉ ghi khi thấy **nhất quán ở ≥3 chỗ**. Ít hơn thì đánh dấu `inferred?` để Owner xác nhận.
* Không chạy lệnh có side effect (install, migrate, build ghi file). Đọc file là đủ.

**Greenfield:** phỏng vấn theo [interview.md](reference/interview.md) + mục "Interview" của profile.

* Dùng AskUserQuestion, **≤4 câu mỗi round**, mỗi câu có option `(Recommended)` làm default.
* Owner trả "tùy bạn" → ghi nguồn `default`, không ghi `owner`.

**Cả hai mode:** những gì code không trả lời được (ý định, convention ngầm, ai là consumer, cái gì là contract) vẫn phải hỏi Owner. Brownfield chỉ rút ngắn phỏng vấn, không thay thế nó.

### 4. Fact sheet → STOP

Trình cho Owner, chưa ghi file:

1. **Bảng fact:** `nhóm → fact → giá trị → nguồn` với nguồn ∈ `code:<path>` · `owner` · `default` · `inferred?` · `UNKNOWN`.
2. **Kế hoạch file:** `path → create | replace-stub | propose-diff | keep`.
3. **Contract list:** những gì sẽ ghi vào `architecture/03-contracts.md`. Mục contract **không được** mang nguồn `default`; phải là `owner` hoặc `code:` đã được Owner gật.
4. **Đề xuất ADR:** lựa chọn có rationale lâu dài (chọn DB, kiểu auth, versioning…) → đề xuất `decisions/NNNN-slug.md`. Không nhét rationale vào `documents/`.
5. **Câu hỏi chưa chốt** → đề xuất thêm vào `decisions/CANDIDATES.md`.

Dừng và chờ Owner duyệt hoặc sửa. Owner sửa fact nào thì cập nhật nguồn thành `owner`.

### 5. Write `documents/`

Sau khi được duyệt, sinh từng file từ [doc-templates/](doc-templates/), thêm section riêng của profile (mục "Sections" trong profile file).

* Thay hết `{{…}}`. Phần không áp dụng → `N/A — <lý do>`. Phần chưa biết → `UNKNOWN — Owner to set`. **Không xoá heading** để agent biết mục đó đã được cân nhắc.
* Xoá các dòng chú thích `<!-- … -->` của template.
* Cuối mỗi file: `Nguồn: <code:… / owner / default>` và `*Cập nhật lần cuối: YYYY-MM-DD — đổi nội dung thì cập nhật luôn dòng này.*`
* **Không bao giờ copy giá trị secret.** Từ `.env*`, config, CI chỉ ghi **tên biến** và mục đích.
* Ngắn gọn: bullet / bảng, mỗi fact nằm ở **đúng một file**; file khác thì link sang.
* `response-format.md`: dùng bản canonical, chỉ thay dòng `Verified:` theo loại evidence của profile.
* ADR / CANDIDATES được duyệt ở bước 4: ADR tạo theo `decisions/README.md`, lấy ID từ folder (`ls decisions/ | grep -E '^[0-9]{4}' | sort | tail -1`).

### 6. Agent file + README

**`management/.claude/agents/<surface>.md`**: tạo theo [templates/agent.md](../../../templates/agent.md) nếu chưa có; đã có thì **chỉ sửa** các mục sau, giữ nguyên scope fence và phần còn lại:

* `description`: stack thật + domain một dòng + surface nào KHÔNG dùng.
* "Your surface / Surface của bạn": stack thật, bỏ chữ "ví dụ".
* Bảng "Read on demand": mỗi tình huống → doc tương ứng trong bộ mới (xem bảng trong `doc-templates/README.md`).
* Verification bar: lệnh build / test / smoke **copy nguyên văn** từ `commands.md`.

**`projects/<surface>/README.md`**: tên + một dòng mô tả, stack, agent sở hữu, link `documents/README.md`. Bỏ chữ "stub ví dụ".

### 7. Self-check (bắt buộc, chạy bằng lệnh)

```bash
cd ../projects/<surface>
grep -rn '{{' documents/                        # phải rỗng
grep -rln 'Stub ví dụ\|stub ví dụ' documents/ README.md   # phải rỗng
grep -rn 'UNKNOWN — Owner to set' documents/    # liệt kê vào report
```

Kiểm tra thêm:

* Mọi file trong `documents/` đều có trong bảng index của `documents/README.md`.
* Mọi link tương đối trong `documents/`, agent file, README đều trỏ tới file tồn tại.
* Lệnh trong agent file trùng khớp với `commands.md`.
* Không có chuỗi giống secret (`grep -rniE '(password|secret|token|api[_-]?key)\s*[:=]\s*\S{6,}' documents/` phải rỗng).

Fail bất kỳ mục nào → sửa rồi chạy lại. Không report Done khi self-check đỏ.

### 8. Report back (data, không phải prose)

```
Surface: <name>  Profile: <backend|web|mobile>  Mode: <greenfield|brownfield|refresh>
Files: created N · replaced-stub N · diff-proposed N · kept N
  - <path> (<action>)
Facts: code N · owner N · default N · inferred? N · UNKNOWN N
UNKNOWN (Owner cần điền):
  - <file> → <mục>
ADR: <đã tạo / đề xuất>   CANDIDATES: <đã thêm>
Agent file: <path> (<created|updated>)   README: <updated>
Self-check: placeholders 0 · stub 0 · broken links 0 · secrets 0
Harness delta: <điều skill này học được, hoặc "None">
```

## Mode `refresh`

Chạy khi docs đã do skill (hoặc Owner) viết và code đã đổi.

1. Quét lại như bước 3 (brownfield).
2. So sánh từng fact với docs hiện có → bảng `file → mục → docs nói → code nói → nguồn`.
3. **STOP**, Owner duyệt từng dòng. Contract lệch → **không sửa docs cho khớp code**: đó có thể là contract bị đổi lặng lẽ. Flag cho PM mở ticket / bug.
4. Chỉ sửa đúng các mục được duyệt, cập nhật dòng `Cập nhật lần cuối`.

## Guardrails

* **Chỉ ghi** vào `projects/<surface>/documents/`, `projects/<surface>/README.md`, `management/.claude/agents/<surface>.md`, và `decisions/` (khi đã duyệt). **Không bao giờ sửa source code** (PM không viết code).
* **Không bịa.** Không có nguồn → hỏi hoặc `UNKNOWN`. Một tài liệu sai còn tệ hơn tài liệu trống, vì agent sẽ tuân thủ nó.
* **Không tự hoà giải mâu thuẫn.** Code nói A, Owner nói B → ghi cả hai vào fact sheet, Owner chọn.
* **Không scope creep.** Thấy bug / nợ kỹ thuật khi quét → ghi vào report để PM mở ticket qua `ticket-new`, không sửa.
* Bài học về chính skill này → harness delta / ADR. **Không sửa `pm-playbook.md`.**
