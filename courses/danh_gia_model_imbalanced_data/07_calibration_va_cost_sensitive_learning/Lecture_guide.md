# Hướng dẫn Giảng dạy - Bài 07: Calibration & Cost-Sensitive Learning

## 1. Lời hứa của Bài học (Lesson Promise)

Sau bài học này, học viên sẽ có thể:
- Giải thích được sự khác biệt giữa một dự đoán "đúng/sai" và một dự đoán "đáng tin cậy".
- Đọc và diễn giải một biểu đồ hiệu chỉnh (Calibration Plot) để đánh giá độ tin cậy của xác suất mô hình.
- Giải thích được triết lý của Cost-Sensitive Learning và sự khác biệt của nó so với resampling.
- Áp dụng `class_weight='balanced'` như một công cụ mạnh mẽ, nhanh chóng để cải thiện mô hình trên dữ liệu mất cân bằng.

## 2. Cốt truyện Giảng dạy (Teaching Storyline)

**Mở đầu (Hook) & Kết nối từ bài trước:**
- "Trong bài trước, chúng ta đã học cách 'phẫu thuật' dữ liệu bằng SMOTE. Đó là một phương pháp rất mạnh. Nhưng nó có một nhược điểm tiềm ẩn: nó có thể làm cho xác suất mà mô hình đưa ra trở nên khó tin. Hôm nay, chúng ta sẽ tìm hiểu tại sao sự 'tự tin' của mô hình lại quan trọng, và khám phá một phương pháp hoàn toàn khác để xử lý mất cân bằng mà không cần đụng đến một byte dữ liệu nào."
- "Hãy tưởng tượng hai bác sĩ cùng chẩn đoán bạn bị bệnh. Bác sĩ A nói 'Tôi đoán bạn bị bệnh'. Bác sĩ B nói 'Tôi chắc chắn 95% bạn bị bệnh'. Rõ ràng, thông tin của bác sĩ B giá trị hơn nhiều. Chúng ta cũng muốn mô hình của mình giống như bác sĩ B."

**Chuyển tiếp vào nội dung chính:**
1.  **Calibration - Sự tự tin có cơ sở:**
    - "Một mô hình nói 'xác suất 80%' có thực sự nghĩa là 80% không? Hay nó chỉ đang 'chém gió'?"
    - Giới thiệu khái niệm Calibration và tầm quan trọng của nó trong việc ra quyết định.
    - Giới thiệu Calibration Plot. Đây là một biểu đồ có thể hơi khó hiểu lúc đầu, cần giải thích kỹ:
        - "Trục X là những gì mô hình NÓI (xác suất dự đoán)."
        - "Trục Y là những gì thực sự XẢY RA (tỷ lệ thực tế)."
        - "Đường chéo y=x là đường 'nói thật'. Nếu mô hình của bạn nằm trên đường này, nó là một mô hình trung thực."
    - Chạy code để so sánh `LogisticRegression` (thường tốt) và `GaussianNB` (thường tệ).
    - Phân tích biểu đồ: "Nhìn xem, Logistic Regression gần như là một 'người trung thực'. Còn Naive Bayes thì sao? Ở các mức xác suất cao, nó quá tự tin (đường cong nằm dưới đường chéo). Nó nói 90% nhưng thực tế chỉ có 80%."
    - **Cảnh báo quan trọng:** "SMOTE có thể phá vỡ sự trung thực này. Sau khi dùng SMOTE, hãy cẩn thận khi tin vào xác suất của mô hình."

2.  **Cost-Sensitive Learning - Thay đổi luật chơi:**
    - "Bây giờ, hãy đến với một triết lý hoàn toàn khác. Thay vì sửa dữ liệu, chúng ta sửa 'luật chơi' cho thuật toán."
    - "Hãy tưởng tượng mô hình là một đứa trẻ đang học. Nếu nó làm sai một bài toán dễ, nó bị phạt 1 roi. Nhưng nếu nó làm sai một bài toán khó (lớp thiểu số), nó bị phạt 10 roi. Đứa trẻ sẽ nhanh chóng học được cách phải cẩn thận hơn với các bài toán khó."
    - Giới thiệu `class_weight` chính là cách chúng ta đặt ra "mức phạt" này.

3.  **`class_weight='balanced'` - Phép màu đơn giản:**
    - "Scikit-learn cung cấp một cây đũa thần tên là `class_weight='balanced'`. Nó tự động tính toán mức phạt hợp lý: lớp nào càng hiếm, bị phạt càng nặng."
    - Chạy code so sánh mô hình gốc và mô hình có `class_weight='balanced'`.
    - Đây là khoảnh khắc "wow". "Chỉ với một tham số, không cần SMOTE, không cần chờ đợi, nhìn vào kết quả xem. Recall lớp 1 tăng vọt. Balanced Accuracy tăng mạnh. Đây là sức mạnh của việc thay đổi luật chơi."
    - Nhấn mạnh: "Đây là phương pháp nhanh nhất, đơn giản nhất và thường rất hiệu quả. **Hãy luôn thử nó đầu tiên!**"

4.  **So sánh các phương pháp - Khi nào dùng gì?**
    - Trình bày bảng so sánh `Resampling (SMOTE)` và `Cost-Sensitive (class_weight)`.
    - **Tốc độ:** `class_weight` nhanh hơn nhiều.
    - **Tính phổ quát:** `SMOTE` hoạt động với mọi thuật toán, `class_weight` thì không.
    - **Độ tin cậy xác suất:** `class_weight` thường giữ được độ hiệu chỉnh tốt hơn `SMOTE`.
    - Đưa ra quy trình làm việc gợi ý: "Bắt đầu với baseline -> Thử `class_weight` -> Thử `SMOTE` -> So sánh và lựa chọn."

**Tổng kết và Chuyển tiếp:**
- "Hôm nay, chúng ta đã hoàn thiện bộ công cụ của mình. Chúng ta không chỉ biết cách sửa dữ liệu (Resampling), mà còn biết cách sửa cả thuật toán (Cost-Sensitive Learning). Chúng ta cũng đã học được cách đánh giá sự 'trung thực' của mô hình qua Calibration."
- "Bạn đã đi từ một người chỉ biết đến Accuracy, đến một người có thể tự tin xử lý một trong những vấn đề phổ biến và khó nhằn nhất trong khoa học dữ liệu. Bạn đã có đủ kiến thức nền tảng."
- "Vậy, làm thế nào để kết hợp tất cả những mảnh ghép này lại trong một dự án thực tế? Trong bài học cuối cùng, chúng ta sẽ không học lý thuyết mới nữa. Thay vào đó, chúng ta sẽ cùng nhau thực hiện một dự án tổng hợp từ A đến Z, áp dụng tất cả những gì đã học để giải quyết một bài toán phân loại gian lận, một kịch bản kinh điển của dữ liệu mất cân bằng."

## 3. Những điểm cần nhấn mạnh bằng ngôn ngữ đơn giản

- "Calibration: Mô hình của bạn có 'nói thật' không?"
- "Cost-Sensitive Learning: Dạy cho mô hình biết 'sai lầm này đắt giá hơn sai lầm kia'."
- "`class_weight='balanced'` là nút bấm 'easy mode'. Hãy bấm nó đầu tiên."
- "Nếu thuật toán của bạn hỗ trợ `class_weight`, hãy thử nó trước khi thử SMOTE. Nó nhanh hơn và đơn giản hơn."

## 4. Những điểm học viên thường bị rối

- **Đọc biểu đồ Calibration:** Cần nhắc lại nhiều lần về ý nghĩa của các trục và đường chéo.
- **Sự khác biệt giữa SMOTE và `class_weight`:** Phép loại suy về "sửa dữ liệu" vs "sửa luật chơi" rất hữu ích.
- **Tại sao `class_weight` lại hiệu quả:** Giải thích rằng nó tăng "gradient" (sức ép sửa lỗi) cho các mẫu thuộc lớp thiểu số trong quá trình tối ưu hóa của thuật toán.

## 5. Phép loại suy hoặc Mô hình tư duy gợi ý

- **Bác sĩ chẩn đoán:** Đã dùng ở trên, hiệu quả cho việc giải thích tầm quan trọng của xác suất.
- **Cha mẹ dạy con:**
    - `Mô hình gốc`: Con làm sai bài nào cũng chỉ bị nhắc nhở nhẹ nhàng.
    - `Cost-Sensitive Learning`: Con làm đổ ly nước (lỗi nhỏ) thì bị nhắc nhở. Con nghịch lửa (lỗi lớn, lớp thiểu số) thì bị phạt nặng. Con sẽ học được phải tránh xa lửa bằng mọi giá.
- **Đầu bếp và công thức:**
    - `SMOTE`: Công thức yêu cầu 1kg thịt và 10g gia vị. Bạn chỉ có 1g gia vị. Bạn cố gắng "chế" ra 9g gia vị nữa từ 1g ban đầu.
    - `Cost-Sensitive Learning`: Bạn vẫn dùng 1kg thịt và 1g gia vị, nhưng bạn thay đổi cách nấu, bạn ninh kỹ hơn, dùng lửa nhỏ hơn để 1g gia vị đó có thể ngấm sâu vào từng thớ thịt.

## 6. Luồng Demo cho Notebook

1.  **Cell 1-4 (Calibration):** Chạy code và tập trung vào việc diễn giải biểu đồ. So sánh sự khác biệt rõ rệt giữa LR và GNB.
2.  **Cell 5-8 (Cost-Sensitive):** Đây là phần chính.
    - Chạy mô hình gốc, cho thấy kết quả tệ.
    - Chạy mô hình với `class_weight='balanced'`. Dừng lại, nhấn mạnh sự cải thiện đáng kể chỉ với một thay đổi nhỏ.
    - Chạy mô hình với `class_weight` tùy chỉnh để minh họa khả năng tinh chỉnh.
3.  **Cell 9 (Bảng so sánh):** Dùng bảng này để tóm tắt lại cuộc đối đầu giữa hai phương pháp chính.
4.  **Cell 10 (Kết luận):** Đọc to phần kết luận và cầu nối, tạo sự hứng khởi cho bài dự án cuối khóa.

## 7. Gợi ý khi nói về ứng dụng thực tế

- "Nhiều mô hình mạnh mẽ như `XGBoost`, `LightGBM` đều hỗ trợ tham số để xử lý mất cân bằng, ví dụ như `scale_pos_weight` trong XGBoost. Nó hoạt động tương tự như `class_weight`."
- "Trong thực tế, việc tinh chỉnh `class_weight` (ví dụ tìm ra tỷ lệ `1:15` hay `1:20` là tốt nhất) thường được thực hiện bằng các kỹ thuật tối ưu hóa siêu tham số như `GridSearchCV` hoặc `RandomizedSearchCV`."
- "Khi trình bày với sếp hoặc khách hàng, việc nói 'Mô hình của tôi có Balanced Accuracy là 85% và đã được hiệu chỉnh tốt' sẽ chuyên nghiệp và đáng tin cậy hơn nhiều so với việc chỉ nói 'Accuracy là 98%'."

## 8. Tổng kết mạnh mẽ và cầu nối đến bài sau

"Qua 7 bài học, chúng ta đã xây dựng một kim tự tháp kiến thức vững chắc. Từ nền móng là hiểu tại sao Accuracy sai, đến việc xây dựng các tầng về chỉ số đánh giá, các kỹ thuật can thiệp vào dữ liệu và thuật toán. Bây giờ là lúc đặt viên đá trên đỉnh kim tự tháp. Bài học tiếp theo sẽ là một thử thách, một cơ hội để bạn tự tay vận dụng tất cả những kỹ năng này vào một kịch bản thực tế. Hãy chuẩn bị sẵn sàng cho dự án cuối khóa, nơi chúng ta sẽ đi từ dữ liệu thô đến một mô hình phân loại gian lận hoàn chỉnh và đáng tin cậy."
