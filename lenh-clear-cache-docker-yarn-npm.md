Dưới đây là hướng dẫn clear cache cho Docker, Yarn, NPM, và PNPM:

### 1. **Docker**
- **Xóa cache build image**:  
  ```bash
  docker builder prune
  ```
  (Thêm `-a` để xóa mọi thứ không sử dụng, bao gồm cả cache chưa gắn thẻ).
- **Xóa cache container, image, volume, network không sử dụng**:  
  ```bash
  docker system prune -a
  ```

---

### 2. **Yarn**
- **Xóa cache Yarn**:  
  ```bash
  yarn cache clean
  ```
  (Thêm `--all` để xóa toàn bộ cache).

---

### 3. **NPM**
- **Xóa cache NPM**:  
  ```bash
  npm cache clean --force
  ```
  (Lưu ý: `--force` bắt buộc vì NPM thường không cho xóa cache mặc định).

- **Kiểm tra lại cache đã được xóa**:  
  ```bash
  npm cache verify
  ```

---

### 4. **PNPM**
- **Xóa cache PNPM**:  
  ```bash
  pnpm store prune
  ```
  (Điều này sẽ xóa các package không còn sử dụng khỏi store).  
- Nếu cần xóa toàn bộ cache:
  ```bash
  pnpm store clear
  ```