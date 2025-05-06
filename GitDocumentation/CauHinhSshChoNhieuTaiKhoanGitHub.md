## Cấu hình SSH cho Nhiều Tài khoản GitHub

**Mục đích:** Cho phép sử dụng đồng thời nhiều tài khoản GitHub (cá nhân, công việc, khách hàng...) trên cùng một máy tính thông qua kết nối SSH an toàn và tiện lợi.

**Điều kiện tiên quyết:**
* Máy tính đã cài đặt `git`.
* Máy tính đã cài đặt `ssh` (thường có sẵn trên Linux/macOS; cần Git Bash hoặc WSL trên Windows).
* Bạn có quyền truy cập vào các tài khoản GitHub cần cấu hình.

---

### 1. Tạo Khóa SSH cho Từng Tài khoản

**Quan trọng:** Mỗi tài khoản GitHub cần một cặp khóa SSH riêng biệt.

1.  **Mở Terminal** (Linux/macOS) hoặc **Git Bash** (Windows).
2.  **Tạo khóa cho tài khoản chính (mặc định):**
    * Chạy lệnh: `ssh-keygen -t ed25519 -C "email_chinh@example.com"`
    * Khi hỏi nơi lưu file (`Enter file in which to save the key`), **nhấn Enter** để chấp nhận đường dẫn mặc định (`~/.ssh/id_ed25519`).
    * (Tùy chọn) Nhập mật khẩu bảo vệ (passphrase) cho khóa để tăng cường bảo mật. Nhấn Enter để bỏ qua.
3.  **Tạo khóa cho tài khoản phụ:**
    * Chạy lệnh, **thay `ten_file_khoa_phu`** bằng tên gợi nhớ (vd: `id_ed25519_congty`) và **thay `email_phu@example.com`**:
        `ssh-keygen -t ed25519 -C "email_phu@example.com" -f ~/.ssh/ten_file_khoa_phu`
    * Khi hỏi nơi lưu file, **đảm bảo đường dẫn đúng** (`~/.ssh/ten_file_khoa_phu`).
    * (Tùy chọn) Nhập mật khẩu bảo vệ (passphrase) cho khóa này.
4.  **Lặp lại bước 3** cho mỗi tài khoản GitHub phụ khác, sử dụng tên file khóa (`-f`) khác nhau cho mỗi tài khoản.

---

### 2. Thêm Public Key (.pub) vào Tài khoản GitHub Tương ứng

1.  **Lấy nội dung Public Key:**
    * **Khóa chính:** `cat ~/.ssh/id_ed25519.pub`
    * **Khóa phụ:** `cat ~/.ssh/ten_file_khoa_phu.pub` (Thay `ten_file_khoa_phu` bằng tên bạn đã đặt).
2.  **Sao chép (Copy)** toàn bộ nội dung hiển thị (bắt đầu bằng `ssh-ed25519...`).
3.  **Đăng nhập** vào tài khoản GitHub tương ứng trên trình duyệt.
4.  Đi tới **Settings** (Cài đặt).
5.  Trong menu bên trái, chọn **SSH and GPG keys**.
6.  Nhấn nút **New SSH key** hoặc **Add SSH key**.
7.  Đặt **Title** (Tiêu đề) gợi nhớ cho khóa (ví dụ: "Máy tính cá nhân", "Laptop Công ty").
8.  **Dán (Paste)** nội dung public key đã sao chép vào ô **Key**.
9.  Nhấn **Add SSH key**.
10. **Lặp lại các bước 3-9** cho mỗi cặp khóa/tài khoản GitHub khác. **Cảnh báo:** Đảm bảo bạn thêm đúng public key vào đúng tài khoản GitHub.

---

### 3. Cấu hình SSH Client (File `~/.ssh/config`)

File này giúp SSH biết dùng khóa nào khi kết nối đến GitHub qua các "bí danh" (alias) khác nhau.

1.  **Mở hoặc Tạo file cấu hình:**
    * Chạy lệnh: `nano ~/.ssh/config` (Hoặc sử dụng trình soạn thảo văn bản khác như `vim`, `code`).
    * Nếu file chưa tồn tại, lệnh `nano` sẽ tạo file mới.
2.  **Thêm nội dung cấu hình:** Dán và chỉnh sửa nội dung sau vào file. **Thay `ten_alias_phu`** (ví dụ: `github-congty`) và **thay `ten_file_khoa_phu`** cho phù hợp.

    ```config
    # --- TÀI KHOẢN GITHUB CHÍNH (Mặc định) ---
    # Sử dụng khóa mặc định ~/.ssh/id_ed25519
    Host github.com
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_ed25519
        IdentitiesOnly yes

    # --- TÀI KHOẢN GITHUB PHỤ 1 (Ví dụ: Công ty) ---
    # Sử dụng alias 'github-congty' và khóa 'id_ed25519_congty'
    Host github-congty  # <--- Đặt Alias tại đây
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_ed25519_congty # <--- Đường dẫn tới private key phụ
        IdentitiesOnly yes

    # --- TÀI KHOẢN GITHUB PHỤ 2 (Ví dụ: Khách hàng A) ---
    # Sử dụng alias 'github-clientA' và khóa 'id_ed25519_clientA'
    Host github-clientA # <--- Alias khác
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_ed25519_clientA # <--- Đường dẫn khóa phụ khác
        IdentitiesOnly yes

    # (Thêm các khối 'Host' tương tự cho các tài khoản phụ khác)
    ```

3.  **Giải thích cấu hình:**
    * `Host`: Đặt tên **Alias** (bí danh) để phân biệt kết nối. `github.com` là mặc định. Các alias khác (vd: `github-congty`) do bạn tự đặt.
    * `HostName`: Luôn là `github.com`.
    * `User`: Luôn là `git`.
    * `IdentityFile`: Chỉ định đường dẫn đến **Private Key** tương ứng với tài khoản.
    * `IdentitiesOnly yes`: **Bắt buộc**. Đảm bảo SSH chỉ sử dụng khóa được chỉ định trong `IdentityFile`, không tự động thử các khóa khác.
4.  **Lưu file và Thoát:**
    * Trong `nano`: Nhấn **Ctrl+X**, sau đó nhấn **Y** (hoặc **C** tùy ngôn ngữ), rồi nhấn **Enter**.
5.  **(Quan trọng) Thiết lập quyền truy cập an toàn:**
    * Chạy lệnh: `chmod 600 ~/.ssh/config`
    * Chạy lệnh cho từng **private key**: `chmod 600 ~/.ssh/ten_file_khoa_private` (Ví dụ: `chmod 600 ~/.ssh/id_ed25519_congty`)

---

### 4. Sử dụng với Git (Clone & Remote)

Khi tương tác với GitHub, bạn sẽ sử dụng alias đã định nghĩa trong `~/.ssh/config` ở phần `Host` của URL SSH.

* **Clone Repository Mới:**
    * **Tài khoản chính (dùng `github.com`):**
        `git clone git@github.com:USERNAME_CHINH/ten-repo.git`
    * **Tài khoản phụ (dùng `Alias` đã đặt):**
        `git clone git@ALIAS:USERNAME_PHU/ten-repo.git`
        *Ví dụ:* `git clone git@github-congty:USERNAME_CONGTY/project-xyz.git`
* **Cấu hình Repository Đã Tồn Tại (Thay đổi Remote URL):**
    1.  **Di chuyển** vào thư mục của repository: `cd /duong/dan/toi/repo`
    2.  **Kiểm tra** remote hiện tại: `git remote -v`
    3.  **Đặt lại URL** cho tài khoản phụ (sử dụng `Alias`):
        `git remote set-url origin git@ALIAS:USERNAME_PHU/ten-repo.git`
        *Ví dụ:* `git remote set-url origin git@github-congty:USERNAME_CONGTY/project-xyz.git`

**Lưu ý:** Thay `USERNAME_CHINH`, `USERNAME_PHU`, `ten-repo`, `ALIAS` bằng giá trị thực tế của bạn.

---

### 5. Kiểm tra Kết nối SSH

Sử dụng lệnh `ssh -T` với Host/Alias tương ứng để kiểm tra.

* **Kiểm tra tài khoản chính:** `ssh -T git@github.com`
* **Kiểm tra tài khoản phụ:** `ssh -T git@ALIAS`
    *Ví dụ:* `ssh -T git@github-congty`

Nếu thành công, bạn sẽ thấy thông báo chào mừng từ GitHub, ví dụ: "Hi `USERNAME`! You've successfully authenticated, but GitHub does not provide shell access."

---

### 6. Lưu ý Quan trọng & Mẹo

* **Bảo mật Private Key:** **Tuyệt đối không** chia sẻ các file private key (những file không có đuôi `.pub`). Đảm bảo chúng có quyền `600` (chỉ chủ sở hữu đọc/ghi được).
* **Đặt Tên Gợi Nhớ:** Sử dụng tên file khóa (`id_ed25519_...`) và alias (`Host ...`) rõ ràng, dễ phân biệt.
* **Kiểm tra Remote:** Trong thư mục dự án, thường xuyên chạy `git remote -v` để đảm bảo bạn đang kết nối đến đúng tài khoản/repository.
* **Cấu hình Git User:** SSH chỉ xử lý việc **xác thực** kết nối. Để các commit ghi đúng **tên tác giả và email** cho từng dự án/tài khoản, hãy cấu hình trong từng thư mục repository:
    * `git config user.name "Tên Hiển Thị Của Bạn"`
    * `git config user.email "email_gan_voi_tai_khoan_github@example.com"`
    * Chạy `git config --list` để kiểm tra cấu hình hiện tại của repo.
* **Trình duyệt:** Khi quản lý các tài khoản GitHub trên web, nên sử dụng các profile trình duyệt riêng biệt hoặc chế độ ẩn danh để tránh lẫn lộn session đăng nhập.
