# Bài 1: Khảo sát FHS và Phân quyền File/Folder nâng cao

## 1. Mục tiêu

- Tạo cấu trúc thư mục theo FHS tại `/var/www/my-app`.
- Thực hành phân quyền thư mục bằng octal.
- Thay đổi Owner và Group của thư mục.
- Sử dụng tài khoản non-root làm Owner và nhóm `www-data` làm Group.

## 2. Cấu trúc thư mục

Đã tạo cấu trúc:

```text
/var/www/my-app/
├── public/
└── logs/
```

- `public`: chứa các trang/tài nguyên static của ứng dụng web.
- `logs`: chứa các file nhật ký hệ thống.

## 3. Các lệnh đã thực hiện

### Tạo cấu trúc thư mục

```bash
sudo mkdir -p /var/www/my-app/public
sudo mkdir -p /var/www/my-app/logs
```

### Tạo tài khoản non-root

```bash
adduser ptit
```

### Kiểm tra tài khoản

```bash
id ptit
```

Kết quả:

```text
uid=1000(ptit) gid=1000(ptit) groups=1000(ptit),100(users)
```

### Gán Owner và Group

```bash
chown -R ptit:www-data /var/www/my-app
```

- Owner: `ptit`
- Group: `www-data`

### Phân quyền thư mục `public`

```bash
chmod 750 /var/www/my-app/public
```

Quyền `750`:

```text
Owner  = rwx
Group  = r-x
Others = ---
```

### Phân quyền thư mục `logs`

```bash
chmod 770 /var/www/my-app/logs
```

Quyền `770`:

```text
Owner  = rwx
Group  = rwx
Others = ---
```

## 4. Kiểm tra kết quả

Lệnh:

```bash
ls -la /var/www/my-app
```

Kết quả:

```text
total 16
drwxr-xr-x 4 ptit www-data 4096 Oct  6 12:18 .
drwxr-xr-x 3 root root     4096 Oct  6 12:18 ..
drwxrwx--- 2 ptit www-data 4096 Oct  6 12:18 logs
drwxr-x--- 2 ptit www-data 4096 Oct  6 12:18 public
```

## 5. Kiểm tra quyền dạng số

Lệnh:

```bash
stat -c '%A %a %U %G %n' /var/www/my-app/public /var/www/my-app/logs
```

Kết quả:

```text
drwxr-x--- 750 ptit www-data /var/www/my-app/public
drwxrwx--- 770 ptit www-data /var/www/my-app/logs
```

## 6. Kết quả

- Thư mục `/var/www/my-app/public` có quyền `750`.
- Thư mục `/var/www/my-app/logs` có quyền `770`.
- Owner của hai thư mục là tài khoản non-root `ptit`.
- Group của hai thư mục là `www-data`.
- Users khác không có quyền trên `public` và `logs`.

Bài tập đã hoàn thành theo yêu cầu.
