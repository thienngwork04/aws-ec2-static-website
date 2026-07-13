# AWS EC2 Static Website Hosting 🚀
---
## Quá trình triển khai 
### Bước 1: Khởi tạo EC2 Instance & Thiết lập Mạng
* **Khởi tạo:** Tạo một thực thể EC2 mới
* **Cấu hình Security Group (Tường lửa):**
  * `SSH (Port 22)`: Chỉ cho phép truy cập từ địa chỉ IP cá nhân (`My IP`) để bảo mật tối đa cho server.
  * `HTTP (Port 80)`: Cho phép mọi lưu lượng truy cập từ Internet (`0.0.0.0/0`) để người dùng có thể vào xem website.
### Bước 2: Quản lý Key Pair & Phương thức kết nối
Quá trình này có thể linh hoạt thực hiện theo 2 cách tùy thuộc vào công cụ sử dụng:
* **Cách 1: Sử dụng File định dạng `.ppk` (WinSCP)**
  * Tạo Key Pair đuôi `.ppk` để cấu hình trong WinSCP.
  * *Lưu ý bảo mật:* Khi truyền file qua WinSCP, đôi khi ta phải tạm thời cấp quyền `chmod 777` cho thư mục nguồn. Đối với môi trường Lab thì cách này nhanh và chấp nhận được, nhưng không khuyến khích cho môi trường chạy thực tế (Production).
* **Cách 2: Sử dụng File định dạng `.pem` (SSH qua Terminal)**
  * Sử dụng file `.pem` để kết nối trực tiếp qua lệnh SSH Terminal.
  * Dùng lệnh `scp` để truyền file trực tiếp từ máy cá nhân lên EC2 một cách an toàn và bảo mật hơn mà không cần nới lỏng quyền thư mục quá mức.
### Bước 3: Cài đặt và Cấu hình Web Server Apache (`httpd`)
Sau khi kết nối thành công vào EC2 thông qua SSH hoặc PuTTY, tiến hành cập nhật hệ thống và cài đặt dịch vụ Apache bằng các lệnh sau:

# Chuyển sang quyền tối cao (Root)
sudo su

# Cập nhật các gói phần mềm hệ thống
yum update -y

# Cài đặt Web Server Apache
yum install httpd -y

### Bước 4: Triển khai Source Code & Khởi chạy Website

1. Di chuyển vào thư mục gốc của Web Server:
```bash
cd /var/www/html

```


2. *(Nếu dùng PuTTY/WinSCP)*: Thực hiện cấp quyền để chuẩn bị nhận file dữ liệu:
```bash
chmod 777 /var/www/html

```


3. Mở **WinSCP**, nhập **Public IP** của EC2, cấu hình Username và nạp file `.ppk` đã chuẩn bị ở Bước 2. Tiến hành kết nối và kéo thả toàn bộ file giao diện trang web (`index.html`, hình ảnh...) từ máy máy tính sang thư mục `/var/www/html`.
4. Quay trở lại cửa sổ Terminal, kích hoạt và khởi chạy dịch vụ Apache:
```bash
service httpd start

```



---

## 🎯 Kết quả

* Sử dụng **Public IP Address** của EC2 Instance dán vào trình duyệt web (ví dụ: `http://<your-ec2-public-ip>`).
<img width="690" height="732" alt="Screenshot 2026-07-13 130321" src="https://github.com/user-attachments/assets/17449ca5-f402-4590-bdce-f65acf71651b" />


---

## ⚠️ Bài học kinh nghiệm về Bảo mật (Security Takeaways)

* **Quyền hạn tối thiểu (Least Privilege):** Việc dùng lệnh `chmod 777` mở toang mọi quyền (Đọc/Ghi/Thực thi) cho tất cả mọi người là một rủi ro lớn trong thực tế.
* **Quản lý tường lửa:** Luôn giới hạn cổng SSH cho chính IP của mình để chặn đứng các cuộc tấn công dò quét mật khẩu từ các Bot trên Internet.

```
