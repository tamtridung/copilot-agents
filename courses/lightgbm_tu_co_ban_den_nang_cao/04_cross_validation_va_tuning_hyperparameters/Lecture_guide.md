# Hướng Dẫn Giảng Dạy: Bài 4 - Cross-Validation và Tuning Hyperparameters

## Lời Hứa của Bài Học
Sau buổi học này, học viên sẽ không còn tin vào "may mắn" của một lần chia train-test nữa. Họ sẽ hiểu và áp dụng được K-Fold Cross-Validation như một phương pháp đánh giá mô hình đáng tin cậy. Quan trọng hơn, họ sẽ nắm được các chiến lược tự động tìm kiếm tham số từ cơ bản (Grid Search, Random Search) đến nâng cao, giúp họ tìm ra những mô hình mạnh mẽ nhất một cách có hệ thống.

## Cốt Truyện Giảng Dạy (Teaching Storyline)

1.  **Mở đầu - Sự mong manh của may mắn:**
    *   "Ở các bài trước, chúng ta đã chia dữ liệu thành train và test. Giả sử mô hình đạt AUC 0.85. Con số này có đáng tin không? Điều gì xảy ra nếu chúng ta xáo trộn dữ liệu và chia lại? Liệu kết quả có còn là 0.85 không?"
    *   "Việc đánh giá trên một tập test duy nhất giống như làm một bài kiểm tra thử. Bạn có thể gặp may trúng đề, hoặc gặp xui vào đề khó. Kết quả đó không phản ánh chính xác năng lực thực sự của bạn. Chúng ta cần một phương pháp 'thi thử' nhiều lần trên nhiều 'đề' khác nhau. Đó chính là Cross-Validation."

2.  **Giới thiệu "Tiêu chuẩn vàng" - K-Fold Cross-Validation:**
    *   Dùng hình ảnh minh họa trong notebook để giải thích.
    *   **Analogy:** "Hãy tưởng tượng bạn có một cuốn sách bài tập gồm 5 chương. Thay vì dùng 4 chương đầu để học và chương 5 để kiểm tra, bạn sẽ làm 5 bài kiểm tra khác nhau:
        *   Lần 1: Học chương 2,3,4,5; kiểm tra trên chương 1.
        *   Lần 2: Học chương 1,3,4,5; kiểm tra trên chương 2.
        *   ... và cứ thế.
        *   Điểm số cuối cùng của bạn là trung bình của 5 bài kiểm tra đó. Rõ ràng là nó đáng tin cậy hơn nhiều."
    *   **Demo với `lgb.cv`:** "LightGBM làm cho việc này trở nên cực kỳ đơn giản với hàm `lgb.cv`. Hãy xem cách nó hoạt động."
        *   Chạy cell code và chỉ vào kết quả: "Đây là điểm AUC trung bình, và đây là độ lệch chuẩn. Độ lệch chuẩn nhỏ cho thấy mô hình hoạt động ổn định trên các tập dữ liệu con khác nhau. `lgb.cv` cũng tự động tìm số cây tối ưu cho chúng ta!"

3.  **Bài toán tìm kiếm - "Mò kim đáy bể":**
    *   "Chúng ta đã biết cách đánh giá một bộ tham số. Nhưng có hàng triệu cách kết hợp các tham số. Làm sao để tìm ra bộ tốt nhất? Đây là lúc chúng ta cần các 'robot tìm kiếm' tự động."
    *   **Giới thiệu Grid Search (Robot ngây thơ):**
        *   "Grid Search giống như một robot được lập trình để kiểm tra từng ô trong một bãi đất hình vuông. Nó sẽ đi qua tất cả các ô, không bỏ sót ô nào."
        *   "Ưu điểm: Chắc chắn tìm ra 'kho báu' nếu nó nằm trong bãi đất đó. Nhược điểm: Nếu bãi đất quá lớn, nó sẽ đi đến 'muôn đời'."
    *   **Giới thiệu Random Search (Robot thông minh hơn):**
        *   "Random Search cũng được giao nhiệm vụ tìm kho báu trong bãi đất đó, nhưng thay vì đi từng ô, nó sẽ nhảy dù ngẫu nhiên xuống một số vị trí định trước (ví dụ 50 lần)."
        *   "Tại sao nó lại hiệu quả? Vì thường thì kho báu không được phân bổ đều. Có những khu vực rộng lớn không có gì. Random Search có cơ hội cao hơn để đáp xuống gần một khu vực 'màu mỡ' mà không lãng phí thời gian vào những vùng 'khô cằn'. Đặc biệt hiệu quả khi chỉ một vài tham số là thực sự quan trọng."
    *   **Giới thiệu Bayesian Optimization (Robot siêu thông minh - nói lướt qua):**
        *   "Đây là robot tối tân nhất. Sau mỗi lần đào, nó sẽ ghi nhớ 'đất ở đây cứng hay mềm', 'có dấu hiệu gì không'. Dựa vào đó, nó sẽ phán đoán xem lần đào tiếp theo nên ở đâu để có khả năng tìm thấy kho báu cao nhất. Nó học hỏi từ kinh nghiệm."

4.  **Demo thực tế - `RandomizedSearchCV`:**
    *   Chuyển sang notebook. "Scikit-learn cung cấp cho chúng ta những con robot này. Hãy xem cách sử dụng `RandomizedSearchCV`."
    *   **Demo Flow:**
        1.  **Định nghĩa không gian tìm kiếm:** "Thay vì một danh sách cố định, chúng ta cho robot biết 'hãy tìm `num_leaves` trong khoảng từ 20 đến 50'." Giải thích cách dùng `randint` và `uniform`.
        2.  **Khởi tạo `RandomizedSearchCV`:** Giải thích các tham số quan trọng: `estimator` (mô hình), `param_distributions` (không gian tìm kiếm), `n_iter` (số lần thử), `cv` (số fold cho mỗi lần thử), `scoring` (tiêu chí đánh giá).
        3.  **Chạy tìm kiếm:** "Bây giờ, hãy để robot làm việc. Nó sẽ tự động thử `n_iter` bộ tham số, mỗi bộ được đánh giá bằng `cv`-fold cross-validation."
        4.  **Phân tích kết quả:** "Và đây là kết quả! `best_params_` cho chúng ta bộ tham số tốt nhất mà nó tìm thấy, và `best_score_` là điểm số cross-validation tương ứng. Chúng ta đã tự động hóa một công việc cực kỳ tẻ nhạt và tốn thời gian."

5.  **Tổng kết và Cầu nối:**
    *   **Tóm tắt quy trình làm việc chuẩn:**
        1.  Bắt đầu với một mô hình cơ sở.
        2.  Sử dụng Cross-Validation để có một đánh giá đáng tin cậy.
        3.  Định nghĩa một không gian tìm kiếm hợp lý cho các siêu tham số.
        4.  Sử dụng Random Search (hoặc các công cụ mạnh hơn) để tự động tìm kiếm bộ tham số tốt nhất.
        5.  Huấn luyện mô hình cuối cùng trên toàn bộ dữ liệu huấn luyện với bộ tham số tốt nhất đã tìm được.
    *   **Cầu nối đến bài tiếp theo:** "Chúng ta đã có một mô hình với điểm số cao, được tinh chỉnh cẩn thận và đánh giá khách quan. Nhưng con số không phải là tất cả. Nếu sếp bạn hỏi: 'Tại sao mô hình lại dự đoán khách hàng A sẽ rời đi?', bạn sẽ trả lời thế nào? Trong bài học cuối cùng, chúng ta sẽ học cách 'nhìn vào bên trong' mô hình, hiểu được quyết định của nó và giải thích chúng một cách thuyết phục."
