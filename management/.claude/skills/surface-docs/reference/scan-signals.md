# Scan signals — từ file trong repo suy ra fact

Dùng ở bước 1 (detect profile) và bước 3 (brownfield). **Chỉ đọc**, không chạy lệnh có side effect. Mỗi fact ghi nguồn `code:<path>`.

## 1. Manifest → ngôn ngữ / framework / profile

| File | Ngôn ngữ / hệ | Đọc gì | Gợi ý profile |
| --- | --- | --- | --- |
| `package.json` | JS / TS | `dependencies`, `devDependencies`, `scripts`, `engines`, `type` | `express`/`fastify`/`@nestjs/*`/`hono` → backend · `react`/`vue`/`svelte`/`next`/`vite` → web · `react-native`/`expo` → mobile |
| `tsconfig.json` | TypeScript | `strict`, `paths`, `target` | — |
| `pom.xml` · `build.gradle(.kts)` có Spring | Java / Kotlin JVM | `spring-boot-starter-*`, Java version | backend |
| `build.gradle(.kts)` có `com.android.application` | Kotlin / Android | `minSdk`, `targetSdk`, `compose`, `hilt` | mobile (android) |
| `*.xcodeproj` · `Package.swift` · `Podfile` | Swift / iOS | deployment target, SwiftUI / UIKit, dependency | mobile (ios) |
| `pubspec.yaml` | Dart / Flutter | `dependencies`, `environment.sdk` | mobile |
| `pyproject.toml` · `requirements*.txt` | Python | `fastapi`/`django`/`flask`, tool config (`ruff`, `pytest`, `mypy`) | backend |
| `go.mod` | Go | module, `gin`/`echo`/`chi` | backend |
| `Cargo.toml` | Rust | `axum`/`actix-web` | backend |
| `*.csproj` | .NET | `Microsoft.AspNetCore.*` | backend |
| `composer.json` | PHP | `laravel/framework`, `symfony/*` | backend |
| `Gemfile` | Ruby | `rails` | backend |

Lockfile (`package-lock.json`, `pnpm-lock.yaml`, `yarn.lock`, `bun.lockb`, `poetry.lock`, `uv.lock`) → **package manager**, quyết định lệnh trong `commands.md`.

## 2. Lệnh build / test / run

Ưu tiên theo thứ tự (cao → thấp), ghi **nguyên văn**:

1. `scripts` trong `package.json`, `Makefile` / `justfile` / `Taskfile.yml` targets.
2. Lệnh CI thật: `.github/workflows/*.yml`, `.gitlab-ci.yml`, `azure-pipelines.yml`, `Jenkinsfile`. CI chạy gì = lệnh "chuẩn".
3. Wrapper: `./mvnw`, `./gradlew`.
4. Convention của tool (`pytest`, `go test ./...`, `cargo test`, `dotnet test`): chỉ dùng khi 1–3 không có, nguồn ghi `inferred?`.

## 3. Cấu trúc & kiến trúc

| Tín hiệu | Fact |
| --- | --- |
| Top-level folder của `src/` (`routes/ controllers/ services/ repositories/`, `domain/ application/ infrastructure/`, `features/`, `modules/`) | Layering / kiểu kiến trúc |
| Entry point (`main.*`, `index.*`, `server.*`, `App.*`, `Application.java`) | Boot flow |
| `Dockerfile`, `docker-compose*.yml` | Runtime, service phụ thuộc (DB, cache, queue) |
| `.env.example`, `config/*`, `application*.yml`, `settings.py` | **Tên** biến môi trường (không bao giờ giá trị) |
| `.editorconfig`, `.prettierrc*`, `.eslintrc*`/`eslint.config.*`, `ruff.toml`, `.swiftlint.yml`, `detekt.yml`, `checkstyle.xml` | Formatting / lint rule đã được cưỡng chế bằng tool → ghi "do tool cưỡng chế", không chép lại từng rule |

## 4. Data & contract

| Tín hiệu | Fact |
| --- | --- |
| `migrations/`, `prisma/schema.prisma`, `db/migrate/`, `alembic/`, `flyway`/`liquibase`, `*.sql` | Datastore, cách migrate, entity chính |
| ORM model / entity class | Data model |
| `openapi.*`, `swagger.*`, `*.proto`, `schema.graphql` | Contract đã formal hoá → link, không chép lại |
| Router / controller definitions | Danh sách endpoint (method + path) |
| Error handler / middleware chung | Error shape |
| Auth middleware, guard, decorator `@PreAuthorize`, policy | Auth & permission model |
| Client gọi API (`fetch`/`axios` base URL, Retrofit, URLSession) | Contract **phía consumer** |

## 5. Test

| Tín hiệu | Fact |
| --- | --- |
| Config: `jest.config.*`, `vitest.config.*`, `pytest.ini`/`[tool.pytest]`, `playwright.config.*`, `cypress.config.*` | Framework test |
| Vị trí file `*.test.*`, `*_test.*`, `test/`, `__tests__/`, `src/test/`, `androidTest/` | Quy ước đặt test |
| Coverage threshold trong config / CI | Ngưỡng coverage |
| Fixture, factory, testcontainers, mock server | Cách dựng dữ liệu test |

## 6. Convention suy ra từ code

Chỉ ghi khi **nhất quán ở ≥3 chỗ**, kèm 1–2 đường dẫn ví dụ. Ít hơn hoặc không nhất quán → `inferred?` và hỏi Owner.

* Naming file / class / function / biến.
* Cách trả lỗi (throw typed error, Result type, error code).
* Logging (thư viện, structured hay không, chỗ log).
* Import style (alias `@/`, relative), barrel file.
* Dependency injection.
