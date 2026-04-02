# Laravel Livewire Sample App

Dự án **laravel-livewire-sample-app** là một ứng dụng mẫu được xây dựng trên nền tảng framework **Laravel 12**, tích hợp cùng **Livewire 4** và thư viện quản trị **Filament 5.4**. Thiết lập tập trung vào việc áp dụng ngay các giải pháp component-based hiện đại để phát triển frontend, đồng thời quản lý giao diện backend dễ dàng với Filament. 

Dự án cũng đi kèm môi trường **Docker** hoàn chỉnh thông qua `docker-compose.yml`, giúp quá trình phát triển (development) và đóng gói triển khai (deployment) trở nên nhanh chóng, khép kín và nhất quán.

## 🚀 Tính năng nổi bật
- **Framework Core**: Sử dụng phiên bản mới nhất từ Laravel 12.x.
- **Micro-Frontend/Reactivity**: Sử dụng Livewire 4 cho phép viết tương tác động (dynamic interfaces) chỉ bằng PHP.
- **Admin Panel**: Tích hợp sẵn hệ sinh thái quản lý dữ liệu với Filament.
- **Môi trường ảo hóa**: Cấu hình chuẩn cho Nginx, PHP sử dụng Docker Compose.

---

## 📂 Cấu trúc thư mục

Dự án được tổ chức theo tiêu chuẩn của Laravel và có bổ sung thêm phần thiết lập Docker hóa. Dưới đây là kiến trúc tóm tắt của hệ thống:

```text
laravel-livewire-sample-app/
├── docker/                 # Cấu hình Docker cho dự án
│   ├── nginx/              # Cấu hình Nginx server
│   └── php/                # Cấu hình PHP (Dockerfile, php.ini)
├── src/                    # Mã nguồn chính của ứng dụng Laravel
│   ├── app/                # Chứa logic cốt lõi (Controllers, Models, Livewire components, Filament resources)
│   ├── bootstrap/          # File khởi động và autoloading cho ứng dụng
│   ├── config/             # Toàn bộ cấu hình hệ thống ứng dụng Laravel
│   ├── database/           # Migrations, factories, và seeders (quản lý CSDL)
│   ├── public/             # Thư mục gốc public web (chứa index.php, CSS, JS, hình ảnh)
│   ├── resources/          # Views (Blade templates), ngôn ngữ và assets chưa biên dịch
│   ├── routes/             # Định nghĩa các route của ứng dụng (web, api, console)
│   ├── storage/            # Không gian lưu trữ cục bộ, cache của framework, tập tin logs
│   ├── tests/              # Chứa các bài test tự động (Pest/PHPUnit: Unit, Feature testing)
│   ├── vendor/             # Các thư viện phụ thuộc được tải về bởi Composer
│   ├── .env.example        # File mẫu cấu hình biến môi trường cục bộ
│   ├── artisan             # Công cụ command-line nội bộ của Laravel
│   ├── composer.json       # Trình quản lý package PHP (Liệt kê: Laravel, Livewire, Filament)
│   ├── package.json        # Định nghĩa các package Node.js (Vite, TailwindCSS, AlpineJS)
│   └── vite.config.js      # Cấu hình Vite build frontend assets
├── docker-compose.yml      # Tệp cấu hình khởi chạy các Docker container (Nginx, PHP, Database)
└── README.md               # Tài liệu này mô tả chi tiết dự án (Bạn đang ở đây)
```

## 🛠️ Hướng dẫn cài đặt và chạy dự án (Docker Compose)

Bạn có thể đưa dự án vào hoạt động thông qua môi trường Docker Compose đã chuẩn bị sẵn theo các bước sau:

**Bước 1:** Chuẩn bị biến môi trường
Mở Terminal/Command Prompt, di chuyển vào thư mục `src` và sao chép cấu hình môi trường:
```bash
cd src
cp .env.example .env
```

**Bước 2:** Khởi chạy các container bằng Docker
Quay lại thư mục gốc dự án (ngang hàng với tệp `docker-compose.yml`) và chạy toàn bộ container (Nginx, PHP, MySQL...):
```bash
cd ..
docker compose up -d --build
```

**Bước 3:** Cài đặt thư viện (PHP + Node.js) và cấu hình Laravel
Sử dụng docker exec để truy cập vào container chạy PHP (ví dụ container có tên service là `php`):
```bash
docker compose exec php bash

# Bên trong container PHP, hãy chạy lần lượt các lệnh:
composer install
php artisan key:generate
php artisan migrate --seed
npm install
npm run build

# Thoát ra môi trường máy host
exit
```

**Bước 4:** Truy cập ứng dụng & Đăng nhập Admin Panel
- **Giao diện người dùng**: Chạy tại địa chỉ `http://localhost:8000` (hoặc cổng được ánh xạ theo cấu hình Docker).
- **Trang Quản Trị (Filament Dashboard)**: Truy cập vào đường dẫn `http://localhost:8000/admin/login`.
  - Bạn có thể tạo tài khoản admin thủ công bằng cách truy cập Tinker trong container ứng dụng bằng lệnh:
    ```bash
    docker exec -it laravel_app php artisan tinker
    ```
  - Sau đó, dán dòng mã sau để tạo tài khoản admin (Email: `admin@admin.com` - Password: `admin`):
    ```php
    App\Models\User::updateOrCreate(['email' => 'admin@admin.com'], ['name' => 'Admin', 'password' => Hash::make('admin')]);
    ```
  - Cuối cùng, sử dụng email và password trên để đăng nhập vào trang quản trị Filament.

