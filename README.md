# Dự Án Data Pipeline

## 1. Giới Thiệu
Dự án này xây dựng một data pipeline module hóa sử dụng dbt và Snowflake, giúp chuyển đổi dữ liệu thô thành thông tin có giá trị thông qua quy trình ETL được tổ chức một cách khoa học.

## 2. Công Nghệ và Công Cụ

### 2.1. Core Technologies
- **dbt (data build tool) v1.9.0**:
  - Framework chính cho data transformation
  - Quản lý dependencies và lineage
  - Tích hợp testing và documentation

- **Snowflake**:
  - Data warehouse platform
  - Xử lý song song và scale linh hoạt
  - Hỗ trợ semi-structured data

### 2.2. Development Tools
- **Python 3.8+**:
  - Runtime environment cho dbt
  - Scripting và automation

- **Git**:
  - Version control
  - Collaboration và code review

### 2.3. Testing & Quality Tools
- **dbt Test Framework**:
  - Generic tests cho schema
  - Custom SQL tests
  - Data quality validation

- **CI/CD Tools**:
  - GitHub Actions cho automation
  - Continuous testing và deployment

### 2.4. Documentation Tools
- **dbt Docs**:
  - Auto-generated documentation
  - Data lineage visualization
  - Schema và model documentation

### 2.5. Dependencies
```txt
dbt-core>=1.9.0
dbt-snowflake>=1.9.0
snowflake-connector-python>=3.0.0
pytest>=7.0.0
```

## 3. Tính Năng Chính
✅ **Thiết Kế Module**: Phân chia rõ ràng thành 3 tầng:
   - **Staging**: Tầng làm sạch dữ liệu thô
   - **Intermediate**: Tầng xử lý logic nghiệp vụ
   - **Mart**: Tầng tạo báo cáo cuối cùng

✅ **Kiểm Soát Chất Lượng**: Tự động kiểm tra và xác thực dữ liệu

✅ **Tài Liệu**: Tự động tạo tài liệu kèm sơ đồ quan hệ (lineage graphs)

✅ **Chuẩn Hóa**: Tuân thủ chuẩn mực của dbt và Snowflake

## 3. Kiến Trúc Dự Án

### 3.1. Sơ Đồ Kiến Trúc
```
[Dữ Liệu Thô] → [Staging] → [Intermediate] → [Mart] → [Báo Cáo]
     ↓             ↓            ↓             ↓          ↓
  [Kiểm Tra     [Làm Sạch]   [Xử Lý]    [Tổng Hợp]   [Trực Quan]
   Dữ Liệu]                   [Logic]     [Dữ Liệu]    [Hóa]
```

### 3.2. Quy Trình Xử Lý Dữ Liệu
1. **Staging Layer**:
   - Chuẩn hóa tên trường
   - Chuyển đổi kiểu dữ liệu
   - Xử lý giá trị null và rỗng
   - Loại bỏ dữ liệu trùng lặp

2. **Intermediate Layer**:
   - Áp dụng quy tắc nghiệp vụ
   - Tính toán các trường phái sinh
   - Kết hợp dữ liệu từ nhiều nguồn
   - Xử lý các trường hợp ngoại lệ

3. **Mart Layer**:
   - Tạo các bảng fact và dimension
   - Tối ưu hóa cho truy vấn
   - Tạo các chỉ số tổng hợp
   - Chuẩn bị dữ liệu cho báo cáo

## 4. Chuẩn Bị Môi Trường

### 4.1. Yêu Cầu Cơ Bản
- **Python**: Phiên bản 3.x (Khuyến nghị Python 3.8 trở lên)
  - Kiểm tra phiên bản: `python --version`
  - Tải Python tại: https://www.python.org/downloads/

- **Git**: 
  - Kiểm tra cài đặt: `git --version`
  - Tải Git tại: https://git-scm.com/downloads

- **Tài khoản Snowflake**:
  - Đăng ký tại: https://signup.snowflake.com/
  - Cần có thông tin: account, username, password, role, warehouse

### 4.2. Cài Đặt pip và virtualenv
1. Kiểm tra pip:
```bash
python -m pip --version
```

2. Nếu chưa có pip, cài đặt:
```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
```

3. Cài đặt virtualenv:
```bash
pip install virtualenv
```

## 5. Cài Đặt Dự Án

### 5.1. Clone và Chuẩn Bị Môi Trường
1. Clone dự án:
```bash
git clone https://github.com/PhamNhatKhanhs/data-pipeline.git
cd data-pipeline
```

2. Tạo môi trường ảo:
```bash
python -m venv dbt-env
```

3. Kích hoạt môi trường ảo:
- Windows:
```bash
dbt-env\Scripts\activate
```
- Linux/Mac:
```bash
source dbt-env/bin/activate
```

### 5.2. Cài Đặt Dependencies
1. Cài đặt dbt-snowflake:
```bash
pip install dbt-snowflake==1.9.0
```

2. Cài đặt các gói phụ thuộc của dự án:
```bash
dbt deps
```

### 5.3. Cấu Hình Kết Nối Snowflake
1. Tạo thư mục .dbt trong thư mục home:
- Windows: `mkdir %USERPROFILE%\.dbt`
- Linux/Mac: `mkdir ~/.dbt`

2. Tạo file profiles.yml:
- Windows: Tạo file `%USERPROFILE%\.dbt\profiles.yml`
- Linux/Mac: Tạo file `~/.dbt/profiles.yml`

3. Thêm cấu hình sau vào profiles.yml:
```yaml
data_pipeline:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: your_account    # VD: xy12345.ap-southeast-1
      user: your_username
      password: your_password
      role: your_role          # VD: ACCOUNTADMIN
      database: your_database  # VD: DBT_DEMO
      warehouse: your_warehouse # VD: COMPUTE_WH
      schema: your_schema     # VD: DBT_SCHEMA
      threads: 4
```

4. Kiểm tra kết nối:
```bash
dbt debug
```

## 6. Phát Triển và Kiểm Thử

### 6.1. Quy Trình Phát Triển
1. **Tạo nhánh phát triển**:
```bash
git checkout -b feature/ten-tinh-nang
```

2. **Viết mã và kiểm thử**:
- Tuân thủ chuẩn mực đặt tên
- Thêm docstring và comments
- Viết unit tests

3. **Kiểm tra chất lượng**:
```bash
dbt test                 # Kiểm tra tính toàn vẹn dữ liệu
dbt run                  # Chạy các models
dbt docs generate        # Tạo tài liệu
```

### 6.2. Kiểm Soát Chất Lượng
1. **Kiểm tra dữ liệu**:
- Tính đầy đủ của dữ liệu
- Tính chính xác của phép tính
- Tính nhất quán của quan hệ

2. **Tối ưu hiệu suất**:
- Sử dụng incremental models
- Tối ưu hóa joins
- Phân vùng dữ liệu

## 7. Cấu Trúc Thư Mục
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

## 8. Xử Lý Lỗi Thường Gặp

### 8.1. Lỗi Cài Đặt
1. **Lỗi "command not found: dbt"**:
   - Đảm bảo đã kích hoạt môi trường ảo
   - Thử cài đặt lại: `pip install dbt-snowflake==1.9.0`
   - Kiểm tra biến môi trường PATH

2. **Lỗi kết nối Snowflake**:
   - Kiểm tra định dạng account
   - Xác nhận thông tin đăng nhập
   - Đảm bảo role có đủ quyền
   - Kiểm tra tường lửa và VPN

### 8.2. Lỗi Thực Thi
1. **Lỗi Compilation**:
   - Kiểm tra cú pháp SQL
   - Xác nhận tên bảng và trường
   - Kiểm tra dependencies

2. **Lỗi Runtime**:
   - Xem logs trong thư mục `target/`
   - Kiểm tra quyền truy cập
   - Xác nhận warehouse có đủ tài nguyên

## 9. Bảo Trì và Cập Nhật

### 9.1. Quy Trình Cập Nhật
1. **Backup dữ liệu**:
   - Tạo snapshot trước khi cập nhật
   - Lưu trữ cấu hình hiện tại

2. **Cập nhật phiên bản**:
   - Kiểm tra compatibility matrix
   - Cập nhật theo thứ tự dependencies
   - Chạy tests sau mỗi bước

### 9.2. Giám Sát và Bảo Trì
1. **Giám sát hiệu suất**:
   - Theo dõi thời gian chạy
   - Kiểm tra sử dụng tài nguyên
   - Đánh giá chất lượng dữ liệu

2. **Bảo trì định kỳ**:
   - Dọn dẹp dữ liệu tạm
   - Tối ưu hóa indexes
   - Cập nhật tài liệu

## 10. Đóng Góp và Hỗ Trợ

### 10.1. Hướng Dẫn Đóng Góp
1. Fork dự án
2. Tạo nhánh tính năng
3. Commit thay đổi
4. Tạo Pull Request

### 10.2. Hỗ Trợ
- Tạo issue trên GitHub
- Tham khảo tài liệu dbt: https://docs.getdbt.com/
- Tham gia cộng đồng Slack dbt

---

📝 **Ghi chú**: Tài liệu này được cập nhật lần cuối: 20255

🎉 **Chúc mừng!** Bạn đã hoàn thành việc cài đặt. Nếu cần hỗ trợ thêm, hãy tạo issue trên GitHub nhé! 😊

