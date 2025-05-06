# LỚP VÀ ĐỐI TƯỢNG


## 1. Giới thiệu Lập trình Hướng đối tượng (OOP) và Java

**Lập trình Hướng đối tượng (Object-Oriented Programming - OOP)** là một mô hình (paradigm) lập trình mạnh mẽ, cho phép tổ chức mã nguồn xoay quanh các "đối tượng" thay vì chỉ tập trung vào các hàm và logic tuần tự. OOP giúp mô hình hóa các thực thể trong thế giới thực (như Người, Xe hơi, Tài khoản ngân hàng) thành các cấu trúc dữ liệu và hành vi tương ứng trong phần mềm.

**Tại sao OOP lại quan trọng?**

* **Mô hình hóa thực tế:** Giúp ánh xạ các khái niệm phức tạp của thế giới thực vào mã nguồn một cách tự nhiên hơn.
* **Tái sử dụng mã (Code Reusability):** Thông qua các cơ chế như kế thừa, các lớp có thể dùng lại thuộc tính và phương thức của các lớp khác.
* **Tính đóng gói (Encapsulation):** Che giấu dữ liệu nội bộ và chỉ cung cấp các giao diện công khai để tương tác, giúp bảo vệ dữ liệu và giảm sự phụ thuộc lẫn nhau giữa các thành phần.
* **Dễ bảo trì và mở rộng:** Mã nguồn được tổ chức thành các module (lớp) độc lập tương đối, giúp việc sửa lỗi, nâng cấp hoặc thêm tính năng mới trở nên dễ dàng hơn.
* **Tính đa hình (Polymorphism):** Cho phép các đối tượng khác nhau phản ứng với cùng một thông điệp (lời gọi phương thức) theo cách riêng của chúng.

**Java và OOP:** Java là một ngôn ngữ được thiết kế từ đầu với tư tưởng hướng đối tượng mạnh mẽ. Hầu hết mọi thứ trong Java đều liên quan đến các lớp và đối tượng. Việc hiểu rõ các khái niệm cốt lõi như **Lớp (Class)** và **Đối tượng (Object)** là nền tảng bắt buộc để lập trình hiệu quả với Java.

* **Lớp (Class):** Là một bản thiết kế (blueprint) hoặc khuôn mẫu định nghĩa các thuộc tính (dữ liệu) và phương thức (hành vi) chung cho một loại đối tượng. Ví dụ: Lớp `NguoiDung` định nghĩa rằng mọi người dùng sẽ có `tenDangNhap`, `matKhau`, và có thể `dangNhap()`, `dangXuat()`.
* **Đối tượng (Object):** Là một thực thể cụ thể (instance) được tạo ra từ một lớp. Mỗi đối tượng có trạng thái riêng (giá trị cụ thể cho các thuộc tính) và có thể thực hiện các hành vi được định nghĩa trong lớp. Ví dụ: `userA` và `userB` là hai đối tượng cụ thể của lớp `NguoiDung`, mỗi người có `tenDangNhap` và `matKhau` riêng.

## 2. Khai báo Lớp (Class) trong Java

Lớp là đơn vị cấu trúc cơ bản trong Java. Để tạo ra các đối tượng, trước tiên bạn cần định nghĩa lớp của chúng.

### 2.1. Cú pháp cơ bản

Sử dụng từ khóa `class` để khai báo một lớp:

```java
[access_modifier] class ClassName {
    // Class Body: Chứa các thành phần của lớp
    // 1. Thuộc tính (Fields / Instance Variables)
    // 2. Phương thức (Methods / Instance Methods)
    // 3. Hàm tạo (Constructors)
    // 4. Khối khởi tạo (Initializer Blocks) - Nâng cao
    // 5. Lớp lồng nhau (Nested Classes) - Nâng cao
}
```

* `access_modifier` (ví dụ: `public`, `default` (không ghi gì)): Kiểm soát phạm vi truy cập của lớp. Chúng ta sẽ tìm hiểu kỹ hơn về access modifiers sau.
* `class`: Từ khóa bắt buộc để khai báo lớp.
* `ClassName`: Tên của lớp bạn muốn định nghĩa.
* `{ ... }`: Khối mã chứa toàn bộ nội dung của lớp (class body).

### 2.2. Quy tắc đặt tên lớp (Naming Conventions)

Việc tuân thủ quy tắc đặt tên giúp mã nguồn dễ đọc và dễ bảo trì hơn.

* **Danh từ (Noun):** Tên lớp nên là một danh từ hoặc cụm danh từ mô tả loại đối tượng mà nó đại diện (ví dụ: `Person`, `Student`, `ShoppingCart`, `DatabaseConnection`).
* **CamelCase (UpperCamelCase):** Viết hoa chữ cái đầu tiên của mỗi từ trong tên lớp (ví dụ: `MyClassName`, `HttpRequest`).
* **Đơn giản và có ý nghĩa:** Tên lớp cần rõ ràng, thể hiện đúng mục đích của lớp.
* **Tránh từ khóa Java:** Không được đặt tên lớp trùng với các từ khóa của Java (như `class`, `int`, `public`, `static`,...).
* **Không bắt đầu bằng số:** Tên lớp không được bắt đầu bằng chữ số. Có thể bắt đầu bằng chữ cái, ký tự `$` hoặc `_` (nhưng `$` và `_` ít được dùng cho tên lớp thông thường).

### 2.3. Thân lớp (Class Body)

Đây là nơi bạn định nghĩa các thành phần cấu tạo nên lớp, bao gồm:

* **Thuộc tính (Fields):** Các biến lưu trữ trạng thái dữ liệu của đối tượng.
* **Phương thức (Methods):** Các hàm định nghĩa hành vi, khả năng của đối tượng.
* **Hàm tạo (Constructors):** Các phương thức đặc biệt dùng để khởi tạo đối tượng khi nó được tạo ra.

## 3. Thuộc tính (Fields / Attributes / Instance Variables)

Thuộc tính đại diện cho dữ liệu hoặc trạng thái của một đối tượng. Mỗi đối tượng của cùng một lớp sẽ có cùng tập hợp các thuộc tính, nhưng giá trị của các thuộc tính đó có thể khác nhau giữa các đối tượng.

### 3.1. Định nghĩa và Mục đích

Thuộc tính lưu trữ thông tin mà một đối tượng cần "biết" hoặc "có". Ví dụ, một đối tượng `SinhVien` có thể có các thuộc tính như `maSV`, `hoTen`, `ngaySinh`, `diemTrungBinh`.

### 3.2. Cú pháp khai báo

```java
[access_modifier] dataType fieldName [= initialValue];
```

* `access_modifier` (ví dụ: `public`, `private`, `protected`, `default`): Kiểm soát ai có thể truy cập thuộc tính này. **Thực hành tốt nhất là sử dụng `private` để đảm bảo tính đóng gói.**
* `dataType`: Kiểu dữ liệu của thuộc tính (ví dụ: `int`, `double`, `String`, `boolean`, hoặc tên một lớp khác).
* `fieldName`: Tên của thuộc tính, thường tuân theo quy tắc `lowerCamelCase` (ví dụ: `studentId`, `firstName`).
* `[= initialValue]`: (Tùy chọn) Cung cấp giá trị khởi tạo ban đầu cho thuộc tính.

### 3.3. Ví dụ

```java
public class Student {
    private String studentId; // Thuộc tính kiểu String, private
    private String fullName;  // Thuộc tính kiểu String, private
    private int age;          // Thuộc tính kiểu int, private
    private double gpa = 0.0; // Thuộc tính kiểu double, private, có giá trị khởi tạo
    private boolean isActive; // Thuộc tính kiểu boolean, private
}
```

### 3.4. Giá trị mặc định (Default Values)

Nếu bạn không gán giá trị khởi tạo ban đầu khi khai báo thuộc tính, Java sẽ tự động gán giá trị mặc định dựa trên kiểu dữ liệu:

* Kiểu số nguyên (byte, short, int, long): `0`
* Kiểu số thực (float, double): `0.0`
* Kiểu boolean: `false`
* Kiểu char: `\u0000` (ký tự null)
* Kiểu tham chiếu (String, hoặc bất kỳ đối tượng nào khác): `null`

**Quan trọng:** Việc một thuộc tính tham chiếu có giá trị `null` nghĩa là nó chưa trỏ đến bất kỳ đối tượng nào trong bộ nhớ. Cố gắng gọi phương thức trên một biến `null` sẽ gây ra lỗi `NullPointerException` - một lỗi rất phổ biến trong Java.

## 4. Phương thức (Methods / Instance Methods)

Phương thức định nghĩa các hành động hoặc hành vi mà một đối tượng có thể thực hiện. Chúng thao tác trên dữ liệu (thuộc tính) của đối tượng hoặc thực hiện các tác vụ liên quan.

### 4.1. Định nghĩa và Mục đích

Phương thức cho phép đối tượng "làm" một điều gì đó. Ví dụ, đối tượng `TaiKhoanNganHang` có thể có các phương thức `rutTien()`, `napTien()`, `kiemTraSoDu()`.

### 4.2. Cú pháp khai báo

```java
[access_modifier] [modifier...] returnType methodName([parameterList]) [throws ExceptionType...] {
    // Method Body: Chứa các câu lệnh thực thi hành vi
    // Có thể truy cập các thuộc tính và gọi các phương thức khác của cùng đối tượng
    // Có thể có câu lệnh return (nếu returnType không phải void)
}
```

* `access_modifier`: (`public`, `private`, `protected`, `default`) Kiểm soát truy cập phương thức. Thường là `public` cho các hành vi chính của đối tượng.
* `[modifier...]`: Các từ khóa bổ sung như `static`, `final`, `abstract` (sẽ tìm hiểu sau).
* `returnType`: Kiểu dữ liệu của giá trị mà phương thức trả về. Dùng `void` nếu phương thức không trả về giá trị nào.
* `methodName`: Tên của phương thức, tuân theo quy tắc `lowerCamelCase` (ví dụ: `calculateSum`, `getUserInfo`). Tên thường là động từ hoặc cụm động từ.
* `[parameterList]`: (Tùy chọn) Danh sách các tham số đầu vào mà phương thức cần để thực hiện công việc, cách nhau bởi dấu phẩy. Mỗi tham số có dạng `dataType parameterName`.
* `[throws ExceptionType...]`: (Tùy chọn) Khai báo các loại ngoại lệ (exception) mà phương thức có thể ném ra.
* `{ ... }`: Thân phương thức chứa logic thực thi.

### 4.3. Ví dụ

```java
public class Calculator {
    // Phương thức cộng hai số nguyên, trả về kết quả kiểu int
    public int add(int num1, int num2) {
        int sum = num1 + num2;
        return sum; // Trả về giá trị
    }

    // Phương thức in lời chào, không trả về giá trị (void)
    public void displayGreeting(String name) {
        System.out.println("Hello, " + name + "!");
        // Không có lệnh return giá trị
    }
}
```

### 4.4. Phân biệt Phương thức và Hàm (trong lập trình thủ tục)

Trong OOP, "phương thức" là thuật ngữ dùng cho các hàm được định nghĩa bên trong một lớp và thường thao tác trên dữ liệu của đối tượng thuộc lớp đó (instance methods). Lập trình thủ tục (ví dụ: ngôn ngữ C) thường dùng thuật ngữ "hàm" cho các đơn vị mã thực thi độc lập. Trong Java, gần như mọi mã thực thi đều nằm trong các phương thức của các lớp.

## 5. Hàm tạo (Constructors)

Hàm tạo là một loại phương thức đặc biệt được tự động gọi khi một đối tượng mới của lớp được tạo ra bằng từ khóa `new`. Mục đích chính của nó là khởi tạo trạng thái ban đầu cho đối tượng.

### 5.1. Mục đích

Đảm bảo đối tượng ở trạng thái hợp lệ ngay sau khi được tạo. Ví dụ, gán giá trị ban đầu cho các thuộc tính quan trọng, thiết lập kết nối, hoặc thực hiện các cài đặt cần thiết.

### 5.2. Đặc điểm

* **Tên trùng với tên lớp:** Hàm tạo phải có tên giống hệt tên lớp (phân biệt chữ hoa/thường).
* **Không có kiểu trả về tường minh:** Không khai báo `void` hay bất kỳ kiểu dữ liệu nào khác trước tên hàm tạo.
* **Có thể có Access Modifier:** Thường là `public` để có thể tạo đối tượng từ bất kỳ đâu. `private` constructor được dùng trong các mẫu thiết kế như Singleton hoặc Factory.
* **Có thể có tham số:** Giống như phương thức thông thường.

### 5.3. Constructor Mặc định (Default Constructor)

Nếu bạn **không** khai báo bất kỳ hàm tạo nào trong lớp của mình, trình biên dịch Java sẽ tự động thêm một hàm tạo mặc định (không có tham số và không có nội dung trong thân hàm).

```java
public class MyClass {
    // Không có constructor nào được khai báo
    // Java sẽ tự động thêm:
    // public MyClass() {
    //     // Gọi constructor của lớp cha (super();) - ngầm định
    // }
}
```

**Lưu ý quan trọng:** Nếu bạn tự định nghĩa **bất kỳ** hàm tạo nào (dù có tham số hay không), Java sẽ **không** tự động thêm hàm tạo mặc định nữa. Nếu bạn vẫn muốn có một hàm tạo không tham số, bạn phải tự khai báo nó.

### 5.4. Constructor Có Tham số (Parameterized Constructor)

Cho phép bạn truyền các giá trị vào khi tạo đối tượng để khởi tạo các thuộc tính.

```java
public class Person {
    private String name;
    private int age;

    // Constructor có tham số
    public Person(String initialName, int initialAge) {
        System.out.println("Creating a Person object...");
        this.name = initialName; // Sử dụng 'this' để phân biệt thuộc tính và tham số
        this.age = initialAge;
    }

    // Getter methods (sẽ nói ở phần Encapsulation)
    public String getName() { return name; }
    public int getAge() { return age; }
}
```

### 5.5. Nạp chồng Constructor (Constructor Overloading)

Một lớp có thể có nhiều hàm tạo, miễn là chúng có danh sách tham số khác nhau (khác về số lượng hoặc kiểu dữ liệu của tham số). Điều này cho phép tạo đối tượng theo nhiều cách khác nhau.

```java
public class Rectangle {
    private double length;
    private double width;

    // Constructor 1: Không tham số (tạo hình chữ nhật mặc định)
    public Rectangle() {
        this.length = 1.0;
        this.width = 1.0;
        System.out.println("Default Rectangle created.");
    }

    // Constructor 2: Có tham số cho cả chiều dài và rộng
    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
        System.out.println("Rectangle with length=" + length + ", width=" + width + " created.");
    }

    // Constructor 3: Tạo hình vuông (chỉ cần 1 cạnh)
    public Rectangle(double side) {
        this.length = side;
        this.width = side;
        System.out.println("Square (Rectangle with side=" + side + ") created.");
    }

    // ... các phương thức khác như calculateArea(), calculatePerimeter() ...
}
```

### 5.6. Ví dụ sử dụng các constructor nạp chồng

```java
Rectangle rect1 = new Rectangle(); // Gọi constructor 1
Rectangle rect2 = new Rectangle(10.0, 5.0); // Gọi constructor 2
Rectangle square = new Rectangle(7.0); // Gọi constructor 3
```

## 6. Khởi tạo và Sử dụng Đối tượng (Objects)

Sau khi đã định nghĩa lớp (bản thiết kế), bạn có thể tạo ra các đối tượng (thực thể cụ thể) từ lớp đó.

### 6.1. Từ khóa `new`

Toán tử `new` được sử dụng để:

1.  **Cấp phát bộ nhớ:** Yêu cầu hệ thống cấp phát đủ dung lượng bộ nhớ trên **Heap** để chứa dữ liệu của đối tượng mới.
2.  **Gọi Hàm tạo:** Thực thi hàm tạo tương ứng của lớp (dựa trên các đối số bạn truyền vào) để khởi tạo trạng thái ban đầu cho đối tượng.
3.  **Trả về Tham chiếu:** Trả về một tham chiếu (địa chỉ bộ nhớ) trỏ đến đối tượng vừa được tạo trên Heap.

### 6.2. Cú pháp tạo đối tượng

```java
ClassName objectName = new ClassName(arguments...);
```

* `ClassName`: Tên của lớp mà bạn muốn tạo đối tượng.
* `objectName`: Tên của biến tham chiếu. Biến này sẽ được lưu trữ trên **Stack** và giữ địa chỉ của đối tượng trên Heap.
* `new ClassName(arguments...)`: Phần thực hiện việc cấp phát và gọi constructor (truyền các `arguments` nếu constructor yêu cầu).

### 6.3. Biến tham chiếu (Reference Variables)

Biến `objectName` không chứa bản thân đối tượng mà chỉ chứa *địa chỉ* nơi đối tượng đó được lưu trữ trong bộ nhớ Heap. Nhiều biến tham chiếu có thể cùng trỏ đến một đối tượng duy nhất.

```java
Person person1 = new Person("Alice", 30);
Person person2 = person1; // person2 bây giờ cũng trỏ đến đối tượng Person mà person1 đang trỏ tới

person2.setName("Alice Smith"); // Thay đổi tên qua person2

System.out.println(person1.getName()); // Output: Alice Smith (vì cả hai cùng trỏ đến 1 đối tượng)
```

### 6.4. Quá trình trong bộ nhớ (Stack và Heap)

* **Stack:** Là vùng nhớ quản lý các lời gọi phương thức và các biến cục bộ (bao gồm cả các biến tham chiếu đối tượng). Khi một phương thức kết thúc, các biến cục bộ của nó trên Stack sẽ bị xóa.
* **Heap:** Là vùng nhớ lớn hơn, nơi các đối tượng thực sự được lưu trữ khi bạn dùng `new`. Các đối tượng trên Heap tồn tại miễn là còn ít nhất một tham chiếu trỏ đến chúng. Khi không còn tham chiếu nào, bộ dọn rác (Garbage Collector - GC) của Java có thể thu hồi vùng nhớ đó.

**Ví dụ minh họa:**

```java
// 1. Khai báo biến tham chiếu trên Stack
Person worker; // worker trên Stack, giá trị ban đầu là null

// 2. Tạo đối tượng trên Heap, gán địa chỉ cho worker
worker = new Person("Bob", 25); // new Person(...) tạo đối tượng trên Heap
                               // worker trên Stack giờ lưu địa chỉ của đối tượng đó

// 3. Truy cập đối tượng thông qua tham chiếu
System.out.println(worker.getName()); // Dùng worker (Stack) để tìm đối tượng (Heap) và gọi getName()
```

### 6.5. Truy xuất Thuộc tính và Gọi Phương thức

Sử dụng **toán tử dấu chấm (`.`)** để truy cập các thuộc tính và gọi các phương thức của một đối tượng thông qua biến tham chiếu của nó.

```java
ClassName objectName = new ClassName(...);

// Truy cập thuộc tính (chỉ nên làm nếu thuộc tính là public - không khuyến khích)
// objectName.fieldName = value;
// variable = objectName.fieldName;

// Gọi phương thức
// objectName.methodName(arguments...);
// result = objectName.methodName(arguments...);
```

**Ví dụ:**

```java
Rectangle myRect = new Rectangle(15.0, 8.0);

// Gọi phương thức để tính diện tích (giả sử có phương thức này)
// double area = myRect.calculateArea();
// System.out.println("Diện tích: " + area);

// Truy cập thuộc tính qua getter (khuyến khích)
// double length = myRect.getLength(); // Giả sử có getter getLength()
// System.out.println("Chiều dài: " + length);
```

**Quan trọng:** Quyền truy cập (có được phép truy cập thuộc tính hay gọi phương thức đó từ bên ngoài lớp hay không) phụ thuộc vào `access modifier` (ví dụ: `public`, `private`) được khai báo cho thuộc tính/phương thức đó.

## 7. Tính Đóng gói (Encapsulation)

Đây là một trong bốn trụ cột chính của OOP. Encapsulation là kỹ thuật **che giấu thông tin (information hiding)**, tức là ẩn đi các chi tiết triển khai nội bộ của một lớp và chỉ cung cấp một giao diện công khai (public interface) để tương tác với nó.

### 7.1. Khái niệm và Lợi ích

* **Che giấu dữ liệu:** Các thuộc tính (trạng thái nội bộ) của đối tượng thường được khai báo là `private`, ngăn chặn việc truy cập và sửa đổi trực tiếp từ bên ngoài lớp.
* **Kiểm soát truy cập:** Việc truy cập và thay đổi dữ liệu phải thông qua các phương thức công khai (public methods), thường là getters và setters. Điều này cho phép lớp kiểm soát cách dữ liệu được đọc và ghi (ví dụ: thêm logic kiểm tra tính hợp lệ - validation - trong setter).
* **Tăng tính module hóa:** Giảm sự phụ thuộc giữa các phần của hệ thống. Nếu chi tiết nội bộ của lớp thay đổi nhưng giao diện công khai giữ nguyên, các lớp khác sử dụng nó sẽ không bị ảnh hưởng.
* **Dễ bảo trì và gỡ lỗi:** Khi dữ liệu bị sai, phạm vi tìm lỗi được thu hẹp vào các phương thức truy cập của lớp đó.

### 7.2. Từ khóa `private`

Là access modifier có mức độ hạn chế truy cập cao nhất. Các thành viên (thuộc tính, phương thức, constructor) được khai báo là `private` chỉ có thể được truy cập từ **bên trong** chính lớp đó.

```java
public class BankAccount {
    private String accountNumber; // Chỉ truy cập được bên trong BankAccount
    private double balance;       // Chỉ truy cập được bên trong BankAccount

    // ... constructors and methods ...
}
```

### 7.3. Phương thức Truy cập (Accessor Methods): Getters và Setters

Để cho phép bên ngoài tương tác có kiểm soát với các thuộc tính `private`, chúng ta cung cấp các phương thức `public`:

* **Getter:** Phương thức dùng để **lấy (get)** giá trị của một thuộc tính.
    * Tên thường bắt đầu bằng `get` theo sau là tên thuộc tính (viết hoa chữ cái đầu - CamelCase). Ví dụ: `getBalance()`, `getAccountNumber()`.
    * Đối với thuộc tính kiểu `boolean`, tên getter thường bắt đầu bằng `is`. Ví dụ: `isActive()`, `isEnabled()`.
    * Kiểu trả về của getter phải trùng với kiểu dữ liệu của thuộc tính.
    * Thường không có tham số.

* **Setter:** Phương thức dùng để **thiết lập (set)** giá trị mới cho một thuộc tính.
    * Tên thường bắt đầu bằng `set` theo sau là tên thuộc tính (viết hoa chữ cái đầu - CamelCase). Ví dụ: `setBalance(double newBalance)`, `setAccountNumber(String accNum)`.
    * Thường có kiểu trả về là `void`.
    * Có một tham số đầu vào với kiểu dữ liệu trùng với kiểu của thuộc tính, dùng để nhận giá trị mới.
    * **Đây là nơi lý tưởng để thêm logic kiểm tra (validation)** trước khi gán giá trị mới cho thuộc tính.

### 7.4. Ví dụ: Lớp `BankAccount` với Encapsulation

```java
public class BankAccount {
    private String accountNumber;
    private double balance;
    private String ownerName;

    // Constructor
    public BankAccount(String accountNumber, String ownerName, double initialBalance) {
        this.accountNumber = accountNumber;
        this.ownerName = ownerName;
        // Sử dụng setter để kiểm tra số dư ban đầu
        setBalance(initialBalance);
    }

    // --- Getters ---
    public String getAccountNumber() {
        return accountNumber;
    }

    public double getBalance() {
        return balance;
    }

    public String getOwnerName() {
        return ownerName;
    }

    // --- Setters ---
    // Setter cho balance có kiểm tra giá trị không âm
    public void setBalance(double newBalance) {
        if (newBalance >= 0) {
            this.balance = newBalance;
        } else {
            System.err.println("Lỗi: Số dư không thể là số âm.");
            // Hoặc ném ra một Exception
            // throw new IllegalArgumentException("Số dư không thể âm.");
        }
    }

    // Không nên có setter cho accountNumber nếu nó là định danh duy nhất không đổi
    // public void setAccountNumber(String accountNumber) { this.accountNumber = accountNumber; }

    // Có thể có setter cho ownerName nếu tên chủ sở hữu có thể thay đổi
    public void setOwnerName(String ownerName) {
        if (ownerName != null && !ownerName.trim().isEmpty()) {
             this.ownerName = ownerName.trim();
        } else {
             System.err.println("Lỗi: Tên chủ sở hữu không được để trống.");
        }
    }


    // --- Other Methods ---
    public void deposit(double amount) {
        if (amount > 0) {
            this.balance += amount;
            System.out.println("Nạp tiền thành công: " + amount);
        } else {
            System.err.println("Lỗi: Số tiền nạp phải lớn hơn 0.");
        }
    }

    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= this.balance) {
            this.balance -= amount;
            System.out.println("Rút tiền thành công: " + amount);
            return true;
        } else if (amount <= 0) {
            System.err.println("Lỗi: Số tiền rút phải lớn hơn 0.");
            return false;
        } else { // amount > balance
            System.err.println("Lỗi: Số dư không đủ.");
            return false;
        }
    }
}
```

**Cách sử dụng:**

```java
BankAccount acc1 = new BankAccount("123456", "Nguyen Van A", 1000.0);

System.out.println("Chủ tài khoản: " + acc1.getOwnerName()); // OK
System.out.println("Số dư ban đầu: " + acc1.getBalance()); // OK

acc1.deposit(500.0);
acc1.withdraw(200.0);
acc1.withdraw(1500.0); // Sẽ báo lỗi số dư không đủ

// Không thể truy cập trực tiếp:
// acc1.balance = -500.0; // Lỗi biên dịch vì balance là private

// Phải dùng setter (nếu có và nếu hợp lý)
acc1.setBalance(2000.0); // OK, setter sẽ gán giá trị
acc1.setBalance(-100.0); // Setter sẽ báo lỗi và không thay đổi giá trị

acc1.setOwnerName("   Tran Thi B   "); // Setter sẽ trim khoảng trắng
System.out.println("Chủ tài khoản mới: " + acc1.getOwnerName()); // Output: Tran Thi B
```

## 8. Từ khóa `this`

Từ khóa `this` trong Java là một biến tham chiếu đặc biệt, luôn **tham chiếu đến đối tượng hiện tại** (đối tượng mà phương thức hoặc constructor đang được gọi trên đó).

### 8.1. Mục đích

`this` được sử dụng bên trong các phương thức instance (không phải static) và constructor để:

1.  Phân biệt giữa thuộc tính (instance variable) và tham số hoặc biến cục bộ có cùng tên.
2.  Gọi một constructor khác của cùng lớp từ bên trong một constructor (constructor chaining).
3.  Truyền đối tượng hiện tại như một tham số cho một phương thức khác.
4.  Trả về đối tượng hiện tại từ một phương thức (thường dùng trong builder pattern).

### 8.2. Trường hợp sử dụng phổ biến

#### 8.2.1. Phân biệt thuộc tính và tham số/biến cục bộ

Đây là trường hợp sử dụng phổ biến nhất, đặc biệt trong các constructor và setter.

```java
public class Point {
    private int x;
    private int y;

    public Point(int x, int y) {
        // this.x là thuộc tính của đối tượng Point
        // x là tham số đầu vào của constructor
        this.x = x;
        this.y = y;
    }

    public void setX(int x) {
        // this.x là thuộc tính
        // x là tham số
        this.x = x;
    }

    public void move(int deltaX, int deltaY) {
        int x; // Biến cục bộ x (ít dùng trùng tên thế này)
        x = 10;

        this.x += deltaX; // Thay đổi thuộc tính x của đối tượng
        this.y += deltaY; // Thay đổi thuộc tính y của đối tượng
        System.out.println("Biến cục bộ x: " + x); // In ra 10
        System.out.println("Thuộc tính x: " + this.x); // In ra giá trị mới của thuộc tính x
    }
}
```

Nếu không dùng `this` trong constructor `Point(int x, int y)` và viết `x = x;`, trình biên dịch sẽ hiểu là bạn đang gán giá trị của tham số `x` cho chính nó, thuộc tính `this.x` sẽ không bị thay đổi (và giữ giá trị mặc định là 0).

#### 8.2.2. Gọi constructor khác (Constructor Chaining)

Bạn có thể gọi một constructor khác của cùng lớp từ bên trong một constructor bằng cách sử dụng `this(...)`.

* Lời gọi `this(...)` phải là **câu lệnh đầu tiên** trong constructor.
* Giúp tránh lặp lại mã khởi tạo.

```java
public class Rectangle {
    private double length;
    private double width;

    // Constructor chính, thực hiện việc gán giá trị
    public Rectangle(double length, double width) {
        if (length <= 0 || width <= 0) {
            throw new IllegalArgumentException("Dimensions must be positive");
        }
        this.length = length;
        this.width = width;
        System.out.println("Rectangle(double, double) called");
    }

    // Constructor tạo hình vuông, gọi constructor chính
    public Rectangle(double side) {
        this(side, side); // Gọi constructor Rectangle(double, double)
                          // Phải là dòng đầu tiên
        System.out.println("Rectangle(double) for square called");
    }

    // Constructor mặc định, gọi constructor tạo hình vuông mặc định (ví dụ 1x1)
    public Rectangle() {
        this(1.0); // Gọi constructor Rectangle(double)
                   // Phải là dòng đầu tiên
        System.out.println("Rectangle() default called");
    }

    // Getters...
}
```

**Cách hoạt động khi gọi `new Rectangle()`:**
1.  `Rectangle()` được gọi.
2.  `this(1.0)` được thực thi -> gọi `Rectangle(double side)` với `side = 1.0`.
3.  `Rectangle(1.0)` được gọi.
4.  `this(1.0, 1.0)` được thực thi -> gọi `Rectangle(double length, double width)` với `length=1.0, width=1.0`.
5.  `Rectangle(1.0, 1.0)` thực thi, gán `this.length=1.0`, `this.width=1.0`, in "Rectangle(double, double) called".
6.  Quay lại `Rectangle(1.0)`, thực thi phần còn lại, in "Rectangle(double) for square called".
7.  Quay lại `Rectangle()`, thực thi phần còn lại, in "Rectangle() default called".

#### 8.2.3. Truyền đối tượng hiện tại làm tham số

```java
public class EventProcessor {
    public void register(EventListener listener) {
        // Giả sử phương thức này cần chính đối tượng EventListener
        listener.onRegister(this); // Truyền đối tượng EventProcessor hiện tại
    }
}

public interface EventListener {
    void onRegister(EventProcessor processor); // Phương thức nhận đối tượng EventProcessor
    // ... other methods
}
```

#### 8.2.4. Trả về đối tượng hiện tại

Thường dùng trong các phương thức setter của mẫu Builder hoặc để tạo giao diện gọi phương thức liên tiếp (fluent interface).

```java
public class EmailBuilder {
    private String to;
    private String subject;
    private String body;

    public EmailBuilder setTo(String to) {
        this.to = to;
        return this; // Trả về chính đối tượng EmailBuilder
    }

    public EmailBuilder setSubject(String subject) {
        this.subject = subject;
        return this; // Trả về chính đối tượng EmailBuilder
    }

    public EmailBuilder setBody(String body) {
        this.body = body;
        return this; // Trả về chính đối tượng EmailBuilder
    }

    public Email build() {
        // Giả sử có lớp Email
        return new Email(to, subject, body);
    }
}

// Sử dụng:
Email email = new EmailBuilder()
                 .setTo("recipient@example.com")
                 .setSubject("Hello")
                 .setBody("This is the email body.")
                 .build();
```

## 9. Ví dụ Tổng hợp & Trường hợp Sử dụng Thực tế

Hãy xây dựng một lớp `Product` hoàn chỉnh hơn để quản lý thông tin sản phẩm trong một cửa hàng.

```java
import java.time.LocalDate;
import java.util.Objects;

public class Product {
    // --- Thuộc tính (private để đóng gói) ---
    private final String productId; // ID là duy nhất và không đổi sau khi tạo
    private String name;
    private String description;
    private double price;
    private int stockQuantity;
    private final LocalDate dateAdded; // Ngày thêm sản phẩm, không đổi

    // --- Constructor ---
    public Product(String productId, String name, double price, int initialStock) {
        // Kiểm tra tính hợp lệ của đầu vào cơ bản
        if (productId == null || productId.trim().isEmpty()) {
            throw new IllegalArgumentException("Product ID cannot be null or empty.");
        }
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Product name cannot be null or empty.");
        }

        this.productId = productId.trim();
        this.name = name.trim();
        setPrice(price); // Sử dụng setter để kiểm tra giá
        setStockQuantity(initialStock); // Sử dụng setter để kiểm tra số lượng
        this.dateAdded = LocalDate.now(); // Gán ngày hiện tại khi tạo
        this.description = ""; // Khởi tạo mô tả rỗng
    }

    // --- Getters ---
    public String getProductId() {
        return productId;
    }

    public String getName() {
        return name;
    }

    public String getDescription() {
        return description;
    }

    public double getPrice() {
        return price;
    }

    public int getStockQuantity() {
        return stockQuantity;
    }

    public LocalDate getDateAdded() {
        return dateAdded;
    }

    // --- Setters (chỉ cho những thuộc tính có thể thay đổi và cần kiểm soát) ---
    public void setName(String name) {
        if (name != null && !name.trim().isEmpty()) {
            this.name = name.trim();
        } else {
            System.err.println("Warning: Product name cannot be set to null or empty.");
        }
    }

    public void setDescription(String description) {
        this.description = (description == null) ? "" : description; // Cho phép mô tả null thành rỗng
    }

    public void setPrice(double price) {
        if (price >= 0) {
            this.price = price;
        } else {
            System.err.println("Warning: Price cannot be negative. Setting to 0.");
            this.price = 0;
            // Hoặc ném Exception nếu muốn chặt chẽ hơn
            // throw new IllegalArgumentException("Price cannot be negative.");
        }
    }

    // Setter cho stockQuantity nên được xử lý qua các phương thức nghiệp vụ hơn
    // Nhưng vẫn có thể cung cấp setter nếu cần thiết lập lại trực tiếp (cẩn thận)
    public void setStockQuantity(int stockQuantity) {
        if (stockQuantity >= 0) {
            this.stockQuantity = stockQuantity;
        } else {
             System.err.println("Warning: Stock quantity cannot be negative. Setting to 0.");
             this.stockQuantity = 0;
             // throw new IllegalArgumentException("Stock quantity cannot be negative.");
        }
    }


    // --- Phương thức nghiệp vụ (Business Logic Methods) ---

    /**
     * Kiểm tra xem sản phẩm có còn hàng hay không.
     * @return true nếu số lượng tồn kho > 0, ngược lại false.
     */
    public boolean isInStock() {
        return this.stockQuantity > 0;
    }

    /**
     * Tăng số lượng tồn kho (ví dụ: khi nhập hàng).
     * @param amount Số lượng cần tăng (phải dương).
     */
    public void increaseStock(int amount) {
        if (amount > 0) {
            this.stockQuantity += amount;
            System.out.println("Increased stock for " + name + " by " + amount + ". New quantity: " + this.stockQuantity);
        } else {
            System.err.println("Error: Amount to increase must be positive.");
        }
    }

     /**
     * Giảm số lượng tồn kho (ví dụ: khi bán hàng).
     * @param amount Số lượng cần giảm (phải dương).
     * @return true nếu giảm thành công, false nếu số lượng không đủ hoặc amount không hợp lệ.
     */
    public boolean decreaseStock(int amount) {
        if (amount <= 0) {
             System.err.println("Error: Amount to decrease must be positive.");
             return false;
        }
        if (amount > this.stockQuantity) {
             System.err.println("Error: Not enough stock for " + name + ". Required: " + amount + ", Available: " + this.stockQuantity);
             return false;
        }
        this.stockQuantity -= amount;
        System.out.println("Decreased stock for " + name + " by " + amount + ". New quantity: " + this.stockQuantity);
        return true;
    }

    /**
     * Hiển thị thông tin chi tiết sản phẩm.
     */
    public void displayProductInfo() {
        System.out.println("--- Product Information ---");
        System.out.println("ID: " + productId);
        System.out.println("Name: " + name);
        System.out.println("Description: " + description);
        System.out.println("Price: $" + String.format("%.2f", price)); // Định dạng giá tiền
        System.out.println("Stock: " + stockQuantity + (isInStock() ? " (In Stock)" : " (Out of Stock)"));
        System.out.println("Date Added: " + dateAdded);
        System.out.println("-------------------------");
    }

    // --- Ghi đè các phương thức của lớp Object (Thường cần thiết) ---

    @Override
    public String toString() {
        // Cung cấp biểu diễn dạng chuỗi của đối tượng, hữu ích cho việc debug và logging
        return "Product{" +
               "productId='" + productId + '\'' +
               ", name='" + name + '\'' +
               ", price=" + price +
               ", stockQuantity=" + stockQuantity +
               '}';
    }

    @Override
    public boolean equals(Object o) {
        // So sánh hai đối tượng Product dựa trên productId
        if (this == o) return true; // Nếu cùng trỏ đến 1 đối tượng
        if (o == null || getClass() != o.getClass()) return false; // Nếu khác kiểu hoặc null
        Product product = (Product) o;
        return Objects.equals(productId, product.productId); // So sánh dựa trên ID
    }

    @Override
    public int hashCode() {
        // Cần ghi đè hashCode khi ghi đè equals, thường dựa trên các trường dùng trong equals
        return Objects.hash(productId);
    }
}
```

**Trường hợp sử dụng:**

Lớp `Product` này có thể được sử dụng trong nhiều hệ thống:

* **Ứng dụng quản lý kho:** Tạo các đối tượng `Product` để lưu trữ thông tin hàng hóa, gọi `increaseStock` khi nhập hàng, `decreaseStock` khi xuất hàng, `displayProductInfo` để xem chi tiết.
* **Website thương mại điện tử:** Hiển thị thông tin sản phẩm (`getName`, `getPrice`, `getDescription`), kiểm tra tình trạng còn hàng (`isInStock`) trước khi cho phép thêm vào giỏ hàng.
* **Hệ thống POS (Point of Sale):** Quét mã vạch để lấy `productId`, tạo hoặc tìm đối tượng `Product` tương ứng, gọi `decreaseStock` khi thanh toán.

**Khi nào nên tạo Lớp và Đối tượng?**

* Khi bạn cần mô hình hóa một khái niệm, thực thể hoặc cấu trúc dữ liệu có cả trạng thái (thuộc tính) và hành vi (phương thức) liên quan.
* Khi bạn muốn đóng gói dữ liệu và hành vi lại với nhau để quản lý độ phức tạp.
* Khi bạn thấy có nhiều thực thể cùng loại (ví dụ: nhiều sản phẩm, nhiều người dùng) và muốn định nghĩa một khuôn mẫu chung cho chúng.
* Khi bạn muốn tận dụng các lợi ích của OOP như tái sử dụng, đóng gói, kế thừa, đa hình.

## 10. Mở rộng và So sánh

### 10.1. So sánh với Lập trình Thủ tục

Lập trình thủ tục (Procedural Programming) tập trung vào việc chia chương trình thành các hàm (procedures) hoặc routines. Dữ liệu thường được lưu trữ tách biệt và các hàm sẽ thao tác trên dữ liệu đó.

| Tiêu chí           | Lập trình Hướng đối tượng (OOP)                       | Lập trình Thủ tục (Procedural)                    |
| :----------------- | :---------------------------------------------------- | :------------------------------------------------ |
| **Trọng tâm**      | Đối tượng (dữ liệu + hành vi)                         | Hàm/Thủ tục (logic thực thi)                      |
| **Tổ chức mã**     | Các lớp chứa thuộc tính và phương thức                | Các hàm độc lập hoặc trong module                 |
| **Dữ liệu**        | Thường được đóng gói (private) bên trong lớp          | Thường là biến toàn cục hoặc cục bộ, dễ truy cập  |
| **Bảo mật DL**     | Cao hơn nhờ Encapsulation                             | Thấp hơn, khó kiểm soát truy cập                  |
| **Tái sử dụng**    | Cao (Kế thừa, Composition)                            | Thấp hơn (chủ yếu qua gọi hàm)                    |
| **Bảo trì**        | Dễ hơn khi thay đổi (thay đổi trong lớp ít ảnh hưởng) | Khó hơn (thay đổi hàm có thể ảnh hưởng nhiều nơi) |
| **Mô hình hóa**    | Gần với thế giới thực hơn                             | Trừu tượng hơn, tập trung vào quy trình           |
| **Ví dụ Ngôn ngữ** | Java, C++, C#, Python, Ruby                           | C, Pascal, Fortran                                |

OOP không phải lúc nào cũng tốt hơn, nhưng nó đặc biệt hiệu quả cho các hệ thống lớn, phức tạp, cần khả năng mở rộng và bảo trì cao.

### 10.2. Các Nguyên lý OOP khác (Giới thiệu sơ lược)

Ngoài Đóng gói, OOP còn có 3 trụ cột quan trọng khác:

* **Kế thừa (Inheritance):** Cho phép một lớp (lớp con - subclass/derived class) thừa hưởng các thuộc tính và phương thức từ một lớp khác (lớp cha - superclass/base class). Giúp tái sử dụng mã và tạo ra hệ thống phân cấp lớp. (Ví dụ: Lớp `Dog` và `Cat` kế thừa từ lớp `Animal`).
* **Đa hình (Polymorphism):** "Nhiều hình dạng". Cho phép các đối tượng của các lớp khác nhau (thường có chung lớp cha hoặc interface) phản ứng với cùng một lời gọi phương thức theo cách riêng của chúng. Giúp viết mã linh hoạt và dễ mở rộng hơn. (Ví dụ: Gọi phương thức `makeSound()` trên đối tượng `Dog` và `Cat` sẽ cho ra tiếng kêu khác nhau).
* **Trừu tượng (Abstraction):** Che giấu các chi tiết triển khai phức tạp và chỉ hiển thị các tính năng cần thiết cho người dùng. Thường được thực hiện thông qua các lớp trừu tượng (abstract classes) và giao diện (interfaces). Giúp giảm độ phức tạp và tập trung vào "cái gì" đối tượng làm thay vì "làm như thế nào".

### 10.3. Thành viên Tĩnh (Static Members)

Bên cạnh các thuộc tính và phương thức gắn liền với từng đối tượng cụ thể (instance members), Java còn cho phép định nghĩa các thành viên tĩnh (static members) bằng từ khóa `static`.

* **Thuộc tính tĩnh (`static` field):**
    * Thuộc về **lớp**, không thuộc về bất kỳ đối tượng cụ thể nào.
    * Chỉ có **một bản sao duy nhất** của thuộc tính tĩnh, được chia sẻ bởi tất cả các đối tượng của lớp đó.
    * Thường dùng để lưu trữ các hằng số (kết hợp `static final`) hoặc bộ đếm chung cho tất cả đối tượng.
    * Truy cập thông qua tên lớp: `ClassName.staticFieldName`.
* **Phương thức tĩnh (`static` method):**
    * Thuộc về **lớp**, không thuộc về đối tượng cụ thể.
    * Được gọi trực tiếp thông qua tên lớp: `ClassName.staticMethodName()`.
    * **Không thể** truy cập trực tiếp các thuộc tính instance hoặc gọi các phương thức instance (vì không có đối tượng `this` cụ thể nào được liên kết).
    * Chỉ có thể truy cập các thành viên tĩnh khác của lớp.
    * Thường dùng cho các phương thức tiện ích (utility methods) không phụ thuộc vào trạng thái của đối tượng cụ thể (ví dụ: `Math.sqrt()`, `Integer.parseInt()`).

```java
public class Counter {
    private static int count = 0; // Thuộc tính tĩnh, chia sẻ bởi mọi đối tượng Counter
    private int instanceId;       // Thuộc tính instance, mỗi đối tượng có riêng

    public Counter() {
        count++; // Tăng bộ đếm tĩnh mỗi khi tạo đối tượng mới
        this.instanceId = count;
    }

    // Phương thức tĩnh để lấy giá trị bộ đếm
    public static int getCount() {
        // Không thể truy cập this.instanceId ở đây
        return count;
    }

    // Phương thức instance
    public int getInstanceId() {
        return this.instanceId;
    }
}

// Sử dụng:
System.out.println("Initial count: " + Counter.getCount()); // Gọi phương thức tĩnh qua tên lớp

Counter c1 = new Counter();
Counter c2 = new Counter();

System.out.println("Count after creating c1, c2: " + Counter.getCount()); // Output: 2
// Cũng có thể gọi qua đối tượng (không khuyến khích): System.out.println(c1.getCount());

System.out.println("c1 ID: " + c1.getInstanceId()); // Output: 1
System.out.println("c2 ID: " + c2.getInstanceId()); // Output: 2
```

## 11. Thực hành Tốt nhất (Best Practices) và Lỗi thường gặp

### 11.1. Thực hành Tốt nhất (Best Practices)

* **Luôn sử dụng Encapsulation:** Khai báo thuộc tính là `private` và cung cấp getters/setters `public` (chỉ khi cần thiết). Validate dữ liệu trong setters.
* **Đặt tên rõ ràng:** Tuân thủ quy ước đặt tên (UpperCamelCase cho lớp, lowerCamelCase cho phương thức/thuộc tính) và chọn tên có ý nghĩa.
* **Khởi tạo hợp lý trong Constructor:** Đảm bảo đối tượng ở trạng thái hợp lệ ngay sau khi tạo. Sử dụng constructor chaining (`this(...)`) để tránh lặp code.
* **Nguyên tắc Trách nhiệm Đơn lẻ (Single Responsibility Principle - SRP):** Mỗi lớp nên chỉ chịu trách nhiệm về một khía cạnh hoặc chức năng duy nhất của hệ thống. Tránh tạo các lớp "thần thánh" (God classes) làm quá nhiều việc.
* **Sử dụng `final`:** Đánh dấu `final` cho các thuộc tính không bao giờ thay đổi giá trị sau khi được khởi tạo trong constructor (ví dụ: `productId`, `dateAdded`). Điều này tăng tính bất biến (immutability) và an toàn.
* **Ghi đè `toString()`, `equals()`, `hashCode()` khi cần:** Đặc biệt là `equals()` và `hashCode()` phải luôn được ghi đè cùng nhau nếu bạn muốn so sánh các đối tượng dựa trên nội dung thay vì chỉ dựa trên địa chỉ bộ nhớ. `toString()` rất hữu ích cho debugging.
* **Ưu tiên tính bất biến (Immutability):** Nếu có thể, hãy thiết kế các lớp sao cho trạng thái của đối tượng không thể thay đổi sau khi tạo. Đối tượng bất biến dễ razon và an toàn hơn trong môi trường đa luồng.
* **Giữ phương thức ngắn gọn:** Mỗi phương thức nên thực hiện một nhiệm vụ rõ ràng và không quá dài.

### 11.2. Lỗi thường gặp (Common Pitfalls)

* **`NullPointerException` (NPE):** Cố gắng gọi phương thức hoặc truy cập thuộc tính trên một biến tham chiếu đang có giá trị `null`.
    * *Cách tránh:* Luôn kiểm tra `if (objectName != null)` trước khi sử dụng, hoặc khởi tạo đối tượng ngay khi khai báo nếu có thể, hoặc sử dụng `Optional` (Java 8+).
* **Quên dùng `this`:** Khi tên tham số trùng với tên thuộc tính trong constructor hoặc setter, dẫn đến việc gán giá trị cho chính tham số thay vì thuộc tính (`x = x;`).
    * *Cách tránh:* Luôn dùng `this.fieldName = parameterName;` trong trường hợp này.
* **Truy cập trực tiếp thuộc tính `public`:** Phá vỡ tính đóng gói, làm mã khó bảo trì và dễ gây lỗi khi thay đổi cấu trúc lớp.
    * *Cách tránh:* Luôn dùng `private` và getters/setters.
* **Logic phức tạp trong Getters/Setters:** Getters chỉ nên trả về giá trị, setters chỉ nên gán giá trị (có thể kèm validation đơn giản). Logic nghiệp vụ phức tạp nên đặt trong các phương thức riêng.
* **Hiểu sai về Tham chiếu:** Nghĩ rằng gán `obj2 = obj1` là tạo bản sao của đối tượng. Thực tế là cả hai biến cùng trỏ đến một đối tượng. Thay đổi qua `obj1` sẽ ảnh hưởng đến `obj2`.
    * *Cách tránh:* Hiểu rõ sự khác biệt giữa biến tham chiếu và đối tượng. Nếu cần bản sao độc lập, bạn cần tự triển khai cơ chế sao chép (cloning hoặc copy constructor).
* **Không ghi đè `equals()` và `hashCode()` đúng cách:** Khiến các cấu trúc dữ liệu như `HashMap`, `HashSet` hoạt động sai nếu chúng chứa đối tượng của lớp bạn.
    * *Cách tránh:* Luôn ghi đè cả hai nếu ghi đè một trong hai, và đảm bảo logic của chúng nhất quán (nếu `a.equals(b)` thì `a.hashCode() == b.hashCode()`).

## 12. Kết luận

Lớp và Đối tượng là những khái niệm nền tảng, cốt lõi của Lập trình Hướng đối tượng trong Java. Việc hiểu rõ cách khai báo lớp, định nghĩa thuộc tính và phương thức, sử dụng constructor để khởi tạo đối tượng, áp dụng tính đóng gói thông qua `private` và getters/setters, cùng với việc sử dụng `this` một cách chính xác, là những kỹ năng thiết yếu cho mọi lập trình viên Java.