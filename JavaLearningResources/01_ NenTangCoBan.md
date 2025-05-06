# NỀN TẢNG CƠ BẢN

## 1. Giới thiệu về Java

Java là một ngôn ngữ lập trình **hướng đối tượng (Object-Oriented Programming - OOP)**, **đa nền tảng (platform-independent)** và được sử dụng cực kỳ rộng rãi trong nhiều lĩnh vực, từ phát triển ứng dụng web doanh nghiệp, ứng dụng di động (Android), hệ thống nhúng, đến các ứng dụng khoa học dữ liệu và Big Data.

### 1.1. Triết lý "Viết một lần, Chạy mọi nơi" (Write Once, Run Anywhere - WORA)

Đây là một trong những ưu điểm nổi bật nhất của Java. Mã nguồn Java không được biên dịch trực tiếp thành mã máy riêng biệt cho từng hệ điều hành (như C/C++). Thay vào đó, nó được biên dịch thành một định dạng trung gian gọi là **bytecode**.

* **Bytecode:** Là một tập hợp các chỉ thị được thiết kế để thực thi bởi **Máy ảo Java (Java Virtual Machine - JVM)**.
* **JVM:** Là một môi trường thực thi trừu tượng. Mỗi hệ điều hành (Windows, macOS, Linux) sẽ có một cài đặt JVM riêng biệt. JVM này sẽ đọc bytecode và *thông dịch (interpret)* hoặc *biên dịch Just-In-Time (JIT compilation)* nó thành mã máy tương ứng với hệ điều hành đó tại thời điểm chạy.

Nhờ kiến trúc này, một tệp `.class` (chứa bytecode) đã được biên dịch có thể chạy trên bất kỳ thiết bị nào đã cài đặt JVM tương thích mà không cần biên dịch lại mã nguồn.

### 1.2. Hệ sinh thái Java: JDK, JRE và JVM

Khi làm việc với Java, bạn sẽ thường gặp các thuật ngữ này:

* **JVM (Java Virtual Machine):** Như đã đề cập, đây là trái tim của việc thực thi mã Java. Nó chịu trách nhiệm tải bytecode, xác minh mã, thực thi mã và cung cấp quản lý bộ nhớ (bao gồm **Garbage Collection - GC**, cơ chế tự động thu hồi bộ nhớ không còn sử dụng). JVM là một *đặc tả (specification)*, và có nhiều *hiện thực (implementation)* khác nhau (ví dụ: HotSpot JVM của Oracle, OpenJ9 của Eclipse).
* **JRE (Java Runtime Environment):** Là môi trường cần thiết để *chạy* các ứng dụng Java đã được biên dịch. Nó bao gồm JVM và các thư viện lõi (Java Class Libraries) mà ứng dụng Java cần để hoạt động (ví dụ: `java.lang`, `java.util`, ...). Nếu bạn chỉ muốn chạy một ứng dụng Java, bạn chỉ cần cài đặt JRE.
* **JDK (Java Development Kit):** Là bộ công cụ đầy đủ dành cho *nhà phát triển* Java. Nó bao gồm mọi thứ có trong JRE, cộng thêm các công cụ phát triển như:
    * `javac`: Trình biên dịch Java (compiler), chuyển đổi mã `.java` thành `.class` (bytecode).
    * `java`: Trình khởi chạy ứng dụng Java (launcher), khởi động JVM để chạy bytecode.
    * `jar`: Công cụ đóng gói thư viện và ứng dụng Java thành tệp `.jar`.
    * `javadoc`: Công cụ tạo tài liệu API từ mã nguồn.
    * Debugger: Công cụ gỡ lỗi.
    * Và nhiều công cụ khác...

**Mối quan hệ:** JDK chứa JRE, và JRE chứa JVM. Lập trình viên cần cài đặt JDK. Người dùng cuối chỉ cần JRE (hoặc đôi khi JRE được đóng gói sẵn cùng ứng dụng).

```
+-----------------------------------------------+
| JDK (Java Development Kit)                    |
|  +-----------------------------------------+  |
|  | JRE (Java Runtime Environment)          |  |
|  |  +-------------------+  +-------------+ |  |
|  |  | JVM               |  | Thư viện lõi| |  |
|  |  | (Virtual Machine) |  | (Core Libs) | |  |
|  |  +-------------------+  +-------------+ |  |
|  +-----------------------------------------+  |
|  +-----------------------------------------+  |
|  | Công cụ phát triển (javac, javadoc,...) |  |
|  +-----------------------------------------+  |
+-----------------------------------------------+

```

### 1.3. So sánh với các ngôn ngữ khác (Ngắn gọn)

* **So với C/C++:** Java dễ quản lý bộ nhớ hơn nhờ cơ chế GC tự động, và có tính đa nền tảng mạnh mẽ hơn (C/C++ cần biên dịch lại cho từng nền tảng). Tuy nhiên, C/C++ thường cho hiệu năng cao hơn do chạy trực tiếp trên phần cứng.
* **So với Python/JavaScript:** Java là ngôn ngữ biên dịch (sang bytecode) và có kiểu dữ liệu tĩnh (static typing), giúp phát hiện lỗi sớm hơn trong quá trình phát triển và thường có hiệu năng tốt hơn cho các tác vụ nặng. Python và JavaScript là ngôn ngữ thông dịch (interpreted) và có kiểu dữ liệu động (dynamic typing), thường giúp phát triển nhanh hơn cho các dự án nhỏ hoặc scripting.

## 2. Thiết lập Môi trường Phát triển

Để bắt đầu lập trình Java, bạn cần:

1.  **Cài đặt JDK:** Tải bản cài đặt JDK phù hợp với hệ điều hành của bạn từ các nguồn uy tín như Oracle JDK hoặc các bản phân phối OpenJDK (ví dụ: AdoptOpenJDK/Temurin, Amazon Corretto, Azul Zulu).
2.  **Chọn và Cài đặt IDE (Integrated Development Environment):** IDE cung cấp các công cụ hỗ trợ lập trình như trình soạn thảo mã thông minh, trình biên dịch, trình gỡ lỗi, quản lý dự án,... Các IDE phổ biến cho Java bao gồm:
    * **IntelliJ IDEA (Community hoặc Ultimate):** Rất mạnh mẽ, thông minh và phổ biến. Phiên bản Community là miễn phí và đủ dùng cho hầu hết các nhu cầu cơ bản.
    * **Eclipse IDE for Java Developers:** Một IDE mã nguồn mở, lâu đời và mạnh mẽ khác.
    * **Visual Studio Code (với Java Extension Pack):** Một trình soạn thảo mã nhẹ nhàng nhưng có thể mở rộng mạnh mẽ để phát triển Java.

### 2.1. Tạo ứng dụng "Hello, World!" với IntelliJ IDEA

Hướng dẫn này giả định bạn sử dụng IntelliJ IDEA.

1.  **Tạo Project mới:**
    * Mở IntelliJ IDEA.
    * Chọn "New Project".
    * Đặt tên cho dự án (ví dụ: `HelloJava`).
    * Chọn ngôn ngữ là "Java".
    * Chọn hệ thống xây dựng (Build system) là "IntelliJ" (cho các dự án đơn giản ban đầu).
    * Chọn JDK bạn đã cài đặt.
    * Bỏ chọn "Add sample code" (chúng ta sẽ tự viết).
    * Nhấn "Create".
2.  **Tạo Lớp (Class) Java:**
    * Trong cửa sổ Project (thường ở bên trái), chuột phải vào thư mục `src`.
    * Chọn "New" -> "Java Class".
    * Đặt tên lớp là `HelloWorld` (Theo quy ước, tên lớp viết hoa chữ cái đầu mỗi từ - PascalCase). Nhấn Enter.
3.  **Viết mã:** IntelliJ sẽ tạo một tệp `HelloWorld.java` với nội dung cơ bản. Sửa lại như sau:

    ```java
    // Khai báo lớp HelloWorld
    public class HelloWorld {

        // Đây là phương thức main - điểm khởi đầu của chương trình Java
        // psvm + Tab (trong IntelliJ) để gõ nhanh
        public static void main(String[] args) {
            // In ra dòng chữ "Hello, World!" ra màn hình console
            // sout + Tab (trong IntelliJ) để gõ nhanh
            System.out.println("Hello, World!");
        }

    } // Kết thúc lớp HelloWorld
    ```

    * **`public class HelloWorld`**: Khai báo một lớp (class) công khai tên là `HelloWorld`. Mọi mã Java đều nằm trong các lớp. Tên tệp `.java` phải trùng với tên lớp `public` bên trong nó.
    * **`public static void main(String[] args)`**: Đây là **phương thức chính (main method)**. Chương trình Java bắt đầu thực thi từ đây.
        * `public`: Có thể truy cập từ bất kỳ đâu.
        * `static`: Có thể gọi phương thức này mà không cần tạo đối tượng từ lớp `HelloWorld`.
        * `void`: Phương thức này không trả về giá trị nào.
        * `main`: Tên đặc biệt mà JVM tìm kiếm để bắt đầu chương trình.
        * `String[] args`: Tham số đầu vào là một mảng các chuỗi (String), thường dùng để nhận các đối số dòng lệnh (command-line arguments).
    * **`System.out.println("Hello, World!");`**: Gọi phương thức `println` (print line) từ đối tượng `out` của lớp `System` để in chuỗi "Hello, World!" và xuống dòng mới ra **standard output** (thường là cửa sổ console).
    * **`//`**: Bắt đầu một dòng ghi chú (comment). Ghi chú bị trình biên dịch bỏ qua, dùng để giải thích mã.
    * **`{ }`**: Dùng để định nghĩa một khối mã (block of code).
    * **`;`**: Kết thúc một câu lệnh (statement) trong Java.

4.  **Chạy ứng dụng:**
    * Chuột phải vào trong trình soạn thảo mã của `HelloWorld.java`.
    * Chọn "Run 'HelloWorld.main()'".
    * Hoặc nhấn vào biểu tượng tam giác màu xanh lá cây bên cạnh khai báo phương thức `main`.
    * Kết quả "Hello, World!" sẽ xuất hiện trong cửa sổ "Run" ở phía dưới IDE.

## 3. Biến, Hằng và Kiểu dữ liệu

### 3.1. Biến (Variables)

Biến là một vùng nhớ được đặt tên dùng để lưu trữ dữ liệu. Giá trị của biến có thể thay đổi trong quá trình thực thi chương trình.

**Khai báo biến:**

```java
dataType variableName;
```

* `dataType`: Xác định loại dữ liệu mà biến có thể lưu trữ (ví dụ: `int`, `double`, `boolean`).
* `variableName`: Tên của biến. Tên biến nên tuân theo quy tắc **camelCase** (viết thường chữ cái đầu, viết hoa chữ cái đầu của các từ tiếp theo, ví dụ: `studentName`, `totalScore`). Tên biến không được bắt đầu bằng số, không chứa ký tự đặc biệt (ngoại trừ `_` và `$`), và không được trùng với từ khóa của Java (như `int`, `class`, `public`,...).

**Ví dụ khai báo:**

```java
int age;         // Khai báo biến age kiểu số nguyên
double salary;    // Khai báo biến salary kiểu số thực dấu phẩy động
boolean isActive; // Khai báo biến isActive kiểu logic (đúng/sai)
String firstName; // Khai báo biến firstName kiểu Chuỗi ký tự (String là một lớp, không phải kiểu nguyên thủy)
```

**Gán giá trị (Assignment):** Dùng toán tử `=` để gán giá trị cho biến.

```java
age = 30;
salary = 5000.75;
isActive = true;
firstName = "Nguyễn Văn"; // Chuỗi ký tự đặt trong dấu nháy kép ""
```

**Khai báo và khởi tạo cùng lúc:**

```java
int score = 100;
char grade = 'A'; // Ký tự đơn đặt trong dấu nháy đơn ''
```

**Khai báo nhiều biến cùng kiểu:**

```java
int x, y, z;      // Khai báo 3 biến int
double width = 10.5, height = 20.0; // Khai báo và khởi tạo 2 biến double
```

**Phạm vi biến (Variable Scope):** Biến được khai báo bên trong một phương thức (như `main`) được gọi là **biến cục bộ (local variable)**. Chúng chỉ tồn tại và có thể truy cập được bên trong khối mã (`{}`) mà chúng được khai báo. Biến cục bộ **phải được khởi tạo** giá trị trước khi sử dụng lần đầu, nếu không sẽ gây lỗi biên dịch.

### 3.2. Hằng số (Constants)

Hằng số là một biến mà giá trị của nó **không thể thay đổi** sau khi được gán lần đầu tiên. Sử dụng hằng số giúp mã dễ đọc hơn (thay vì dùng các giá trị "magic numbers") và ngăn chặn việc thay đổi giá trị quan trọng một cách vô tình.

**Khai báo hằng:** Sử dụng từ khóa `final`. Theo quy ước, tên hằng số được viết hoa toàn bộ và các từ cách nhau bởi dấu gạch dưới (`_`) - **UPPER_SNAKE_CASE**.

```java
final dataType CONSTANT_NAME = value;
```

**Ví dụ:**

```java
final double PI = 3.14159;
final int MAX_USERS = 100;
final String DEFAULT_GREETING = "Welcome!";

// Lỗi biên dịch nếu cố gắng thay đổi giá trị hằng số:
// PI = 3.14; // Compile-time error!
```

**Khi nào dùng hằng số?** Khi bạn có một giá trị cố định, có ý nghĩa trong chương trình và không bao giờ thay đổi (ví dụ: số Pi, số lượng tối đa, một cấu hình mặc định).

### 3.3. Kiểu dữ liệu Nguyên thủy (Primitive Data Types)

Java có 8 kiểu dữ liệu nguyên thủy được định nghĩa sẵn trong ngôn ngữ:

| Kiểu      | Kích thước | Mô tả                                         | Khoảng giá trị                                        | Giá trị mặc định (cho biến instance/static) |
| :-------- | :--------- | :-------------------------------------------- | :---------------------------------------------------- | :------------------------------------------ |
| `byte`    | 1 byte     | Số nguyên có dấu                              | -128 đến 127                                          | `0`                                         |
| `short`   | 2 bytes    | Số nguyên có dấu                              | -32,768 đến 32,767                                    | `0`                                         |
| `int`     | 4 bytes    | Số nguyên có dấu (phổ biến nhất)              | -2<sup>31</sup> đến 2<sup>31</sup> - 1                | `0`                                         |
| `long`    | 8 bytes    | Số nguyên có dấu lớn (thêm `L` hoặc `l` cuối) | -2<sup>63</sup> đến 2<sup>63</sup> - 1                | `0L`                                        |
| `float`   | 4 bytes    | Số thực dấu phẩy động, độ chính xác đơn       | Khoảng ±3.4e−38 đến ±3.4e+38 (thêm `F` hoặc `f` cuối) | `0.0f`                                      |
| `double`  | 8 bytes    | Số thực dấu phẩy động, độ chính xác kép       | Khoảng ±1.7e−308 đến ±1.7e+308                        | `0.0d`                                      |
| `boolean` | ~1 bit\*   | Logic (đúng/sai)                              | `true` hoặc `false`                                   | `false`                                     |
| `char`    | 2 bytes    | Ký tự Unicode đơn (dùng nháy đơn `''`)        | `\u0000` (0) đến `\uffff` (65,535)                    | `\u0000` (ký tự null)                       |

*Ghi chú về `boolean`: Kích thước thực tế của `boolean` trong bộ nhớ không được JVM quy định chặt chẽ, nhưng thường là 1 byte cho biến riêng lẻ, và có thể được tối ưu thành 1 bit trong mảng.*

**Ví dụ:**

```java
byte age = 25;
int population = 1000000;
long worldPopulation = 8000000000L; // Thêm L để chỉ đây là số long
float price = 19.99F;             // Thêm F để chỉ đây là số float
double preciseValue = 123.456789012;
boolean isAvailable = true;
char initial = 'T';
```

**Giá trị mặc định:** Chỉ áp dụng cho **biến instance** (khai báo trực tiếp trong lớp, ngoài phương thức) và **biến static**. Biến cục bộ (local variables) **không** có giá trị mặc định và phải được khởi tạo rõ ràng trước khi dùng.

### 3.4. Kiểu dữ liệu Tham chiếu (Reference Data Types)

Ngoài 8 kiểu nguyên thủy, tất cả các kiểu khác trong Java đều là kiểu tham chiếu. Biến kiểu tham chiếu không lưu trữ trực tiếp giá trị dữ liệu mà lưu trữ **địa chỉ bộ nhớ** nơi đối tượng thực sự được lưu trữ (thường là trong vùng nhớ **Heap**).

* **Các lớp (Classes):** `String`, `Integer`, `Double`, các lớp do bạn tự định nghĩa (`HelloWorld`, `Student`, `Car`,...).
* **Mảng (Arrays):** `int[]`, `String[]`, `double[][]`,...
* **Interfaces:** Các kiểu giao diện.
* **Enums:** Các kiểu liệt kê.

Giá trị mặc định cho mọi biến kiểu tham chiếu (nếu là biến instance hoặc static) là `null`. `null` có nghĩa là biến đó không trỏ đến bất kỳ đối tượng nào.

**Sự khác biệt chính giữa Primitive và Reference:**

* **Lưu trữ:** Primitive lưu giá trị trực tiếp. Reference lưu địa chỉ.
* **Truyền tham số:** Khi truyền biến primitive vào phương thức, một *bản sao* của giá trị được truyền (pass-by-value). Khi truyền biến reference, một *bản sao* của địa chỉ được truyền (vẫn là pass-by-value, nhưng giá trị là địa chỉ, cho phép phương thức thay đổi đối tượng gốc).
* **Giá trị mặc định:** Như đã nêu trên (`0`, `false`, `\u0000` cho primitives; `null` cho references).
* **Toán tử `==`:** Với primitives, `==` so sánh giá trị. Với references, `==` so sánh địa chỉ (nghĩa là kiểm tra xem hai biến có trỏ đến *cùng một đối tượng* trong bộ nhớ hay không). Để so sánh *nội dung* của các đối tượng (ví dụ: hai chuỗi có giống nhau không), bạn phải dùng phương thức `.equals()`.

**Ví dụ về `String` và `==` vs `.equals()` (Lỗi phổ biến):**

```java
String s1 = "Hello";              // String literal - thường được đưa vào String Pool
String s2 = "Hello";              // s2 cũng tham chiếu đến cùng đối tượng trong Pool
String s3 = new String("Hello"); // Tạo một đối tượng String mới trên Heap
String s4 = new String("Hello"); // Tạo một đối tượng String mới khác trên Heap

System.out.println(s1 == s2);       // Output: true (cùng tham chiếu trong Pool)
System.out.println(s1 == s3);       // Output: false (khác đối tượng, khác địa chỉ)
System.out.println(s3 == s4);       // Output: false (khác đối tượng, khác địa chỉ)

System.out.println(s1.equals(s2));  // Output: true (nội dung giống nhau)
System.out.println(s1.equals(s3));  // Output: true (nội dung giống nhau)
System.out.println(s3.equals(s4));  // Output: true (nội dung giống nhau)

// ==> Luôn dùng .equals() để so sánh nội dung chuỗi!
```

### 3.5. Ép kiểu (Type Casting)

Ép kiểu là quá trình chuyển đổi một giá trị từ kiểu dữ liệu này sang kiểu dữ liệu khác.

* **Ép kiểu ngầm định (Implicit Casting / Widening Conversion):** Tự động xảy ra khi chuyển từ kiểu nhỏ hơn sang kiểu lớn hơn (không mất mát thông tin).
    `byte -> short -> int -> long -> float -> double`
    `char -> int`

    ```java
    int myInt = 100;
    long myLong = myInt; // Tự động chuyển int sang long
    float myFloat = myLong; // Tự động chuyển long sang float
    ```

* **Ép kiểu tường minh (Explicit Casting / Narrowing Conversion):** Phải thực hiện thủ công khi chuyển từ kiểu lớn hơn sang kiểu nhỏ hơn. Có thể gây mất mát dữ liệu. Cú pháp: `(targetType) value`

    ```java
    double myDouble = 9.78;
    int myInt = (int) myDouble; // Phải ép kiểu tường minh
    System.out.println(myDouble); // Output: 9.78
    System.out.println(myInt);    // Output: 9 (phần thập phân bị cắt bỏ)

    long bigNum = 10000000000L;
    int smallNum = (int) bigNum; // Ép kiểu tường minh, có thể gây tràn số nếu giá trị long quá lớn
    System.out.println(smallNum); // Kết quả có thể không như mong đợi nếu tràn số
    ```

    **Cẩn thận:** Ép kiểu tường minh có thể gây mất dữ liệu hoặc tràn số (overflow/underflow) nếu giá trị gốc nằm ngoài phạm vi của kiểu đích.

## 4. Toán tử (Operators)

Toán tử là các ký hiệu đặc biệt thực hiện các phép toán trên các giá trị (toán hạng - operands).

### 4.1. Toán tử Số học (Arithmetic Operators)

Dùng cho các phép tính số học cơ bản.

| Toán tử | Mô tả       | Ví dụ (`int a = 10; int b = 3;`) | Kết quả (`result`) | Ghi chú                                       |
| :------ | :---------- | :------------------------------- | :----------------- | :-------------------------------------------- |
| `+`     | Cộng        | `int result = a + b;`            | `13`               | Cũng dùng để nối chuỗi (String concatenation) |
| `-`     | Trừ         | `int result = a - b;`            | `7`                |                                               |
| `*`     | Nhân        | `int result = a * b;`            | `30`               |                                               |
| `/`     | Chia        | `int result = a / b;`            | `3`                | **Chia số nguyên**: Kết quả là phần nguyên    |
| `%`     | Chia lấy dư | `int result = a % b;`            | `1`                |                                               |

**Lưu ý về phép chia số nguyên:** Nếu cả hai toán hạng đều là số nguyên (`byte`, `short`, `int`, `long`), kết quả của phép chia `/` sẽ là phần nguyên, phần thập phân bị loại bỏ. Để có kết quả chính xác với phần thập phân, ít nhất một toán hạng phải là kiểu số thực (`float` hoặc `double`).

```java
int x = 5;
int y = 2;
System.out.println(x / y); // Output: 2 (không phải 2.5)

double d1 = 5.0;
double d2 = 2.0;
System.out.println(d1 / d2); // Output: 2.5

System.out.println(x / d2); // Output: 2.5 (int x được tự động chuyển thành double trước khi chia)
System.out.println((double) x / y); // Output: 2.5 (ép kiểu x thành double trước khi chia)
```

### 4.2. Toán tử Gán (Assignment Operators)

Dùng để gán giá trị cho biến.

| Toán tử | Tương đương với | Ví dụ (`int x = 10;`) | Giá trị mới của `x` |
| :------ | :-------------- | :-------------------- | :------------------ |
| `=`     |                 | `x = 20;`             | `20`                |
| `+=`    | `x = x + value` | `x += 5;`             | `15`                |
| `-=`    | `x = x - value` | `x -= 3;`             | `7`                 |
| `*=`    | `x = x * value` | `x *= 2;`             | `20`                |
| `/=`    | `x = x / value` | `x /= 4;`             | `2` (chia nguyên)   |
| `%=`    | `x = x % value` | `x %= 3;`             | `1`                 |

### 4.3. Toán tử Một ngôi (Unary Operators)

Chỉ tác động lên một toán hạng.

| Toán tử | Mô tả                  | Ví dụ (`int count = 5; boolean flag = true;`) | Kết quả           | Ghi chú                                   |
| :------ | :--------------------- | :-------------------------------------------- | :---------------- | :---------------------------------------- |
| `+`     | Dấu dương (thường ẩn)  | `int result = +count;`                        | `5`               | Ít dùng.                                  |
| `-`     | Dấu âm (đảo dấu)       | `int result = -count;`                        | `-5`              |                                           |
| `++`    | Tăng lên 1 (increment) | `count++;` hoặc `++count;`                    | `count` thành `6` | Xem giải thích về prefix/postfix bên dưới |
| `--`    | Giảm đi 1 (decrement)  | `count--;` hoặc `--count;`                    | `count` thành `4` | Xem giải thích về prefix/postfix bên dưới |
| `!`     | Phủ định logic (NOT)   | `boolean result = !flag;`                     | `false`           | Chỉ dùng với `boolean`                    |

**Prefix (`++var`, `--var`) vs Postfix (`var++`, `var--`):**

* **Prefix (tiền tố):** Tăng/giảm giá trị của biến *trước*, sau đó trả về giá trị *mới* cho biểu thức.
* **Postfix (hậu tố):** Trả về giá trị *hiện tại* của biến cho biểu thức *trước*, sau đó mới tăng/giảm giá trị của biến.

```java
int a = 5;
int b = ++a; // Prefix: a tăng lên 6, sau đó b được gán giá trị mới của a
// Kết quả: a = 6, b = 6

int c = 5;
int d = c++; // Postfix: d được gán giá trị hiện tại của c (là 5), sau đó c tăng lên 6
// Kết quả: c = 6, d = 5
```

### 4.4. Toán tử So sánh (Comparison/Relational Operators)

So sánh hai giá trị và trả về kết quả là `boolean` (`true` hoặc `false`).

| Toán tử | Mô tả                  | Ví dụ (`int x = 10; int y = 5;`) | Kết quả (`result`) |
| :------ | :--------------------- | :------------------------------- | :----------------- |
| `==`    | Bằng nhau              | `boolean result = (x == 10);`    | `true`             |
| `!=`    | Không bằng nhau (khác) | `boolean result = (x != y);`     | `true`             |
| `>`     | Lớn hơn                | `boolean result = (x > y);`      | `true`             |
| `<`     | Nhỏ hơn                | `boolean result = (x < y);`      | `false`            |
| `>=`    | Lớn hơn hoặc bằng      | `boolean result = (x >= 10);`    | `true`             |
| `<=`    | Nhỏ hơn hoặc bằng      | `boolean result = (y <= 5);`     | `true`             |

**Lưu ý:** Không nhầm lẫn toán tử gán `=` với toán tử so sánh bằng `==`. Đây là lỗi rất phổ biến!

### 4.5. Toán tử Logic (Logical Operators)

Kết hợp các biểu thức `boolean`.

| Toán tử | Mô tả                           | Ví dụ (`boolean a = true; boolean b = false;`) | Kết quả (`result`) | Ghi chú                                                                                           |
| :------ | :------------------------------ | :--------------------------------------------- | :----------------- | :------------------------------------------------------------------------------------------------ |
| `&&`    | Logic AND (Và) - Short-circuit  | `boolean result = (a && b);`                   | `false`            | Trả về `true` chỉ khi **cả hai vế** đều `true`. Nếu vế trái là `false`, vế phải sẽ **bị bỏ qua**. |
| `∥`     | Logic OR (Hoặc) - Short-circuit | `boolean result = (a∥b);`                      | `true`             | Trả về `true` nếu **ít nhất một vế** là `true`. Nếu vế trái là `true`, vế phải sẽ **bị bỏ qua**.  |
| `!`     | Logic NOT (Phủ định)            | `boolean result = !a;`                         | `false`            | Đảo ngược giá trị boolean. Nếu `a = true` thì `!a = false`, ngược lại.                            |



*Short-circuit:* Toán tử `&&` và `||` được gọi là "short-circuit" vì chúng có thể không cần đánh giá toán hạng thứ hai nếu kết quả đã được xác định bởi toán hạng thứ nhất. Điều này hữu ích để tránh lỗi (ví dụ: kiểm tra `object != null && object.someMethod()`) và tối ưu hiệu năng. Java cũng có các toán tử `&` và `|` (không short-circuit) nhưng ít được dùng cho logic hơn.

### 4.6. Toán tử Điều kiện (Ternary Operator)

Là cách viết tắt cho một cấu trúc `if-else` đơn giản.
Cú pháp: `condition ? value_if_true : value_if_false`

```java
int score = 75;
String result = (score >= 50) ? "Đạt" : "Trượt";
// Tương đương với:
// String result;
// if (score >= 50) {
//     result = "Đạt";
// } else {
//     result = "Trượt";
// }
System.out.println(result); // Output: Đạt
```

### 4.7. Độ ưu tiên Toán tử (Operator Precedence)

Khi một biểu thức có nhiều toán tử, Java sẽ thực hiện chúng theo một thứ tự ưu tiên nhất định (ví dụ: nhân/chia trước cộng/trừ). Bạn có thể dùng dấu ngoặc đơn `()` để thay đổi thứ tự ưu tiên hoặc làm rõ biểu thức.

Tham khảo bảng độ ưu tiên đầy đủ trong tài liệu Java chính thức khi cần thiết. Một số quy tắc cơ bản:
1.  `()` có độ ưu tiên cao nhất.
2.  Toán tử một ngôi (`++`, `--`, `!`, `-` (unary)) cao hơn toán tử số học.
3.  `*`, `/`, `%` cao hơn `+`, `-` (cộng trừ số học).
4.  Toán tử so sánh (`>`, `<`, `>=`, `<=`, `==`, `!=`) cao hơn toán tử logic.
5.  `&&` cao hơn `||`.
6.  Toán tử gán (`=`, `+=`, ...) có độ ưu tiên thấp nhất.

```java
int result = 5 + 3 * 2; // Tương đương 5 + (3 * 2) = 11
int result2 = (5 + 3) * 2; // = 8 * 2 = 16
boolean check = x > 0 && y < 10 || z == 0; // Tương đương (x > 0 && y < 10) || (z == 0)
```

## 5. Cấu trúc Điều khiển Luồng (Control Flow Statements)

Cho phép chương trình đưa ra quyết định hoặc lặp lại các khối mã dựa trên các điều kiện nhất định.

### 5.1. Lệnh `if`

Thực thi một khối mã nếu một điều kiện là đúng (`true`).

**Cú pháp:**

```java
if (condition) {
    // Khối mã được thực thi nếu condition là true
    // statements;
}
// Các lệnh tiếp theo sau khối if
```

* `condition`: Biểu thức trả về giá trị `boolean`.

**Ví dụ:**

```java
int age = 20;
if (age >= 18) {
    System.out.println("Bạn đủ tuổi bầu cử.");
}
System.out.println("Kiểm tra tuổi kết thúc.");
```

Nếu `age` là 20 (>= 18), cả hai dòng `println` sẽ được thực thi. Nếu `age` là 15, chỉ dòng thứ hai được thực thi.

*Lưu ý:* Nếu khối mã bên trong `if` chỉ có một câu lệnh duy nhất, bạn có thể bỏ qua cặp dấu ngoặc nhọn `{}`. Tuy nhiên, **khuyến nghị luôn sử dụng `{}`** để tăng tính rõ ràng và tránh lỗi khi thêm các lệnh sau này.

```java
// Có thể viết:
if (age >= 18) System.out.println("Đủ tuổi.");

// Nhưng nên viết:
if (age >= 18) {
    System.out.println("Đủ tuổi.");
}
```

### 5.2. Lệnh `if-else`

Thực thi một khối mã nếu điều kiện đúng, và một khối mã khác nếu điều kiện sai (`false`).

**Cú pháp:**

```java
if (condition) {
    // Khối mã 1: Thực thi nếu condition là true
    // statements_if_true;
} else {
    // Khối mã 2: Thực thi nếu condition là false
    // statements_if_false;
}
```

**Ví dụ:**

```java
int score = 45;
if (score >= 50) {
    System.out.println("Kết quả: Đạt");
} else {
    System.out.println("Kết quả: Trượt");
}
// Output: Kết quả: Trượt
```

### 5.3. Lệnh `if-else if-else` (Bậc thang)

Kiểm tra nhiều điều kiện một cách tuần tự. Khối mã của điều kiện `true` đầu tiên gặp phải sẽ được thực thi, và các điều kiện còn lại sẽ bị bỏ qua. Khối `else` cuối cùng (tùy chọn) sẽ thực thi nếu không có điều kiện nào ở trên là `true`.

**Cú pháp:**

```java
if (condition1) {
    // Khối mã 1: Thực thi nếu condition1 là true
} else if (condition2) {
    // Khối mã 2: Thực thi nếu condition1 false VÀ condition2 true
} else if (condition3) {
    // Khối mã 3: Thực thi nếu condition1, condition2 false VÀ condition3 true
}
// ... có thể có nhiều else if khác ...
else {
    // Khối mã cuối: Thực thi nếu TẤT CẢ các condition ở trên đều false
}
```

**Ví dụ (Xếp loại học lực):**

```java
int diem = 78;
char xepLoai;

if (diem >= 90) {
    xepLoai = 'A'; // Xuất sắc
} else if (diem >= 80) {
    xepLoai = 'B'; // Giỏi
} else if (diem >= 65) {
    xepLoai = 'C'; // Khá
} else if (diem >= 50) {
    xepLoai = 'D'; // Trung bình
} else {
    xepLoai = 'F'; // Yếu
}

System.out.println("Điểm: " + diem + ", Xếp loại: " + xepLoai);
// Output: Điểm: 78, Xếp loại: C
```

### 5.4. Lệnh `if` lồng nhau (Nested if)

Bạn có thể đặt một cấu trúc `if` (hoặc `if-else`) bên trong một khối `if` hoặc `else` khác.

**Ví dụ:**

```java
int age = 25;
boolean hasTicket = true;

if (age >= 18) {
    System.out.println("Đã đủ tuổi vào cổng.");
    if (hasTicket) {
        System.out.println("Có vé. Mời vào xem phim!");
    } else {
        System.out.println("Chưa có vé. Vui lòng mua vé.");
    }
} else {
    System.out.println("Chưa đủ tuổi vào cổng.");
}
```

**Cẩn thận:** Lồng quá nhiều cấp `if` có thể làm mã trở nên phức tạp và khó đọc. Cân nhắc tái cấu trúc mã hoặc sử dụng các kỹ thuật khác nếu cần.

### 5.5. Lệnh `switch-case`

Là một cách khác để thực hiện lựa chọn dựa trên giá trị của một biểu thức duy nhất. Thường dùng khi bạn cần so sánh một biến với nhiều giá trị cố định (hằng số).

**Cú pháp:**

```java
switch (expression) {
    case value1:
        // Khối mã thực thi nếu expression == value1
        // statements1;
        break; // Quan trọng: Thoát khỏi switch sau khi thực thi case này
    case value2:
        // Khối mã thực thi nếu expression == value2
        // statements2;
        break;
    // ... các case khác ...
    case valueN:
        // Khối mã thực thi nếu expression == valueN
        // statementsN;
        break;
    default: // Tùy chọn
        // Khối mã thực thi nếu expression không khớp với bất kỳ value nào ở trên
        // default_statements;
        // break; // break ở default thường không cần thiết nhưng có thể đặt
}
```

* `expression`: Biểu thức có giá trị cần so sánh. Kiểu dữ liệu của `expression` phải là `byte`, `short`, `char`, `int`, `String` (từ Java 7), hoặc `Enum`.
* `value1`, `value2`, ...: Các giá trị hằng số (literal hoặc `final` variable) có cùng kiểu hoặc tương thích với `expression`. **Không thể là biến thông thường hoặc biểu thức.**
* `break`: Từ khóa **rất quan trọng**. Nếu thiếu `break` ở cuối một `case`, chương trình sẽ tiếp tục thực thi các câu lệnh của `case` tiếp theo ("fall-through"), bất kể giá trị có khớp hay không, cho đến khi gặp `break` hoặc kết thúc `switch`. Đây là một nguồn lỗi phổ biến.
* `default`: Khối mã tùy chọn này sẽ thực thi nếu không có `case` nào khớp.

**Ví dụ (Kiểm tra ngày trong tuần):**

```java
int dayOfWeek = 3; // Giả sử 1 = Chủ Nhật, 2 = Thứ Hai, ..., 7 = Thứ Bảy
String dayName;

switch (dayOfWeek) {
    case 1:
        dayName = "Chủ Nhật";
        break;
    case 2:
        dayName = "Thứ Hai";
        break;
    case 3:
        dayName = "Thứ Ba";
        break;
    case 4:
        dayName = "Thứ Tư";
        break;
    case 5:
        dayName = "Thứ Năm";
        break;
    case 6:
        dayName = "Thứ Sáu";
        break;
    case 7:
        dayName = "Thứ Bảy";
        break;
    default:
        dayName = "Ngày không hợp lệ";
        break; // break ở đây không bắt buộc nhưng là thói quen tốt
}
System.out.println("Hôm nay là: " + dayName);
// Output: Hôm nay là: Thứ Ba
```

**Ví dụ về "Fall-through" (khi thiếu `break`):**

```java
char grade = 'B';

switch (grade) {
    case 'A':
        System.out.println("Xuất sắc!");
        // break; // Cố tình bỏ qua break
    case 'B':
        System.out.println("Giỏi!");
        // break; // Cố tình bỏ qua break
    case 'C':
        System.out.println("Khá!");
        break; // Có break ở đây
    default:
        System.out.println("Cần cố gắng hơn.");
}
/*
Output:
Giỏi!
Khá!
*/
// Vì thiếu break ở case 'A' và 'B', khi grade là 'B', nó thực thi lệnh của case 'B'
// và tiếp tục "rơi" xuống thực thi lệnh của case 'C', sau đó gặp break và thoát.
// Đôi khi fall-through được dùng có chủ đích (ví dụ gộp các case có cùng hành động),
// nhưng thường là do lỗi quên break.
```

**Gộp các `case` có cùng hành động:**

```java
int month = 2;
int numDays;

switch (month) {
    case 1: case 3: case 5: case 7: case 8: case 10: case 12:
        numDays = 31;
        break;
    case 4: case 6: case 9: case 11:
        numDays = 30;
        break;
    case 2:
        // Giả sử năm không nhuận
        numDays = 28;
        break;
    default:
        numDays = 0; // Tháng không hợp lệ
        break;
}
System.out.println("Số ngày trong tháng " + month + ": " + numDays);
// Output: Số ngày trong tháng 2: 28
```

### 5.6. So sánh `if-else if` và `switch-case`

| Đặc điểm           | `if-else if-else`                                                                                      | `switch-case`                                                                  | Khi nào dùng                                                                                                                                                                                             |
| :----------------- | :----------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Loại so sánh**   | Có thể thực hiện mọi loại so sánh logic (`>`, `<`, `==`, `!=`, kết hợp `&&`, `∥`, gọi phương thức,...) | Chủ yếu chỉ so sánh bằng (`==`) giá trị của `expression` với các `case` value. | `switch`: Khi cần rẽ nhánh dựa trên nhiều giá trị cụ thể của một biến duy nhất (int, char, String, Enum).<br>`if-else`: Khi điều kiện phức tạp, liên quan đến khoảng giá trị, hoặc nhiều biến khác nhau. |
| **Kiểu biểu thức** | Biểu thức điều kiện phải trả về `boolean`.                                                             | Biểu thức `switch` phải là `byte`, `short`, `char`, `int`, `String`, `Enum`.   | -                                                                                                                                                                                                        |
| **Giá trị `case`** | -                                                                                                      | Phải là hằng số (literal, `final` variable). Không thể là biến thường.         | -                                                                                                                                                                                                        |
| **"Fall-through"** | Không có (mỗi khối là độc lập).                                                                        | Có thể xảy ra nếu thiếu `break`.                                               | `switch` có thể gọn hơn `if-else` cho các trường hợp so sánh bằng đơn giản. `if-else` linh hoạt hơn.                                                                                                     |
| **Độ đọc hiểu**    | Có thể khó đọc nếu quá nhiều `else if`.                                                                | Dễ đọc hơn cho việc so sánh với nhiều hằng số cụ thể.                          | Cân nhắc số lượng điều kiện và độ phức tạp.                                                                                                                                                              |


Trong nhiều trường hợp, `switch-case` có thể được tối ưu hóa bởi trình biên dịch Java tốt hơn một chuỗi dài `if-else if` (ví dụ: sử dụng bảng nhảy - jump table), nhưng sự khác biệt về hiệu năng thường không đáng kể trừ khi số lượng `case` cực lớn. Ưu tiên chọn cấu trúc nào giúp mã rõ ràng và dễ bảo trì hơn.

## 6. Thực hành và Phương pháp tốt nhất (Best Practices)

### 6.1. Đặt tên (Naming Conventions)

* **Lớp (Classes) và Giao diện (Interfaces):** `PascalCase` (ví dụ: `Student`, `DataProcessor`, `Runnable`).
* **Phương thức (Methods) và Biến (Variables):** `camelCase` (ví dụ: `calculateScore`, `userName`, `totalAmount`).
* **Hằng số (Constants - `final static`):** `UPPER_SNAKE_CASE` (ví dụ: `MAX_CONNECTIONS`, `DEFAULT_TIMEOUT`).
* **Gói (Packages):** `lowercase.separated.by.dots` (ví dụ: `com.mycompany.myapp.util`). Tên gói thường dựa trên tên miền đảo ngược.

Sử dụng tên có ý nghĩa, mô tả rõ mục đích của biến, phương thức, lớp đó. Tránh tên quá ngắn hoặc mơ hồ (`a`, `b`, `x`, `temp`, `data`).

### 6.2. Thụt lề và Định dạng mã (Indentation and Formatting)

* Luôn thụt lề (indent) mã bên trong các khối (`{}`, `if`, `for`, `while`, `switch`,...). Sử dụng 4 dấu cách (spaces) hoặc 1 tab (cấu hình IDE nhất quán).
* Sử dụng dấu cách xung quanh toán tử (`x = y + 5;` thay vì `x=y+5;`).
* Đặt dấu `{` ở cuối dòng khai báo (phổ biến trong Java) hoặc ở dòng mới (ít phổ biến hơn). Giữ nhất quán trong toàn bộ dự án.
    ```java
    // Cách phổ biến (K&R style)
    if (condition) {
        //...
    } else {
        //...
    }

    // Cách khác (Allman style)
    if (condition)
    {
        //...
    }
    else
    {
        //...
    }
    ```
* Sử dụng các công cụ định dạng mã tự động của IDE (ví dụ: `Ctrl+Alt+L` trong IntelliJ) để đảm bảo mã luôn tuân thủ quy tắc.

Định dạng mã nhất quán giúp mã dễ đọc và dễ bảo trì hơn rất nhiều.

### 6.3. Ghi chú (Comments)

* Sử dụng ghi chú để giải thích những phần mã **phức tạp**, **không rõ ràng**, hoặc **lý do đằng sau** một quyết định thiết kế nào đó.
* **Không** ghi chú những điều hiển nhiên mà mã đã tự nói lên.
* Giữ ghi chú ngắn gọn, cập nhật khi mã thay đổi.
* Các loại ghi chú:
    * `// Ghi chú một dòng`
    * `/* Ghi chú nhiều dòng */`
    * `/** Javadoc comment - Dùng để tạo tài liệu API */`

```java
/**
 * Tính toán chỉ số BMI (Body Mass Index).
 * Công thức: weight / (height * height)
 *
 * @param weight Cân nặng tính bằng kilogram (kg). Phải là số dương.
 * @param height Chiều cao tính bằng mét (m). Phải là số dương.
 * @return Chỉ số BMI, hoặc trả về -1.0 nếu đầu vào không hợp lệ.
 * @throws IllegalArgumentException nếu weight hoặc height không dương. // Ví dụ về Javadoc tag
 */
public double calculateBMI(double weight, double height) {
    // Kiểm tra đầu vào cơ bản
    if (weight <= 0 || height <= 0) {
        // Thay vì trả về -1, ném ra exception rõ ràng hơn là best practice
        throw new IllegalArgumentException("Cân nặng và chiều cao phải là số dương.");
        // return -1.0; // Tránh trả về "magic numbers" như -1 khi có thể
    }

    // Tính toán BMI
    double bmi = weight / (height * height);
    return bmi;
}
```

### 6.4. Tránh "Magic Numbers"

Không sử dụng các hằng số số học không rõ ý nghĩa trực tiếp trong mã. Thay vào đó, khai báo chúng dưới dạng hằng số `final` có tên rõ ràng.

```java
// Không nên:
if (userRole == 1) { // Số 1 có nghĩa là gì?
    // ...
}
double finalPrice = price * 1.1; // 1.1 là gì? Thuế VAT?

// Nên:
final int ADMIN_ROLE = 1;
final int EDITOR_ROLE = 2;
final double VAT_RATE = 0.1; // Thuế suất VAT

if (userRole == ADMIN_ROLE) {
    // ...
}
double finalPrice = price * (1 + VAT_RATE);
```

### 6.5. Lỗi phổ biến cần tránh (Common Pitfalls)

* **Nhầm lẫn `=` (gán) và `==` (so sánh):** `if (x = 5)` sẽ gán 5 cho `x` và (trong Java) gây lỗi biên dịch vì biểu thức gán không trả về boolean. Phải là `if (x == 5)`.
* **So sánh chuỗi bằng `==` thay vì `.equals()`:** Như đã giải thích ở phần Kiểu Tham chiếu. Luôn dùng `.equals()` để so sánh nội dung chuỗi.
* **Quên `break` trong `switch-case`:** Dẫn đến hành vi "fall-through" không mong muốn.
* **Chia số nguyên:** `5 / 2` bằng `2`, không phải `2.5`. Ép kiểu sang `double` hoặc `float` nếu cần kết quả thập phân.
* **Lỗi dấu chấm phẩy `;`:** Quên hoặc đặt sai vị trí dấu `;`.
* **Lỗi dấu ngoặc `{}`:** Không đóng mở ngoặc đúng cặp, đặc biệt trong các cấu trúc lồng nhau.
* **Sử dụng biến cục bộ chưa khởi tạo:** Gây lỗi biên dịch.
* **`NullPointerException` (NPE):** Cố gắng truy cập thành viên (phương thức, thuộc tính) của một biến tham chiếu đang có giá trị `null`. Cần kiểm tra `null` trước khi sử dụng đối tượng.

    ```java
    String name = null;
    // System.out.println(name.length()); // Sẽ ném ra NullPointerException!

    // Kiểm tra null trước khi dùng:
    if (name != null) {
        System.out.println(name.length());
    } else {
        System.out.println("Tên là null.");
    }
    ```

* **Lỗi độ chính xác của `float`/`double`:** Không dùng `float` hoặc `double` cho các phép tính tài chính chính xác do lỗi làm tròn nhị phân. Sử dụng lớp `java.math.BigDecimal` thay thế.

### 6.6. Gỡ lỗi (Debugging)

* Sử dụng trình gỡ lỗi (debugger) của IDE (IntelliJ, Eclipse, VS Code).
* Đặt **điểm dừng (breakpoints)** để tạm dừng thực thi chương trình tại một dòng cụ thể.
* **Kiểm tra giá trị biến (Inspect variables):** Xem giá trị của các biến tại điểm dừng.
* **Bước qua (Step over):** Thực thi dòng hiện tại và di chuyển đến dòng tiếp theo.
* **Bước vào (Step into):** Nếu dòng hiện tại là một lời gọi phương thức, di chuyển vào bên trong phương thức đó.
* **Bước ra (Step out):** Thực thi phần còn lại của phương thức hiện tại và quay trở lại nơi gọi nó.
* Sử dụng `System.out.println()` để in giá trị biến hoặc thông điệp kiểm tra ra console (cách gỡ lỗi đơn giản nhưng đôi khi hiệu quả).