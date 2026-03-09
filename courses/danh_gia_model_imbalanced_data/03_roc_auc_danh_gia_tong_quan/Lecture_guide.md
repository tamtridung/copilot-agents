# Hướng dẫn Giảng dạy - Bài 03: Đánh giá Tổng quan với ROC và AUC

## 1. Lời hứa của Bài học (Lesson Promise)

Sau bài học này, học viên sẽ có thể:
- Định nghĩa và phân biệt được True Positive Rate (TPR) và False Positive Rate (FPR).
- Đọc và diễn giải được một đường cong ROC để đánh giá khả năng phân tách của mô hình.
- Giải thích được ý nghĩa của chỉ số AUC như một thước đo tổng quan, không phụ thuộc vào ngưỡng.
- Đưa ra quyết định khi nào nên dùng ROC/AUC và khi nào nên dùng Precision-Recall curve, đặc biệt trong bối cảnh dữ liệu mất cân bằng.

## 2. Cốt truyện Giảng dạy (Teaching Storyline)

**Mở đầu (Hook) & Kết nối từ bài trước:**
- "Ở bài trước, chúng ta đã học cách 'vặn nút' (ngưỡng quyết định) để cân bằng giữa Precision và Recall. Nhưng việc này giống như việc bạn phải quyết định xem nên dùng kính hiển vi hay ống nhòm. Mỗi cái cho một góc nhìn khác nhau. Câu hỏi hôm nay là: 'Liệu có cách nào để đánh giá 'sức mạnh' của cả cái máy ảnh, bất kể bạn lắp ống kính nào vào không?' Chúng ta muốn một con số duy nhất nói lên khả năng phân biệt cốt lõi của mô hình."
- "Hãy tưởng tượng bạn có hai mô hình. Thay vì hỏi 'Ở ngưỡng 0.5, mô hình nào tốt hơn?', chúng ta muốn hỏi 'Nhìn chung, mô hình nào có khả năng tách biệt tốt hơn giữa chó và mèo?'"

**Chuyển tiếp vào nội dung chính:**
1.  **Giới thiệu cặp đối thủ mới: TPR vs FPR:**
    - "Để có góc nhìn mới, chúng ta cần hai chỉ số mới. Đầu tiên là **TPR - True Positive Rate**. Tin vui là các bạn đã biết nó rồi, nó chính là **Recall**." -> Tạo sự quen thuộc. "Nó vẫn đo lường khả năng 'vớt' được các ca Positive."
    - "Đối thủ của nó là **FPR - False Positive Rate**." Giải thích công thức `FP / (FP + TN)`. Dùng ngôn ngữ đơn giản: "Trong tất cả những người khỏe mạnh, có bao nhiêu phần trăm bị mô hình báo động nhầm là 'bệnh'?". Đây là **tỷ lệ báo động giả (false alarm rate)**.
    - "Một mô hình tốt sẽ muốn 'vớt' được nhiều (TPR cao) và 'báo động giả' ít (FPR thấp)."

2.  **Vẽ nên bức tranh toàn cảnh - Đường cong ROC:**
    - Giới thiệu ROC là viết tắt của Receiver Operating Characteristic - một cái tên lịch sử từ thời radar, không cần quá tập trung vào tên gọi.
    - "Đường cong ROC vẽ cuộc đối đầu giữa khả năng 'vớt' (TPR) và tỷ lệ 'báo động giả' (FPR) ở mọi ngưỡng có thể."
    - **Giải thích các điểm đặc biệt trên biểu đồ:**
        - **Góc trên bên trái (0,1):** "Thiên đường. Vớt được 100% ca bệnh mà không báo động nhầm ai." -> Mô hình hoàn hảo.
        - **Đường chéo:** "Sự tầm thường. Một mô hình tung đồng xu. Muốn vớt được 50% ca bệnh, nó cũng báo động nhầm 50% người khỏe. Vô dụng."
        - **Góc dưới bên phải (1,0):** "Địa ngục. Báo động nhầm 100% người khỏe mà không vớt được ca bệnh nào."
    - Chạy cell code vẽ ROC curve. "Đường cong của chúng ta càng phình to về phía 'thiên đường', mô hình càng xịn."

3.  **Một con số để thống trị tất cả - AUC:**
    - "Nhìn đường cong thì cũng hay đấy, nhưng để so sánh 2-3 mô hình với nhau thì hơi rối mắt. Chúng ta cần một con số."
    - Giới thiệu **AUC - Area Under the Curve**. "Nó đơn giản là diện tích của phần nằm dưới đường cong ROC."
    - **Giải thích ý nghĩa của AUC:**
        - AUC = 1: Mô hình hoàn hảo.
        - AUC = 0.5: Mô hình vô dụng như tung đồng xu.
        - Dùng cách diễn giải trực quan nhất: "AUC = 0.9 có nghĩa là: nếu bạn nhặt ngẫu nhiên một người bệnh và một người khỏe, có 90% khả năng mô hình của bạn sẽ xếp hạng người bệnh cao hơn người khỏe. Đây là một thước đo rất hay về khả năng 'xếp hạng' đúng."

4.  **Cuộc đối đầu cuối cùng: ROC/AUC vs. Precision-Recall:**
    - Đây là phần quan trọng nhất của bài học.
    - **Kịch bản 1: Dữ liệu cân bằng.** Dùng ROC/AUC. Nó cho cái nhìn tổng quan, ổn định.
    - **Kịch bản 2: Dữ liệu mất cân bằng (phát hiện gian lận).**
        - Dùng ví dụ sốc: 1 triệu giao dịch, chỉ 100 gian lận.
        - Mô hình A: Dự đoán đúng 90/100 ca gian lận (TP=90), nhưng báo động nhầm 1000 giao dịch thường (FP=1000).
        - **Tính toán:**
            - TPR (Recall) = 90/100 = 0.9 (Rất cao!).
            - FPR = 1000 / (1000 + 999000-1000) ≈ 0.001 (Rất thấp!).
            - -> Điểm (FPR, TPR) trên ROC curve sẽ là (0.001, 0.9), rất gần "thiên đường". AUC sẽ rất cao!
            - **NHƯNG**, Precision = 90 / (90 + 1000) ≈ 0.08 (Cực kỳ thấp!). Cứ 100 báo động thì chỉ 8 cái đúng.
        - **Kết luận:** ROC/AUC có thể quá "lạc quan" trên dữ liệu mất cân bằng. Precision-Recall curve sẽ "vạch trần" sự thật về hiệu năng trên lớp thiểu số.

**Tổng kết và Chuyển tiếp:**
- Tóm tắt lại: ROC/AUC đo khả năng phân tách tổng thể, tuyệt vời cho việc so sánh mô hình và dữ liệu cân bằng. P-R Curve là "chuyên gia" cho dữ liệu mất cân bằng và khi bạn quan tâm đến lớp Positive.
- "Chúng ta đã trang bị đầy đủ vũ khí để đánh giá mô hình trong mọi tình huống. Giờ là lúc đối mặt với kẻ thù chính: dữ liệu mất cân bằng. Nó là gì, tại sao nó lại là vấn đề, và nó trông như thế nào? Mời các bạn đến với bài học tiếp theo."

## 3. Những điểm cần nhấn mạnh bằng ngôn ngữ đơn giản

- "TPR là Recall. Đừng hoảng sợ, nó là người quen."
- "FPR là tỷ lệ báo động giả. Càng thấp càng tốt."
- "ROC là cuộc chiến giữa 'bắt được bao nhiêu' (TPR) và 'báo nhầm bao nhiêu' (FPR)."
- "AUC là điểm số cuối cùng cho khả năng phân biệt của mô hình. Càng gần 1 càng tốt."
- "Dữ liệu mất cân bằng? Hãy cẩn thận với AUC. Hãy gọi chuyên gia P-R Curve ra."

## 4. Những điểm học viên thường bị rối

- **Nhớ công thức FPR:** Học viên đã quen với Precision/Recall, FPR là một khái niệm mới. Cần nhấn mạnh mẫu số của nó là `FP + TN` (tổng số Negative thực sự).
- **Tại sao AUC lại tốt:** Ý tưởng về "độc lập với ngưỡng" có thể hơi trừu tượng. Cách giải thích về "xác suất xếp hạng đúng" thường dễ hiểu hơn.
- **Khi nào dùng cái nào:** Đây là câu hỏi kinh điển. Hãy đóng khung nó lại:
    - **So sánh tổng quan, dữ liệu cân bằng -> ROC/AUC.**
    - **Lớp Positive hiếm và quan trọng -> P-R Curve.**

## 5. Phép loại suy hoặc Mô hình tư duy gợi ý

- **Hệ thống an ninh sân bay:**
    - **Hành khách:** Toàn bộ người đi qua cổng an ninh.
    - **Kẻ xấu (Positive):** Người mang vật cấm.
    - **Người thường (Negative):** Hành khách bình thường.
    - **Máy quét:** Mô hình.
    - **TPR (Recall):** Trong số tất cả kẻ xấu, máy quét phát hiện được bao nhiêu?
    - **FPR:** Trong số tất cả người thường, có bao nhiêu người bị máy kêu "bíp" oan?
    - **ROC Curve:** Vẽ biểu đồ hiệu năng khi bạn chỉnh độ nhạy của máy quét. Độ nhạy cao -> TPR cao, FPR cũng cao. Độ nhạy thấp -> TPR thấp, FPR cũng thấp.
    - **AUC:** "Sức mạnh" tổng thể của cái máy quét này, bất kể bạn chỉnh độ nhạy thế nào.

## 6. Luồng Demo cho Notebook

1.  **Cell 1-4:** Giới thiệu lý thuyết về TPR, FPR, ROC.
2.  **Cell 5 (Code vẽ ROC):**
    - Chạy code.
    - Chỉ vào đường cong: "Đây là hiệu năng của mô hình chúng ta."
    - Chỉ vào đường chéo: "Nó tốt hơn hẳn việc tung đồng xu."
    - Chỉ vào góc trên bên trái: "Nó vẫn còn một khoảng cách để trở nên hoàn hảo."
3.  **Cell 6-7 (Code tính và vẽ AUC):**
    - Chạy code.
    - "Và con số cho diện tích này là 0.90. Một điểm số khá tốt."
4.  **Cell 8 (Thảo luận ROC vs PR):** Đây là cell markdown quan trọng. Dành thời gian giải thích kỹ lưỡng ví dụ về dữ liệu mất cân bằng, có thể viết các con số ra bảng trắng để học viên theo dõi.

## 7. Gợi ý khi nói về ứng dụng thực tế

- "Khi so sánh các mô hình nền tảng (off-the-shelf) như Logistic Regression, Random Forest, XGBoost, người ta thường dùng AUC làm chỉ số đầu tiên để xem mô hình nào có tiềm năng nhất."
- "Trong các cuộc thi trên Kaggle, AUC thường là một trong những chỉ số đánh giá chính vì nó công bằng và không phụ thuộc vào việc thí sinh chọn ngưỡng nào."
- "Tuy nhiên, khi bạn trình bày kết quả cho sếp (business stakeholder), họ không quan tâm đến AUC. Họ sẽ hỏi: 'Nếu chúng ta dùng mô hình này, mỗi ngày sẽ có bao nhiêu báo động sai? Chúng ta sẽ bắt được bao nhiêu ca gian lận?'. Lúc này, bạn phải chọn một ngưỡng cụ thể và trình bày Precision, Recall, F1-Score."

## 8. Tổng kết mạnh mẽ và cầu nối đến bài sau

"Hôm nay, chúng ta đã chinh phục được một trong những công cụ đánh giá mạnh mẽ và phổ biến nhất trong Machine Learning: ROC và AUC. Chúng ta đã hiểu được sức mạnh của chúng trong việc cung cấp một cái nhìn tổng quan, không thiên vị về khả năng của mô hình. Quan trọng hơn, chúng ta đã biết được giới hạn của chúng, và nhận ra rằng không có một 'viên đạn bạc' nào cho mọi bài toán. Việc lựa chọn giữa ROC/AUC và Precision-Recall phụ thuộc vào chính bài toán của bạn, đặc biệt là sự cân bằng của dữ liệu. Và sự mất cân bằng đó chính là chủ đề chúng ta sẽ mổ xẻ trong bài học tiếp theo. Chúng ta sẽ định nghĩa nó, xem các ví dụ, và hiểu tại sao nó lại là một thách thức lớn trong thực tế."
