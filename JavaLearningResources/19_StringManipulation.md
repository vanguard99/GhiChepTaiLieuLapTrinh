# XỬ LÝ CHUỖI

## 1\. Giới thiệu

Trong lập trình Java, việc thao tác và xử lý chuỗi (string) là một tác vụ cực kỳ phổ biến và quan trọng. Từ việc hiển thị thông tin cho người dùng, xử lý dữ liệu đầu vào, giao tiếp qua mạng, đến làm việc với cơ sở dữ liệu, chuỗi luôn đóng vai trò trung tâm. Hướng dẫn này sẽ cung cấp cái nhìn toàn diện và sâu sắc về cách làm việc hiệu quả với chuỗi trong Java, bao gồm lớp `String` cốt lõi, các lớp `StringBuilder` và `StringBuffer` cho việc thay đổi chuỗi, và sức mạnh của Biểu thức Chính quy (Regular Expressions) để xử lý các mẫu phức tạp.

## 2\. Lớp `String` trong Java

Lớp `java.lang.String` là nền tảng cho việc xử lý chuỗi trong Java. Nó đại diện cho một chuỗi các ký tự không thay đổi (immutable).

### 2.1. Khái niệm và Tính Bất biến (Immutability)

  * **Định nghĩa:** Một đối tượng `String` trong Java biểu diễn một dãy các ký tự. Bạn có thể tạo một chuỗi bằng cách sử dụng dấu ngoặc kép:
    ```java
    String loiChao = "Xin chào Java!";
    String ten = "Người dùng";
    ```
  * **Tính Bất biến:** Đây là đặc tính quan trọng nhất của lớp `String`. Một khi đối tượng `String` đã được tạo, nội dung (dãy ký tự) của nó **không thể bị thay đổi**. Bất kỳ thao tác nào có vẻ như "sửa đổi" một chuỗi (ví dụ: nối chuỗi, chuyển đổi chữ hoa/thường) thực chất đều tạo ra một đối tượng `String` *mới* trong bộ nhớ, còn đối tượng gốc vẫn giữ nguyên.
    ```java
    String s1 = "Học";
    s1 = s1.concat(" Java"); // s1 bây giờ tham chiếu đến đối tượng mới "Học Java"
                            // Đối tượng "Học" ban đầu vẫn tồn tại (nếu không có tham chiếu khác)
                            // nhưng không còn được s1 trỏ tới.
    ```
  * **Lợi ích của Tính Bất biến:**
      * **An toàn (Security):** Chuỗi thường được dùng để lưu trữ thông tin nhạy cảm (tên người dùng, mật khẩu, đường dẫn file, URL). Tính bất biến ngăn chặn việc thay đổi giá trị chuỗi một cách vô tình hoặc cố ý sau khi nó đã được tạo, đảm bảo tính toàn vẹn dữ liệu trong các ngữ cảnh quan trọng.
      * **An toàn Đa luồng (Thread Safety):** Vì không thể thay đổi, các đối tượng `String` có thể được chia sẻ an toàn giữa nhiều luồng mà không cần cơ chế đồng bộ hóa phức tạp. Nhiều luồng có thể đọc cùng một đối tượng `String` mà không gây ra xung đột dữ liệu (race conditions).
      * **Caching Hashcode:** Giá trị hashcode của một chuỗi có thể được tính toán một lần và lưu trữ lại (cache). Vì chuỗi là bất biến, hashcode của nó sẽ không bao giờ thay đổi, giúp tăng hiệu năng khi sử dụng chuỗi làm khóa trong các cấu trúc dữ liệu dựa trên bảng băm như `HashMap`, `HashSet`.
      * **Hiệu năng (Performance) thông qua String Pool:** Tính bất biến cho phép JVM tối ưu hóa việc lưu trữ chuỗi.

### 2.2. String Pool (Vùng lưu trữ Chuỗi Hằng)

  * **Khái niệm:** String Pool (hay String Constant Pool/String Intern Pool) là một vùng nhớ đặc biệt trong Heap của Java. JVM sử dụng vùng này để lưu trữ các chuỗi hằng (string literals - các chuỗi được định nghĩa trực tiếp trong mã nguồn bằng dấu ngoặc kép).
  * **Cơ chế hoạt động:** Khi bạn tạo một chuỗi hằng (ví dụ: `String str = "Java";`), JVM sẽ kiểm tra xem chuỗi "Java" đã tồn tại trong String Pool hay chưa.
      * Nếu đã tồn tại, JVM sẽ trả về tham chiếu đến đối tượng `String` đang có trong pool thay vì tạo mới.
      * Nếu chưa tồn tại, JVM sẽ tạo một đối tượng `String` mới với giá trị "Java", lưu nó vào pool, và trả về tham chiếu đến đối tượng vừa tạo.
  * **Lợi ích:** Cơ chế này giúp tiết kiệm bộ nhớ đáng kể vì nhiều biến chuỗi có cùng nội dung sẽ cùng trỏ đến một đối tượng duy nhất trong pool.
    ```java
    String a = "Test"; // Tạo "Test" trong pool, a trỏ tới nó
    String b = "Test"; // "Test" đã có trong pool, b trỏ tới cùng đối tượng với a
    String c = new String("Test"); // Tạo đối tượng "Test" MỚI ngoài pool

    System.out.println(a == b); // true (cùng tham chiếu đến đối tượng trong pool)
    System.out.println(a == c); // false (a trong pool, c ngoài pool)
    System.out.println(a.equals(c)); // true (nội dung giống nhau)
    ```
  * **Phương thức `intern()`:** Bạn có thể chủ động thêm một chuỗi (kể cả chuỗi được tạo bằng `new`) vào pool hoặc lấy tham chiếu từ pool bằng phương thức `intern()`.
    ```java
    String c = new String("Test");
    String d = c.intern(); // Kiểm tra "Test" trong pool, nếu có trả về tham chiếu, nếu không thêm vào và trả về.
    String a = "Test";
    System.out.println(a == d); // true
    ```

### 2.3. Các Phương thức Phổ biến của Lớp `String`

Lớp `String` cung cấp nhiều phương thức hữu ích để thao tác với chuỗi (nhớ rằng các phương thức này đều trả về chuỗi *mới*):

| Phương thức                                                            | Mô tả                                                                        | Ví dụ                                                                       |
| :--------------------------------------------------------------------- | :--------------------------------------------------------------------------- | :-------------------------------------------------------------------------- |
| `int length()`                                                         | Trả về độ dài (số lượng ký tự) của chuỗi.                                    | `"Java".length()` trả về `4`.                                               |
| `char charAt(int index)`                                               | Trả về ký tự tại vị trí `index` (chỉ số bắt đầu từ 0).                       | `"Java".charAt(1)` trả về `'a'`.                                            |
| `String concat(String str)`                                            | Nối chuỗi `str` vào cuối chuỗi hiện tại. (Toán tử `+` thường dùng hơn).      | `"Học ".concat("Java")` trả về `"Học Java"`.                                |
| `String toUpperCase()`                                                 | Chuyển đổi tất cả ký tự thành chữ hoa.                                       | `"java".toUpperCase()` trả về `"JAVA"`.                                     |
| `String toLowerCase()`                                                 | Chuyển đổi tất cả ký tự thành chữ thường.                                    | `"JAVA".toLowerCase()` trả về `"java"`.                                     |
| `String trim()`                                                        | Loại bỏ các ký tự khoảng trắng (whitespace) ở đầu và cuối chuỗi.             | `"\t Hello \n".trim()` trả về `"Hello"`.                                    |
| `String substring(int beginIndex)`                                     | Trả về chuỗi con từ vị trí `beginIndex` đến hết chuỗi.                       | `"Welcome".substring(3)` trả về `"come"`.                                   |
| `String substring(int beginIndex, int endIndex)`                       | Trả về chuỗi con từ `beginIndex` đến `endIndex - 1`.                         | `"Welcome".substring(3, 5)` trả về `"co"`.                                  |
| `int indexOf(char ch)`                                                 | Trả về chỉ số của lần xuất hiện đầu tiên của ký tự `ch`.                     | `"banana".indexOf('a')` trả về `1`.                                         |
| `int indexOf(String str)`                                              | Trả về chỉ số của lần xuất hiện đầu tiên của chuỗi `str`.                    | `"banana".indexOf("na")` trả về `2`.                                        |
| `int lastIndexOf(char ch)`                                             | Trả về chỉ số của lần xuất hiện cuối cùng của ký tự `ch`.                    | `"banana".lastIndexOf('a')` trả về `5`.                                     |
| `int lastIndexOf(String str)`                                          | Trả về chỉ số của lần xuất hiện cuối cùng của chuỗi `str`.                   | `"banana".lastIndexOf("na")` trả về `4`.                                    |
| `boolean isEmpty()`                                                    | Kiểm tra chuỗi có rỗng không (độ dài bằng 0).                                | `"".isEmpty()` trả về `true`.                                               |
| `boolean isBlank()` (Java 11+)                                         | Kiểm tra chuỗi có rỗng hoặc chỉ chứa khoảng trắng không.                     | `" ".isBlank()` trả về `true`. `"".isBlank()` cũng trả về `true`.           |
| `String replace(char oldChar, char newChar)`                           | Thay thế tất cả các lần xuất hiện của `oldChar` bằng `newChar`.              | `"Java".replace('a', 'o')` trả về `"Jovo"`.                                 |
| `String replace(CharSequence target, CharSequence replacement)`        | Thay thế tất cả các lần xuất hiện của chuỗi `target` bằng `replacement`.     | `"Hello World".replace("World", "Java")` trả về `"Hello Java"`.             |
| `String[] split(String regex)`                                         | Tách chuỗi thành mảng các chuỗi con dựa trên biểu thức chính quy `regex`.    | `"one,two,three".split(",")` trả về `["one", "two", "three"]`.              |
| `static String join(CharSequence delimiter, CharSequence... elements)` | Nối các chuỗi `elements` lại với nhau, phân cách bởi `delimiter`.            | `String.join("-", "A", "B", "C")` trả về `"A-B-C"`.                         |
| `static String format(String format, Object... args)`                  | Định dạng chuỗi theo mẫu `format` với các đối số `args` (tương tự `printf`). | `String.format("Số: %d, Chuỗi: %s", 10, "Hi")` trả về `"Số: 10, Chuỗi: Hi"` |

**Ví dụ Mã nguồn:**

```java
public class StringExamples {
    public static void main(String[] args) {
        String greeting = "   Chào mừng đến với Java!   ";
        System.out.println("Độ dài ban đầu: " + greeting.length()); // Output: 29
        System.out.println("Ký tự tại vị trí 4: " + greeting.charAt(4)); // Output: 'à'

        String trimmedGreeting = greeting.trim();
        System.out.println("Sau khi trim(): '" + trimmedGreeting + "'"); // Output: 'Chào mừng đến với Java!'
        System.out.println("Độ dài sau trim: " + trimmedGreeting.length()); // Output: 24

        String upperCaseGreeting = trimmedGreeting.toUpperCase();
        System.out.println("Chữ hoa: " + upperCaseGreeting); // Output: CHÀO MỪNG ĐẾN VỚI JAVA!

        String sub = trimmedGreeting.substring(11, 18); // đến với
        System.out.println("Chuỗi con: '" + sub + "'"); // Output: 'đến với'

        int index = trimmedGreeting.indexOf("Java");
        System.out.println("Vị trí của 'Java': " + index); // Output: 19

        String replaced = trimmedGreeting.replace("Java", "Lập trình");
        System.out.println("Sau khi thay thế: " + replaced); // Output: Chào mừng đến với Lập trình!

        String[] words = trimmedGreeting.split(" ");
        System.out.println("Số từ: " + words.length); // Output: 5
        System.out.println("Từ cuối cùng: " + words[words.length - 1]); // Output: Java!

        String joined = String.join("_", words);
        System.out.println("Các từ nối bằng '_': " + joined); // Output: Chào_mừng_đến_với_Java!
    }
}
```

### 2.4. So sánh Chuỗi

Việc so sánh chuỗi là rất quan trọng và cần thực hiện đúng cách:

  * **So sánh bằng `==`:** Toán tử `==` chỉ so sánh **tham chiếu** của đối tượng. Nó chỉ trả về `true` nếu cả hai biến cùng trỏ đến **cùng một đối tượng** trong bộ nhớ (thường xảy ra khi cả hai đều là chuỗi hằng giống nhau trong String Pool). **KHÔNG** nên dùng `==` để so sánh nội dung chuỗi trong hầu hết các trường hợp.
  * **So sánh bằng `equals(Object obj)`:** Đây là phương thức **chuẩn** để so sánh **nội dung** của hai chuỗi. Nó trả về `true` nếu chuỗi hiện tại có cùng dãy ký tự với chuỗi được truyền vào, không phân biệt chúng có phải là cùng một đối tượng hay không.
  * **So sánh bằng `equalsIgnoreCase(String anotherString)`:** Tương tự `equals()`, nhưng bỏ qua sự khác biệt về chữ hoa/thường khi so sánh.
  * **So sánh thứ tự `compareTo(String anotherString)`:** So sánh hai chuỗi theo thứ tự từ điển (dựa trên giá trị Unicode của ký tự).
      * Trả về 0 nếu hai chuỗi bằng nhau.
      * Trả về giá trị âm nếu chuỗi hiện tại đứng trước `anotherString`.
      * Trả về giá trị dương nếu chuỗi hiện tại đứng sau `anotherString`.
      * (Có phiên bản `compareToIgnoreCase()` để so sánh không phân biệt hoa/thường).
  * **Kiểm tra tiền tố/hậu tố:**
      * `startsWith(String prefix)`: Kiểm tra chuỗi có bắt đầu bằng `prefix` không.
      * `endsWith(String suffix)`: Kiểm tra chuỗi có kết thúc bằng `suffix` không.
  * **Kiểm tra chứa chuỗi con:**
      * `contains(CharSequence s)`: Kiểm tra chuỗi có chứa chuỗi con `s` không.

**Ví dụ So sánh:**

```java
String s1 = "Java";
String s2 = "Java";
String s3 = new String("Java");
String s4 = "java";

System.out.println("s1 == s2: " + (s1 == s2));       // true (cùng đối tượng trong pool)
System.out.println("s1 == s3: " + (s1 == s3));       // false (khác đối tượng)
System.out.println("s1.equals(s3): " + s1.equals(s3)); // true (nội dung giống nhau)
System.out.println("s1.equals(s4): " + s1.equals(s4)); // false (khác chữ hoa/thường)
System.out.println("s1.equalsIgnoreCase(s4): " + s1.equalsIgnoreCase(s4)); // true

System.out.println("s1.compareTo(s2): " + s1.compareTo(s2)); // 0
System.out.println("s1.compareTo(s4): " + s1.compareTo(s4)); // < 0 (J đứng trước j)
System.out.println("s4.compareTo(s1): " + s4.compareTo(s1)); // > 0 (j đứng sau J)

System.out.println("s1.startsWith(\"Ja\"): " + s1.startsWith("Ja")); // true
System.out.println("s1.endsWith(\"va\"): " + s1.endsWith("va")); // true
System.out.println("s1.contains(\"av\"): " + s1.contains("av")); // true
```

## 3\. Lớp `StringBuilder` và `StringBuffer`

Khi bạn cần thực hiện nhiều thao tác sửa đổi chuỗi (nối, chèn, xóa), việc liên tục tạo đối tượng `String` mới sẽ không hiệu quả về mặt bộ nhớ và tốc độ. `StringBuilder` và `StringBuffer` được thiết kế để giải quyết vấn đề này.

### 3.1. Giới thiệu và Mục đích

  * **Khả năng thay đổi (Mutability):** Cả `StringBuilder` và `StringBuffer` đều đại diện cho chuỗi ký tự **có thể thay đổi** (mutable). Các thao tác như `append()`, `insert()`, `delete()` sẽ sửa đổi trực tiếp nội dung của đối tượng hiện tại thay vì tạo đối tượng mới.
  * **Hiệu quả:** Chúng hiệu quả hơn `String` khi cần nhiều lần sửa đổi chuỗi, đặc biệt là trong các vòng lặp.
  * **Sự khác biệt chính:**
      * `StringBuilder` (Java 1.5+): **Không đồng bộ hóa** (non-synchronized). Nó nhanh hơn nhưng **không an toàn** khi sử dụng trong môi trường đa luồng (nhiều luồng cùng sửa đổi một đối tượng `StringBuilder` có thể dẫn đến kết quả không mong muốn).
      * `StringBuffer` (Java 1.0+): **Đồng bộ hóa** (synchronized). Các phương thức của nó được đồng bộ hóa, đảm bảo rằng chỉ một luồng có thể sửa đổi đối tượng tại một thời điểm. Điều này làm cho nó **an toàn** trong môi trường đa luồng nhưng chậm hơn `StringBuilder` do chi phí của việc đồng bộ hóa.

### 3.2. Khởi tạo

Bạn tạo đối tượng `StringBuilder` hoặc `StringBuffer` bằng toán tử `new`:

```java
// Khởi tạo rỗng với dung lượng mặc định (thường là 16 ký tự)
StringBuilder sb1 = new StringBuilder();
StringBuffer sbf1 = new StringBuffer();

// Khởi tạo với dung lượng ban đầu xác định
StringBuilder sb2 = new StringBuilder(50); // Có thể chứa 50 ký tự trước khi cần mở rộng
StringBuffer sbf2 = new StringBuffer(100);

// Khởi tạo từ một chuỗi String có sẵn
StringBuilder sb3 = new StringBuilder("Khởi đầu");
StringBuffer sbf3 = new StringBuffer("Bắt đầu");
```

### 3.3. Các Phương thức Chính

Các phương thức quan trọng của `StringBuilder` và `StringBuffer` gần như giống hệt nhau (chỉ khác về đồng bộ hóa):

| Phương thức                                         | Mô tả                                                                               | Ví dụ (dùng StringBuilder)            |
| :-------------------------------------------------- | :---------------------------------------------------------------------------------- | :------------------------------------ |
| `append(...)` (nhiều kiểu dữ liệu)                  | Nối thêm dữ liệu (chuỗi, ký tự, số, boolean...) vào cuối chuỗi hiện tại.            | `sb.append(" World").append(123);`    |
| `insert(int offset, ...)` (nhiều kiểu dữ liệu)      | Chèn dữ liệu vào vị trí `offset` (chỉ số).                                          | `sb.insert(0, "Hello ");`             |
| `delete(int startIndex, int endIndex)`              | Xóa chuỗi con từ `startIndex` đến `endIndex - 1`.                                   | `sb.delete(5, 11);` // Xóa " World"   |
| `deleteCharAt(int index)`                           | Xóa ký tự tại vị trí `index`.                                                       | `sb.deleteCharAt(0);` // Xóa 'H'      |
| `replace(int startIndex, int endIndex, String str)` | Thay thế chuỗi con từ `startIndex` đến `endIndex - 1` bằng chuỗi `str`.             | `sb.replace(0, 5, "Goodbye");`        |
| `reverse()`                                         | Đảo ngược chuỗi hiện tại.                                                           | `sb.reverse();`                       |
| `int length()`                                      | Trả về độ dài hiện tại của chuỗi.                                                   | `int len = sb.length();`              |
| `int capacity()`                                    | Trả về dung lượng hiện tại của bộ đệm ký tự (số ký tự có thể chứa).                 | `int cap = sb.capacity();`            |
| `void setLength(int newLength)`                     | Đặt lại độ dài của chuỗi (cắt bớt hoặc thêm ký tự null nếu cần).                    | `sb.setLength(10);`                   |
| `char charAt(int index)`                            | Trả về ký tự tại vị trí `index`.                                                    | `char c = sb.charAt(1);`              |
| `void setCharAt(int index, char ch)`                | Thay đổi ký tự tại vị trí `index` thành `ch`.                                       | `sb.setCharAt(0, 'J');`               |
| `String substring(int start)`                       | Trả về một đối tượng `String` mới chứa chuỗi con từ vị trí `start`.                 | `String sub1 = sb.substring(5);`      |
| `String substring(int start, int end)`              | Trả về một đối tượng `String` mới chứa chuỗi con từ `start` đến `end - 1`.          | `String sub2 = sb.substring(0, 5);`   |
| `String toString()`                                 | **Quan trọng:** Chuyển đổi đối tượng `StringBuilder`/`StringBuffer` thành `String`. | `String finalString = sb.toString();` |

**Ví dụ Mã nguồn:**

```java
public class BuilderBufferExample {
    public static void main(String[] args) {
        // Sử dụng StringBuilder cho hiệu năng tốt hơn trong môi trường đơn luồng
        StringBuilder builder = new StringBuilder("Bắt đầu: ");

        // Nối chuỗi và số
        builder.append("Java ").append(11);
        System.out.println("Sau append: " + builder); // Output: Bắt đầu: Java 11

        // Chèn vào vị trí
        builder.insert(9, "ngôn ngữ "); // Chèn vào trước "Java"
        System.out.println("Sau insert: " + builder); // Output: Bắt đầu: ngôn ngữ Java 11

        // Thay thế
        builder.replace(0, 8, "Kết thúc"); // Thay "Bắt đầu:"
        System.out.println("Sau replace: " + builder); // Output: Kết thúc ngôn ngữ Java 11

        // Xóa
        builder.delete(8, 18); // Xóa " ngôn ngữ "
        System.out.println("Sau delete: " + builder); // Output: Kết thúcJava 11

        // Đảo ngược
        builder.reverse();
        System.out.println("Sau reverse: " + builder); // Output: 11 avaJkúhtcếK

        // Chuyển về String để sử dụng
        String finalResult = builder.reverse().toString(); // Đảo ngược lại và lấy String
        System.out.println("Kết quả cuối cùng (String): " + finalResult); // Output: Kết thúcJava 11
    }
}
```

### 3.4. Khi nào nên dùng `String`, `StringBuilder`, `StringBuffer`?

  * **Dùng `String` khi:**
      * Bạn cần biểu diễn chuỗi không thay đổi.
      * Số lượng thao tác sửa đổi chuỗi là ít hoặc không có.
      * Bạn cần tận dụng String Pool để tiết kiệm bộ nhớ cho các chuỗi hằng giống nhau.
      * An toàn đa luồng là yêu cầu và chuỗi không cần thay đổi.
  * **Dùng `StringBuilder` khi:**
      * Bạn cần thực hiện nhiều thao tác sửa đổi chuỗi (nối, chèn, xóa...).
      * Ứng dụng của bạn là **đơn luồng** hoặc bạn có thể đảm bảo rằng chỉ một luồng truy cập vào đối tượng `StringBuilder` tại một thời điểm.
      * Hiệu năng là ưu tiên hàng đầu.
  * **Dùng `StringBuffer` khi:**
      * Bạn cần thực hiện nhiều thao tác sửa đổi chuỗi.
      * Ứng dụng của bạn là **đa luồng** và nhiều luồng có thể cùng truy cập và sửa đổi đối tượng chuỗi đó.
      * An toàn đa luồng là yêu cầu bắt buộc, và bạn chấp nhận sự đánh đổi về hiệu năng so với `StringBuilder`.

## 4\. Biểu thức Chính quy (Regular Expressions - Regex)

Biểu thức chính quy là một công cụ cực kỳ mạnh mẽ để tìm kiếm, so khớp và thao tác với các mẫu văn bản phức tạp.

### 4.1. Khái niệm

  * **Định nghĩa:** Regex là một chuỗi đặc biệt mô tả một **mẫu tìm kiếm** (search pattern). Nó định nghĩa một tập hợp các chuỗi ký tự tuân theo một quy tắc nhất định.
  * **Ứng dụng:**
      * **Kiểm tra định dạng dữ liệu (Validation):** Kiểm tra xem email, số điện thoại, mật khẩu, mã bưu chính, URL,... có đúng định dạng không.
      * **Tìm kiếm (Searching):** Tìm kiếm các chuỗi con khớp với một mẫu trong một đoạn văn bản lớn.
      * **Trích xuất dữ liệu (Extraction):** Lấy ra các phần thông tin cụ thể từ văn bản (ví dụ: lấy tất cả các URL từ một trang HTML, lấy ngày tháng từ log file).
      * **Thay thế (Replacement):** Tìm và thay thế các chuỗi con khớp với mẫu.

### 4.2. Cú pháp và Ký tự đặc biệt (Metacharacters)

Regex sử dụng các ký tự thông thường kết hợp với các ký tự đặc biệt (metacharacters) để tạo thành mẫu. Dưới đây là một số thành phần cơ bản:

  * **Ký tự thông thường:** Khớp với chính nó (ví dụ: `a` khớp với 'a', `1` khớp với '1').
  * **Ký tự đại diện (Wildcard):**
      * `.` (dấu chấm): Khớp với **bất kỳ một ký tự nào** (trừ ký tự xuống dòng, trừ khi bật cờ `DOTALL`).
  * **Lớp ký tự (Character Classes):**
      * `[abc]`: Khớp với một trong các ký tự `a`, `b`, hoặc `c`.
      * `[^abc]`: Khớp với bất kỳ ký tự nào **ngoại trừ** `a`, `b`, `c`.
      * `[a-z]`: Khớp với bất kỳ ký tự chữ thường nào từ `a` đến `z`.
      * `[A-Za-z0-9]`: Khớp với bất kỳ chữ cái (hoa hoặc thường) hoặc chữ số nào.
      * `\d`: Khớp với một ký tự **chữ số** (tương đương `[0-9]`).
      * `\D`: Khớp với một ký tự **không phải chữ số** (tương đương `[^0-9]`).
      * `\w`: Khớp với một ký tự "word" (chữ cái, số, hoặc dấu gạch dưới `_`) (tương đương `[a-zA-Z_0-9]`).
      * `\W`: Khớp với một ký tự **không phải "word"**.
      * `\s`: Khớp với một ký tự **khoảng trắng** (space, tab `\t`, newline `\n`, carriage return `\r`, form feed `\f`, vertical tab `\x0B`).
      * `\S`: Khớp với một ký tự **không phải khoảng trắng**.
  * **Biên (Boundary Matchers):**
      * `^`: Khớp với **bắt đầu** của dòng (hoặc bắt đầu của chuỗi nếu không ở chế độ multi-line).
      * `$`: Khớp với **kết thúc** của dòng (hoặc kết thúc của chuỗi nếu không ở chế độ multi-line).
      * `\b`: Khớp với **ranh giới từ** (vị trí giữa ký tự `\w` và `\W`, hoặc giữa `\w` và đầu/cuối chuỗi). Ví dụ: `\bcat\b` sẽ khớp với "cat" trong "the cat sat" nhưng không khớp trong "catalog".
      * `\B`: Khớp với vị trí **không phải ranh giới từ**.
  * **Bộ định lượng (Quantifiers):** Xác định số lần xuất hiện của mẫu đứng trước nó.
      * `*`: Khớp với **0 hoặc nhiều** lần xuất hiện. Ví dụ: `a*` khớp với "", "a", "aa", "aaa",...
      * `+`: Khớp với **1 hoặc nhiều** lần xuất hiện. Ví dụ: `a+` khớp với "a", "aa", "aaa",... (nhưng không khớp với "").
      * `?`: Khớp với **0 hoặc 1** lần xuất hiện. Ví dụ: `colou?r` khớp với "color" và "colour".
      * `{n}`: Khớp với **đúng `n`** lần xuất hiện. Ví dụ: `\d{3}` khớp với đúng 3 chữ số.
      * `{n,}`: Khớp với **ít nhất `n`** lần xuất hiện. Ví dụ: `\d{2,}` khớp với 2 chữ số trở lên.
      * `{n,m}`: Khớp với **từ `n` đến `m`** lần xuất hiện. Ví dụ: `\w{3,5}` khớp với từ 3 đến 5 ký tự word.
  * **Nhóm và Tham chiếu ngược (Groups and Backreferences):**
      * `( )`: Nhóm một phần của biểu thức lại với nhau. Ví dụ: `(ab)+` khớp với "ab", "abab", "ababab",...
      * Nhóm cũng **bắt giữ (capture)** chuỗi con khớp với phần bên trong dấu ngoặc.
      * `\1`, `\2`, ...: Tham chiếu ngược đến chuỗi con đã được bắt giữ bởi nhóm thứ 1, 2,... Ví dụ: `(\d)\1` khớp với hai chữ số giống nhau liền kề (như "22", "99").
  * **Hoặc (Alternation):**
      * `|`: Hoạt động như toán tử OR. Ví dụ: `cat|dog` khớp với "cat" hoặc "dog".
  * **Thoát ký tự đặc biệt (Escaping):**
      * `\`: Dùng để khớp với chính ký tự đặc biệt đó. Ví dụ: để khớp với dấu chấm `.` thật sự, bạn dùng `\.`; để khớp với dấu `*`, bạn dùng `\*`. Trong chuỗi Java, bạn cần viết là `"\\."` hoặc `"\\*"`.

**Lưu ý về dấu `\` trong Java:** Vì `\` cũng là ký tự escape trong chuỗi Java, nên khi viết một ký tự Regex đặc biệt như `\d` trong chuỗi Java, bạn phải viết là `"\\d"`. Tương tự, `\b` thành `"\\b"`, `\.` thành `"\\."`.

### 4.3. Lớp `Pattern` và `Matcher` trong Java

Java cung cấp gói `java.util.regex` để làm việc với biểu thức chính quy, chủ yếu thông qua hai lớp:

  * **`Pattern`:** Đại diện cho một biểu thức chính quy đã được **biên dịch (compiled)**. Việc biên dịch trước giúp tăng hiệu năng nếu bạn sử dụng cùng một mẫu nhiều lần.
      * Không có constructor public.
      * Sử dụng phương thức tĩnh `Pattern.compile(String regex)` để tạo đối tượng `Pattern`.
      * Có thể thêm các cờ (flags) khi biên dịch, ví dụ: `Pattern.CASE_INSENSITIVE` (không phân biệt hoa/thường), `Pattern.MULTILINE` (cho `^` và `$` khớp đầu/cuối dòng thay vì đầu/cuối chuỗi), `Pattern.DOTALL` (cho `.` khớp cả ký tự xuống dòng).
        ```java
        Pattern emailPattern = Pattern.compile("^[\\w-\\.]+@([\\w-]+\\.)+[\\w-]{2,4}$");
        Pattern namePattern = Pattern.compile("^[A-Z][a-z]+$", Pattern.CASE_INSENSITIVE);
        ```
  * **`Matcher`:** Thực hiện các thao tác so khớp trên một **chuỗi đầu vào (input sequence)** cụ thể bằng cách sử dụng một đối tượng `Pattern`.
      * Không có constructor public.
      * Sử dụng phương thức `pattern.matcher(CharSequence input)` để tạo đối tượng `Matcher`.
      * Các phương thức chính:
          * `boolean matches()`: Kiểm tra xem **toàn bộ** chuỗi đầu vào có khớp với mẫu không.
          * `boolean find()`: Tìm lần xuất hiện **tiếp theo** của mẫu trong chuỗi đầu vào. Thường dùng trong vòng lặp `while(matcher.find())`.
          * `boolean lookingAt()`: Kiểm tra xem phần **đầu** của chuỗi đầu vào có khớp với mẫu không.
          * `String group()` hoặc `group(0)`: Trả về chuỗi con đã khớp trong lần `find()` hoặc `matches()` gần nhất.
          * `String group(int groupIndex)`: Trả về chuỗi con được bắt giữ bởi nhóm thứ `groupIndex` (ví dụ: `group(1)` cho nhóm đầu tiên).
          * `int start()`: Trả về chỉ số bắt đầu của chuỗi con đã khớp.
          * `int end()`: Trả về chỉ số kết thúc (chỉ số của ký tự ngay sau ký tự cuối cùng) của chuỗi con đã khớp.
          * `String replaceAll(String replacement)`: Thay thế **tất cả** các lần xuất hiện khớp với mẫu bằng chuỗi `replacement`.
          * `String replaceFirst(String replacement)`: Thay thế **lần xuất hiện đầu tiên** khớp với mẫu bằng chuỗi `replacement`.

**Cách sử dụng tiện lợi (ít hiệu năng hơn nếu dùng nhiều lần):**

  * `inputString.matches(regex)`: Tương đương `Pattern.matches(regex, inputString)`. Kiểm tra toàn bộ chuỗi.
  * `inputString.split(regex)`: Tách chuỗi.
  * `inputString.replaceAll(regex, replacement)`: Thay thế tất cả.
  * `inputString.replaceFirst(regex, replacement)`: Thay thế lần đầu.

**Ví dụ sử dụng `Pattern` và `Matcher`:**

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class RegexDemo {
    public static void main(String[] args) {
        // 1. Validation (Kiểm tra Email)
        String emailRegex = "^[\\w-\\.]+@([\\w-]+\\.)+[\\w-]{2,4}$";
        Pattern emailPattern = Pattern.compile(emailRegex);

        String email1 = "test.user@example.com";
        String email2 = "invalid-email@";

        Matcher matcher1 = emailPattern.matcher(email1);
        System.out.println("'" + email1 + "' is valid: " + matcher1.matches()); // true

        Matcher matcher2 = emailPattern.matcher(email2);
        System.out.println("'" + email2 + "' is valid: " + matcher2.matches()); // false

        // Cách dùng nhanh (ít hiệu quả hơn nếu lặp lại)
        System.out.println("'" + email1 + "' is valid (quick): " + email1.matches(emailRegex)); // true

        System.out.println("-----");

        // 2. Finding (Tìm tất cả các số trong chuỗi)
        String text = "Đơn hàng #123 giá 45.67 USD, mã SP: 987XYZ.";
        String numberRegex = "\\d+(\\.\\d+)?"; // Tìm số nguyên hoặc số thập phân
        Pattern numberPattern = Pattern.compile(numberRegex);
        Matcher numberMatcher = numberPattern.matcher(text);

        System.out.println("Các số tìm thấy trong: '" + text + "'");
        while (numberMatcher.find()) {
            System.out.println(" - " + numberMatcher.group() + " (tại vị trí " + numberMatcher.start() + ")");
        }
        // Output:
        // - 123 (tại vị trí 10)
        // - 45.67 (tại vị trí 19)
        // - 987 (tại vị trí 36)

        System.out.println("-----");

        // 3. Extraction (Trích xuất thông tin từ URL)
        String url = "[https://www.example.com/path/to/resource?id=100&type=A](https://www.example.com/path/to/resource?id=100&type=A)";
        // Regex để bắt protocol, domain, và các query params
        String urlRegex = "^(https?)://([^/]+)(/.*?)(?:\\?(.*))?$";
        Pattern urlPattern = Pattern.compile(urlRegex);
        Matcher urlMatcher = urlPattern.matcher(url);

        if (urlMatcher.matches()) {
            System.out.println("Phân tích URL: " + url);
            System.out.println("  Protocol: " + urlMatcher.group(1)); // group(1) là (https?)
            System.out.println("  Domain:   " + urlMatcher.group(2)); // group(2) là ([^/]+)
            System.out.println("  Path:     " + urlMatcher.group(3)); // group(3) là (/.*?)
            if (urlMatcher.group(4) != null) { // group(4) là (.*) sau dấu ?
                System.out.println("  Query String: " + urlMatcher.group(4));
                // Phân tích thêm query string
                String[] params = urlMatcher.group(4).split("&");
                for (String param : params) {
                    String[] kv = param.split("=");
                    if (kv.length == 2) {
                         System.out.println("    - Param: " + kv[0] + " = " + kv[1]);
                    }
                }
            } else {
                System.out.println("  Query String: (không có)");
            }
        } else {
            System.out.println("URL không hợp lệ: " + url);
        }
        // Output:
        // Phân tích URL: [https://www.example.com/path/to/resource?id=100&type=A](https://www.example.com/path/to/resource?id=100&type=A)
        //   Protocol: https
        //   Domain:   [www.example.com](https://www.example.com)
        //   Path:     /path/to/resource
        //   Query String: id=100&type=A
        //     - Param: id = 100
        //     - Param: type = A

        System.out.println("-----");

        // 4. Replacement (Thay thế khoảng trắng thừa)
        String spacedText = "   Nhiều    khoảng   trắng   ";
        String cleanedText = spacedText.replaceAll("\\s+", " ").trim(); // Thay 1 hoặc nhiều khoảng trắng bằng 1 dấu cách, rồi trim
        System.out.println("Văn bản gốc: '" + spacedText + "'");
        System.out.println("Sau khi dọn dẹp: '" + cleanedText + "'"); // Output: 'Nhiều khoảng trắng'
    }
}
```

## 5\. Mã hóa Ký tự (Character Encoding)

Hiểu về mã hóa ký tự là cần thiết khi làm việc với chuỗi, đặc biệt khi đọc/ghi file hoặc giao tiếp qua mạng.

  * **Khái niệm:** Máy tính lưu trữ văn bản dưới dạng các byte. Mã hóa ký tự là quy tắc ánh xạ giữa các ký tự (như 'A', 'á', '猫') và các chuỗi byte tương ứng.
  * **UTF-8 và UTF-16:**
      * **UTF-16:** Java sử dụng UTF-16 **bên trong** để biểu diễn các đối tượng `String` trong bộ nhớ. UTF-16 sử dụng 2 byte (16 bit) cho hầu hết các ký tự phổ biến (trong Basic Multilingual Plane - BMP) và 4 byte cho các ký tự hiếm hơn.
      * **UTF-8:** Là mã hóa **phổ biến nhất** trên web và trong trao đổi dữ liệu. UTF-8 là mã hóa có độ dài thay đổi:
          * Dùng 1 byte cho các ký tự ASCII (tương thích ngược với ASCII).
          * Dùng 2, 3, hoặc 4 byte cho các ký tự khác (bao gồm tiếng Việt và các ngôn ngữ tượng hình).
  * **Tầm quan trọng:** Khi bạn đọc dữ liệu từ một nguồn bên ngoài (file, network) hoặc ghi dữ liệu ra, bạn cần biết dữ liệu đó được mã hóa theo chuẩn nào (thường là UTF-8) và chỉ định đúng mã hóa đó trong Java để tránh lỗi hiển thị (ví dụ: đọc file UTF-8 mà không chỉ định, Java có thể dùng mã hóa mặc định của hệ thống gây lỗi font tiếng Việt).
  * **Chỉ định Mã hóa trong Java:**
      * **Khi đọc/ghi file:** Sử dụng các lớp như `InputStreamReader`, `OutputStreamWriter`, `Files.newBufferedReader`, `Files.newBufferedWriter` và truyền vào `StandardCharsets.UTF_8` (hoặc mã hóa khác).
        ```java
        import java.nio.charset.StandardCharsets;
        import java.nio.file.Files;
        import java.nio.file.Paths;
        import java.io.BufferedReader;
        import java.io.BufferedWriter;
        import java.io.IOException;

        // ... trong phương thức có throws IOException ...
        // Ghi file với UTF-8
        try (BufferedWriter writer = Files.newBufferedWriter(Paths.get("output.txt"), StandardCharsets.UTF_8)) {
            writer.write("Xin chào thế giới!");
        }

        // Đọc file với UTF-8
        try (BufferedReader reader = Files.newBufferedReader(Paths.get("output.txt"), StandardCharsets.UTF_8)) {
            String line = reader.readLine();
            System.out.println("Đọc từ file: " + line);
        }
        ```
      * **Khi chuyển đổi `String` thành `byte[]` và ngược lại:**
          * `byte[] bytes = myString.getBytes(StandardCharsets.UTF_8);` // Chuyển String thành byte array UTF-8
          * `String myString = new String(bytes, StandardCharsets.UTF_8);` // Chuyển byte array UTF-8 thành String

## 6\. Thực hành tốt nhất & Khắc phục sự cố (Best Practices & Troubleshooting)

### 6.1. Hiệu năng (Performance Considerations)

  * **Tránh nối chuỗi bằng `+` trong vòng lặp:** Mỗi lần dùng `+` để nối `String` sẽ tạo ra một đối tượng `String` mới và có thể cả `StringBuilder` trung gian. Trong vòng lặp, điều này cực kỳ lãng phí tài nguyên. **Luôn sử dụng `StringBuilder` (hoặc `StringBuffer` nếu cần thread-safe) để nối chuỗi trong vòng lặp.**
    ```java
    // KHÔNG NÊN: Rất chậm và tốn bộ nhớ
    String resultBad = "";
    for (int i = 0; i < 10000; i++) {
        resultBad += " " + i; // Tạo nhiều đối tượng String và StringBuilder trung gian
    }

    // NÊN LÀM: Nhanh và hiệu quả
    StringBuilder resultGood = new StringBuilder();
    for (int i = 0; i < 10000; i++) {
        resultGood.append(" ").append(i); // Chỉ sửa đổi một đối tượng StringBuilder
    }
    String finalResult = resultGood.toString();
    ```
  * **Biên dịch `Pattern` một lần:** Nếu bạn sử dụng cùng một biểu thức chính quy nhiều lần, hãy biên dịch nó thành đối tượng `Pattern` một lần và tái sử dụng đối tượng `Matcher` (có thể dùng `matcher.reset(newInput)`) thay vì gọi `String.matches()` hoặc `Pattern.compile()` liên tục.
  * **Cẩn thận với Regex phức tạp:** Các biểu thức chính quy quá phức tạp, đặc biệt là những biểu thức có nhiều bộ định lượng lồng nhau hoặc nhiều lựa chọn `|`, có thể dẫn đến hiện tượng "catastrophic backtracking" làm ứng dụng bị treo hoặc chạy rất chậm trên một số chuỗi đầu vào đặc biệt. Hãy giữ Regex đơn giản và cụ thể nhất có thể. Tối ưu hóa các lựa chọn bằng cách nhóm các tiền tố chung: `tra(vel|de|ce)` tốt hơn `(travel|trade|trace)`.
  * **Sử dụng `String.join()`:** Khi cần nối một mảng hoặc danh sách các chuỗi với một dấu phân cách đơn giản, `String.join()` thường rõ ràng và hiệu quả hơn việc tự dùng `StringBuilder` và vòng lặp.

### 6.2. Các lỗi thường gặp (Common Pitfalls)

  * **So sánh chuỗi bằng `==` thay vì `equals()`:** Đây là lỗi kinh điển. Luôn nhớ dùng `equals()` để so sánh nội dung chuỗi.
  * **Quên tính bất biến của `String`:** Gọi một phương thức như `toUpperCase()` mà không gán kết quả trả về cho một biến nào đó, nghĩ rằng chuỗi gốc đã thay đổi.
    ```java
    String str = "lower";
    str.toUpperCase(); // Lỗi: Không gán kết quả! str vẫn là "lower"
    // Sửa:
    // str = str.toUpperCase();
    ```
  * **Xử lý sai Mã hóa Ký tự:** Đọc/ghi file hoặc dữ liệu mạng mà không chỉ định đúng mã hóa (thường là UTF-8), dẫn đến hiển thị sai ký tự (đặc biệt là tiếng Việt hoặc các ngôn ngữ khác ngoài tiếng Anh).
  * **Regex quá tham lam (Greedy Quantifiers):** Các bộ định lượng `*`, `+`, `{n,}` mặc định là "tham lam" (greedy), chúng cố gắng khớp với chuỗi dài nhất có thể. Đôi khi bạn cần chúng khớp với chuỗi ngắn nhất có thể (lazy/reluctant). Thêm dấu `?` sau bộ định lượng để chuyển sang chế độ lazy: `*?`, `+?`, `{n,}?`, `{n,m}?`.
      * Ví dụ: Với chuỗi `"<p>text1</p><p>text2</p>"`, regex `<p>.*</p>` (greedy) sẽ khớp toàn bộ chuỗi. Regex `<p>.*?</p>` (lazy) sẽ khớp `<p>text1</p>` lần đầu, và `<p>text2</p>` lần thứ hai.
  * **Quên ký tự thoát `\` trong Regex và Java:** Viết `\d` thay vì `\\d` trong chuỗi Java, hoặc `.` thay vì `\\.` khi muốn khớp dấu chấm thật sự.

### 6.3. Mẹo và Kỹ thuật (Tips and Techniques)

  * **Sử dụng `String.format()` hoặc `MessageFormat`:** Để tạo các chuỗi có định dạng phức tạp hoặc nội địa hóa (localization), `String.format()` (cho các trường hợp đơn giản) hoặc lớp `java.text.MessageFormat` (cho các mẫu phức tạp hơn và nội địa hóa) thường dễ đọc và bảo trì hơn là nối chuỗi thủ công bằng `+`.
  * **Kiểm tra null và rỗng:** Trước khi thao tác với chuỗi nhận từ bên ngoài, hãy kiểm tra xem nó có `null` hoặc rỗng (`isEmpty()`) hoặc chỉ chứa khoảng trắng (`isBlank()`) không để tránh `NullPointerException` hoặc lỗi logic. Thư viện Apache Commons Lang cung cấp các lớp tiện ích như `StringUtils` với các phương thức `isEmpty()`, `isBlank()`, `isNotBlank()` rất hữu ích.
  * **Giữ Regex đơn giản và dễ đọc:** Chia các Regex phức tạp thành các phần nhỏ hơn nếu có thể. Sử dụng cờ `Pattern.COMMENTS` để thêm chú thích vào Regex (ít phổ biến nhưng có thể hữu ích). Đặt tên biến `Pattern` rõ ràng.
  * **Kiểm thử Regex kỹ lưỡng:** Sử dụng các công cụ kiểm thử Regex trực tuyến hoặc viết các unit test để đảm bảo Regex hoạt động đúng với nhiều trường hợp đầu vào khác nhau, bao gồm cả các trường hợp biên và trường hợp gây lỗi tiềm ẩn.