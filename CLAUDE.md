# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

---

## 1. Repo này là gì

Đây là **Gangline** — một *operating model cấp workspace* cho đội gồm người và coding agent, đóng gói dưới dạng **template**. Nó không phải framework để cài vào một codebase, không phải bộ persona đóng vai agile team, cũng không phải catalog subagent. Gangline giả định harness của bạn (Claude Code, Codex, …) đã chạy tốt; việc của nó là **tổ chức** những harness đó thành một đội kéo chung một tải.

Ba ý tưởng cốt lõi:

- **Một PM agent không bao giờ viết code.** Nó bàn intent, chia việc thành ticket, và giao cho sub-agent theo từng surface.
- **Một sơ đồ tổ chức cho agent.** Ai được tự quyết, ai phải hỏi Owner — được viết ra, version hoá, review được.
- **Board chỉ là file.** Ticket / bug / decision là file markdown đánh số trong repo, index bằng `STATUS.md`. Không database, không service — **`git` là toàn bộ backend**.

> **Repo hiện tại chỉ chứa scaffolding.** Toàn bộ 42 file tracked là markdown (+1 ảnh). Không có source code, không `package.json`, không build system → **không có lệnh build / lint / test nào để chạy**. Mọi "lệnh" trong repo này là thao tác trên file board (xem §6).

---

## 2. Điểm khởi động — đọc trước khi làm bất cứ việc gì

**Nơi làm việc thật là `management/`, không phải thư mục gốc này.**

Claude Code chỉ nạp `.claude/` theo thư mục làm việc. Nghĩa là một agent mở ở **gốc repo** (tình huống bạn đang ở) **không** có:

- 5 sub-agent trong `management/.claude/agents/`
- 4 skill trong `management/.claude/skills/`
- Các luật trong [management/CLAUDE.md](management/CLAUDE.md)

Vì vậy:

| Nếu việc cần làm là… | Thì… |
|---|---|
| Sửa README, LICENSE, cấu trúc chung, hoặc chính file này | Làm ngay tại gốc — đó là phạm vi của session gốc |
| Vận hành board (mở/đóng ticket), delegate cho sub-agent, chạy skill | **Bảo người dùng mở agent tại `management/`** — đó là hub. Nếu không thể, phải tự đọc và tuân thủ [management/CLAUDE.md](management/CLAUDE.md) + [management/pm-playbook.md](management/pm-playbook.md) thủ công |
| Sửa code trong một sub-project | Không làm từ đây. Việc đó thuộc sub-agent sở hữu surface đó |

---

## 3. Vai trò từng phần & cách nó thực thi vai trò

| Phần | Vai trò | Thực thi bằng cách nào |
|---|---|---|
| [management/CLAUDE.md](management/CLAUDE.md) | Hiến pháp của hub | Auto-load khi agent chạy trong `management/`; chốt luật tối cao "PM không viết code" và "board là file" |
| [management/pm-playbook.md](management/pm-playbook.md) | Operating model đầy đủ | Định nghĩa standard flow, **autonomy rubric 4 tín hiệu**, delegation brief, evidence bar, harness delta. Sửa file này = sửa chính sách, không phải ghi chú |
| [management/backlog/](management/backlog/) · [management/bugs/](management/bugs/) | Board of record | 1 file = 1 hạng mục (`NNNN-slug.md`). `STATUS.md` là index lane (Open / Awaiting Owner / Epics / Closed), **duy trì bằng tay nên luôn có thể lag** |
| [management/decisions/](management/decisions/) | Trí nhớ dài hạn của workspace | ADR đánh số ghi *lý do* chọn X thay vì Y; khi đảo ngược thì đánh dấu `Superseded by NNNN` chứ không xoá. [CANDIDATES.md](management/decisions/CANDIDATES.md) giữ câu hỏi chưa chốt |
| [management/templates/](management/templates/) | Khuôn dạng chuẩn | [ticket.md](management/templates/ticket.md) (bản light cho việc nhỏ + bản PRD cho việc nặng), [adr.md](management/templates/adr.md), [agent.md](management/templates/agent.md) |
| [management/.claude/agents/](management/.claude/agents/) | Người thi công | 5 agent: `api` · `app` · `android` · `ios` (mỗi agent sở hữu đúng 1 folder trong `projects/`, có scope fence tuyệt đối) + [`ops`](management/.claude/agents/ops.md) — agent duy nhất chạm public edge, mọi hành động không thể hoàn tác đều hard-gate trên Owner |
| [management/.claude/skills/](management/.claude/skills/) | Biến luật thành checklist tự cưỡng chế | 4 skill Tier-1, mỗi cái bảo vệ đúng một quy tắc hay bị bỏ qua khi vội — xem §5 |
| [projects/*/documents/](projects/) | **Luật của từng surface** | `architecture/`, `coding-conventions.md`, `response-format.md`. Agent sở hữu đọc *trước* khi code; xung đột giữa ticket và documents → **dừng và flag**, không tự chế cách giải quyết |

**Ba vai trò người/agent:**

- **Owner** — con người chịu trách nhiệm. Gate mọi quyết định sản phẩm, mọi thay đổi contract, mọi edge không thể hoàn tác.
- **PM agent** — agent chính chạy từ `management/`. Bàn intent, scope, ghi ticket, delegate, log outcome. **Không bao giờ viết code.**
- **Sub-agent** — mỗi surface một agent, mang theo convention riêng của surface đó.

---

## 4. Ba bất biến — vi phạm là hỏng mô hình

1. **PM không bao giờ viết implementation code.** Mọi thay đổi source đi qua agent sở hữu surface. PM tự code = bỏ qua convention của surface, đúng thứ khiến multi-agent còn nhất quán.
2. **`documents/` của sub-project là luật.** Agent đọc trước, và **dừng lại flag** khi ticket mâu thuẫn với documents — không tự hoà giải.
3. **Contract thì phải hỏi.** Response shape, permission, DB schema, URL công khai, CLI flag — bất cứ thứ gì consumer phụ thuộc vào. Sub-agent flag lên PM; PM route; không ai đổi lặng lẽ.

Kèm theo: **không scope creep trong ticket.** Thứ phát hiện giữa chừng → ticket mới hoặc mục "Out of scope", không âm thầm gộp vào.

---

## 5. Flow làm việc đề xuất

Rút gọn từ [pm-playbook.md](management/pm-playbook.md) — đọc bản đầy đủ khi cần chi tiết.

```
1. Intent          Owner nêu nhu cầu (hoặc đã có ticket sẵn)
2. Discuss         bàn, khảo sát, lặp lại — PM tổng hợp thành plan tại một breakpoint tự nhiên
3. Search board    quét STATUS.md (Open + Awaiting Owner + Epics): đã có ticket liên quan?
                   → CÓ: mở rộng nó hoặc thêm phase. TUYỆT ĐỐI không mở ticket anh em.
4. Rubric          chạy autonomy rubric 4 tín hiệu ↓
                   → cả 4 đều Auto: persist + delegate luôn, báo "đã bắt đầu"
                   → bất kỳ Ask nào: trình plan, chờ Owner duyệt TRƯỚC khi persist
5. Persist         viết backlog/NNNN-slug.md (hoặc bugs/), thêm dòng vào STATUS.md
6. Delegate        giao cho sub-agent: đường dẫn ticket + nội dung ticket CHÍNH LÀ spec
7. Execute         sub-agent build, trả về evidence dạng data (không phải văn xuôi)
8. Close           PM ghi Outcome vào chính file ticket + harness delta, chuyển lane trong STATUS.md
```

**Autonomy rubric — chỉ tự động khi cả bốn đều "Auto":**

| Tín hiệu | Auto | Ask |
|---|---|---|
| Blast radius | 1 surface / 1 sub-agent | Nhiều surface |
| Change type | Correction (fix bug, typo, revert) | Addition (feature/endpoint/screen mới) |
| Product decision | Không có — đáp án rõ ràng | Bất kỳ câu "A hay B?" nào đã nổi lên |
| Contract | Nội bộ | Thứ consumer phụ thuộc |

Chạm public deploy / DNS / registry → **không bao giờ auto**, luôn qua `ops` gate với cái gật đầu tường minh của Owner cho *từng* hành động.

**Skill nào ứng với bước nào** (chỉ dùng được khi agent chạy trong `management/`):

| Skill | Dùng ở bước | Ép cái gì |
|---|---|---|
| [`ticket-new`](management/.claude/skills/ticket-new/SKILL.md) | 3 → 5 | Search-before-open · lấy ID từ **folder** không phải STATUS · chạy đủ rubric trước khi persist |
| [`ticket-close`](management/.claude/skills/ticket-close/SKILL.md) | 8 | Evidence bar **có con số** · bắt buộc viết harness delta (thứ ai cũng quên) |
| [`board-doctor`](management/.claude/skills/board-doctor/SKILL.md) | khi STATUS lệch | Dò trùng ID, orphan, drift lane; rebuild STATUS.md từ folder |
| [`ingest`](management/.claude/skills/ingest/SKILL.md) | khi mang tài liệu cũ vào | Classify → confirm → write. Không bao giờ bulk-import mù |

**Evidence bar khi đóng ticket** — "done" cần bằng chứng, không cần cảm giác: test xanh *kèm số lượng*; build xanh *kèm tên công cụ*; và **hành vi được quan sát ở nơi nó chạy** (request thật, dev-server smoke, DOM render, boot trên simulator). Thiếu → không đóng.

**Harness delta** — mỗi ticket đóng lại phải trả lời *"ticket này dạy hệ thống điều gì?"* và PM **hành động ngay** (sửa playbook / mở ADR / mở ticket mới). `"None"` là câu trả lời hợp lệ; **bỏ trống thì không**.

---

## 6. Lệnh — toàn bộ command surface của repo

Không có build, không có test. Chỉ có thao tác board, và hai lệnh dưới đây quan trọng vì **lý do** đứng sau chúng:

```bash
# ID kế tiếp — LUÔN lấy từ folder, KHÔNG lấy từ STATUS.md.
# STATUS.md duy trì bằng tay nên lag; hơn nữa nó sort xuống cuối và sẽ trả về
# chính nó như "ticket trước đó" → grep '^[0-9]{4}' loại nó ra. Rỗng = chưa có ticket, bắt đầu 0001.
ls backlog/ | grep -E '^[0-9]{4}' | sort | tail -1
ls bugs/    | grep -E '^[0-9]{4}' | sort | tail -1

# Search-before-open (chạy trước mọi lần mở ticket mới)
grep -ril <keyword> backlog/ bugs/
```

> **Windows:** shell mặc định của session này là PowerShell, nơi `ls | grep` không tồn tại. Dùng **Bash tool** cho các lệnh POSIX trên, hoặc bản PowerShell tương đương:
> `Get-ChildItem backlog | Where-Object Name -match '^\d{4}' | Sort-Object Name | Select-Object -Last 1`

---

## 7. Quy ước đặt tên & định dạng

- `kebab-case` cho file và slug; `ALL_CAPS` cho file index (`STATUS.md`, `CANDIDATES.md`, `README.md`).
- Ticket ID là 4 chữ số, zero-pad: `0001-example-settings-screen.md`. ADR bắt đầu từ `0000` (baseline).
- **Ticket dùng header in đậm inline** (`**Status:** Open`), **không** dùng YAML frontmatter — [`board-doctor`](management/.claude/skills/board-doctor/SKILL.md) parse theo đúng dạng này, đừng đổi sang `---` fence.
- Sub-agent thì **có** YAML frontmatter (`name`, `description`) — đó là contract của Claude Code, xem [templates/agent.md](management/templates/agent.md).
- **Ticket đang mở thì viết lại, không thêm phụ lục.** Khi scope đổi, sửa thẳng các mục liên quan để body luôn đọc như vừa viết hôm nay. Chỉ khi ticket **Done** thì body mới đóng băng thành hồ sơ bất biến, và việc liên quan sau đó là ticket mới.

---

## 8. Đây là template — đừng nhầm ví dụ với thực tế

Những thứ sau là **hàng mẫu để học pattern**, xoá hoặc thay khi dùng thật, đừng bảo trì như code thật:

- `projects/api` · `projects/app` · `projects/android` · `projects/ios` — 4 stub sub-project. Khi dùng thật, mỗi surface là repo riêng: thả code vào folder, điền `documents/`, thêm `.claude/agents/<name>.md`.
- [backlog/0001-example-settings-screen.md](management/backlog/0001-example-settings-screen.md) — ticket ví dụ, tự nó ghi rõ "delete when you start".
- Stack trong mỗi agent (Node/React/Kotlin/Swift) là ví dụ — thay bằng stack thật của bạn.

**Khi nào rời file board:** board dạng file chỉ hợp với *một người, một máy*. Khi có teammate thứ hai (nhất là người không dùng git), agent chạy song song đụng nhau trên `STATUS.md`, hoặc cần permission model / audit trail — đó là lúc nâng cấp lên **Musher** (board chuyển vào database dùng chung). Playbook, rubric, template, sub-agent giữ nguyên; chỉ *nơi board sống* thay đổi. Xem [pm-playbook.md → "Nâng cấp lên team board"](management/pm-playbook.md).
