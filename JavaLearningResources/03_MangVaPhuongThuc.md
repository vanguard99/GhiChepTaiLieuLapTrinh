# MẢNG VÀ PHƯƠNG THỨC

## 1. Giới thiệu

Chào mừng bạn đến với hướng dẫn chi tiết về **Mảng (Arrays)** và **Phương thức (Methods)** trong ngôn ngữ lập trình Java. Đây là hai khái niệm nền tảng và cực kỳ quan trọng mà bất kỳ lập trình viên Java nào cũng cần nắm vững.

* **Mảng** cung cấp một cơ chế hiệu quả để lưu trữ và quản lý một tập hợp các phần tử dữ liệu có cùng kiểu.
* **Phương thức** cho phép chúng ta đóng gói các đoạn mã thực hiện những tác vụ cụ thể, giúp mã nguồn trở nên module hóa, dễ đọc, dễ bảo trì và tái sử dụng.

Tài liệu này sẽ đi sâu vào cách khai báo, khởi tạo, thao tác với mảng một chiều, mảng đa chiều, giới thiệu các tiện ích từ lớp `java.util.Arrays`, đồng thời so sánh mảng với các cấu trúc dữ liệu khác như `ArrayList`. Bên cạnh đó, chúng ta sẽ khám phá cách định nghĩa, gọi phương thức, cơ chế truyền tham số, nạp chồng phương thức (overloading), đệ quy và các phương pháp hay nhất khi làm việc với chúng.

Mục tiêu là cung cấp một cái nhìn toàn diện, sâu sắc về mặt kỹ thuật, kèm theo các ví dụ mã nguồn thực tế và lời khuyên hữu ích, giúp bạn không chỉ biết *cách sử dụng* mà còn hiểu *tại sao* và áp dụng hiệu quả vào các dự án của mình.

## 2. Mảng (Arrays) trong Java

### 2.1. Khái niệm Cốt lõi

**Mảng** là một cấu trúc dữ liệu cơ bản trong Java, cho phép lưu trữ một tập hợp các **phần tử có cùng kiểu dữ liệu** một cách liên tục trong bộ nhớ. Các phần tử này được truy cập thông qua một **chỉ số (index)** duy nhất.

**Tại sao cần Mảng?**
Trong lập trình, chúng ta thường xuyên cần xử lý nhiều giá trị cùng loại, ví dụ: danh sách điểm số của sinh viên, danh sách tên người dùng, tọa độ các điểm ảnh. Mảng cung cấp một cách thức có cấu trúc và hiệu quả để lưu trữ, truy cập và thao tác với các tập hợp dữ liệu này thay vì phải khai báo vô số biến riêng lẻ.

**Đặc điểm chính:**

* **Kiểu dữ liệu đồng nhất:** Tất cả các phần tử trong một mảng phải có cùng một kiểu dữ liệu (ví dụ: `int`, `double`, `String`, hoặc một kiểu đối tượng cụ thể).
* **Kích thước cố định (Fixed Size):** Một khi mảng đã được khởi tạo với một kích thước nhất định, kích thước đó không thể thay đổi được nữa.
* **Truy cập dựa trên chỉ số:** Mỗi phần tử trong mảng được liên kết với một chỉ số, bắt đầu từ `0`. Phần tử đầu tiên có chỉ số `0`, phần tử thứ hai có chỉ số `1`, và cứ thế tiếp tục. Chỉ số cuối cùng là `độ_dài - 1`.
* **Biến tham chiếu:** Biến mảng trong Java không trực tiếp chứa dữ liệu mảng mà chứa một **tham chiếu (reference)** trỏ đến vị trí của đối tượng mảng thực sự trong vùng nhớ Heap.

### 2.2. Khai báo và Khởi tạo Mảng

**Khai báo biến mảng:**
Có hai cú pháp tương đương để khai báo một biến mảng:

```java
// Cách 1 (Ưa chuộng hơn trong Java)
kiểu_dữ_liệu[] tên_biến_mảng;

// Cách 2 (Giống C/C++)
kiểu_dữ_liệu tên_biến_mảng[];
```

Ví dụ:
```java
int[] scores;       // Khai báo mảng số nguyên tên là scores
String[] names;     // Khai báo mảng chuỗi tên là names
double studentGrades[]; // Khai báo mảng số thực tên là studentGrades
```

**Biến mảng là biến tham chiếu:**
Khi bạn chỉ khai báo biến mảng như trên, bạn mới chỉ tạo ra một biến có khả năng **trỏ** đến một đối tượng mảng. Biến này chưa thực sự trỏ đến đâu cả và có giá trị mặc định là `null`.

```java
int[] numbers;
System.out.println(numbers); // Sẽ in ra: null
// numbers[0] = 10; // Lỗi! NullPointerException vì mảng chưa được tạo
```

**Khởi tạo mảng:**
Để mảng thực sự tồn tại và có thể chứa dữ liệu, bạn cần **khởi tạo** nó bằng toán tử `new`, chỉ định kiểu dữ liệu và kích thước (số lượng phần tử tối đa).

```java
// Cú pháp khởi tạo
tên_biến_mảng = new kiểu_dữ_liệu[kích_thước];
```

Ví dụ:
```java
scores = new int[5];      // Khởi tạo mảng scores có thể chứa 5 số nguyên
names = new String[10];   // Khởi tạo mảng names có thể chứa 10 chuỗi
```

Việc khởi tạo này sẽ cấp phát một vùng nhớ liên tục trên Heap đủ để chứa số lượng phần tử đã chỉ định, và gán địa chỉ của vùng nhớ đó cho biến mảng.

**Kết hợp Khai báo và Khởi tạo:**
Thông thường, người ta kết hợp hai bước này thành một:

```java
int[] dataPoints = new int[100];
String[] userNames = new String[50];
```

**Khởi tạo nhanh (Array Initializer):**
Java cung cấp cú pháp gọn hơn để vừa khai báo, vừa khởi tạo, vừa gán giá trị ban đầu cho các phần tử mảng cùng lúc:

```java
// Cú pháp
kiểu_dữ_liệu[] tên_biến_mảng = {giá_trị_0, giá_trị_1, ..., giá_trị_n};
```

Ví dụ:
```java
double[] coefficients = {1.9, 2.9, 3.4, 3.5}; // Mảng double có 4 phần tử
String[] weekdays = {"Thứ Hai", "Thứ Ba", "Thứ Tư", "Thứ Năm", "Thứ Sáu", "Thứ Bảy", "Chủ Nhật"};
boolean[] flags = {true, false, true};
```
Với cú pháp này, kích thước mảng được tự động xác định dựa trên số lượng giá trị bạn cung cấp.

**Giá trị mặc định của phần tử mảng:**
Khi bạn khởi tạo mảng bằng `new` mà không gán giá trị ngay, các phần tử sẽ nhận giá trị mặc định dựa trên kiểu dữ liệu của mảng:
* Kiểu số ( `byte`, `short`, `int`, `long`, `float`, `double`): `0` (hoặc `0.0`)
* Kiểu `char`: `\u0000` (ký tự null)
* Kiểu `boolean`: `false`
* Kiểu tham chiếu (Đối tượng, bao gồm `String`): `null`

```java
int[] counts = new int[3]; // counts[0]=0, counts[1]=0, counts[2]=0
String[] labels = new String[2]; // labels[0]=null, labels[1]=null
boolean[] states = new boolean[4]; // states[0]=false, ..., states[3]=false
```

### 2.3. Truy cập và Thao tác Phần tử Mảng

**Truy cập phần tử:**
Sử dụng tên mảng theo sau là chỉ số (index) của phần tử mong muốn đặt trong cặp dấu ngoặc vuông `[]`.

```java
int[] myNumbers = {10, 20, 30, 40, 50};

int firstNumber = myNumbers[0]; // Truy cập phần tử đầu tiên (chỉ số 0), firstNumber = 10
int thirdNumber = myNumbers[2]; // Truy cập phần tử thứ ba (chỉ số 2), thirdNumber = 30
```

**Gán giá trị cho phần tử:**
Tương tự như truy cập, bạn có thể gán giá trị mới cho một phần tử bằng cách sử dụng chỉ số:

```java
String[] fruits = new String[3];
fruits[0] = "Apple";
fruits[1] = "Banana";
fruits[2] = "Orange";

// Thay đổi giá trị
fruits[1] = "Mango"; // Bây giờ fruits là {"Apple", "Mango", "Orange"}
```

**Lấy độ dài mảng:**
Mỗi đối tượng mảng có một thuộc tính (field) công khai là `length`, chứa số lượng phần tử mà mảng có thể lưu trữ (kích thước đã khai báo).

```java
double[] temperatures = new double[10];
int size = temperatures.length; // size sẽ là 10

String[] colors = {"Red", "Green", "Blue"};
int numberOfColors = colors.length; // numberOfColors sẽ là 3
```
Lưu ý: `length` là một *thuộc tính*, không phải phương thức, nên không có dấu ngoặc đơn `()` theo sau.

**Chỉ số mảng và `ArrayIndexOutOfBoundsException`:**
Chỉ số hợp lệ của mảng chạy từ `0` đến `length - 1`. Nếu bạn cố gắng truy cập một phần tử với chỉ số nằm ngoài phạm vi này (ví dụ: chỉ số âm hoặc chỉ số lớn hơn hoặc bằng `length`), Java sẽ ném ra một ngoại lệ `ArrayIndexOutOfBoundsException` tại thời điểm chạy (runtime).

```java
int[] data = new int[5]; // Chỉ số hợp lệ: 0, 1, 2, 3, 4
// data[5] = 100; // Lỗi! ArrayIndexOutOfBoundsException
// data[-1] = 0;  // Lỗi! ArrayIndexOutOfBoundsException
```
Đây là một lỗi rất phổ biến, cần kiểm tra cẩn thận giới hạn vòng lặp và các phép tính liên quan đến chỉ số.

### 2.4. Duyệt Mảng

Duyệt qua các phần tử của mảng là một thao tác rất phổ biến. Có hai cách chính để làm điều này trong Java:

**Sử dụng vòng lặp `for` truyền thống:**
Cách này cho phép bạn kiểm soát hoàn toàn chỉ số `i`, hữu ích khi bạn cần biết vị trí của phần tử hoặc cần thay đổi giá trị phần tử trong mảng.

```java
int[] values = {5, 10, 15, 20, 25};
System.out.println("Duyệt bằng for truyền thống:");
for (int i = 0; i < values.length; i++) {
    System.out.println("Phần tử tại chỉ số " + i + ": " + values[i]);
    // Có thể thay đổi giá trị: values[i] = values[i] * 2;
}
```

**Sử dụng vòng lặp `for-each` (Enhanced for loop):**
Cú pháp này gọn gàng hơn, thích hợp khi bạn chỉ cần truy cập giá trị của từng phần tử mà không cần quan tâm đến chỉ số. Nó dễ đọc hơn và ít khả năng gây lỗi `ArrayIndexOutOfBoundsException`.

```java
String[] languages = {"Java", "Python", "JavaScript"};
System.out.println("\nDuyệt bằng for-each:");
for (String lang : languages) {
    System.out.println("Ngôn ngữ: " + lang);
    // Lưu ý: Thay đổi biến `lang` ở đây không làm thay đổi phần tử gốc trong mảng `languages`.
    // lang = "C++"; // Chỉ thay đổi bản sao `lang`, không ảnh hưởng mảng gốc
}
```

**Khi nào dùng loại nào?**
* Dùng `for` truyền thống khi:
    * Bạn cần biết/sử dụng chỉ số của phần tử.
    * Bạn cần thay đổi giá trị các phần tử trong mảng.
    * Bạn cần duyệt ngược mảng hoặc duyệt với bước nhảy khác 1.
* Dùng `for-each` khi:
    * Bạn chỉ cần đọc giá trị của từng phần tử.
    * Bạn muốn mã nguồn ngắn gọn và dễ đọc hơn.
    * Bạn không cần chỉ số.

### 2.5. Mảng Đa chiều (Multidimensional Arrays)

Java hỗ trợ mảng đa chiều, thực chất là **mảng của các mảng**. Phổ biến nhất là mảng hai chiều, thường được dùng để biểu diễn ma trận, bảng biểu, hoặc lưới (grid).

**Khai báo và Khởi tạo Mảng Hai Chiều:**

```java
// Khai báo
kiểu_dữ_liệu[][] tên_mảng;

// Khởi tạo (mảng chữ nhật)
tên_mảng = new kiểu_dữ_liệu[số_hàng][số_cột];

// Kết hợp
int[][] matrix = new int[3][4]; // Tạo ma trận 3 hàng, 4 cột

// Khởi tạo nhanh
int[][] table = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
}; // Mảng 3x3
```

**Truy cập Phần tử Mảng Hai Chiều:**
Sử dụng hai cặp ngoặc vuông: `tên_mảng[chỉ_số_hàng][chỉ_số_cột]`.

```java
matrix[0][0] = 10; // Gán giá trị cho ô ở hàng 0, cột 0
int value = table[1][2]; // Lấy giá trị ở hàng 1, cột 2 (value = 6)
```

**Mảng "Răng cưa" (Jagged Arrays):**
Trong Java, các hàng của mảng hai chiều không nhất thiết phải có cùng độ dài. Bạn có thể tạo ra mảng "răng cưa".

```java
int[][] jaggedArray = new int[3][]; // Chỉ định số hàng
jaggedArray[0] = new int[2];      // Hàng 0 có 2 cột
jaggedArray[1] = new int[4];      // Hàng 1 có 4 cột
jaggedArray[2] = new int[3];      // Hàng 2 có 3 cột

jaggedArray[0][0] = 1;
jaggedArray[1][3] = 5;
// jaggedArray[0][2] = 9; // Lỗi! ArrayIndexOutOfBoundsException vì hàng 0 chỉ có 2 cột
```

**Duyệt Mảng Hai Chiều:**
Thường sử dụng vòng lặp lồng nhau:

```java
System.out.println("\nDuyệt ma trận:");
for (int row = 0; row < table.length; row++) { // table.length là số hàng
    for (int col = 0; col < table[row].length; col++) { // table[row].length là số cột của hàng `row`
        System.out.print(table[row][col] + "\t");
    }
    System.out.println(); // Xuống dòng sau mỗi hàng
}

// Hoặc dùng for-each lồng nhau
System.out.println("\nDuyệt ma trận bằng for-each:");
for (int[] row : table) {
    for (int element : row) {
        System.out.print(element + "\t");
    }
    System.out.println();
}
```

### 2.6. Lớp `java.util.Arrays`

Java cung cấp lớp tiện ích `java.util.Arrays` chứa nhiều phương thức `static` hữu ích để thao tác với mảng, giúp tiết kiệm thời gian và công sức so với việc tự viết các thuật toán cơ bản.

Một số phương thức quan trọng:

* `sort(array)`: Sắp xếp các phần tử của mảng theo thứ tự tăng dần (sử dụng thuật toán hiệu quả như Quicksort hoặc Timsort).
* `binarySearch(array, key)`: Tìm kiếm giá trị `key` trong mảng *đã được sắp xếp* bằng thuật toán tìm kiếm nhị phân. Trả về chỉ số nếu tìm thấy, hoặc giá trị âm nếu không tìm thấy.
* `equals(array1, array2)`: So sánh hai mảng xem chúng có bằng nhau không (cùng kích thước và tất cả các phần tử tương ứng bằng nhau).
* `fill(array, value)`: Gán giá trị `value` cho tất cả các phần tử của mảng.
* `copyOf(originalArray, newLength)`: Tạo một bản sao của mảng `originalArray` với độ dài `newLength`. Nếu `newLength` lớn hơn, các phần tử thừa sẽ có giá trị mặc định. Nếu nhỏ hơn, mảng sẽ bị cắt bớt.
* `copyOfRange(originalArray, fromIndex, toIndex)`: Tạo bản sao một phần của mảng gốc, từ `fromIndex` (bao gồm) đến `toIndex` (không bao gồm).
* `toString(array)`: Trả về một biểu diễn chuỗi của nội dung mảng, rất hữu ích cho việc gỡ lỗi (debugging).

**Ví dụ sử dụng:**

```java
import java.util.Arrays; // Cần import lớp này

public class ArraysDemo {
    public static void main(String[] args) {
        int[] numbers = {5, 2, 8, 1, 9, 4};

        // In mảng gốc
        System.out.println("Mảng gốc: " + Arrays.toString(numbers)); // Output: [5, 2, 8, 1, 9, 4]

        // Sắp xếp mảng
        Arrays.sort(numbers);
        System.out.println("Mảng sau sắp xếp: " + Arrays.toString(numbers)); // Output: [1, 2, 4, 5, 8, 9]

        // Tìm kiếm nhị phân (mảng phải được sắp xếp trước)
        int index = Arrays.binarySearch(numbers, 8);
        System.out.println("Chỉ số của số 8: " + index); // Output: 4
        int notFoundIndex = Arrays.binarySearch(numbers, 3);
        System.out.println("Chỉ số của số 3 (không tìm thấy): " + notFoundIndex); // Output: giá trị âm

        // Tạo bản sao
        int[] numbersCopy = Arrays.copyOf(numbers, numbers.length);
        System.out.println("Bản sao: " + Arrays.toString(numbersCopy)); // Output: [1, 2, 4, 5, 8, 9]
        System.out.println("numbers == numbersCopy ? " + (numbers == numbersCopy)); // Output: false (là 2 đối tượng khác nhau)
        System.out.println("Arrays.equals(numbers, numbersCopy) ? " + Arrays.equals(numbers, numbersCopy)); // Output: true (nội dung giống nhau)


        // Gán giá trị cho tất cả phần tử
        int[] filledArray = new int[5];
        Arrays.fill(filledArray, -1);
        System.out.println("Mảng được fill: " + Arrays.toString(filledArray)); // Output: [-1, -1, -1, -1, -1]
    }
}
```

### 2.7. So sánh Mảng với `ArrayList` (Collections Framework)

Mặc dù mảng rất hữu ích, nhưng nhược điểm lớn nhất là kích thước cố định. Khi bạn cần một cấu trúc dữ liệu có thể thay đổi kích thước linh hoạt trong quá trình chạy, **`ArrayList`** (từ `java.util` package, thuộc Java Collections Framework) thường là lựa chọn tốt hơn.

| Đặc điểm             | Mảng (Array)                                                                               | `ArrayList<E>`                                                                                                                                               |
| :------------------- | :----------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Kích thước**       | Cố định (Fixed size) sau khi khởi tạo                                                      | Động (Dynamic size), có thể thêm/xóa phần tử                                                                                                                 |
| **Kiểu dữ liệu**     | Có thể lưu kiểu nguyên thủy (`int`, `double`...) và kiểu đối tượng (`String`, `Object`...) | Chỉ lưu trữ kiểu **Đối tượng** (Object). Để lưu kiểu nguyên thủy, phải dùng các lớp Wrapper tương ứng (`Integer`, `Double`...) - xảy ra autoboxing/unboxing. |
| **Khai báo**         | `type[] name;` hoặc `type name[];`                                                         | `ArrayList<ElementType> name = new ArrayList<>();`                                                                                                           |
| **Thêm phần tử**     | Không có phương thức trực tiếp (phải tạo mảng mới lớn hơn và sao chép)                     | `add(element)`, `add(index, element)`                                                                                                                        |
| **Xóa phần tử**      | Không có phương thức trực tiếp (phải tạo mảng mới nhỏ hơn và sao chép)                     | `remove(index)`, `remove(object)`                                                                                                                            |
| **Lấy kích thước**   | Thuộc tính `length`                                                                        | Phương thức `size()`                                                                                                                                         |
| **Truy cập phần tử** | `array[index]` (Nhanh, O(1))                                                               | `get(index)` (Nhanh, thường là O(1))                                                                                                                         |
| **Thay đổi phần tử** | `array[index] = value;`                                                                    | `set(index, value)`                                                                                                                                          |
| **Hiệu năng**        | Thường nhanh hơn một chút cho truy cập trực tiếp do cấu trúc đơn giản hơn                  | Hơi chậm hơn mảng do có thêm lớp trừu tượng, đặc biệt khi thay đổi kích thước (có thể cần cấp phát lại bộ nhớ và sao chép)                                   |
| **API tích hợp**     | Cơ bản, có hỗ trợ từ `java.util.Arrays`                                                    | Rất nhiều phương thức tiện ích trong Collections Framework                                                                                                   |

**Khi nào nên chọn Mảng?**
* Khi bạn biết chắc chắn số lượng phần tử cần lưu trữ và nó không thay đổi.
* Khi bạn cần hiệu năng tối ưu nhất cho việc truy cập phần tử (ví dụ: trong các thuật toán tính toán số học nặng).
* Khi bạn cần lưu trữ trực tiếp các kiểu dữ liệu nguyên thủy mà không muốn overhead của việc boxing/unboxing.
* Khi làm việc với các API cũ hoặc thư viện yêu cầu đầu vào/đầu ra là mảng.

**Khi nào nên chọn `ArrayList`?**
* Khi bạn không biết trước số lượng phần tử hoặc số lượng phần tử có thể thay đổi thường xuyên.
* Khi bạn cần thường xuyên thêm hoặc xóa phần tử.
* Khi bạn muốn tận dụng các phương thức tiện ích mạnh mẽ của Collections Framework.
* Khi bạn chỉ làm việc với các đối tượng.

## 3. Phương thức (Methods) trong Java

### 3.1. Khái niệm và Vai trò

**Phương thức (Method)** trong Java là một khối mã được đặt tên, chứa một tập hợp các câu lệnh được thiết kế để thực hiện một tác vụ cụ thể. Phương thức là thành phần cơ bản để tổ chức và cấu trúc mã trong các lớp Java. Trong các ngôn ngữ lập trình khác, khái niệm tương tự có thể được gọi là *hàm (function)* hoặc *thủ tục (procedure)*.

**Tại sao cần Phương thức?**
* **Tái sử dụng mã (Code Reusability):** Viết một lần, gọi nhiều lần. Thay vì lặp lại cùng một đoạn mã ở nhiều nơi, bạn đóng gói nó vào một phương thức và gọi tên phương thức đó khi cần.
* **Module hóa (Modularity):** Chia một chương trình lớn thành các đơn vị nhỏ hơn, dễ quản lý hơn. Mỗi phương thức thực hiện một nhiệm vụ rõ ràng.
* **Trừu tượng hóa (Abstraction):** Che giấu chi tiết cài đặt bên trong. Người gọi phương thức chỉ cần biết phương thức làm gì (thông qua tên và tham số) mà không cần quan tâm nó làm như thế nào.
* **Dễ đọc và Bảo trì (Readability & Maintainability):** Mã được chia thành các phương thức có tên gọi ý nghĩa sẽ dễ đọc, dễ hiểu và dễ sửa lỗi hoặc nâng cấp hơn.

### 3.2. Định nghĩa Phương thức

Cấu trúc đầy đủ của một định nghĩa phương thức trong Java như sau:

```java
modifiers returnType methodName(parameterList) throws ExceptionList {
    // Method body (thân phương thức)
    // Chứa các câu lệnh để thực hiện tác vụ
    // Có thể có câu lệnh 'return'
}
```

Giải thích từng thành phần:

* **`modifiers` (Tùy chọn):** Các từ khóa xác định quyền truy cập và các thuộc tính khác của phương thức. Một số modifiers phổ biến:
    * **Quyền truy cập:** `public`, `private`, `protected`, (mặc định - package-private). Xác định ai có thể gọi phương thức này.
        * `public`: Gọi được từ bất kỳ đâu.
        * `private`: Chỉ gọi được từ bên trong cùng một lớp.
        * `protected`: Gọi được từ bên trong cùng lớp, các lớp con, và các lớp trong cùng package.
        * (Mặc định): Gọi được từ các lớp trong cùng package.
    * **`static`:** Cho biết phương thức thuộc về lớp chứ không phải một đối tượng cụ thể. Có thể gọi trực tiếp bằng tên lớp mà không cần tạo đối tượng (`ClassName.methodName()`). Phương thức `main` là một ví dụ.
    * **`final`:** Nếu áp dụng cho phương thức, nó không thể bị ghi đè (override) bởi lớp con.
    * **`abstract`:** Khai báo phương thức không có thân hàm (body). Phải được cài đặt (implement) bởi lớp con không trừu tượng. Chỉ dùng trong lớp trừu tượng (abstract class) hoặc interface.
    * `synchronized`: Dùng trong lập trình đa luồng để đảm bảo chỉ một luồng có thể thực thi phương thức tại một thời điểm.
* **`returnType`:** Kiểu dữ liệu của giá trị mà phương thức sẽ trả về sau khi thực thi xong.
    * Nếu phương thức không trả về giá trị nào, sử dụng từ khóa `void`.
    * Nếu có trả về, phải là một kiểu dữ liệu hợp lệ (nguyên thủy hoặc đối tượng).
* **`methodName`:** Tên của phương thức. Nên tuân theo quy tắc đặt tên `camelCase` (bắt đầu bằng chữ thường, các từ tiếp theo viết hoa chữ cái đầu, ví dụ: `calculateSum`, `getUserName`). Tên nên gợi tả về tác vụ mà phương thức thực hiện.
* **`parameterList` (Tùy chọn):** Danh sách các tham số đầu vào mà phương thức cần để thực hiện công việc. Được đặt trong dấu ngoặc đơn `()`.
    * Mỗi tham số bao gồm `kiểu_dữ_liệu tên_tham_số`.
    * Các tham số cách nhau bởi dấu phẩy `,`.
    * Nếu phương thức không cần tham số nào, để trống cặp ngoặc đơn `()`.
* **`throws ExceptionList` (Tùy chọn):** Khai báo các loại ngoại lệ (checked exceptions) mà phương thức có thể ném ra trong quá trình thực thi nhưng không tự xử lý.
* **`Method body`:** Phần thân phương thức, nằm trong cặp dấu ngoặc nhọn `{}`. Chứa các câu lệnh Java thực hiện logic của phương thức.
    * Nếu `returnType` không phải là `void`, thân phương thức *phải* chứa ít nhất một câu lệnh `return` trả về một giá trị có kiểu tương thích với `returnType`.
    * Nếu `returnType` là `void`, câu lệnh `return;` (không có giá trị) có thể được dùng để kết thúc phương thức sớm, hoặc không cần `return` nếu kết thúc ở cuối khối lệnh.

**Ví dụ định nghĩa phương thức:**

```java
// Phương thức tính tổng 2 số nguyên, trả về kiểu int
public static int add(int num1, int num2) {
    int sum = num1 + num2;
    return sum; // Trả về giá trị tổng
}

// Phương thức in lời chào, không trả về giá trị (void), nhận 1 tham số String
public static void printGreeting(String name) {
    if (name == null || name.isEmpty()) {
        System.out.println("Hello there!");
        return; // Kết thúc sớm nếu tên không hợp lệ
    }
    System.out.println("Hello, " + name + "!");
    // Không cần return ở đây vì là cuối phương thức void
}

// Phương thức kiểm tra số chẵn, trả về boolean
private boolean isEven(int number) {
    return number % 2 == 0;
}
```

**Method Signature (Chữ ký phương thức):** Bao gồm **tên phương thức** và **kiểu dữ liệu của các tham số theo đúng thứ tự**. Chữ ký phương thức dùng để phân biệt các phương thức với nhau, đặc biệt quan trọng trong Nạp chồng phương thức (Method Overloading). Kiểu trả về *không* phải là một phần của chữ ký phương thức.

Ví dụ chữ ký của các phương thức trên:
* `add(int, int)`
* `printGreeting(String)`
* `isEven(int)`

### 3.3. Gọi Phương thức (Method Invocation)

Để thực thi mã bên trong một phương thức, bạn cần **gọi (call hoặc invoke)** nó.

**Cú pháp gọi:**

* **Đối với phương thức `static` (phương thức của lớp):**
    `TênLớp.tênPhươngThức(đốiSố1, đốiSố2, ...);`
    Hoặc nếu gọi từ bên trong cùng lớp: `tênPhươngThức(đốiSố1, đốiSố2, ...);`
* **Đối với phương thức non-static (phương thức của đối tượng):**
    Cần tạo đối tượng trước: `TênLớp tênĐốiTượng = new TênLớp();`
    Sau đó gọi: `tênĐốiTượng.tênPhươngThức(đốiSố1, đốiSố2, ...);`

**Truyền đối số (Arguments):**
Khi gọi phương thức, bạn cần cung cấp các giá trị cụ thể cho các tham số đã định nghĩa. Các giá trị này được gọi là **đối số (arguments)**. Số lượng, kiểu dữ liệu và thứ tự của đối số phải khớp với danh sách tham số của phương thức.

**Xử lý giá trị trả về:**
Nếu phương thức có kiểu trả về khác `void`, nó sẽ trả về một giá trị sau khi thực thi. Bạn có thể:
* Gán giá trị trả về cho một biến có kiểu tương ứng.
* Sử dụng trực tiếp giá trị trả về trong một biểu thức khác.
* In trực tiếp giá trị trả về.
* Bỏ qua giá trị trả về (nhưng thường thì bạn gọi phương thức có giá trị trả về là để sử dụng giá trị đó).

**Ví dụ gọi phương thức:**

```java
public class MethodCallDemo {

    public static int add(int a, int b) { // Phương thức static
        return a + b;
    }

    public void displayMessage(String msg) { // Phương thức non-static
        System.out.println("Message: " + msg);
    }

    public static void main(String[] args) {
        // Gọi phương thức static
        int result = add(5, 3); // Gọi add, gán kết quả cho result
        System.out.println("Tổng là: " + result); // Output: Tổng là: 8
        System.out.println("Tổng khác: " + add(10, 20)); // Gọi add và in trực tiếp kết quả

        // Gọi phương thức non-static
        MethodCallDemo obj = new MethodCallDemo(); // Tạo đối tượng
        obj.displayMessage("Học Java rất thú vị!"); // Gọi displayMessage qua đối tượng

        // Gọi phương thức void
        printGreeting("Alice"); // Giả sử printGreeting được định nghĩa static trong lớp này hoặc lớp khác
                                // (Ví dụ: HelperClass.printGreeting("Alice");)
    }

    public static void printGreeting(String name) { // Thêm định nghĩa cho ví dụ
        System.out.println("Xin chào, " + name + "!");
    }
}
```

**Luồng điều khiển (Control Flow):**
Khi một phương thức được gọi, luồng thực thi của chương trình sẽ tạm dừng tại điểm gọi, chuyển sang thực thi các câu lệnh bên trong phương thức được gọi. Sau khi phương thức hoàn thành (gặp `return` hoặc hết khối lệnh), luồng điều khiển sẽ quay trở lại câu lệnh ngay sau điểm gọi ban đầu, mang theo giá trị trả về (nếu có).

### 3.4. Tham số và Đối số

Cần phân biệt rõ hai khái niệm này:

* **Tham số (Parameter):** Là biến được khai báo trong **định nghĩa** phương thức (phần `parameterList`). Chúng hoạt động như các biến cục bộ bên trong phương thức, nhận giá trị từ các đối số được truyền vào khi phương thức được gọi. Chúng còn được gọi là *tham số hình thức (formal parameters)*.
* **Đối số (Argument):** Là giá trị **thực tế** được truyền vào phương thức khi nó được **gọi**. Chúng còn được gọi là *tham số thực tế (actual parameters)*.

```java
public static void processData(int count, String label) { // count, label là tham số (parameters)
    // ...
}

public static void main(String[] args) {
    int numberOfItems = 10;
    String category = "Electronics";
    processData(numberOfItems, category); // numberOfItems, category là đối số (arguments)
    processData(5, "Books");             // 5, "Books" cũng là đối số
}
```

**Cơ chế truyền tham số trong Java: Truyền giá trị (Pass-by-Value)**

Đây là một điểm *cực kỳ quan trọng* cần hiểu rõ trong Java. Java **luôn luôn** sử dụng cơ chế truyền giá trị (pass-by-value). Điều này có nghĩa là:

1.  **Đối với kiểu dữ liệu nguyên thủy (`int`, `float`, `boolean`, ...):**
    Khi bạn truyền một biến kiểu nguyên thủy làm đối số, **một bản sao của giá trị** của biến đó sẽ được tạo ra và gán cho tham số tương ứng trong phương thức. Mọi thay đổi đối với tham số bên trong phương thức sẽ **không** ảnh hưởng đến biến gốc bên ngoài.

    ```java
    public static void modifyPrimitive(int value) {
        System.out.println("Bên trong modifyPrimitive (trước): " + value); // 5
        value = 100; // Thay đổi bản sao
        System.out.println("Bên trong modifyPrimitive (sau): " + value);  // 100
    }

    public static void main(String[] args) {
        int originalValue = 5;
        System.out.println("Bên ngoài (trước): " + originalValue); // 5
        modifyPrimitive(originalValue);
        System.out.println("Bên ngoài (sau): " + originalValue);  // 5 (Không thay đổi!)
    }
    ```

2.  **Đối với kiểu dữ liệu tham chiếu (Đối tượng, bao gồm Mảng):**
    Khi bạn truyền một biến tham chiếu (ví dụ: biến mảng, biến đối tượng) làm đối số, **một bản sao của giá trị tham chiếu (địa chỉ bộ nhớ)** sẽ được tạo ra và gán cho tham số tương ứng trong phương thức. Cả biến gốc bên ngoài và tham số bên trong phương thức bây giờ đều **trỏ đến cùng một đối tượng** trên Heap.
    * Nếu bạn thay đổi **trạng thái (nội dung) của đối tượng** thông qua tham số bên trong phương thức (ví dụ: thay đổi phần tử mảng, thay đổi thuộc tính đối tượng), thay đổi đó sẽ **ảnh hưởng** đến đối tượng gốc, vì cả hai tham chiếu đều trỏ đến nó.
    * Tuy nhiên, nếu bạn cố gắng **gán một tham chiếu mới** cho tham số bên trong phương thức (ví dụ: `thamSo = new KieuDoiTuong();` hoặc `thamSoMang = new int[10];`), điều này chỉ thay đổi bản sao của tham chiếu bên trong phương thức, làm nó trỏ đến một đối tượng khác. Nó **không** ảnh hưởng đến tham chiếu gốc bên ngoài (biến gốc vẫn trỏ đến đối tượng ban đầu).

    ```java
    import java.util.Arrays;

    public static void modifyArray(int[] arrParam) {
        System.out.println("Bên trong modifyArray (trước): " + Arrays.toString(arrParam)); // [1, 2, 3]
        arrParam[0] = 99; // Thay đổi nội dung của đối tượng mảng mà arrParam trỏ tới
        System.out.println("Bên trong modifyArray (sau thay đổi phần tử): " + Arrays.toString(arrParam)); // [99, 2, 3]

        // Cố gắng gán tham chiếu mới cho tham số -> chỉ thay đổi bản sao tham chiếu arrParam
        arrParam = new int[] {100, 200};
        System.out.println("Bên trong modifyArray (sau khi gán mảng mới): " + Arrays.toString(arrParam)); // [100, 200]
    }

     public static void main(String[] args) {
        int[] originalArray = {1, 2, 3};
        System.out.println("Bên ngoài (trước): " + Arrays.toString(originalArray)); // [1, 2, 3]
        modifyArray(originalArray);
        // originalArray vẫn trỏ đến đối tượng ban đầu, nhưng nội dung đã bị thay đổi bởi arrParam[0]=99
        System.out.println("Bên ngoài (sau): " + Arrays.toString(originalArray)); // [99, 2, 3] (Bị ảnh hưởng bởi thay đổi nội dung)
                                                                                  // (Không bị ảnh hưởng bởi việc gán mảng mới bên trong)
    }
    ```

Hiểu rõ pass-by-value, đặc biệt là với kiểu tham chiếu, là chìa khóa để tránh các lỗi logic khó chịu liên quan đến việc thay đổi trạng thái dữ liệu ngoài ý muốn.

### 3.5. Nạp chồng Phương thức (Method Overloading)

Nạp chồng phương thức là một tính năng trong Java cho phép bạn định nghĩa **nhiều phương thức có cùng tên** trong cùng một lớp, miễn là chúng có **danh sách tham số khác nhau**. Sự khác biệt này có thể là:
* Khác nhau về **số lượng** tham số.
* Khác nhau về **kiểu dữ liệu** của tham số.
* Khác nhau về **thứ tự** kiểu dữ liệu của tham số.

**Lưu ý:** Kiểu trả về (`returnType`) hoặc các `modifiers` khác **không** được sử dụng để phân biệt các phương thức nạp chồng. Trình biên dịch Java xác định phương thức nào sẽ được gọi dựa trên **chữ ký phương thức** (tên + danh sách kiểu tham số) tại thời điểm biên dịch.

**Lợi ích:**
* Tăng tính dễ đọc và dễ sử dụng của API: Cung cấp nhiều cách linh hoạt để gọi cùng một loại tác vụ với các loại đầu vào khác nhau, thay vì phải nhớ nhiều tên phương thức khác nhau. Ví dụ: phương thức `println()` của `System.out` được nạp chồng để nhận nhiều kiểu dữ liệu khác nhau (`int`, `double`, `String`, `Object`, ...).

**Ví dụ:**

```java
public class OverloadingDemo {

    // Nạp chồng phương thức add
    public static int add(int a, int b) {
        System.out.println("Gọi add(int, int)");
        return a + b;
    }

    public static double add(double a, double b) {
        System.out.println("Gọi add(double, double)");
        return a + b;
    }

    public static int add(int a, int b, int c) {
        System.out.println("Gọi add(int, int, int)");
        return a + b + c;
    }

    // Thứ tự kiểu dữ liệu khác nhau
    public static void process(int id, String name) {
         System.out.println("Gọi process(int, String): ID=" + id + ", Name=" + name);
    }

    public static void process(String name, int id) {
         System.out.println("Gọi process(String, int): Name=" + name + ", ID=" + id);
    }

    // public static String add(int a, int b) { // Lỗi! Không thể nạp chồng chỉ dựa trên kiểu trả về
    //     return "Sum: " + (a + b);
    // }

    public static void main(String[] args) {
        add(5, 10);          // Gọi add(int, int)
        add(3.5, 2.5);       // Gọi add(double, double)
        add(1, 2, 3);        // Gọi add(int, int, int)
        process(101, "Bob");   // Gọi process(int, String)
        process("Alice", 202); // Gọi process(String, int)
    }
}
```

### 3.6. Phạm vi Biến (Variable Scope)

Phạm vi (scope) của một biến xác định nơi mà biến đó có thể được truy cập trong mã nguồn.

* **Biến cục bộ (Local Variables):**
    * Được khai báo **bên trong** một phương thức (hoặc một khối lệnh `{}` nhỏ hơn như trong vòng lặp `for`, `if`).
    * Chỉ có thể được truy cập từ **điểm khai báo** cho đến **hết khối lệnh** chứa nó.
    * Không có giá trị mặc định, phải được khởi tạo giá trị trước khi sử dụng lần đầu.
    * Biến cục bộ được tạo ra khi luồng thực thi đi vào khối lệnh chứa nó và bị hủy khi luồng thực thi thoát ra khỏi khối lệnh đó (lưu trên vùng nhớ Stack).
    * **Tham số phương thức** cũng được coi là biến cục bộ của phương thức đó.

    ```java
    public void myMethod(int param) { // param là biến cục bộ (tham số)
        int localVar = 10; // localVar là biến cục bộ

        if (param > 5) {
            String message = "Greater than 5"; // message là biến cục bộ trong khối if
            System.out.println(message);
            System.out.println(localVar); // OK
        }
        // System.out.println(message); // Lỗi! message không tồn tại ngoài khối if

        System.out.println(localVar); // OK
        System.out.println(param);   // OK
    } // localVar và param bị hủy khi phương thức kết thúc
    ```

* **Biến instance (Instance Variables) / Thuộc tính (Fields):**
    * Được khai báo **bên trong một lớp** nhưng **bên ngoài** bất kỳ phương thức nào.
    * Mỗi **đối tượng (instance)** của lớp sẽ có một bản sao riêng của biến này.
    * Có thể được truy cập từ bất kỳ phương thức non-static nào trong cùng lớp. Quyền truy cập từ bên ngoài phụ thuộc vào `modifier` (public, private...).
    * Có giá trị mặc định (giống như phần tử mảng).
    * Tồn tại cùng với vòng đời của đối tượng (lưu trên vùng nhớ Heap).
* **Biến lớp (Class Variables) / Biến static (Static Variables):**
    * Được khai báo giống biến instance nhưng có thêm từ khóa `static`.
    * Chỉ có **một bản sao duy nhất** của biến này cho **toàn bộ lớp**, bất kể có bao nhiêu đối tượng được tạo ra.
    * Có thể được truy cập từ cả phương thức static và non-static trong cùng lớp (thường truy cập bằng `TênLớp.tênBiếnStatic`).
    * Có giá trị mặc định.
    * Tồn tại suốt vòng đời của chương trình (thường lưu trong vùng nhớ đặc biệt cho dữ liệu static).

Phạm vi biến giúp ngăn ngừa xung đột tên và quản lý bộ nhớ hiệu quả.

### 3.7. Đệ quy (Recursion)

**Đệ quy** là một kỹ thuật lập trình trong đó một phương thức **tự gọi lại chính nó** để giải quyết một vấn đề. Phương pháp này thường được áp dụng cho các bài toán có thể được chia nhỏ thành các bài toán con tương tự nhưng với kích thước nhỏ hơn.

Một phương thức đệ quy cần có hai thành phần chính:

1.  **Trường hợp cơ sở (Base Case):** Một hoặc nhiều điều kiện dừng mà tại đó phương thức không gọi lại chính nó nữa, mà trả về một giá trị cụ thể. Đây là điều kiện **bắt buộc** để tránh vòng lặp vô hạn.
2.  **Bước đệ quy (Recursive Step):** Phương thức gọi lại chính nó với một đầu vào đã được thay đổi (thường là nhỏ hơn hoặc tiến gần hơn đến trường hợp cơ sở) và kết hợp kết quả trả về từ lời gọi đệ quy đó để giải quyết vấn đề ban đầu.

**Ví dụ: Tính giai thừa (n!)**
Giai thừa của n (n!) được định nghĩa là:
* `n! = n * (n-1) * (n-2) * ... * 1`
* `0! = 1` (Trường hợp cơ sở)
* `n! = n * (n-1)!` (Bước đệ quy)

```java
public class RecursionDemo {

    public static long factorial(int n) {
        // Trường hợp cơ sở
        if (n < 0) {
            throw new IllegalArgumentException("Số âm không có giai thừa");
        }
        if (n == 0 || n == 1) {
            return 1;
        }
        // Bước đệ quy
        else {
            return n * factorial(n - 1); // Gọi lại chính nó với n-1
        }
    }

    public static void main(String[] args) {
        System.out.println("5! = " + factorial(5)); // Output: 120
        System.out.println("0! = " + factorial(0)); // Output: 1
        // System.out.println("factorial(-2): " + factorial(-2)); // Ném IllegalArgumentException
    }
}
```

**Ưu điểm của Đệ quy:**
* Thường cho mã nguồn ngắn gọn, thanh lịch và gần gũi với định nghĩa toán học của vấn đề.
* Hữu ích cho các cấu trúc dữ liệu dạng cây (tree) hoặc các bài toán chia để trị (divide and conquer).

**Nhược điểm của Đệ quy:**
* **Hiệu năng:** Mỗi lần gọi đệ quy sẽ tạo một frame mới trên call stack, tốn bộ nhớ và thời gian hơn so với vòng lặp tương đương.
* **Nguy cơ `StackOverflowError`:** Nếu đệ quy quá sâu (quá nhiều lần gọi lồng nhau mà chưa đạt đến trường hợp cơ sở) hoặc thiếu trường hợp cơ sở, call stack có thể bị đầy và gây lỗi chương trình.
* Đôi khi khó theo dõi và gỡ lỗi hơn vòng lặp.

Nhiều bài toán giải bằng đệ quy cũng có thể giải bằng vòng lặp (iterative approach). Lựa chọn giữa đệ quy và vòng lặp phụ thuộc vào tính chất bài toán và sự cân nhắc giữa tính rõ ràng của mã và hiệu năng.

### 3.8. Phương thức `main`

Phương thức `public static void main(String[] args)` là một phương thức rất đặc biệt trong Java. Nó đóng vai trò là **điểm khởi đầu (entry point)** cho bất kỳ ứng dụng Java độc lập nào.

* **`public`:** Để Máy ảo Java (JVM) có thể gọi được phương thức này từ bên ngoài lớp.
* **`static`:** Để JVM có thể gọi phương thức này mà không cần phải tạo đối tượng của lớp chứa nó. Chương trình cần một điểm bắt đầu trước khi bất kỳ đối tượng nào được tạo.
* **`void`:** Phương thức `main` không trả về giá trị nào cho JVM khi nó kết thúc.
* **`main`:** Đây là tên quy ước mà JVM tìm kiếm để bắt đầu thực thi.
* **`String[] args`:** Đây là một tham số bắt buộc. Nó là một mảng các đối tượng `String`, dùng để nhận các đối số được truyền vào chương trình từ dòng lệnh (command line) khi chạy ứng dụng.

Khi bạn chạy một chương trình Java, JVM sẽ tìm lớp có chứa phương thức `main` và bắt đầu thực thi các câu lệnh từ bên trong phương thức này.

```java
public class MainDemo {
    public static void main(String[] args) { // Điểm bắt đầu
        System.out.println("Chương trình bắt đầu!");
        if (args.length > 0) {
            System.out.println("Các đối số dòng lệnh:");
            for (String arg : args) {
                System.out.println("- " + arg);
            }
        } else {
            System.out.println("Không có đối số dòng lệnh nào được cung cấp.");
        }
        System.out.println("Chương trình kết thúc.");
    }
}
// Cách chạy với đối số dòng lệnh:
// javac MainDemo.java
// java MainDemo arg1 "argument 2" 123
```

## 4. Thực tiễn Tốt nhất và Gỡ lỗi (Best Practices & Troubleshooting)

### 4.1. Với Mảng

* **Luôn kiểm tra giới hạn chỉ số:** Trước khi truy cập `array[i]`, đặc biệt trong vòng lặp hoặc khi chỉ số được tính toán, hãy đảm bảo `0 <= i < array.length` để tránh `ArrayIndexOutOfBoundsException`.
* **Sử dụng `java.util.Arrays`:** Tận dụng các phương thức tiện ích như `sort()`, `binarySearch()`, `copyOf()`, `toString()` thay vì tự viết lại các chức năng cơ bản này. `Arrays.toString()` cực kỳ hữu ích khi gỡ lỗi.
* **Cân nhắc `ArrayList`:** Nếu bạn không chắc về kích thước hoặc cần thay đổi kích thước thường xuyên, hãy sử dụng `ArrayList` hoặc các cấu trúc dữ liệu khác từ Collections Framework.
* **Tránh "Magic Numbers":** Thay vì dùng trực tiếp các số cụ thể làm kích thước mảng (ví dụ: `new int[100]`), hãy sử dụng các hằng số có tên gọi ý nghĩa (`final static int MAX_USERS = 100; ... new User[MAX_USERS];`) để mã dễ đọc và dễ thay đổi hơn.
* **Cẩn thận với tham chiếu:** Nhớ rằng gán một biến mảng cho biến khác (`arrayB = arrayA;`) chỉ là sao chép tham chiếu, cả hai biến cùng trỏ đến một đối tượng mảng. Sử dụng `Arrays.copyOf()` hoặc vòng lặp để tạo bản sao sâu (deep copy) nếu cần đối tượng mảng độc lập.

### 4.2. Với Phương thức

* **Nguyên tắc trách nhiệm đơn lẻ (Single Responsibility Principle - SRP):** Mỗi phương thức nên thực hiện tốt **một** nhiệm vụ cụ thể và duy nhất. Tránh các phương thức "làm tất cả". Phương thức ngắn gọn, tập trung thường dễ hiểu, dễ kiểm thử và dễ tái sử dụng hơn.
* **Đặt tên rõ ràng, có ý nghĩa:** Tên phương thức nên là động từ hoặc cụm động từ mô tả hành động mà nó thực hiện (ví dụ: `calculateTotal`, `getUserById`, `validateInput`).
* **Hạn chế số lượng tham số:** Phương thức có quá nhiều tham số (thường > 3 hoặc 4) trở nên khó gọi và khó hiểu. Cân nhắc:
    * Nhóm các tham số liên quan vào một đối tượng riêng (Parameter Object).
    * Chia phương thức thành các phương thức nhỏ hơn.
* **Sử dụng `final` cho tham số (nếu phù hợp):** Đánh dấu tham số là `final` (`void process(final int id, final String data)`) nếu bạn không có ý định gán lại giá trị mới cho tham số đó bên trong phương thức. Điều này có thể giúp tránh lỗi và làm rõ ý định, đặc biệt hữu ích với tham số kiểu tham chiếu để tránh vô tình gán lại tham chiếu.
* **Viết tài liệu (Javadoc):** Đối với các phương thức (đặc biệt là `public`), hãy viết σχόλια Javadoc (`/** ... */`) để mô tả mục đích, các tham số (`@param`), giá trị trả về (`@return`), và các ngoại lệ có thể ném ra (`@throws`). Điều này giúp người khác (và chính bạn trong tương lai) hiểu cách sử dụng phương thức mà không cần đọc mã nguồn chi tiết.
* **Xử lý ngoại lệ một cách nhất quán:** Quyết định xem phương thức nên tự xử lý ngoại lệ (dùng `try-catch`) hay nên ném ra cho nơi gọi xử lý (dùng `throws`).

### 4.3. Lỗi thường gặp và Cách khắc phục

* **`NullPointerException` (NPE):**
    * **Nguyên nhân:** Cố gắng truy cập thuộc tính hoặc gọi phương thức trên một biến tham chiếu đang có giá trị `null` (chưa được khởi tạo hoặc đã bị gán `null`). Thường gặp với biến mảng hoặc biến đối tượng chưa `new`.
    * **Khắc phục:** Luôn đảm bảo biến tham chiếu đã được khởi tạo (trỏ đến một đối tượng hợp lệ) trước khi sử dụng. Kiểm tra `null` trước khi truy cập (`if (myArray != null && ...)`).
* **`ArrayIndexOutOfBoundsException`:**
    * **Nguyên nhân:** Truy cập mảng với chỉ số nằm ngoài phạm vi `[0, length - 1]`.
    * **Khắc phục:** Kiểm tra lại logic vòng lặp, các phép tính chỉ số. Đảm bảo chỉ số luôn nằm trong giới hạn hợp lệ, đặc biệt là các trường hợp biên (phần tử đầu, phần tử cuối).
* **Lỗi logic trong thuật toán:**
    * **Nguyên nhân:** Thuật toán duyệt, tìm kiếm, sắp xếp hoặc xử lý dữ liệu trong mảng/phương thức bị sai.
    * **Khắc phục:** Sử dụng trình gỡ lỗi (debugger) để theo dõi giá trị biến từng bước. In ra các giá trị trung gian (`System.out.println` hoặc logging framework). Viết các bài kiểm thử đơn vị (Unit Tests) để xác minh tính đúng đắn của phương thức với các đầu vào khác nhau.
* **Sai kiểu dữ liệu:**
    * **Nguyên nhân:** Gán giá trị không tương thích cho phần tử mảng, trả về sai kiểu từ phương thức, hoặc truyền sai kiểu đối số khi gọi phương thức.
    * **Khắc phục:** Kiểm tra kỹ kiểu dữ liệu khi khai báo, gán giá trị, định nghĩa và gọi phương thức. Trình biên dịch thường sẽ báo lỗi này.
* **`StackOverflowError` (với đệ quy):**
    * **Nguyên nhân:** Phương thức đệ quy thiếu trường hợp cơ sở (điều kiện dừng) hoặc trường hợp cơ sở không bao giờ đạt được, dẫn đến việc gọi lại chính nó vô hạn lần, làm đầy bộ nhớ stack. Hoặc bài toán yêu cầu độ sâu đệ quy quá lớn so với giới hạn của stack.
    * **Khắc phục:** Đảm bảo có ít nhất một trường hợp cơ sở rõ ràng và nó chắc chắn sẽ đạt được sau một số hữu hạn bước đệ quy. Xem xét chuyển sang giải pháp dùng vòng lặp nếu đệ quy quá sâu.

## 5. Kết luận

Mảng và Phương thức là những viên gạch nền tảng không thể thiếu trong lập trình Java. Việc nắm vững cách khai báo, khởi tạo, sử dụng mảng, kết hợp với khả năng thiết kế và gọi các phương thức một cách hiệu quả sẽ giúp bạn xây dựng các ứng dụng có cấu trúc tốt, dễ bảo trì và mạnh mẽ.

Hãy nhớ rằng mảng cung cấp hiệu năng cao cho việc lưu trữ và truy cập dữ liệu có kích thước cố định, trong khi `ArrayList` linh hoạt hơn cho dữ liệu có kích thước động. Phương thức giúp bạn tổ chức mã, tái sử dụng logic và xây dựng các chương trình phức tạp từ những thành phần đơn giản hơn.