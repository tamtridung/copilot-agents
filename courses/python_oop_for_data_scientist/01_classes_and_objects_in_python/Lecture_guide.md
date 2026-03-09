
# Lecture Guide: Bài 1 - Nền tảng Classes và Objects

## 1. Lời hứa của bài học (The Promise)

"Sau buổi học hôm nay, các bạn sẽ nắm được viên gạch nền tảng nhất của lập trình hướng đối tượng: `class` và `object`. Các bạn không chỉ hiểu định nghĩa, mà còn có thể tự tay tạo ra những 'khuôn mẫu' code đầu tiên của mình, giúp tổ chức dữ liệu và hành vi một cách gọn gàng, chuyên nghiệp hơn. Đây là bước đầu tiên để chuyển từ việc viết script đơn giản sang xây dựng các hệ thống phức tạp trong data science."

## 2. Cốt truyện giảng dạy (Teaching Storyline)

1.  **Mở đầu - Nỗi đau của việc code không có tổ chức:**
    *   Bắt đầu bằng việc hỏi học viên: "Có bao giờ bạn copy-paste một đoạn code xử lý dữ liệu ở nhiều nơi không? Có bao giờ bạn nhìn lại file script dài 500 dòng của mình và cảm thấy 'ngợp' không?"
    *   Đây là lúc giới thiệu OOP như một giải pháp, một cách tư duy để giải quyết sự hỗn loạn đó. "OOP không phải là một cú pháp mới, mà là một 'hệ điều hành' mới cho cách chúng ta viết code."

2.  **Tạo sự tò mò - Phép ẩn dụ về Khuôn bánh:**
    *   "Hãy quên code đi một chút. Tưởng tượng chúng ta đang ở trong một tiệm bánh."
    *   Dùng hình ảnh **khuôn bánh (class)** và **cái bánh (object)**. Nhấn mạnh: khuôn bánh chỉ là bản thiết kế, nó không ăn được. Cái bánh mới là sản phẩm thật. Một khuôn có thể tạo ra nhiều cái bánh.
    *   Đây là "aha moment" đầu tiên, giúp học viên có một mô hình tư duy trực quan.

3.  **Chuyển tiếp vào Code - Từ Khuôn bánh đến `class Dog`:**
    *   "Bây giờ, hãy biến cái 'khuôn bánh' đó thành code."
    *   Giới thiệu cú pháp `class Dog: pass`. Giải thích đây là cái khuôn đơn giản nhất, một bản thiết kế trống.
    *   Tạo 2 object `dog1 = Dog()`, `dog2 = Dog()`. In chúng ra để chứng minh chúng là 2 "cái bánh" riêng biệt (có địa chỉ bộ nhớ khác nhau).

4.  **Xây dựng khái niệm từng bước (Step-by-Step):**
    *   **Nhu cầu về trạng thái ban đầu (`__init__`):** "Một chú chó khi sinh ra phải có tên, có tuổi chứ? Làm sao để 'đúc' những thông tin đó vào ngay từ đầu?" -> Giới thiệu `__init__` là "nghi thức khởi tạo".
    *   **Dữ liệu (`attributes`):** "Tên và tuổi đó được lưu ở đâu? Chúng ta gọi chúng là thuộc tính." -> Giới thiệu `self.name = name`. Nhấn mạnh `self` là cách object tự nói về chính nó.
    *   **Hành vi (`methods`):** "Chó thì phải biết sủa. Làm sao để nó 'làm' một việc gì đó?" -> Giới thiệu method `bark(self)`. Nhấn mạnh `self` vẫn là bắt buộc.

5.  **Củng cố bằng ví dụ thực tế (`SimpleDataset`):**
    *   "Lý thuyết vậy đủ rồi, data scientist thì phải làm việc với data."
    *   Chuyển từ ví dụ `Dog` sang `SimpleDataset`. Đây là bước quan trọng để kết nối với công việc hàng ngày của học viên.
    *   **Demo flow:**
        1.  Tạo một DataFrame đơn giản.
        2.  Định nghĩa class `SimpleDataset` với `__init__` nhận vào data, name, description.
        3.  Thêm các method hữu ích: `get_summary()`, `get_shape()`, `show_head()`.
        4.  Tạo một object `employee_data` từ class.
        5.  Gọi các method trên object đó. Nhấn mạnh sự gọn gàng: `employee_data.get_summary()` thay vì phải nhớ tên DataFrame gốc.

6.  **Tổng kết và kết nối (Strong Closing):**
    *   Recap lại 5 khái niệm cốt lõi (class, object, `__init__`, attribute, method) bằng phép ẩn dụ khuôn bánh.
    *   "Hôm nay chúng ta đã xây được 'viên gạch'. Buổi sau, chúng ta sẽ học cách xây những 'bức tường' bằng cách cho các class kế thừa lẫn nhau, một khái niệm gọi là Inheritance."

## 3. Những điểm cần nhấn mạnh bằng ngôn ngữ đơn giản

*   "**Class là bản vẽ, Object là ngôi nhà.** Bạn không thể sống trong bản vẽ, bạn sống trong ngôi nhà."
*   "**`self` là cách object tự gọi mình.** Giống như khi bạn nói 'tôi', bạn đang ám chỉ chính bản thân bạn."
*   "**`__init__` là thứ chạy đầu tiên, tự động, không cần gọi.** Nó giống như việc lắp ráp các bộ phận cho một món đồ chơi ngay khi bạn mở hộp."
*   "**Attribute là danh từ (cái gì), Method là động từ (làm gì).** `dog.name` là một danh từ. `dog.bark()` là một động từ."

## 4. Những điểm học viên thường bị rối

*   **Quên `self`:** Đây là lỗi số 1. Hãy lặp đi lặp lại "tham số đầu tiên của method luôn là `self`". Giải thích rằng Python tự động truyền object vào đó khi bạn gọi `my_object.my_method()`.
*   **Sự khác biệt giữa `name` và `self.name` trong `__init__`:**
    *   `name` (bên phải dấu bằng) là biến tạm thời, là tham số được truyền vào.
    *   `self.name` (bên trái dấu bằng) là thuộc tính, là thứ sẽ "sống" cùng object.
*   **Gọi method trên Class thay vì Object:** `Dog.bark()` là sai. Phải nhấn mạnh rằng "hành động 'sủa' phải do một con chó cụ thể thực hiện, không phải do 'loài chó' nói chung."

## 5. Phép ẩn dụ hoặc mô hình tư duy gợi ý

*   **Class `Car`:**
    *   Attributes: `color`, `brand`, `max_speed` (dữ liệu, tính chất).
    *   Methods: `start_engine()`, `accelerate()`, `brake()` (hành vi, khả năng).
*   **Class `Character` trong game:**
    *   Attributes: `name`, `health`, `mana`, `level`.
    *   Methods: `attack()`, `cast_spell()`, `level_up()`.

## 6. Gợi ý Demo Flow cho phần code

1.  **File 1 (Clean):** Bắt đầu với một notebook trống.
2.  **Cell 1:** Gõ `class Dog: pass`. Chạy.
3.  **Cell 2:** Gõ `d1 = Dog()`, `d2 = Dog()`, `print(d1)`, `print(d2)`. Chạy và chỉ ra sự khác biệt.
4.  **Cell 3:** Sửa lại `class Dog` để có `__init__(self, name, age)`. Cố tình chạy `d1 = Dog()` để thấy lỗi `TypeError: __init__() missing 2 required positional arguments`. Đây là một teaching moment.
5.  **Cell 4:** Sửa lại `d1 = Dog("Milo", 2)`. Chạy thành công. In `d1.name`, `d1.age`.
6.  **Cell 5:** Thêm method `bark(self)`. Gọi `d1.bark()`.
7.  **Chuyển sang ví dụ Data Science:** Lặp lại quy trình tương tự với `SimpleDataset`, nhấn mạnh sự tương đồng trong cấu trúc.

## 7. Gợi ý kịch bản nói về ứng dụng thực tế

*   "Trong một pipeline ETL, bạn có thể tạo một class `DataLoader` để xử lý việc đọc dữ liệu từ nhiều nguồn (CSV, SQL, API). Mỗi nguồn có thể là một object với các method riêng như `.read()`, `.clean()`."
*   "Khi huấn luyện mô hình, bạn có thể tạo class `ModelTrainer` để đóng gói mô hình, các tham số, và các bước training. Bạn có thể dễ dàng tạo nhiều object trainer để thử nghiệm các mô hình khác nhau: `xgb_trainer = ModelTrainer(...)`, `rf_trainer = ModelTrainer(...)`."
*   "Một class `FeatureEngineering` có thể chứa tất cả các bước biến đổi đặc trưng. Điều này giúp bạn tái sử dụng logic feature engineering cho các bộ dữ liệu khác nhau mà không cần copy-paste."

## 8. Tóm tắt cuối bài và cầu nối

"Hôm nay, chúng ta đã học cách tạo ra những 'viên gạch' code đầu tiên bằng `class` và `object`. Chúng ta đã thấy cách đóng gói dữ liệu và hành vi vào một nơi duy nhất, giúp code trở nên sạch sẽ và dễ quản lý hơn. Đây là nền tảng của OOP."

"Tuy nhiên, sức mạnh thực sự của OOP chỉ bộc lộ khi các 'viên gạch' này có thể tương tác với nhau. Trong bài học tiếp theo, chúng ta sẽ khám phá **Inheritance (Tính kế thừa)**, một cách để một class có thể 'thừa hưởng' toàn bộ sức mạnh của một class khác, giúp chúng ta tiết kiệm rất nhiều công sức và xây dựng các hệ thống phức tạp một cách hiệu quả."
