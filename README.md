# Financial-DIP

Dự án này cung cấp quy trình tự động hóa để trích xuất, nhận diện và chuyển đổi dữ liệu bảng biểu tài chính từ định dạng PDF sang CSV, sử dụng các mô hình Deep Learning (Table Transformer).

## Quy trình xử lý (Workflow)

Hệ thống được chia thành 3 bước chính:

### Bước 1: Huấn luyện mô hình Table Transformer (Nhận diện cấu trúc bảng)

Thư mục `table_transformer_training` chứa mã nguồn để huấn luyện mô hình. Dữ liệu huấn luyện đã được tích hợp sẵn trong notebook.

**Cách chạy trên Google Colab:**
1. Upload file `table_transformer.ipynb` lên Google Colab.
2. Upload file `table-transformer.zip` (Mã nguồn gốc, chỉ giải nén và chạy trên môi trường Colab).
3. Upload trọng số ban đầu `TATR-v1.1-All-msft.pth` để thực hiện fine-tune mô hình.

**Kết quả đầu ra:**
- File model đã huấn luyện: `model_20 (3).pt` (Dùng để dự đoán cấu trúc bảng ở Bước 2).
- Kết quả dự đoán mẫu được xuất ra định dạng HTML và lưu trong thư mục `result`.

---

### Bước 2: Huấn luyện và chạy pipeline trích xuất bảng từ PDF

Sử dụng file `Extract_title_and_vintern_predict_to_html.ipynb` để huấn luyện mô hình phát hiện đối tượng (bảng, tiêu đề, số trang) và chạy quy trình dự đoán đầy đủ.

**Huấn luyện:**
- Sau khi chạy huấn luyện, trọng số tốt nhất sẽ được lưu tại: `best (4).pt`.

**Quy trình dự đoán (Pipeline):**
1. Import `table-transformer.zip`.
2. Load trọng số `model_20 (3).pt` (Output từ Bước 1).
3. Thực hiện chuỗi xử lý:
    - **PDF to Image**: Chuyển đổi trang tài liệu thành ảnh.
    - **Detection**: Nhận diện tiêu đề (Title) và xác định bảng cân đối kế toán.
    - **Structure Recognition**: Dùng Table Transformer để dự đoán cấu trúc dòng/cột.
    - **Export**: Xuất kết quả cuối cùng ra file HTML.

---

### Bước 3: Xuất dữ liệu từ HTML sang CSV

Sử dụng script/notebook `export_to_csv` để chuyển đổi định dạng cuối cùng.

- **Input**: File HTML (từ Bước 2).
- **Output**: File CSV (sẵn sàng cho các bước phân tích dữ liệu tiếp theo).
