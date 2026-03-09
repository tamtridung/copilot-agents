# Hướng dẫn Giảng dạy - Bài 06: Kỹ thuật Lấy mẫu lại (Resampling)

## 1. Lời hứa của Bài học (Lesson Promise)

Sau bài học này, học viên sẽ có thể:
- Giải thích được nguyên tắc vàng của resampling: **chỉ áp dụng trên tập train**.
- Áp dụng được 3 kỹ thuật resampling phổ biến: `RandomUnderSampler`, `RandomOverSampler`, và `SMOTE` bằng thư viện `imbalanced-learn`.
- Phân tích và so sánh kết quả của mô hình trước và sau khi resampling, đặc biệt là sự đánh đổi giữa Recall của hai lớp.
- Trình bày được ưu và nhược điểm của từng phương pháp và biết khi nào nên chọn phương pháp nào làm điểm khởi đầu.

## 2. Cốt truyện Giảng dạy (Teaching Storyline)

**Mở đầu (Hook) & Kết nối từ bài trước:**
- "Ở các bài trước, chúng ta đã trở thành chuyên gia trong việc 'đo lường' và 'chẩn đoán' vấn đề. Chúng ta biết mô hình của mình đang 'ốm' nặng. Hôm nay, chúng ta sẽ bắt đầu 'chữa bệnh'. Đây là bài học thực hành và can thiệp đầu tiên để thực sự cải thiện mô hình."
- "Ý tưởng rất đơn giản: Nếu mô hình học kém vì 'thầy' (dữ liệu) quá thiên vị, vậy thì hãy thay đổi 'thầy' trước. Chúng ta sẽ 'dạy lại' cho dữ liệu của mình cách trở nên công bằng hơn."

**Chuyển tiếp vào nội dung chính:**
1.  **Nguyên tắc Vàng - The Golden Rule:**
    - Đây là phần **quan trọng nhất**, phải được nhấn mạnh đầu tiên và lặp lại nhiều lần.
    - "Trước khi học bất kỳ kỹ thuật nào, có một quy tắc bạn phải khắc cốt ghi tâm, một lời thề bạn phải thực hiện. Đó là: **CHỈ RESAMPLE TRÊN TẬP TRAIN.**"
    - Dùng phép loại suy: "Hãy tưởng tượng bạn đang luyện thi. Bạn có thể dùng mọi loại 'phao', học tủ, học mẹo trên các bộ đề luyện tập (tập train). Nhưng khi vào phòng thi thật (tập test), bạn phải đối mặt với đề thi nguyên bản. Nếu bạn 'resample' cả tập test, nó giống như bạn biết trước đề thi và tự sửa lại đề cho dễ hơn. Kết quả 10 điểm của bạn lúc đó là hoàn toàn vô nghĩa."
    - Vẽ sơ đồ quy trình đúng: `Data -> Split -> Resample Train Set -> Train Model -> Evaluate on Original Test Set`.

2.  **Undersampling - Bớt đi cho cân bằng:**
    - "Phương pháp đầu tiên, đơn giản nhất: lớp đa số quá đông? Hãy 'mời' bớt một số người ra ngoài cho đến khi hai bên bằng nhau."
    - Giới thiệu `RandomUnderSampler`.
    - Chạy code và cho thấy sự thay đổi về số lượng mẫu. "Tập train của chúng ta đã nhỏ đi đáng kể."
    - Huấn luyện mô hình và đánh giá. Đây là khoảnh khắc "aha" thứ hai.
    - Phân tích `classification_report`: "Nhìn này! Recall của lớp 1 đã tăng vọt! Nhưng hãy nhìn vào Recall của lớp 0, nó đã giảm xuống. Đây là một **sự đánh đổi (trade-off)**. Chúng ta đã hy sinh một chút hiệu suất trên lớp đa số để đạt được sự cải thiện lớn trên lớp thiểu số."
    - Thảo luận về nhược điểm: "Cái giá phải trả là gì? Chúng ta đã vứt đi rất nhiều dữ liệu. Có thể chúng ta đã vô tình vứt đi những 'viên ngọc quý', những thông tin quan trọng."

3.  **Oversampling - Thêm vào cho cân bằng:**
    - "Phương pháp thứ hai: lớp thiểu số quá ít? Hãy 'nhân bản' họ lên."
    - Giới thiệu `RandomOverSampler`.
    - Chạy code và cho thấy sự thay đổi. "Bây giờ tập train của chúng ta lớn hơn nhiều."
    - Huấn luyện và đánh giá. Kết quả thường tương tự Undersampling.
    - Thảo luận về nhược điểm: "Nghe có vẻ tốt hơn vì không mất dữ liệu, nhưng có một nguy cơ tiềm ẩn. Bằng cách nhân bản, chúng ta đang cho mô hình ăn đi ăn lại cùng một món. Nó có thể bị 'học vẹt' (overfitting) và không thể nhận ra các món ăn mới lạ trong thực tế."

4.  **SMOTE - Sự nhân bản thông minh:**
    - "Random Oversampling hơi 'ngây thơ'. Nếu có một cách 'nhân bản' thông minh hơn thì sao? Thay vì tạo ra các bản sao y hệt, chúng ta tạo ra những người 'anh em sinh đôi' có một chút khác biệt."
    - Giới thiệu ý tưởng của SMOTE một cách trực quan: "Nó không copy, nó 'lai tạo'. Nó tìm hai điểm gần nhau và tạo ra một điểm mới nằm giữa chúng."
    - Dùng hình ảnh minh họa để làm rõ. "SMOTE giúp lấp đầy không gian giữa các điểm dữ liệu của lớp thiểu số, tạo ra một vùng nhận dạng lớn hơn và liền mạch hơn."
    - Chạy code, huấn luyện và đánh giá.
    - So sánh kết quả của SMOTE với hai phương pháp trước. "Thông thường, SMOTE sẽ cho kết quả tốt nhất. Nó cân bằng được việc cải thiện Recall lớp 1 mà không phá hủy quá nhiều Recall lớp 0, và ít bị overfitting hơn."

**Tổng kết và Chuyển tiếp:**
- Trình bày bảng so sánh 3 kỹ thuật. Nhấn mạnh SMOTE là "điểm khởi đầu tốt nhất" trong hầu hết các trường hợp.
- "Vậy là chúng ta đã có trong tay một bộ công cụ cực kỳ mạnh mẽ để can thiệp vào dữ liệu. Resampling là một trong những phương pháp hiệu quả nhất và được sử dụng rộng rãi nhất."
- "Nhưng liệu có cách nào khác không? Thay vì thay đổi dữ liệu, liệu chúng ta có thể 'dạy' cho chính thuật toán biết rằng việc đoán sai lớp thiểu số sẽ bị 'phạt' nặng hơn không? Câu trả lời là có. Đó là triết lý của **Cost-Sensitive Learning**, và chúng ta sẽ khám phá nó trong bài học cuối cùng của chương này."

## 3. Những điểm cần nhấn mạnh bằng ngôn ngữ đơn giản

- "Luật số 1 của Resampling Club: Bạn không được resample tập test."
- "Undersampling: Vứt bớt dữ liệu đa số. Nhanh nhưng có thể vứt nhầm hàng tốt."
- "Oversampling: Copy-paste dữ liệu thiểu số. Dễ nhưng mô hình dễ học vẹt."
- "SMOTE: 'Lai tạo' dữ liệu thiểu số. Thông minh hơn, hiệu quả hơn, hãy thử nó đầu tiên."
- "Mọi kỹ thuật resampling đều là một sự đánh đổi. Bạn tăng Recall lớp 1 và thường sẽ phải chấp nhận giảm Recall lớp 0."

## 4. Những điểm học viên thường bị rối

- **Quy trình `split` rồi `resample`:** Đây là lỗi phổ biến nhất. Cần phải kiểm tra lại và nhấn mạnh nhiều lần. Bài tập thực hành về sửa lỗi code là rất quan trọng.
- **Tại sao Recall lớp 0 lại giảm:** Học viên có thể nghĩ rằng mô hình chỉ tốt lên. Cần giải thích rằng khi mô hình bớt "thiên vị" lớp 0, nó cũng trở nên ít quyết đoán hơn với lớp 0 và có thể phân loại nhầm một số mẫu lớp 0 thành lớp 1.
- **SMOTE hoạt động như thế nào:** Không cần đi quá sâu vào công thức toán học, chỉ cần dùng hình ảnh và phép loại suy "lai tạo" là đủ để học viên nắm được ý tưởng.

## 5. Phép loại suy hoặc Mô hình tư duy gợi ý

- **Luyện thi:** Đã dùng ở trên, rất hiệu quả cho việc giải thích "không resample tập test".
- **Nấu ăn:**
    - `Undersampling`: Món súp quá mặn (lớp đa số). Bạn múc bớt nước súp đổ đi và thêm nước lọc. Súp nhạt hơn nhưng cũng mất đi vị ngọt của xương.
    - `Oversampling`: Món súp quá nhạt (lớp thiểu số). Bạn chỉ có một ít gia vị, bạn cứ cho đi cho lại đúng một loại gia vị đó. Súp đậm hơn nhưng vị rất đơn điệu.
    - `SMOTE`: Món súp quá nhạt. Bạn lấy một ít gia vị, kết hợp chúng với nhau để tạo ra một loại gia vị tổng hợp mới rồi cho vào. Súp đậm đà và có chiều sâu hơn.

## 6. Luồng Demo cho Notebook

1.  **Cell 1-4:** Chuẩn bị, nhấn mạnh tầm quan trọng của `imblearn` và quy tắc vàng.
2.  **Cell 5-7 (Undersampling):** Chạy, phân tích sự thay đổi kích thước, rồi phân tích `classification_report`. So sánh trực tiếp với kết quả của mô hình gốc (có thể chạy lại ở đầu notebook).
3.  **Cell 8-10 (Oversampling):** Tương tự, chạy, phân tích kích thước, phân tích report.
4.  **Cell 11-13 (SMOTE):** Tương tự, chạy, phân tích, và so sánh kết quả với cả hai phương pháp trên. Đây là phần quan trọng nhất, hãy chỉ ra tại sao các chỉ số của SMOTE thường tốt hơn.
5.  **Cell 14 (Bảng tổng kết):** Dùng bảng này để củng cố lại kiến thức và đưa ra lời khuyên thực tế.

## 7. Gợi ý khi nói về ứng dụng thực tế

- "Trong `scikit-learn`, có một công cụ gọi là `Pipeline`. Bạn có thể kết hợp một bước resampler (như SMOTE) và một bước model (như LogisticRegression) vào chung một pipeline. Điều này đảm bảo rằng khi bạn thực hiện cross-validation, việc resampling sẽ được áp dụng đúng cách bên trong mỗi fold của tập train, tránh rò rỉ dữ liệu. Đây là cách làm chuyên nghiệp nhất."
- "Các biến thể của SMOTE như `BorderlineSMOTE`, `SVMSMOTE`, `ADASYN` giải quyết một số nhược điểm của SMOTE gốc (như tạo nhiễu). Khi bạn đã thành thạo SMOTE, hãy tìm hiểu thêm về chúng. Chúng đều có trong `imblearn`."
- "Đừng mù quáng tin vào kết quả của một phương pháp. Hãy thử nghiệm vài phương pháp (ít nhất là Undersampling và SMOTE) và chọn ra mô hình cho kết quả tốt nhất trên tập test của bạn."

## 8. Tổng kết mạnh mẽ và cầu nối đến bài sau

"Hôm nay, chúng ta đã học cách 'phẫu thuật' bộ dữ liệu của mình, cân bằng lại nó để mô hình có cơ hội học tập công bằng. Resampling là một kỹ năng không thể thiếu của bất kỳ nhà khoa học dữ liệu nào khi đối mặt với các bài toán trong thế giới thực. Nhưng đây mới chỉ là một trong hai cách tiếp cận chính. Cách tiếp cận còn lại không đụng đến dữ liệu, mà thay đổi chính 'bộ não' của mô hình. Trong bài học cuối cùng, chúng ta sẽ tìm hiểu về Cost-Sensitive Learning, nơi chúng ta 'dạy' cho mô hình biết rằng 'sai lầm này đắt giá hơn sai lầm kia', và xem nó sẽ thay đổi cuộc chơi như thế nào."
