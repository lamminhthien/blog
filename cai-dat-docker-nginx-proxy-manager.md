
# Hướng Dẫn Cài Đặt và Sử Dụng Nginx Proxy Manager  

## 1. **Cài Đặt Nginx Proxy Manager**

### Yêu cầu hệ thống:
- Server Linux (Ubuntu/Debian)
- Docker và Docker Compose đã cài đặt.

### Các bước thực hiện:
1. **Cài đặt Docker và Docker Compose**:  
   ```
   sudo apt update
   sudo apt install -y docker.io docker-compose
   ```

2. **Tạo thư mục để chứa cấu hình Nginx Proxy Manager**:
   ```
   mkdir -p /opt/nginx-proxy-manager/data
   cd /opt/nginx-proxy-manager
   ```

3. **Tạo tệp `docker-compose.yml`**:  
   Tạo tệp `docker-compose.yml` trong thư mục và thêm nội dung sau:
   ```yaml
   version: '3'
   services:
     app:
       image: 'jc21/nginx-proxy-manager:latest'
       restart: always
       ports:
         - '80:80'
         - '81:81'
         - '443:443'
       environment:
         DB_MYSQL_HOST: "db"
         DB_MYSQL_PORT: 3306
         DB_MYSQL_USER: "npm"
         DB_MYSQL_PASSWORD: "npm"
         DB_MYSQL_NAME: "npm"
       volumes:
         - ./data:/data
         - ./letsencrypt:/etc/letsencrypt

     db:
       image: 'mariadb:latest'
       restart: always
       environment:
         MYSQL_ROOT_PASSWORD: "root"
         MYSQL_DATABASE: "npm"
         MYSQL_USER: "npm"
         MYSQL_PASSWORD: "npm"
       volumes:
         - ./mysql:/var/lib/mysql
   ```

4. **Khởi động Nginx Proxy Manager**:
   ```
   docker-compose up -d
   ```
   Sau khi khởi động, truy cập giao diện quản lý tại: `http://<server-ip>:81`  
   Đăng nhập với tài khoản mặc định:
   - Email: `admin@example.com`
   - Password: `changeme`

5. **Thay đổi mật khẩu và cấu hình cơ bản**:  
   Đăng nhập và cập nhật mật khẩu để đảm bảo an toàn.

---

## 2. **Hướng Dẫn Sử Dụng Nginx Proxy Manager Với Tên Miền**

### Mục tiêu:
- Sử dụng domain `example.com` đã mua tại Namecheap.
- Trỏ subdomain `api.example.com` tới IP của server và cổng `3500`.

### Các bước thực hiện:
1. **Trỏ Tên Miền Từ Namecheap**:
   - Truy cập **Namecheap Dashboard**.
   - Chọn domain `example.com` và vào phần **Advanced DNS**.
   - Thêm bản ghi A trỏ domain/subdomain về IP server:
     - **Host**: `@` (hoặc `api` nếu trỏ subdomain)
     - **Type**: `A Record`
     - **Value**: IP của server
     - **TTL**: `Automatic`

2. **Cấu Hình Proxy Trong Nginx Proxy Manager**:
   - Đăng nhập giao diện Nginx Proxy Manager.
   - Chọn **Proxy Hosts** -> **Add Proxy Host**:
     - **Domain Names**: `api.example.com`
     - **Scheme**: `http`
     - **Forward Hostname / IP**: `IPServer` (ví dụ: 192.168.1.100)
     - **Forward Port**: `3500`
     - **Block Common Exploits**: Chọn
     - **Websockets Support**: Chọn
   - Tại tab **SSL**:
     - Chọn **Request a new SSL certificate**.
     - Tick chọn **Force SSL**.
     - Nhấn **Save**.

3. **Kiểm Tra Kết Quả**:
   - Mở trình duyệt và truy cập `https://api.example.com`.
   - Nếu cấu hình đúng, trang sẽ hiển thị nội dung API từ server cổng 3500.

---

### Ghi chú:
- Đảm bảo port 80 và 443 được mở trên firewall.
- Cập nhật IP server nếu IP thay đổi (với bản ghi A).
- Kiểm tra log của Nginx Proxy Manager nếu gặp lỗi.