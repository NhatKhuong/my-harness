# Gangline

![Gangline — một đội worker đã harness, nối với nhau bằng một sợi dây, do người musher điều khiển](gangline.png)

**Hệ điều hành cho đội gồm người và coding agent.**

Trong môn chó kéo xe tuyết, *gangline* là sợi dây trung tâm nối harness của từng con chó vào xe. Harness làm một con chó hữu dụng; gangline biến cả đàn thành một đội. Mấy năm nay giới AI tập trung làm harness tốt hơn — Claude Code, Codex, Cursor, các bộ subagent. Gangline là lớp nằm trên: sợi dây nối nhiều harness — người và AI — thành một đội kéo cùng một tải.

> Nhiều harness. Một sợi dây.

## Ý tưởng

- **PM agent không viết code.** Một agent duy nhất điều phối: bàn intent, chia việc thành ticket, giao cho sub-agent theo từng surface. Mỗi sub-agent mang convention riêng của surface đó.
- **Agent có sơ đồ tổ chức.** Sub-agent thi công; Owner chốt những quyết định thuộc về mình. Ai được tự quyết — viết ra, version hoá, review được.
- **Board chỉ là file.** Ticket, bug, decision là file markdown đánh số trong repo, index bằng `STATUS.md`. Không database, không service, không tài khoản — `git` là backend.

Gangline **không** phải framework cài vào codebase, không phải bộ persona đóng vai agile team, không phải catalog subagent. Nó giả định harness của bạn đã chạy tốt, và lo phần tổ chức chúng.

## Dùng template này

```bash
git clone <your-workspace> my-workspace && cd my-workspace/management
# mở coding agent (Claude Code, …) NGAY TẠI management/
```

`management/` là hub — nhà của PM agent. Đọc [management/CLAUDE.md](management/CLAUDE.md) rồi [management/pm-playbook.md](management/pm-playbook.md) (flow, autonomy rubric, evidence bar).

Board có sẵn, không cần setup:

```
management/backlog/   ← work item, mỗi file một cái (STATUS.md là index)
management/bugs/      ← bug, cùng cấu trúc
management/decisions/ ← ADR: vì sao workspace được thiết kế như vậy
```

PM mở ticket bằng cách viết `management/backlog/NNNN-slug.md`, cập nhật lane trong `STATUS.md`, rồi giao cho sub-agent — ticket chính là spec.

## Cấu trúc

```
management/   ← hub: khởi chạy agent ở đây (CLAUDE.md, pm-playbook.md, board, sub-agent)
projects/     ← 4 stub ví dụ (api / app / android / ios), mỗi stub tự mô tả
```

Bốn thư mục trong `projects/` là **stub để học pattern** — giữ lại, hoặc thay bằng project thật (mỗi cái một repo). Mỗi stub có `documents/` riêng (architecture, conventions, response-format): đó là spec mà sub-agent build theo.

## Khi nào cần board xịn hơn

File board hoàn hảo cho một người, một máy. Có team rồi thì nó bắt đầu vỡ: `STATUS.md` lag so với ticket, hai người đụng nhau trên cùng một lane, người không dùng git thì không có cửa vào, và không có permission model hay audit trail.

Lúc đó nâng lên **Musher**: cùng operating model, board chuyển vào database dùng chung — web app cho người, `musher` CLI cho agent, một nguồn sự thật dưới một permission model. Playbook, rubric, template, sub-agent giữ nguyên; chỉ board đổi chỗ ở. Xem [pm-playbook.md → "Nâng cấp lên team board"](management/pm-playbook.md).

## Tác giả

**Kevin Nguyen** ([@bangnguyenanh](https://github.com/bangnguyenanh)).

## License

MIT © 2026 Kevin Nguyen — xem [LICENSE](LICENSE).
