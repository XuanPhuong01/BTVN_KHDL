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

<img width="801" height="647" alt="image" src="https://github.com/user-attachments/assets/ebc02914-0282-4106-808e-b367cff11472" />


## 4. Cấu hình Nginx Reverse Proxy
Thao tác: Chỉnh sửa file ./nginx/nginx.conf.

Cấu hình chính:

listen 80: Lắng nghe cổng 80.

server_name: Đặt sub-domain tùy ý .

location /: Trỏ về thư mục /myweb (chứa file index.html).

location /api: Sử dụng proxy_pass để đẩy yêu cầu sang Node-RED.

Giải thích: Nginx đóng vai trò là "cổng bảo vệ", tiếp nhận mọi yêu cầu và điều hướng chúng đến đúng nơi.

<img width="781" height="633" alt="image" src="https://github.com/user-attachments/assets/68e8bf0a-875e-486e-95df-795c351c205e" />

## 5. Khởi chạy hệ thống lần đầu
Thao tác: Chạy lệnh để Docker tải ảnh (image) và tạo các file hệ thống ban đầu.

Lệnh thực hiện: docker compose up -d

Giải thích: Tham số -d (detached mode) giúp container chạy ngầm dưới nền. Lúc này Node-RED sẽ tự động sinh file settings.js trong thư mục ./nodered.


<img width="793" height="119" alt="image" src="https://github.com/user-attachments/assets/ff335a57-7d9f-4345-b591-759392ee884e" />


## 6. Cấu hình bảo mật cho Node-RED
Thao tác: Chỉnh sửa file ./nodered/settings.js để kích hoạt tính năng adminAuth.

Giải thích: Theo mặc định Node-RED không có mật khẩu. Việc sửa file này giúp bắt buộc người dùng phải đăng nhập mới được vào trang quản trị flow.

Thao tác sau khi sửa: Khởi động lại để áp dụng cấu hình: docker compose restart.


<img width="781" height="316" alt="image" src="https://github.com/user-attachments/assets/319f607d-5c2f-401c-a490-12e6c3f79eb3" />


## 7. Kiểm tra kết quả cuối cùng
Thao tác: Truy cập vào trình duyệt theo địa chỉ IP máy ảo hoặc sub-domain đã cấu hình.

Kết quả mong đợi:

Truy cập cổng 80: Hiện thông tin cá nhân.

Truy cập cổng 1880: Hiện màn hình đăng nhập Node-RED.

<img width="934" height="919" alt="image" src="https://github.com/user-attachments/assets/80eab34e-c407-4cbd-bb8e-e4e2e14f804c" />


<img width="1914" height="1079" alt="image" src="https://github.com/user-attachments/assets/32fb64f8-7286-411a-bd9d-f81b4b326896" />



