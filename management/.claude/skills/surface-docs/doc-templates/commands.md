# Commands

Lệnh **nguyên văn**, chạy từ `projects/{{surface}}/`. Verification bar của agent lấy lệnh từ đây; lệnh trong agent file phải khớp file này.

<!-- Nguồn ưu tiên: scripts/Makefile → CI → wrapper → convention của tool (ghi inferred?). -->

| Mục đích | Lệnh | Kết quả "xanh" trông thế nào |
| --- | --- | --- |
| Cài dependency | {{npm ci}} | {{exit 0}} |
| Build | {{npm run build}} | {{dòng kết quả, vd. "built in 2.1s"}} |
| Type check | {{npx tsc --noEmit}} | {{không có output}} |
| Lint | {{npm run lint}} | {{0 problems}} |
| Test (unit) | {{npm test}} | {{"Tests: N passed"}} |
| Test (integration / e2e) | {{…}} | {{…}} |
| Chạy local | {{npm run dev}} | {{"listening on :3000"}} |
| Smoke | {{curl -s localhost:3000/health}} | {{200 {"status":"ok"}}} |

## Điều kiện trước

- {{runtime version, service phụ thuộc phải chạy (vd. docker compose up -d db), file .env lấy từ .env.example}}

## Không được chạy

- {{vd. lệnh deploy / publish / migrate prod — thuộc ops, hard-gate Owner}}

Nguồn: {{code:… / owner / inferred?}}

*Cập nhật lần cuối: {{YYYY-MM-DD}} — đổi nội dung thì cập nhật luôn dòng này.*
