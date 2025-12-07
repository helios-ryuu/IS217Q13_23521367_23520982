# DATA MINING PROJECT - US ACCIDENTS

Phân tích 5.5M tai nạn giao thông Hoa Kỳ (2018-2023). Chỉ giữ các notebook tạo ra figures hiện còn: classification, clustering, sequential.

## Nhanh: Thiết lập & Chạy
```powershell
cd D:\Projects\IS217Q13_23521367_23520982\source\data_mining_project
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
jupyter notebook
```

Dataset: `../ssis_project/preprocess/US_Accidents_March23-preprocessed.csv` (đảm bảo file tồn tại).

## Notebook còn lại
- `01_classification_xgboost.ipynb` – phân loại severity.
- `02_clustering_hierarchical.ipynb` – phân cụm tai nạn.
- `03_sequential_timeseries.ipynb` – mẫu thời gian.

## Figures hiện có
- `figures/classification/` (ma trận nhầm lẫn, báo cáo).
- `figures/clustering/` (profile cần thiết).
- `figures/sequential/` (các hình còn giữ lại sau khi lược bỏ).
- Các thư mục `regression/`, `association/`, `anomaly/` đã gỡ cùng với notebook tương ứng.

## Lưu ý
- Các section ROC/feature-importance hoặc biểu đồ không cần thiết đã được lược bỏ trong notebooks còn lại.
- Nếu chạy lại notebooks, thư mục `figures/` sẽ được tạo khi cần.

---

## 📊 Dataset Information

### 📂 Source
- **File**: `US_Accidents_March23-preprocessed.csv`
- **Location**: `../ssis_project/preprocess/`
- **Size**: 1.22 GB
- **Rows**: 5,539,531
- **Columns**: 41

### 📋 Column Descriptions

| Column | Type | Description | Example Values |
|--------|------|-------------|----------------|
| **SEVERITY** | int8 | Mức độ nghiêm trọng | 1, 2, 3, 4 |
| **DISTANCE** | float32 | Khoảng cách ảnh hưởng (mi) | 0.0 - 441.75 |
| **DURATION** | int32 | Thời gian kéo dài (seconds) | 73 - 134M |
| **DATE** | datetime | Ngày tai nạn | 2018-01-01 |
| **YEAR** | int16 | Năm | 2018 - 2023 |
| **QUARTER** | int8 | Quý | 1, 2, 3, 4 |
| **MONTH** | int8 | Tháng | 1 - 12 |
| **DAY** | int8 | Ngày trong tháng | 1 - 31 |
| **HOUR** | int8 | Giờ trong ngày | 0 - 23 |
| **IS_WEEKEND** | bool | Cuối tuần | True/False |
| **STATE** | category | Bang | CA, TX, FL... |
| **COUNTY** | str | Quận | Los Angeles... |
| **CITY** | str | Thành phố | Miami, Houston... |
| **LATITUDE** | float32 | Vĩ độ | 24.55 - 49.00 |
| **LONGITUDE** | float32 | Kinh độ | -124.55 - -67.40 |
| **TEMPERATURE** | float32 | Nhiệt độ (°F) | -89 - 207 |
| **WIND_CHILL** | float32 | Wind chill (°F) | -89 - 207 |
| **HUMIDITY** | float32 | Độ ẩm (%) | 1 - 100 |
| **PRESSURE** | float32 | Áp suất (in) | 0 - 58.63 |
| **VISIBILITY** | float32 | Tầm nhìn (mi) | 0 - 140 |
| **WIND_SPEED** | float32 | Tốc độ gió (mph) | 0 - 984 |
| **PRECIPITATION** | float32 | Lượng mưa (in) | 0 - 36.47 |
| **WIND_DIRECTION** | category | Hướng gió | N, S, E, W, CALM... |
| **WEATHER_CONDITION** | category | Điều kiện thời tiết | Fair, Rain, Snow... |
| **SUNRISE_SUNSET** | category | Ban ngày/đêm | Day, Night |
| **AMENITY** | bool | Tiện ích gần đó | True/False |
| **BUMP** | bool | Gờ giảm tốc | True/False |
| **CROSSING** | bool | Vạch băng qua | True/False |
| **GIVE_WAY** | bool | Biển nhường đường | True/False |
| **JUNCTION** | bool | Giao lộ | True/False |
| **NO_EXIT** | bool | Đường cụt | True/False |
| **RAILWAY** | bool | Đường sắt | True/False |
| **ROUNDABOUT** | bool | Vòng xoay | True/False |
| **STATION** | bool | Trạm (gas, police) | True/False |
| **STOP** | bool | Biển dừng | True/False |
| **TRAFFIC_CALMING** | bool | Giảm tốc độ | True/False |
| **TRAFFIC_SIGNAL** | bool | Đèn giao thông | True/False |
| **TURNING_LOOP** | bool | Vòng quay đầu | True/False |

### 📊 Data Quality
- **Missing Values**: 7 columns có missing (1-21%)
- **Handled**: Fill với median (numeric) hoặc mode (categorical)
- **Quality Score**: 98.7/100 (từ preprocessing report)

### 💡 Key Takeaways

1. **Classification**: Dự đoán severity giúp ưu tiên ứng phó (XGBoost)
2. **Clustering**: Phân nhóm tai nạn giúp hiểu patterns (Hierarchical)
3. **Sequential Patterns**: Xu hướng thời gian giúp dự báo (Time Series)

## Notebook Details (static info moved khỏi notebooks)

### Classification (XGBoost)
- **Mục tiêu**: Dự đoán mức độ nghiêm trọng (SEVERITY 1-4) dựa trên thời gian, thời tiết, hạ tầng, vị trí.
- **Tiền xử lý chính**: Optimize dtype (int8/int16/float32/category), fill missing (median/mode), drop high-cardinality địa chỉ (CITY/COUNTY/STREET/ZIPCODE), encode categorical, convert bool → int8, stratified train/test 80/20.
- **Mô hình**: `XGBClassifier` (`multi:softprob`, depth=6, lr=0.1, subsample/colsample=0.8, 100 trees, CPU mặc định; đặt `USE_GPU=True` trong notebook nếu môi trường hỗ trợ).
- **Đầu ra giữ lại**: Confusion matrix (`figures/classification/01_confusion_matrix.png`), classification report (`figures/classification/classification_report.txt`), top feature importances (in-notebook print), severity distribution plot (`figures/classification/00_severity_distribution.png`).
- **Cách chạy gọn**: Mở notebook, chạy tuần tự; notebook chỉ in các số liệu động (shape, missing tổng, kích thước train/test, accuracy, top features).

### Sequential Time Series
- **Mục tiêu**: Khảo sát xu hướng tai nạn theo thời gian (2018-2023), mẫu theo giờ/ngày/tháng/mùa.
- **Pipeline**: Load subset cột thời gian/thời tiết, thêm DAY_OF_WEEK/WEEK_OF_YEAR, lọc 2018-2023, tổng hợp daily series (fill missing dates), heatmap giờ×ngày trong tuần, heatmap tháng×năm, phân tích mùa/tuần.
- **Chỉ số chính (động)**: Trend 2018→2023 (% thay đổi), mùa cao nhất, giờ bận nhất, so sánh weekday vs weekend (in-notebook print gọn).
- **Hình giữ lại**: `figures/sequential/03_heatmap_hour_dayofweek.png`, `figures/sequential/04_heatmap_month_year.png`, `figures/sequential/07_seasonal_weekly_patterns.png`.
- **Khuyến nghị (tĩnh)**: Tăng giám sát giờ 16-18h và mùa đông; chú ý thứ 6; người lái xe cẩn trọng giờ cao điểm và thời tiết xấu; nghiên cứu tiếp tục với mô hình dự báo (ARIMA/Prophet) và dữ liệu lưu lượng.
