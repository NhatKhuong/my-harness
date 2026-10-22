# Profile — backend (API / service / worker)

## Scan

* Router / controller: liệt kê `METHOD /path → handler` (đủ để điền bảng endpoint trong `03-contracts.md`).
* Middleware chain: auth, validation, error handler, rate limit, CORS.
* Validation: thư viện (zod, joi, class-validator, pydantic, Bean Validation) và nơi đặt.
* Data access: ORM / query builder, repository pattern, transaction boundary.
* Migration: tool, thư mục, forward-only hay có down.
* Background job / queue / cron, nếu có.
* Health check endpoint (`/health`, `/actuator/health`) → dùng cho smoke test.

## Interview (sau bộ chung)

1. Response envelope? — `(Recommended)` không envelope, trả resource trực tiếp + error shape thống nhất `{ error: { code, message, details? } }`
2. Pagination? — `(Recommended)` cursor-based `?cursor=&limit=`; khác: offset/page
3. Migration policy? — `(Recommended)` forward-only, không bao giờ sửa migration đã merge
4. Idempotency / retry cho thao tác ghi? — có header `Idempotency-Key` hay không

## Sections bổ sung khi write

**`architecture/01-overview.md`**: thêm mục *Request lifecycle* (request → middleware → handler → service → data → response) và *Background work*.

**`architecture/02-data-model.md`**: bắt buộc điền đủ (entity, quan hệ, migration policy).

**`architecture/03-contracts.md`**: bảng endpoint `Method | Path | Auth | Request | Response | Errors`, error shape, pagination, versioning. Có OpenAPI → link file, bảng chỉ liệt kê method + path + link.

**`testing.md`**: integration test với DB thật hay mock; cách seed data.

**`response-format.md`** — dòng Verified:

```
Verified: tests: N passed (<tool>) · build: pass (<tool>) · request: <METHOD path → status>
```

## Default verification

* Build: lệnh compile / typecheck.
* Test: unit + integration, báo số lượng.
* Behavior: boot server local, gửi request thật tới endpoint bị thay đổi, ghi status + body rút gọn.
