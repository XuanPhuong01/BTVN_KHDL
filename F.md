# F. QUẢN TRỊ VÀ GỠ LỖI HỆ THỐNG (DEBUGGING)
## 1. Kiểm tra trạng thái và Nhật ký (Logs)
Thao tác: Khi hệ thống không hoạt động như ý muốn, bước đầu tiên là kiểm tra trạng thái container và xem nhật ký hoạt động của từng dịch vụ.

Lệnh thực hiện:

docker compose ps: Kiểm tra danh sách container, xem có cái nào bị trạng thái "Exit" hoặc "Restarting" không.

docker logs mynginx: Xem nhật ký của Nginx (để kiểm tra lỗi cấu hình Proxy).

docker logs mynodered: Xem nhật ký của Node-RED (để kiểm tra lỗi API).

Giải thích: Log giúp ta biết chính xác tại sao dịch vụ bị lỗi (ví dụ: thiếu file, sai cú pháp, hoặc lỗi kết nối mạng).

<img width="977" height="118" alt="image" src="https://github.com/user-attachments/assets/4adc0dd0-b6fe-423f-80c0-929fd4576c2f" />

<img width="980" height="240" alt="image" src="https://github.com/user-attachments/assets/9c57855a-dbb9-4f5b-ba79-1c844c5ab9d1" />

<img width="886" height="580" alt="image" src="https://github.com/user-attachments/assets/3d5ab0b1-afe7-4a53-9205-617204d06be6" />

## 2. Cấu hình Healthcheck cho dịch vụ
Thao tác: Thêm tính năng tự động kiểm tra sức khỏe hệ thống vào file docker-compose.yml.

Cấu hình mẫu:

YAML

healthcheck:

  test: ["CMD", "curl", "-f", "http://localhost:1880"]
  
  interval: 30s
  
  timeout: 10s
  
  retries: 3

Giải thích: Healthcheck giúp Docker tự động theo dõi xem dịch vụ bên trong container có thực sự phản hồi hay không, thay vì chỉ kiểm tra xem container đó có đang bật hay không.

<img width="964" height="768" alt="image" src="https://github.com/user-attachments/assets/3c1b54b6-d954-40d4-834e-3e6f7e90206d" />


## 3. Giới hạn tài nguyên (Resource Limits)
Thao tác: Thiết lập mức trần tiêu thụ RAM và CPU cho từng dịch vụ để tránh việc một container gặp lỗi làm treo toàn bộ máy ảo.

Cấu hình mẫu:

YAML

deploy:

  resources:
  
    limits:
    
      memory: 512M

Giải thích: Việc giới hạn RAM (ví dụ 512MB) giúp hệ thống vận hành ổn định, đặc biệt là khi chạy nhiều dịch vụ như Nginx và Node-RED cùng lúc trên máy ảo có cấu hình thấp.

## 4. Giám sát hiệu năng thực tế
Thao tác: Quan sát mức độ chiếm dụng tài nguyên thực tế của các dịch vụ đang chạy.

Lệnh thực hiện: docker compose stats.

Giải thích: Lệnh này hiển thị bảng thống kê thời gian thực về %CPU, lượng RAM đang sử dụng và lưu lượng mạng của từng container. Đây là công cụ đắc lực để tối ưu hóa hệ thống.

<img width="945" height="233" alt="image" src="https://github.com/user-attachments/assets/95d6459b-ee86-4884-8fac-8eb1180349f5" />
