# Hướng dẫn Setup Odoo với Supabase

## Yêu cầu
- Docker và Docker Compose
- Kết nối internet ổn định

## Cài đặt lần đầu

### 1. Clone repository
```bash
git clone <repository-url>
cd odoo
```

### 2. Tạo file .env (không có trong Git)
```bash
cp .env.example .env
```
Hoặc tạo file `.env` với nội dung:
```
ODOO_VERSION=17.0
```

### 3. Khởi động và cài đặt database lần đầu
```bash
# Khởi tạo database với module base
docker compose run --rm odoo odoo -i base -d postgres --stop-after-init

# Sau đó chạy bình thường
docker compose up -d
```

### 4. Truy cập Odoo
- URL: http://localhost:8069
- Database: postgres
- Email: admin
- Password: admin (đổi sau lần đăng nhập đầu)

## Lưu ý quan trọng

### Filestore
- Thư mục `odoo-data/` chứa filestore và được ignore một phần
- **KHÔNG** push `odoo-data/filestore/` lên Git (quá lớn)
- Mỗi máy mới cần khởi tạo lại database như hướng dẫn trên

### Cấu hình Supabase
File `config/odoo.conf` đã được cấu hình với Supabase:
- Host: aws-1-ap-southeast-1.pooler.supabase.com
- Port: 6543 (Transaction pooler)
- Database: postgres
- SSL: required

## Troubleshooting

### Lỗi "FileNotFoundError" khi load assets
Chạy lại lệnh khởi tạo:
```bash
docker compose down
docker compose run --rm odoo odoo -i base -d postgres --stop-after-init
docker compose up -d
```

### Lỗi kết nối Database
Kiểm tra:
1. Supabase có đang hoạt động không
2. IP của bạn có được whitelist trong Supabase không
3. Thông tin đăng nhập trong `config/odoo.conf` có đúng không

### Reset hoàn toàn
```bash
docker compose down -v
rm -rf odoo-data/
# Sau đó làm lại bước 3
```
