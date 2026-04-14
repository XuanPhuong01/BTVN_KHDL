# G. TRIỂN KHAI ỨNG DỤNG ĐẾN END-USER
Mục này mô tả quá trình đưa ứng dụng từ môi trường local lên môi trường Internet công cộng thông qua giải pháp Cloudflare Tunnel, giúp End-user có thể truy cập mà không cần mở port trên Router (Port Forwarding).

## 1. Quy trình triển khai hệ thống
Quá trình triển khai được thực hiện qua các bước chuẩn hóa từ câu lệnh Docker đơn lẻ sang hệ thống quản lý tập trung:

Khởi tạo Tunnel: Tạo đường hầm bảo mật trên Dashboard của Cloudflare, chọn phương thức triển khai dành cho Docker.

Chuyển đổi cấu hình: Thực hiện Convert các lệnh docker run thủ công sang định dạng chuẩn trong file docker-compose.yml để dễ dàng quản lý và mở rộng.

Cấu hình hệ thống: Khai báo cấu hình Tunnel và các dịch vụ liên quan vào tệp thực thi chính.

Vận hành: Chạy lệnh docker compose up -d để khởi động toàn bộ hạ tầng ngầm.

Public ứng dụng: Cấu hình Public Hostname trỏ tới container Nginx qua cổng 80 nội bộ. Dữ liệu sẽ đi qua Tunnel an toàn dưới dạng Sub-domain.

## 2. Sơ đồ kiến trúc triển khai
2.1. Góc nhìn của Nhà phát triển (Developer View)
Sơ đồ này thể hiện luồng tư duy từ việc sử dụng các lệnh Ubuntu cơ bản, khai báo qua Docker Compose để quản lý đồng bộ các dịch vụ Nginx (Web/API) và Node-RED trước khi tới tay người dùng.

[Bỏ ảnh: image_3831eb.png - Sơ đồ luồng triển khai từ Ubuntu đến End-user]

2.2. Góc nhìn của Hệ thống (System Architecture View)
Sơ đồ này mô tả luồng dữ liệu thực tế: End-user truy cập qua Internet -> Đi qua đường hầm Cloudflare Tunnel -> Nginx đóng vai trò điều phối (Proxy) -> Trỏ tới các dịch vụ API hoặc Giao diện tương ứng.

[Bỏ ảnh: image_38329e.png - Sơ đồ luồng dữ liệu qua Cloudflare Tunnel]

## 3. Cấu trúc thư mục dự án
Để hệ thống vận hành ổn định, cấu trúc thư mục được tổ chức như sau:

Plaintext
myapp/
├── docker-compose.yml      # Tệp cấu hình chính điều phối toàn bộ container
├── nginx/
│   └── nginx.conf          # Cấu hình Reverse Proxy và Routing cho Web/API
├── myweb/
│   └── index.html          # Giao diện Frontend hiển thị thông tin sinh viên
└── nodered/                # Thư mục lưu trữ dữ liệu của Node-RED
    ├── flows.json          # Lưu trữ các luồng xử lý API Backend
    └── settings.js         # Cấu hình bảo mật (Admin Auth) yêu cầu đăng nhập
## 4. Kết quả đạt được
Bảo mật: Ứng dụng không cần phơi bày IP thật của máy chủ ra Internet.

Tính sẵn sàng: End-user có thể truy cập ổn định thông qua Sub-domain (Ví dụ: app.yourdomain.com).

Quản trị: Dễ dàng kiểm soát quyền truy cập và giám sát lưu lượng thông qua Cloudflare Dashboard.

[Bỏ ảnh: Ảnh chụp màn hình trình duyệt truy cập thành công qua URL sub-domain của Cloudflare]
