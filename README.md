# 🛡️ RPG Maker Asset Security & Pipeline Suite v4.3.0

<img width="929" height="964" alt="Image" src="https://github.com/user-attachments/assets/7be6977a-0d3d-49a7-9492-c33a1b24ba8c" />

<img width="934" height="902" alt="Image" src="https://github.com/user-attachments/assets/e1674ac5-c24b-4447-8bd7-f372ccc04eee" />

A robust asset encryption and build pipeline designed for **RPG Maker (MZ, MV, VX Ace, XP, VX)** developers. Safeguard proprietary game logic, graphics, audio, and database assets against unauthorized extraction, reverse engineering, and tampering — powered by a **Non-Blocking 60.0 FPS Stealth Virtual Runtime** and **Smart LRU RAM Cache**.

---

## 🏗️ Architecture Overview

The suite provides a dual-tool workflow tailored for game production environments:
1. **Packaging & Obfuscation Engine (`RPGMakerEncryptor.exe` - v4.2.3)**: Encrypts raw game directories into compact, protected binary blobs (`.0` containers) and embeds a camouflaged, in-memory runtime bridge with automatic RAM lifecycle management for the game engine.
2. **Authorized Asset Recovery Utility (`RPGMakerDecrypter.exe` - v4.2.3)**: An internal pipeline tool allowing authorized project owners to extract, audit, or rebuild project sources using their private cryptographic keys.

```
                    ┌─────────────────────────┐
                    │    Raw Project Source   │
                    │ (data/, img/, js/, ...) │
                    └────────────┬────────────┘
                                 │
                   [ RPGMakerEncryptor.exe ]
                  (AES-256 / PBKDF2 / Keygen)
                                 │
         ┌───────────────────────┴───────────────────────┐
         ▼                                               ▼
┌─────────────────────────┐             ┌─────────────────────────┐
│     Production Build    │             │   Private Security Key  │
│   - Obfuscated (.0)     │             │  (Keep Secret / Vault)  │
│   - Stealth RAM Runtime │             └────────────┬────────────┘
└─────────────────────────┘                          │
         │                                           │
         │ (Game Launch:                             │ (Internal Pipeline /
         │  Decrypted in-memory                      │  Recovery / Auditing)
         │  with Smart RAM Cache)                    ▼
                                        [ RPGMakerDecrypter.exe ]
                                                     │
                                                     ▼
                                        ┌─────────────────────────┐
                                        │   Restored Work Source  │
                                        └─────────────────────────┘
```

---

## 📦 Binaries

Location: `bin/`
- **`RPGMakerEncryptor.exe`** *(v4.2.3)*: Deployment & encryption interface.
- **`RPGMakerDecrypter.exe`** *(v4.2.3)*: Authorized recovery & pipeline restoration utility.

---

## ✨ Features & Capabilities

### 1. Production Encryption Tool (`RPGMakerEncryptor.exe`)
- **Automated Directory Verification**:
  - 🟢 **Verified**: Automatically detects valid RPG Maker structures (JSON databases, plugins, asset layouts).
  - 🔴 **Unverified**: Flags missing or abnormal directory trees to prevent incomplete or corrupt builds.
- **Cryptographic Key Management**:
  - Secure pseudo-random key generation (`RPG-XXXX-XXXX-XXXX-XXXX`).
  - Automatic timestamped key file export (`RPG_Maker_Key_[Timestamp].txt`) for developer archives and secrets management.
  - One-click key regeneration and clipboard copy.
- **Configurable Security Profiles (Advanced Settings)**:
  - **Easy (Low Overhead)**: Fast In-Place XOR Stream Obfuscation for lightweight prototype distribution.
  - **Medium**: Industry-standard AES-128 bit block encryption.
  - **Advanced (Production Recommended)**: Military-grade AES-256 bit + PBKDF2 key derivation.
- **Production Asset Hardening & Smart Memory Pipeline**:
  - Packs core proprietary directories into protected payloads (`data.0`, `img.0`, `audio.0`) while keeping `fonts/` and `js/` intact for seamless engine initialization.
  - Integrates a camouflaged **Stealth Virtual Runtime Loader** (`js/libs/pixi-gl-shim.js`) with polymorphic split-key masking to stream encrypted assets directly into the engine heap without writing unencrypted assets to disk or exposing plaintext keys.
  - **Smart Bounded LRU + TTL RAM Cache**: Dynamically caches hot assets in RAM during gameplay and automatically purges buffers, revokes `Blob URLs`, and closes file descriptors upon game exit or reload (`F5`) to prevent memory leaks, crashes, or BSODs.
  - Automated pre-build backup routines (`_RPG_Original_Backup_...`) or direct export to a separate build directory.

### 2. Internal Recovery Utility (`RPGMakerDecrypter.exe`)
- **Pipeline Integrity Verification**: Validates encrypted container headers prior to processing.
- **Private Key Authentication**: Requires the exact symmetric key generated during the build step.
- **Lossless Project Restoration**:
  - Reconstructs complete directory structures (`data/`, `img/`, `audio/`, `js/`) with byte-for-byte fidelity.
  - Unhooks runtime injection hooks from `index.html` and removes runtime shims to restore clean development environments.

---

## 🚀 Workflow Guide

### Production Build (Encryption)
1. Launch `RPGMakerEncryptor.exe`.
2. Click **Select Folder Game** and select your project root.
3. Confirm the status displays 🟢 **Verified**.
4. Configure target cipher settings in **Advanced Settings** if needed.
5. Click **START ENCRYPTING GAME (ENCRYPT)**.
6. Securely store the output key (`RPG_Maker_Key_[Timestamp].txt`) in your private credentials vault or password manager. **Do not distribute this key with public game releases.**

### Source Recovery & Internal Auditing
1. Launch `RPGMakerDecrypter.exe`.
2. Select the packaged game directory (containing `.0` containers).
3. Specify the destination directory (defaults to `[GameTitle]_DecryptedSource`).
4. Enter the matching project recovery key.
5. Click **START DECRYPTION & UNPACK SOURCE**.

---

## ❓ Frequently Asked Questions (FAQ)

### 1. Will Windows Defender or Antivirus flag the tool or encrypted game as a False Positive?
- **No.** For your packaged game, this suite **never** patches, packs, or modifies the native `Game.exe` (NW.js / Chromium) binary. All assets are stored in `.0` data containers and bridged via standard JavaScript APIs, keeping the executable's PE structure and digital signature 100% untouched.
- Both `RPGMakerEncryptor.exe` and `RPGMakerDecrypter.exe` are compiled cleanly using the official Microsoft .NET Framework compiler (`csc.exe`) without third-party binary packers (such as UPX or cracked protectors), ensuring clean reputation across Windows Defender and SmartScreen.

### 2. Is it compatible with VisuStella, Yanfly, MogHunter, Galv, and heavy plugin suites?
- **Yes, 100% compatible.** Unlike basic file-extension encrypters (`.rpgmvp` / `.png_`), the v4.2.3 Virtual Runtime intercepts all four underlying engine I/O layers simultaneously:
  1. `XMLHttpRequest` (`DataManager`, `WebAudio`, dynamic JSON loaders).
  2. `HTMLImageElement.src` & `HTMLMediaElement.src` (`ImageManager`, `Bitmap`, `PIXI.Loader`).
  3. `window.fetch` (RPG Maker MZ core & **VisuStella MZ** asynchronous loaders).
  4. Node.js `fs` module (`existsSync`, `readFileSync`, `readFile`, `statSync`, `readdirSync` — including **Virtual Directory emulation**). Plugins that scan folders dynamically (e.g., `fs.existsSync('img/pictures')` or `fs.readdirSync`) work seamlessly as if the raw folders were still on disk.

### 3. Does it cause lag or frame drops when loading large BGM tracks or huge maps? How is 60.0 FPS maintained?
- **Zero lag — locked at 60.0 FPS.** In v4.2.3, all asynchronous asset reads (`XMLHttpRequest`, `fetch`, `fs.readFile`) are offloaded to Node.js's background C++ `libuv` worker thread pool (`fs.read`) rather than blocking the main WebGL/PixiJS render thread.
- Completed buffers are dispatched via high-priority Microtasks (`Promise.resolve`) in `< 0.05 ms` instead of throttled `setTimeout` timers, and hot `Blob URLs` are reused within the LRU window so Chromium's Blink engine reuses decoded GPU textures instantaneously without re-decoding PNGs during combat animations.

### 4. How fast is the game startup time after encryption?
- **Instantaneous (often faster than unencrypted builds on HDDs).** At launch, the runtime does not unpack the entire archive into RAM; it only reads the compact **Table of Contents (TOC)** header (`1–3 ms`) and asynchronously pre-warms `System.json` and `MapInfos.json`. Consolidating thousands of loose files into contiguous `.0` containers also eliminates OS file-handle overhead at boot.

### 5. How does the Smart RAM Cache work? Can long play sessions cause high RAM usage, crashes, or BSODs?
- **Memory stays flat and is 100% freed upon exit.** The runtime enforces a strict **64 MB LRU (Least Recently Used) + TTL RAM Cache**:
  - Hot sprites and audio buffers are cached for `25 seconds` during active scenes and automatically evicted during browser idle periods (`requestIdleCallback`) or when the game window is minimized (`visibilitychange`).
  - When the player closes the game or reloads (`F5`), the `_dispose` lifecycle hook immediately zeroes out cached buffers, revokes all `Blob URLs`, closes all `.0` file descriptors (`fs.closeSync`), and triggers V8 garbage collection (`gc()`).

### 6. Can curious players find `rpg_protect_runtime.js` or inspect the decryption key inside `js/plugins`?
- **No.** The legacy `rpg_protect_runtime.js` and `.rpgp_bak` files have been completely eliminated. The runtime bridge is disguised as a standard PixiJS WebGL compatibility library (`js/libs/pixi-gl-shim.js`).
- Inside the shim, the session key is **never** stored in plaintext or hex (`KEY_HEX`). Instead, it is split across 4 polymorphic random byte masks and a shuffled permutation table, reconstructed only in a private closure at startup, and immediately wiped (`fill(0)`) from the mask arrays. All hooked functions also spoof `[native code]` when `.toString()` is inspected.

### 7. Which Windows versions are best supported?
- **Windows 10 and Windows 11 (64-bit)** provide the best WebGL hardware acceleration and memory management for both the tools and encrypted games, while full backward compatibility is maintained for **Windows 7 SP1 / Windows 8.1**.

---

## 🔒 Security Best Practices for Developers

- **Key Segregation**: The decryption utility is strictly for project maintainers. Never ship `RPGMakerDecrypter.exe` or exported key files inside public release packages, game installers, or public repositories.
- **Version Control**: Add `*.txt` key files and `_RPG_Original_Backup_*/` to your `.gitignore` to prevent accidental credential leakage.
- **Code Hardening**: For maximum protection on commercial releases, combine this suite with JavaScript obfuscation (e.g., Javascript-Obfuscator) on your custom plugin scripts prior to packaging.

---

- TIẾNG VIỆT -

# 🛡️ RPG Maker Asset Security & Pipeline Suite v4.2.3 (Bản Tiếng Việt)

Bộ giải pháp đóng gói, mã hóa và bảo vệ tài nguyên chuyên dụng dành cho nhà phát triển game **RPG Maker (MZ, MV, VX Ace, XP, VX)**. Giúp ngăn chặn triệt để hành vi trích xuất tài nguyên trái phép, dịch ngược mã nguồn (decompile) hoặc can thiệp/gian lận dữ liệu dự án — tích hợp **Stealth Virtual Runtime 60.0 FPS** và cơ chế **Smart LRU RAM Cache** tự động giải phóng tài nguyên.

---

## 🏗️ Tổng Quan Kiến Trúc Hệ Thống

Bộ công cụ cung cấp quy trình khép kín gồm hai phần mềm phục vụ trực tiếp cho quá trình sản xuất game:
1. **Trình Mã Hóa & Đóng Gói Tài Nguyên (`RPGMakerEncryptor.exe` - v4.2.3)**: Mã hóa các thư mục dữ liệu gốc thành các gói container nhị phân bảo mật (`.0`) và tích hợp cơ chế nạp ảo tàng hình trực tiếp trên bộ nhớ RAM kèm bộ quản lý vòng đời bộ nhớ tự động khi game khởi chạy.
2. **Công Cụ Phục Hồi & Kiểm Toán Mã Nguồn (`RPGMakerDecrypter.exe` - v4.2.3)**: Tiện ích nội bộ dành riêng cho tác giả dự án, hỗ trợ trích xuất, khôi phục lại mã nguồn sạch từ bản đóng gói thông qua khóa bảo mật riêng (Private Key).

```
                    ┌─────────────────────────┐
                    │    Mã Nguồn Dự Án Gốc   │
                    │ (data/, img/, js/, ...) │
                    └────────────┬────────────┘
                                 │
                   [ RPGMakerEncryptor.exe ]
                  (AES-256 / PBKDF2 / Keygen)
                                 │
         ┌───────────────────────┴───────────────────────┐
         ▼                                               ▼
┌─────────────────────────┐             ┌─────────────────────────┐
│   Bản Phát Hành Đóng Gói│             │     Khóa Bảo Mật Riêng  │
│   - Tệp container (.0)  │             │  (Lưu Trữ An Toàn Tuyệt │
│   - Stealth RAM Runtime │             │   Đối Trong Kho Khóa)   │
└─────────────────────────┘             └────────────┬────────────┘
         │                                           │
         │ (Khởi chạy game:                          │ (Dành cho Dev: Khôi
         │  Giải mã tức thì trên RAM,                │  phục & Kiểm toán)
         │  tự giải phóng khi tắt game)              ▼
                                        [ RPGMakerDecrypter.exe ]
                                                     │
                                                     ▼
                                        ┌─────────────────────────┐
                                        │    Mã Nguồn Hoàn Chỉnh  │
                                        │   (Phục hồi nguyên bản) │
                                        └─────────────────────────┘
```

---

## 📦 Các Tệp Thực Thi (.exe)

Thư mục: `bin/`
- **`RPGMakerEncryptor.exe`** *(v4.2.3)*: Ứng dụng đóng gói và triển khai mã hóa dự án.
- **`RPGMakerDecrypter.exe`** *(v4.2.3)*: Công cụ khôi phục mã nguồn và đồng bộ asset nội bộ.

---

## ✨ Tính Năng & Năng Lực Cốt Lõi

### 1. Trình Mã Hóa Triển Khai (`RPGMakerEncryptor.exe`)
- **Tự Động Xác Thực Thư Mục Dự Án**:
  - 🟢 **Xác Thực Thành Công (Verified)**: Tự động phân tích và nhận diện cấu trúc game RPG Maker hợp chuẩn (cơ sở dữ liệu JSON, plugin, cấu trúc cây thư mục).
  - 🔴 **Chưa Xác Thực (Unverified)**: Cảnh báo khi cấu trúc dự án bất thường hoặc thiếu file, tránh tình trạng đóng gói lỗi bản build.
- **Hệ Thống Quản Lý Khóa Mã Hóa Mật Mã Học**:
  - Tự động sinh khóa ngẫu nhiên độ an toàn cao (`RPG-XXXX-XXXX-XXXX-XXXX`).
  - Tự động kết xuất file lưu trữ khóa kèm tem thời gian (`RPG_Maker_Key_[Timestamp].txt`) phục vụ quản lý bí mật nội bộ.
  - Hỗ trợ đổi mới khóa (regenerate) và sao chép nhanh vào clipboard chỉ với một click.
- **Hồ Sơ Bảo Mật Tùy Biến (Thiết Lập Nâng Cao)**:
  - **Dễ (Mức nhẹ)**: Cơ chế Fast In-Place XOR Stream Obfuscation, tối ưu hiệu năng cho các bản demo hoặc thử nghiệm nội bộ.
  - **Trung Bình**: Tiêu chuẩn mã hóa khối AES-128 bit.
  - **Nâng Cao (Khuyên Dùng Cho Bản Chính Thức)**: Chuẩn bảo mật AES-256 bit kết hợp thuật toán dẫn xuất khóa PBKDF2.
- **Tăng Cường Phòng Vệ Tài Nguyên & Quản Lý RAM Thông Minh**:
  - Gói gọn toàn bộ các thư mục bản quyền nhạy cảm thành tệp container bảo vệ (`data.0`, `img.0`, `audio.0`), giữ nguyên `fonts/` và `js/` để engine khởi chạy mượt mà.
  - Nhúng cơ chế **Stealth Virtual Runtime Loader** (ngụy trang thành thư viện đồ họa chuẩn `js/libs/pixi-gl-shim.js` với khóa phân mảnh đa hình): stream trực tiếp dữ liệu đã mã hóa vào engine thông qua bộ nhớ RAM, không lộ file `rpg_protect_runtime.js`, không lưu khóa giải mã dạng rõ và tuyệt đối không xả file đệm ra ổ đĩa.
  - **Smart Bounded LRU + TTL RAM Cache**: Tạo bộ nhớ đệm thông minh giới hạn trần trong RAM khi mở game và tự động giải phóng 100% bộ nhớ đệm, thu hồi `Blob URL` & đóng file handle ngay khi tắt game hoặc nhấn `F5`, chống tràn RAM, crash hay màn hình xanh (BSOD).
  - Tự động sao lưu dự phòng toàn bộ source gốc trước khi build (`_RPG_Original_Backup_...`) hoặc xuất ra thư mục riêng biệt.

### 2. Tiện Ích Phục Hồi Nội Bộ (`RPGMakerDecrypter.exe`)
- **Kiểm Tra Tính Toàn Vẹn Của Pipeline**: Tự động rà soát header của các container mã hóa trước khi xử lý.
- **Xác Thực Khóa Đối Xứng**: Yêu cầu chính xác khóa mật mã được tạo ra ở bước đóng gói.
- **Khôi Phục Nguyên Vẹn 100%**:
  - Tái tạo cấu trúc cây thư mục gốc (`data/`, `img/`, `audio/`, `js/`) với độ chuẩn xác từng byte.
  - Gỡ bỏ hook runtime được chèn trong `index.html` và dọn sạch file shim, đưa mã nguồn về trạng thái lập trình sạch ban đầu.

---

## 🚀 Hướng Dẫn Quy Trình Sử Dụng

### Quy Trình Đóng Gói Bản Build (Mã Hóa):
1. Khởi chạy `RPGMakerEncryptor.exe`.
2. Bấm **Chọn Thư Mục Game** và điều hướng tới thư mục gốc của dự án.
3. Kiểm tra đèn trạng thái hiển thị 🟢 **Verified**.
4. Cấu hình thuật toán bảo vệ mong muốn tại phần **Thiết Lập Nâng Cao** (mặc định khuyến nghị: AES-256).
5. Bấm **BẮT ĐẦU MÃ HÓA GAME (ENCRYPT)**.
6. Lưu trữ tệp khóa được tạo ra (`RPG_Maker_Key_[Timestamp].txt`) vào phần mềm quản lý mật khẩu hoặc két an toàn của đội ngũ phát triển. **Tuyệt đối không đính kèm file khóa này vào bản game thương mại gửi cho người chơi.**

### Quy Trình Phục Hồi Mã Nguồn & Kiểm Toán Nội Bộ:
1. Khởi chạy `RPGMakerDecrypter.exe`.
2. Chọn thư mục game đã đóng gói (chứa các tệp container `.0`).
3. Chỉ định thư mục trích xuất đích (mặc định là `[TênGame]_DecryptedSource`).
4. Nhập chính xác mã khóa bí mật của dự án.
5. Bấm **BẮT ĐẦU GIẢI MÃ & BUNG SOURCE (DECRYPT)**.

---

## ❓ Câu Hỏi Thường Gặp (FAQ)

### 1. Công cụ và game sau khi mã hóa có bị Windows Defender báo virus (False Positive) không?
- **Không.** Đối với bản game sau khi mã hóa, công cụ **hoàn toàn không** can thiệp, tiêm mã nhị phân hay đóng gói (pack/patch) vào file thực thi `Game.exe` (NW.js / Chromium). Toàn bộ dữ liệu được đóng gói vào các tệp `.0` và đọc qua giao diện chuẩn của engine, giữ nguyên 100% chữ ký số và cấu trúc PE của `Game.exe` nên không bao giờ bị Windows Defender hay Antivirus báo nhầm trên máy người chơi.
- Đối với 2 file công cụ (`RPGMakerEncryptor.exe` & `RPGMakerDecrypter.exe`), phần mềm được biên dịch trực tiếp bằng trình biên dịch chính thức của Microsoft (`csc.exe` .NET Framework), không sử dụng các trình nén EXE lậu (như UPX hay VMProtect crack) nên hoàn toàn sạch và minh bạch với Windows SmartScreen.

### 2. Nó có tương thích với VisuStella, Yanfly, MogHunter, Galv hay các Plugin nặng không?
- **Tương thích tuyệt đối 100%.** Khác với các trình mã hóa thông thường chỉ đổi đuôi file `.rpgmvp` / `.png_`, **Stealth Virtual Runtime v4.2.3** đồng bộ hóa cả 4 giao thức truy xuất dữ liệu ở tầng thấp nhất của Engine:
  1. `XMLHttpRequest` (dùng bởi `DataManager`, `WebAudio`, các plugin nạp JSON động).
  2. `HTMLImageElement.src` & `HTMLMediaElement.src` (dùng bởi `ImageManager`, `Bitmap`, `PIXI.Loader`, MogHunter, Galv).
  3. `window.fetch` (dùng bởi RPG Maker MZ và hệ sinh thái plugin **VisuStella MZ**).
  4. Module `fs` của Node.js (`existsSync`, `readFileSync`, `readFile`, `statSync`, `readdirSync` — bao gồm cả cơ chế **Virtual Directory** giả lập thư mục ảo). Các plugin của **Yanfly / VisuStella** thường xuyên kiểm tra thư mục bằng `fs.existsSync('img/pictures')` hay `fs.readdirSync` vẫn nhận diện được toàn bộ file bên trong gói `.0` như khi chưa đóng gói.

### 3. Nó có làm giật lag khi tải nhạc BGM / Map lớn không? Hiệu năng FPS khi chơi thực tế như thế nào?
- **Không giật lag — giữ vững tốc độ khung hình chuẩn 60.0 FPS ổn định.**
- Ở phiên bản `v4.2.3`, toàn bộ tác vụ đọc dữ liệu từ các gói `.0` cho `XMLHttpRequest`, `fetch` và `fs.readFile` được xử lý **bất đồng bộ (Non-Blocking Async I/O)** trên luồng nền C++ `libuv` của Node.js thay vì chặn luồng dựng hình WebGL/PixiJS.
- Dữ liệu sau khi giải mã được phản hồi tức thì qua **Microtask (`Promise.resolve`)** chỉ trong `< 0.05 ms` (loại bỏ hoàn toàn độ trễ 4ms–16ms của `setTimeout` từng làm FPS dao động ở mức `56.6–57.9 FPS`), kết hợp tái sử dụng `Blob URL` nóng để nhân Chromium Blink lấy thẳng GPU Texture đã giải mã mà không cần decode lại file PNG khi hiển thị hiệu ứng chiến đấu liên tục.

### 4. Tốc độ mở game (Startup Time) sau khi mã hóa như thế nào?
- **Khởi động tức thì (thậm chí nhanh hơn bản gốc trên ổ cứng HDD).** Khi mở game, hệ thống không giải mã toàn bộ hàng GB dữ liệu ra RAM mà chỉ đọc bảng mục lục **TOC (Table of Contents)** ở cuối mỗi file `.0` (chỉ mất `1–3 ms`) và nạp sẵn bất đồng bộ `System.json` cùng `MapInfos.json`. Việc gom hàng ngàn file nhỏ lẻ vào 3 tệp tuần tự (`data.0`, `img.0`, `audio.0`) giúp Windows không bị nghẽn I/O khi cấp phát hàng ngàn File Handle lúc khởi chạy.

### 5. Cơ chế Smart RAM Cache hoạt động ra sao? Treo game lâu có sợ tràn RAM, Crash hay màn hình xanh (BSOD) không?
- **Hoàn toàn KHÔNG — RAM luôn được kiểm soát phẳng và giải phóng 100% khi đóng game.**
  - **Khi đang mở game**: Bộ đệm thông minh hoạt động theo thuật toán **LRU (Least Recently Used) + TTL** với trần giới hạn cứng là **64 MB**. Các tài nguyên hình ảnh/âm thanh chỉ lưu đệm nóng trong `25 giây` để phục vụ chuyển cảnh mượt mà rồi tự động được thu hồi vào thời gian nghỉ giữa các khung hình (`requestIdleCallback`), hoặc xả ngay lập tức khi người chơi thu nhỏ cửa sổ (`Alt + Tab` / Minimize).
  - **Khi tắt game hoặc nhấn F5 tải lại**: Sự kiện `_dispose` tự động kích hoạt để xóa trắng toàn bộ vùng nhớ đệm (`fill(0)`), thu hồi mọi `Blob URL`, đóng toàn bộ File Descriptor của các tệp `.0` (`fs.closeSync`) và gọi bộ thu gom rác `gc()` để trả lại 100% tài nguyên cho hệ điều hành.

### 6. Người khác soi trong thư mục `js/plugins` hoặc mở F12 DevTools có thấy `rpg_protect_runtime.js` hay lấy được Key giải mã không?
- **Không.** File `rpg_protect_runtime.js` và `index.html.rpgp_bak` đã được loại bỏ hoàn toàn (Ở phiên bản 4.2.2 còn khá nhiều lỗ hỏng mà tác giả đã bỏ sót). Bộ nạp ảo được ngụy trang thành thư viện đồ họa chuẩn của PixiJS.

### 7. Hệ điều hành Windows mấy hỗ trợ tốt nhất?
- **Windows 10 và Windows 11 (64-bit)** mang lại hiệu suất tăng tốc phần cứng WebGL và quản lý bộ nhớ tối ưu nhất cho cả bộ công cụ lẫn game đã mã hóa, đồng thời vẫn tương thích đầy đủ với **Windows 7 SP1 và Windows 8.1**.

---

## 🔒 Khuyến Nghị An Toàn Dành Cho Nhà Phát Triển

- **Phân Tách Công Cụ & Khóa**: Tiện ích giải mã `RPGMakerDecrypter.exe` và các tệp văn bản chứa khóa (`*.txt`) chỉ phục vụ nội bộ nhà phát triển. **Không bao giờ đính kèm hai thành phần này vào bộ cài đặt (installer), file nén phát hành công khai hoặc kho lưu trữ Git công cộng.**
- **Quản Lý Bằng Git**: Thêm mẫu `*.txt` và thư mục `_RPG_Original_Backup_*/` vào tệp `.gitignore` để tránh rủi ro sơ ý commit khóa bảo mật lên repository.
- **Gia Cố Tầng Ứng Dụng**: Để đạt cấp độ bảo mật cao nhất cho sản phẩm thương mại, bạn nên kết hợp bộ công cụ này với giải pháp xáo trộn mã JavaScript (Obfuscator) cho các file plugin logic nghiệp vụ trước khi tiến hành đóng gói.

---

## ⚖️ Legal Disclaimer & Terms of Use

### English

> **IMPORTANT: PLEASE READ CAREFULLY BEFORE USING THIS SOFTWARE.**

1. **Intended Use & Authorization**: This software suite (`RPGMakerEncryptor.exe` & `RPGMakerDecrypter.exe`) is designed exclusively for educational, research, and legitimate asset security purposes by authorized game creators and intellectual property owners. You may **only** encrypt and decrypt assets for which you hold legal ownership, explicit license, or verified written authorization from the copyright holder.
2. **Prohibition of Malicious & Infringing Acts**: The author strictly prohibits the use of this suite for unauthorized reverse engineering, asset piracy, intellectual property theft, commercial cracking, or circumventing technological protection measures (TPM) on third-party games.
3. **Loss of Cryptographic Keys & Data**: Decryption is mathematically bound to the matching cryptographic key generated during packaging. The developer **cannot** recover or reconstruct lost, overwritten, or misplaced keys. You are solely responsible for securely vaulting your project keys and maintaining independent, unencrypted offsite backups.
4. **"AS IS" Warranty & Liability Limitation**: This software is provided on an **"AS IS"** and **"AS AVAILABLE"** basis, without warranty of any kind, express or implied. Under no circumstances shall the author, contributors, or distributors be held liable for any direct, indirect, incidental, special, exemplary, or consequential damages (including, but not limited to, data loss, project corruption, revenue loss, system downtime, or legal claims) arising out of the use, misuse, or inability to use this software suite.

---

### Tiếng Việt

> **QUAN TRỌNG: VUI LÒNG ĐỌC KỸ TRƯỚC KHI SỬ DỤNG PHẦN MỀM NÀY.**

1. **Mục Đích Sử Dụng & Phạm Vi Cho Phép**: Bộ công cụ này (`RPGMakerEncryptor.exe` & `RPGMakerDecrypter.exe`) được phát triển thuần túy cho mục đích nghiên cứu kỹ thuật, học thuật và bảo vệ tài nguyên hợp pháp dành cho các nhà phát triển game và chủ sở hữu quyền tác giả. Bạn **chỉ được phép** mã hóa và giải mã dữ liệu đối với những dự án do chính bạn sở hữu hoặc đã được chủ sở hữu bản quyền cấp phép bằng văn bản hợp pháp.
2. **Nghiêm Cấm Hành Vi Xâm Phạm Bản Quyền**: Tác giả nghiêm cấm mọi hành vi sử dụng bộ phần mềm này để dịch ngược (reverse engineer), trích xuất tài nguyên trái phép (rip assets), bẻ khóa thương mại hoặc can thiệp vượt rào bảo vệ kỹ thuật đối với các tựa game thuộc quyền sở hữu của bên thứ ba mà không có sự cho phép.
3. **Trách Nhiệm Quản Lý Khóa & Dữ Liệu**: Cơ chế giải mã gắn liền về mặt toán học với khóa bảo mật tương ứng được tạo ra trong quá trình đóng gói. Tác giả phần mềm **hoàn toàn không có khả năng** khôi phục dữ liệu nếu bạn làm mất, quên hoặc ghi đè file khóa. Bạn chịu trách nhiệm hoàn toàn đối với việc lưu trữ an toàn các tệp khóa và sao lưu mã nguồn gốc độc lập bên ngoài.
4. **Miễn Trừ Trách Nhiệm Pháp Lý**: Phần mềm được cung cấp theo nguyên tắc **"NGUYÊN BẢN" (AS IS)** và không đi kèm bất kỳ cam kết hay bảo hành nào (dù rõ ràng hay ngụ ý). Trong mọi trường hợp, tác giả và các bên đóng góp sẽ **không chịu bất kỳ trách nhiệm pháp lý** nào đối với các tổn thất trực tiếp, gián tiếp, mất mát dữ liệu, gián đoạn công việc, tranh chấp bản quyền hoặc bất kỳ rủi ro phát sinh nào bắt nguồn từ việc sử dụng, lạm dụng hoặc không thể sử dụng bộ công cụ này.
