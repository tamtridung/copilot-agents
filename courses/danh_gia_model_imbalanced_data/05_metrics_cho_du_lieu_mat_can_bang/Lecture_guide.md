# Hướng dẫn Giảng dạy - Bài 05: Các Chỉ số Đánh giá cho Dữ liệu Mất cân bằng

## 1. Lời hứa của Bài học (Lesson Promise)

Sau bài học này, học viên sẽ có thể:
- Giải thích được tại sao cần các chỉ số vượt ra ngoài `Accuracy` và `ROC AUC`.
- Tính toán và diễn giải ý nghĩa của `Balanced Accuracy`, `G-Mean`, và `F-beta Score`.
- Lựa chọn được chỉ số đánh giá phù hợp nhất cho một bài toán cụ thể dựa trên mục tiêu kinh doanh (ví dụ: ưu tiên Precision hay Recall).
- Đọc một bảng so sánh các chỉ số và đưa ra quyết định nên dùng chỉ số nào trong tình huống nào.

## 2. Cốt truyện Giảng dạy (Teaching Storyline)

**Mở đầu (Hook) & Kết nối từ bài trước:**
- "Ở bài trước, chúng ta đã đóng vai một 'bác sĩ' chẩn đoán 'căn bệnh' của mô hình trên dữ liệu mất cân bằng. Chúng ta đã thấy `Accuracy` là một nhiệt kế hỏng, và `ROC AUC` đôi khi cho kết quả quá lạc quan. Hôm nay, chúng ta sẽ trang bị cho mình những dụng cụ y tế xịn hơn, những 'máy xét nghiệm' chuyên dụng để đo lường sức khỏe của mô hình một cách chính xác nhất."
- "Câu hỏi đặt ra là: Nếu không dùng Accuracy, vậy chúng ta dùng cái gì để có một con số duy nhất, nhanh chóng so sánh mô hình A với mô hình B? Đó là mục tiêu của bài học hôm nay."

**Chuyển tiếp vào nội dung chính:**
1.  **Balanced Accuracy - Sự công bằng đơn giản:**
    - "Hãy bắt đầu với ý tưởng đơn giản nhất: nếu vấn đề là một lớp bị coi nhẹ, vậy hãy đối xử công bằng với cả hai. Hãy lấy 'điểm' của lớp 0 và 'điểm' của lớp 1 rồi chia đôi."
    - Giới thiệu `Recall` của lớp 0 (Specificity) và `Recall` của lớp 1 (Sensitivity).
    - "Balanced Accuracy chính là trung bình cộng của hai điểm số này."
    - Chạy cell code, so sánh `Accuracy` (0.95) với `Balanced Accuracy` (0.60). "Con số nào trông 'thật' hơn? Rõ ràng 0.60 đã vạch trần được vấn đề là mô hình đang 'học lệch'."
    - Nhấn mạnh: "Đây là sự thay thế trực tiếp và tốt hơn nhiều cho Accuracy."

2.  **G-Mean - Kẻ trừng phạt nghiêm khắc:**
    - "Balanced Accuracy dùng phép cộng, khá 'nhân từ'. Nếu một lớp được 1 điểm, lớp kia được 0.1 điểm, trung bình vẫn là 0.55. Nhưng nếu chúng ta muốn một giám khảo nghiêm khắc hơn thì sao?"
    - Giới thiệu phép "trung bình nhân" (Geometric Mean). Dùng ví dụ đơn giản: `sqrt(10*10) = 10`, nhưng `sqrt(1*10) ≈ 3.16`. "Trung bình nhân bị kéo mạnh về phía số nhỏ hơn."
    - "G-Mean chính là trung bình nhân của Recall hai lớp. Nó chỉ cao khi cả hai cùng cao."
    - Chạy cell code. "Nhìn xem, G-Mean chỉ còn 0.45! Nó còn thấp hơn cả Balanced Accuracy. Nó đang hét lên rằng 'Này, hiệu suất trên một lớp đang rất tệ đấy!'"
    - Nhấn mạnh: "Dùng G-Mean khi bạn không chấp nhận bất kỳ sự đánh đổi nào. Cả hai lớp đều phải tốt."

3.  **F-beta Score - Bộ điều chỉnh linh hoạt:**
    - "Cuộc sống là những sự đánh đổi. Đôi khi chúng ta không cần sự hoàn hảo ở cả hai phía. Đôi khi, bắt sót một tên tội phạm (ưu tiên Recall) quan trọng hơn việc bắt nhầm một người vô tội. Đôi khi, gợi ý một video dở (ưu tiên Precision) lại tệ hơn việc bỏ lỡ một video hay."
    - Giới thiệu lại F1-score là sự cân bằng 50-50 giữa Precision và Recall.
    - Giới thiệu `beta` như một "nút vặn":
        - `beta > 1` (như `beta=2`): "Vặn nút về phía Recall. Tôi muốn bắt được nhiều tội phạm hơn."
        - `beta < 1` (như `beta=0.5`): "Vặn nút về phía Precision. Tôi muốn các gợi ý của mình phải chất lượng."
    - Chạy cell code tính F0.5, F1, F2.
    - Phân tích kết quả: "Với mô hình của chúng ta, Recall rất tệ (0.21) và Precision khá hơn (0.58). Vì vậy, F2-score (ưu tiên Recall) bị điểm rất thấp (0.24), trong khi F0.5-score (ưu tiên Precision) được điểm cao hơn (0.44). Điều này cho chúng ta biết chính xác điểm yếu của mô hình nằm ở đâu."

**Tổng kết và Chuyển tiếp:**
- Trình bày bảng so sánh tổng kết. Đi qua từng hàng, tóm tắt lại điểm mạnh, yếu và trường hợp sử dụng của từng chỉ số.
- "Vậy là chúng ta đã có một bộ công cụ đo lường hoàn chỉnh. Chúng ta biết cách chẩn đoán (bài 4) và giờ chúng ta biết cách đo lường chính xác (bài 5). Nhưng đo xong để đó thì không giải quyết được vấn đề."
- "Câu hỏi tiếp theo và quan trọng nhất là: Làm thế nào để **cải thiện** những con số này? Làm sao để tăng G-Mean từ 0.45 lên 0.8? Làm sao để tăng F2-score từ 0.24 lên 0.7? Đó là lúc chúng ta cần can thiệp vào dữ liệu. Trong bài học tới, chúng ta sẽ học kỹ thuật phổ biến nhất để làm điều đó: **Resampling**."

## 3. Những điểm cần nhấn mạnh bằng ngôn ngữ đơn giản

- "Balanced Accuracy = (Điểm của lớp A + Điểm của lớp B) / 2. Rất công bằng."
- "G-Mean = căn bậc hai của (Điểm của lớp A * Điểm của lớp B). Rất nghiêm khắc, một điểm 0 sẽ kéo cả kết quả về 0."
- "F-beta là F1-score có 'nút vặn'. Vặn về beta=2 nếu bạn sợ bỏ sót. Vặn về beta=0.5 nếu bạn sợ sai lầm."
- "Đừng bao giờ chỉ nhìn vào một con số. Hãy bắt đầu với `classification_report`, sau đó chọn một chỉ số chính để tối ưu."

## 4. Những điểm học viên thường bị rối

- **Sự khác biệt giữa Balanced Accuracy và G-Mean:** Cần dùng ví dụ số cụ thể để thấy G-Mean bị "phạt" nặng hơn như thế nào khi một trong hai thành phần thấp.
- **Ý nghĩa của `beta`:** Liên hệ `beta > 1` với việc "Recall quan trọng hơn" và `beta < 1` với "Precision quan trọng hơn". Lấy ví dụ thực tế (y tế vs. gợi ý sản phẩm) là cách tốt nhất.
- **Khi nào dùng cái gì:** Bảng tổng kết là rất quan trọng. Hãy nhấn mạnh rằng không có câu trả lời "đúng" duy nhất, mà phụ thuộc vào bài toán.

## 5. Phép loại suy hoặc Mô hình tư duy gợi ý

- **Hội đồng giám khảo:**
    - `Accuracy`: Một giám khảo chỉ quan tâm đến số đông.
    - `Balanced Accuracy`: Một giám khảo công bằng, cho mỗi bên một phiếu và lấy trung bình.
    - `G-Mean`: Một giám khảo cực kỳ khó tính, chỉ cho điểm cao nếu cả hai bên cùng xuất sắc.
    - `F-beta Score`: Một giám khảo linh hoạt, có thể được "chỉ thị" trước là "hãy coi trọng ý kiến của bên A hơn bên B".

## 6. Luồng Demo cho Notebook

1.  **Cell 1-4:** Chạy lại phần chuẩn bị dữ liệu và in `classification_report` để nhắc lại vấn đề.
2.  **Cell 5-7 (Balanced Accuracy):** Chạy code, so sánh trực tiếp `Accuracy` và `Balanced Accuracy`. Dừng lại và hỏi "Tại sao lại có sự khác biệt lớn này?". Phân tích phần tính toán thủ công để làm rõ.
3.  **Cell 8-9 (G-Mean):** Chạy code, so sánh `G-Mean` với `Balanced Accuracy` vừa tính. Nhấn mạnh tại sao nó lại thấp hơn.
4.  **Cell 10-11 (F-beta):** Chạy code, hiển thị cả 3 giá trị F-score. Giải thích tại sao F2 thấp nhất và F0.5 cao nhất dựa trên giá trị Precision và Recall của mô hình.
5.  **Cell 12 (Bảng tổng kết):** Dành thời gian đi qua bảng này. Đây là phần kiến thức cô đọng nhất của bài học. Sử dụng nó để tóm tắt và củng cố.

## 7. Gợi ý khi nói về ứng dụng thực tế

- "Khi bạn làm việc với Product Manager, hãy hỏi họ: 'Giữa việc bỏ sót một khách hàng tiềm năng và việc làm phiền một khách hàng không tiềm năng, cái nào tệ hơn? Tệ hơn bao nhiêu lần?'. Câu trả lời sẽ giúp bạn chọn `beta` cho F-beta score."
- "Trong các cuộc thi trên Kaggle về dữ liệu mất cân bằng, `Balanced Accuracy` hoặc `F1-score` (macro-averaged) thường được dùng làm chỉ số đánh giá chính. Biết rõ cách chúng hoạt động sẽ giúp bạn tối ưu đúng mục tiêu."
- "Khi trình bày kết quả, thay vì nói 'Mô hình của em đạt F1-score 0.7', hãy nói 'Mô hình của em đạt F1-score 0.7, có nghĩa là nó đang cân bằng tốt giữa việc tìm ra các trường hợp gian lận và việc không báo động nhầm. Nếu chúng ta muốn tìm ra nhiều ca gian lận hơn nữa, chúng ta có thể tối ưu theo F2-score, nhưng sẽ phải chấp nhận nhiều cảnh báo sai hơn.'"

## 8. Tổng kết mạnh mẽ và cầu nối đến bài sau

"Hôm nay, chúng ta đã nâng cấp toàn bộ 'bảng điều khiển' của mình. Chúng ta không còn bay trong sương mù với những chỉ số sai lệch nữa. Chúng ta đã có những công cụ đo lường chính xác, nghiêm khắc và linh hoạt. Nhưng một bảng điều khiển tốt chỉ cho bạn biết bạn đang bay nhanh hay chậm, nó không giúp bạn tăng tốc. Để thực sự 'tăng tốc' - để cải thiện những chỉ số này - chúng ta cần can thiệp vào 'động cơ'. Trong bài học tiếp theo, chúng ta sẽ mở nắp capo và tìm hiểu về các kỹ thuật Resampling, phương pháp trực tiếp và mạnh mẽ nhất để 'độ' lại bộ dữ liệu của chúng ta, buộc mô hình phải chú ý đến lớp thiểu số."
