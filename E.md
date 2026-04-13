# E. Triển khai (level test) ứng dụng
## 1. Khởi chạy hệ thống đồng bộ với Docker Compose
Sử dụng Docker Compose để khởi tạo toàn bộ các dịch vụ (Nginx, Node-RED) đã được định nghĩa. Việc này giúp hệ thống chạy nhất quán trong môi trường container.

Lệnh thực hiện:

Bash
cd ~/myapp
docker compose up -d --force-recreate
Kiểm tra trạng thái: Đảm bảo các container đều ở trạng thái Running (Up).

[DÁN ẢNH]: Ảnh màn hình Terminal khi gõ lệnh docker ps (hiện danh sách container mynginx và mynodered đang Up).

## 2. Cấu hình API Backend trên Node-RED (Cổng 1880)
Để kiểm thử khả năng kết nối hệ thống, tiến hành xây dựng một API đơn giản bằng Node-RED.

Cấu hình luồng: Sử dụng các node http in, function và http response.

Endpoint: GET /hello

Nội dung xử lý (Function):

JavaScript
msg.payload = { message: "Chào bạn Phương, kết nối API qua Nginx thành công!" };
return msg;
<img width="967" height="376" alt="image" src="https://github.com/user-attachments/assets/aa6b797d-c6f9-42c3-b9ba-30902c7ae433" />


## 3. Tích hợp Fetch API vào Frontend (Nginx)
Thực hiện sửa file index.html tại thư mục /myweb để gọi dữ liệu từ Node-RED thông qua cơ chế Reverse Proxy đã cấu hình tại Nginx (location /api/).

Thao tác sửa file:

Bash
nano ~/myapp/myweb/index.html
[DÁN ẢNH]: Ảnh chụp màn hình Terminal lúc đang sửa file index.html (giống ảnh image_cc2b51.png bạn có, hiện rõ tên Phương và đoạn mã script).

## 4. Kiểm thử kết quả cuối cùng (End-to-End Test)
Truy cập trình duyệt tại địa chỉ 127.0.0.1 để kiểm tra sự phối hợp giữa các dịch vụ.

Yêu cầu: Trang web phải hiển thị đầy đủ thông tin cá nhân và load được thông báo từ API Node-RED.

Kết quả: Trang web hiển thị nội dung cá nhân kèm dòng chữ: "Chào bạn Phương, kết nối API qua Nginx thành công!". Điều này chứng minh Nginx đã chuyển tiếp yêu cầu (proxy) đến Node-RED chính xác.

[DÁN ẢNH]: Ảnh chụp trình duyệt Chrome lúc gõ 127.0.0.1 hiện thông tin cá nhân của bạn.
