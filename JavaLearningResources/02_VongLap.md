# VÒNG LẶP

## 1. Giới thiệu về Vòng lặp (Introduction to Loops)

Trong lập trình, chúng ta thường xuyên gặp phải các tác vụ cần được thực hiện lặp đi lặp lại nhiều lần. Thay vì viết cùng một đoạn mã nhiều lần, các ngôn ngữ lập trình cung cấp **cấu trúc điều khiển lặp** hay **vòng lặp** (loops). Vòng lặp cho phép thực thi một khối mã (body of the loop) nhiều lần dựa trên một điều kiện nhất định.

**Tại sao cần vòng lặp?**

* **Tự động hóa tác vụ lặp lại:** Giảm thiểu sự trùng lặp mã (DRY - Don't Repeat Yourself).
* **Xử lý tập dữ liệu:** Duyệt qua các phần tử của mảng, collection, hoặc dòng dữ liệu.
* **Thực hiện tính toán lặp:** Ví dụ: tính tổng, tìm giá trị lớn nhất/nhỏ nhất trong một dãy số.
* **Mô phỏng hoặc chờ đợi:** Duy trì trạng thái chờ cho đến khi một điều kiện được thỏa mãn.

Java cung cấp một số loại vòng lặp chính, mỗi loại phù hợp với các tình huống sử dụng khác nhau:

* `for`: Thường dùng khi biết trước số lần lặp hoặc cần một biến đếm rõ ràng.
* `while`: Thích hợp khi số lần lặp không xác định trước và việc lặp phụ thuộc vào một điều kiện logic. Khối lệnh có thể không được thực thi lần nào.
* `do-while`: Tương tự `while`, nhưng đảm bảo khối lệnh được thực thi ít nhất một lần trước khi kiểm tra điều kiện.
* `for-each` (Enhanced `for` loop): Cú pháp đơn giản hóa để duyệt qua từng phần tử của mảng hoặc collection.

Ngoài ra, có các lệnh điều khiển luồng có thể sử dụng bên trong vòng lặp:

* `break`: Thoát khỏi vòng lặp ngay lập tức.
* `continue`: Bỏ qua phần còn lại của lần lặp hiện tại và chuyển sang lần lặp tiếp theo.

## 2. Vòng lặp `for` (The `for` Loop)

Vòng lặp `for` là cấu trúc lặp phổ biến nhất khi bạn biết số lần lặp cần thực hiện hoặc cần kiểm soát chặt chẽ biến đếm (control variable/counter).

### 2.1. Cú pháp cơ bản (Basic Syntax)

```java
for (khởi_tạo; điều_kiện_tiếp_tục; cập_nhật) {
    // Khối lệnh (statements) sẽ được thực thi lặp lại
}
```

* **`khởi_tạo` (initialization):** Một hoặc nhiều biểu thức được thực thi *một lần duy nhất* khi vòng lặp bắt đầu. Thường dùng để khai báo và gán giá trị ban đầu cho biến điều khiển. Ví dụ: `int i = 0`.
* **`điều_kiện_tiếp_tục` (loop-continuation-condition):** Một biểu thức boolean. Trước mỗi lần lặp (kể cả lần đầu tiên), biểu thức này được đánh giá. Nếu là `true`, khối lệnh bên trong được thực thi. Nếu là `false`, vòng lặp kết thúc. Ví dụ: `i < 10`.
* **`cập_nhật` (action-after-each-iteration / update):** Một hoặc nhiều biểu thức được thực thi *sau* mỗi lần khối lệnh bên trong hoàn thành. Thường dùng để thay đổi giá trị biến điều khiển. Ví dụ: `i++`, `i += 2`.
* **Khối lệnh (statement(s)):** Các câu lệnh Java sẽ được thực thi trong mỗi lần lặp khi điều kiện là `true`.

### 2.2. Luồng thực thi (Execution Flow)

1.  **Khởi tạo:** Phần `khởi_tạo` được thực thi.
2.  **Kiểm tra điều kiện:** Phần `điều_kiện_tiếp_tục` được đánh giá.
3.  **Thực thi hoặc Kết thúc:**
    * Nếu điều kiện là `true`:
        * Thực thi **Khối lệnh** bên trong vòng lặp.
        * Thực thi phần `cập_nhật`.
        * Quay lại bước 2 (Kiểm tra điều kiện).
    * Nếu điều kiện là `false`: Vòng lặp kết thúc. Chương trình tiếp tục thực thi câu lệnh ngay sau vòng lặp `for`.

### 2.3. Ví dụ minh họa (Illustrative Examples)

**Ví dụ 1: In các số từ 1 đến 5**

```java
public class ForExample1 {
    public static void main(String[] args) {
        System.out.println("In các số từ 1 đến 5:");
        for (int i = 1; i <= 5; i++) { // Khởi tạo i=1, lặp khi i<=5, tăng i sau mỗi lần
            System.out.println("Số: " + i);
        }
        System.out.println("Kết thúc vòng lặp.");
    }
}
// Output:
// In các số từ 1 đến 5:
// Số: 1
// Số: 2
// Số: 3
// Số: 4
// Số: 5
// Kết thúc vòng lặp.
```

**Ví dụ 2: Tính giai thừa của một số (N!)**

```java
import java.util.Scanner;

public class Factorial {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Nhập một số nguyên không âm n: ");
        int n = scanner.nextInt();
        long factorial = 1; // Sử dụng long để tránh tràn số với n lớn

        if (n < 0) {
            System.out.println("Không thể tính giai thừa cho số âm.");
        } else {
            // Bắt đầu từ 1 hoặc 2 đều được, nhân đến n
            for (int i = 1; i <= n; i++) {
                factorial = factorial * i;
            }
            System.out.println(n + "! = " + factorial);
        }
        scanner.close(); // Đóng scanner khi không dùng nữa
    }
}
```

**Ví dụ 3: Duyệt qua các phần tử của mảng**

```java
public class ForArrayExample {
    public static void main(String[] args) {
        char[] vowels = {'a', 'e', 'i', 'o', 'u'};

        System.out.println("Các nguyên âm:");
        // Dùng index để truy cập từng phần tử
        for (int i = 0; i < vowels.length; i++) {
            System.out.print(vowels[i] + " ");
        }
        System.out.println("\nKết thúc.");
    }
}
// Output:
// Các nguyên âm:
// a e i o u
// Kết thúc.
```

### 2.4. Phạm vi biến điều khiển (Control Variable Scope)

Nếu biến điều khiển (ví dụ: `i` trong các ví dụ trên) được khai báo *bên trong* phần `khởi_tạo` của vòng lặp `for`, thì phạm vi (scope) của nó chỉ giới hạn trong vòng lặp đó. Bạn không thể truy cập biến này từ bên ngoài vòng lặp.

```java
for (int count = 0; count < 3; count++) {
    System.out.println("Lần lặp: " + count);
}
// System.out.println(count); // Lỗi biên dịch! count không tồn tại ở đây.

int total; // Khai báo bên ngoài
for (total = 0; total < 5; total++) {
    // ...
}
System.out.println("Giá trị cuối của total: " + total); // OK, total có thể truy cập được.
```

### 2.5. Các biến thể (Variations)

* **Nhiều biểu thức khởi tạo/cập nhật:** Bạn có thể có nhiều câu lệnh trong phần khởi tạo và cập nhật, cách nhau bởi dấu phẩy.
    ```java
    for (int i = 0, j = 10; i < j; i++, j--) {
        System.out.println("i = " + i + ", j = " + j);
    }
    ```
* **Bỏ trống các phần:** Bất kỳ phần nào của `for` (khởi tạo, điều kiện, cập nhật) cũng có thể được bỏ trống, nhưng dấu chấm phẩy (`;`) vẫn phải giữ lại.
    ```java
    int k = 0;
    for (; k < 5; ) { // Khởi tạo bên ngoài, cập nhật bên trong
        System.out.println("k = " + k);
        k++;
    }

    // Vòng lặp vô hạn (cẩn thận!)
    // for (;;) {
    //     System.out.println("Lặp mãi mãi...");
    //     // Cần có lệnh break hoặc điều kiện thoát khác bên trong
    // }
    ```

## 3. Vòng lặp `while` (The `while` Loop)

Vòng lặp `while` thực thi một khối lệnh lặp đi lặp lại *chừng nào* (while) biểu thức điều kiện còn đúng (`true`). Nó phù hợp khi bạn không biết trước số lần lặp, mà việc lặp phụ thuộc vào một điều kiện có thể thay đổi trong quá trình thực thi.

### 3.1. Cú pháp và Đặc điểm (Syntax and Characteristics)

```java
while (điều_kiện_tiếp_tục) {
    // Khối lệnh (statements) sẽ được thực thi
    // Cần có câu lệnh làm thay đổi điều_kiện_tiếp_tục để tránh lặp vô hạn
}
```

* **`điều_kiện_tiếp_tục` (loop-continuation-condition):** Biểu thức boolean được kiểm tra *trước* mỗi lần lặp.
* **Đặc điểm quan trọng:** `while` là vòng lặp kiểm tra trước (pre-test loop). Điều kiện được kiểm tra ngay từ đầu. Nếu điều kiện ban đầu là `false`, khối lệnh bên trong sẽ **không bao giờ** được thực thi.

### 3.2. Luồng thực thi (Execution Flow)

1.  **Kiểm tra điều kiện:** Phần `điều_kiện_tiếp_tục` được đánh giá.
2.  **Thực thi hoặc Kết thúc:**
    * Nếu điều kiện là `true`:
        * Thực thi **Khối lệnh** bên trong vòng lặp.
        * Quay lại bước 1 (Kiểm tra điều kiện).
    * Nếu điều kiện là `false`: Vòng lặp kết thúc. Chương trình tiếp tục với câu lệnh sau vòng lặp `while`.

### 3.3. Ví dụ minh họa (Illustrative Examples)

**Ví dụ 1: Đọc dữ liệu đến khi gặp giá trị sentinel (-1)**

```java
import java.util.Scanner;

public class WhileSentinel {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        int data;
        int sum = 0;

        System.out.print("Nhập các số nguyên (nhập -1 để kết thúc): ");
        data = input.nextInt(); // Đọc giá trị đầu tiên

        while (data != -1) { // Kiểm tra điều kiện trước khi vào lặp
            sum += data;
            System.out.print("Nhập số tiếp theo (-1 để kết thúc): ");
            data = input.nextInt(); // Đọc giá trị tiếp theo bên trong vòng lặp
        }

        System.out.println("Tổng các số đã nhập là: " + sum);
        input.close();
    }
}
```

**Ví dụ 2: Giảm dần giá trị đến 0**

```java
public class Countdown {
    public static void main(String[] args) {
        int counter = 5;
        System.out.println("Bắt đầu đếm ngược:");
        while (counter > 0) {
            System.out.println(counter);
            counter--; // Quan trọng: Cập nhật biến điều kiện
        }
        System.out.println("Kết thúc!");
    }
}
// Output:
// Bắt đầu đếm ngược:
// 5
// 4
// 3
// 2
// 1
// Kết thúc!
```

### 3.4. So sánh với `for` (Comparison with `for`)

* Sử dụng `for` khi:
    * Biết rõ số lần lặp (ví dụ: lặp 10 lần, lặp qua tất cả phần tử mảng).
    * Cần một biến đếm được quản lý chặt chẽ (khởi tạo, điều kiện, cập nhật rõ ràng ở một nơi).
* Sử dụng `while` khi:
    * Số lần lặp không xác định trước, phụ thuộc vào một điều kiện phức tạp hơn hoặc thay đổi do các yếu tố bên ngoài (ví dụ: nhập liệu người dùng, trạng thái hệ thống).
    * Không cần một biến đếm tường minh.

Về mặt kỹ thuật, mọi vòng lặp `for` đều có thể viết lại bằng `while` và ngược lại, nhưng việc lựa chọn cấu trúc phù hợp giúp mã nguồn rõ ràng và dễ hiểu hơn.

## 4. Vòng lặp `do-while` (The `do-while` Loop)

Vòng lặp `do-while` tương tự như `while`, nhưng có một khác biệt cơ bản: điều kiện lặp được kiểm tra *sau* khi khối lệnh bên trong được thực thi. Điều này đảm bảo rằng khối lệnh **luôn được thực thi ít nhất một lần**, ngay cả khi điều kiện ban đầu là `false`.

### 4.1. Cú pháp và Đặc điểm (Syntax and Characteristics)

```java
do {
    // Khối lệnh (statements) sẽ được thực thi
    // Cần có câu lệnh làm thay đổi điều_kiện_tiếp_tục
} while (điều_kiện_tiếp_tục); // Lưu ý dấu chấm phẩy ở cuối
```

* **Đặc điểm quan trọng:** `do-while` là vòng lặp kiểm tra sau (post-test loop). Khối lệnh thực thi trước, sau đó mới kiểm tra `điều_kiện_tiếp_tục`.

### 4.2. Luồng thực thi (Execution Flow)

1.  **Thực thi Khối lệnh:** Khối lệnh bên trong `do {...}` được thực thi.
2.  **Kiểm tra điều kiện:** Phần `điều_kiện_tiếp_tục` sau `while` được đánh giá.
3.  **Lặp lại hoặc Kết thúc:**
    * Nếu điều kiện là `true`: Quay lại bước 1 (Thực thi Khối lệnh).
    * Nếu điều kiện là `false`: Vòng lặp kết thúc. Chương trình tiếp tục với câu lệnh sau `do-while`.

### 4.3. Ví dụ minh họa (Illustrative Examples)

**Ví dụ 1: Menu lựa chọn đơn giản**

```java
import java.util.Scanner;

public class DoWhileMenu {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int choice;

        do {
            // Hiển thị menu (luôn hiển thị ít nhất 1 lần)
            System.out.println("\n--- MENU ---");
            System.out.println("1. Xem thông tin");
            System.out.println("2. Cập nhật cài đặt");
            System.out.println("0. Thoát");
            System.out.print("Nhập lựa chọn của bạn: ");

            choice = scanner.nextInt(); // Nhận lựa chọn

            switch (choice) {
                case 1:
                    System.out.println("=> Đang hiển thị thông tin...");
                    break;
                case 2:
                    System.out.println("=> Đang cập nhật cài đặt...");
                    break;
                case 0:
                    System.out.println("=> Tạm biệt!");
                    break;
                default:
                    System.out.println("=> Lựa chọn không hợp lệ. Vui lòng chọn lại.");
            }
        } while (choice != 0); // Lặp lại nếu chưa chọn thoát (0)

        scanner.close();
    }
}
```

**Ví dụ 2: Yêu cầu nhập mật khẩu đúng**

```java
import java.util.Scanner;

public class PasswordCheck {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        String passwordAttempt;
        final String CORRECT_PASSWORD = "admin"; // Mật khẩu đúng

        do {
            System.out.print("Nhập mật khẩu: ");
            passwordAttempt = input.nextLine(); // Đọc cả dòng

            if (!passwordAttempt.equals(CORRECT_PASSWORD)) {
                System.out.println("Mật khẩu sai. Thử lại.");
            }
        } while (!passwordAttempt.equals(CORRECT_PASSWORD)); // Lặp lại nếu nhập sai

        System.out.println("Đăng nhập thành công!");
        input.close();
    }
}
```

### 4.4. So sánh với `while` (Comparison with `while`)

* **`while`**: Kiểm tra điều kiện *trước*. Có thể không thực thi khối lệnh lần nào.
* **`do-while`**: Kiểm tra điều kiện *sau*. Luôn thực thi khối lệnh ít nhất một lần.
* Sử dụng `do-while` khi bạn cần đảm bảo hành động bên trong vòng lặp xảy ra tối thiểu một lần, ví dụ như hiển thị menu, yêu cầu nhập liệu lần đầu.

## 5. Vòng lặp `for-each` (Enhanced `for` Loop)

Được giới thiệu từ Java 5, vòng lặp `for-each` (còn gọi là enhanced `for` loop) cung cấp một cú pháp gọn gàng và dễ đọc hơn để duyệt qua **tất cả** các phần tử của một **mảng** (array) hoặc một đối tượng **collection** (như `ArrayList`, `HashSet`, `LinkedList`, v.v., tức là các lớp cài đặt interface `java.lang.Iterable`).

### 5.1. Cú pháp và Mục đích (Syntax and Purpose)

```java
for (Kiểu_Phần_Tử tên_biến_tạm : nguồn_dữ_liệu) {
    // Sử dụng tên_biến_tạm để truy cập phần tử hiện tại
}
```

* **`Kiểu_Phần_Tử` (Type):** Kiểu dữ liệu của các phần tử trong mảng/collection. Phải khớp hoặc tương thích (ví dụ: kiểu cha).
* **`tên_biến_tạm` (variable):** Một biến cục bộ sẽ lần lượt nhận giá trị của từng phần tử trong `nguồn_dữ_liệu` qua mỗi lần lặp.
* **`nguồn_dữ_liệu` (collection/array):** Mảng hoặc đối tượng collection cần duyệt.

**Mục đích:** Đơn giản hóa việc lặp tuần tự qua từng phần tử mà không cần quan tâm đến chỉ số (index) hay bộ lặp (iterator) một cách tường minh.

### 5.2. Cách thức hoạt động (How it Works Internally - Expansion)

Về bản chất, `for-each` là một dạng "syntactic sugar" (cú pháp tiện lợi).

* **Đối với mảng:** Trình biên dịch Java sẽ biến đổi nó thành một vòng lặp `for` thông thường sử dụng chỉ số.
* **Đối với collections (lớp cài đặt `Iterable`):** Trình biên dịch sẽ biến đổi nó thành mã sử dụng `Iterator`. Nó gọi phương thức `iterator()` của collection để lấy một `Iterator`, sau đó sử dụng vòng lặp `while` với các phương thức `hasNext()` và `next()` của iterator đó để duyệt qua các phần tử.

```java
// Ví dụ với ArrayList<String> names;
// for (String name : names) { System.out.println(name); }
// Tương đương (về mặt khái niệm) với:
Iterator<String> it = names.iterator();
while (it.hasNext()) {
    String name = it.next();
    System.out.println(name);
}
```

Hiểu điều này giúp nhận biết một số hạn chế của `for-each`.

### 5.3. Ví dụ minh họa (Illustrative Examples)

**Ví dụ 1: Duyệt `ArrayList<String>`**

```java
import java.util.ArrayList;
import java.util.List;

public class ForEachList {
    public static void main(String[] args) {
        List<String> fruits = new ArrayList<>();
        fruits.add("Táo");
        fruits.add("Cam");
        fruits.add("Chuối");

        System.out.println("Danh sách trái cây:");
        for (String fruit : fruits) { // Đơn giản và dễ đọc
            System.out.println("- " + fruit);
        }
    }
}
```

**Ví dụ 2: Duyệt mảng `int[]`**

```java
public class ForEachArray {
    public static void main(String[] args) {
        int[] scores = { 90, 85, 92, 78, 88 };
        int sum = 0;

        System.out.print("Các điểm số: ");
        for (int score : scores) {
            System.out.print(score + " ");
            sum += score;
        }
        System.out.println("\nTổng điểm: " + sum);
        double average = (double) sum / scores.length;
        System.out.println("Điểm trung bình: " + average);
    }
}
```

### 5.4. Ưu điểm và Hạn chế (Advantages and Limitations)

**Ưu điểm:**

* **Ngắn gọn, dễ đọc:** Cú pháp rõ ràng, thể hiện ý định duyệt qua tất cả phần tử.
* **Ít lỗi hơn:** Tránh được các lỗi phổ biến liên quan đến chỉ số (ví dụ: `ArrayIndexOutOfBoundsException`).
* **Ẩn chi tiết lặp:** Không cần quản lý biến đếm hoặc `Iterator` thủ công.

**Hạn chế:**

* **Chỉ duyệt tới (Forward only):** Không thể duyệt ngược hoặc nhảy cóc phần tử.
* **Không có quyền truy cập chỉ số (index):** Nếu bạn cần biết vị trí của phần tử đang xét, bạn phải dùng vòng lặp `for` truyền thống.
* **Không thể sửa đổi cấu trúc collection khi đang duyệt:** Việc thêm/xóa phần tử khỏi collection *bên trong* vòng lặp `for-each` (trừ khi thông qua `Iterator.remove()`, nhưng `for-each` không cung cấp trực tiếp `Iterator`) thường dẫn đến lỗi `ConcurrentModificationException`. Vòng lặp `for` truyền thống với chỉ số cũng có thể gặp vấn đề nếu kích thước mảng/list thay đổi không đúng cách.
* **Không thể gán lại giá trị cho phần tử mảng/collection qua biến tạm:** Biến tạm (`tên_biến_tạm`) chỉ là một bản sao (đối với kiểu nguyên thủy) hoặc một tham chiếu sao chép (đối với đối tượng). Việc gán giá trị mới cho biến tạm này không làm thay đổi phần tử gốc trong mảng/collection (trừ khi bạn gọi phương thức thay đổi trạng thái *bên trong* đối tượng đó).
    ```java
    int[] numbers = {1, 2, 3};
    for (int num : numbers) {
        num = num * 2; // Chỉ thay đổi biến tạm 'num', không đổi phần tử trong 'numbers'
    }
    // numbers vẫn là {1, 2, 3}

    List<StringBuilder> builders = new ArrayList<>();
    builders.add(new StringBuilder("a"));
    for (StringBuilder sb : builders) {
        sb.append("b"); // Thay đổi trạng thái *bên trong* đối tượng StringBuilder
    }
    // builders bây giờ chứa ["ab"]
    ```

## 6. Các Lệnh Điều khiển Vòng lặp (Loop Control Statements)

Java cung cấp hai lệnh chính để thay đổi luồng thực thi bình thường của vòng lặp: `break` và `continue`.

### 6.1. Lệnh `break` (The `break` Statement)

**Chức năng:**

* Khi gặp lệnh `break` bên trong một vòng lặp (`for`, `while`, `do-while`) hoặc một cấu trúc `switch`, nó sẽ **ngay lập tức chấm dứt** (terminate) vòng lặp hoặc `switch` đó.
* Việc thực thi sẽ tiếp tục tại câu lệnh *ngay sau* cấu trúc vòng lặp/`switch` bị chấm dứt.
* Nếu `break` nằm trong vòng lặp lồng nhau, nó chỉ thoát khỏi vòng lặp **bên trong nhất** chứa nó.

**Ví dụ:** Tìm phần tử đầu tiên trong mảng và dừng lại.

```java
public class BreakExample {
    public static void main(String[] args) {
        int[] numbers = { 10, 25, 5, 40, 5, 50 };
        int target = 5;
        int foundIndex = -1; // Khởi tạo là không tìm thấy

        for (int i = 0; i < numbers.length; i++) {
            System.out.println("Đang kiểm tra index " + i + " (giá trị " + numbers[i] + ")");
            if (numbers[i] == target) {
                foundIndex = i;
                System.out.println("Tìm thấy " + target + " tại index " + i + ". Thoát vòng lặp.");
                break; // Thoát khỏi vòng lặp for ngay lập tức
            }
        }

        if (foundIndex != -1) {
            System.out.println("Phần tử " + target + " được tìm thấy đầu tiên tại vị trí: " + foundIndex);
        } else {
            System.out.println("Phần tử " + target + " không có trong mảng.");
        }
        System.out.println("Câu lệnh sau vòng lặp.");
    }
}
```

**Sử dụng `break` với nhãn (Labels - *Expansion*):**

Đôi khi bạn muốn thoát khỏi một vòng lặp *bên ngoài* từ bên trong một vòng lặp lồng nhau. Bạn có thể sử dụng nhãn (label) cho vòng lặp và chỉ định nhãn đó cho lệnh `break`.

```java
public class LabeledBreak {
    public static void main(String[] args) {
        outerLoop: // Đây là nhãn cho vòng lặp for bên ngoài
        for (int i = 1; i <= 3; i++) {
            System.out.println("Bắt đầu vòng lặp ngoài i = " + i);
            for (int j = 1; j <= 3; j++) {
                System.out.println("  Vòng lặp trong j = " + j);
                if (i == 2 && j == 2) {
                    System.out.println("  ==> Điều kiện đặc biệt! Thoát cả hai vòng lặp.");
                    break outerLoop; // Thoát khỏi vòng lặp có nhãn 'outerLoop'
                }
            }
            System.out.println("Kết thúc vòng lặp ngoài i = " + i); // Dòng này sẽ không chạy khi i=2
        }
        System.out.println("Đã thoát khỏi vòng lặp ngoài.");
    }
}
```

### 6.2. Lệnh `continue` (The `continue` Statement)

**Chức năng:**

* Khi gặp lệnh `continue` bên trong một vòng lặp (`for`, `while`, `do-while`), nó sẽ **bỏ qua** (skip) tất cả các câu lệnh còn lại trong lần lặp *hiện tại* của vòng lặp đó.
* Luồng thực thi sẽ chuyển ngay đến phần **cập nhật** (đối với `for`) hoặc **kiểm tra điều kiện** (đối với `while`, `do-while`) để bắt đầu lần lặp tiếp theo (nếu điều kiện vẫn còn `true`).

**Ví dụ:** Chỉ xử lý các số dương trong mảng.

```java
public class ContinueExample {
    public static void main(String[] args) {
        int[] data = { 10, -5, 20, 0, -15, 30 };
        int sumOfPositives = 0;

        System.out.println("Xử lý mảng:");
        for (int number : data) {
            System.out.print("Xét số: " + number);
            if (number <= 0) {
                System.out.println(" => Bỏ qua (không phải số dương).");
                continue; // Bỏ qua phần còn lại của lần lặp này
            }
            // Chỉ thực thi nếu không continue
            System.out.println(" => Cộng vào tổng.");
            sumOfPositives += number;
        }
        System.out.println("\nTổng các số dương là: " + sumOfPositives);
    }
}
```

**Sử dụng `continue` với nhãn (Labels - *Expansion*):**

Tương tự như `break`, bạn có thể dùng `continue` với nhãn để bỏ qua lần lặp hiện tại của vòng lặp bên ngoài từ bên trong vòng lặp lồng nhau, và chuyển đến lần lặp *tiếp theo* của vòng lặp có nhãn đó.

```java
public class LabeledContinue {
    public static void main(String[] args) {
        outer: // Nhãn cho vòng lặp ngoài
        for (int i = 1; i <= 3; i++) {
            System.out.println("Bắt đầu vòng lặp ngoài i=" + i);
            for (int j = 1; j <= 3; j++) {
                if (j == 2) {
                    System.out.println("  j=" + j + " => Bỏ qua lần lặp ngoài hiện tại (i=" + i +")");
                    continue outer; // Bỏ qua phần còn lại của cả lần lặp i hiện tại
                                     // và chuyển sang lần lặp i tiếp theo
                }
                System.out.println("    Thực thi: i=" + i + ", j=" + j); // Sẽ không chạy khi j=2
            }
            System.out.println("Kết thúc vòng lặp trong cho i=" + i); // Sẽ không chạy khi j=2 kích hoạt continue outer
        }
        System.out.println("Hoàn thành tất cả.");
    }
}
```

## 7. Vòng lặp lồng nhau (Nested Loops)

### 7.1. Khái niệm (Concept)

Bạn có thể đặt một vòng lặp bên trong một vòng lặp khác. Đây được gọi là **vòng lặp lồng nhau** (nested loops). Vòng lặp bên trong sẽ hoàn thành tất cả các lần lặp của nó cho *mỗi* lần lặp của vòng lặp bên ngoài.

### 7.2. Ví dụ (Example)

**Ví dụ 1: In một ma trận (mảng 2 chiều)**

```java
public class NestedLoopMatrix {
    public static void main(String[] args) {
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };

        System.out.println("In ma trận:");
        // Vòng lặp ngoài duyệt qua từng hàng (row)
        for (int i = 0; i < matrix.length; i++) {
            // Vòng lặp trong duyệt qua từng phần tử (column) trong hàng hiện tại
            for (int j = 0; j < matrix[i].length; j++) {
                System.out.print(matrix[i][j] + "\t"); // In phần tử và tab
            }
            System.out.println(); // Xuống dòng sau khi hết một hàng
        }
    }
}
```

**Ví dụ 2: Vẽ một tam giác vuông bằng dấu `*`**

```java
public class StarTriangle {
    public static void main(String[] args) {
        int height = 5;
        System.out.println("Vẽ tam giác:");
        for (int i = 1; i <= height; i++) { // Vòng ngoài kiểm soát số hàng
            for (int j = 1; j <= i; j++) { // Vòng trong kiểm soát số '*' trên mỗi hàng
                System.out.print("* ");
            }
            System.out.println(); // Xuống dòng
        }
    }
}
```

### 7.3. Cân nhắc hiệu năng (Performance Considerations - *Expansion*)

Vòng lặp lồng nhau có thể ảnh hưởng đáng kể đến hiệu năng, đặc biệt khi tập dữ liệu lớn.

* **Độ phức tạp thời gian (Time Complexity):** Nếu vòng lặp ngoài chạy N lần và vòng lặp trong chạy M lần cho mỗi lần lặp ngoài, tổng số lần thực thi của khối lệnh bên trong nhất sẽ là N * M. Nếu cả hai vòng lặp đều chạy N lần, độ phức tạp là O(N^2) (bậc hai). Với 3 vòng lặp lồng nhau, có thể là O(N^3), v.v.
* **Tối ưu hóa:**
    * Cố gắng giảm thiểu công việc thực hiện bên trong vòng lặp trong cùng.
    * Xem xét liệu có thể tính toán trước một số giá trị bên ngoài vòng lặp trong không.
    * Kiểm tra xem có thể sử dụng cấu trúc dữ liệu hoặc thuật toán khác hiệu quả hơn để tránh lặp lồng sâu không.

## 8. Thực tiễn Tốt nhất và Lỗi thường gặp (Best Practices and Common Pitfalls)

### 8.1. Lựa chọn vòng lặp phù hợp (Choosing the Right Loop)

* **`for`:** Khi biết số lần lặp, cần biến đếm rõ ràng (ví dụ: duyệt mảng theo chỉ số).
* **`for-each`:** Khi chỉ cần duyệt tuần tự qua tất cả phần tử của mảng/collection mà không cần chỉ số hoặc sửa đổi cấu trúc. Ưu tiên vì tính dễ đọc.
* **`while`:** Khi số lần lặp không xác định, phụ thuộc điều kiện, và có thể không cần lặp lần nào.
* **`do-while`:** Khi cần đảm bảo khối lệnh thực thi ít nhất một lần (ví dụ: menu, nhập liệu lần đầu).

### 8.2. Tránh vòng lặp vô hạn (Avoiding Infinite Loops)

Lỗi vòng lặp chạy mãi mãi thường xảy ra do:

* **Điều kiện luôn đúng:** Điều kiện trong `while` hoặc `for` không bao giờ trở thành `false`.
* **Thiếu cập nhật:** Biến điều khiển trong điều kiện không được cập nhật đúng cách bên trong vòng lặp để tiến tới điểm dừng.
* **Logic sai:** Lỗi trong `break` hoặc `continue` khiến không đạt được điều kiện thoát.

**Debugging:**

* Sử dụng `System.out.println` để in giá trị của biến điều khiển trong mỗi lần lặp.
* Sử dụng debugger để theo dõi giá trị biến và luồng thực thi từng bước.
* Kiểm tra kỹ logic điều kiện và các lệnh cập nhật.

```java
// Lỗi: Vòng lặp vô hạn
int count = 0;
while (count < 10) {
    System.out.println("Đang chạy...");
    // Quên tăng count: count++;
}

// Lỗi: Điều kiện sai
for (int i = 1; i > 0; i++) { // i sẽ luôn > 0 nếu không có tràn số
    // ...
}
```

### 8.3. Phạm vi biến (Variable Scope)

* Khai báo biến bên trong vòng lặp (`for(int i...`) giới hạn phạm vi của nó trong vòng lặp đó. Điều này thường tốt hơn vì tránh làm "ô nhiễm" không gian tên bên ngoài.
* Nếu cần giá trị của biến sau khi vòng lặp kết thúc, hãy khai báo nó bên ngoài vòng lặp.

### 8.4. Tối ưu hóa (Optimization)

* **Tránh tính toán lặp lại:** Nếu một phép tính không thay đổi giá trị qua các lần lặp, hãy thực hiện nó trước khi vào vòng lặp.
    ```java
    // Kém hiệu quả
    for (int i = 0; i < list.size(); i++) { // list.size() gọi mỗi lần
       // ...
    }

    // Tốt hơn
    int size = list.size(); // Gọi 1 lần
    for (int i = 0; i < size; i++) {
       // ...
    }
    ```
* **Giảm thiểu công việc trong vòng lặp:** Đặc biệt là vòng lặp lồng nhau, hãy di chuyển các thao tác không cần thiết ra ngoài.
* **Thận trọng với tối ưu hóa sớm (Premature Optimization):** Chỉ tối ưu khi thực sự cần thiết và có bằng chứng (profiling) cho thấy vòng lặp là điểm nghẽn. Đôi khi mã rõ ràng quan trọng hơn một chút hiệu năng nhỏ.

### 8.5. Tính dễ đọc (Readability)

* Sử dụng tên biến có ý nghĩa (`index`, `count`, `item` thay vì `i`, `j`, `x`).
* Giữ khối lệnh bên trong vòng lặp ngắn gọn và tập trung vào một nhiệm vụ. Nếu phức tạp, tách ra thành phương thức riêng.
* Thụt lề (indentation) nhất quán.

### 8.6. Sử dụng `for-each` khi có thể (Using `for-each` When Possible)

Nếu chỉ cần duyệt qua các phần tử mà không cần chỉ số, `for-each` thường là lựa chọn tốt nhất vì nó ngắn gọn và ít lỗi hơn.

### 8.7. Cẩn thận khi sửa đổi Collection khi duyệt (Caution Modifying Collections During Iteration)

Như đã đề cập ở phần `for-each`, việc thêm/xóa phần tử khỏi một `Collection` (như `ArrayList`) trong khi đang duyệt nó bằng `for-each` hoặc `for` truyền thống dựa trên chỉ số có thể gây ra lỗi `ConcurrentModificationException` hoặc hành vi không mong muốn (bỏ sót phần tử, lỗi chỉ số). Nếu cần sửa đổi collection khi duyệt, hãy sử dụng `Iterator` một cách tường minh và phương thức `remove()` của nó, hoặc duyệt ngược bằng `for` truyền thống, hoặc tạo một collection mới để lưu kết quả.

## 9. Mở rộng: Vòng lặp và Java Streams API (Extension: Loops and Java Streams API - Expansion)

Từ Java 8, **Streams API** cung cấp một cách tiếp cận khác, mang tính **khai báo (declarative)** hơn để xử lý các tập hợp dữ liệu (collections, mảng). Thay vì viết chi tiết *cách* lặp (imperative loops), bạn mô tả *những gì* bạn muốn làm với dữ liệu.

**So sánh cách tiếp cận:**

* **Vòng lặp (Imperative):** Bạn tự quản lý việc lặp (biến đếm, điều kiện), thực hiện các thao tác trực tiếp.
* **Streams (Declarative):** Bạn tạo một luồng (stream) từ nguồn dữ liệu, áp dụng các **thao tác trung gian** (intermediate operations) như `filter` (lọc), `map` (ánh xạ/biến đổi), `sorted` (sắp xếp), và kết thúc bằng một **thao tác cuối** (terminal operation) như `forEach` (duyệt), `collect` (thu thập vào collection khác), `count` (đếm), `reduce` (giảm thiểu).

**Ví dụ tương đương:** Tính tổng bình phương các số lẻ trong một list.

**Cách dùng vòng lặp:**

```java
import java.util.Arrays;
import java.util.List;

public class LoopVsStream {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        int sumOfOddSquaresLoop = 0;

        for (int n : numbers) {
            if (n % 2 != 0) { // Lọc số lẻ
                int square = n * n; // Bình phương
                sumOfOddSquaresLoop += square; // Cộng dồn
            }
        }
        System.out.println("Tổng bình phương số lẻ (Loop): " + sumOfOddSquaresLoop);
    }
}
```

**Cách dùng Streams API:**

```java
import java.util.Arrays;
import java.util.List;

public class LoopVsStream {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        int sumOfOddSquaresStream = numbers.stream() // 1. Tạo stream
                                         .filter(n -> n % 2 != 0) // 2. Lọc số lẻ (intermediate)
                                         .mapToInt(n -> n * n) // 3. Bình phương (intermediate)
                                         .sum(); // 4. Tính tổng (terminal)

        System.out.println("Tổng bình phương số lẻ (Stream): " + sumOfOddSquaresStream);
    }
}
```

**Khi nào nên cân nhắc Streams:**

* Khi cần thực hiện chuỗi các thao tác (lọc, biến đổi, sắp xếp, gom nhóm) trên collection.
* Mã nguồn thường ngắn gọn và dễ đọc hơn cho các pipeline xử lý phức tạp.
* Streams có thể tận dụng xử lý song song (`parallelStream()`) dễ dàng hơn để tăng hiệu năng trên hệ thống đa lõi (cần cẩn thận với các tác vụ có trạng thái).
* Tuy nhiên, với các logic lặp rất đơn giản hoặc cần kiểm soát luồng chi tiết (dùng `break`/`continue`), vòng lặp truyền thống vẫn có thể phù hợp và đôi khi hiệu quả hơn. Debugging streams có thể khó hơn lúc ban đầu.

## 10. Kết luận (Conclusion)

Vòng lặp là một công cụ nền tảng và không thể thiếu trong lập trình Java cũng như các ngôn ngữ khác. Việc hiểu rõ cú pháp, cách hoạt động, ưu nhược điểm của từng loại vòng lặp (`for`, `while`, `do-while`, `for-each`) và các lệnh điều khiển (`break`, `continue`) là cực kỳ quan trọng. Lựa chọn đúng loại vòng lặp cho từng tình huống, viết mã rõ ràng, và nhận biết các lỗi phổ biến sẽ giúp bạn xây dựng các chương trình hiệu quả, dễ bảo trì và chính xác hơn. Cùng với sự phát triển của ngôn ngữ, các cách tiếp cận mới như Streams API cung cấp thêm lựa chọn mạnh mẽ cho việc xử lý dữ liệu, bổ sung cho các cấu trúc lặp truyền thống.