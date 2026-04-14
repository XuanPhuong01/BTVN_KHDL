# G. TRIỂN KHAI ỨNG DỤNG ĐẾN END-USER
Phần này trình bày quá trình đưa ứng dụng từ máy chủ nội bộ Ubuntu ra Internet công cộng thông qua giải pháp Cloudflare Tunnel, giúp người dùng truy cập an toàn mà không cần mở cổng trên thiết bị mạng.

## 1. Tạo tunnel (đường hầm) trên Cloudflare
Truy cập Dashboard Cloudflare Zero Trust, chọn Networks -> Tunnels để tạo đường hầm mới mang tên xuanphuong-tuunel.

Chọn loại triển khai là Docker để nhận mã Token định danh cho kết nối bảo mật giữa máy chủ và Cloudflare.

<img width="1835" height="1019" alt="Screenshot 2026-04-14 095628" src="https://github.com/user-attachments/assets/d89c28f1-4f4d-40d0-ac28-f39059882714" />

## 2. Convert lệnh docker run sang dạng Docker Compose
Từ lệnh gốc của Cloudflare: docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token <TOKEN>.

Tiến hành chuyển đổi các tham số này sang định dạng YAML để đưa vào tệp điều phối dịch vụ chung, giúp quản lý đồng bộ với Nginx và Node-RED.

## 3. Khai báo vào file docker-compose.yml
Cấu trúc dịch vụ tunnel được thêm vào tệp cấu hình chính. Việc này giúp tự động hóa quá trình kết nối mỗi khi hệ thống khởi động.

<img width="956" height="1021" alt="Screenshot 2026-04-14 100417" src="https://github.com/user-attachments/assets/5e6c0307-22cd-43ad-a3dd-f5e4a9d1dcde" />


## 4. Chạy lại Docker Compose
Sử dụng lệnh docker compose up -d --force-recreate để hệ thống khởi tạo lại toàn bộ các dịch vụ bao gồm cả đường hầm Cloudflare.

Kiểm tra trạng thái các Container để đảm bảo tất cả đều vận hành ổn định.

<img width="957" height="536" alt="Screenshot 2026-04-14 100739" src="https://github.com/user-attachments/assets/900df43f-ed44-4c6a-83d1-afde60fcb334" />


## 5. Public ứng dụng bằng cách thêm Router
Tại mục Public Hostname trên Cloudflare, thiết lập một quy tắc định tuyến để dẫn luồng dữ liệu từ Internet vào máy chủ.

Cấu hình tên miền app.xuanphuong.id.vn trỏ về dịch vụ nội bộ http://mynginx:80.

<img width="1797" height="152" alt="image" src="https://github.com/user-attachments/assets/ff302664-91a9-422a-b283-a76c6a918f29" />


<img width="1919" height="1064" alt="Screenshot 2026-04-14 101035" src="https://github.com/user-attachments/assets/6da14293-6d7a-4274-a082-2e4d200bc9b7" />

<img width="1536" height="703" alt="Screenshot 2026-04-14 102258" src="https://github.com/user-attachments/assets/58012c5b-31e2-434a-89b6-2b8d5333d65d" />


<img width="1068" height="887" alt="Screenshot 2026-04-14 102422" src="https://github.com/user-attachments/assets/dc9c221c-c62e-4bd4-96dd-0374dabb856a" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2dba5f8e-e660-4b42-aa94-511463c07f8a" />


## 6. Kiểm tra url sub-domain cho mọi End-user
Sử dụng trình duyệt trên thiết bị di động (sử dụng mạng 4G/ngoài mạng nội bộ) để kiểm tra tính sẵn sàng của ứng dụng.

Kết quả: Ứng dụng hiển thị đầy đủ giao diện và các tính năng API đã thiết lập.

<img width="828" height="1792" alt="image" src="https://github.com/user-attachments/assets/133ebf4b-4ffe-4278-aed4-05c659b44c31" />


Cấu trúc thư mục dự án:
Plaintext
myapp/
├── docker-compose.yml      # Tệp cấu hình điều phối các Container

├── nginx/
│   └── nginx.conf          # Cấu hình Reverse Proxy cho Web/API

├── myweb/
│   └── index.html          # Giao diện thông tin sinh viên

└── nodered/                # Thư mục lưu trữ dữ liệu Backend
    └── settings.js         # Cấu hình bảo mật (Admin Auth)
