# Hướng Dẫn Cài Đặt PostgreSQL Bằng Docker

Trong hướng dẫn này, chúng ta sẽ cài đặt PostgreSQL bằng Docker. Chúng ta sẽ sử dụng biến môi trường để đặt username và password, và map volume của PostgreSQL ra thư mục của host.

## 1. Cài Đặt Docker

Trước tiên, bạn cần cài đặt Docker trên máy tính của bạn. Bạn có thể tham khảo hướng dẫn cài đặt Docker ở [đây](./cai-dat-docker.md).

## 2. Tạo Thư Mục Lưu Trữ Dữ Liệu

Trước khi chạy PostgreSQL, chúng ta cần tạo một thư mục trên máy tính của bạn để lưu trữ dữ liệu. Bạn có thể đặt tên thư mục theo ý muốn, ví dụ: `postgres_data`.

## 3. Chạy PostgreSQL

Sử dụng lệnh sau để chạy PostgreSQL:

```bash
docker run -d --name postgres -e POSTGRES_USER=$POSTGRES_USER -e POSTGRES_PASSWORD=$POSTGRES_PASSWORD -v /path/to/your/postgres_data:/var/lib/postgresql/data -p 5432:5432 postgres
```

Trong đó, bạn cần thay thế `$POSTGRES_USER` và `$POSTGRES_PASSWORD` bằng username và password bạn muốn đặt. Và thay `/path/to/your/postgres_data` bằng đường dẫn tới thư mục bạn đã tạo ở bước 2.

## 4. Kiểm Tra PostgreSQL

Sau khi chạy lệnh trên, bạn có thể kiểm tra PostgreSQL bằng cách sử dụng một công cụ quản lý cơ sở dữ liệu như DBeaver hoặc pgAdmin. Bạn cũng có thể kiểm tra trạng thái của PostgreSQL bằng lệnh sau:

```bash
docker ps
```

Nếu bạn thấy container có tên `postgres` đang chạy, PostgreSQL đã được cài đặt thành công.

Chúc bạn cài đặt thành công PostgreSQL và sử dụng hiệu quả!


