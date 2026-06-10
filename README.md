# Automatic License Plate Recognition

Đồ án này là một hệ thống Nhận diện Biển Số Xe Tự Động (ALPR) sử dụng YOLO/InceptionResNetV2 để phát hiện biển số và Tesseract OCR để trích xuất văn bản từ biển số. 

Sau khi cấu trúc lại, dự án bao gồm một tệp huấn luyện (train) và một giao diện Web đơn giản được xây dựng bằng Flask để tải ảnh lên và nhận diện trực tiếp.

## Cấu trúc thư mục

```text
source/
├── data/
│   ├── images/         # Thư mục chứa ảnh dùng để train mô hình
│   └── labels.csv      # File csv chứa thông tin bounding box (toạ độ biển số)
├── static/             # Thư mục chứa file tĩnh của Web App (upload, predict)
├── templates/          # Thư mục giao diện HTML của Web App
├── app.py              # File chạy ứng dụng Web (Flask)
├── deeplearning.py     # Chứa các hàm nhận diện mô hình và đọc chữ (OCR)
├── main.py             # File train mô hình
├── requirements.txt    # Danh sách các thư viện Python cần cài đặt
└── README.md           # Hướng dẫn chạy dự án
```

## Yêu cầu cài đặt

1. Đảm bảo bạn đã cài đặt Python 3 (tốt nhất là 3.8 - 3.10).
2. Cài đặt thêm ứng dụng Tesseract OCR trên hệ điều hành của bạn:
   - **Windows:** Tải bộ cài từ [UB-Mannheim](https://github.com/UB-Mannheim/tesseract/wiki) và thêm thư mục cài đặt vào biến môi trường `PATH`.
   - **macOS:** `brew install tesseract`
   - **Linux:** `sudo apt install tesseract-ocr`
3. Cài đặt các thư viện Python cần thiết:

```bash
pip install -r requirements.txt
```

## Hướng dẫn chạy sản phẩm

### Bước 1: Huấn luyện mô hình (Training)

Mô hình học máy `object_detection.keras` chưa có sẵn nên bạn cần phải huấn luyện lại nghiệm từ tập dữ liệu có trong thư mục `data/images/`.

Chạy lệnh sau tại thư mục gốc của project:

```bash
python main.py
```

Quá trình này sẽ sử dụng dữ liệu để train và sau khi hoàn thành, nó sẽ tự động lưu ra file `object_detection.keras`. Quá trình này có thể mất một thời gian tùy thuộc vào cấu hình máy tính của bạn.

### Bước 2: Chạy Web App (Chạy giao diện)

Sau khi đã có file `object_detection.keras` từ bước 1, bạn có thể khởi động giao diện Web để dùng thử sản phẩm:

```bash
python app.py
```

Ứng dụng sẽ chạy tại địa chỉ: `http://127.0.0.1:5000/`. Mở trình duyệt, tải lên hình ảnh một chiếc xe ô tô và hệ thống sẽ hiển thị kết quả biển số xe.

---
**Lưu ý:** Quá trình huấn luyện đòi hỏi máy tính có cấu hình ổn (tốt nhất có GPU). Nếu chỉ muốn kiểm tra code chạy được hay không, bạn có thể thay đổi tham số `epochs=100` trong file `main.py` xuống một số nhỏ hơn (ví dụ: `epochs=5`) để tiết kiệm thời gian chờ đợi.