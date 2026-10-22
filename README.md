# 🛡️ RPG Maker Game Protection & Decryption Suite

Bộ công cụ chuyên dụng bảo vệ mã nguồn, hình ảnh, âm thanh và dữ liệu game **RPG Maker (MZ, MV, VX Ace, XP, VX)** chống bung source code, decompile hoặc hack/mod.

---

## 📦 File Thực Thi (.exe)

Thư mục: `bin/`
- **`RPGMakerEncryptor.exe`**: Ứng dụng mã hóa game RPG Maker.
- **`RPGMakerDecrypter.exe`**: Ứng dụng giải mã và bung lại source code game.

---

## ✨ Tính Năng Nổi Bật

### 1. App Mã Hóa (`RPGMakerEncryptor.exe`)
- **Nút "Select Folder Game"**: Chọn thư mục game cần mã hóa.
- **Đèn báo trạng thái Verify Folder Game**:
  - 🟢 **Xanh lá**: `● Verify Folder Game` – Nhận diện chính xác game RPG Maker hợp lệ (MZ, MV, VX Ace, XP, v.v.).
  - 🔴 **Đỏ**: `● UnVerify Folder Game` – Nếu chọn sai thư mục, app hiển thị cảnh báo lỗi nhưng vẫn cho phép tiếp tục nếu người dùng xác nhận.
- **Mục Key Decrypt**:
  - Tự động sinh khóa mã hóa ngẫu nhiên (VD: `RPG-XXXX-XXXX-XXXX-XXXX`).
  - Tự động xuất file text chứa Key (`RPG_Maker_Key_[Timestamp].txt`) ngay tại thư mục app đang mở.
  - Nút **Sao Chép Key** & **Tạo Key Mới**.
- **Mục "▼ Nâng Cao" (Advanced)**:
  - Nhấp vào mũi tên để mở rộng/thu gọn.
  - **Ngôn ngữ**: Tiếng Việt (Mặc định) / English.
  - **Mức độ mã hóa**:
    1. **Dễ (Easy)**: Fast XOR Stream Obfuscation
    2. **Trung Bình (Medium)**: Tiêu chuẩn AES-128 bit
    3. **Nâng Cao (Advanced)**: Chuẩn quân sự AES-256 bit + PBKDF2 (Khuyên dùng)
  - **Tùy chọn an toàn**:
    - Tự động sao lưu bản gốc (`_RPG_Original_Backup_...`).
    - Gói các thư mục `data/` -> `data.0`, `img/` -> `img.0`, `audio/` -> `audio.0`, `js/` -> `js.0`.
    - Tích hợp bộ nạp ảo Virtual Runtime Loader (`rpg_protect_runtime.js`) cho phép game vẫn đọc được file và chạy bình thường mà không lộ bất kỳ file nào ra ngoài.

### 2. App Giải Mã (`RPGMakerDecrypter.exe`)
- **Chọn thư mục game đã mã hóa**: Tự động quét và xác thực `Verify Folder Game` (kiểm tra các file `.0`).
- **Chọn thư mục xuất**: Tự động tạo thư mục `[TênGame]_DecryptedSource`.
- **Nhập Key Decrypt**: Hỗ trợ cả Key ngẫu nhiên và Mã đặc quyền của tác giả.
- **Khôi phục hoàn toàn 100%**:
  - Bung toàn bộ các file `.0` trở về thư mục gốc `data/`, `img/`, `audio/`, `js/` nguyên vẹn 100%.
  - Tự động dọn dẹp các hook runtime trong `index.html`, khôi phục source code ban đầu.

---

## 🚀 Hướng Dẫn Sử Dụng Nhanh

### Để Mã Hóa Game:
1. Mở `RPGMakerEncryptor.exe`.
2. Bấm **Chọn Thư Mục Game** và chọn thư mục game của bạn.
3. Kiểm tra đèn xanh **Verify Folder Game**.
4. Chọn mức độ mã hóa trong mục **Nâng Cao** nếu muốn.
5. Bấm **BẮT ĐẦU MÃ HÓA GAME**.
6. File text chứa Key sẽ tự động được tạo ra ngay bên cạnh file `.exe`.

### Để Giải Mã & Bung Source:
1. Mở `RPGMakerDecrypter.exe`.
2. Bấm **Chọn Thư Mục Game** (chọn thư mục game có chứa file `.0`).
3. Nhập Key giải mã (hoặc nhập Mã đặc quyền tác giả).
4. Bấm **BẮT ĐẦU GIẢI MÃ & BUNG SOURCE**.
5. Toàn bộ source code sẽ được bung ra thư mục xuất hoàn tất.
