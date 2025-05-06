# Git Cheatsheet Tiếng Việt

## 1\. Cấu hình Ban đầu (Initial Setup)

| Hành động                    | Lệnh                                                                       |
| :--------------------------- | :------------------------------------------------------------------------- |
| Thiết lập tên người dùng     | `git config --global user.name "Tên Của Bạn"`                              |
| Thiết lập email              | `git config --global user.email "email@cua.ban"`                           |
| Xem cấu hình hiện tại        | `git config --list`                                                        |
| Xem tên người dùng           | `git config --global --get user.name`                                      |
| Xem email                    | `git config --global --get user.email`                                     |
| Đổi trình soạn thảo mặc định | `git config --global core.editor "vim"` (hoặc `nano`, `code --wait`, etc.) |

## 2\. Tạo & Clone Repository

  * **Khởi tạo repo mới tại thư mục hiện tại:**
    ```bash
    git init
    ```
  * **Clone repo từ URL (HTTPS):**
    ```bash
    git clone https://github.com/username/repository.git
    ```
  * **Clone repo từ URL (SSH):**
    ```bash
    git clone git@github.com:username/repository.git
    ```
  * **Chuyển vào thư mục repo vừa clone:**
    ```bash
    cd repository
    ```

## 3\. Luồng làm việc Cơ bản (Basic Workflow)

| Hành động                      | Lệnh                                         | Mô tả cực ngắn                                    |
| :----------------------------- | :------------------------------------------- | :------------------------------------------------ |
| **Kiểm tra trạng thái**        | `git status`                                 | Xem các thay đổi, file chưa theo dõi (untracked). |
| **Đưa thay đổi vào Staging**   |                                              |                                                   |
| Đưa 1 file cụ thể              | `git add <tên_file>`                         | Chuẩn bị file cho commit.                         |
| Đưa tất cả thay đổi            | `git add .`                                  | Chuẩn bị tất cả thay đổi hiện tại cho commit.     |
| **Ghi nhận thay đổi (Commit)** |                                              |                                                   |
| Commit các file đã `add`       | `git commit -m "Mô tả commit ngắn gọn"`      | Lưu snapshot vào lịch sử repo.                    |
| Sửa commit cuối cùng           | `git commit --amend -m "Mô tả mới"`          | Chỉnh sửa commit gần nhất (chưa push).            |
| **Xem lịch sử commit**         |                                              |                                                   |
| Xem log đầy đủ                 | `git log`                                    | Hiển thị lịch sử commit chi tiết.                 |
| Xem log ngắn gọn               | `git log --oneline`                          | Mỗi commit trên 1 dòng (ID ngắn + tiêu đề).       |
| Xem log dạng đồ thị            | `git log --graph --all --decorate --oneline` | Xem lịch sử trực quan với các nhánh.              |

## 4\. Làm việc với Nhánh (Branching)

| Hành động                                 | Lệnh                          | Mô tả cực ngắn                             |
| :---------------------------------------- | :---------------------------- | :----------------------------------------- |
| Liệt kê tất cả nhánh (local)              | `git branch`                  | Nhánh hiện tại có dấu `*`.                 |
| Liệt kê tất cả nhánh (remote)             | `git branch -r`               | Xem các nhánh trên remote.                 |
| Liệt kê tất cả nhánh (all)                | `git branch -a`               | Xem cả nhánh local và remote.              |
| Tạo nhánh mới                             | `git branch <tên_nhánh>`      | Tạo nhánh từ commit hiện tại.              |
| Chuyển sang nhánh khác                    | `git checkout <tên_nhánh>`    | Di chuyển `HEAD` sang nhánh khác.          |
| Tạo và chuyển nhánh ngay                  | `git checkout -b <tên_nhánh>` | Tương đương `git branch` + `git checkout`. |
| Gộp (merge) nhánh khác vào nhánh hiện tại | `git merge <tên_nhánh_nguồn>` | Trộn các thay đổi từ nhánh nguồn.          |
| Xóa nhánh (đã merge)                      | `git branch -d <tên_nhánh>`   | Xóa nhánh local đã được gộp.               |
| Xóa nhánh (chưa merge - force)            | `git branch -D <tên_nhánh>`   | **Cẩn thận:** Xóa nhánh chưa gộp.          |

## 5\. Làm việc với Remote Repository

| Hành động                            | Lệnh                                         | Mô tả cực ngắn                                     |
| :----------------------------------- | :------------------------------------------- | :------------------------------------------------- |
| Liệt kê các remote đã kết nối        | `git remote -v`                              | Hiển thị tên và URL của các remote.                |
| Thêm remote mới                      | `git remote add <tên_remote> <url_repo>`     | Thường dùng `origin` cho remote chính.             |
| Đổi tên remote                       | `git remote rename <tên_cũ> <tên_mới>`       |                                                    |
| Xóa kết nối remote                   | `git remote remove <tên_remote>`             |                                                    |
| Đẩy (push) commit lên remote         | `git push <tên_remote> <tên_nhánh_local>`    | Gửi các commit local lên remote.                   |
| Push lần đầu                         | `git push -u origin <tên_nhánh>`             | `-u` thiết lập upstream tracking.                  |
| Lấy (pull) thay đổi từ remote        | `git pull <tên_remote> <tên_nhánh_remote>`   | Tương đương `git fetch` + `git merge`.             |
| Lấy thông tin thay đổi (không merge) | `git fetch <tên_remote>`                     | Cập nhật trạng thái remote, không merge vào local. |
| Xóa nhánh trên remote                | `git push <tên_remote> --delete <tên_nhánh>` | Xóa nhánh trên kho lưu trữ từ xa.                  |

## 6\. Hoàn tác thay đổi (Undoing Changes)

| Hành động                                             | Lệnh                                       | Mô tả cực ngắn                                       |
| :---------------------------------------------------- | :----------------------------------------- | :--------------------------------------------------- |
| Hủy thay đổi ở Working Directory (file cụ thể)        | `git checkout -- <tên_file>`               | Loại bỏ thay đổi chưa `add` ở file đó.               |
| Bỏ file khỏi Staging Area (unstage)                   | `git reset HEAD <tên_file>`                | Giữ lại thay đổi trong Working Directory.            |
| Hủy tất cả thay đổi chưa commit (WD & Staging)        | `git reset --hard HEAD`                    | **Cẩn thận:** Mất hết thay đổi chưa commit.          |
| Xóa commit cuối cùng, giữ lại thay đổi (WD & Staging) | `git reset --soft HEAD~1`                  | Di chuyển `HEAD` về commit trước, giữ thay đổi.      |
| Xóa commit cuối cùng, giữ thay đổi (WD)               | `git reset HEAD~1` (mặc định là `--mixed`) | Di chuyển `HEAD`, đưa thay đổi về Working Directory. |
| Tạo commit mới để đảo ngược commit cũ                 | `git revert <commit_id>`                   | An toàn hơn `reset` khi đã push, tạo commit mới.     |

## 7\. Lưu tạm thay đổi (Stashing)

| Hành động                     | Lệnh                                      | Mô tả cực ngắn                                 |
| :---------------------------- | :---------------------------------------- | :--------------------------------------------- |
| Lưu tạm các thay đổi hiện tại | `git stash` hoặc `git stash save "mô tả"` | Cất các thay đổi chưa commit để làm việc khác. |
| Xem danh sách các stash       | `git stash list`                          | Liệt kê các bộ thay đổi đã lưu tạm.            |
| Áp dụng lại stash gần nhất    | `git stash pop`                           | Áp dụng và xóa stash khỏi danh sách.           |
| Áp dụng stash cụ thể          | `git stash apply stash@{index}`           | Áp dụng nhưng không xóa stash.                 |
| Xóa stash gần nhất            | `git stash drop`                          |                                                |
| Xóa stash cụ thể              | `git stash drop stash@{index}`            |                                                |
| Xóa tất cả stash              | `git stash clear`                         |                                                |

## 8\. Gắn thẻ phiên bản (Tagging)

| Hành động                        | Lệnh                                   | Mô tả cực ngắn                                     |
| :------------------------------- | :------------------------------------- | :------------------------------------------------- |
| Tạo tag nhẹ (lightweight)        | `git tag <tên_tag>`                    | Chỉ là con trỏ đến một commit.                     |
| Tạo tag có chú thích (annotated) | `git tag -a <tên_tag> -m "mô tả"`      | Có thông tin người tạo, ngày, mô tả (khuyên dùng). |
| Liệt kê tất cả tag               | `git tag`                              |                                                    |
| Xem thông tin tag cụ thể         | `git show <tên_tag>`                   |                                                    |
| Đẩy 1 tag lên remote             | `git push <remote> <tên_tag>`          | Đẩy tag cụ thể.                                    |
| Đẩy tất cả tag lên remote        | `git push <remote> --tags`             | Đẩy tất cả các tag local chưa có trên remote.      |
| Xóa tag local                    | `git tag -d <tên_tag>`                 |                                                    |
| Xóa tag trên remote              | `git push <remote> --delete <tên_tag>` |                                                    |

## 9\. Merge & Xử lý Conflict

1.  Chuyển sang nhánh đích (ví dụ: `main`): `git checkout main`
2.  Thực hiện merge: `git merge <nhánh_nguồn>`
3.  Nếu có **Conflict**:
      * Git sẽ báo lỗi và đánh dấu conflict trong file (ví dụ: `<<<<<<< HEAD`, `=======`, `>>>>>>> <tên_nhánh_nguồn>`).
      * Mở file bị conflict, **sửa thủ công** để giữ lại phần code mong muốn, xóa các dấu đánh dấu conflict.
      * Đưa file đã sửa vào staging: `git add <file_đã_sửa>`
      * Commit kết quả merge: `git commit -m "Resolved merge conflict in <file>"` (Hoặc để Git tự tạo message mặc định).

## 10\. Rebase (Làm sạch lịch sử - *Nâng cao*)

  * **Mục đích:** Viết lại lịch sử commit của nhánh hiện tại lên trên một nhánh khác (thường là `main`/`master`), giúp lịch sử commit thẳng hàng, sạch sẽ hơn *trước khi* merge vào nhánh chính.
  * **Cơ bản:** (Đang ở nhánh feature) `git rebase <nhánh_đích>` (ví dụ: `git rebase main`)
  * **Rebase tương tác (Interactive):** Chỉnh sửa, gộp (squash), xóa, sắp xếp lại các commit gần đây.
    ```bash
    git rebase -i HEAD~<số_commit>
    # Ví dụ: Chỉnh sửa 3 commit gần nhất
    git rebase -i HEAD~3
    ```
    *(Sau đó làm theo hướng dẫn trong trình soạn thảo: `pick`, `reword`, `edit`, `squash`, `fixup`, `exec`, `drop`)*
  * **Cảnh báo:** **Không bao giờ rebase các commit đã được push lên remote chung** (nhánh mà người khác cũng đang làm việc), vì nó viết lại lịch sử, gây conflict lớn cho người khác. Chỉ nên rebase trên nhánh cá nhân của bạn.

## 11\. Xóa file khỏi Git

| Hành động                                           | Lệnh                         | Mô tả cực ngắn                                                           |
| :-------------------------------------------------- | :--------------------------- | :----------------------------------------------------------------------- |
| Xóa file khỏi Working Directory **và** Staging Area | `git rm <tên_file>`          | Tương đương `rm <tên_file>` rồi `git add <tên_file>`.                    |
| Chỉ xóa khỏi Staging Area (giữ lại file vật lý)     | `git rm --cached <tên_file>` | Hữu ích khi muốn bỏ theo dõi file (ví dụ: thêm vào `.gitignore` sau đó). |

## 12\. SSH Keys (Thiết lập nâng cao với GitHub/GitLab...)

| Hành động                                | Lệnh                                            | Mô tả cực ngắn                                       |
| :--------------------------------------- | :---------------------------------------------- | :--------------------------------------------------- |
| Kiểm tra khóa SSH hiện có                | `ls -al ~/.ssh`                                 | Tìm các file như `id_rsa`, `id_ed25519`.             |
| Tạo khóa SSH mới (RSA)                   | `ssh-keygen -t rsa -b 4096 -C "email@cua.ban"`  | Tạo cặp khóa private/public.                         |
| Tạo khóa SSH mới (Ed25519 - khuyên dùng) | `ssh-keygen -t ed25519 -C "email@cua.ban"`      | An toàn và hiệu năng tốt hơn RSA.                    |
| Hiển thị khóa công khai (để copy)        | `cat ~/.ssh/id_ed25519.pub` (hoặc `id_rsa.pub`) | Copy nội dung này vào cài đặt SSH của GitHub/GitLab. |
| Thêm khóa private vào ssh-agent          | `ssh-add ~/.ssh/id_ed25519` (hoặc `id_rsa`)     | Để không phải nhập passphrase mỗi lần.               |
| Kiểm tra kết nối SSH với GitHub          | `ssh -T git@github.com`                         | Xác nhận thiết lập thành công.                       |
