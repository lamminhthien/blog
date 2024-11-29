# Hướng Dẫn Cài Đặt Docker và Docker Compose Trên Ubuntu

Docker và Docker Compose là những công cụ quan trọng trong việc triển khai và quản lý ứng dụng container. Dưới đây là hướng dẫn chi tiết và dễ hiểu để bạn cài đặt Docker và Docker Compose trên Ubuntu.

---

## 1. Cài Đặt Docker

### 1.1. Cài đặt nhanh với một lệnh
Bạn có thể sử dụng lệnh duy nhất để cài đặt Docker từ script chính thức:

```bash
curl -fsSL https://get.docker.com/ | sudo sh
```

### 1.2. Kiểm tra Docker đã được cài đặt thành công
Sau khi cài đặt, kiểm tra phiên bản Docker:

```bash
docker --version
```

Nếu thấy thông báo phiên bản Docker (ví dụ: `Docker version 24.0.5, build xxx`), Docker đã được cài đặt thành công.

---

## 2. Cài Đặt Docker Compose

Docker Compose hiện được phân phối dưới dạng plugin của Docker. Để cài đặt, chạy lệnh sau:

```bash
sudo apt update
sudo apt install docker-compose-plugin -y
```

### 2.1. Kiểm tra Docker Compose
Sau khi cài đặt, kiểm tra phiên bản Docker Compose:

```bash
docker compose version
```

Nếu bạn thấy thông tin phiên bản, Docker Compose đã được cài đặt thành công.

---

## 3. Cấp Quyền Sử Dụng Docker Không Cần `sudo`

Để tiện lợi khi sử dụng Docker mà không cần gõ `sudo`, hãy thêm người dùng hiện tại vào nhóm `docker`:

```bash
sudo usermod -aG docker $USER
```

Sau đó, **đăng xuất và đăng nhập lại** để thay đổi có hiệu lực.

Kiểm tra bằng lệnh sau (không cần `sudo`):

```bash
docker run hello-world
```

Nếu thấy thông báo "Hello from Docker!", Docker đã hoạt động tốt.

---

## 4. Kiểm Tra Hoạt Động của Docker Compose

### 4.1. Tạo tệp cấu hình Docker Compose
Tạo tệp `docker-compose.yml` với nội dung sau:

```yaml
version: '3.8'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
```

Lưu tệp này vào một thư mục trên máy.

### 4.2. Chạy Docker Compose
Chạy lệnh sau để khởi động dịch vụ Nginx:

```bash
docker compose up
```

Mở trình duyệt và truy cập `http://localhost:8080`. Nếu bạn thấy trang mặc định của Nginx, Docker Compose đã hoạt động chính xác.

---

## 5. Cập Nhật Docker và Docker Compose

Để đảm bảo bạn luôn sử dụng phiên bản mới nhất:

```bash
sudo apt update
sudo apt upgrade -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

---

## 6. Gỡ Bỏ Docker và Docker Compose

Nếu bạn muốn gỡ bỏ hoàn toàn Docker và Docker Compose, chạy:

```bash
sudo apt remove -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo apt purge -y docker-ce docker-ce-cli containerd.io
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

---

## Lưu Ý Quan Trọng
- Cài đặt bằng script chính thức là cách nhanh nhất nhưng không cung cấp nhiều tùy chọn cài đặt chi tiết. Nếu cần kiểm soát chặt chẽ hơn, bạn có thể cài đặt Docker theo cách thủ công.
- Docker Compose hiện được tích hợp vào Docker CLI, vì vậy sử dụng lệnh `docker compose` thay vì `docker-compose`.

Chúc bạn cài đặt thành công và sử dụng hiệu quả Docker và Docker Compose!