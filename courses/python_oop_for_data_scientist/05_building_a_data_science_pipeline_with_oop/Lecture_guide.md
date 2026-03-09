
# Lecture Guide: Bài 5 - Xây dựng Pipeline Khoa học dữ liệu với OOP

## 1. Lời hứa của bài học (The Promise)

"Hôm nay là ngày chúng ta gặt hái thành quả. Chúng ta sẽ không học thêm bất kỳ khái niệm mới nào. Thay vào đó, chúng ta sẽ tổng hợp tất cả những vũ khí đã học - Classes, Inheritance, Polymorphism, Design Patterns - để cùng nhau xây dựng một pipeline machine learning hoàn chỉnh từ đầu đến cuối. Các bạn sẽ thấy cách tư duy hướng đối tượng biến một mớ code lộn xộn thành một cỗ máy được tổ chức tốt, module hóa, và cực kỳ chuyên nghiệp. Đây là buổi học biến lý thuyết thành thực hành, biến kiến thức thành sản phẩm."

## 2. Cốt truyện giảng dạy (Teaching Storyline)

1.  **Mở đầu - Từ Script đến Hệ thống:**
    *   Bắt đầu bằng cách cho xem một ví dụ về một script ML điển hình: một file Python dài, tất cả code từ load data, xử lý, train, test đều nằm chung.
    *   Hỏi học viên: "Script này chạy được, nhưng có vấn đề gì? Nếu muốn thay đổi mô hình thì sao? Nếu muốn tái sử dụng phần load data cho dự án khác? Nếu có lỗi ở bước scaling, bạn sẽ tìm ở đâu?"
    *   Dẫn dắt: "Hôm nay, chúng ta sẽ nâng cấp script này thành một hệ thống phần mềm thực thụ bằng OOP."

2.  **Lên kế hoạch - Tư duy như một Kiến trúc sư:**
    *   "Trước khi code, hãy thiết kế. Pipeline của chúng ta có những bước nào?" -> Load, Process, Train, Evaluate.
    *   "Điểm chung của các bước này là gì?" -> Chúng đều là một "hành động", một "công đoạn".
    *   "Vậy tại sao không tạo một 'khuôn' chung cho tất cả các công đoạn đó?" -> Giới thiệu ý tưởng về một class cha `PipelineStep` với một method trừu tượng `process()`. Đây là lúc củng cố về **Inheritance** và **Polymorphism**.

3.  **Xây dựng từng viên gạch (Từng Step):**
    *   **Demo 1: `DataLoader`**
        *   Tạo class `DataLoader(PipelineStep)`.
        *   Implement `process()` để tải dữ liệu và trả về DataFrame. Đơn giản, rõ ràng.
    *   **Demo 2: `DataProcessor`**
        *   Tạo class `DataProcessor(PipelineStep)`.
        *   Implement `process()` để nhận DataFrame, chia train/test, scale dữ liệu.
        *   Gợi ý: "Ở đây chúng ta đang hard-code `SimpleImputer(strategy='mean')`. Nếu muốn linh hoạt hơn thì sao? À, chúng ta có thể dùng **Strategy Pattern** đã học ở bài trước!" (Không cần implement ngay, chỉ cần gợi mở để kết nối kiến thức).
    *   **Demo 3: `ModelTrainer`**
        *   Tạo class `ModelTrainer(PipelineStep)`.
        *   Implement `process()` để nhận dữ liệu đã xử lý và huấn luyện mô hình.
        *   Gợi ý: "Chúng ta đang hard-code `LinearRegression()`. Nếu muốn chọn `RandomForest` thì sao? À, chúng ta có thể dùng **Factory Pattern**!" -> Implement một `ModelFactory` đơn giản.
    *   **Demo 4: `ModelEvaluator`**
        *   Tạo class `ModelEvaluator(PipelineStep)`.
        *   Implement `process()` để nhận dữ liệu test và mô hình đã train, sau đó tính toán và in ra metrics.

4.  **Lắp ráp cỗ máy - Class `Pipeline`:**
    *   "Chúng ta đã có các bộ phận. Giờ cần một 'dây chuyền lắp ráp' để nối chúng lại."
    *   Tạo class `Pipeline` nhận một list các `steps` trong `__init__`.
    *   Viết method `run()`:
        *   Duyệt qua từng `step` trong list.
        *   Gọi `step.process()`.
        *   Truyền output của bước này làm input cho bước tiếp theo.
    *   **Nhấn mạnh "Aha Moment":** "Hãy nhìn vào vòng lặp này. Nó chỉ gọi `step.process()`. Nó không cần biết `step` là `DataLoader` hay `ModelTrainer`. Đây chính là **Đa hình (Polymorphism)** đang hoạt động! Sức mạnh của việc có một interface chung."

5.  **Chạy thử và Tận hưởng thành quả:**
    *   Tạo các instance của từng step.
    *   Tạo instance của `Pipeline` với list các step đó.
    *   Chạy `pipeline.run()`.
    *   Xem output của từng bước được in ra một cách tuần tự, rõ ràng.
    *   **Thử nghiệm sự linh hoạt:**
        *   "Bây giờ sếp muốn thử `RandomForest`."
        *   Chỉ cần thay đổi một chuỗi `model_name = 'random_forest'` khi khởi tạo `ModelTrainer`.
        *   Chạy lại pipeline. Mọi thứ hoạt động mà không cần sửa gì thêm.
        *   So sánh kết quả RMSE của hai mô hình.

6.  **Tổng kết và Nhìn lại:**
    *   Mở lại script ban đầu và so sánh với cấu trúc OOP vừa xây dựng.
    *   Chỉ ra những lợi ích đã đạt được: Module hóa, Tái sử dụng, Linh hoạt, Dễ bảo trì.
    *   "Bằng cách đầu tư thời gian vào việc thiết kế với OOP, chúng ta đã xây dựng một hệ thống không chỉ giải quyết bài toán hiện tại, mà còn sẵn sàng cho những thay đổi và yêu cầu trong tương lai."
    *   Gợi ý các hướng cải tiến (dùng Singleton cho config, dùng Strategy cho data processing,...) để học viên có thể tự mình khám phá thêm.

## 3. Những điểm cần nhấn mạnh bằng ngôn ngữ đơn giản

*   "Mỗi class là một 'công nhân' chuyên nghiệp. Công nhân `DataLoader` chỉ biết đọc dữ liệu. Công nhân `ModelTrainer` chỉ biết huấn luyện. Không ai làm việc của ai."
*   "`PipelineStep` là 'bản mô tả công việc' chung. Ai muốn làm việc trong dây chuyền này đều phải biết làm một việc gọi là `process`."
*   "Class `Pipeline` là 'quản đốc'. Ông ta không cần biết chi tiết công việc của từng công nhân, chỉ cần ra lệnh 'Làm đi!' (`process!`) theo đúng thứ tự."
*   "Thay đổi mô hình từ `LinearRegression` sang `RandomForest` giống như việc đổi một công nhân này bằng một công nhân khác có cùng chuyên môn. Dây chuyền vẫn hoạt động trơn tru."

## 4. Những điểm học viên thường bị rối

*   **Dữ liệu được truyền giữa các bước như thế nào?** Đây là điểm cốt lõi. Cần vẽ ra hoặc giải thích rõ luồng dữ liệu:
    *   `DataLoader.process()` -> `DataFrame`
    *   `DataProcessor.process(DataFrame)` -> `(X_train, X_test, y_train, y_test)`
    *   `ModelTrainer.process((X_train, ...))` -> `trained_model`
    *   `ModelEvaluator.process((..., X_test, ...))` -> `metrics`
    *   Cần có một biến `data_so_far` trong vòng lặp của `Pipeline` để hứng và truyền dữ liệu.
*   **Tại sao `ModelEvaluator` lại phức tạp hơn?** Nó cần cả `processed_data` và `trained_model`. Có thể giải thích 2 cách tiếp cận:
    1.  **Cách đơn giản (như trong notebook):** Chạy các bước thủ công và truyền các kết quả cần thiết một cách tường minh.
    2.  **Cách phức tạp hơn (dành cho người muốn tìm hiểu sâu):** Class `Pipeline` có thể có một "context" hoặc "cache" để lưu kết quả của tất cả các bước trước đó, và mỗi bước có thể khai báo nó cần những gì từ context đó. (Ví dụ: `evaluator.process(context)`).

## 5. Phép ẩn dụ hoặc mô hình tư duy gợi ý

*   **Dây chuyền lắp ráp ô tô:**
    *   `PipelineStep`: Trạm làm việc (Workstation).
    *   `DataLoader`: Trạm lắp khung xe.
    *   `DataProcessor`: Trạm lắp động cơ.
    *   `ModelTrainer`: Trạm sơn xe.
    *   `Pipeline`: Băng chuyền tự động di chuyển xe từ trạm này đến trạm khác.

## 6. Tóm tắt cuối bài và cầu nối

"Chúc mừng tất cả các bạn! Hôm nay, các bạn không chỉ viết code, mà các bạn đã thiết kế và xây dựng một hệ thống. Các bạn đã thấy được rằng OOP không phải là những cú pháp khô khan, mà là một phương pháp tư duy mạnh mẽ để chinh phục sự phức tạp trong các dự án thực tế."

"Khóa học này chỉ là bước khởi đầu. Con đường của một data scientist chuyên nghiệp đòi hỏi việc liên tục học hỏi và thực hành. Hãy mang tư duy hướng đối tượng này vào các dự án tiếp theo của bạn, từ những bài tập nhỏ đến những sản phẩm thực tế. Hãy bắt đầu xây dựng 'danh mục đầu tư' các class và pipeline của riêng bạn. Chúc các bạn thành công!"
