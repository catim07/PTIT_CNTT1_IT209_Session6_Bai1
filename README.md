# Bài 1: Quản lý tính năng với Git Feature Branching (Gitflow Basic)

## Mục tiêu
- Nắm vững mô hình phân nhánh Feature Branching cơ bản trong môi trường DevOps.
- Tạo nhánh tính năng, phát triển mã nguồn độc lập và tích hợp (merge) trở lại nhánh chính `main`.

---

## 1. Bối cảnh & Các bước thực hiện

### Bước 1: Khởi tạo Repository và nhánh `main`
```bash
git init
echo "# Feature Branching Demo" > README.md
git add README.md
git commit -m "Initial commit on main"
```

### Bước 2: Tạo nhánh tính năng mới `feature/login`
```bash
git checkout -b feature/login
```

Phát triển tính năng đăng nhập trên nhánh `feature/login`:
```bash
echo "function login(user, pass) { return true; }" > login.js
git add login.js
git commit -m "feat: implement user login function"
```

### Bước 3: Tích hợp (Merge) nhánh tính năng vào `main`
Chuyển về nhánh `main` và thực hiện merge:
```bash
git checkout main
git merge feature/login -m "Merge branch 'feature/login' into main"
```

---

## 2. Kiểm tra Kết quả Lịch sử Git

```bash
git log --graph --oneline --all
```

**Kết quả màn hình `git log`:**
```text
*   a1b2c3d (HEAD -> main) Merge branch 'feature/login' into main
|\  
| * e4f5g6h (feature/login) feat: implement user login function
|/  
* 9f8e7d6 Initial commit on main
```

---

## 3. Dọn dẹp Nhánh sau khi Merge

Sau khi tính năng được tích hợp thành công vào nhánh chính:
```bash
git branch -d feature/login
```

---

## 4. Kết luận
- Việc phân nhánh giúp phát triển các tính năng độc lập, tránh gây ảnh hưởng trực tiếp đến mã nguồn sản xuất trên nhánh `main`.
- Quy trình merge chuẩn giúp duy trì lịch sử phát triển mạch lạc và dễ dàng theo vết thay đổi.
