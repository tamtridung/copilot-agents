# Hướng Dẫn Giảng Dạy: Bài 2 - Tối Ưu Hóa Hiệu Năng với Parameters

## Lời Hứa của Bài Học
Sau buổi học này, học viên sẽ không còn nhìn vào các tham số của LightGBM như một "hộp đen" nữa. Họ sẽ hiểu rõ ý nghĩa của các tham số cốt lõi, biết cách tinh chỉnh chúng một cách có chiến lược để cân bằng giữa độ chính xác và overfitting, và có thể áp dụng `early stopping` để tự động tìm ra số lượng cây tối ưu.

## Cốt Truyện Giảng Dạy (Teaching Storyline)

1.  **Mở đầu - Kết nối với Bài 1:**
    *   "Ở bài trước, chúng ta đã xây dựng thành công mô hình LightGBM đầu tiên. Nó chạy rất nhanh và cho kết quả khá tốt với thiết lập mặc định. Nhưng 'khá tốt' có phải là tốt nhất chưa?"
    *   Sử dụng analogy: "Một chiếc xe đua F1 có động cơ mạnh mẽ, nhưng để chiến thắng, các kỹ sư phải tinh chỉnh từng chi tiết nhỏ: cánh gió, độ cứng của hệ thống treo, loại lốp... Tinh chỉnh tham số trong LightGBM cũng tương tự như vậy. Nó quyết định sự khác biệt giữa một mô hình tốt và một mô hình xuất sắc."

2.  **Hệ thống hóa kiến thức - Ba nhóm tham số:**
    *   "Để không bị 'ngợp' trước hàng chục tham số, chúng ta sẽ chia chúng thành 3 nhóm logic."
    *   **Nhóm 1: Điều khiển cấu trúc cây (The Architects):**
        *   **`num_leaves`:** "Đây là nút vặn quan trọng nhất. Nó quyết định độ phức tạp của mỗi cây. Tăng nó lên giống như cho phép kiến trúc sư vẽ một bản thiết kế phức tạp hơn. Mạnh mẽ hơn, nhưng cũng dễ sai sót (overfitting) hơn."
        *   **`max_depth`:** "Đây là một cái 'thắng an toàn', ngăn cây mọc quá sâu. Tuy nhiên, trong LightGBM, chúng ta thường 'lái' bằng `num_leaves`."
        *   **`min_child_samples`:** "Quy tắc của người thợ mộc: 'Đừng cắt một miếng gỗ nếu nó quá nhỏ'. Tham số này ngăn mô hình tạo ra những quyết định dựa trên quá ít dữ liệu."
    *   **Nhóm 2: Điều khiển quá trình học (The Pacing Coach):**
        *   **`learning_rate`:** "Đây là 'tốc độ' mà mô hình học. Tốc độ cao (0.1) giúp học nhanh, nhưng có thể 'chạy lố' điểm tốt nhất. Tốc độ chậm (0.01) giúp học cẩn thận hơn, từng bước nhỏ, thường cho kết quả tốt hơn."
        *   **`n_estimators`:** "Số 'bước' mà mô hình sẽ đi. Nếu `learning_rate` là kích thước của mỗi bước, thì `n_estimators` là tổng số bước."
        *   **Nhấn mạnh mối quan hệ:** "Bước nhỏ thì phải đi nhiều bước. `learning_rate` nhỏ thì cần `n_estimators` lớn."
    *   **Nhóm 3: Điều khiển dữ liệu (The Samplers):**
        *   **`feature_fraction` & `bagging_fraction`:** "Để tránh 'học vẹt', ở mỗi vòng, chúng ta không cho mô hình xem toàn bộ dữ liệu. Chúng ta cho nó xem một phần ngẫu nhiên các 'câu hỏi' (`feature_fraction`) và một phần ngẫu nhiên các 'học sinh' (`bagging_fraction`). Điều này buộc mô hình phải học các quy luật tổng quát hơn."

3.  **Demo thực tế - So sánh trực tiếp:**
    *   Chuyển sang notebook. "Bây giờ, hãy xem các 'thiết lập' khác nhau ảnh hưởng đến 'thành tích' của chiếc xe đua của chúng ta như thế nào."
    *   **Demo Flow:**
        1.  **Chạy Mô hình Baseline:** "Đây là thành tích của chúng ta với thiết lập nhà sản xuất. Ghi nhớ con số này: Accuracy và ROC AUC."
        2.  **Chạy Mô hình "Liều lĩnh" (Overfitting):** Tăng `num_leaves` và `n_estimators`. "Hãy xem điều gì xảy ra khi chúng ta 'độ' xe quá mức. Có thể nó chạy rất nhanh trên đường thử (tập train), nhưng ra đường đua thật (tập test) thì kết quả có thể không như ý." (Nếu có thể, cho thấy score trên tập train cao vút nhưng test thì không).
        3.  **Chạy Mô hình "Cẩn trọng" (Regularized):**
            *   Giới thiệu bộ tham số đã được tinh chỉnh. Giải thích *tại sao* lại chọn các giá trị đó (ví dụ: `learning_rate` nhỏ, `num_leaves` vừa phải, có subsampling...).
            *   **Giới thiệu Early Stopping:** "Nhưng làm sao chúng ta biết cần bao nhiêu cây là đủ? 1000? 2000? Sẽ rất tốn thời gian để đoán. Đây là lúc `early stopping` tỏa sáng. Nó giống như một người giám sát thông minh, theo dõi hiệu suất trên tập validation và hô 'Dừng!' khi mô hình không còn tốt lên nữa."
            *   Chạy cell code và chỉ vào kết quả: "Hãy xem, ROC AUC đã cải thiện! Và `early stopping` đã cho chúng ta biết con số cây tối ưu là `xxx`, không phải 1000. Chúng ta đã tiết kiệm được rất nhiều thời gian tính toán."

4.  **Tổng kết và Chiến lược tinh chỉnh:**
    *   **Tóm tắt các quy tắc ngón tay cái (Rules of Thumb):**
        *   Để chống overfitting: Giảm `num_leaves`, tăng `min_child_samples`, dùng subsampling, dùng `learning_rate` nhỏ + `early stopping`.
        *   Để tăng accuracy: Tăng `num_leaves`, nhưng phải theo dõi validation score cẩn thận.
    *   **Cung cấp một chiến lược tinh chỉnh đơn giản:**
        1.  Bắt đầu với `learning_rate` nhỏ (0.05) và `n_estimators` lớn (1000+).
        2.  Sử dụng `early stopping` để tìm số cây tối ưu.
        3.  Tinh chỉnh các tham số cây (`num_leaves`, `max_depth`, `min_child_samples`).
        4.  Tinh chỉnh các tham số regularization (`feature_fraction`, `bagging_fraction`, `reg_alpha`, `reg_lambda`).
    *   **Cầu nối đến bài tiếp theo:** "Chúng ta đã biết cách tinh chỉnh 'động cơ'. Nhưng hiệu suất của xe còn phụ thuộc vào 'nhiên liệu' - chính là dữ liệu. Trong bài tới, chúng ta sẽ học cách 'pha chế' dữ liệu, đặc biệt là cách xử lý các biến categorical một cách hiệu quả nhất, để LightGBM có thể phát huy hết sức mạnh của nó."

## Gợi ý và Analogies
*   **Overfitting:** Học thuộc lòng đáp án cho đề cương ôn thi, nhưng khi gặp đề thi thật với câu hỏi hơi khác một chút thì không làm được.
*   **Regularization:** Một hình thức "khiêm tốn hóa" mô hình, không cho phép nó quá tự tin vào những gì nó học được từ tập train.
*   **Early Stopping:** Một huấn luyện viên chạy marathon theo dõi thời gian hoàn thành mỗi vòng của vận động viên. Nếu sau vài vòng mà thời gian không cải thiện, ông ấy biết rằng vận động viên đã đạt đến giới hạn và cho họ dừng lại để tránh kiệt sức hoặc chấn thương.

## Cues cho Ứng dụng Thực tế
*   "Khi bạn trình bày kết quả, đừng chỉ nói 'em đã chạy LightGBM'. Hãy nói 'em đã sử dụng LightGBM với learning rate nhỏ và early stopping để đảm bảo mô hình không bị overfitting, và kết quả ROC AUC trên tập test là 0.85'."
*   "Trong thực tế, bạn sẽ không tinh chỉnh bằng tay như thế này. Bạn sẽ dùng các công cụ như Grid Search, Random Search, hoặc Bayesian Optimization (ví dụ: Optuna, Hyperopt). Nhưng việc hiểu ý nghĩa các tham số sẽ giúp bạn xác định không gian tìm kiếm (search space) một cách thông minh, tiết kiệm hàng giờ, thậm chí hàng ngày tính toán."
