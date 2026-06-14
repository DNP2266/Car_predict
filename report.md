# Báo cáo: Dự đoán giá xe ô tô (Regression)

---

## 1. Giới thiệu bài toán

Bài toán **Regression** — dự đoán **giá xe ô tô** (`Price`) dựa trên thông tin kỹ thuật và đặc điểm xe (hãng, model, năm sản xuất, số km, loại nhiên liệu, v.v.). Mục tiêu là xây dựng mô hình ML ước lượng giá gần với giá thực tế trên tập test.

---

## 2. Mô tả dataset

- **Nguồn:** `data/df_car.csv` (19.237 dòng, 18 cột sau khi loại trùng lặp).
- **Biến mục tiêu:** `Price` (giá xe, USD).
- **Biến đặc trưng chính:** `Manufacturer`, `Model`, `Prod Year`, `Category`, `Leather interior`, `Fuel type`, `Engine volume`, `Mileage`, `Cylinders`, `Gear box type`, `Drive wheels`, `Doors`, `Wheel`, `Color`, `Airbags`, `Levy`.

---

## 3. Khám phá dữ liệu (EDA)

- Phân phối `Price` lệch phải; 
- `Levy` có giá trị `'-'` 
- `Mileage`, `Engine volume` cần làm sạch chuỗi (`km`, `Turbo`).
- Biển phân loại (`Fuel type`, `Gear box type`, …) có ảnh hưởng đến giá (boxplot).
- Ma trận tương quan: `Prod Year`, `Engine volume`, `Levy` tương quan dương với giá; `Mileage` tương quan âm.
- Hình minh họa: histogram, boxplot, heatmap trong notebook và `outputs/figures/`.

---

## 4. Tiền xử lý dữ liệu

| Bước | Xử lý |
|------|--------|
| Missing | `Levy` (`'-'` → mean), outlier `Price` → `New Price` |
| Trùng lặp | `drop_duplicates()` |
| Outlier | IQR trên `Price`, Loại bỏ |
| Encode | One-Hot Encoding (categorical) |
| Scale | StandardScaler (numerical) |
| Chia dữ liệu | 80% train / 20% test, `random_state=42` |
| Pipeline | Scaler/encoder **chỉ fit trên train** (tránh data leakage) |

---

## 5. Mô hình sử dụng

Huấn luyện **5 mô hình** trên cùng tập train:

1. **Linear Regression**
2. **Decision Tree Regressor** (`max_depth=12`, `random_state=42`)
3. **Random Forest Regressor** (`n_estimators=100`, `max_depth=15`, `random_state=42`)
4. **KNN Regressor** (`n_neighbors=7`)
5. **SVR** (`kernel='linear'`, `C=1.0`)

---

## 6. Kết quả đánh giá

| Mô hình | MAE | MSE | RMSE | R² |
|---------|-----|-----|------|-----|
| Random Forest | 4107.56 | 3.843751e+07 | 6199.80 | 0.6856 |
| KNN | 4684.10 | 4.973318e+07 | 7052.18 | 0.5932 |
| Decision Tree | 4792.89 | 5.365704e+07 | 7325.10 | 0.5611 |
| Linear Regression | 6313.00 | 82609644.73 | 8396.18 | 0.4234 |
| SVR | 7395.38 | 7.049588e+07 | 10004.24 | 0.1813 |

Biểu đồ: `outputs/figures/actual_vs_predicted_residual.png`, `model_comparison.png`, `feature_importance.png`.

---

## 7. So sánh mô hình

- **Mô hình tốt nhất:** Random Forest (RMSE thấp nhất ~6199, R² cao nhất ~0.69).
- **Chỉ số chọn:** RMSE (sai số cùng đơn vị với giá) và R² (mức giải thích phương sai).
- **Lý do:** Random Forest nắm bắt quan hệ phi tuyến và tương tác giữa nhiều biến phân loại/số; Linear Regression và SVR underfit.
- **Overfitting:** Decision Tree có nguy cơ overfit nếu depth lớn; Random Forest ổn định hơn nhờ ensemble.

---

## 8. Kết luận và hướng cải thiện

Random Forest cho kết quả tốt nhất trên tập test nội bộ, giải thích ~69% biến thiên giá xe. Có thể cải thiện bằng hyperparameter tuning (GridSearchCV), feature engineering (tuổi xe, gom nhóm hãng hiếm), thử XGBoost/LightGBM, và log-transform biến mục tiêu để giảm ảnh hưởng outlier.

---

