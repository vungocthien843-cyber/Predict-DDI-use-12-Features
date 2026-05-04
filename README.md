Data Source: Drugbank 5.0

## Data Processing Pipeline

Dự án áp dụng chiến lược **Data-Centric (lấy dữ liệu làm trung tâm)** với quy trình tinh lọc khắt khe nhằm duy trì tính phân phối thực tế của dữ liệu y sinh, thay vì sử dụng các kỹ thuật ép cân bằng nhân tạo. Quy trình bao gồm các bước sau:

* **Bước 1: Sàng lọc tập dữ liệu (Data Curation)**
  * Dữ liệu thô được trích xuất từ cơ sở dữ liệu DrugBank.
  * Giữ nguyên tập mẫu âm (negative samples) để duy trì sự mất cân bằng tự nhiên (natural imbalanced distribution), giúp phản ánh chính xác tỷ lệ tương tác DDI trong môi trường dược lý thực tế.

* **Bước 2: Trích xuất đặc trưng đa góc nhìn (Multi-View Feature Extraction)**
  * Xây dựng không gian nguyên bản gồm **12 chiều đặc trưng**, kết hợp giữa:
    * *Đặc trưng cấu trúc đồ thị (Topological Features)*: Rút trích từ mạng lưới tương tác thuốc.
    * *Đặc trưng ngữ nghĩa sinh học (Semantic Features)*: Các thuộc tính hóa sinh liên quan.

* **Bước 3: Đánh giá và Tinh lọc đặc trưng (Feature Evaluation & Refinement)**
  * Áp dụng công cụ **SHAP (SHapley Additive exPlanations)** trên mô hình Random Forest để định lượng mức độ đóng góp của từng đặc trưng.
  * Thực hiện thực nghiệm bóc tách (Ablation study) để nhận diện và loại bỏ các đặc trưng gây nhiễu (như ATC và ADE). 
  * *Kết quả:* Không gian dữ liệu được tối ưu hóa từ 12 đặc trưng ban đầu xuống còn **10 đặc trưng lõi**.

* **Bước 4: Phân chia dữ liệu và Đánh giá chéo (Data Splitting & Cross-Validation)**
  * Dữ liệu được chuẩn hóa và phân chia theo chiến lược **5-Fold Cross-Validation** để đảm bảo tính khách quan và kiểm soát hiện tượng quá khớp (overfitting) trong quá trình huấn luyện các mô hình phân lớp và kiến trúc Stacking Ensemble.
