# Hướng dẫn Giảng dạy - Bài 01: Giới Thiệu Về Đánh Giá Hiệu Quả Mô Hình

## 1. Lời hứa của Bài học (Lesson Promise)

Sau bài học này, học viên sẽ có thể:
- Giải thích được tầm quan trọng của việc đánh giá mô hình trong một dự án khoa học dữ liệu.
- Phân biệt được các khái niệm cốt lõi: True Positive, True Negative, False Positive, và False Negative.
- Tự tính và giải thích được ý nghĩa của 3 chỉ số cơ bản: Accuracy, Precision, và Recall.
- Nhận biết được trường hợp nào nên ưu tiên Precision và trường hợp nào nên ưu tiên Recall thông qua các ví dụ thực tế.
- Hiểu được tại sao Accuracy là một chỉ số nguy hiểm trên dữ liệu mất cân bằng.

## 2. Cốt truyện Giảng dạy (Teaching Storyline)

**Mở đầu (Hook):**
- Bắt đầu bằng một câu chuyện thực tế. "Hãy tưởng tượng bạn làm việc cho một ngân hàng và xây dựng một mô hình AI để phát hiện giao dịch lừa đảo. Một buổi sáng, sếp của bạn hỏi: 'Mô hình của cậu hoạt động tốt không?'. Bạn sẽ trả lời như thế nào? 'Tốt ạ' không phải là câu trả lời. Chúng ta cần những con số, những bằng chứng cụ thể. Đó là lý do tại sao bài học hôm nay tồn tại."
- Hoặc một ví dụ khác về y tế: "Nếu một mô hình AI chẩn đoán ung thư, 'độ chính xác' của nó có ý nghĩa gì? Nếu nó bỏ sót một bệnh nhân thì sao? Nếu nó chẩn đoán nhầm một người khỏe mạnh thì sao? Chi phí cho mỗi sai lầm là khác nhau."

**Chuyển tiếp vào nội dung chính:**
1.  **Tại sao phải đo lường?** Từ câu chuyện mở đầu, nhấn mạnh rằng "cảm giác" là không đủ. Chúng ta cần các thước đo khách quan để so sánh, cải thiện và tin tưởng vào mô hình.
2.  **Giới thiệu Confusion Matrix:** Đây là "bảng điểm" của mô hình. Đừng chỉ đưa ra định nghĩa. Hãy xây dựng nó một cách trực quan.
    - "Chúng ta có 2 sự thật: Bệnh (Positive) hoặc Không Bệnh (Negative)."
    - "Mô hình của chúng ta cũng đưa ra 2 dự đoán: Dự đoán Bệnh hoặc Dự đoán Không Bệnh."
    - "Khi ghép 2x2 lại, chúng ta có 4 kịch bản." Dùng các ví dụ dễ hiểu để giải thích TP, TN, FP, FN.
3.  **Từ Confusion Matrix đến các chỉ số:**
    - **Accuracy:** Bắt đầu với chỉ số dễ hiểu nhất. "Tỷ lệ đoán đúng là bao nhiêu?". Sau đó, ngay lập tức đưa ra软肋 (điểm yếu) của nó bằng ví dụ về dữ liệu mất cân bằng (99% không bệnh, 1% bệnh). Một mô hình lười biếng chỉ đoán "không bệnh" cũng có accuracy 99%!
    - **Precision và Recall:** Giới thiệu chúng như một cặp bài trùng để giải quyết vấn đề của Accuracy.
        - **Precision:** Gắn nó với từ khóa "chắc chắn". "Khi mô hình của tôi nói 'Positive', tôi muốn nó phải chắc chắn đúng." Dùng ví dụ lọc email spam (FP - email quan trọng vào spam - rất tệ).
        - **Recall:** Gắn nó với từ khóa "không bỏ sót". "Trong tất cả những ca Positive thật sự, mô hình của tôi tìm ra được bao nhiêu?". Dùng ví dụ chẩn đoán bệnh (FN - bỏ sót bệnh nhân - cực kỳ nguy hiểm).

**Demo và Code:**
- Chạy đoạn code Python để hiện thực hóa các khái niệm.
- Cho `y_true` và `y_pred` đơn giản.
- Dùng `sklearn.metrics.confusion_matrix` và `seaborn.heatmap` để trực quan hóa.
- Tính tay các chỉ số từ TP, TN, FP, FN rồi so sánh với kết quả từ `sklearn` để học viên tin tưởng vào công thức.

**Tổng kết và Chuyển tiếp:**
- Tóm tắt lại 3 chỉ số và "câu hỏi" mà mỗi chỉ số trả lời.
- "Accuracy hỏi: Tỷ lệ đúng là bao nhiêu?"
- "Precision hỏi: Trong số những lần tôi đoán 'Có', bao nhiêu lần là đúng?"
- "Recall hỏi: Trong số những cái 'Có' thật sự, tôi tìm được bao nhiêu?"
- Gợi mở: "Rõ ràng, muốn Precision cao thì phải 'khó tính' hơn, nhưng như vậy dễ bỏ sót (Recall thấp). Muốn Recall cao thì phải 'dễ tính' hơn, nhưng lại dễ đoán sai (Precision thấp). Sự đánh đổi này là gì? Chúng ta sẽ tìm hiểu trong bài tiếp theo về Precision-Recall Tradeoff và F1-Score."

## 3. Những điểm cần nhấn mạnh bằng ngôn ngữ đơn giản

- "Confusion matrix không có gì 'nhầm lẫn' cả, nó chỉ là một cái bảng 2x2 cho bạn biết mô hình đúng ở đâu, sai ở đâu."
- "Accuracy giống như điểm trung bình ở trường, dễ tính nhưng không nói lên bạn giỏi môn nào nhất."
- "**Precision cao**: Thà giết nhầm còn hơn bỏ sót... à không, ngược lại! **Thà bỏ sót còn hơn bắt nhầm**. (Don't cry wolf!)"
- "**Recall cao**: **Thà bắt nhầm còn hơn bỏ sót**. (Better safe than sorry!)"
- "Dữ liệu mất cân bằng là kẻ thù của Accuracy."

## 4. Những điểm học viên thường bị rối

- **Lẫn lộn giữa Precision và Recall:** Đây là điểm khó nhất. Hãy lặp đi lặp lại câu hỏi mà mỗi chỉ số trả lời và gắn nó với ví dụ spam/ung thư. Dùng hình ảnh (ví dụ: lưới đánh cá, Recall là bắt được bao nhiêu cá trong hồ, Precision là trong lưới có bao nhiêu % là cá, bao nhiêu % là rác).
- **Trục của Confusion Matrix:** Học viên có thể lẫn lộn giữa trục `Actual` và `Predicted`. Luôn ghi rõ `yticklabels` và `xticklabels` khi vẽ biểu đồ.
- **FP và FN:** Tên gọi hơi ngược. "False Positive" -> Dự đoán là Positive nhưng sai. "False Negative" -> Dự đoán là Negative nhưng sai. Cần giải thích chậm và rõ.

## 5. Phép loại suy hoặc Mô hình tư duy gợi ý

- **Precision/Recall và Lưới đánh cá:**
    - **Cái hồ:** Toàn bộ dữ liệu.
    - **Cá (lớp Positive):** Những thứ bạn muốn bắt.
    - **Rác (lớp Negative):** Những thứ bạn không muốn.
    - **Cái lưới bạn quăng:** Những gì mô hình dự đoán là Positive.
    - **Recall:** Trong tổng số cá trong hồ, bạn bắt được bao nhiêu con? (TP / (TP+FN))
    - **Precision:** Trong cái lưới bạn kéo lên, tỷ lệ cá so với rác là bao nhiêu? (TP / (TP+FP))
    - *Tradeoff:* Lưới mắt to thì Precision cao (chỉ bắt cá to, ít rác) nhưng Recall thấp (lọt cá nhỏ). Lưới mắt nhỏ thì Recall cao (bắt được nhiều cá) nhưng Precision thấp (dính nhiều rác).

## 6. Luồng Demo cho Notebook

1.  **Cell 1-5:** Giới thiệu, giải thích lý thuyết. Chạy từng cell và diễn giải.
2.  **Cell 6 (Code Confusion Matrix):**
    - Chạy code.
    - Chỉ vào output của `confusion_matrix` và đối chiếu với `y_true`, `y_pred` để giải thích từng con số.
    - Chỉ vào biểu đồ heatmap và nhấn mạnh nó chỉ là cách vẽ cho đẹp hơn.
3.  **Cell 7-11 (Code Accuracy, Precision, Recall):**
    - Chạy từng cell code.
    - Với mỗi chỉ số, đọc lại công thức, thay số từ confusion matrix đã tính ở trên vào, tính nhẩm ra kết quả.
    - So sánh kết quả tính nhẩm với output của `sklearn` để xác nhận.
    - Nhấn mạnh lại ý nghĩa của con số: "Precision 0.8 nghĩa là gì? Nghĩa là trong 5 lần mô hình đoán là 1, có 4 lần đúng."

## 7. Gợi ý khi nói về ứng dụng thực tế

- "Trong ngành tài chính, mô hình duyệt vay tín dụng sẽ ưu tiên Precision. Họ không muốn cấp khoản vay cho người không có khả năng trả (FP)."
- "Trong kiểm soát chất lượng sản xuất, mô hình phát hiện sản phẩm lỗi sẽ ưu tiên Recall. Họ không muốn để lọt sản phẩm lỗi đến tay khách hàng (FN)."
- "Trong marketing, khi gửi coupon giảm giá cho khách hàng tiềm năng, bạn có thể chấp nhận Precision thấp hơn một chút để có Recall cao hơn, tiếp cận được nhiều người hơn, vì chi phí cho một FP (gửi nhầm coupon) là không lớn."

## 8. Tổng kết mạnh mẽ và cầu nối đến bài sau

"Hôm nay chúng ta đã học được cách 'đọc vị' một mô hình phân loại qua confusion matrix và 3 chỉ số quan trọng. Chúng ta đã thấy Accuracy có thể 'nói dối' và Precision/Recall cho chúng ta một cái nhìn sâu sắc hơn về hiệu quả của mô hình trong các bối cảnh khác nhau. Nhưng bạn có nhận thấy không? Cải thiện Precision thường làm giảm Recall và ngược lại. Làm thế nào để cân bằng chúng? Có chỉ số nào kết hợp cả hai không? Đó chính là nội dung của bài học tiếp theo, nơi chúng ta sẽ khám phá về F1-Score và Precision-Recall Curve."
