# B. Cài đặt Ubuntu + Docker
## 1. Cài đặt hệ điều hành Ubuntu 24.04.4 LTS
Công cụ giả lập: VirtualBox.

Hệ điều hành: Ubuntu Server 24.04 LTS.

Trạng thái: Đã cài đặt hoàn tất và khởi động thành công vào hệ thống.

<img width="1280" height="800" alt="VirtualBox_Ubuntu_Docker_13_04_2026_18_40_22" src="https://github.com/user-attachments/assets/8247795e-a5bd-4bbd-a403-ed63121d1e2e" />


## 2. Cấu hình mạng và truy cập SSH từ Windows
Phương thức: Sử dụng mạng NAT và cấu hình Port Forwarding.

Thông số cấu hình:

Host Port: 2222.

Guest Port: 22.

Lệnh truy cập từ CMD Windows: ssh phuong@127.0.0.1 -p 2222.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4c781dfa-05b5-4363-9215-d078d6d88289" />
<img width="1093" height="639" alt="image" src="https://github.com/user-attachments/assets/d87f08fd-72bc-4fb9-a37e-34834673996c" />


## 3. Tìm hiểu các lệnh cơ bản của Ubuntu
Thực hiện thao tác các lệnh quản trị hệ thống:

ip -4 addr: Xem địa chỉ IP của máy ảo (Kết quả: 10.0.2.15).
<img width="916" height="198" alt="image" src="https://github.com/user-attachments/assets/a1d9799d-2988-4802-8d17-d51ed8fc1c99" />

ls, mkdir, cd: Kiểm tra và quản lý thư mục. 222
<img width="605" height="191" alt="image" src="https://github.com/user-attachments/assets/f1fb0a4d-06fa-4847-8bcf-2e97c8f9b7a6" />

sudo nano: Chỉnh sửa cấu hình hệ thống.
<img width="937" height="642" alt="image" src="https://github.com/user-attachments/assets/04895d0c-8ef3-4751-bbd0-ce46122b1b24" />

## 4. Cài đặt Docker cho Ubuntu
Sử dụng các lệnh sau để cài đặt môi trường Docker:

sudo apt updatecd
<img width="521" height="67" alt="image" src="https://github.com/user-attachments/assets/223cf3b3-8428-49d0-8786-3e6f44f8163a" />

sudo apt install docker.io -y
<img width="632" height="148" alt="image" src="https://github.com/user-attachments/assets/8d3da3d1-51cc-4c7e-a370-c630160673d6" />

sudo systemctl enable --now docker

## 5. Kiểm tra phiên bản Docker và Docker Compose
Docker version: 29.1.3.

Lệnh kiểm tra:

Bash
docker --version
<img width="530" height="40" alt="image" src="https://github.com/user-attachments/assets/7f8cc70e-43f3-412d-a518-52a80be417d3" />

docker compose version
<img width="499" height="53" alt="image" src="https://github.com/user-attachments/assets/84c37782-47bf-4c01-8d95-e081636cab33" />

## 6. Cấu hình Docker chạy không cần quyền sudo
Thêm người dùng hiện tại vào nhóm docker để thực thi lệnh nhanh hơn:

Bash
sudo usermod -aG docker $USER
newgrp docker
Kiểm tra: Chạy lệnh docker ps thành công mà không cần tiền tố sudo.
<img width="714" height="96" alt="image" src="https://github.com/user-attachments/assets/718213e6-2513-46dc-bbcb-47b368c26d48" />


## 7. Cấu hình tường lửa (UFW) cho các cổng dịch vụ
Cho phép lưu thông qua các cổng 80 (Web), 1880 (Node-RED), và 9630:

Bash
sudo ufw allow 80/tcp
sudo ufw allow 1880/tcp
sudo ufw allow 9630/tcp
sudo ufw reload
sudo ufw status
<img width="618" height="179" alt="image" src="https://github.com/user-attachments/assets/80590f4b-efc4-4b7e-b778-ad942c7ea041" />
