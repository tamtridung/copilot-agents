
# Lecture Guide: Bài 3 - Encapsulation và Special Methods

## 1. Lời hứa của bài học (The Promise)

"Hôm nay, chúng ta sẽ học cách biến những object 'ngây thơ' của mình thành những 'thực thể' thông minh và an toàn. Các bạn sẽ khám phá **Encapsulation (Tính đóng gói)**, nghệ thuật che giấu và bảo vệ dữ liệu bên trong object, giống như một két sắt. Sau đó, chúng ta sẽ mở khóa 'phép thuật' của Python qua các **Special Methods**, cho phép object của bạn hoạt động mượt mà với các hàm `len()`, `print()` và nhiều hơn nữa, làm cho code của bạn không chỉ đúng mà còn rất 'Pythonic'."

## 2. Cốt truyện giảng dạy (Teaching Storyline)

1.  **Mở đầu - Vấn đề của sự "dễ dãi":**
    *   Bắt đầu bằng ví dụ từ bài trước: `my_dog.age = -5`. "Code của chúng ta đã chấp nhận một giá trị vô lý! Object của chúng ta quá 'dễ dãi', ai muốn thay đổi gì cũng được. Điều này rất nguy hiểm trong các hệ thống lớn."
    *   Giới thiệu **Encapsulation** như một giải pháp: "Chúng ta cần xây những 'bức tường' và 'cánh cổng' để kiểm soát ai được phép và không được phép thay đổi dữ liệu bên trong."

2.  **Xây dựng khái niệm Encapsulation:**
    *   **Phép ẩn dụ "Ngôi nhà":**
        *   `public`: Phòng khách, ai cũng vào được.
        *   `_protected`: Phòng ngủ, chỉ người trong nhà (lập trình viên của class và class con) mới nên vào. Khách lạ (code bên ngoài) không nên tự ý vào.
        *   `__private`: Két sắt, chỉ chủ nhân (chính class đó) mới biết cách mở.
    *   **Demo 1: Quy ước đặt tên.**
        1.  Tạo class `MyClass` với 3 loại biến `public_var`, `_protected_var`, `__private_var`.
        2.  Tạo object và thử truy cập cả 3.
        3.  `public_var` và `_protected_var` truy cập được. Nhấn mạnh `_` chỉ là **gợi ý**.
        4.  `__private_var` gây `AttributeError`. Giải thích về *name mangling* (`_ClassName__private_var`) như một cơ chế bảo vệ mạnh hơn.
    *   **Chuyển tiếp sang Properties:** "Vậy làm sao để khách (code bên ngoài) có thể tương tác với dữ liệu một cách an toàn? Qua 'người quản gia' - Properties."

3.  **Properties - Người quản gia thông minh:**
    *   **Demo 2: `@property` và `@setter`**
        1.  Sửa lại class `Person` với `_age`.
        2.  Tạo getter với `@property` cho `age`. "Đây là cách chúng ta cho phép người khác *xem* tuổi."
        3.  Tạo setter với `@age.setter`. "Đây là 'cánh cổng' kiểm soát. Mọi giá trị mới đều phải đi qua đây."
        4.  Trong setter, thêm `if value < 0: raise ValueError`.
        5.  Thử tạo object, gán tuổi hợp lệ, rồi gán tuổi âm để thấy lỗi. Nhấn mạnh rằng cú pháp bên ngoài vẫn là `p.age`, rất đẹp và tự nhiên.

4.  **Chuyển tiếp sang Special Methods:**
    *   "Object của chúng ta đã an toàn, nhưng nó vẫn còn hơi 'vô tri'. Hãy xem điều gì xảy ra khi ta `print()` nó." -> In ra một địa chỉ bộ nhớ khó hiểu.
    *   "Tại sao `print([1, 2])` lại đẹp, còn `print(my_person)` thì không? Vì list có 'phép thuật', và hôm nay chúng ta sẽ học phép thuật đó."
    *   Giới thiệu **Dunder Methods** (`__...__`) là cách để object của chúng ta giao tiếp với thế giới Python.

5.  **Khám phá các Special Methods phổ biến:**
    *   **Demo 3: `__str__` và `__repr__`**
        1.  Thêm `__str__` và `__repr__` vào class `Person`.
        2.  Giải thích sự khác biệt: `__str__` cho người dùng (đẹp, dễ đọc), `__repr__` cho lập trình viên (rõ ràng, tái tạo được object).
        3.  `print(p)` -> gọi `__str__`.
        4.  Gõ `p` trong cell Jupyter -> gọi `__repr__`.
    *   **Demo 4: `__len__` và `__getitem__`**
        *   Chuyển sang ví dụ `SmartDataset`.
        *   Thêm `__len__` trả về `len(self.data)`. Giờ `len(my_dataset)` hoạt động.
        *   Thêm `__getitem__` trả về `self.data[key]`. Giờ `my_dataset['column_name']` hoạt động.

6.  **Tổng hợp tất cả trong ví dụ `SmartDataset`:**
    *   Walkthrough class `SmartDataset` đã hoàn thiện, chỉ ra từng phần:
        *   `__init__` dùng setter để validation.
        *   `data` là một property với getter và setter.
        *   `__str__`, `__repr__`, `__len__`, `__getitem__` làm cho nó hoạt động như một cấu trúc dữ liệu thực thụ.
    *   Chạy các cell test để chứng minh tất cả các tính năng hoạt động cùng nhau một cách hài hòa.

7.  **Tổng kết và kết nối:**
    *   Recap lại Encapsulation (bảo vệ dữ liệu) và Special Methods (tích hợp với Python).
    *   "Chúng ta đã học cách tạo ra những object không chỉ mạnh mẽ về tính năng, mà còn an toàn và dễ sử dụng. Đây là những kỹ năng cốt lõi để viết code OOP chuyên nghiệp."
    *   "Ở các bài học trước, chúng ta đã học các khái niệm riêng lẻ. Trong bài tới, chúng ta sẽ học cách 'tổ hợp' chúng lại thành các **Design Patterns** - những công thức giải quyết vấn đề đã được kiểm chứng trong thực tế."

## 3. Những điểm cần nhấn mạnh bằng ngôn ngữ đơn giản

*   "**Encapsulation là 'việc nhà ai nhà nấy lo'.** Đừng tự ý vào nhà người khác thay đổi đồ đạc. Hãy hỏi chủ nhà (gọi public method)."
*   "**Property là một phương thức 'giả dạng' làm thuộc tính.** Nó trông như một thuộc tính, nhưng bên trong là cả một logic."
*   "**Setter là 'lính gác cổng'.** Mọi dữ liệu đi vào đều phải được kiểm tra."
*   "**Special Methods là cách bạn 'dạy' cho object của mình biết nói tiếng Python.** `__len__` dạy nó cách trả lời câu hỏi 'bạn dài bao nhiêu?'. `__str__` dạy nó cách tự giới thiệu bản thân."

## 4. Những điểm học viên thường bị rối

*   **`_age` và `age`:** Học viên thường bối rối tại sao có cả hai. Giải thích rõ: `_age` là nơi **lưu trữ dữ liệu thực sự** (kho), còn `age` là **cổng giao tiếp** (property). Setter của `age` sẽ ghi vào `_age`. Getter của `age` sẽ đọc từ `_age`.
*   **Khi nào dùng `__str__` vs `__repr__`:** "Khi bạn `print`, bạn muốn thấy gì? Đó là `__str__`. Khi code của bạn crash và bạn muốn xem giá trị của biến lúc đó, bạn muốn thấy gì? Đó là `__repr__`."
*   **Tại sao không dùng `get_age()` và `set_age(value)` kiểu Java?** "Đó cũng là một cách, nhưng `@property` là cách làm 'Pythonic' hơn. Nó cho phép người dùng truy cập thuộc tính một cách tự nhiên (`obj.age`) thay vì phải gọi hàm (`obj.get_age()`), trong khi vẫn giữ được sự an toàn."

## 5. Phép ẩn dụ hoặc mô hình tư duy gợi ý

*   **Thermostat (Cái điều chỉnh nhiệt độ):**
    *   `_temperature`: Nhiệt độ thực tế bên trong.
    *   `temperature` (property):
        *   Getter: Trả về `_temperature`.
        *   Setter: Bạn gán `thermostat.temperature = 25`. Setter sẽ kiểm tra (ví dụ: không cho phép đặt dưới 16 độ hoặc trên 30 độ) rồi mới thay đổi `_temperature`.

## 6. Gợi ý kịch bản nói về ứng dụng thực tế

*   "Trong một class `DataPipeline`, bạn có thể có một thuộc tính `steps` là một list các bước xử lý. Bạn có thể dùng `__len__` để `len(pipeline)` trả về số bước. Dùng `__getitem__` để `pipeline[0]` trả về bước đầu tiên. Dùng `__str__` để `print(pipeline)` in ra một tóm tắt các bước."
*   "Một class `ModelResult` chứa kết quả huấn luyện có thể dùng `__str__` để in ra một bảng tóm tắt đẹp các metrics (accuracy, precision, recall), và dùng `__repr__` để hiển thị các thông tin cần cho việc tái tạo (tên model, ngày huấn luyện, version code)."
*   "Khi bạn viết một class để đọc một định dạng file đặc biệt, bạn có thể implement `__enter__` và `__exit__` để nó hoạt động với câu lệnh `with`, tự động mở và đóng file một cách an toàn."

## 7. Tóm tắt cuối bài và cầu nối

"Hôm nay, chúng ta đã trang bị cho các object của mình 'giáp trụ' (Encapsulation) và 'kỹ năng giao tiếp' (Special Methods). Code của chúng ta giờ đây không chỉ hoạt động đúng mà còn an toàn, linh hoạt và đẹp mắt. Bạn đã tiến một bước rất dài trên con đường trở thành một lập trình viên Python chuyên nghiệp."

"Với tất cả những công cụ mạnh mẽ này - Classes, Inheritance, Polymorphism, và Encapsulation - chúng ta đã sẵn sàng để xây dựng những thứ lớn lao hơn. Trong bài học tiếp theo, chúng ta sẽ không học thêm khái niệm mới, mà sẽ học cách kết hợp chúng lại thành các **Design Patterns**, những 'bí kíp' được đúc kết để giải quyết các vấn-đề-thường-gặp trong thiết kế phần mềm cho khoa học dữ liệu."
