# Bài 4: Quản lý tệp tin bỏ qua (.gitignore) và Sửa lịch sử (Amend)

## 1. Bối cảnh
File `credentials.txt` chứa thông tin bảo mật (credentials) đã bị commit nhầm lên Git.
Mục tiêu: gỡ file khỏi sự theo dõi của Git **nhưng vẫn giữ lại file vật lý** trên máy local,
cấu hình `.gitignore` để Git bỏ qua file này về sau, và sửa lại thông điệp commit gần nhất.

---

## 2. Gỡ file khỏi cache theo dõi của Git

```bash
git rm --cached credentials.txt
```

- `git rm --cached` chỉ gỡ file khỏi **vùng staging / index** (cache theo dõi) của Git.
- File vật lý `credentials.txt` **vẫn còn nguyên** trong thư mục làm việc (working directory).
- Nếu dùng `git rm` (không có `--cached`) thì file sẽ bị xóa luôn trên ổ đĩa → vi phạm ràng buộc đề bài.

Kết quả sau lệnh:
```
rm 'credentials.txt'
```

---

## 3. Cấu hình .gitignore

Thêm `credentials.txt` vào file `.gitignore`:

```
credentials.txt
```

**Lưu ý quan trọng:** `.gitignore` chỉ có hiệu lực với các file **chưa được track**.
Vì `credentials.txt` đã bị commit trước đó (đang được track), nên **bắt buộc** phải chạy
`git rm --cached` ở bước 2 trước thì `.gitignore` mới phát huy tác dụng.

---

## 4. Sửa thông điệp commit gần nhất bằng --amend

```bash
git add .gitignore
git commit --amend -m "Remove credentials from tracking and add .gitignore"
```

- `--amend` sửa trực tiếp vào commit gần nhất, **không tạo thêm commit mới**.
- Thao tác này gộp luôn thay đổi của `.gitignore` và việc gỡ `credentials.txt`
  vào commit gần nhất với thông điệp mới, sạch sẽ.

---

## 5. Kiểm tra kết quả

### `git log -n 1`
```
commit caa45426e3154b15b516c94bd276452b729e4d8c (HEAD -> master)
Author: cccc9126 <huyhuy9126@gmail.com>
Date:   Mon Oct 5 10:07:33 2026 +0700

    Remove credentials from tracking and add .gitignore

```

### `git status`
```
On branch main
nothing to commit, working tree clean
```

- File `credentials.txt` **không còn xuất hiện** trong danh sách theo dõi của Git
  (không ở trạng thái Staged hay Modified), vì đã bị `.gitignore` loại trừ.
- Lịch sử commit gần nhất hiển thị **thông điệp đã được sửa đổi** thành công.

---

## 6. Tổng kết lệnh đã dùng

| Bước | Lệnh | Mục đích |
|------|------|----------|
| 1 | `git rm --cached credentials.txt` | Gỡ file khỏi tracking, giữ file vật lý |
| 2 | Tạo `.gitignore` chứa `credentials.txt` | Bỏ qua file về sau |
| 3 | `git add .gitignore` | Đưa .gitignore vào staging |
| 4 | `git commit --amend -m "..."` | Sửa message commit gần nhất |
| 5 | `git status` / `git log -n 1` | Kiểm tra kết quả |
