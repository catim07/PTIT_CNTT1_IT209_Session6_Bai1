# Bài 1: Quản lý người dùng giới hạn và Truyền tải dữ liệu qua SFTP trên Windows

## Mục tiêu
- Tạo tài khoản người dùng giới hạn `sftp-user` phục vụ tác vụ truyền nhận tệp tin từ xa.
- Kết nối và truyền tải tệp tin nhật ký (`/var/log/app-backup/backup-check.log`) an toàn từ VPS Linux về máy tính Windows thông qua WinSCP / Bitvise SFTP Client.

---

## 1. Thiết lập trên Máy chủ Linux (VPS)

### Bước 1: Khởi tạo tài khoản `sftp-user` không thuộc nhóm sudo
```bash
sudo adduser sftp-user
```
*(Nhập mật khẩu an toàn và hoàn tất khởi tạo user)*

### Bước 2: Tạo thư mục log giả lập và gán quyền truy cập
```bash
sudo mkdir -p /var/log/app-backup/
sudo touch /var/log/app-backup/backup-check.log
sudo bash -c 'echo "Backup status: SUCCESS at $(date)" > /var/log/app-backup/backup-check.log'

# Phân quyền hạn chế
sudo chown -R root:sftp-user /var/log/app-backup
sudo chmod 750 /var/log/app-backup
sudo chmod 640 /var/log/app-backup/backup-check.log
```

---

## 2. Thao tác trên máy tính Windows (SFTP Client)

### Cấu hình kết nối Bitvise SSH Client / WinSCP:
- **Host / IP:** `103.x.x.x` (IP Public của VPS)
- **Port:** `22` (hoặc Port SSH tùy chỉnh)
- **Username:** `sftp-user`
- **Password:** `<Mật_khẩu_sftp-user>`

### Thực hiện tải File:
1. Mở **New SFTP Window** trong phần mềm Client.
2. Truy cập đường dẫn Remote: `/var/log/app-backup/`.
3. Tải tệp `backup-check.log` về thư mục máy tính Windows cục bộ.

---

## 3. Nhật ký kiểm tra (Verification Logs)

### Lệnh kiểm tra trên VPS:
```bash
$ id sftp-user
uid=1002(sftp-user) gid=1002(sftp-user) groups=1002(sftp-user)

$ ls -l /var/log/app-backup/backup-check.log
-rw-r----- 1 root sftp-user 45 Oct 7 10:30 /var/log/app-backup/backup-check.log
```

### Nội dung file log tải về trên Windows:
```text
Backup status: SUCCESS at Wed Oct  7 10:30:15 UTC 2026
```

---

## 4. Kết luận
- User `sftp-user` không có quyền `sudo`, đảm bảo nguyên tắc bảo mật tối thiểu (Principle of Least Privilege).
- File log đã được truyền tải an toàn và nguyên vẹn từ máy chủ về Windows qua giao thức SFTP.
