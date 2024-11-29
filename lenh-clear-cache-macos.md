**Hướng dẫn Clear Cache trên MacOS Sequoia bằng Terminal**  

### 1. **Xóa Cache Hệ Thống**  
```bash
sudo rm -rf /Library/Caches/*
sudo rm -rf ~/Library/Caches/*
```

**Giải thích:**  
- `/Library/Caches/*`: Xóa các file cache của toàn hệ thống.  
- `~/Library/Caches/*`: Xóa cache của user hiện tại.  
**Lưu ý:** Lệnh yêu cầu quyền `sudo`. Hãy nhập mật khẩu khi được yêu cầu.

---

### 2. **Xóa DNS Cache**  
```bash
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
```

**Giải thích:**  
- Lệnh này làm mới cache DNS để khắc phục các vấn đề liên quan đến mạng.

---

### 3. **Xóa Cache Ứng Dụng Cụ Thể**  
Nếu muốn xóa cache của một ứng dụng cụ thể:  
```bash
rm -rf ~/Library/Caches/com.[tên ứng dụng]/*
```

**Ví dụ:**  
```bash
rm -rf ~/Library/Caches/com.apple.Safari/*
```

---

### 4. **Xóa Cache Trình Duyệt (Safari, Chrome)**  
#### Safari:  
```bash
rm -rf ~/Library/Safari/*
rm -rf ~/Library/Caches/com.apple.Safari/*
```

#### Chrome:  
```bash
rm -rf ~/Library/Application\ Support/Google/Chrome/Default/Cache/*
```

---

### 5. **Khởi Động Lại Hệ Thống (Tùy Chọn)**  
Để đảm bảo các thay đổi được áp dụng hoàn toàn:  
```bash
sudo shutdown -r now
```

---

**Lưu ý Quan Trọng:**  
- Hãy sao lưu dữ liệu trước khi thực hiện xóa cache để tránh mất thông tin quan trọng.  
- Chỉ xóa cache nếu gặp vấn đề, vì cache giúp tăng tốc độ hoạt động của hệ thống.  