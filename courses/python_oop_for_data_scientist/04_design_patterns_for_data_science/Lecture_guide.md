
# Lecture Guide: Bài 4 - Design Patterns cho Khoa học dữ liệu

## 1. Lời hứa của bài học (The Promise)

"Hôm nay, chúng ta sẽ không học thêm những khái niệm OOP mới. Thay vào đó, chúng ta sẽ học cách trở thành những 'kiến trúc sư' phần mềm. Các bạn sẽ được giới thiệu về **Design Patterns** - những 'bí kíp' võ công đã được các cao thủ lập trình đúc kết, giúp chúng ta giải quyết các vấn đề thiết kế phổ biến một cách thanh lịch và hiệu quả. Sau buổi học, bạn sẽ biết cách sử dụng các mẫu như **Factory**, **Strategy** để xây dựng những hệ thống data science linh hoạt, dễ mở rộng, và sẵn sàng đối mặt với sự thay đổi."

## 2. Cốt truyện giảng dạy (Teaching Storyline)

1.  **Mở đầu - Từ Thợ xây đến Kiến trúc sư:**
    *   "Ở các bài trước, chúng ta đã học cách làm ra 'gạch' (class), xây 'tường' (inheritance), và trang trí 'nội thất' (special methods). Chúng ta là những người thợ xây lành nghề. Nhưng làm thế nào để xây một tòa nhà chọc trời thay vì chỉ một ngôi nhà cấp bốn?"
    *   Giới thiệu **Design Patterns** như là bản vẽ của kiến trúc sư. "Nó không phải là một viên gạch hay bức tường cụ thể, mà là cách sắp xếp gạch và tường để tạo ra một kết cấu vững chắc, giải quyết một vấn đề cụ thể (ví dụ: làm sao để hành lang thông thoáng, làm sao để có nhiều ánh sáng tự nhiên)."

2.  **Pattern 1: Factory - Nhà máy sản xuất Object:**
    *   **Vấn đề:** "Hãy tưởng tượng bạn cần xử lý dữ liệu bằng `StandardScaler`, `MinMaxScaler`, hoặc `OneHotEncoder` dựa trên một lựa chọn trong file config. Cách làm tự nhiên là viết một chuỗi `if/elif/else` dài ngoằng. Trông rất tệ và mỗi khi có thêm một lựa chọn mới, bạn phải sửa lại chuỗi `if` này."
    *   **Giải pháp (Factory):** "Thay vì tự mình đi xây từng cái, tại sao không xây một 'nhà máy' và chỉ cần nói với nó: 'Này nhà máy, cho tôi một cái StandardScaler!'?"
    *   **Demo:**
        1.  Tạo các class sản phẩm (`Scaler`, `Encoder`).
        2.  Tạo class `PreprocessorFactory` với method `get_preprocessor(name)`.
        3.  Bên trong method này, đặt chuỗi `if/elif/else` vào đó.
        4.  Code client (bên sử dụng) giờ chỉ cần gọi `factory.get_preprocessor('min_max_scaler')`.
    *   **Lợi ích:** "Toàn bộ logic tạo đối tượng phức tạp đã được đóng gói vào một chỗ. Code client trở nên sạch sẽ. Khi có preprocessor mới, ta chỉ cần sửa nhà máy, không cần sửa code client."

3.  **Pattern 2: Strategy - Các chiến lược có thể hoán đổi:**
    *   **Vấn đề:** "Bây giờ, hãy xem xét việc xử lý giá trị thiếu. Có rất nhiều cách: điền bằng mean, median, mode, hằng số, hoặc một mô hình phức tạp. Nếu viết tất cả vào một hàm `handle_missing`, nó sẽ lại là một mớ `if/elif/else` hỗn độn."
    *   **Giải pháp (Strategy):** "Hãy coi mỗi cách xử lý là một 'chiến lược' riêng. Chúng ta sẽ tạo một class 'Tướng quân' (`MissingValueHandler`) không tự mình chiến đấu, mà sẽ chọn một 'Binh pháp' (`ImputationStrategy`) và giao cho nó thực hiện."
    *   **Demo:**
        1.  Tạo interface `ImputationStrategy` với method `impute`.
        2.  Tạo các class chiến lược cụ thể: `MeanImputation`, `MedianImputation`, `ConstantImputation`.
        3.  Tạo class context `MissingValueHandler` nhận một object strategy trong `__init__`.
        4.  Method `handle` của `MissingValueHandler` chỉ đơn giản gọi `self.strategy.impute(data)`.
        5.  Cho thấy cách dễ dàng thay đổi chiến lược bằng `handler.set_strategy(...)` mà không cần sửa code của `handler`.
    *   **Lợi ích:** "Cực kỳ linh hoạt. Bạn có thể thêm chiến lược mới mà không cần chạm vào `MissingValueHandler`. Tuân thủ Open/Closed Principle."

4.  **Pattern 3: Singleton - Kẻ độc hành:**
    *   **Vấn đề:** "Trong một dự án lớn, bạn có một file `config.json` chứa mọi cài đặt. Nhiều module khác nhau trong code đều cần đọc file này. Chẳng lẽ mỗi module lại tự mở và đọc file, tạo ra một object config riêng? Rất lãng phí và có thể gây ra sự không nhất quán."
    *   **Giải pháp (Singleton):** "Chúng ta cần đảm bảo rằng trong toàn bộ vũ trụ của ứng dụng, chỉ có **một và chỉ một** object config tồn tại."
    *   **Demo:**
        1.  Tạo class `PipelineConfig`.
        2.  Giải thích cách ghi đè `__new__` để kiểm soát quá trình tạo object.
        3.  Trong `__new__`, kiểm tra xem `_instance` đã tồn tại chưa. Nếu chưa thì tạo mới, nếu rồi thì trả về cái cũ.
        4.  Tạo `config1 = PipelineConfig()`, `config2 = PipelineConfig()`.
        5.  In ra `config1 is config2` để chứng minh chúng là một.
    *   **Cảnh báo:** Nhấn mạnh rằng Singleton là một con dao hai lưỡi, thường bị lạm dụng. Khuyến khích dùng Dependency Injection (truyền object vào) như một giải pháp thay thế linh hoạt hơn trong nhiều trường hợp.

5.  **Tổng kết và kết nối:**
    *   Recap lại 3 patterns và vấn đề chúng giải quyết:
        *   **Factory:** Che giấu logic tạo object phức tạp.
        *   **Strategy:** Làm cho các thuật toán có thể hoán đổi.
        *   **Singleton:** Đảm bảo chỉ có một instance duy nhất.
    *   "Design Patterns không phải để học thuộc lòng, mà để nhận biết vấn đề và áp dụng đúng giải pháp. Chúng là công cụ tư duy của những lập trình viên chuyên nghiệp."
    *   "Với tất cả kiến thức từ đầu khóa học đến giờ, bạn đã sẵn sàng. Trong bài học cuối cùng, chúng ta sẽ không học gì mới nữa, mà sẽ cùng nhau xắn tay áo lên, tổng hợp mọi thứ để xây dựng một pipeline data science hoàn chỉnh bằng OOP."

## 3. Những điểm cần nhấn mạnh bằng ngôn ngữ đơn giản

*   "**Factory là 'Menu gọi món'.** Bạn chỉ cần nói tên món, nhà bếp (factory) sẽ tự biết cách làm và mang ra cho bạn."
*   "**Strategy là 'Hộp bút chì màu'.** Bạn có một bức tranh (vấn đề), và bạn có thể chọn bất kỳ cây bút chì nào (chiến lược) để tô màu cho nó."
*   "**Singleton là 'Nữ hoàng'.** Trong một vương quốc, chỉ có một Nữ hoàng duy nhất. Mọi người khi cần đều phải tìm đến bà ấy."

## 4. Những điểm học viên thường bị rối

*   **Factory vs. Strategy:** "Cả hai đều có vẻ giống nhau, đều có nhiều class con."
    *   **Phân biệt:** Factory tập trung vào việc **tạo ra** các đối tượng. Mục đích của nó là để che giấu `if/else` khi `new`/`__init__`. Strategy tập trung vào việc **thực thi** một hành vi. Mục đích của nó là để che giấu `if/else` bên trong một phương thức. Factory trả về các object có thể có interface khác nhau, trong khi các Strategy bắt buộc phải có cùng một interface.
*   **Khi nào dùng Factory?** "Khi bạn có một hàm hoặc phương thức mà logic chính của nó là một chuỗi `if/elif/else` để quyết định `return ClassA()` hay `return ClassB()`."
*   **Khi nào dùng Strategy?** "Khi bạn có một phương thức mà bên trong nó có một chuỗi `if/elif/else` để quyết định sẽ thực hiện logic A hay logic B trên cùng một dữ liệu."

## 5. Phép ẩn dụ hoặc mô hình tư duy gợi ý

*   **Game đối kháng:**
    *   **Factory:** `CharacterFactory` có thể tạo ra `Warrior`, `Mage`, `Archer` dựa trên lựa chọn của người chơi.
    *   **Strategy:** Một nhân vật `Warrior` có thể có các "thế đánh" (chiến lược) khác nhau: `DefensiveStance`, `AggressiveStance`. Anh ta có thể thay đổi thế đánh giữa trận đấu.

## 6. Gợi ý kịch bản nói về ứng dụng thực tế

*   **Factory trong ML:** Một `ModelFactory` có thể tạo ra các mô hình `sklearn` (`RandomForestClassifier`, `SVC`, `LogisticRegression`) đã được cấu hình sẵn các tham số mặc định của công ty bạn.
*   **Strategy trong ML:** Một class `CrossValidator` có thể nhận các chiến lược chia dữ liệu khác nhau: `KFoldStrategy`, `StratifiedKFoldStrategy`, `TimeSeriesSplitStrategy`.
*   **Singleton trong ML:** Một `ExperimentLogger` (như MLflow hoặc WandB wrapper) có thể là một Singleton để đảm bảo tất cả các phần của pipeline (data loading, training, evaluation) đều ghi log vào cùng một "run" (phiên thí nghiệm).

## 7. Tóm tắt cuối bài và cầu nối

"Hôm nay, chúng ta đã nâng cấp tư duy của mình từ việc chỉ viết code chạy được sang việc thiết kế những hệ thống có cấu trúc và linh hoạt. Các bạn đã học được cách nhận diện các vấn đề thiết kế phổ biến và áp dụng các mẫu Factory, Strategy, Singleton để giải quyết chúng một cách chuyên nghiệp."

"Đây là mảnh ghép lý thuyết cuối cùng. Các bạn đã có trong tay toàn bộ công cụ cần thiết. Trong buổi học cuối cùng, chúng ta sẽ không nhìn vào slide nữa. Chúng ta sẽ cùng nhau bắt tay vào một dự án thực tế, xây dựng một pipeline hoàn chỉnh từ A-Z, nơi tất cả những gì chúng ta đã học sẽ được kết hợp lại. Hãy sẵn sàng để áp dụng kiến thức của mình!"
