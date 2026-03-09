
# Lecture Guide: Bài 2 - Kế thừa và Đa hình

## 1. Lời hứa của bài học (The Promise)

"Trong buổi học trước, chúng ta đã học cách tạo ra những 'viên gạch' code. Hôm nay, chúng ta sẽ học cách xây 'những bức tường' bằng cách xếp các viên gạch đó lên nhau. Các bạn sẽ khám phá **Inheritance (Kế thừa)** - một kỹ thuật cho phép bạn tạo ra class mới dựa trên những class đã có, giúp tiết kiệm tối đa công sức. Cùng với đó là **Polymorphism (Đa hình)**, một 'phép màu' giúp bạn viết code linh hoạt hơn bao giờ hết, có thể xử lý nhiều loại đối tượng khác nhau mà không cần quan tâm đến chi tiết bên trong của chúng."

## 2. Cốt truyện giảng dạy (Teaching Storyline)

1.  **Mở đầu - Vấn đề của sự lặp lại:**
    *   Bắt đầu bằng một câu hỏi gợi mở: "Giả sử chúng ta đã có class `Dog`. Bây giờ muốn tạo class `Cat`. Mèo và chó đều là động vật, đều có tên, biết ăn, biết ngủ. Chẳng lẽ chúng ta lại copy-paste code từ `Dog` sang `Cat`?"
    *   Dẫn dắt vào vấn đề: Copy-paste là kẻ thù của lập trình viên. Nó tạo ra code khó bảo trì. Nếu sửa một lỗi ở `Dog`, bạn phải nhớ sửa cả ở `Cat`.
    *   Giới thiệu **Kế thừa** như một giải pháp thanh lịch: "Thay vì copy, tại sao không cho `Cat` và `Dog` cùng 'thừa hưởng' những đặc điểm chung từ một class cha là `Animal`?"

2.  **Xây dựng khái niệm Kế thừa (Inheritance):**
    *   **Demo 1: Kế thừa đơn giản.**
        1.  Tạo class cha `Animal` với `__init__(name)` và method `eat()`.
        2.  Tạo class con `Dog(Animal): pass`.
        3.  Tạo object `my_dog = Dog("Milo")` và gọi `my_dog.eat()`. Nhấn mạnh rằng `Dog` không có code gì nhưng vẫn hoạt động. Đây là "aha moment" đầu tiên.
    *   **Demo 2: Mở rộng (Extending).**
        *   "Chó có những đặc điểm riêng. Nó biết sủa."
        *   Thêm method `bark()` vào class `Dog`. Giờ `Dog` vừa có khả năng của `Animal`, vừa có khả năng của riêng nó.
    *   **Demo 3: Ghi đè (Overriding).**
        *   "Mèo cũng biết ăn, nhưng nó ăn khác chó. Nó ăn cá."
        *   Tạo class `Cat(Animal)` và định nghĩa lại method `eat()`.
        *   Tạo object `my_cat` và gọi `my_cat.eat()`. So sánh kết quả với `my_dog.eat()`. Đây là "aha moment" thứ hai.
    *   **Demo 4: `super()` - Người trợ giúp đắc lực.**
        *   Đặt vấn đề: "Khi tạo `Dog`, ngoài `name`, tôi muốn có thêm `breed` (giống loài). Tôi phải ghi đè `__init__`. Nhưng tôi không muốn viết lại logic gán `self.name`."
        *   Giới thiệu `super().__init__(name)` như một cách để "nhờ class cha làm phần việc của nó trước". `super()` giúp chúng ta bổ sung chứ không thay thế hoàn toàn.

3.  **Chuyển tiếp sang Đa hình (Polymorphism):**
    *   "Phần hay nhất đây rồi. Khi chúng ta có `Dog` và `Cat` cùng kế thừa từ `Animal` và cùng có method `eat()` (dù hành động khác nhau), một điều kỳ diệu sẽ xảy ra."
    *   Tạo một list `[dog, cat]`.
    *   Viết một hàm `feed_animal(animal)` nhận vào một animal bất kỳ và gọi `animal.eat()`.
    *   Cho hàm này chạy với list trên. Nhấn mạnh rằng hàm `feed_animal` không hề biết nó đang cho chó hay mèo ăn. Nó chỉ ra lệnh "ăn đi", và mỗi con vật tự biết phải ăn như thế nào.
    *   "Đây chính là Đa hình - Polymorphism. Cùng một lời gọi, nhiều hình thái thực hiện khác nhau."

4.  **Củng cố bằng ví dụ Data Science (`BaseModel`):**
    *   "Lý thuyết hay đấy, nhưng nó giúp gì cho data scientist?"
    *   Chuyển sang ví dụ thực tế: So sánh các mô hình machine learning.
    *   **Demo flow:**
        1.  Tạo class cha `BaseModel` với các method "trừu tượng": `train()` (gây lỗi `NotImplementedError`), `predict()`, `evaluate()`. Giải thích đây là một "bản hợp đồng" mà các class con phải tuân theo.
        2.  Tạo class con `LinearRegressionModel(BaseModel)`. Implement `__init__` và `train()` bằng `sklearn`.
        3.  Tạo class con `RandomForestModel(BaseModel)`. Implement `__init__` và `train()` bằng `sklearn`.
        4.  Tạo một list chứa các object của 2 class model trên.
        5.  Viết một vòng lặp duy nhất để `train` và `evaluate` tất cả các model. Nhấn mạnh sự thanh lịch: không `if/else`, chỉ cần gọi `model.train()`.
        6.  Đặt câu hỏi: "Nếu mai sếp yêu cầu thử thêm mô hình XGBoost thì sao? Đơn giản, chỉ cần tạo class `XGBoostModel` và thêm vào list. Vòng lặp không cần sửa đổi."

5.  **Tổng kết và kết nối:**
    *   Recap lại Kế thừa (tái sử dụng code) và Đa hình (linh hoạt trong xử lý).
    *   "Kế thừa giúp chúng ta xây dựng cấu trúc, còn Đa hình giúp chúng ta vận hành cấu trúc đó một cách linh hoạt."
    *   "Trong bài tiếp theo, chúng ta sẽ học cách 'bảo vệ' dữ liệu bên trong các object của mình với **Encapsulation**, và làm cho chúng 'thông minh' hơn với các **Special Methods**."

## 3. Những điểm cần nhấn mạnh bằng ngôn ngữ đơn giản

*   "**Kế thừa là 'con giống cha'.** Con có mọi thứ của cha, và có thể có thêm những thứ của riêng mình."
*   "**Ghi đè (override) là 'con hơn cha'.** Con cũng làm được việc đó, nhưng làm theo cách tốt hơn hoặc khác đi."
*   "**`super()` là 'hỏi ý kiến cha'.** Trước khi làm việc của mình, con hỏi cha làm phần của cha trước."
*   "**Đa hình là 'ra lệnh chung, tự biết cách làm'.** Giống như sếp nói 'báo cáo đi', phòng kế toán sẽ làm báo cáo tài chính, phòng kinh doanh sẽ làm báo cáo doanh số. Cùng một lệnh, mỗi bộ phận tự biết làm báo cáo của mình."

## 4. Những điểm học viên thường bị rối

*   **Khi nào dùng `super()`?** Học viên thường không biết khi nào cần gọi `super()`. Quy tắc đơn giản: "Khi bạn ghi đè một phương thức, nhưng vẫn muốn thực thi cả logic của phương thức gốc ở class cha, hãy dùng `super()`." `__init__` là trường hợp phổ biến nhất.
*   **`super()` vs. `Animal.__init__(self, ...)`:** Giải thích rằng `super()` là cách làm hiện đại và linh hoạt hơn, đặc biệt trong đa kế thừa (sẽ học sau). Khuyên họ luôn dùng `super()`.
*   **Đa hình và Duck Typing:** Học viên có thể hỏi "Vậy chỉ cần có cùng tên method là được, không cần kế thừa cũng được phải không?". Trả lời: "Đúng vậy, đó gọi là Duck Typing trong Python. Tuy nhiên, kế thừa giúp tạo ra một cấu trúc rõ ràng và đảm bảo rằng các class con *chắc chắn* có những method đó. Nó là một mối quan hệ 'is-a' (chó 'là một' loài động vật), mang tính ngữ nghĩa cao hơn."

## 5. Phép ẩn dụ hoặc mô hình tư duy gợi ý

*   **Hệ thống File:**
    *   `BaseFile` (class cha): có `filename`, `size`, method `open()`.
    *   `TextFile(BaseFile)` (con): có thêm method `count_words()`.
    *   `ImageFile(BaseFile)` (con): có thêm method `show_dimensions()`.
    *   `ZipFile(BaseFile)` (con): ghi đè method `open()` để giải nén.
*   **Hệ thống thanh toán:**
    *   `PaymentMethod` (cha): có method `pay(amount)`.
    *   `CreditCardPayment(PaymentMethod)` (con): implement `pay` bằng cách gọi API của ngân hàng.
    *   `PayPalPayment(PaymentMethod)` (con): implement `pay` bằng cách redirect tới trang PayPal.
    *   `CashPayment(PaymentMethod)` (con): implement `pay` bằng cách ghi nhận "thu tiền mặt".

## 6. Gợi ý kịch bản nói về ứng dụng thực tế

*   "Trong xử lý ngôn ngữ tự nhiên (NLP), bạn có thể có một class `BaseTokenizer` và các class con như `WordTokenizer`, `SentenceTokenizer`, `RegexTokenizer`. Tất cả đều có chung method `.tokenize(text)` nhưng thực hiện logic khác nhau."
*   "Khi làm việc với API, bạn có thể có `BaseAPIClient` xử lý việc authentication, logging. Sau đó các class con `FacebookAPIClient`, `GoogleAPIClient` kế thừa từ nó và chỉ cần implement các endpoint cụ thể."
*   "Trong một pipeline data, bạn có thể có `BaseDataCleaner` và các class con `MissingValueCleaner`, `OutlierCleaner`, `DtypeCleaner`. Bạn có thể áp dụng một chuỗi các cleaner này lên DataFrame một cách linh hoạt."

## 7. Tóm tắt cuối bài và cầu nối

"Hôm nay chúng ta đã thấy làm thế nào để code của mình không chỉ chạy được, mà còn có cấu trúc, dễ tái sử dụng và cực kỳ linh hoạt nhờ vào Kế thừa và Đa hình. Chúng ta đã đi từ việc tạo các object riêng lẻ sang việc xây dựng cả một họ các object có liên quan đến nhau."

"Tuy nhiên, các object của chúng ta hiện tại vẫn còn khá 'ngây thơ'. Dữ liệu bên trong chúng có thể bị thay đổi một cách tự do từ bên ngoài. Làm thế nào để bảo vệ dữ liệu và kiểm soát cách nó được thay đổi? Câu trả lời nằm ở **Encapsulation (Tính đóng gói)**, và đó sẽ là chủ đề chính của buổi học tiếp theo."
