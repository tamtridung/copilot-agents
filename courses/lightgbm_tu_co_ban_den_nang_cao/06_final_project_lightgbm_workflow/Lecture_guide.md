# Hướng Dẫn Giảng Dạy: Project Cuối Khóa - Dự Đoán Vỡ Nợ Tín Dụng

## Lời Hứa của Project
"Sau khi hoàn thành project này, học viên sẽ không chỉ là người biết dùng LightGBM, mà sẽ trở thành một người thực hành khoa học dữ liệu có năng lực. Họ sẽ tự tin xử lý một bài toán thực tế từ đầu đến cuối: từ dữ liệu thô, mất cân bằng đến một mô hình đã được tối ưu, đánh giá và quan trọng nhất là có thể giải thích được. Đây là bài kiểm tra tổng hợp để biến kiến thức lý thuyết thành kỹ năng thực chiến."

## Cốt Truyện Giảng Dạy (Khi giới thiệu Project)

1.  **Mở đầu - Bài toán thực tế:**
    *   "Chúng ta đã học rất nhiều về LightGBM. Bây giờ là lúc áp dụng tất cả vào một trong những bài toán kinh điển và có giá trị nhất trong ngành tài chính: **Dự đoán rủi ro vỡ nợ**."
    *   "Hãy tưởng tượng bạn là một Data Scientist tại Home Credit. Mỗi ngày có hàng ngàn đơn vay mới. Làm sao để biết ai là người có khả năng trả nợ và ai không? Một quyết định sai lầm có thể khiến công ty mất hàng triệu đô la. Đây không còn là bài toán lý thuyết, đây là vấn đề kinh doanh thực sự."

2.  **Giới thiệu Nhiệm vụ và Dữ liệu:**
    *   "Nhiệm vụ của các bạn là xây dựng một mô hình tốt nhất có thể để giải quyết bài toán này. Chúng ta sẽ sử dụng một bộ dữ liệu mẫu từ cuộc thi Kaggle 'Home Credit Default Risk'."
    *   **Nhấn mạnh thách thức chính:** "Hãy nhìn vào phân phối của biến mục tiêu. Đây là một bài toán **mất cân bằng điển hình**. Hầu hết mọi người đều trả nợ đúng hạn. Nhiệm vụ của chúng ta là tìm ra những 'viên sỏi' hiếm hoi trong một 'bãi cát' khổng lồ. Đây là một bài kiểm tra thực sự về kỹ năng xử lý dữ liệu của bạn."

3.  **Vạch ra Lộ trình (Các bước gợi ý):**
    *   Trình bày các bước trong notebook project brief một cách rõ ràng.
    *   **Bước 1: Thám tử dữ liệu (EDA & Preprocessing):** "Đừng vội vàng xây dựng mô hình. Hãy dành thời gian để hiểu dữ liệu của bạn. Có bao nhiêu cột? Dữ liệu thiếu ở đâu? Tỷ lệ vỡ nợ chính xác là bao nhiêu? Đây là bước quan trọng nhất."
    *   **Bước 2: Điểm xuất phát (Baseline Model):** "Hãy xây dựng một mô hình đơn giản trước. Đây sẽ là 'cột mốc' để chúng ta so sánh. Đừng quên xử lý vấn đề mất cân bằng ngay từ đầu với `scale_pos_weight`."
    *   **Bước 3: Cuộc đi săn tham số (Hyperparameter Tuning):** "Mô hình cơ sở đã tốt, nhưng chúng ta có thể làm tốt hơn. Hãy để `RandomizedSearchCV` hoặc `Optuna` làm việc, giúp chúng ta 'săn' tìm bộ tham số tốt nhất."
    *   **Bước 4: Mô hình vô địch (Final Model):** "Sau khi có bộ tham số tốt nhất, hãy huấn luyện lại mô hình cuối cùng và xem nó hoạt động tốt đến mức nào trên tập test."
    *   **Bước 5: Kể chuyện bằng dữ liệu (Interpretation):** "Đây là bước biến bạn từ một 'thợ code' thành một 'nhà khoa học dữ liệu'. Mô hình của bạn nói gì? Đặc trưng nào quan trọng nhất? Tại sao một khách hàng lại bị đánh giá là rủi ro? Hãy dùng SHAP để kể câu chuyện đó."

4.  **Thiết lập Kỳ vọng và Tiêu chí:**
    *   "Mục tiêu không phải là đạt được một con số AUC cụ thể, mà là **hoàn thành một quy trình làm việc chuyên nghiệp**."
    *   "Tôi sẽ đánh giá notebook của các bạn dựa trên sự rõ ràng, logic, và chiều sâu của phân tích. Một notebook được trình bày tốt với những nhận xét sâu sắc sẽ được đánh giá cao hơn một notebook chỉ có code và cho ra điểm số cao hơn một chút."

5.  **Lời khuyên và Động viên:**
    *   "Đừng ngại thử nghiệm. Thay đổi cách xử lý missing data, thử một không gian tìm kiếm tham số khác, hoặc đào sâu hơn vào một đặc trưng thú vị mà bạn tìm thấy."
    *   "Đây là cơ hội để các bạn tổng hợp tất cả những gì đã học. Hãy xem nó như một dự án portfolio đầu tiên của bạn về LightGBM. Chúc các bạn thành công!"

## Khi Review Lời Giải (Solution Notebook)

*   **Tập trung vào "Tại sao":** Khi trình bày lời giải, không chỉ chạy code. Hãy giải thích *tại sao* lại chọn `scale_pos_weight`, *tại sao* `LabelEncoder` lại phù hợp ở đây, *tại sao* `EXT_SOURCE_2` lại quan trọng như vậy dựa trên biểu đồ SHAP.
*   **So sánh Trước và Sau:** Luôn so sánh kết quả của mô hình cuối cùng với mô hình cơ sở. "Nhìn xem, sau khi tuning, điểm AUC của chúng ta đã tăng từ X lên Y. Điều này cho thấy quá trình tìm kiếm tham số đã có hiệu quả."
*   **Liên kết với Kinh doanh:** Dịch các kết quả kỹ thuật sang ngôn ngữ kinh doanh. "Kết quả từ SHAP cho thấy chúng ta nên đặc biệt cẩn trọng với những khách hàng trẻ tuổi và có điểm tín dụng bên ngoài thấp. Đây là thông tin mà bộ phận thẩm định rủi ro có thể sử dụng ngay lập tức."
