## **Hướng dẫn Nhanh: Setup 3 SSH Key cho 3 Tài Khoản GitHub**

**Mục tiêu:** Sử dụng 3 tài khoản GitHub (`soMot`, `SoHai`, `soBa`) với 3 khóa SSH riêng biệt trên cùng một máy.

**Bước 1: Tạo 3 cặp khóa SSH**

Mở terminal và chạy lần lượt các lệnh sau:

```bash
# Khóa cho soMot
ssh-keygen -t ed25519 -C "a@a.a" -f ~/.ssh/soMot
# Nhấn Enter 2 lần nếu không muốn đặt passphrase

# Khóa cho SoHai
ssh-keygen -t ed25519 -C "a@a.a" -f ~/.ssh/SoHai
# Nhấn Enter 2 lần nếu không muốn đặt passphrase

# Khóa cho soBa
ssh-keygen -t ed25519 -C "a@a.a" -f ~/.ssh/soBa
# Nhấn Enter 2 lần nếu không muốn đặt passphrase

# Đặt quyền truy cập cho khóa riêng tư
chmod 600 ~/.ssh/soMot ~/.ssh/SoHai ~/.ssh/soBa
```

**Bước 2: Thêm Khóa Công Khai (.pub) lên GitHub**

1.  **Lấy nội dung từng khóa công khai:**
    ```bash
    cat ~/.ssh/soMot.pub
    # Copy output

    cat ~/.ssh/SoHai.pub
    # Copy output

    cat ~/.ssh/soBa.pub
    # Copy output
    ```
2.  **Đăng nhập vào từng tài khoản GitHub:**
    * **Với tài khoản `soMot`:** Vào `Settings` -> `SSH and GPG keys` -> `New SSH key`. Dán nội dung `soMot.pub`, đặt tên và lưu.
    * **Với tài khoản `SoHai`:** Làm tương tự, dán nội dung `SoHai.pub`.
    * **Với tài khoản `soBa`:** Làm tương tự, dán nội dung `soBa.pub`.

**Bước 3: Cấu hình SSH Client (`~/.ssh/config`)**

1.  Mở hoặc tạo file `~/.ssh/config`:
    ```bash
    nano ~/.ssh/config
    ```
2.  Dán nội dung sau vào file:
    ```
    # Tài khoản soMot
    Host gh-soMot
      HostName github.com
      User git
      IdentityFile ~/.ssh/soMot
      IdentitiesOnly yes

    # Tài khoản SoHai
    Host gh-SoHai
      HostName github.com
      User git
      IdentityFile ~/.ssh/SoHai
      IdentitiesOnly yes

    # Tài khoản soBa
    Host gh-soBa
      HostName github.com
      User git
      IdentityFile ~/.ssh/soBa
      IdentitiesOnly yes
    ```
3.  Lưu file và thoát (`Ctrl+O`, `Enter`, `Ctrl+X` trong nano).

**Bước 4: Sử dụng với Git**

* **Clone repository mới:**
    ```bash
    # Từ soMot
    git clone git@gh-soMot:<username_soMot>/<repo>.git

    # Từ SoHai
    git clone git@gh-SoHai:<username_SoHai>/<repo>.git

    # Từ soBa
    git clone git@gh-soBa:<username_soBa>/<repo>.git
    ```
    *(Thay `<username_...>` và `<repo>` bằng thông tin thực tế).*

* **Cập nhật remote của repo đã có (ví dụ cho `soMot`):**
    ```bash
    cd /path/to/repo
    git remote set-url origin git@gh-soMot:<username_soMot>/<repo>.git
    ```

**Bước 5: Kiểm tra kết nối (Tùy chọn)**

```bash
ssh -T git@gh-soMot
ssh -T git@gh-SoHai
ssh -T git@gh-soBa
```

Nếu thấy thông báo `Hi <username>! You've successfully authenticated...` cho mỗi lệnh là thành công.