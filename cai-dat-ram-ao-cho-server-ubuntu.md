**Hướng dẫn cài đặt RAM ảo (Swap) trên Ubuntu**

RAM ảo (Swap) là một phần không gian trên ổ cứng được sử dụng làm bộ nhớ tạm thời khi RAM vật lý bị đầy. Swap giúp hệ điều hành hoạt động mượt mà hơn, đặc biệt khi chạy các ứng dụng nặng.

---

### 1. Kiểm tra tình trạng Swap hiện tại
Chạy lệnh sau để kiểm tra xem hệ thống đã có Swap chưa:
```
sudo swapon --show
```
Nếu không có kết quả trả về, nghĩa là hệ thống chưa có Swap.

---

### 2. Kiểm tra dung lượng trống ổ đĩa
Xác minh dung lượng trống bằng lệnh:
```
df -h
```
Đảm bảo rằng ổ đĩa còn đủ dung lượng để tạo file Swap.

---

### 3. Tạo file Swap
Tạo file Swap với dung lượng mong muốn, ví dụ 2GB:
```
sudo fallocate -l 2G /swapfile
```
Nếu `fallocate` không khả dụng, dùng lệnh:
```
sudo dd if=/dev/zero of=/swapfile bs=1M count=2048
```

---

### 4. Cấp quyền truy cập cho file Swap
Thay đổi quyền để file Swap chỉ có thể được truy cập bởi root:
```
sudo chmod 600 /swapfile
```

---

### 5. Định dạng file Swap
Định dạng file để sử dụng làm Swap:
```
sudo mkswap /swapfile
```

---

### 6. Kích hoạt Swap
Kích hoạt file Swap vừa tạo:
```
sudo swapon /swapfile
```
Xác nhận Swap đã kích hoạt:
```
sudo swapon --show
```

---

### 7. Thiết lập kích hoạt Swap khi khởi động
Thêm file Swap vào file cấu hình `fstab` để tự động kích hoạt:
```
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

---

### 8. Tinh chỉnh hiệu suất Swap (Tùy chọn)
Điều chỉnh tham số `swappiness` để kiểm soát tần suất sử dụng Swap (giá trị khuyến nghị là 10-20 cho hệ thống desktop):
```
sudo sysctl vm.swappiness=10
```
Để thiết lập vĩnh viễn, chỉnh sửa file `/etc/sysctl.conf`:
```
sudo nano /etc/sysctl.conf
```
Thêm dòng:
```
vm.swappiness=10
```

---

### 9. Xóa Swap (Nếu cần)
Để tắt Swap:
```
sudo swapoff /swapfile
```
Xóa file Swap:
```
sudo rm /swapfile
```
Gỡ bỏ dòng trong `/etc/fstab` nếu đã thêm trước đó.

---

Bây giờ, hệ thống Ubuntu của bạn đã được cấu hình Swap thành công!