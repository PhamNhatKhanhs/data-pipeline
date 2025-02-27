# Dự Án Data Pipeline

## 1. Tổng Quan
Dự án này xây dựng một data pipeline module hóa sử dụng dbt và Snowflake, được thiết kế để chuyển đổi dữ liệu thô thành những thông tin có ý nghĩa thông qua quy trình ETL được cấu trúc tốt.

## 2. Tính Năng Chính
✅ **Thiết Kế Module**: Phân tách rõ ràng giữa các tầng **staging**, **intermediate**, và **mart**.

✅ **Chất Lượng Dữ Liệu**: Kiểm tra và xác thực dữ liệu tự động.

✅ **Tài Liệu**: Tự động tạo tài liệu với **lineage graphs**.

✅ **Chuẩn Mực**: Tuân thủ các tiêu chuẩn ngành của **dbt** và **Snowflake**.

## 3. Yêu Cầu Hệ Thống
- Python 3.x
- dbt 1.9.x
- Tài khoản Snowflake
- Git

## 4. Bắt Đầu

### 4.1 Cài Đặt

1. Clone repository:
```bash
git clone https://github.com/PhamNhatKhanhs/data-pipeline.git
cd data-pipeline
```

2. Cài đặt dbt và các dependencies:
```bash
pip install dbt-snowflake
```

3. Cài đặt project dependencies:
```bash
dbt deps
```

### 4.2 Cấu Hình

1. Thiết lập kết nối Snowflake:
   - Tạo bản sao của file profiles mẫu:
   ```bash
   cp ~/.dbt/profiles.yml.example ~/.dbt/profiles.yml
   ```
   - Chỉnh sửa `~/.dbt/profiles.yml` với thông tin đăng nhập Snowflake của bạn:
   ```yaml
   data_pipeline:
     target: dev
     outputs:
       dev:
         type: snowflake
         account: your_account
         user: your_username
         password: your_password
         role: your_role
         database: your_database
         warehouse: your_warehouse
         schema: your_schema
         threads: 4
   ```

2. Kiểm tra kết nối:
```bash
dbt debug
```

### 4.3 Chạy Pipeline

1. Chạy các models:
```bash
dbt run
```

2. Chạy kiểm tra dữ liệu:
```bash
dbt test
```

3. Tạo và xem tài liệu:
```bash
dbt docs generate
dbt docs serve
```

## 5. Cấu Trúc Dự Án
```
data-pipeline/
├── models/
│   ├── staging/        # Làm sạch dữ liệu thô
│   ├── intermediate/   # Xử lý logic nghiệp vụ
│   └── marts/         # Tầng báo cáo
├── tests/             # Kiểm tra dữ liệu
├── macros/           # Các hàm SQL có thể tái sử dụng
└── dbt_project.yml   # Cấu hình dự án
```

## 6. Xử Lý Sự Cố

### Các Vấn Đề Thường Gặp

1. **Vấn Đề Kết Nối**
   - Kiểm tra thông tin đăng nhập Snowflake trong profiles.yml
   - Kiểm tra kết nối mạng
   - Đảm bảo quyền hạn role phù hợp

2. **Lỗi Model**
   - Kiểm tra logs trong target/run_results.json
   - Xác nhận dữ liệu nguồn có sẵn
   - Xem xét các dependencies của model

3. **Vấn Đề Tài Liệu**
   - Xóa cache trình duyệt
   - Kiểm tra cổng có sẵn (mặc định: 8080)

## 7. Đóng Góp
Chúng tôi hoan nghênh mọi đóng góp! Vui lòng mở issue hoặc gửi pull request để giúp cải thiện dự án.

## 8. Giấy Phép
Dự án này được phát hành dưới Giấy phép MIT.

---

🚀 **Liên Hệ**: Nếu có câu hỏi, vui lòng mở issue trên GitHub! Cảm ơn bạn đã quan tâm đến dự án! 😊

