# BÁO CÁO THỰC HÀNH: BÀI 2
## Quản lý nhánh và Giải quyết xung đột (Merge Conflict)

- **Học viên:** Học viên IT209
- **Đường dẫn nộp bài:** `homework/session_04/ex2/`
- **Tệp nộp:** [README.md](file:///e:/IT209/IT209-SS4/SS4-B2/README.md), [REPORT.md](file:///e:/IT209/IT209-SS4/SS4-B2/REPORT.md)

---

### 1. Mục tiêu bài tập
1. Tạo mới và di chuyển linh hoạt giữa các nhánh cục bộ (`main` và `feature-update`).
2. Chủ động tạo xung đột gộp nhánh (Merge Conflict) trên cùng một dòng trong tệp `README.md`.
3. Xử lý xung đột thủ công bằng cách loại bỏ các ký hiệu đánh dấu (`<<<<<<<`, `=======`, `>>>>>>>`) và tổng hợp nội dung hợp lý.
4. Hiểu và áp dụng cơ chế gộp 3 vùng (3-Way Merge).

---

### 2. Các bước thực hiện chi tiết

#### Bước 1: Khởi tạo repository và commit đầu tiên trên nhánh `main`
```bash
git init -b main
# Tạo file README.md ban đầu
git add README.md
git commit -m "Initial commit: create README.md"
```

#### Bước 2: Tạo và chuyển sang nhánh `feature-update`
```bash
git checkout -b feature-update
```
- Chỉnh sửa dòng mô tả trong `README.md` thành:
  ```markdown
  Phiên bản nâng cấp tính năng mới được phát triển từ nhánh feature-update.
  ```
- Commit thay đổi:
```bash
git add README.md
git commit -m "Update version description on feature-update branch"
```

#### Bước 3: Chuyển lại về nhánh `main` và chỉnh sửa đồng thời
```bash
git checkout main
```
- Chỉnh sửa chính dòng mô tả đó trong `README.md` thành:
  ```markdown
  Phiên bản cập nhật trực tiếp trên nhánh chính main song song với nhánh phụ.
  ```
- Commit thay đổi:
```bash
git add README.md
git commit -m "Update version description on main branch"
```

#### Bước 4: Thực hiện gộp nhánh và kích hoạt xung đột (Conflict)
```bash
git merge feature-update
```
**Kết quả từ Git:**
```text
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

#### Bước 5: Xử lý xung đột thủ công
Nội dung file `README.md` khi xuất hiện xung đột:
```markdown
<<<<<<< HEAD
Phiên bản cập nhật trực tiếp trên nhánh chính main song song với nhánh phụ.
=======
Phiên bản nâng cấp tính năng mới được phát triển từ nhánh feature-update.
>>>>>>> feature-update
```

**Cách xử lý:**
- Xóa bỏ hoàn toàn các marker `<<<<<<< HEAD`, `=======`, `>>>>>>> feature-update`.
- Tổng hợp nội dung cả hai nhánh thành nội dung hoàn chỉnh, thống nhất.
- Lưu file `README.md`.

#### Bước 6: Hoàn tất commit gộp (Merge Commit)
```bash
git add README.md
git commit -m "Merge branch 'feature-update' and resolve merge conflict in README.md"
```

---

### 3. Kết quả kiểm tra lịch sử nhánh (`git log --graph --oneline`)

```text
*   8635203 Merge branch 'feature-update' and resolve merge conflict in README.md
|\  
| * e37b574 Update version description on feature-update branch
* | 4a085f2 Update version description on main branch
|/  
* 94b8798 Initial commit: create README.md
```

---

### 4. Giải thích cơ chế gộp 3 vùng (3-Way Merge)
1. **Base Commit (Tổ tiên chung):** Commit `94b8798` (nơi nhánh `feature-update` được rẽ nhánh từ `main`).
2. **Current Branch Commit (HEAD):** Commit `4a085f2` trên `main`.
3. **Incoming Branch Commit:** Commit `e37b574` trên `feature-update`.

Khi cả 2 nhánh cùng sửa đổi cùng một dòng so với commit tổ tiên chung, Git không thể tự quyết định lấy thay đổi nào nên đưa ra trạng thái `CONFLICT`. Lập trình viên phải xử lý thủ công (3-Way Merge) và tạo commit gộp có 2 nhánh cha (parents).
