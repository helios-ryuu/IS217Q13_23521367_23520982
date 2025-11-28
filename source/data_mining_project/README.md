# 📊 DATA MINING PROJECT - US ACCIDENTS ANALYSIS
## Phân tích 5.5 triệu tai nạn giao thông Hoa Kỳ (2018-2023)

---

## 📁 Giới Thiệu Project

Project này áp dụng **6 kỹ thuật Data Mining** để phân tích dataset **US Accidents** với:
- **5,539,531 records** (tai nạn)
- **41 features** (thời tiết, thời gian, địa điểm, cơ sở hạ tầng)
- **Thời gian**: 2018-2023
- **Kích thước**: 1.22 GB (CSV đã tiền xử lý)

### 🎯 Mục Tiêu
Khám phá insights từ dữ liệu tai nạn để:
1. **Dự đoán** mức độ nghiêm trọng tai nạn
2. **Phân nhóm** các loại tai nạn theo pattern
3. **Dự báo** thời gian kéo dài tai nạn
4. **Tìm kiếm** mối liên hệ giữa các yếu tố
5. **Phát hiện** tai nạn bất thường
6. **Phân tích** xu hướng theo thời gian

---

## 💻 Yêu Cầu Hệ Thống

### Hardware
- **CPU**: Intel Core i5-12500H (hoặc tương đương, 6+ cores)
- **GPU**: NVIDIA RTX 3050Ti 4GB (optional, cho XGBoost acceleration)
- **RAM**: 16 GB+ (recommended 32GB cho full dataset)
- **Storage**: 5 GB free space (data + figures)

### Software
- **Python**: 3.14+ (hoặc 3.10+)
- **OS**: Windows 11 (hoặc Windows 10, Linux, macOS)
- **Jupyter**: Jupyter Notebook hoặc JupyterLab

---

## 🚀 Installation & Setup

### Bước 1: Clone/Download Project
```powershell
cd D:\Projects\IS217Q13_23521367_23520982\source\data_mining_project
```

### Bước 2: Tạo Virtual Environment
```powershell
# Tạo venv
python -m venv .venv

# Activate venv
.\.venv\Scripts\Activate.ps1  # PowerShell
```

### Bước 3: Install Dependencies
```powershell
pip install --upgrade pip
pip install -r requirements.txt
```

**Lưu ý**: Installation mất ~5-10 phút, download ~500MB packages.

### Bước 4: Verify Installation
```powershell
python -c "import pandas, numpy, xgboost, sklearn, seaborn; print('✅ All libraries installed!')"
```

### Bước 5: Launch Jupyter Notebook
```powershell
jupyter notebook
```

Browser sẽ tự động mở `http://localhost:8888`. Navigate đến folder project.

---

## 📚 Cấu Trúc Project

```
data_mining_project/
│
├── 01_classification_xgboost.ipynb      # Classification (XGBoost)
├── 02_clustering_hierarchical.ipynb     # Clustering (Hierarchical)
├── 03_regression_xgboost.ipynb          # Regression (XGBoost)
├── 04_association_fpgrowth.ipynb        # Association Rules (FP-Growth)
├── 05_anomaly_isolation_forest.ipynb    # Anomaly Detection (Isolation Forest)
├── 06_sequential_timeseries.ipynb       # Sequential Patterns (Time Series)
│
├── figures/                              # Output visualizations
│   ├── classification/                   # 4 figures (confusion matrix, ROC, etc.)
│   ├── clustering/                       # 8 figures (dendrogram, scatter, etc.)
│   ├── regression/                       # 4 figures (scatter, residual, etc.)
│   ├── association/                      # 4 figures (rules network, etc.)
│   ├── anomaly/                          # 4 figures (PCA scatter, etc.)
│   └── sequential/                       # 7 figures (time series, heatmaps, etc.)
│
├── requirements.txt                      # Python dependencies
├── README.md                             # Documentation này
└── .venv/                                # Virtual environment (gitignored)
```

### 📂 Dataset Path
Dataset nằm tại: `../ssis_project/preprocess/US_Accidents_March23-preprocessed.csv`

**Lưu ý**: Đảm bảo file CSV tồn tại trước khi chạy notebooks!

---

## 📖 6 Kỹ Thuật Data Mining & Cách Đọc Kết Quả

### 1️⃣ Classification (Phân Loại) - XGBoost
**File**: `01_classification_xgboost.ipynb`

#### Mục Tiêu
Dự đoán **SEVERITY** (1-4) dựa trên weather, time, infrastructure.

#### Mô Hình
- **XGBoost Classifier**: Gradient boosting với 100 trees
- **GPU Support**: Tự động detect RTX 3050Ti
- **Features**: 35 features (weather, time, infrastructure, location)

#### Cách Đọc Kết Quả

**📊 Confusion Matrix (Ma trận nhầm lẫn)**
- **Đường chéo** (diagonal) = Dự đoán **ĐÚNG**
- **Off-diagonal** = Dự đoán **SAI** (nhầm class này với class khác)
- **Ví dụ**: Cell [1,2] = Thực tế Severity 1, dự đoán là Severity 2

**📈 ROC Curves (Đường cong ROC)**
- **AUC** (Area Under Curve): 0.5 = random, 1.0 = perfect
- **AUC > 0.8** = Mô hình **TỐT**
- **AUC > 0.9** = Mô hình **XUẤT SẮC**

**🔝 Feature Importance (Tầm quan trọng)**
- **Giá trị cao** = Feature **QUAN TRỌNG** cho dự đoán
- Top features cho biết yếu tố nào ảnh hưởng severity nhiều nhất

**📋 Classification Report**
- **Precision**: Trong số dự đoán X, % nào đúng?
- **Recall**: Trong số thực tế X, tìm được % nào?
- **F1-Score**: Trung bình hài hòa (cao = tốt)

#### Insights Chính
- Yếu tố nào ảnh hưởng severity nhất?
- Thời điểm nào tai nạn nghiêm trọng hơn?
- Infrastructure nào nguy hiểm nhất?

---

### 2️⃣ Clustering (Phân Cụm) - Hierarchical
**File**: `02_clustering_hierarchical.ipynb`

#### Mục Tiêu
Gom nhóm tai nạn có đặc điểm **tương tự** (không cần biết trước nhãn).

#### Mô Hình
- **Hierarchical Clustering**: Ward linkage
- **Features**: LATITUDE, LONGITUDE, HOUR, TEMPERATURE, VISIBILITY
- **Optimal Clusters**: Xác định bằng dendrogram + silhouette score

#### Cách Đọc Kết Quả

**🌳 Dendrogram (Biểu đồ cây)**
- **Chiều cao** (height) = độ khác biệt giữa clusters
- **Cắt ở height X** → có Y clusters
- **Chiều cao thấp** = clusters rất khác nhau (tốt)

**📍 Geographic Scatter (Bản đồ phân cụm)**
- Mỗi **màu** = 1 cluster
- **Clusters riêng biệt** địa lý = phân cụm tốt

**📊 Silhouette Score**
- **Score > 0.5** = Clustering **TỐT**
- **Score > 0.7** = Clustering **XUẤT SẮC**
- **Score < 0.3** = Clustering **YẾU**

**🔥 Cluster Profiles (Đặc điểm cụm)**
- Heatmap cho thấy giá trị trung bình mỗi feature
- Màu **đỏ** = giá trị cao, **xanh** = giá trị thấp

#### Insights Chính
- Có mấy loại tai nạn chính?
- Mỗi loại có đặc điểm gì? (thời tiết, thời gian, địa điểm)
- Hotspots địa lý ở đâu?

---

### 3️⃣ Regression (Hồi Quy) - XGBoost
**File**: `03_regression_xgboost.ipynb`

#### Mục Tiêu
Dự đoán **DURATION** (thời gian kéo dài tai nạn, phút).

#### Mô Hình
- **XGBoost Regression**: Gradient boosting trees
- **GPU Support**: Tự động detect RTX 3050Ti (nhanh gấp 10-20x)
- **Sample**: 500K rows (XGBoost cực kỳ nhanh và scalable)
- **Features**: Weather + time + severity + infrastructure (19 features)
- **Hyperparameter tuning**: RandomizedSearchCV (30 iterations)

#### Cách Đọc Kết Quả

**📈 Scatter: Actual vs Predicted**
- **Gần đường y=x** = Dự đoán **CHÍNH XÁC**
- **Xa đường y=x** = Dự đoán **SAI**

**📊 Residual Plot (Phân bố sai số)**
- **Random scatter** quanh y=0 = Mô hình **TỐT**
- **Pattern** (cone shape, curve) = Mô hình **BIAS**
**🎯 Metrics**
- **R² (0-1)**: % variance explained
  - R² > 0.7 = **TỐT**
  - R² > 0.9 = **XUẤT SẮC**
- **MAE** (Mean Absolute Error): Sai số trung bình (thấp = tốt)
- **RMSE** (Root Mean Squared Error): Phạt sai số lớn (thấp = tốt)

**🌲 Feature Importance (XGBoost)**
- **Built-in importance**: Gain-based (nhanh và chính xác)
- **Gain Importance**: Đo độ cải thiện loss khi split
- Không cần feature scaling (trees are scale-invariant)
- GPU acceleration cho cả training và importance calculation

#### Insights Chính
- Duration phụ thuộc yếu tố nào nhiều nhất?
- Điều kiện nào làm tai nạn kéo dài?
- Dự đoán duration cho planning ứng phó
- Random Forest nhanh hơn và scalable hơn SVR
- Dự đoán duration cho planning ứng phó

---

### 4️⃣ Association Rules (Luật Kết Hợp) - FP-Growth
**File**: `04_association_fpgrowth.ipynb`

#### Mục Tiêu
Tìm mối liên hệ **"nếu A thì B"** giữa các yếu tố.

#### Mô Hình
- **FP-Growth Algorithm**: Nhanh hơn Apriori
- **Items**: Infrastructure + weather categories + time periods
- **Rules**: Support > 0.5%, Confidence > 40%, Lift > 1.5

#### Cách Đọc Kết Quả

**📋 Rules Table**
- **Antecedent → Consequent**: Nếu A thì B
- **Support**: % transactions có cả A và B (càng cao = pattern phổ biến)
- **Confidence**: P(B|A) = nếu có A, % nào có B (càng cao = rule mạnh)
- **Lift**: Độ tương quan
  - Lift = 1: A và B **KHÔNG** liên quan
  - Lift > 1.5: A và B **TƯƠNG QUAN MẠNH**
  - Lift < 1: A và B **ĐỐI LẬP**

**🔗 Network Graph**
- **Nodes** = Items
- **Edges** = Rules
- **Màu/độ dày** = Lift (đỏ/dày = lift cao)

**📊 Support-Confidence Scatter**
- **Góc trên phải** = Rules tốt nhất (support cao + confidence cao)
- **Màu** = Lift

#### Insights Chính
- Infrastructure nào thường xuất hiện cùng nhau?
- Weather + infrastructure → Severity?
- Patterns để dự đoán và phòng tránh

---

### 5️⃣ Anomaly Detection (Phát Hiện Bất Thường) - Isolation Forest
**File**: `05_anomaly_isolation_forest.ipynb`

#### Mục Tiêu
Phát hiện tai nạn **BẤT THƯỜNG** (outliers).

#### Mô Hình
- **Isolation Forest**: Tree-based anomaly detection
- **Contamination**: 0.5% (giả định 0.5% là outliers)
- **Features**: DURATION, DISTANCE, TEMPERATURE, WIND_SPEED, VISIBILITY, PRECIPITATION

#### Cách Đọc Kết Quả

**📊 Anomaly Score**
- **Score < -0.5**: **BẤT THƯỜNG** (anomaly)
- **Score ~ 0**: **BÌNH THƯỜNG** (normal)
- **Càng âm** = càng bất thường

**🔍 PCA Scatter**
- **Màu đỏ/tím**: Anomalies
- **Màu xanh/vàng**: Normal points
- **Scatter xa nhóm chính** = outliers

**📦 Boxplots**
- So sánh normal vs anomaly
- **Outliers** vượt ra ngoài whiskers

#### Insights Chính
- Tai nạn nào bất thường? (duration cực dài, weather cực đoan)
- Data quality issues (lỗi nhập liệu)
- Extreme events cần điều tra thêm

---

### 6️⃣ Sequential Patterns (Mẫu Tuần Tự) - Time Series
**File**: `06_sequential_timeseries.ipynb`

#### Mục Tiêu
Phát hiện **XU HƯỚNG** và **TÍNH MÙA VỤ** theo thời gian.

#### Mô Hình
- **Time Series Aggregation**: Daily/monthly accident counts
- **STL Decomposition**: Tách Trend + Seasonal + Residual
- **Heatmaps**: Hourly × Day, Monthly × Year

#### Cách Đọc Kết Quả

**📈 Time Series Line Plot**
- **Trend tăng** = Tai nạn ngày càng nhiều
- **Trend giảm** = Tai nạn giảm dần
- **Flat** = Ổn định

**🔄 STL Decomposition**
- **Trend**: Xu hướng dài hạn (5 năm)
- **Seasonal**: Pattern lặp lại (mùa hè/đông, cuối tuần)
- **Residual**: Nhiễu random

**🔥 Heatmap Hour × Day**
- **Màu đỏ** = Nhiều tai nạn
- **Màu xanh** = Ít tai nạn
- **Pattern**: Rush hours (7-9h, 16-18h) thường đỏ

**🗓️ Heatmap Month × Year**
- Seasonal peaks: Tháng nào nhiều tai nạn nhất?
- Yearly changes: Năm nào có xu hướng thay đổi?

**📊 Autocorrelation**
- **Lag 7**: Tương quan với 7 ngày trước (weekly pattern)
- **Lag 365**: Tương quan với 1 năm trước (yearly seasonality)

#### Insights Chính
- Xu hướng: Tăng/giảm qua các năm?
- Mùa vụ: Mùa hè/đông, cuối tuần có khác biệt?
- Giờ cao điểm: Khi nào cần tăng cường tuần tra?

---

## 🎨 Hướng Dẫn Đọc Figures

Tất cả biểu đồ được lưu trong folder `figures/` với cấu trúc:

```
figures/
├── classification/
│   ├── 00_severity_distribution.png      # Phân bố SEVERITY gốc
│   ├── 01_confusion_matrix.png           # Ma trận nhầm lẫn
│   ├── 02_roc_curves.png                 # ROC curves 4 classes
│   └── 03_feature_importance.png         # Top 20 features quan trọng
│
├── regression/
│   ├── 01_xgb_comprehensive_analysis.png  # 4 plots (scatter, residual, error, metrics)
│   ├── 02_xgb_feature_importance.png      # Top 15 features XGBoost
│   ├── 03_xgb_error_by_duration_range.png # MAE/RMSE theo duration bins
│   └── 04_xgb_learning_curve.png          # Learning curveểm clusters
│
├── regression/
│   ├── 01_analysis_comprehensive.png     # 4 plots (scatter, residual, error, metrics)
│   ├── 02_feature_importance.png         # Top 15 features SVR
│   ├── 03_error_by_duration.png          # MAE/RMSE theo duration bins
│   └── 04_learning_curve.png             # Learning curve
│
├── association/
│   ├── 01_rules_comparison.png           # Top 30 rules table
│   ├── 02_rules_network.png              # Network graph
│   ├── 03_support_confidence.png         # Scatter support vs confidence
│   └── 04_item_frequency.png             # Bar chart tần suất items
│
├── anomaly/
│   ├── 01_pca_scatter.png                # PCA 2D colored by anomaly score
│   ├── 02_anomaly_score_distribution.png # Histogram scores
│   ├── 03_feature_comparison.png         # Boxplots normal vs anomaly
│   └── 04_feature_importance.png         # Permutation importance
│
└── sequential/
    ├── 01_timeseries_daily.png           # Daily accidents 2018-2023
    ├── 02_stl_decomposition.png          # Trend + Seasonal + Residual
    ├── 03_heatmap_hour_day.png           # Giờ × Ngày trong tuần
    ├── 04_heatmap_month_year.png         # Tháng × Năm
    ├── 05_top10_states.png               # Top 10 states time series
    ├── 06_autocorrelation.png            # ACF plots
    └── 07_seasonal_analysis.png          # 4 mùa + weekend patterns
```

### 📊 Metrics Thresholds (Ngưỡng Đánh Giá)

| Metric | Xuất Sắc | Tốt | Trung Bình | Yếu |
|--------|----------|-----|------------|-----|
| **Accuracy** | >95% | 85-95% | 70-85% | <70% |
| **AUC** | >0.9 | 0.8-0.9 | 0.7-0.8 | <0.7 |
| **R²** | >0.9 | 0.7-0.9 | 0.5-0.7 | <0.5 |
| **Silhouette** | >0.7 | 0.5-0.7 | 0.3-0.5 | <0.3 |
| **Lift** | >3.0 | 1.5-3.0 | 1.0-1.5 | <1.0 |

---

## ⚙️ Chạy Notebooks

### Thứ Tự Chạy
Notebooks **HOÀN TOÀN ĐỘC LẬP**, có thể chạy theo thứ tự bất kỳ:

```powershell
# Mở Jupyter
jupyter notebook
```

# Trong browser, chọn notebook muốn chạy
# Ví dụ: 01_classification_xgboost.ipynb

# Chạy toàn bộ notebook:
| Notebook | GPU (RTX 3050Ti) | CPU (i5-12500H) | Dataset Size |
|----------|------------------|-----------------|--------------|
| 01_classification | ~10 phút | ~20 phút | Full 5.5M |
| 02_clustering | ~15 phút | ~25 phút | Full 5.5M |
| 03_regression | ~30 giây | ~2 phút | Sample 500K |
| 04_association | ~8 phút | ~10 phút | Sample 100K |
| 05_anomaly | ~10 phút | ~15 phút | Full 5.5M |
| 06_sequential | ~5 phút | ~5 phút | Aggregated |

**Tổng cộng**: ~45-75 phút chạy toàn bộ 6 notebooks.

**⚡ XGBoost (03_regression) với GPU nhanh nhất: 10-30 giây!**
| 03_regression | ~5 phút | ~8 phút | Sample 100K |
| 04_association | ~8 phút | ~10 phút | Sample 100K |
| 05_anomaly | ~10 phút | ~15 phút | Full 5.5M |
| 06_sequential | ~5 phút | ~5 phút | Aggregated |

**Tổng cộng**: ~50-80 phút chạy toàn bộ 6 notebooks.

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
3. **Regression**: Dự báo duration giúp lập kế hoạch (XGBoost - nhanh nhất, GPU support)
4. **Association Rules**: Tìm mối liên hệ giúp phòng tránh (FP-Growth)
5. **Anomaly Detection**: Phát hiện outliers giúp quality control (Isolation Forest)
6. **Sequential Patterns**: Xu hướng thời gian giúp dự báo (Time Series)
