# 🚀 Hướng Dẫn Sử Dụng preprocess.py

## 📋 Mô Tả
Script tiền xử lý dữ liệu tự động cho SQL Server Data Warehouse, hỗ trợ xử lý file CSV lớn theo khối (chunked processing). Hệ thống được tối ưu hóa để xử lý dataset lớn (hàng triệu bản ghi) một cách hiệu quả với báo cáo chi tiết về quá trình xử lý.

## 🔧 Cài Đặt Dependencies
```bash
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## 💻 Cách Sử Dụng

### ✅ Cách sử dụng cơ bản
```bash
python preprocess.py ../../../data/US_Accidents_March23.csv
```

### ⚙️ Các tùy chọn nâng cao

#### 📁 Chỉ định file đầu ra
```bash
python preprocess.py input.csv -o output_processed.csv
```

#### 🔢 Tùy chỉnh kích thước khối xử lý
```bash
python preprocess.py data.csv -c 100000
# Khối 1 triệu dòng (thay vì 2.6 triệu mặc định)
```

#### 📅 Thay đổi ngày cắt lọc
```bash
python preprocess.py data.csv -d 2020-01-01
# Chỉ giữ dữ liệu từ 2020 trở lên (mặc định: 2018-01-01)
```

#### 🗑️ Tùy chỉnh cột cần xóa
```bash
python preprocess.py data.csv --delete-columns "ID,Country,Description,Custom_Column"
```

#### 📊 Chế độ verbose (chi tiết)
```bash
python preprocess.py data.csv -v
```

### 🔗 Kết hợp nhiều tùy chọn
```bash
python preprocess.py large_data.csv \
  -o processed_data.csv \
  -c 500000 \
  -d 2019-01-01 \
  --delete-columns "ID,Country,Weather_Timestamp" \
  -v
```

## 📈 Kết Quả & Báo Cáo

### 📄 File đầu ra
- **File CSV tối ưu hóa**: Dataset đã được xử lý, sẵn sàng cho SQL Server
- **Báo cáo chi tiết** (`.txt`): Phân tích đầy đủ quá trình xử lý

## ❓ Trợ Giúp & Troubleshooting

### 💡 Xem trợ giúp
```bash
python preprocess.py --help
```

### 🔍 Kiểm tra phiên bản
```bash
python preprocess.py --version
```

### 🐛 Các lỗi thường gặp

#### ❌ "File không tồn tại"
```bash
# Đảm bảo đường dẫn file chính xác
python preprocess.py "path/to/your/file.csv"
```

#### ❌ "Memory Error"
```bash
# Giảm kích thước chunk
python preprocess.py data.csv -c 1000000
```

#### ❌ "Date parsing error"
```bash
# Kiểm tra format ngày (YYYY-MM-DD)
python preprocess.py data.csv -d "2020-01-01"
```

### 📋 Requirements
- **Python**: 3.7+
- **RAM**: Tối thiểu 4GB (khuyến nghị 8GB+)
- **Disk Space**: 2-3x kích thước file gốc
- **Dependencies**: Xem `requirements.txt`

