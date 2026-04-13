# C. CẤU HÌNH DOCKER COMPOSE
## 1. Khởi tạo cấu trúc thư mục 
Thao tác: Tạo thư mục gốc myapp và các thư mục con (myweb, nginx, nodered) để quản lý dữ liệu riêng biệt cho từng dịch vụ.

Lệnh thực hiện:

Bash
mkdir ~/myapp && cd ~/myapp

mkdir myweb nginx nodered

Giải thích: Việc chia thư mục giúp dữ liệu không bị mất khi xóa container (Volume Mapping).

<img width="627" height="186" alt="image" src="https://github.com/user-attachments/assets/45ec2124-0352-49f0-9209-09e798256e0e" />


## 2. Tạo nội dung trang Web cá nhân
Thao tác: Tạo file index.html trong thư mục ./myweb.

Lệnh thực hiện: nano ./myweb/index.html

Giải thích: Đây là trang tĩnh sẽ được Nginx hiển thị khi truy cập vào tên miền.


<img width="789" height="629" alt="image" src="https://github.com/user-attachments/assets/2cdd5978-4c93-40fd-be33-b4f3d2fdb85c" />


## 3. Cấu hình Docker Compose
Thao tác: Tạo file docker-compose.yml để định nghĩa các dịch vụ:

Node-RED: Chạy tại cổng 1880, lưu dữ liệu tại ./nodered.

Nginx: Chạy tại cổng 80, ánh xạ file cấu hình và thư mục web từ máy ảo vào container.

Giải thích: File này giúp quản lý toàn bộ hệ thống bằng một lệnh duy nhất thay vì chạy từng container riêng lẻ.

[PASTE ẢNH: Chụp nội dung file docker-compose.yml bạn đã soạn thảo]


## 4. Cấu hình Nginx Reverse Proxy
Thao tác: Chỉnh sửa file ./nginx/nginx.conf.

Cấu hình chính:

listen 80: Lắng nghe cổng 80.

server_name: Đặt sub-domain tùy ý .

location /: Trỏ về thư mục /myweb (chứa file index.html).

location /api: Sử dụng proxy_pass để đẩy yêu cầu sang Node-RED.

Giải thích: Nginx đóng vai trò là "cổng bảo vệ", tiếp nhận mọi yêu cầu và điều hướng chúng đến đúng nơi.


<img width="781" height="639" alt="image" src="https://github.com/user-attachments/assets/29b2d1f3-101c-4a19-9c82-0d52cffa8cfb" />


## 5. Khởi chạy hệ thống lần đầu
Thao tác: Chạy lệnh để Docker tải ảnh (image) và tạo các file hệ thống ban đầu.

Lệnh thực hiện: docker compose up -d

Giải thích: Tham số -d (detached mode) giúp container chạy ngầm dưới nền. Lúc này Node-RED sẽ tự động sinh file settings.js trong thư mục ./nodered.

[PASTE ẢNH: Chụp kết quả lệnh docker compose up -d hiện chữ [OK] hoặc Done]

## 6. Cấu hình bảo mật cho Node-RED
Thao tác: Chỉnh sửa file ./nodered/settings.js để kích hoạt tính năng adminAuth.

Giải thích: Theo mặc định Node-RED không có mật khẩu. Việc sửa file này giúp bắt buộc người dùng phải đăng nhập mới được vào trang quản trị flow.

Thao tác sau khi sửa: Khởi động lại để áp dụng cấu hình: docker compose restart.

[PASTE ẢNH: Chụp đoạn code adminAuth trong file settings.js bạn đã bỏ dấu comment //]

## 7. Kiểm tra kết quả cuối cùng
Thao tác: Truy cập vào trình duyệt theo địa chỉ IP máy ảo hoặc sub-domain đã cấu hình.

Kết quả mong đợi:

Truy cập cổng 80: Hiện thông tin cá nhân.

Truy cập cổng 1880: Hiện màn hình đăng nhập Node-RED.

[PASTE ẢNH: Chụp màn hình trình duyệt hiển thị trang Web cá nhân của bạn]
[PASTE ẢNH: Chụp màn hình trình duyệt hiển thị trang đăng nhập Node-RED]
