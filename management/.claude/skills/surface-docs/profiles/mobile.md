# Profile — mobile (Android / iOS / cross-platform)

## Scan

* Platform & UI toolkit: Compose / Views, SwiftUI / UIKit, React Native, Flutter.
* Version: `minSdk` / `targetSdk` / `compileSdk`; iOS deployment target; Xcode / Gradle / AGP version.
* Kiến trúc: MVVM / MVI / TCA / Clean; DI (Hilt, Koin, Swinject, Factory).
* Navigation: Navigation Compose, NavigationStack, coordinator, React Navigation, go_router.
* Networking & persistence: Retrofit/Ktor, URLSession/Alamofire; Room, DataStore, Core Data, SwiftData, SQLite.
* Concurrency: coroutines/Flow, async/await, Combine.
* Build variant / flavor / scheme; signing config (**chỉ ghi là có, nơi cấu hình — không ghi giá trị**).
* Permission khai báo (`AndroidManifest.xml`, `Info.plist`).

## Interview (sau bộ chung)

1. Kiến trúc màn hình? — `(Recommended)` MVVM: View ↔ ViewModel (state bất biến) → Repository → data source
2. Offline? — online-only / cache-đọc / offline-first có sync
3. Thiết bị / version tối thiểu phải hỗ trợ, có tablet / dark mode / landscape không?
4. Release & signing do ai giữ? — `(Recommended)` chỉ `ops`, hard-gate trên Owner; surface agent không bao giờ chạm signing / store

## Sections bổ sung khi write

**`architecture/01-overview.md`**: thêm *Screen & navigation map*, *Platform constraints* (min version, permission), *Build variants*.

**`architecture/02-data-model.md`**: local persistence (entity, migration của Room/Core Data) nếu có; không có thì `N/A`.

**`architecture/03-contracts.md`**: API mà app phụ thuộc (link docs backend), deep link / universal link, push payload, permission xin người dùng, analytics event. Ghi chú: **app đã phát hành không thể rút lại** → contract phía app khó đổi hơn backend.

**`security.md`**: lưu token ở đâu (Keystore / Keychain), certificate pinning, không log PII.

**`response-format.md`** — dòng Verified:

```
Verified: tests: N passed (<tool>) · build: pass (<tool, variant>) · boot: <emulator/simulator + OS version → màn hình quan sát được>
```

## Default verification

* Build: `./gradlew assembleDebug` / `xcodebuild … build` (lấy lệnh thật từ `commands.md`).
* Test: unit test (+ UI test nếu có), báo số lượng.
* Behavior: boot trên emulator / simulator, đi tới màn hình bị thay đổi, ghi điều quan sát được.
