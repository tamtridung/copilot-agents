# Hướng dẫn Giảng dạy - Bài 08: Đồ án cuối khóa

## 1. Lời hứa của Bài học (Lesson Promise)

Sau khi hoàn thành đồ án này, học viên sẽ có thể:
- Tự tin thực hiện một dự án khoa học dữ liệu về phân loại trên dữ liệu mất cân bằng từ đầu đến cuối.
- Áp dụng một cách có hệ thống các bước: EDA, tiền xử lý, lựa chọn chỉ số, xây dựng mô hình, và so sánh kết quả.
- Đưa ra quyết định lựa chọn mô hình cuối cùng dựa trên phân tích định lượng (số liệu) và định tính (biểu đồ), gắn liền với mục tiêu kinh doanh.
- Trình bày và bảo vệ được lựa chọn của mình một cách chuyên nghiệp.

## 2. Cốt truyện Giảng dạy (Teaching Storyline)

**Mở đầu (Hook) & Kết nối từ các bài trước:**
- "Chào mừng các bạn đến với bài học cuối cùng và cũng là quan trọng nhất. Trong suốt 7 bài học vừa qua, chúng ta đã tích lũy cho mình một kho vũ khí đầy đủ. Hôm nay là ngày chúng ta ra trận. Chúng ta sẽ không học lý thuyết mới, mà sẽ dùng toàn bộ vũ khí đó để giải quyết một trong những bài toán kinh điển và có giá trị nhất trong ngành tài chính: phát hiện gian lận thẻ tín dụng."
- "Đây không còn là những ví dụ nhỏ lẻ nữa. Đây là một kịch bản thực tế, nơi mỗi quyết định của bạn đều có thể ảnh hưởng đến tiền bạc của công ty và trải nghiệm của khách hàng. Hãy cùng nhau vào vai một nhà khoa học dữ liệu thực thụ."

**Chuyển tiếp vào nội dung chính (hướng dẫn học viên thực hiện):**
1.  **Bước 1: Thấu hiểu Kẻ thù (EDA):**
    - "Trước khi chiến đấu, ta phải hiểu kẻ thù. Hãy bắt đầu bằng việc khám phá dữ liệu."
    - Hướng dẫn học viên chạy các lệnh cơ bản và tập trung vào câu hỏi: "Dữ liệu mất cân bằng đến mức nào?". Cho họ tự tính toán và cảm nhận con số 0.17%.
    - "Câu hỏi tiếp theo: Có gì bất thường không? Hãy nhìn vào `Amount` và `Time`. Chúng có cùng 'ngôn ngữ' với các cột V khác không? Rõ ràng là không. Chúng ta cần một 'thông dịch viên'. Đó chính là `Scaler`."

2.  **Bước 2: Mài sắc Vũ khí (Tiền xử lý):**
    - "Bây giờ, hãy chuẩn bị vũ khí của chúng ta."
    - Hướng dẫn học viên áp dụng `RobustScaler`. Giải thích ngắn gọn tại sao nó tốt hơn `StandardScaler` trong trường hợp này (ít bị ảnh hưởng bởi các giao dịch có giá trị cực lớn - outliers).
    - Nhấn mạnh lại quy tắc vàng: "Và tất nhiên, trước khi làm bất cứ điều gì, chúng ta phải làm gì đầu tiên? **Chia dữ liệu!** Hãy nhớ `stratify=y` để đảm bảo chiến trường (tập test) phản ánh đúng thực tế."

3.  **Bước 3: Xác định Mục tiêu Chiến thắng (Lựa chọn Chỉ số):**
    - "Chúng ta không thể chiến đấu mà không biết thế nào là thắng. Vậy, mục tiêu của chúng ta là gì?"
    - Đặt câu hỏi gợi mở: "Giữa việc bỏ sót 1 tên trộm và việc bắt nhầm 10 người dân, cái nào tệ hơn trong bối cảnh này? Rõ ràng là bỏ sót tên trộm. Vậy chúng ta ưu tiên `Precision` hay `Recall`?"
    - Hướng dẫn học viên đi đến kết luận nên dùng **F2-score** (ưu tiên Recall) và **PR AUC** (đánh giá tổng quan).

4.  **Bước 4: Tung ra các Chiến thuật (Xây dựng Mô hình):**
    - "Bây giờ là lúc hành động. Chúng ta sẽ thử 3 chiến thuật khác nhau."
    - **Chiến thuật 1: Ngây thơ (Baseline):** "Hãy xem nếu chúng ta không làm gì cả thì sao." Chạy mô hình gốc và cho thấy kết quả tệ hại của nó trên `Recall`. "Đây là lý do tại sao chúng ta có mặt ở đây."
    - **Chiến thuật 2: Thông minh & Nhanh gọn (Cost-Sensitive):** "Hãy thử cây đũa thần `class_weight='balanced'`. Nhanh, gọn, và xem hiệu quả của nó." Cho học viên thấy sự cải thiện ngay lập tức.
    - **Chiến thuật 3: Chăm chỉ & Cẩn thận (SMOTE):** "Bây giờ, hãy thử phương pháp 'cày cuốc' hơn. Dùng SMOTE để tạo dữ liệu." Nhắc nhở họ về việc SMOTE chỉ `fit_resample` trên tập train.
    - Yêu cầu học viên in ra kết quả của cả 3 và tự mình so sánh sơ bộ.

5.  **Bước 5: Tổng kết Chiến trường (Phân tích & Lựa chọn):**
    - "Trận chiến đã kết thúc. Bây giờ là lúc một vị tướng giỏi phân tích chiến trường."
    - Hướng dẫn học viên tạo bảng tổng kết. "Những con số nói lên điều gì? Hãy nhìn vào cột `Recall` và `F2-score`."
    - Hướng dẫn vẽ biểu đồ PR Curve. "Một bức tranh hơn ngàn lời nói. Đường cong nào bao phủ diện tích lớn nhất? Đường cong nào cho thấy sự đánh đổi tốt nhất?"
    - **Khoảnh khắc quyết định:** "Dựa trên tất cả những điều này, với tư cách là một nhà khoa học dữ liệu, bạn sẽ đề xuất mô hình nào cho công ty? Hãy viết ra lựa chọn và 3 gạch đầu dòng bảo vệ quyết định của bạn."

**Tổng kết và Hướng đi tiếp theo:**
- "Xin chúc mừng! Bạn không chỉ hoàn thành một dự án, mà bạn đã hoàn thành một quy trình làm việc hoàn chỉnh mà các nhà khoa học dữ liệu thực hiện hàng ngày. Bạn đã đối mặt với một vấn đề khó, thử nghiệm nhiều giải pháp, phân tích sự đánh đổi và đưa ra một quyết định có cơ sở."
- "Kiến thức bạn học được trong khóa này là nền tảng. Thế giới khoa học dữ liệu còn rộng lớn hơn nữa. Từ đây, bạn có thể khám phá:
    - Các thuật toán mạnh mẽ hơn như XGBoost, LightGBM.
    - Các kỹ thuật phát hiện bất thường (Anomaly Detection) như Isolation Forest, One-Class SVM.
    - Cách triển khai mô hình (deployment) để nó có thể hoạt động trong thời gian thực.
- "Nhưng quan trọng nhất, bạn đã có tư duy đúng đắn để giải quyết vấn đề. Đó là tài sản quý giá nhất. Chúc bạn thành công trên con đường sự nghiệp của mình!"

## 3. Những điểm cần nhấn mạnh

- **Tính hệ thống:** Nhấn mạnh việc đi theo một quy trình có cấu trúc là cực kỳ quan trọng.
- **Sự đánh đổi (Trade-off):** Không có mô hình nào là hoàn hảo. Luôn có sự đánh đổi giữa Precision và Recall. Nhiệm vụ của nhà khoa học dữ liệu là tìm ra điểm cân bằng phù hợp nhất với yêu cầu kinh doanh.
- **Lập luận:** Điểm số không phải là tất cả. Khả năng giải thích *tại sao* bạn chọn mô hình đó, *tại sao* chỉ số này lại quan trọng, mới là điều phân biệt một người giỏi và một người chỉ biết chạy code.
- **Kết nối với thực tế:** Luôn nhắc lại bối cảnh "phát hiện gian lận" để các quyết định của học viên (chọn chỉ số, chọn mô hình) đều có mục đích rõ ràng.

## 4. Những điểm học viên thường gặp khó khăn

- **Code quá dài và rối:** Khuyến khích họ chia nhỏ các bước ra thành từng cell, thêm các comment và tiêu đề markdown để giữ cho notebook sạch sẽ.
- **Quên `stratify=y`:** Một lỗi nhỏ nhưng nghiêm trọng, cần nhắc lại.
- **Lúng túng khi diễn giải kết quả:** Dành thời gian hướng dẫn họ cách đọc `classification_report` và so sánh các con số một cách có hệ thống. Bảng tổng kết là một công cụ rất hữu ích cho việc này.
- **Không biết chọn mô hình nào:** Nhấn mạnh rằng không có câu trả lời "đúng" tuyệt đối. Miễn là họ có lập luận vững chắc dựa trên các chỉ số đã chọn, lựa chọn của họ đều có giá trị.

## 5. Gợi ý Demo/Hướng dẫn

- Khi demo, hãy thực hiện từng bước một cách chậm rãi. Sau mỗi bước, hãy dừng lại và đặt câu hỏi: "Chúng ta vừa làm gì? Tại sao chúng ta làm vậy? Kết quả này cho chúng ta biết điều gì?"
- Khi đến phần so sánh, hãy đặt các `classification_report` cạnh nhau và khoanh tròn những con số quan trọng (Recall lớp 1, F2-score) để làm nổi bật sự khác biệt.
- Khi vẽ PR Curve, hãy chỉ vào các vùng trên biểu đồ và giải thích ý nghĩa của chúng (ví dụ: "Ở vùng Recall cao này, cả hai mô hình đều có Precision thấp, nhưng mô hình A vẫn tốt hơn một chút so với mô hình B").

Đây là một bài học tổng kết, vì vậy vai trò của người hướng dẫn là một người đồng hành, đặt câu hỏi gợi mở và định hướng tư duy hơn là chỉ cung cấp đáp án.
