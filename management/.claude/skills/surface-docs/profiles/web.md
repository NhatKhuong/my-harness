# Profile — web (SPA / SSR / web client)

## Scan

* Framework & rendering: SPA (Vite/CRA), SSR/SSG (Next, Nuxt, Remix, Astro).
* Routing: file-based hay config; liệt kê route chính.
* State: server state (React Query, SWR, RTK Query) vs client state (Redux, Zustand, Pinia, Context).
* Styling: Tailwind, CSS Modules, styled-components, UI kit (shadcn, MUI, Ant); file design token / theme.
* API client: base URL lấy từ env nào, có generated client (OpenAPI codegen) không.
* Form & validation library.
* i18n, accessibility lint (`eslint-plugin-jsx-a11y`), e2e (Playwright / Cypress).

## Interview (sau bộ chung)

1. Tổ chức code? — `(Recommended)` feature-based (`src/features/<feature>/{components,hooks,api}`), shared UI trong `src/components/ui`
2. Server state lấy thế nào? — `(Recommended)` một thư viện server-state duy nhất; không fetch trực tiếp trong component
3. Design system / token: nguồn ở đâu? Agent được tự tạo component mới hay phải dùng kit có sẵn?
4. Browser / thiết bị hỗ trợ, yêu cầu a11y (vd. WCAG AA)?

## Sections bổ sung khi write

**`architecture/01-overview.md`**: thêm *Routing map* (route → page → quyền truy cập) và *State strategy*.

**`architecture/02-data-model.md`**: thường `N/A — client không sở hữu persistence`, thay bằng *Client data shapes* (type/model nhận từ API) nếu có.

**`architecture/03-contracts.md`**: ghi **contract mà web phụ thuộc** (endpoint nào của backend, link sang docs của surface đó) và **contract web cung cấp** (public URL, deep link, query param, SEO meta, analytics event).

**`coding-conventions.md`**: thêm *Component rules* (khi nào tách component, props naming, không inline style ngoài token) và *Styling rules*.

**`response-format.md`** — dòng Verified:

```
Verified: tests: N passed (<tool>) · build: pass (<tool>) · render: <route → điều quan sát được trên dev server>
```

## Default verification

* Build: production build (`vite build`, `next build`…) + typecheck.
* Test: unit/component test, báo số lượng.
* Behavior: chạy dev server, mở route bị thay đổi (ưu tiên skill `browser-automation` nếu có), ghi console error = 0 và DOM/screen quan sát được.
