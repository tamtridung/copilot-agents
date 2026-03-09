# Hướng dẫn Giảng dạy - Bài 04: Vấn đề Mất cân bằng Dữ liệu

## 1. Lời hứa của Bài học (Lesson Promise)

Sau bài học này, học viên sẽ có thể:
- Định nghĩa được dữ liệu mất cân bằng và nhận ra nó trong các bài toán thực tế.
- Giải thích được gốc rễ tại sao các mô hình học máy tiêu chuẩn lại thất bại trên dữ liệu mất cân bằng.
- Phân tích một `classification_report` để chẩn đoán vấn đề, đặc biệt là nhìn vào chỉ số Recall của lớp thiểu số.
- Hiểu được tại sao Accuracy và ROC AUC có thể "nói dối" và tại sao Precision-Recall curve lại đáng tin cậy hơn trong bối cảnh này.

## 2. Cốt truyện Giảng dạy (Teaching Storyline)

**Mở đầu (Hook) & Kết nối từ bài trước:**
- "Trong suốt 3 bài học vừa qua, chúng ta đã liên tục nhắc đến một 'nhân vật phản diện' có tên là 'dữ liệu mất cân bằng'. Chúng ta đã thấy nó có thể làm Accuracy trở nên vô dụng, có thể đánh lừa cả ROC AUC. Hôm nay, chúng ta sẽ đưa 'kẻ phản diện' này ra ánh sáng. Nó thực sự là gì? Nó đến từ đâu? Và tại sao nó lại nguy hiểm đến vậy?"
- Bắt đầu bằng một câu hỏi tương tác: "Hãy kể tên một bài toán mà bạn nghĩ rằng việc thu thập dữ liệu cho 'có' và 'không' là không đều nhau." (Gợi ý: phát hiện bệnh hiếm, phát hiện gian lận, click quảng cáo...).

**Chuyển tiếp vào nội dung chính:**
1.  **Định nghĩa và Trực quan hóa:**
    - Đưa ra định nghĩa chính thức: sự chênh lệch lớn về số lượng mẫu giữa các lớp. Giới thiệu thuật ngữ "lớp đa số" (majority) và "lớp thiểu số" (minority).
    - "Đây không phải là lỗi, đây là bản chất của rất nhiều bài toán trong thực tế. Những sự kiện hiếm thường là những sự kiện chúng ta quan tâm nhất."
    - Chạy cell code để tạo dữ liệu giả lập và vẽ biểu đồ. "Nhìn xem, lớp 1 chỉ là một chấm nhỏ trên bản đồ. Làm sao mô hình có thể chú ý đến nó?"

2.  **Giải thích "Tại sao?": Tâm lý của Mô hình:**
    - Dùng phép loại suy về một học sinh lười biếng. "Hãy tưởng tượng một kỳ thi có 100 câu hỏi, 99 câu là dạng A và chỉ 1 câu là dạng B. Để được 99 điểm, học sinh chỉ cần học tủ dạng A và bỏ qua hoàn toàn dạng B. Các thuật toán của chúng ta, vì muốn tối thiểu hóa lỗi (tối đa hóa điểm), cũng hành xử 'lười biếng' y như vậy."
    - Giải thích ở mức độ kỹ thuật hơn: Hàm mất mát (loss function) tính tổng lỗi trên toàn bộ tập dữ liệu. Lỗi từ việc đoán sai lớp thiểu số chỉ là một phần nhỏ trong tổng lỗi, nên mô hình không có đủ "sức ép" để học cách nhận diện chúng.

3.  **Thí nghiệm thực tế - Vạch trần sự thật:**
    - Chạy cell code huấn luyện `LogisticRegression` trên dữ liệu mất cân bằng.
    - Nhìn vào kết quả `classification_report`. Đây là khoảnh khắc "aha!".
    - **Bước 1:** "Nhìn vào accuracy, 95%! Trông có vẻ tuyệt vời!"
    - **Bước 2:** "Nhưng hãy nhìn kỹ hơn. Đi sâu vào chi tiết của lớp `1`."
    - **Bước 3:** "Precision có thể cao, nhưng hãy nhìn vào **Recall**: 0.20! Có nghĩa là mô hình đã **bỏ lót 80%** những gì chúng ta thực sự muốn tìm! Đây là một thảm họa."
    - Nhấn mạnh: "Đây là triệu chứng kinh điển. Accuracy cao, nhưng Recall của lớp thiểu số cực thấp."

4.  **Kiểm tra lại các công cụ đánh giá:**
    - "Bây giờ chúng ta đã biết vấn đề, hãy xem lại các công cụ của chúng ta hoạt động ra sao trong môi trường khắc nghiệt này."
    - **Accuracy:** "Vô dụng. Chúng ta chính thức 'sa thải' nó khỏi các bài toán mất cân bằng."
    - **ROC AUC:** Chạy code và cho thấy AUC vẫn có thể rất cao. Giải thích lại lý do (FPR khó tăng). "Nó cho chúng ta một cảm giác an toàn giả tạo."
    - **Precision-Recall Curve:** Chạy code và vẽ P-R curve. "Nhìn xem, đường cong này trông 'khiêm tốn' hơn nhiều. Nó cho thấy rõ để có được một chút Recall, Precision đã giảm đi như thế nào. Đây mới là sự thật."
    - So sánh song song hai biểu đồ ROC và PR để thấy sự khác biệt rõ rệt.

**Tổng kết và Chuyển tiếp:**
- Tóm tắt lại: Dữ liệu mất cân bằng là phổ biến, nó làm các mô hình thiên vị lớp đa số, dẫn đến Recall thấp ở lớp thiểu số. Accuracy và ROC AUC có thể che giấu vấn đề này, trong khi `classification_report` (chi tiết từng lớp) và P-R Curve là những người bạn đáng tin cậy.
- "Chúng ta đã chẩn đoán được 'căn bệnh'. Vậy 'thuốc chữa' là gì? Rất may, cộng đồng khoa học dữ liệu đã phát triển nhiều kỹ thuật để chống lại sự mất cân bằng. Trong bài học tiếp theo, chúng ta sẽ bắt đầu với nhóm giải pháp đầu tiên: sử dụng các chỉ số đánh giá được thiết kế riêng cho những bài toán như thế này."

## 3. Những điểm cần nhấn mạnh bằng ngôn ngữ đơn giản

- "Mô hình học máy giống như một chính trị gia, nó muốn làm hài lòng số đông (lớp đa số) và thường bỏ qua tiếng nói của thiểu số."
- "Trong dữ liệu mất cân bằng, Accuracy là 'kẻ nói dối'."
- "Luôn luôn nhìn vào `classification_report`. Con số quan trọng nhất thường là **Recall của lớp 1**."
- "ROC curve giống như một bản báo cáo PR lạc quan. P-R curve là một cuộc kiểm toán tài chính khắc nghiệt."

## 4. Những điểm học viên thường bị rối

- **Tại sao mô hình lại "chọn" bỏ qua lớp thiểu số:** Khái niệm về tối ưu hóa hàm mất mát có thể hơi trừu tượng. Phép loại suy về học sinh lười/chính trị gia thường giúp làm rõ.
- **Tại sao ROC AUC lại cao:** Cần giải thích lại công thức của FPR và ảnh hưởng của mẫu số rất lớn (tổng số lớp Negative).
- **`stratify=y`:** Nhiều người quên tham số này. Cần nhấn mạnh tầm quan trọng của nó để đảm bảo việc đánh giá là đáng tin cậy.

## 5. Phép loại suy hoặc Mô hình tư duy gợi ý

- **Học sinh lười biếng:** Đã dùng ở trên, rất hiệu quả.
- **Đánh bắt cá:** Bạn muốn bắt một loài cá hiếm (lớp thiểu số) trong một cái hồ đầy cá tạp (lớp đa số). Nếu bạn dùng một cái lưới mắt rộng (mô hình tiêu chuẩn), bạn sẽ bắt được rất nhiều cá tạp và có vẻ như bạn đã làm việc hiệu quả (accuracy cao), nhưng thực ra bạn đã để lọt gần hết số cá hiếm.

## 6. Luồng Demo cho Notebook

1.  **Cell 1-4:** Giới thiệu, định nghĩa, và trực quan hóa. Chạy và diễn giải.
2.  **Cell 5-7 (Thí nghiệm):** Đây là phần cốt lõi.
    - Chạy cell code.
    - Dành thời gian phân tích `classification_report`. Khoanh tròn hoặc chỉ rõ vào các con số: accuracy, recall của lớp 1, f1-score của lớp 1. So sánh sự tương phản.
3.  **Cell 8-10 (So sánh các công cụ đánh giá):**
    - Chạy cell code để tính AUC và vẽ 2 biểu đồ.
    - Đặt 2 biểu đồ cạnh nhau. "Bên trái, ROC curve, AUC=0.92, trông rất tuyệt. Bên phải, P-R curve, AUC=0.58, trông thực tế hơn nhiều. Bạn tin ai?"

## 7. Gợi ý khi nói về ứng dụng thực tế

- "Đây là lý do tại sao trong các dự án thực tế, bước đầu tiên sau khi làm sạch dữ liệu là `df['target'].value_counts(normalize=True)`. Luôn luôn kiểm tra tỷ lệ các lớp trước khi làm bất cứ điều gì khác."
- "Khi bạn báo cáo kết quả cho sếp về một bài toán mất cân bằng, đừng bao giờ bắt đầu bằng Accuracy. Hãy bắt đầu bằng việc giải thích bối cảnh mất cân bằng và đi thẳng vào Precision/Recall của lớp thiểu số."
- "Nhiều công ty xây dựng các hệ thống cảnh báo (alerting systems) dựa trên mô hình. Nếu mô hình có Recall thấp trên lớp thiểu số (ví dụ: sự cố hệ thống), hệ thống cảnh báo đó gần như vô dụng."

## 8. Tổng kết mạnh mẽ và cầu nối đến bài sau

"Hôm nay, chúng ta đã nhìn thẳng vào 'kẻ thù'. Chúng ta đã hiểu bản chất, sự nguy hiểm và cách nó qua mặt các công cụ đánh giá thông thường. Nhận diện được vấn đề là bước đầu tiên và quan trọng nhất. Từ bây giờ, mỗi khi gặp một bài toán phân loại, câu hỏi đầu tiên trong đầu bạn phải là 'Dữ liệu có mất cân bằng không?'. Khi đã biết vấn đề, chúng ta cần các công cụ tốt hơn để đo lường và sau đó là các kỹ thuật để khắc phục. Trong bài học tiếp theo, chúng ta sẽ học về các chỉ số đánh giá được 'tinh chỉnh' đặc biệt để hoạt động tốt ngay cả khi dữ liệu mất cân bằng, như Balanced Accuracy và G-Mean."
