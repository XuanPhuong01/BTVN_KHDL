
## 1. Tại sao dùng Nginx làm Reverse Proxy mà không trỏ thẳng Tunnel vào Node-RED?
Quản lý tập trung: Nginx đóng vai trò là "cổng chào", giúp điều phối nhiều dịch vụ cùng lúc (vừa chạy Web tĩnh, vừa chạy API Node-RED) trên cùng một tên miền.

Bảo mật & Hiệu suất: Nginx giúp ẩn địa chỉ thực của Node-RED, hỗ trợ đệm dữ liệu (caching) và xử lý các kết nối đồng thời tốt hơn.

## 2. Sự khác biệt giữa Mount file và Mount thư mục trong Docker?
Mount file: Chỉ gắn kết một tệp duy nhất (ví dụ: nginx.conf). Thường dùng khi chỉ muốn ghi đè cấu hình cụ thể mà không làm ảnh hưởng các file khác trong container.

Mount thư mục: Gắn kết toàn bộ folder (ví dụ: ./nodered). Dùng để đồng bộ và lưu trữ lượng lớn dữ liệu phát sinh (Flows, Settings) bền vững trên máy chủ.

## 3. Thay đổi file index.html nội dung có đổi ngay không? Tại sao?
Có thay đổi ngay.

Tại sao: Vì chúng ta sử dụng cơ chế Volume Mount. File index.html ở máy Ubuntu và file bên trong container thực chất là "soi gương" của nhau. Khi lưu file ở Ubuntu, container Nginx sẽ đọc được nội dung mới ngay lập tức mà không cần khởi động lại.

## 4. Ý nghĩa của restart: always và restart: unless-stopped?
restart: always: Container luôn tự khởi động lại trong mọi trường hợp (lỗi, tắt máy, reset docker).

restart: unless-stopped: Tương tự như always, nhưng nếu bạn chủ động dùng lệnh docker stop để dừng container thì nó sẽ không tự chạy lại cho đến khi bạn bật thủ công.

## 5. Khai báo chung 1 Network và lợi ích?
Cách làm: Thêm cấu hình networks: vào cuối file và khai báo tên mạng cho từng service.

Lợi ích: Các container có thể "gọi tên" nhau (DNS nội bộ) thay vì dùng IP. Giúp cô lập các dịch vụ, tăng tính bảo mật (dữ liệu đi nội bộ, không lộ ra ngoài).

Sửa đổi file:

YAML
services:
  mynodered:
    networks: [myapp_net]
  mynginx:
    networks: [myapp_net]
  tunnel:
    networks: [myapp_net]
networks:
  myapp_net:
    driver: bridge
## 6. Sử dụng file .env và bảo mật mã nguồn?
Cách làm: Tạo file .env chứa TUNNEL_TOKEN=eyJh..., trong docker-compose.yml gọi biến ${TUNNEL_TOKEN}. Thêm .env vào .gitignore.

Tầm quan trọng: Ngăn chặn việc lộ thông tin nhạy cảm (Token, mật khẩu) lên GitHub. Nếu lộ Token, kẻ xấu có thể chiếm quyền điều khiển đường hầm (Tunnel) và tấn công trực tiếp vào máy chủ của bạn.

## 7. Tại sao nên thêm hậu tố :ro khi mount file cấu hình Nginx?
:ro (Read-Only): Thiết lập quyền "Chỉ đọc".

Lý do: Đảm bảo container Nginx không thể vô tình ghi đè hoặc làm hỏng file cấu hình gốc trên máy Ubuntu, giúp hệ thống vận hành an toàn và ổn định.

## 8. Có cần thiết phải mở cổng (Port) cho các service nữa không?
Không cần thiết. * Lý do: Cloudflare Tunnel tạo một kết nối ngược (Outbound) từ bên trong máy ảo ra Internet. Dữ liệu đi qua "đường hầm" này nên bạn không cần mở Port trên Router (Port Forwarding), giúp tránh được các cuộc tấn công dò quét cổng từ hacker.
