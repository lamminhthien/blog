### **Hướng dẫn sử dụng các lệnh Docker liên quan đến container và log**

#### **1. `docker ps -a`: Hiển thị tất cả container**
Lệnh `docker ps -a` được sử dụng để liệt kê tất cả container trên hệ thống, bao gồm cả container đang chạy, đã dừng hoặc đã thoát.

**Cách sử dụng:**
```bash
docker ps -a
```

**Ý nghĩa các cột hiển thị:**
- `CONTAINER ID`: ID của container.
- `IMAGE`: Image được sử dụng để tạo container.
- `COMMAND`: Lệnh được chạy trong container.
- `CREATED`: Thời gian container được tạo.
- `STATUS`: Trạng thái hiện tại (đang chạy, đã thoát, bị dừng).
- `PORTS`: Các port được ánh xạ.
- `NAMES`: Tên của container.

#### **2. Ghi log lỗi trong Docker container**
Docker container ghi lại log của các ứng dụng hoặc tiến trình chạy bên trong. Để kiểm tra log lỗi, sử dụng lệnh `docker logs`.

**Cách sử dụng:**
```bash
docker logs [OPTIONS] CONTAINER_NAME_OR_ID
```

**Các tùy chọn phổ biến:**
- `--tail n`: Chỉ hiển thị n dòng log cuối cùng.
- `--follow` hoặc `-f`: Theo dõi log theo thời gian thực.
- `--since`: Hiển thị log bắt đầu từ thời điểm cụ thể (VD: `--since 1h` để xem log trong 1 giờ qua).

**Ví dụ:**
```bash
docker logs --tail 100 --follow my_container
```
Hiển thị 100 dòng log cuối và theo dõi log theo thời gian thực.

#### **3. Ghi log và lọc log trong Docker container**
Docker không hỗ trợ lọc log trực tiếp qua lệnh `docker logs`, nhưng bạn có thể kết hợp với các công cụ dòng lệnh như `grep` hoặc `jq`.

**Cách lọc log:**
- Lọc theo từ khóa:
```bash
docker logs CONTAINER_NAME_OR_ID | grep "ERROR"
```

- Lọc log JSON:
Nếu container ghi log theo định dạng JSON, sử dụng `jq` để lọc:
```bash
docker logs CONTAINER_NAME_OR_ID | jq '. | select(.level == "error")'
```

#### **4. Lưu log vào file**
Nếu cần lưu log để phân tích sau:
```bash
docker logs CONTAINER_NAME_OR_ID > logs.txt
```

#### **5. Kiểm tra log của container đã thoát**
Để kiểm tra log của container đã dừng:
```bash
docker ps -a
docker logs CONTAINER_NAME_OR_ID
```

#### **6. Xoá log cũ của container**
Docker không có lệnh xóa log trực tiếp. Để xóa log:
- Dừng container:
```bash
docker stop CONTAINER_NAME_OR_ID
```

- Xóa file log (đường dẫn log có thể thay đổi tùy hệ điều hành):
```bash
sudo truncate -s 0 /var/lib/docker/containers/CONTAINER_ID/CONTAINER_ID-json.log
```

- Khởi động lại container:
```bash
docker start CONTAINER_NAME_OR_ID
```