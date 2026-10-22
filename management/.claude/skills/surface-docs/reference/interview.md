# Interview — bộ câu hỏi chung

Dùng ở bước 3. Hỏi bằng AskUserQuestion, **≤4 câu mỗi round**, mỗi câu có một option `(Recommended)`.

* **Brownfield:** bỏ qua câu mà code đã trả lời rõ (nguồn `code:`). Chỉ hỏi để xác nhận những mục `inferred?` và những gì code không thể nói.
* **Greenfield:** đi hết các round. Default đề xuất lấy từ profile file.
* Sau phần chung, hỏi tiếp các câu trong mục "Interview" của profile.
* Owner chọn "Other" / giải thích thêm → ghi nguyên ý, nguồn `owner`.

## Round 1 — Danh tính & phạm vi

1. Surface này làm gì, cho ai? (một câu: domain + người dùng)
2. Consumer là ai? (web app, mobile app, service khác, bên thứ ba, người dùng cuối) → quyết định cái gì là **contract**
3. Stack chính? (ngôn ngữ · framework · datastore). Brownfield: chỉ xác nhận.
4. Surface này **không** làm gì? (ranh giới với surface khác, giúp `description` của agent rõ)

## Round 2 — Kiến trúc

1. Kiểu kiến trúc / layering? (vd. layered, hexagonal, feature-based, MVVM)
2. Quy tắc phụ thuộc giữa các layer? (ai được import ai)
3. Hạ tầng phụ thuộc? (DB, cache, queue, storage, service bên ngoài)
4. Config & môi trường? (env var, các môi trường: local / staging / prod)

## Round 3 — Contract (bắt buộc Owner xác nhận, không dùng default)

1. Contract công khai gồm những gì? (API shape, status code, error shape, URL, event, CLI flag, deep link…)
2. Versioning & breaking change: làm thế nào? (vd. `/v1`, header, không version, chỉ additive)
3. Auth & permission model? (ai được gọi gì, kiểm ở đâu)
4. Có spec formal không? (OpenAPI, proto, GraphQL schema) — nếu có, đó là source of truth

## Round 4 — Convention

1. Naming (file, type, function, biến)?
2. Error handling: throw, Result type hay error code? Lỗi trả cho consumer ra sao?
3. Logging & observability?
4. Comment / docs trong code: quy định gì? (mặc định Gangline: comment giải thích *vì sao*)

## Round 5 — Chất lượng & verification

1. Test framework và mức yêu cầu? (unit / integration / e2e, coverage tối thiểu)
2. Lệnh build / test / lint / run chuẩn?
3. "Behavior quan sát ở nơi nó chạy" cho surface này nghĩa là gì? (request thật, dev-server render, boot simulator…) → nuôi dòng `Verified:` trong `response-format.md`
4. Có gì tuyệt đối không được làm? (vd. không sửa migration cũ, không thêm dependency khi chưa hỏi)

## Round 6 — Domain & bảo mật

1. Thuật ngữ domain quan trọng? (từ → nghĩa → không nhầm với)
2. Dữ liệu nhạy cảm? (PII, payment, token) và quy tắc xử lý
3. Secret quản lý thế nào? (env, vault, CI secret)
4. Lựa chọn nào ở trên có lý do đáng lưu lâu dài? → đề xuất ADR
