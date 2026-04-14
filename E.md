# E. Triển khai (level test) ứng dụng
## 1. Khởi chạy hệ thống đồng bộ với Docker Compose
Sử dụng Docker Compose để khởi tạo toàn bộ các dịch vụ (Nginx, Node-RED) đã được định nghĩa. Việc này giúp hệ thống chạy nhất quán trong môi trường container.

Lệnh thực hiện:

Bash
cd ~/myapp
docker compose up -d --force-recreate
Kiểm tra trạng thái: Đảm bảo các container đều ở trạng thái Running (Up).

<img width="1078" height="311" alt="image" src="https://github.com/user-attachments/assets/d6c0b360-fed1-40c4-b7ad-9d57d140c565" />


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
 
 <img width="1103" height="629" alt="image" src="https://github.com/user-attachments/assets/64e5c543-9bdc-4e3f-9583-941e6cf0f6ed" />


## 4. Kiểm thử kết quả cuối cùng (End-to-End Test)
Truy cập trình duyệt tại địa chỉ 127.0.0.1:8080 để kiểm tra sự phối hợp giữa các dịch vụ.

Yêu cầu: Trang web phải hiển thị đầy đủ thông tin cá nhân và load được thông báo từ API Node-RED.

Kết quả: Trang web hiển thị nội dung cá nhân kèm dòng chữ: "Chào bạn Phương, kết nối API qua Nginx thành công!". Điều này chứng minh Nginx đã chuyển tiếp yêu cầu (proxy) đến Node-RED chính xác.

<img width="694" height="727" alt="image" src="https://github.com/user-attachments/assets/c79bad6f-2548-40e5-b43c-12191230bd87" />


