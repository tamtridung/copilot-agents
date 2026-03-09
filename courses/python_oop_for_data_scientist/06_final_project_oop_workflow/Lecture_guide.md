
# Lecture Guide: Đồ án cuối khóa - Xây dựng Framework Thử nghiệm ML

## 1. Lời hứa của bài học (The Promise)

"Chào mừng các bạn đến với bài kiểm tra cuối cùng, cũng là cơ hội để các bạn tỏa sáng. Hôm nay, các bạn sẽ không học lý thuyết nữa, mà sẽ trở thành những kiến trúc sư phần mềm thực thụ. Nhiệm vụ của các bạn là xây dựng một sản phẩm hoàn chỉnh: một framework nhỏ để tự động hóa việc chạy và so sánh các mô hình Machine Learning. Đây là lúc để kết hợp tất cả những gì chúng ta đã học - từ `class` đơn giản đến các `design pattern` phức tạp - để tạo ra một công cụ mạnh mẽ, linh hoạt và có thể tái sử dụng, một công cụ mà bất kỳ Data Scientist nào cũng muốn có trong bộ đồ nghề của mình."

## 2. Cốt truyện giảng dạy (Teaching Storyline)

1.  **Mở đầu - Nỗi đau của người làm Khoa học dữ liệu:**
    *   Bắt đầu bằng một câu chuyện: "Hãy tưởng tượng bạn đang làm việc với một bộ dữ liệu mới. Sếp của bạn yêu cầu: 'Hãy thử Logistic Regression, Decision Tree, và Random Forest xem cái nào tốt nhất'. Bạn làm gì? Bạn copy-paste code, thay đổi tên mô hình, chạy lại, rồi ghi kết quả vào Excel? Tuần sau, sếp lại yêu cầu: 'Thử thêm XGBoost và LightGBM nữa'. Bạn lại copy-paste... Quá trình này rất thủ công, dễ sai sót và không chuyên nghiệp."
    *   Đặt vấn đề: "Làm thế nào để chúng ta có thể tự động hóa quy trình này? Làm thế nào để chỉ cần định nghĩa một lần và chạy cho hàng chục mô hình?"
    *   Dẫn dắt: "Câu trả lời nằm ở việc xây dựng một framework. Và vũ khí của chúng ta chính là OOP."

2.  **Giới thiệu Đồ án - Bức tranh toàn cảnh:**
    *   Trình bày slide hoặc notebook về **Mục tiêu đồ án**: Xây dựng một framework linh hoạt để chạy thử nghiệm ML.
    *   Hiển thị **Sơ đồ kiến trúc** ở mức cao: Một `ExperimentRunner` ở trung tâm, điều phối các thành phần khác như `DataLoader`, `Preprocessor`, `ModelFactory`, `Evaluator`.
    *   Nhấn mạnh: "Đây không phải là việc viết một script, mà là thiết kế một hệ thống. Mỗi hộp trong sơ đồ này sẽ là một `class`."

3.  **Phân tích Yêu cầu Thiết kế (Trọng tâm của bài giảng):**
    *   Đi qua từng yêu cầu OOP và giải thích tại sao nó lại quan trọng trong bối cảnh này.
    *   **Classes:** "Tại sao không để tất cả trong một class? -> Vì mỗi class nên có một trách nhiệm duy nhất (Single Responsibility). `DataLoader` chỉ lo tải dữ liệu, không quan tâm đến việc huấn luyện mô hình."
    *   **Inheritance/Polymorphism:** "Tại sao cần `BaseModel`? -> Để `ExperimentRunner` có thể làm việc với bất kỳ mô hình nào mà không cần biết chi tiết bên trong nó là `LogisticRegression` hay `RandomForest`. Runner chỉ cần biết nó có thể `train()` và `predict()`."
    *   **Factory Pattern:** "Tại sao không `if-elif` trong `ExperimentRunner`? -> Vì nếu làm vậy, mỗi lần thêm mô hình mới, bạn phải sửa code của `ExperimentRunner`. Dùng Factory giúp tách biệt logic tạo mô hình ra khỏi logic chạy thử nghiệm. Runner chỉ cần nói: 'Này nhà máy, cho tôi một mô hình tên là 'random_forest' với các tham số này'."
    *   **Encapsulation:** "Runner không cần biết `SklearnModelWrapper` dùng thư viện gì bên trong. Nó chỉ cần giao tiếp qua các phương thức công khai `train` và `predict`. Điều này cho phép chúng ta có thể thay thế `SklearnModelWrapper` bằng `TensorFlowModelWrapper` trong tương lai mà không ảnh hưởng đến Runner."

4.  **Hướng dẫn từng bước (Gợi ý lộ trình):**
    *   Trình bày các bước gợi ý trong notebook project brief.
    *   **Bước 1: Thiết kế trên giấy.** Khuyến khích học viên thực sự dành 15-20 phút để vẽ ra các class và mối quan hệ của chúng. "Đây là bước quan trọng nhất của một kỹ sư phần mềm."
    *   **Bước 2-4: Xây dựng các thành phần.** Hướng dẫn học viên xây dựng các class "phụ tùng" trước: `BaseModel`, `SklearnModelWrapper`, `DataLoader`, `Evaluator`.
    *   **Bước 5: Xây dựng "Nhạc trưởng".** Sau khi có đủ phụ tùng, mới bắt đầu xây dựng `ExperimentRunner` để lắp ráp chúng lại.
    *   **Bước 6: Cấu hình và chạy.** Giải thích tầm quan trọng của việc tách cấu hình (config) ra khỏi logic. "Logic của bạn là bất biến, nhưng thử nghiệm thì luôn thay đổi. Hãy để sự thay đổi đó nằm trong file config."

5.  **Demo Lời giải (Sau khi học viên đã có thời gian tự làm):**
    *   Walk-through qua notebook lời giải.
    *   Không chỉ copy-paste code, mà giải thích **tại sao** lại thiết kế như vậy ở mỗi bước.
    *   Chạy thử nghiệm và cho xem kết quả cuối cùng - một bảng so sánh đẹp mắt được in ra.
    *   **"Aha Moment":** Sửa file `config`, thêm một mô hình mới (ví dụ, một `DecisionTreeClassifier` với `max_depth=10`). Chạy lại. "Thấy không? Chúng ta vừa thêm một thử nghiệm mới mà không cần sửa một dòng code logic nào. Đây chính là sức mạnh của một thiết kế tốt."

## 3. Những điểm cần nhấn mạnh bằng ngôn ngữ đơn giản

*   "`ExperimentRunner` là ông chủ. Ông ta không tự làm gì cả, chỉ ra việc cho các chuyên gia."
*   "`DataLoader` là chuyên gia kho vận, chỉ biết đi lấy hàng (dữ liệu)."
*   "`Preprocessor` là chuyên gia sơ chế, chỉ biết làm sạch và chuẩn bị hàng."
*   "`ModelFactory` là quản đốc nhà máy sản xuất, có thể tạo ra bất kỳ loại máy móc (mô hình) nào theo yêu cầu."
*   "`BaseModel` là bản vẽ kỹ thuật chung cho tất cả các loại máy móc. Máy nào cũng phải có nút 'Bật' (`train`) và nút 'Chạy' (`predict`)."
*   "`config` là tờ giấy yêu cầu công việc mà ông chủ đưa cho cả đội. Mọi người chỉ cần đọc tờ giấy đó và làm theo."

## 4. Những điểm học viên thường bị rối

*   **Làm sao để truyền dữ liệu giữa các class?** `ExperimentRunner` sẽ đóng vai trò trung gian, lưu trữ kết quả của mỗi bước (ví dụ: `df` từ `DataLoader`, `X_train, ...` từ `Preprocessor`) và truyền chúng vào các bước tiếp theo.
*   **`*args` và `**kwargs` trong `ModelFactory` hoạt động như thế nào?** Giải thích rằng `**params` là một cách linh hoạt để nhận một dictionary các tham số và "giải nén" chúng khi gọi constructor của mô hình sklearn (`model_class(**params)`).
*   **Tại sao phải bọc (wrap) mô hình sklearn trong một class khác?** Để tuân thủ "hợp đồng" của `BaseModel`. Mô hình sklearn có phương thức `fit`, nhưng `BaseModel` của chúng ta yêu cầu một phương thức tên là `train`. Wrapper class giúp điều chỉnh sự khác biệt này (Adapter Pattern).

## 5. Tóm tắt cuối bài và cầu nối

"Hôm nay, các bạn đã vượt qua một cột mốc quan trọng. Các bạn không chỉ là người sử dụng các công cụ khoa học dữ liệu, mà đã trở thành người tạo ra chúng. Framework mà các bạn xây dựng là một tài sản quý giá, có thể được điều chỉnh và tái sử dụng cho rất nhiều dự án trong tương lai."

"Lập trình Hướng đối tượng là một hành trình. Những gì chúng ta đã học là nền tảng vững chắc. Từ đây, hãy tiếp tục khám phá các design pattern khác, học cách viết unit test cho các class của bạn, và áp dụng tư duy này vào mọi vấn đề bạn giải quyết. Bạn đã có trong tay bộ công cụ để xây dựng các giải pháp khoa học dữ liệu một cách chuyên nghiệp, bền vững và hiệu quả. Chúc mừng và chúc các bạn thành công trên con đường sự nghiệp của mình!"
