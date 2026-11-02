# 🛡️ RPG Maker Asset Security & Pipeline Suite

A robust asset encryption and build pipeline designed for **RPG Maker (MZ, MV, VX Ace, XP, VX)** developers. Safeguard proprietary game logic, graphics, audio, and database assets against unauthorized extraction, reverse engineering, and tampering.

---

## 🏗️ Architecture Overview

The suite provides a dual-tool workflow tailored for game production environments:
1. **Packaging & Obfuscation Engine (`RPGMakerEncryptor.exe`)**: Encrypts raw game directories into compact, protected binary blobs (`.0` containers) and embeds an in-memory runtime bridge for the game engine.
2. **Authorized Asset Recovery Utility (`RPGMakerDecrypter.exe`)**: An internal pipeline tool allowing authorized project owners to extract, audit, or rebuild project sources using their private cryptographic keys.

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
│   - Virtual Runtime     │             └────────────┬────────────┘
└─────────────────────────┘                          │
         │                                           │
         │ (Game Launch:                             │ (Internal Pipeline /
         │  Decrypted in-memory                      │  Recovery / Auditing)
         │  without disk dump)                       ▼
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
- **`RPGMakerEncryptor.exe`**: Deployment & encryption interface.
- **`RPGMakerDecrypter.exe`**: Authorized recovery & pipeline restoration utility.

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
  - **Easy (Low Overhead)**: Fast XOR Stream Obfuscation for lightweight prototype distribution.
  - **Medium**: Industry-standard AES-128 bit block encryption.
  - **Advanced (Production Recommended)**: Military-grade AES-256 bit + PBKDF2 key derivation.
- **Production Asset Hardening**:
  - Packs core directories into protected payloads (`data.0`, `img.0`, `audio.0`, `js.0`).
  - Integrates an in-memory Virtual Runtime Loader (`rpg_protect_runtime.js`) to stream encrypted assets directly into the engine heap without writing unencrypted assets to disk.
  - Automated pre-build backup routines (`_RPG_Original_Backup_...`).

### 2. Internal Recovery Utility (`RPGMakerDecrypter.exe`)
- **Pipeline Integrity Verification**: Validates encrypted container headers prior to processing.
- **Private Key Authentication**: Requires the exact symmetric key generated during the build step.
- **Lossless Project Restoration**:
  - Reconstructs complete directory structures (`data/`, `img/`, `audio/`, `js/`) with byte-for-byte fidelity.
  - Unhooks runtime injection hooks from `index.html` to restore clean development environments.

---

## 🚀 Workflow Guide

### Production Build (Encryption)
1. Launch `RPGMakerEncryptor.exe`.
2. Click **Select Folder Game** and select your project root.
3. Confirm the status displays 🟢 **Verified**.
4. Configure target cipher settings in **Advanced Settings** if needed.
5. Click **BUILD SECURE ASSETS**.
6. Securely store the output key (`RPG_Maker_Key_[Timestamp].txt`) in your private credentials vault or password manager. **Do not distribute this key with public game releases.**

### Source Recovery & Internal Auditing
1. Launch `RPGMakerDecrypter.exe`.
2. Select the packaged game directory (containing `.0` containers).
3. Specify the destination directory (defaults to `[GameTitle]_DecryptedSource`).
4. Enter the matching project recovery key.
5. Click **RESTORE PROJECT ASSETS**.

---

## 🔒 Security Best Practices for Developers

- **Key Segregation**: The decryption utility is strictly for project maintainers. Never ship `RPGMakerDecrypter.exe` or exported key files inside public release packages, game installers, or public repositories.
- **Version Control**: Add `*.txt` key files and `_RPG_Original_Backup_*/` to your `.gitignore` to prevent accidental credential leakage.
- **Code Hardening**: For maximum protection on commercial releases, combine this suite with JavaScript obfuscation (e.g., Javascript-Obfuscator) on your custom plugin scripts prior to packaging.

- TIẾNG VIỆT -
# 🛡️ RPG Maker Asset Security & Pipeline Suite (Bản Tiếng Việt)

Bộ giải pháp đóng gói, mã hóa và bảo vệ tài nguyên chuyên dụng dành cho nhà phát triển game **RPG Maker (MZ, MV, VX Ace, XP, VX)**. Giúp ngăn chặn triệt để hành vi trích xuất tài nguyên trái phép, dịch ngược mã nguồn (decompile) hoặc can thiệp/gian lận dữ liệu dự án.

---

## 🏗️ Tổng Quan Kiến Trúc Hệ Thống

Bộ công cụ cung cấp quy trình khép kín gồm hai phần mềm phục vụ trực tiếp cho quá trình sản xuất game:
1. **Trình Mã Hóa & Đóng Gói Tài Nguyên (`RPGMakerEncryptor.exe`)**: Mã hóa các thư mục dữ liệu gốc thành các gói container nhị phân bảo mật (`.0`) và tích hợp cơ chế nạp ảo trực tiếp trên bộ nhớ RAM khi game khởi chạy.
2. **Công Cụ Phục Hồi & Kiểm Toán Mã Nguồn (`RPGMakerDecrypter.exe`)**: Tiện ích nội bộ dành riêng cho tác giả dự án, hỗ trợ trích xuất, khôi phục lại mã nguồn sạch từ bản đóng gói thông qua khóa bảo mật riêng (Private Key).

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
│   - Runtime ảo nạp RAM  │             │   Đối Trong Kho Khóa)   │
└─────────────────────────┘             └────────────┬────────────┘
         │                                           │
         │ (Khởi chạy game:                          │ (Dành cho Dev: Khôi
         │  Giải mã tức thì trên RAM,                │  phục & Kiểm toán)
         │  không xả file ra ổ cứng)                 ▼
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
- **`RPGMakerEncryptor.exe`**: Ứng dụng đóng gói và triển khai mã hóa dự án.
- **`RPGMakerDecrypter.exe`**: Công cụ khôi phục mã nguồn và đồng bộ asset nội bộ.

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
  - **Dễ (Mức nhẹ)**: Cơ chế Fast XOR Stream Obfuscation, tối ưu hiệu năng cho các bản demo hoặc thử nghiệm nội bộ.
  - **Trung Bình**: Tiêu chuẩn mã hóa khối AES-128 bit.
  - **Nâng Cao (Khuyên Dùng Cho Bản Chính Thức)**: Chuẩn bảo mật AES-256 bit kết hợp thuật toán dẫn xuất khóa PBKDF2.
- **Tăng Cường Phòng Vệ Tài Nguyên Trực Tiếp**:
  - Gói gọn toàn bộ các thư mục nhạy cảm thành tệp container bảo vệ (`data.0`, `img.0`, `audio.0`, `js.0`).
  - Nhúng cơ chế Virtual Runtime Loader (`rpg_protect_runtime.js`): stream trực tiếp dữ liệu đã mã hóa vào engine thông qua bộ nhớ RAM, tuyệt đối không tạo file đệm không mã hóa ra ổ đĩa (chống rip asset bằng disk cache).
  - Tự động sao lưu dự phòng toàn bộ source gốc trước khi build (`_RPG_Original_Backup_...`).

### 2. Tiện Ích Phục Hồi Nội Bộ (`RPGMakerDecrypter.exe`)
- **Kiểm Tra Tính Toàn Vẹn Của Pipeline**: Tự động rà soát header của các container mã hóa trước khi xử lý.
- **Xác Thực Khóa Đối Xứng**: Yêu cầu chính xác khóa mật mã được tạo ra ở bước đóng gói.
- **Khôi Phục Nguyên Vẹn 100%**:
  - Tái tạo cấu trúc cây thư mục gốc (`data/`, `img/`, `audio/`, `js/`) với độ chuẩn xác từng byte.
  - Gỡ bỏ hook runtime được chèn trong `index.html`, đưa mã nguồn về trạng thái lập trình sạch ban đầu.

---

## 🚀 Hướng Dẫn Quy Trình Sử Dụng

### Quy Trình Đóng Gói Bản Build (Mã Hóa):
1. Khởi chạy `RPGMakerEncryptor.exe`.
2. Bấm **Chọn Thư Mục Game** và điều hướng tới thư mục gốc của dự án.
3. Kiểm tra đèn trạng thái hiển thị 🟢 **Verified**.
4. Cấu hình thuật toán bảo vệ mong muốn tại phần **Thiết Lập Nâng Cao** (mặc định khuyến nghị: AES-256).
5. Bấm **BẮT ĐẦU ĐÓNG GÓI BẢO VỆ**.
6. Lưu trữ tệp khóa được tạo ra (`RPG_Maker_Key_[Timestamp].txt`) vào phần mềm quản lý mật khẩu hoặc két an toàn của đội ngũ phát triển. **Tuyệt đối không đính kèm file khóa này vào bản game thương mại gửi cho người chơi.**

### Quy Trình Phục Hồi Mã Nguồn & Kiểm Toán Nội Bộ:
1. Khởi chạy `RPGMakerDecrypter.exe`.
2. Chọn thư mục game đã đóng gói (chứa các tệp container `.0`).
3. Chỉ định thư mục trích xuất đích (mặc định là `[TênGame]_DecryptedSource`).
4. Nhập chính xác mã khóa bí mật của dự án.
5. Bấm **BẮT ĐẦU PHỤC HỒI DỰ ÁN**.

---

## 🔒 Khuyến Nghị An Toàn Dành Cho Nhà Phát Triển

- **Phân Tách Công Cụ & Khóa**: Tiện ích giải mã `RPGMakerDecrypter.exe` và các tệp văn bản chứa khóa (`*.txt`) chỉ phục vụ nội bộ nhà phát triển. **Không bao giờ đính kèm hai thành phần này vào bộ cài đặt (installer), file nén phát hành công khai hoặc kho lưu trữ Git công cộng.**
- **Quản Lý Bằng Git**: Thêm mẫu `*.txt` và thư mục `_RPG_Original_Backup_*/` vào tệp `.gitignore` để tránh rủi ro sơ ý commit khóa bảo mật lên repository.
- **Gia Cố Tầng Ứng Dụng**: Để đạt cấp độ bảo mật cao nhất cho sản phẩm thương mại, bạn nên kết hợp bộ công cụ này với giải pháp xáo trộn mã JavaScript (Obfuscator) cho các file plugin logic nghiệp vụ trước khi tiến hành đóng gói.
