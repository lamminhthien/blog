Dưới đây là hướng dẫn chi tiết để thiết lập một workflow trong GitHub Actions, thực hiện backup dữ liệu PostgreSQL và upload file backup lên S3 bucket của AWS:

---

### 1. **Cấu hình Secrets trong GitHub**
Truy cập **Settings** của repository, chọn **Secrets and variables > Actions** và thêm các secrets sau:

#### PostgreSQL Secrets:
- `PG_USERNAME`: Username PostgreSQL.
- `PG_PASSWORD`: Password PostgreSQL.
- `PG_HOST`: Hostname của PostgreSQL (e.g., `localhost`, `your-vps-ip`).
- `PG_DATABASE`: Tên database PostgreSQL.

#### AWS S3 Secrets:
- `AWS_ACCESS_KEY_ID`: Access Key ID từ AWS IAM.
- `AWS_SECRET_ACCESS_KEY`: Secret Access Key từ AWS IAM.
- `AWS_S3_BUCKET`: Tên bucket S3.
- `AWS_REGION`: Khu vực của bucket S3 (e.g., `us-east-1`).

---

### 2. **Workflow GitHub Actions**
Tạo file `.github/workflows/backup-postgres-to-s3.yml` với nội dung:

```yaml
name: Backup PostgreSQL and Upload to S3

on:
  schedule:
    - cron: '0 2 * * *' # Chạy hàng ngày lúc 2:00 AM
  workflow_dispatch: # Cho phép chạy thủ công

jobs:
  backup-and-upload:
    runs-on: ubuntu-latest

    steps:
    # 1. Checkout repository (nếu cần)
    - name: Checkout code
      uses: actions/checkout@v3

    # 2. Cài đặt AWS CLI và các công cụ cần thiết
    - name: Setup dependencies
      run: |
        sudo apt-get update
        sudo apt-get install -y postgresql-client gzip awscli

    # 3. Backup PostgreSQL
    - name: Backup PostgreSQL database
      env:
        PGHOST: ${{ secrets.PG_HOST }}
        PGUSER: ${{ secrets.PG_USERNAME }}
        PGPASSWORD: ${{ secrets.PG_PASSWORD }}
      run: |
        TIMESTAMP=$(date +"%Y%m%d%H%M%S")
        BACKUP_FILE="backup_${{ secrets.PG_DATABASE }}_$TIMESTAMP.sql.gz"
        pg_dump -h $PGHOST -U $PGUSER ${{ secrets.PG_DATABASE }} | gzip > $BACKUP_FILE
        echo "Backup created: $BACKUP_FILE"

    # 4. Upload file backup lên S3
    - name: Upload to S3
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        AWS_REGION: ${{ secrets.AWS_REGION }}
      run: |
        TIMESTAMP=$(date +"%Y%m%d%H%M%S")
        BACKUP_FILE="backup_${{ secrets.PG_DATABASE }}_$TIMESTAMP.sql.gz"
        aws s3 cp $BACKUP_FILE s3://${{ secrets.AWS_S3_BUCKET }}/backups/$BACKUP_FILE
```

---

### 3. **Giải thích Workflow**
1. **Trigger**:
   - Workflow được kích hoạt theo lịch định kỳ (cron) hoặc thủ công.
   - Lịch `0 2 * * *` chạy lúc 2:00 AM mỗi ngày.

2. **Cài đặt Dependencies**:
   - Cài đặt PostgreSQL Client (`pg_dump`) và gzip để nén file backup.

3. **Backup PostgreSQL**:
   - Sử dụng lệnh `pg_dump` để tạo file backup, được nén bằng `gzip`.
   - File backup được đặt tên theo định dạng `backup_<database>_<timestamp>.sql.gz`.

4. **Upload S3**:
   - Sử dụng AWS CLI để upload file backup vào S3 bucket.
   - File backup được lưu trong thư mục `/backups` của bucket.

---

### 4. **Cấu hình AWS IAM**
- Tạo IAM user với quyền truy cập S3 (policy `AmazonS3FullAccess` hoặc tùy chỉnh chỉ cho bucket cụ thể).
- Lấy **Access Key ID** và **Secret Access Key**, thêm vào GitHub Secrets.

---

### 5. **Kiểm tra và Giám sát**
- Kiểm tra file backup trong S3 bucket sau mỗi lần chạy.
- Sử dụng tab **Actions** trên GitHub để xem log của workflow khi chạy.

Workflow này giúp bạn tự động hóa quá trình backup PostgreSQL và upload an toàn lên AWS S3.