# Car Price Prediction (Regression)

Dự án Machine Learning — dự đoán giá xe ô tô (biến mục tiêu: `Price`).

## Cấu trúc thư mục

```text
Car_Predict/
├── README.md
├── report.md
├── notebook.ipynb
├── requirements.txt
├── data/
│   ├── train.csv
│   └── test.csv
└── outputs/
    ├── figures/
    └── results.csv
```

## Cài đặt

```bash
cd Car_Predict
pip install -r requirements.txt
```

## Chạy notebook

```bash
jupyter notebook notebook.ipynb
```

Chạy từng cell theo thứ tự. Sau khi hoàn thành, lưu biểu đồ vào `outputs/figures/` và cập nhật `outputs/results.csv`.

## Ghi chú

- `data/df_car.csv`: dữ liệu có nhãn `Price` — dùng để EDA, huấn luyện và chia train/test nội bộ.
- `report.md`: báo cáo kết quả (điền sau khi chạy xong notebook).
