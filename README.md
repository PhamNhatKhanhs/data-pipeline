# DỰ Án Data Pipeline

Một pipeline chuyển đổi dữ liệu hiện đại được xây dựng với dbt và Snowflake, áp dụng các thực hành tốt nhất trong mô hình dữ liệu và kỹ thuật dữ liệu.

---

## 1. **Tổng Quan Dự Án**

Dự án này triển khai một **pipeline xử lý dữ liệu đơn hàng** sử dụng **modern data stack**. Nó tuân theo mô hình **Medallion Architecture**, chuyển đổi dữ liệu thô qua nhiều lớp để tạo ra các mô hình dữ liệu sẵn sàng phục vụ phân tích.

### **1.1 Kiến Trúc Pipeline**
```
Lớp Nguồn (Raw Data)
    │
    ├── tpch.orders
    │
Lớp Staging
    │
    ├── stg_tpch_orders
    │
Lớp Trung Gian
    │
    ├── int_order_items
    ├── int_order_items_summary
    │
Lớp Mart (Dữ liệu phục vụ báo cáo)
    │
    ├── fct_orders
    ├── fct_orders_discount
    └── fct_orders_date_valid
```

---

## 2. **Tính Năng Chính**
✅ **Thiết kế module hoá**: Tách biệt rõ ràng giữa các tầng **staging**, **intermediate** và **mart**.

✅ **Đảm bảo chất lượng dữ liệu**: Tích hợp kiểm tra dữ liệu tự động.

✅ **Tài liệu hoá đầy đủ**: Sinh tài liệu tự động kèm theo **biểu đồ lineage**.

✅ **Áp dụng best practices**: Sử dụng **dbt** và **Snowflake** theo tiêu chuẩn tốt nhất.

---

## 3. **Công Nghệ Sử Dụng**
- **dbt**: Chuyển đổi dữ liệu và mô hình hoá dữ liệu.
- **Snowflake**: Kho dữ liệu đám mây.
- **SQL**: Viết logic xử lý dữ liệu.
- **Airflow**: Quản lý DAG để tự động hoá pipeline (nếu cần).
- **Git**: Quản lý version control.

---

## 4. **Bắt Đầu Sử Dụng**

### **4.1 Yêu Cầu Trước Khi Cài Đặt**
- Python 3.x
- dbt 1.9.x
- Tài khoản Snowflake

### **4.2 Cài Đặt**
```bash
# Clone repository
git clone https://github.com/PhamNhatKhanhs/data-pipeline.git
cd data-pipeline

# Cài đặt dbt và dependencies
pip install dbt-snowflake
```

### **4.3 Cấu Hình**
- **Cấu hình Snowflake** trong file `profiles.yml`.
- Cập nhật file `dbt_project.yml` nếu cần.

### **4.4 Chạy Dự Án**
```bash
# Cài đặt dependencies cho dbt
dbt deps

# Chạy các mô hình
dbt run

# Kiểm tra chất lượng dữ liệu
dbt test

# Sinh tài liệu và xem lineage graph
dbt docs generate
dbt docs serve
```

---

## 5. **Cấu Trúc Thư Mục Dự Án**
```
data-pipeline/
├── models/
│   ├── staging/        # Làm sạch dữ liệu gốc
│   ├── intermediate/   # Xử lý logic nghiệp vụ
│   ├── marts/         # Lớp dữ liệu phục vụ báo cáo
├── tests/              # Kiểm thử dữ liệu
├── macros/             # Chứa các hàm SQL có thể tái sử dụng
└── dbt_project.yml     # Cấu hình dự án dbt
```

---

## 6. **Chi Tiết Các Mô Hình Dữ Liệu**
### **6.1 Lớp Staging** (Chuẩn hoá dữ liệu gốc)
- `stg_tpch_orders`: Làm sạch và chuẩn hoá dữ liệu đơn hàng.

### **6.2 Lớp Trung Gian** (Xử lý logic nghiệp vụ)
- `int_order_items`: Tách chi tiết từng dòng đơn hàng.
- `int_order_items_summary`: Tổng hợp các chỉ số đơn hàng.

### **6.3 Lớp Mart (Báo cáo & phân tích)**
- `fct_orders`: Bảng fact chính về đơn hàng.
- `fct_orders_discount`: Thống kê chi tiết về giảm giá.
- `fct_orders_date_valid`: Kiểm tra tính hợp lệ của dữ liệu theo ngày.

---

## 7. **Cách Đóng Góp**
Chúng tôi hoan nghênh mọi đóng góp từ cộng đồng! Hãy mở issue hoặc gửi pull request nếu bạn muốn cải thiện dự án.

### **Cách gửi PR**
```bash
# Tạo nhánh mới
git checkout -b feature/new-feature

# Commit thay đổi
git add .
git commit -m "Thêm tính năng XYZ"

git push origin feature/new-feature

# Tạo pull request trên GitHub
```

---

## 8. **Giấy Phép**
Dự án này được phát hành theo giấy phép MIT.

---

🚀 **Liên hệ**: Nếu có câu hỏi, hãy mở issue hoặc liên hệ qua GitHub! Cảm ơn bạn đã quan tâm đến dự án này! 😊

