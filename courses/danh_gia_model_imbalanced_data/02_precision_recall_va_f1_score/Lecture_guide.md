# Hướng dẫn Giảng dạy - Bài 02: Precision, Recall, F1-Score và Sự Đánh Đổi

## 1. Lời hứa của Bài học (Lesson Promise)

Sau bài học này, học viên sẽ có thể:
- Giải thích một cách tường tận về sự đánh đổi (trade-off) giữa Precision và Recall.
- Hiểu được vai trò của "ngưỡng quyết định" (decision threshold) và cách nó ảnh hưởng đến kết quả dự đoán.
- Đọc và phân tích được một biểu đồ Precision-Recall Curve để lựa chọn ngưỡng phù hợp.
- Tính toán và giải thích được ý nghĩa của F1-Score như một thước đo cân bằng.
- Tự tìm được ngưỡng quyết định tối ưu để tối đa hóa F1-Score cho một mô hình.

## 2. Cốt truyện Giảng dạy (Teaching Storyline)

**Mở đầu (Hook) & Kết nối từ bài trước:**
- "Ở bài trước, chúng ta đã gặp hai 'chuyên gia' đánh giá mô hình: Mr. Precision, người cực kỳ cẩn trọng và không muốn bắt sai ai, và Ms. Recall, người không muốn bỏ sót bất kỳ ai. Vấn đề là, hai người này thường không đồng ý với nhau. Hôm nay, chúng ta sẽ tìm hiểu tại sao họ lại 'cãi nhau' và làm thế nào để hòa giải họ."
- "Câu hỏi cốt lõi là: Làm thế nào một mô hình như Logistic Regression quyết định một email là spam hay không? Nó không chỉ đơn giản nói 'có' hoặc 'không'. Nó đưa ra một điểm số. Và chính cách chúng ta diễn giải điểm số đó đã tạo ra sự đánh đổi."

**Chuyển tiếp vào nội dung chính:**
1.  **Giải mã "Hộp đen" - Ngưỡng Quyết Định:**
    - Giới thiệu khái niệm `predict_proba`. "Mô hình không nói 'đây là chó'. Nó nói 'tôi nghĩ 85% đây là chó'."
    - "Ngưỡng 0.5 mặc định giống như một giám khảo trung lập. Trên 50% thì đậu, dưới thì rớt."
    - Dùng phép loại suy về một giám khảo chấm thi.
        - **Tăng ngưỡng (ví dụ 0.9):** "Giám khảo này siêu khó tính. Chỉ những bài gần như hoàn hảo (90%) mới đậu." -> Ít người đậu (FP thấp, FN cao) -> **Precision cao, Recall thấp**.
        - **Giảm ngưỡng (ví dụ 0.3):** "Giám khảo này rất dễ dãi. Chỉ cần làm được chút ít (30%) là cho qua." -> Nhiều người đậu (FP cao, FN thấp) -> **Precision thấp, Recall cao**.
    - Chạy cell code đầu tiên để chứng minh bằng số liệu.

2.  **Trực quan hóa sự đánh đổi - Precision-Recall Curve:**
    - "Thử từng ngưỡng một thì quá mất công. Tại sao không vẽ tất cả các khả năng lên một biểu đồ?"
    - Giới thiệu P-R Curve. Giải thích trục hoành (Recall) và trục tung (Precision).
    - "Mỗi điểm trên đường này là một 'vị giám khảo' với một độ khó tính khác nhau."
    - "Đường cong lý tưởng ở đâu? Ở góc trên bên phải (Precision=1, Recall=1) - một vị giám khảo hoàn hảo. Mô hình của chúng ta càng gần góc đó càng tốt."
    - Chạy cell code vẽ P-R curve và phân tích nó. "Nhìn xem, để có Recall cao (đi sang phải), chúng ta phải chấp nhận Precision thấp hơn (đường đi xuống)."

3.  **Tìm người hòa giải - F1-Score:**
    - "Vậy cuối cùng nên chọn giám khảo nào? Chúng ta cần một chỉ số duy nhất để so sánh các mô hình một cách công bằng."
    - Giới thiệu F1-Score. Nhấn mạnh nó là **trung bình điều hòa**.
    - **Tại sao là điều hòa?** Dùng ví dụ cực đoan: Precision=1, Recall=0.01.
        - Trung bình cộng = (1 + 0.01)/2 = 0.505 (trông có vẻ ổn).
        - Trung bình điều hòa (F1) ≈ 0.02 (rất tệ, phản ánh đúng sự thật).
    - "F1-Score giống như một người quản lý thông thái, sẽ cho điểm thấp nếu một trong hai chuyên gia (Precision hoặc Recall) làm việc quá tệ."

4.  **Tìm điểm cân bằng vàng:**
    - "Với F1-Score, giờ chúng ta có thể tìm ra 'vị giám khảo' tốt nhất - người tạo ra sự cân bằng tốt nhất giữa Precision và Recall."
    - Chạy cell code cuối cùng để tìm ngưỡng tối ưu hóa F1-Score và đánh dấu nó trên biểu đồ. "Đây, đây chính là điểm ngọt ngào (sweet spot) mà chúng ta tìm kiếm cho bài toán này."

**Tổng kết và Chuyển tiếp:**
- Tóm tắt lại hành trình: Từ việc hiểu ngưỡng quyết định, thấy được sự đánh đổi qua P-R curve, và cuối cùng tìm được điểm cân bằng với F1-Score.
- "F1-Score rất tuyệt, nhưng nó giả định rằng Precision và Recall quan trọng như nhau. Nhưng có phải lúc nào cũng vậy không? Và P-R curve chỉ là một cách nhìn. Có một cách nhìn tổng quan khác, không phụ thuộc vào ngưỡng, giúp so sánh 'sức mạnh' cốt lõi của các mô hình không? Đó là câu chuyện về đường cong ROC và chỉ số AUC, chúng ta sẽ khám phá trong bài học tiếp theo."

## 3. Những điểm cần nhấn mạnh bằng ngôn ngữ đơn giản

- "Ngưỡng quyết định là 'thanh xà' mà mô hình phải nhảy qua. Thanh xà càng cao, càng ít vận động viên qua được (Precision cao, Recall thấp)."
- "P-R Curve là bản đồ về mọi khả năng đánh đổi của mô hình."
- "F1-Score là một 'bộ gộp' thông minh của Precision và Recall. Nó không cho phép một trong hai chỉ số quá tệ."
- "Tìm F1-score cao nhất cũng giống như tìm điểm cao nhất trên một ngọn đồi. Điểm đó cho bạn sự kết hợp P-R tốt nhất."

## 4. Những điểm học viên thường bị rối

- **`predict` vs `predict_proba`:** Học viên mới thường chỉ biết `predict()` và không hiểu rằng đằng sau nó là một điểm số và một ngưỡng. Cần làm rõ điều này.
- **Trục của P-R Curve:** Có thể nhầm lẫn trục nào là Precision, trục nào là Recall.
- **Công thức F1-Score:** Công thức trung bình điều hòa trông phức tạp. Quan trọng là giải thích *tại sao* nó được dùng (để trừng phạt các giá trị thấp) hơn là bắt học viên nhớ công thức.
- **Mối quan hệ giữa `thresholds` và `precisions`/`recalls`:** Mảng `thresholds` trả về từ `precision_recall_curve` có ít hơn 1 phần tử. Đây là một chi tiết kỹ thuật của scikit-learn, cần lưu ý khi code để tránh lỗi `index out of bounds`.

## 5. Phép loại suy hoặc Mô hình tư duy gợi ý

- **Giám khảo chấm thi:** Đã dùng ở trên, rất hiệu quả để giải thích ngưỡng quyết định.
- **Bộ lọc email:**
    - **Ngưỡng cao:** Bộ lọc rất nghiêm ngặt. Hòm thư đến của bạn sẽ siêu sạch, không có spam. (Precision cao). Nhưng có thể một vài email quan trọng (newsletter, hóa đơn) sẽ bị vào mục spam. (Recall thấp).
    - **Ngưỡng thấp:** Bộ lọc rất thoải mái. Bạn sẽ nhận được tất cả email quan trọng. (Recall cao). Nhưng hòm thư đến sẽ có nhiều thư rác hơn. (Precision thấp).

## 6. Luồng Demo cho Notebook

1.  **Cell 1-4:** Giới thiệu và giải thích lý thuyết về ngưỡng.
2.  **Cell 5 (Code thay đổi ngưỡng):**
    - Chạy code.
    - Phân tích output: "Nhìn này, ngưỡng 0.8 cho Precision 0.9 nhưng Recall chỉ còn 0.6. Ngược lại, ngưỡng 0.3 cho Recall 0.9 nhưng Precision chỉ còn 0.8. Thấy sự đánh đổi chưa?"
3.  **Cell 6-7 (Code P-R Curve):**
    - Chạy code để vẽ biểu đồ.
    - Dùng con trỏ chuột di chuyển trên đường cong và giải thích: "Ở điểm này, Recall cao nhưng Precision thấp. Ở điểm này, Precision cao nhưng Recall thấp."
4.  **Cell 8-9 (Code F1-Score):**
    - Chạy code tính F1.
    - So sánh các giá trị F1: "Ngưỡng 0.5 và 0.3 cho F1-score tốt hơn ngưỡng 0.8, cho thấy sự cân bằng tốt hơn."
5.  **Cell 10-11 (Code tìm ngưỡng F1 tốt nhất):**
    - Chạy code.
    - Chỉ vào kết quả in ra: "Vậy là với mô hình này, ngưỡng khoảng 0.55 cho chúng ta F1-Score cao nhất."
    - Chỉ vào điểm màu đỏ trên biểu đồ: "Và đây là vị trí của nó trên bản đồ P-R."

## 7. Gợi ý khi nói về ứng dụng thực tế

- "Khi bạn tìm kiếm trên Google, bạn muốn 10 kết quả đầu tiên phải cực kỳ liên quan. Đó là ưu tiên **Precision**. Google không cần phải cho bạn thấy *tất cả* các trang web liên quan trên thế giới (Recall không cần = 1)."
- "Khi một sân bay dùng hệ thống nhận diện khuôn mặt để tìm một kẻ tình nghi, họ không thể để lọt người đó. Họ sẽ chấp nhận một vài báo động nhầm (FP) để đảm bảo bắt được kẻ tình nghi (FN thấp). Họ sẽ chỉnh ngưỡng để ưu tiên **Recall**."
- "Hầu hết các bài toán kinh doanh thông thường, như phân loại khách hàng tiềm năng, phát hiện churn, đều muốn một sự cân bằng. Vì vậy, bắt đầu với **F1-Score** là một lựa chọn mặc định rất hợp lý."

## 8. Tổng kết mạnh mẽ và cầu nối đến bài sau

"Hôm nay, chúng ta đã đi sâu vào 'trái tim' của sự đánh đổi trong các mô hình phân loại. Chúng ta đã học cách sử dụng ngưỡng quyết định như một 'nút vặn' để điều chỉnh giữa Precision và Recall, và dùng F1-Score để tìm ra điểm cân bằng tối ưu. Nhưng tất cả những điều này đều xoay quanh việc phân loại đúng/sai lớp Positive. Liệu có cách nào đánh giá khả năng *phân tách* tổng thể giữa hai lớp của mô hình, bất kể ta chọn ngưỡng nào không? Câu trả lời là có, và đó là sức mạnh của đường cong ROC và chỉ số AUC. Hẹn gặp lại các bạn trong bài học tiếp theo."
