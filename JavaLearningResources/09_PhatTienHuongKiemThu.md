# PHÁT TIỂN HƯỚNG KIỂM THỬ

## 1\. Giới thiệu

Trong quy trình phát triển phần mềm truyền thống, giai đoạn kiểm thử (testing) thường được thực hiện sau khi mã nguồn đã được viết xong. Cách tiếp cận này tiềm ẩn nhiều rủi ro: lỗi có thể được phát hiện muộn, dẫn đến chi phí sửa lỗi tăng cao, trễ tiến độ dự án và ảnh hưởng đến chất lượng tổng thể của sản phẩm.

**Test-First** là một triết lý, một sự thay đổi trong tư duy lập trình, nơi chúng ta ưu tiên viết các bài kiểm thử (test cases) *trước khi* viết mã nguồn thực thi cho chức năng đó. **Phát triển Hướng Kiểm thử (Test-Driven Development - TDD)** là một quy trình phát triển phần mềm cụ thể hóa triết lý Test-First thành một chu trình lặp đi lặp lại gồm ba bước cốt lõi: Viết một test thất bại (Red), viết mã nguồn đủ để vượt qua test đó (Green), và sau đó tái cấu trúc mã nguồn (Refactor).

Hướng dẫn này sẽ đi sâu vào các khái niệm, quy trình, lợi ích và cách triển khai TDD trong thực tế, đặc biệt với ngôn ngữ lập trình Java và thư viện JUnit, nhằm giúp lập trình viên xây dựng phần mềm chất lượng cao, dễ bảo trì và linh hoạt hơn.

**Lợi ích chính của TDD:**

  * **Chất lượng mã nguồn cao hơn:** Các test đảm bảo mã nguồn hoạt động đúng như mong đợi và giảm thiểu lỗi.
  * **Thiết kế tốt hơn:** Việc phải viết test trước buộc lập trình viên phải suy nghĩ về cách sử dụng và giao diện (API) của mã nguồn, dẫn đến thiết kế rõ ràng, module hóa và ít phụ thuộc hơn.
  * **Phát hiện lỗi sớm:** Lỗi được tìm thấy ngay trong quá trình phát triển, khi chi phí sửa chữa còn thấp.
  * **Tăng sự tự tin khi thay đổi:** Bộ test hoạt động như một mạng lưới an toàn, cho phép lập trình viên tái cấu trúc hoặc thêm tính năng mới mà không sợ làm hỏng chức năng hiện có.
  * **Tài liệu sống:** Các unit test mô tả chính xác cách các đơn vị mã nguồn hoạt động và cách sử dụng chúng.

## 2\. Khái niệm Cốt lõi

### 2.1. Kiểm thử Đơn vị (Unit Testing)

**Kiểm thử Đơn vị (Unit Testing)** là mức kiểm thử cơ bản nhất trong phát triển phần mềm, tập trung vào việc kiểm tra tính đúng đắn của các **đơn vị (unit)** mã nguồn nhỏ nhất một cách độc lập.

  * **Đơn vị (Unit) là gì?** Thông thường, một "đơn vị" trong lập trình hướng đối tượng là một phương thức (`method`) hoặc đôi khi là một lớp (`class`). Mục tiêu là kiểm tra đơn vị này hoạt động đúng với các đầu vào khác nhau mà không phụ thuộc vào các thành phần khác của hệ thống (như database, network, file system, các lớp khác).
  * **Mục đích:** Xác minh rằng từng phần nhỏ của mã nguồn hoạt động chính xác theo thiết kế trước khi tích hợp chúng lại với nhau.
  * **Người thực hiện:** Unit test thường do chính lập trình viên viết ra trong quá trình phát triển.

**Đặc tính của một Unit Test tốt (Nguyên tắc F.I.R.S.T):**

  * **Fast (Nhanh):** Unit test phải chạy rất nhanh. Một bộ hàng trăm, hàng nghìn unit test nên hoàn thành trong vài giây hoặc vài phút. Điều này khuyến khích lập trình viên chạy chúng thường xuyên.
  * **Independent/Isolated (Độc lập/Cô lập):** Các unit test không nên phụ thuộc lẫn nhau. Thứ tự chạy test không được ảnh hưởng đến kết quả. Mỗi test nên tự thiết lập môi trường cần thiết và dọn dẹp sau khi chạy xong. Chúng cũng cần được cô lập khỏi các yếu tố bên ngoài (mạng, cơ sở dữ liệu).
  * **Repeatable (Lặp lại được):** Test phải cho cùng một kết quả mỗi khi chạy, bất kể môi trường (máy phát triển, máy chủ CI) hay thời gian chạy.
  * **Self-Validating (Tự xác thực):** Test phải tự xác định được kết quả là pass hay fail mà không cần sự can thiệp thủ công (ví dụ: đọc log). Kết quả thường là boolean (đúng/sai).
  * **Timely (Kịp thời):** Trong TDD, unit test được viết *ngay trước* khi viết mã nguồn sản xuất mà nó kiểm thử.

### 2.2. Vòng lặp TDD (Red-Green-Refactor)

TDD hoạt động theo một chu trình ngắn, lặp đi lặp lại gồm ba bước:

1.  **Bước 1: Viết Test Thất bại (Red)**

      * **Mục tiêu:** Định nghĩa rõ ràng yêu cầu cho một phần nhỏ của chức năng sắp viết thông qua một bài kiểm thử tự động.
      * **Hành động:** Viết một unit test cho chức năng *chưa tồn tại* hoặc chưa được triển khai đúng. Đảm bảo rằng test này *thất bại* khi chạy (thường hiển thị màu đỏ trong các công cụ test runner). Test thất bại vì mã nguồn cần thiết chưa có hoặc chưa đúng.
      * **Tại sao?** Bước này buộc bạn phải suy nghĩ về *cái gì* cần làm và *cách sử dụng* nó trước khi đi vào chi tiết *cách thực hiện*.

2.  **Bước 2: Viết Code Đủ để Vượt qua Test (Green)**

      * **Mục tiêu:** Làm cho test vừa viết chuyển sang trạng thái thành công (pass).
      * **Hành động:** Viết lượng mã nguồn *tối thiểu nhất* cần thiết để test ở Bước 1 chạy thành công (thường hiển thị màu xanh). Ở bước này, *không* cần quan tâm đến việc tối ưu hay mã nguồn có "đẹp" hay không, miễn là nó làm cho test pass.
      * **Tại sao?** Giúp tập trung vào việc giải quyết yêu cầu cụ thể của test, tránh viết thừa chức năng và đảm bảo tiến độ nhanh chóng.

3.  **Bước 3: Tái cấu trúc (Refactor)**

      * **Mục tiêu:** Cải thiện chất lượng mã nguồn vừa viết mà không làm thay đổi hành vi bên ngoài của nó.
      * **Hành động:** Xem xét lại mã nguồn (cả mã sản xuất và mã test) và thực hiện các cải tiến như: loại bỏ trùng lặp, làm rõ tên biến/hàm, tách phương thức/lớp cho dễ hiểu hơn, áp dụng các mẫu thiết kế (design patterns) phù hợp, tối ưu hóa (nếu cần). Quan trọng nhất là **sau mỗi thay đổi nhỏ trong quá trình refactor, phải chạy lại toàn bộ bộ test để đảm bảo không có gì bị phá vỡ (tất cả test vẫn phải Green).**
      * **Tại sao?** Đảm bảo mã nguồn không chỉ hoạt động đúng mà còn sạch sẽ, dễ đọc, dễ hiểu, dễ bảo trì và mở rộng trong tương lai.

Chu trình Red-Green-Refactor này được lặp lại cho từng phần nhỏ của chức năng, giúp xây dựng phần mềm một cách có kiểm soát và chất lượng.

### 2.3. Tư duy "Test-First"

Áp dụng TDD không chỉ là làm theo các bước Red-Green-Refactor. Nó đòi hỏi một sự thay đổi trong cách suy nghĩ:

  * **Thiết kế hướng hành vi:** Thay vì nghĩ về cấu trúc dữ liệu trước, bạn nghĩ về *hành vi* mong muốn của đối tượng/phương thức và cách bạn sẽ kiểm tra hành vi đó.
  * **Làm rõ yêu cầu:** Viết test trước buộc bạn phải hiểu rõ yêu cầu và các trường hợp biên (edge cases) trước khi bắt đầu code.
  * **Phản hồi nhanh:** Chu trình ngắn giúp nhận phản hồi gần như tức thì về tính đúng đắn của mã nguồn.

## 3\. Triển khai TDD với JUnit

**JUnit** là một framework kiểm thử đơn vị (unit testing framework) mã nguồn mở và rất phổ biến cho ngôn ngữ lập trình Java. Nó cung cấp các công cụ và cấu trúc để viết và chạy unit test một cách hiệu quả.

### 3.1. Thiết lập Môi trường

Để sử dụng JUnit (phiên bản 5 trở lên, còn gọi là JUnit Jupiter), bạn cần thêm dependency vào công cụ quản lý build của dự án (như Maven hoặc Gradle).

**Ví dụ với Maven (`pom.xml`):**

```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-api</artifactId>
    <version>5.10.2</version> <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>5.10.2</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-params</artifactId>
    <version>5.10.2</version>
    <scope>test</scope>
</dependency>
```

**Ví dụ với Gradle (`build.gradle` hoặc `build.gradle.kts`):**

```groovy
// build.gradle (Groovy DSL)
dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.10.2'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.10.2'
    // testImplementation 'org.junit.jupiter:junit-jupiter-params:5.10.2' // Nếu cần
}

test {
    useJUnitPlatform()
}
```

```kotlin
// build.gradle.kts (Kotlin DSL)
dependencies {
    testImplementation("org.junit.jupiter:junit-jupiter-api:5.10.2")
    testRuntimeOnly("org.junit.jupiter:junit-jupiter-engine:5.10.2")
    // testImplementation("org.junit.jupiter:junit-jupiter-params:5.10.2") // Nếu cần
}

tasks.test {
    useJUnitPlatform()
}
```

*(Lưu ý: Luôn kiểm tra và sử dụng phiên bản JUnit ổn định mới nhất)*

### 3.2. Cấu trúc một Lớp Test JUnit

Một lớp test JUnit thường có cấu trúc như sau:

```java
import static org.junit.jupiter.api.Assertions.*; // Nhập các hàm assertion
import org.junit.jupiter.api.*; // Nhập các annotation

class MyClassTest {

    // Chạy một lần duy nhất trước tất cả các test trong lớp này
    @BeforeAll
    static void setupAll() {
        System.out.println("Thiết lập chung cho tất cả các test...");
        // Ví dụ: khởi tạo kết nối DB dùng chung (ít phổ biến trong unit test)
    }

    // Chạy trước mỗi phương thức test (@Test)
    @BeforeEach
    void setup() {
        System.out.println("Thiết lập cho từng test...");
        // Ví dụ: tạo đối tượng cần test mới cho mỗi test
    }

    // Annotation đánh dấu đây là một phương thức test
    @Test
    // Annotation cung cấp tên hiển thị thân thiện hơn cho test
    @DisplayName("Kiểm tra cộng hai số dương thành công")
    void testAddTwoPositiveNumbers() {
        // Arrange: Chuẩn bị dữ liệu và đối tượng
        Calculator calculator = new Calculator();
        int numberA = 2;
        int numberB = 3;
        int expectedResult = 5;

        // Act: Thực thi phương thức cần test
        int actualResult = calculator.add(numberA, numberB);

        // Assert: Kiểm tra kết quả có đúng như mong đợi không
        assertEquals(expectedResult, actualResult, "Kết quả phép cộng 2 số dương không đúng");
        // assertTrue(actualResult > 0); // Ví dụ assertion khác
    }

    @Test
    @DisplayName("Kiểm tra cộng số dương và số âm")
    void testAddPositiveAndNegative() {
        Calculator calculator = new Calculator();
        assertEquals(-1, calculator.add(2, -3));
    }

    // Annotation để tạm thời vô hiệu hóa một test
    @Test
    @Disabled("Chức năng này chưa hoàn thiện")
    void testFutureFeature() {
        fail("Test này bị bỏ qua và sẽ thất bại nếu chạy");
    }

    @Test
    @DisplayName("Kiểm tra ném ra ngoại lệ khi chia cho 0")
    void testDivideByZeroThrowsException() {
        Calculator calculator = new Calculator();
        // Assert: Kiểm tra rằng một ngoại lệ cụ thể được ném ra
        assertThrows(IllegalArgumentException.class, () -> {
            // Act: Hành động gây ra ngoại lệ
            calculator.divide(10, 0);
        }, "Phép chia cho 0 phải ném ra IllegalArgumentException");
    }


    // Chạy sau mỗi phương thức test (@Test)
    @AfterEach
    void tearDown() {
        System.out.println("Dọn dẹp sau mỗi test...");
        // Ví dụ: giải phóng tài nguyên được tạo trong @BeforeEach
    }

    // Chạy một lần duy nhất sau tất cả các test trong lớp này
    @AfterAll
    static void tearDownAll() {
        System.out.println("Dọn dẹp chung sau tất cả các test...");
        // Ví dụ: đóng kết nối DB đã mở trong @BeforeAll
    }
}
```

**Các Annotations JUnit 5 phổ biến:**

  * `@Test`: Đánh dấu phương thức là một test case.
  * `@DisplayName`: Cung cấp tên mô tả dễ đọc cho lớp hoặc phương thức test.
  * `@BeforeEach`: Chạy trước *mỗi* phương thức `@Test`. Dùng để thiết lập điều kiện ban đầu chung cho nhiều test.
  * `@AfterEach`: Chạy sau *mỗi* phương thức `@Test`. Dùng để dọn dẹp tài nguyên sau mỗi test.
  * `@BeforeAll`: Chạy *một lần duy nhất* trước tất cả các test trong lớp. Phương thức phải là `static`. Dùng cho các thiết lập tốn kém chỉ cần làm một lần.
  * `@AfterAll`: Chạy *một lần duy nhất* sau tất cả các test trong lớp. Phương thức phải là `static`. Dùng để giải phóng tài nguyên tạo bởi `@BeforeAll`.
  * `@Disabled`: Vô hiệu hóa một lớp hoặc phương thức test. Test này sẽ không được chạy.
  * `@Tag`: Gán nhãn (tag) cho các test để có thể lọc và chạy theo nhóm.
  * `@Nested`: Cho phép tạo các lớp test lồng nhau để nhóm các test liên quan.

**Các Assertions JUnit 5 phổ biến (trong `org.junit.jupiter.api.Assertions`):**

  * `assertEquals(expected, actual)`: Kiểm tra hai giá trị bằng nhau.
  * `assertNotEquals(unexpected, actual)`: Kiểm tra hai giá trị không bằng nhau.
  * `assertTrue(condition)`: Kiểm tra một điều kiện là đúng (`true`).
  * `assertFalse(condition)`: Kiểm tra một điều kiện là sai (`false`).
  * `assertNull(object)`: Kiểm tra một đối tượng là `null`.
  * `assertNotNull(object)`: Kiểm tra một đối tượng không phải là `null`.
  * `assertSame(expected, actual)`: Kiểm tra hai biến tham chiếu đến *cùng một đối tượng* trong bộ nhớ.
  * `assertNotSame(unexpected, actual)`: Kiểm tra hai biến tham chiếu đến *các đối tượng khác nhau*.
  * `assertArrayEquals(expectedArray, actualArray)`: Kiểm tra hai mảng bằng nhau (cùng độ dài và các phần tử tương ứng bằng nhau).
  * `assertThrows(expectedExceptionType, executable)`: Kiểm tra rằng việc thực thi `executable` (thường là một lambda expression) ném ra một ngoại lệ thuộc kiểu `expectedExceptionType`.
  * `assertTimeout(duration, executable)`: Kiểm tra rằng việc thực thi `executable` hoàn thành trong khoảng thời gian `duration` cho phép.
  * `fail()`: Làm cho test thất bại ngay lập tức (thường dùng trong các nhánh logic không mong muốn).

### 3.3. Ví dụ Minh họa Chi tiết: TDD cho Lớp `Calculator`

Hãy áp dụng chu trình Red-Green-Refactor để xây dựng phương thức `add(int a, int b)` cho lớp `Calculator`.

**Yêu cầu:** Viết phương thức `add` nhận hai số nguyên và trả về tổng của chúng.

**Bước 1: RED - Viết Test Thất bại**

Tạo lớp test `CalculatorTest.java`:

```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.*;

class CalculatorTest {

    @Test
    @DisplayName("Test cộng 2 số dương")
    void testAddTwoPositiveNumbers() {
        Calculator calculator = new Calculator(); // Lỗi: Calculator chưa tồn tại
        int result = calculator.add(2, 3);      // Lỗi: phương thức add chưa tồn tại
        assertEquals(5, result);
    }
}
```

*Chạy test này.* Nó sẽ thất bại vì lớp `Calculator` và phương thức `add` chưa được tạo. Đây là trạng thái **RED**.

**Bước 2: GREEN - Viết Code Tối thiểu để Pass Test**

Tạo lớp `Calculator.java`:

```java
public class Calculator {
    // Viết code tối thiểu nhất để testAddTwoPositiveNumbers pass
    public int add(int a, int b) {
        // Cách 1: Trả về giá trị cứng để pass test cụ thể này
        // return 5;
        // Cách 2: Implement logic đơn giản nhất (thường là tốt hơn)
        return a + b;
    }
}
```

*Chạy lại test `testAddTwoPositiveNumbers`.* Bây giờ nó sẽ thành công. Đây là trạng thái **GREEN**.

**Bước 3: REFACTOR - Tái cấu trúc (Nếu cần)**

Trong trường hợp này, mã nguồn `return a + b;` đã khá đơn giản và rõ ràng. Mã test cũng ổn. Có thể không cần refactor gì thêm ở bước này.

**Lặp lại Chu trình cho Yêu cầu Mới/Trường hợp Biên**

Bây giờ, hãy thêm một yêu cầu/test case mới: cộng một số dương và một số âm.

**Bước 1 (RED):** Thêm test mới vào `CalculatorTest.java`:

```java
    @Test
    @DisplayName("Test cộng số dương và số âm")
    void testAddPositiveAndNegative() {
        Calculator calculator = new Calculator();
        int result = calculator.add(5, -3);
        assertEquals(2, result); // Mong đợi 5 + (-3) = 2
    }
```

*Chạy tất cả các test.* Test mới này (và cả test cũ) nên pass ngay lập tức vì logic `return a + b;` đã xử lý được trường hợp này. Nếu nó không pass, chúng ta sẽ quay lại Bước 2.

**Bước 1 (RED):** Thêm test cho trường hợp cộng với số 0.

```java
    @Test
    @DisplayName("Test cộng với số không")
    void testAddWithZero() {
        Calculator calculator = new Calculator();
        assertEquals(5, calculator.add(5, 0));
        assertEquals(0, calculator.add(0, 0));
        assertEquals(-3, calculator.add(0, -3));
    }
```

*Chạy lại tất cả các test.* Các test này cũng nên pass.

**Bước 2 (GREEN):** (Bỏ qua nếu test đã pass)

**Bước 3 (REFACTOR):** Mã nguồn vẫn ổn. Có thể xem xét tái cấu trúc mã test nếu có sự lặp lại. Ví dụ, việc khởi tạo `Calculator` lặp lại trong mỗi test. Có thể dùng `@BeforeEach`:

```java
class CalculatorTest {

    private Calculator calculator; // Khai báo biến instance

    @BeforeEach
    void setup() {
        // Khởi tạo đối tượng mới trước mỗi test
        calculator = new Calculator();
        System.out.println("Đã tạo đối tượng Calculator mới.");
    }

    @Test
    @DisplayName("Test cộng 2 số dương")
    void testAddTwoPositiveNumbers() {
        // Không cần tạo calculator ở đây nữa
        int result = calculator.add(2, 3);
        assertEquals(5, result);
    }

    @Test
    @DisplayName("Test cộng số dương và số âm")
    void testAddPositiveAndNegative() {
        assertEquals(2, calculator.add(5, -3));
    }

    @Test
    @DisplayName("Test cộng với số không")
    void testAddWithZero() {
        assertEquals(5, calculator.add(5, 0));
        assertEquals(0, calculator.add(0, 0));
        assertEquals(-3, calculator.add(0, -3));
    }
    // ... các test khác và @AfterEach, @BeforeAll, @AfterAll nếu cần ...
}
```

*Chạy lại toàn bộ test* sau khi refactor để đảm bảo mọi thứ vẫn **GREEN**.

Quá trình này tiếp tục cho các phương thức khác (`subtract`, `multiply`, `divide`...) và các trường hợp biên khác (số lớn, `null`,...).

## 4\. Các Chủ đề Nâng cao & Mở rộng

### 4.1. Test Doubles (Mocking, Stubbing)

Khi một đơn vị mã nguồn (ví dụ: một lớp `OrderService`) phụ thuộc vào các thành phần khác có tác động bên ngoài (ví dụ: `DatabaseRepository`, `EmailService`, `PaymentGateway`), việc kiểm thử đơn vị trở nên khó khăn vì chúng ta muốn *cô lập* `OrderService` khỏi các phụ thuộc đó. Đây là lúc **Test Doubles** phát huy tác dụng.

**Test Double** là một thuật ngữ chung cho các đối tượng "giả" thay thế các đối tượng phụ thuộc thật trong quá trình kiểm thử. Các loại phổ biến bao gồm:

  * **Stub:** Cung cấp các câu trả lời được định sẵn cho các lời gọi phương thức trong quá trình test. Chúng không quan tâm đến việc phương thức có được gọi hay không, chỉ cung cấp dữ liệu giả.
  * **Mock:** Là các đối tượng được lập trình sẵn với các kỳ vọng về cách chúng sẽ được gọi. Mock có thể xác minh rằng các phương thức cụ thể đã được gọi với các tham số đúng và đúng số lần.
  * **Fake:** Các đối tượng có cài đặt hoạt động (working implementation) nhưng thường đơn giản hơn và không phù hợp cho môi trường sản xuất (ví dụ: database trong bộ nhớ thay vì database thật).
  * **Spy:** Giống như Stub nhưng cũng ghi lại thông tin về cách chúng được gọi (bao nhiêu lần, với tham số nào). Thường bao bọc một đối tượng thật.
  * **Dummy:** Các đối tượng được truyền vào nhưng không bao giờ thực sự được sử dụng. Thường chỉ để đáp ứng danh sách tham số.

**Mockito** là một thư viện mocking rất phổ biến cho Java, giúp tạo ra các Test Doubles (chủ yếu là Mock và Stub) một cách dễ dàng.

**Tại sao cần Mocking/Stubbing?**

  * **Cô lập (Isolation):** Đảm bảo unit test chỉ kiểm tra logic của đơn vị đang xét, không bị ảnh hưởng bởi lỗi hoặc hành vi của các phụ thuộc.
  * **Tốc độ (Speed):** Tránh các thao tác chậm như gọi mạng, truy vấn DB thật.
  * **Tính lặp lại (Repeatability):** Loại bỏ sự không chắc chắn từ các dịch vụ bên ngoài (ví dụ: API của bên thứ ba có thể không ổn định).
  * **Kiểm soát (Control):** Cho phép giả lập các kịch bản khó tái tạo trong môi trường thật (ví dụ: lỗi mạng, database trả về lỗi).

**Ví dụ cơ bản với Mockito (Giả định có dependency JUnit và Mockito):**

```java
import static org.mockito.Mockito.*; // Nhập các hàm của Mockito
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.*;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension; // Cần extension cho @Mock, @InjectMocks
import org.junit.jupiter.api.extension.ExtendWith;

// Giả sử có các lớp này
interface UserRepository {
    User findUserById(String userId);
    void saveUser(User user);
}
class User { /* ... */ }

class UserService {
    private UserRepository userRepository;

    // Constructor injection
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User getUser(String userId) {
        // Logic nghiệp vụ...
        return userRepository.findUserById(userId);
    }

    public void updateUser(User user) {
        // Logic nghiệp vụ...
        userRepository.saveUser(user);
    }
}

// Lớp Test sử dụng Mockito
@ExtendWith(MockitoExtension.class) // Kích hoạt Mockito annotations
class UserServiceTest {

    @Mock // Annotation của Mockito để tạo một Mock object
    private UserRepository mockUserRepository;

    @InjectMocks // Annotation để tạo instance của UserService và tự động inject các @Mock vào đó
    private UserService userService;

    @Test
    @DisplayName("Test getUser trả về user khi tìm thấy")
    void testGetUser_Success() {
        // Arrange: Định nghĩa hành vi cho Mock (Stubbing)
        String userId = "user123";
        User expectedUser = new User(/*...*/); // User giả định
        when(mockUserRepository.findUserById(userId)).thenReturn(expectedUser);

        // Act: Gọi phương thức cần test
        User actualUser = userService.getUser(userId);

        // Assert: Kiểm tra kết quả
        assertNotNull(actualUser);
        assertEquals(expectedUser, actualUser);

        // Verify (Optional): Kiểm tra xem phương thức của Mock có được gọi đúng hay không
        verify(mockUserRepository, times(1)).findUserById(userId);
    }

    @Test
    @DisplayName("Test getUser trả về null khi không tìm thấy")
    void testGetUser_NotFound() {
        // Arrange: Stubbing cho trường hợp không tìm thấy
        String userId = "unknownUser";
        when(mockUserRepository.findUserById(userId)).thenReturn(null);

        // Act
        User actualUser = userService.getUser(userId);

        // Assert
        assertNull(actualUser);
        verify(mockUserRepository).findUserById(userId); // Mặc định là times(1)
    }

    @Test
    @DisplayName("Test updateUser gọi saveUser trên repository")
    void testUpdateUser_CallsSave() {
        // Arrange
        User userToUpdate = new User(/*...*/);

        // Act
        userService.updateUser(userToUpdate);

        // Assert/Verify: Kiểm tra xem saveUser có được gọi với đúng đối tượng user không
        verify(mockUserRepository).saveUser(userToUpdate);
        // verify(mockUserRepository, never()).findUserById(anyString()); // Ví dụ kiểm tra không gọi
    }
}
```

### 4.2. BDD (Behavior-Driven Development)

**Phát triển Hướng Hành vi (BDD)** là một sự mở rộng của TDD, tập trung nhiều hơn vào việc mô tả *hành vi* (behavior) của hệ thống bằng ngôn ngữ tự nhiên, dễ hiểu cho cả lập trình viên, kiểm thử viên và các bên liên quan phi kỹ thuật (như BA, PO).

  * **So sánh với TDD:**
      * **TDD:** Tập trung vào việc kiểm tra các *đơn vị* mã nguồn hoạt động đúng kỹ thuật. Ngôn ngữ test thường mang tính kỹ thuật (tên hàm, lớp).
      * **BDD:** Tập trung vào *hành vi* mong muốn của hệ thống từ góc độ người dùng hoặc nghiệp vụ. Sử dụng cấu trúc `Given-When-Then` để mô tả các kịch bản.
  * **Cấu trúc Given-When-Then:**
      * **Given (Bối cảnh):** Mô tả trạng thái ban đầu của hệ thống hoặc điều kiện tiên quyết.
      * **When (Hành động):** Mô tả hành động hoặc sự kiện xảy ra.
      * **Then (Kết quả):** Mô tả kết quả mong đợi hoặc trạng thái của hệ thống sau hành động.
  * **Frameworks:** Các công cụ như Cucumber (Java, Ruby, JS,...), JBehave (Java), SpecFlow (.NET) cho phép viết các đặc tả hành vi (feature files) bằng ngôn ngữ tự nhiên (Gherkin) và liên kết chúng với mã nguồn kiểm thử (step definitions).

BDD thường được áp dụng ở các mức kiểm thử cao hơn unit test (như integration test, acceptance test) nhưng cũng có thể dùng để định hướng unit test theo hướng hành vi.

### 4.3. TDD và Thiết kế Phần mềm (Emergent Design)

Một trong những lợi ích lớn nhất nhưng thường bị bỏ qua của TDD là nó thúc đẩy **thiết kế tốt hơn**.

  * **Thiết kế Hướng Kiểm thử (Testability):** Để viết unit test hiệu quả (đặc biệt là test cô lập), mã nguồn của bạn *phải* được thiết kế để dễ kiểm thử. Điều này thường dẫn đến:
      * **Tính gắn kết cao (High Cohesion):** Các lớp/phương thức tập trung vào một nhiệm vụ cụ thể.
      * **Tính khớp nối thấp (Low Coupling):** Giảm sự phụ thuộc trực tiếp giữa các lớp, thay vào đó sử dụng Dependency Injection, interfaces.
      * **Nguyên tắc Trách nhiệm Đơn lẻ (Single Responsibility Principle - SRP):** Mỗi lớp/phương thức có một lý do duy nhất để thay đổi.
  * **Thiết kế Tịnh tiến (Emergent Design):** Thay vì cố gắng thiết kế toàn bộ hệ thống một cách hoàn hảo ngay từ đầu, TDD khuyến khích thiết kế "nổi lên" dần dần thông qua các chu trình Red-Green-Refactor. Bạn chỉ viết mã đủ để pass test và sau đó refactor để cải thiện cấu trúc. Thiết kế sẽ được hình thành và tinh chỉnh dựa trên các yêu cầu thực tế được thể hiện qua các test cases.
  * **Phản hồi Thiết kế Nhanh:** Nếu bạn thấy khó khăn khi viết test cho một đoạn mã, đó thường là dấu hiệu cho thấy thiết kế của đoạn mã đó có vấn đề (quá phức tạp, phụ thuộc quá nhiều). TDD cung cấp phản hồi sớm về chất lượng thiết kế.

### 4.4. Tích hợp TDD vào Quy trình Phát triển

  * **Tích hợp Liên tục (Continuous Integration - CI):** TDD hoạt động hiệu quả nhất khi kết hợp với CI. Mỗi khi mã nguồn được đẩy lên repository, hệ thống CI sẽ tự động build và chạy toàn bộ bộ test. Nếu có bất kỳ test nào fail, build sẽ bị đánh dấu là thất bại, giúp phát hiện lỗi tích hợp sớm.
  * **Thách thức khi áp dụng:**
      * **Đường cong học tập:** Cần thời gian để làm quen với quy trình và các công cụ (JUnit, Mockito).
      * **Thời gian ban đầu:** Viết test trước có thể cảm thấy tốn thời gian hơn lúc đầu so với việc chỉ viết code. Tuy nhiên, lợi ích về lâu dài (giảm thời gian debug, bảo trì) thường bù đắp lại.
      * **Kiểm thử cái gì?:** Biết được nên viết unit test cho cái gì và không nên viết cho cái gì (ví dụ: không test getter/setter đơn giản, không test code của thư viện ngoài).
      * **Test bảo trì:** Mã test cũng cần được bảo trì song song với mã sản xuất.

## 5\. Thực tiễn Tốt nhất & Khắc phục sự cố (Best Practices & Troubleshooting)

### 5.1. Best Practices

  * **Viết test nhỏ và tập trung:** Mỗi test chỉ nên kiểm tra một khía cạnh logic hoặc một hành vi cụ thể.
  * **Tên test rõ ràng:** Tên phương thức test nên mô tả rõ ràng kịch bản đang được kiểm thử (ví dụ: `givenState_whenAction_thenExpectedResult` hoặc `shouldDoSomething_whenCondition`). Sử dụng `@DisplayName` để mô tả chi tiết hơn.
  * **Giữ test độc lập (FIRST):** Đảm bảo test không phụ thuộc vào nhau hoặc vào môi trường bên ngoài. Sử dụng `@BeforeEach`/`@AfterEach` để thiết lập và dọn dẹp.
  * **Chạy test thường xuyên:** Chạy toàn bộ bộ test sau mỗi thay đổi nhỏ (đặc biệt là sau khi refactor) để nhận phản hồi nhanh. Tích hợp vào CI.
  * **Refactor liên tục:** Đừng bỏ qua bước Refactor. Mã nguồn sạch sẽ cũng quan trọng như mã nguồn chạy đúng. Refactor cả mã sản xuất và mã test.
  * **Test hành vi, không test chi tiết triển khai:** Tập trung vào việc kiểm tra *kết quả* hoặc *trạng thái* mong muốn sau khi một hành vi được thực hiện, thay vì kiểm tra các bước bên trong của phương thức đó (trừ khi cần thiết, ví dụ như kiểm tra tương tác với mock). Điều này làm cho test ít bị vỡ hơn khi bạn refactor mã nguồn.
  * **Sử dụng Test Doubles một cách hợp lý:** Dùng Mock/Stub để cô lập unit test khỏi các phụ thuộc bên ngoài. Tránh mocking các kiểu dữ liệu đơn giản hoặc các lớp trong cùng module mà bạn có thể kiểm soát.
  * **Cấu trúc Arrange-Act-Assert (AAA):** Tổ chức mỗi phương thức test theo ba phần rõ ràng:
      * `Arrange`: Thiết lập dữ liệu đầu vào, đối tượng, mock/stub.
      * `Act`: Gọi phương thức hoặc thực hiện hành động cần kiểm thử.
      * `Assert`: Kiểm tra kết quả hoặc trạng thái sau hành động có đúng như mong đợi không. (Có thể bao gồm cả `Verify` trong Mockito).
  * **Không kiểm thử code của thư viện/framework:** Bạn nên tin tưởng rằng các thư viện (như thư viện chuẩn Java, Spring Framework, JUnit) đã được kiểm thử. Chỉ test *cách bạn sử dụng* chúng hoặc sự tích hợp của code bạn với chúng.

### 5.2. Lỗi Phổ biến (Common Pitfalls)

  * **Viết quá nhiều code trước khi chạy test (Bỏ qua Red/Green):** Cố gắng viết hoàn chỉnh một tính năng lớn rồi mới viết test, làm mất đi lợi ích phản hồi nhanh và định hướng thiết kế của TDD.
  * **Test quá lớn, không phải Unit Test:** Test kiểm tra sự tương tác của nhiều lớp hoặc có phụ thuộc vào database/mạng thật. Đây更像是 Integration Test, chúng thường chậm và không ổn định.
  * **Bỏ qua bước Refactor:** Mã nguồn hoạt động nhưng lộn xộn, khó hiểu, trùng lặp, dẫn đến khó bảo trì về sau.
  * **Test phụ thuộc lẫn nhau:** Test A chạy thành công chỉ khi Test B đã chạy trước đó. Điều này vi phạm nguyên tắc Independent.
  * **Test các chi tiết triển khai (Testing Implementation Details):** Ví dụ, kiểm tra xem một phương thức private cụ thể có được gọi hay không (thường là không nên). Khi bạn refactor cấu trúc bên trong, test sẽ bị hỏng dù hành vi bên ngoài không đổi.
  * **Quá lạm dụng Mocking (Over-mocking):** Mock quá nhiều thứ, kể cả các lớp đơn giản hoặc các lớp bạn tự viết, có thể làm cho test trở nên phức tạp, khó hiểu và gắn chặt vào cấu trúc hiện tại, cản trở refactoring.
  * **Assertion không đủ mạnh:** Chỉ kiểm tra `assertNotNull` trong khi có thể kiểm tra giá trị cụ thể bằng `assertEquals`.
  * **Bỏ quên các trường hợp biên (Edge Cases):** Chỉ test các trường hợp "happy path" mà quên các giá trị `null`, rỗng, số âm, số 0, giá trị lớn nhất/nhỏ nhất,...

### 5.3. Debugging Tips khi Test Thất bại

  * **Đọc kỹ thông báo lỗi:** Thông báo lỗi từ JUnit/Mockito thường rất chi tiết, cho biết assertion nào thất bại, giá trị mong đợi và giá trị thực tế là gì, hoặc mock nào không nhận được lời gọi mong muốn.
  * **Chạy lại chỉ test bị lỗi:** Các IDE cho phép chạy riêng lẻ từng phương thức test. Điều này giúp cô lập vấn đề.
  * **Sử dụng Debugger:** Đặt breakpoint trong mã test và mã sản xuất để theo dõi luồng thực thi và giá trị của các biến tại thời điểm xảy ra lỗi.
  * **Kiểm tra lại bước Arrange:** Đảm bảo dữ liệu đầu vào và trạng thái của mock/stub được thiết lập đúng như bạn nghĩ.
  * **Kiểm tra lại bước Assert/Verify:** Đảm bảo bạn đang kiểm tra đúng điều kiện và giá trị mong đợi là chính xác.
  * **Tạm thời comment out các test khác:** Nếu nghi ngờ có sự phụ thuộc ngầm giữa các test, hãy thử chạy test lỗi một mình.

## 6\. Kết luận

Phát triển Hướng Kiểm thử (TDD) không chỉ là một kỹ thuật viết test, mà là một phương pháp luận phát triển phần mềm toàn diện giúp nâng cao chất lượng mã nguồn, cải thiện thiết kế, giảm thiểu lỗi và tăng sự tự tin cho lập trình viên khi thay đổi hệ thống. Mặc dù có đường cong học tập ban đầu, việc nắm vững và áp dụng chu trình Red-Green-Refactor cùng các công cụ hỗ trợ như JUnit và Mockito sẽ mang lại lợi ích to lớn trong dài hạn cho cá nhân lập trình viên và toàn bộ dự án. Hãy bắt đầu thực hành TDD với những chức năng nhỏ và dần dần tích hợp nó vào quy trình làm việc hàng ngày của bạn.