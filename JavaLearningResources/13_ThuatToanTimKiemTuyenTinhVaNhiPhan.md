# THUẬT TOÁN TÌM KIẾM TUYẾN TÍNH VÀ NHỊ PHÂN

## **1. Giới thiệu**

Tìm kiếm dữ liệu là một trong những thao tác nền tảng và được thực hiện thường xuyên nhất trong khoa học máy tính, đặc biệt là trong các hệ thống quản lý và lưu trữ thông tin. Khi khối lượng dữ liệu ngày càng lớn, việc lựa chọn và triển khai một thuật toán tìm kiếm hiệu quả trở nên cực kỳ quan trọng để đảm bảo hiệu năng của ứng dụng.

Mục tiêu cơ bản của một thuật toán tìm kiếm là xác định vị trí (hoặc sự tồn tại) của một phần tử cụ thể (thường được gọi là **khóa tìm kiếm** - `search key`) trong một tập hợp dữ liệu (như mảng, danh sách, cây, v.v.).

Hướng dẫn này sẽ tập trung phân tích sâu hai thuật toán tìm kiếm cơ bản và phổ biến nhất trên các cấu trúc dữ liệu dạng danh sách hoặc mảng:

* **Tìm kiếm Tuyến tính (Linear Search)**: Một phương pháp đơn giản, duyệt tuần tự qua từng phần tử. Thường được áp dụng cho dữ liệu chưa sắp xếp.
* **Tìm kiếm Nhị phân (Binary Search)**: Một phương pháp hiệu quả hơn đáng kể, nhưng yêu cầu dữ liệu đầu vào phải được sắp xếp.

Chúng ta sẽ đi sâu vào nguyên tắc hoạt động, cách cài đặt, phân tích hiệu năng, so sánh ưu nhược điểm, và các ứng dụng thực tế của từng thuật toán.

## **2. Tìm kiếm Tuyến tính (Linear Search)**

Tìm kiếm tuyến tính, hay còn gọi là **tìm kiếm tuần tự (Sequential Search)**, là thuật toán tìm kiếm đơn giản nhất.

### **2.1. Nguyên tắc hoạt động**

Ý tưởng cốt lõi của tìm kiếm tuyến tính là duyệt qua lần lượt từng phần tử trong tập dữ liệu (ví dụ: mảng hoặc danh sách) từ đầu đến cuối. Tại mỗi phần tử, thuật toán so sánh giá trị của phần tử đó với khóa tìm kiếm (`targetValue`):

* Nếu tìm thấy một phần tử có giá trị bằng với khóa tìm kiếm, thuật toán sẽ dừng lại và trả về vị trí (**chỉ số** - `index`) của phần tử đó.
* Nếu duyệt hết toàn bộ tập dữ liệu mà không tìm thấy phần tử nào khớp với khóa tìm kiếm, thuật toán sẽ kết luận rằng khóa tìm kiếm không tồn tại trong tập dữ liệu và thường trả về một giá trị đặc biệt (ví dụ: `-1`) để báo hiệu.

### **2.2. Thuật toán (Pseudocode)**

```plaintext
FUNCTION linearSearch(list, targetValue):
  FOR i FROM 0 TO length(list) - 1:
    IF list[i] == targetValue:
      RETURN i  // Trả về chỉ số nếu tìm thấy
    END IF
  END FOR
  RETURN -1 // Trả về -1 nếu không tìm thấy
END FUNCTION
```

### **2.3. Ví dụ Minh họa**

Xét mảng `data = [10, 14, 19, 26, 27, 31, 33, 35, 42, 44]` và chúng ta muốn tìm phần tử có giá trị `targetValue = 33`.

1.  So sánh `data[0]` (10) với 33. Không khớp.
2.  So sánh `data[1]` (14) với 33. Không khớp.
3.  So sánh `data[2]` (19) với 33. Không khớp.
4.  So sánh `data[3]` (26) với 33. Không khớp.
5.  So sánh `data[4]` (27) với 33. Không khớp.
6.  So sánh `data[5]` (31) với 33. Không khớp.
7.  So sánh `data[6]` (33) với 33. **Khớp!** Trả về chỉ số `6`.

### **2.4. Cài đặt (Java)**

```java
/**
 * Thực hiện tìm kiếm tuyến tính trên một mảng số nguyên.
 *
 * @param arr Mảng đầu vào để tìm kiếm.
 * @param value Giá trị cần tìm.
 * @return Chỉ số của phần tử đầu tiên tìm thấy có giá trị bằng 'value',
 * hoặc -1 nếu không tìm thấy.
 */
public int linearSearch(int[] arr, int value) {
    // Duyệt qua từng phần tử của mảng
    for (int i = 0; i < arr.length; i++) {
        // Nếu phần tử hiện tại bằng giá trị cần tìm
        if (arr[i] == value) {
            return i; // Trả về chỉ số của phần tử đó
        }
    }
    // Nếu duyệt hết mảng mà không tìm thấy, trả về -1
    return -1;
}
```

### **2.5. Phân tích Độ phức tạp**

**Độ phức tạp thời gian** của tìm kiếm tuyến tính được phân tích như sau:

* **Trường hợp tốt nhất (Best Case):** Khóa tìm kiếm nằm ngay ở phần tử đầu tiên của mảng. Chỉ cần 1 phép so sánh. Độ phức tạp là O(1).
* **Trường hợp xấu nhất (Worst Case):** Khóa tìm kiếm nằm ở phần tử cuối cùng, hoặc không có trong mảng. Cần N phép so sánh (với N là số lượng phần tử). Độ phức tạp là O(N).
* **Trường hợp trung bình (Average Case):** Trung bình cần khoảng N/2 phép so sánh. Độ phức tạp vẫn là O(N).

**Độ phức tạp không gian (Space Complexity)** của tìm kiếm tuyến tính là O(1) vì nó chỉ cần một lượng bộ nhớ cố định, không phụ thuộc vào kích thước đầu vào.

### **2.6. Ưu và Nhược điểm**

**Ưu điểm:**

* **Đơn giản:** Rất dễ hiểu và cài đặt.
* **Không yêu cầu sắp xếp:** Hoạt động tốt trên cả dữ liệu đã sắp xếp và chưa sắp xếp.
* **Linh hoạt:** Có thể áp dụng cho nhiều loại cấu trúc dữ liệu dạng danh sách (mảng, danh sách liên kết).

**Nhược điểm:**

* **Không hiệu quả:** Với các tập dữ liệu lớn, việc phải duyệt qua từng phần tử trở nên rất chậm chạp. Độ phức tạp O(N) là không lý tưởng cho hiệu năng.

### **2.7. Khi nào nên sử dụng?**

Tìm kiếm tuyến tính phù hợp nhất trong các trường hợp sau:

* Tập dữ liệu nhỏ.
* Dữ liệu chưa được sắp xếp và việc sắp xếp lại tốn kém hơn hoặc không khả thi.
* Khi sự đơn giản trong cài đặt được ưu tiên hơn hiệu năng.

## **3. Tìm kiếm Nhị phân (Binary Search)**

Tìm kiếm nhị phân là một thuật toán tìm kiếm hiệu quả hơn nhiều so với tìm kiếm tuyến tính, nhưng nó có một yêu cầu tiên quyết quan trọng: **tập dữ liệu phải được sắp xếp**.

### **3.1. Nguyên tắc hoạt động**

Tìm kiếm nhị phân hoạt động dựa trên chiến lược "**chia để trị**" (`divide and conquer`):

1.  Bắt đầu bằng việc xem xét toàn bộ phạm vi của mảng đã sắp xếp.
2.  Xác định phần tử ở vị trí giữa (`mid`) của phạm vi hiện tại.
3.  So sánh khóa tìm kiếm (`targetValue`) với giá trị của phần tử giữa (`list[mid]`):
    * Nếu `targetValue == list[mid]`: Đã tìm thấy phần tử. Trả về chỉ số `mid`.
    * Nếu `targetValue < list[mid]`: Vì mảng đã được sắp xếp, nếu `targetValue` tồn tại, nó chắc chắn phải nằm trong nửa trái của phạm vi hiện tại (từ đầu đến `mid - 1`). Thuật toán sẽ loại bỏ nửa phải và lặp lại quá trình tìm kiếm trên nửa trái.
    * Nếu `targetValue > list[mid]`: Tương tự, nếu `targetValue` tồn tại, nó phải nằm trong nửa phải của phạm vi hiện tại (từ `mid + 1` đến cuối). Thuật toán sẽ loại bỏ nửa trái và lặp lại quá trình tìm kiếm trên nửa phải.
4.  Quá trình này lặp đi lặp lại, thu hẹp phạm vi tìm kiếm đi một nửa sau mỗi lần lặp, cho đến khi tìm thấy phần tử hoặc phạm vi tìm kiếm trở nên rỗng (không còn phần tử nào để xét), nghĩa là khóa tìm kiếm không có trong mảng.

### **3.2. Thuật toán (Pseudocode - Phiên bản lặp)**

```plaintext
FUNCTION binarySearchIterative(sortedList, targetValue):
  low = 0                      // Chỉ số đầu tiên
  high = length(sortedList) - 1 // Chỉ số cuối cùng

  WHILE low <= high:           // Khi phạm vi tìm kiếm còn hợp lệ
    // Tính chỉ số giữa, tránh tràn số với (low + high) / 2
    mid = low + floor((high - low) / 2)

    IF sortedList[mid] == targetValue:
      RETURN mid             // Tìm thấy, trả về chỉ số
    ELSE IF targetValue < sortedList[mid]:
      high = mid - 1         // Tìm kiếm ở nửa trái
    ELSE: // targetValue > sortedList[mid]
      low = mid + 1          // Tìm kiếm ở nửa phải
    END IF
  END WHILE

  RETURN -1                   // Không tìm thấy
END FUNCTION
```

### **3.3. Ví dụ Minh họa**

Xét mảng đã sắp xếp `data = [6, 13, 14, 25, 33, 43, 51, 53, 64, 72, 84, 93, 95, 96, 97]` và tìm `targetValue = 33`.

* **Lần 1:**
    * `low = 0`, `high = 14`
    * `mid = 0 + (14 - 0) / 2 = 7`
    * `data[7]` là `53`.
    * Vì `33 < 53`, tìm kiếm ở nửa trái: cập nhật `high = mid - 1 = 6`.
* **Lần 2:**
    * `low = 0`, `high = 6`
    * `mid = 0 + (6 - 0) / 2 = 3`
    * `data[3]` là `25`.
    * Vì `33 > 25`, tìm kiếm ở nửa phải: cập nhật `low = mid + 1 = 4`.
* **Lần 3:**
    * `low = 4`, `high = 6`
    * `mid = 4 + (6 - 4) / 2 = 5`
    * `data[5]` là `43`.
    * Vì `33 < 43`, tìm kiếm ở nửa trái: cập nhật `high = mid - 1 = 4`.
* **Lần 4:**
    * `low = 4`, `high = 4`
    * `mid = 4 + (4 - 4) / 2 = 4`
    * `data[4]` là `33`.
    * Vì `33 == 33`, **tìm thấy!** Trả về chỉ số `4`.

### **3.4. Cài đặt (Java)**

#### **3.4.1. Phiên bản Lặp (Iterative)**

```java
/**
 * Thực hiện tìm kiếm nhị phân trên một mảng số nguyên đã sắp xếp (phiên bản lặp).
 *
 * @param list Mảng đã sắp xếp để tìm kiếm.
 * @param key Giá trị cần tìm.
 * @return Chỉ số của phần tử tìm thấy, hoặc -1 nếu không tìm thấy.
 */
public static int binarySearchIterative(int[] list, int key) {
    int low = 0;
    int high = list.length - 1;

    while (high >= low) {
        // Tính mid, tránh tràn số với (low + high) / 2
        int mid = low + (high - low) / 2;

        if (key < list[mid]) {
            high = mid - 1; // Tìm ở nửa trái
        } else if (key == list[mid]) {
            return mid;     // Tìm thấy
        } else { // key > list[mid]
            low = mid + 1;  // Tìm ở nửa phải
        }
    }

    return -1; // Không tìm thấy
}
```

#### **3.4.2. Phiên bản Đệ quy (Recursive)**

```java
/**
 * Thực hiện tìm kiếm nhị phân trên một mảng số nguyên đã sắp xếp (phiên bản đệ quy).
 *
 * @param arr Mảng đã sắp xếp để tìm kiếm.
 * @param low Chỉ số bắt đầu của phạm vi tìm kiếm hiện tại.
 * @param high Chỉ số kết thúc của phạm vi tìm kiếm hiện tại.
 * @param value Giá trị cần tìm.
 * @return Chỉ số của phần tử tìm thấy, hoặc -1 nếu không tìm thấy.
 */
int binarySearchRecursive(int[] arr, int low, int high, int value) {
    if (high >= low) { // Điều kiện dừng: phạm vi còn hợp lệ
        // Tính mid, tránh tràn số
        int mid = low + (high - low) / 2;

        // Nếu phần tử giữa là giá trị cần tìm
        if (arr[mid] == value) {
            return mid;
        }

        // Nếu giá trị cần tìm nhỏ hơn phần tử giữa, tìm ở nửa trái
        if (arr[mid] > value) {
            // Gọi đệ quy cho nửa trái
            return binarySearchRecursive(arr, low, mid - 1, value);
        }

        // Ngược lại (arr[mid] < value), tìm ở nửa phải
        // Gọi đệ quy cho nửa phải
        return binarySearchRecursive(arr, mid + 1, high, value);
    }

    // Nếu high < low, nghĩa là phạm vi không hợp lệ, không tìm thấy
    return -1;
}

/**
 * Hàm gọi ban đầu tiện lợi hơn cho phiên bản đệ quy.
 *
 * @param arr Mảng đã sắp xếp.
 * @param value Giá trị cần tìm.
 * @return Chỉ số của phần tử tìm thấy, hoặc -1 nếu không tìm thấy.
 */
public int search(int[] arr, int value) {
    return binarySearchRecursive(arr, 0, arr.length - 1, value);
}
```

### **3.5. Phân tích Độ phức tạp**

Điểm mạnh lớn nhất của tìm kiếm nhị phân nằm ở **độ phức tạp thời gian** của nó:

* **Trường hợp tốt nhất (Best Case):** Khóa tìm kiếm nằm ngay ở phần tử giữa trong lần so sánh đầu tiên. Độ phức tạp là O(1).
* **Trường hợp xấu nhất (Worst Case) và Trung bình (Average Case):** Sau mỗi lần so sánh, không gian tìm kiếm giảm đi một nửa. Nếu có N phần tử, số lần so sánh tối đa là số lần có thể chia N cho 2 cho đến khi còn 1. Điều này tương đương với logarit cơ số 2 của N. Do đó, độ phức tạp là O(log N).

Đây là một cải thiện vượt trội so với O(N) của tìm kiếm tuyến tính, đặc biệt với các tập dữ liệu lớn. Ví dụ, với 1 triệu phần tử, tìm kiếm tuyến tính có thể cần tới 1 triệu phép so sánh, trong khi tìm kiếm nhị phân chỉ cần khoảng 20 phép so sánh (log_2{1,000,000} \approx 19.9).

**Độ phức tạp không gian (Space Complexity):**

* **Phiên bản lặp:** O(1) (chỉ dùng vài biến tạm).
* **Phiên bản đệ quy:** O(log N) do sử dụng bộ nhớ stack cho các lời gọi đệ quy. Chiều sâu tối đa của stack là O(log N).

### **3.6. Ưu và Nhược điểm**

**Ưu điểm:**

* **Hiệu quả cao:** Cực kỳ nhanh trên các tập dữ liệu lớn đã sắp xếp với độ phức tạp O(log N).
* **Đơn giản (tương đối):** Logic "chia để trị" khá trực quan.

**Nhược điểm:**

* **Yêu cầu sắp xếp:** Bắt buộc phải thực hiện trên dữ liệu đã được sắp xếp. Chi phí cho việc sắp xếp ban đầu (thường là O(N log N)) cần được tính đến nếu dữ liệu chưa được sắp xếp.
* **Chỉ hiệu quả với truy cập ngẫu nhiên:** Hoạt động tốt nhất trên cấu trúc dữ liệu hỗ trợ truy cập nhanh đến phần tử bất kỳ (như mảng). Kém hiệu quả hơn trên danh sách liên kết (`linked list`) vì việc tìm phần tử giữa tốn O(N) thời gian.

### **3.7. Khi nào nên sử dụng?**

Tìm kiếm nhị phân là lựa chọn hàng đầu khi:

* Tập dữ liệu lớn.
* Dữ liệu đã được sắp xếp hoặc chi phí sắp xếp một lần là chấp nhận được so với lợi ích từ việc tìm kiếm nhanh nhiều lần.
* Cấu trúc dữ liệu cho phép truy cập ngẫu nhiên hiệu quả (như mảng).
* Yêu cầu về hiệu năng tìm kiếm là quan trọng.

## **4. Đánh giá Độ phức tạp Thuật toán (Algorithm Complexity)**

Đánh giá độ phức tạp là cách để đo lường và so sánh hiệu quả của các thuật toán, chủ yếu dựa trên thời gian thực thi và không gian bộ nhớ mà chúng yêu cầu khi kích thước đầu vào tăng lên.

### **4.1. Tại sao cần đánh giá?**

* **So sánh thuật toán:** Xác định thuật toán nào tốt hơn, nhanh hơn, hoặc sử dụng ít tài nguyên hơn cho cùng một bài toán.
* **Dự đoán hiệu năng:** Ước lượng thời gian chạy hoặc bộ nhớ cần thiết khi kích thước dữ liệu đầu vào (`n`) tăng lên.
* **Lựa chọn thuật toán:** Đưa ra quyết định sáng suốt về việc sử dụng thuật toán nào phù hợp nhất với yêu cầu cụ thể của ứng dụng.

### **4.2. Khái niệm cơ bản**

* **Kích thước đầu vào (`n`):** Thường là số lượng phần tử trong cấu trúc dữ liệu (ví dụ: số phần tử mảng).
* **Thời gian thực thi (`T(n)`):** Số lượng phép tính cơ bản (so sánh, gán, phép toán số học) mà thuật toán thực hiện, biểu diễn dưới dạng một hàm phụ thuộc vào `n`.
* **Không gian bộ nhớ (`S(n)`):** Lượng bộ nhớ cần thiết (ngoài bộ nhớ lưu trữ đầu vào) mà thuật toán sử dụng, cũng là một hàm của `n`.
* **Các trường hợp phân tích:**
    * **Trường hợp xấu nhất (Worst Case):** Thời gian chạy/bộ nhớ lớn nhất có thể với mọi đầu vào kích thước `n`. Đây là thước đo phổ biến nhất vì nó đảm bảo giới hạn trên về hiệu năng.
    * **Trường hợp tốt nhất (Best Case):** Thời gian chạy/bộ nhớ ít nhất có thể. Ít hữu ích trong thực tế.
    * **Trường hợp trung bình (Average Case):** Thời gian chạy/bộ nhớ trung bình trên tất cả các đầu vào có thể có cùng kích thước `n`. Thường khó tính toán chính xác.

### **4.3. Ký hiệu Big-O**

Ký hiệu **O-lớn (Big-O)** là cách phổ biến nhất để biểu diễn độ phức tạp thời gian hoặc không gian ở trường hợp xấu nhất. Nó mô tả tốc độ tăng trưởng của hàm thời gian/không gian khi kích thước đầu vào `n` tiến đến vô cùng, bỏ qua các hằng số và các số hạng bậc thấp hơn.

Nói T(n) = O(f(n)) có nghĩa là tồn tại các hằng số dương C và n_0 sao cho T(n) \le C \cdot f(n) với mọi n \ge n_0. f(n) là giới hạn trên của tốc độ tăng trưởng.

Các độ phức tạp phổ biến (từ nhanh đến chậm):

* O(1): **Hằng số:** Thời gian thực hiện không đổi khi `n` tăng (ví dụ: truy cập phần tử mảng bằng chỉ số, phép gán đơn giản).
* O(log N): **Logarit:** Thời gian tăng chậm khi `n` tăng (ví dụ: tìm kiếm nhị phân).
* O(N): **Tuyến tính:** Thời gian tăng tỷ lệ thuận với `n` (ví dụ: tìm kiếm tuyến tính, duyệt mảng).
* O(N log N): **Tuyến tính-Logarit:** Nhanh hơn bậc hai (ví dụ: các thuật toán sắp xếp hiệu quả như Merge Sort, Quick Sort).
* O(N^2): **Bậc hai:** Thời gian tăng theo bình phương của `n` (ví dụ: các vòng lặp lồng nhau đơn giản, các thuật toán sắp xếp đơn giản như Bubble Sort, Insertion Sort).
* O(N^3): **Bậc ba:** Ví dụ: nhân ma trận cơ bản.
* O(2^N): **Hàm mũ:** Thời gian tăng rất nhanh, thường không khả thi với `n` lớn (ví dụ: bài toán duyệt tập con).
* O(N!): **Giai thừa:** Thời gian tăng cực kỳ nhanh (ví dụ: bài toán người du lịch sử dụng duyệt toàn bộ).

### **4.4. Quy tắc tính toán Big-O**

* **Quy tắc bỏ hằng số:** O(C \cdot f(n)) = O(f(n)). Ví dụ: O(5N^2) = O(N^2).
* **Quy tắc cộng (cho các khối lệnh nối tiếp):** O(f(n) + g(n)) = O(\max(f(n), g(n))). Giữ lại số hạng tăng nhanh nhất. Ví dụ: O(N^2 + N) = O(N^2).
* **Quy tắc nhân (cho các khối lệnh lồng nhau):** O(f(n) \cdot g(n)).
    * Ví dụ 1: Vòng lặp O(N) chứa vòng lặp O(N) bên trong sẽ có độ phức tạp O(N \cdot N) = O(N^2).
    * Ví dụ 2: Vòng lặp O(N) chứa vòng lặp O(1) (ví dụ: lặp 20 lần) bên trong sẽ có độ phức tạp O(N \cdot 1) = O(N).
* **Lệnh đơn giản:** Các lệnh gán, đọc/ghi, so sánh, phép toán số học cơ bản thường mất O(1) thời gian.
* **Lệnh rẽ nhánh (`if-else`):** Độ phức tạp bằng thời gian kiểm tra điều kiện (thường O(1)) cộng với độ phức tạp của nhánh chạy lâu nhất (worst-case). Ví dụ: `if (condition)` { O(1) } `else` { O(N) } thì độ phức tạp tổng là O(N).

## **5. So sánh Tìm kiếm Tuyến tính và Nhị phân**

| Đặc điểm                   | Tìm kiếm Tuyến tính (Linear Search) | Tìm kiếm Nhị phân (Binary Search)    |
| :------------------------- | :---------------------------------- | :----------------------------------- |
| **Yêu cầu dữ liệu**        | Không cần sắp xếp                   | **Phải được sắp xếp**                |
| **Độ phức tạp (Worst)**    | O(N)                                | O(log N)                             |
| **Độ phức tạp (Best)**     | O(1)                                | O(1)                                 |
| **Độ phức tạp (Avg)**      | O(N)                                | O(log N)                             |
| **Độ phức tạp không gian** | O(1)                                | O(1) (lặp), O(log N) (đệ quy)        |
| **Loại cấu trúc**          | Mảng, Danh sách liên kết,...        | Mảng (hiệu quả nhất), Cây TKNPT, ... |
| **Tốc độ (dữ liệu lớn)**   | Chậm                                | Rất nhanh                            |
| **Độ đơn giản**            | Rất đơn giản                        | Tương đối đơn giản                   |

## **6. Mở rộng và Các Kỹ thuật Liên quan**

* **Tìm kiếm Nội suy (Interpolation Search):** Một biến thể cải tiến của tìm kiếm nhị phân, đặc biệt hiệu quả khi dữ liệu được phân bố đều (`uniformly distributed`). Thay vì luôn chọn điểm giữa, nó ước lượng vị trí của khóa tìm kiếm dựa trên giá trị của khóa đó so với giá trị ở đầu và cuối phạm vi. Trong trường hợp tốt và trung bình với dữ liệu phân bố đều, độ phức tạp có thể đạt O(log log N), nhanh hơn O(log N). Tuy nhiên, trường hợp xấu nhất vẫn là O(N).
* **Bảng băm (Hash Tables):** Cung cấp khả năng tìm kiếm với độ phức tạp trung bình là O(1). Hoạt động bằng cách sử dụng một hàm băm để ánh xạ khóa tìm kiếm trực tiếp đến một vị trí (hoặc một danh sách các vị trí) trong bảng. Nhược điểm là trường hợp xấu nhất có thể lên tới O(N) (khi xảy ra nhiều **xung đột** - `collisions`), cần xử lý xung đột, và không duy trì thứ tự của các phần tử.
* **Cây Tìm kiếm Nhị phân (Binary Search Trees - BST):** Cấu trúc dữ liệu dạng cây cho phép tìm kiếm, thêm, xóa phần tử với độ phức tạp trung bình là O(log N) (nếu cây cân bằng). Các biến thể như cây AVL, cây Đỏ-Đen đảm bảo cây luôn cân bằng, giữ độ phức tạp O(log N) ở trường hợp xấu nhất.
* **Tìm kiếm trên Chuỗi (String Searching):** Các thuật toán chuyên biệt như Knuth-Morris-Pratt (KMP), Boyer-Moore, Rabin-Karp được sử dụng để tìm kiếm sự xuất hiện của một chuỗi con (`pattern`) trong một chuỗi văn bản lớn (`text`) một cách hiệu quả.

## **7. Thực tiễn và Tối ưu hóa**

* **Tầm quan trọng của dữ liệu đã sắp xếp:** Luôn nhớ rằng Binary Search chỉ hoạt động chính xác trên dữ liệu đã sắp xếp. Nếu dữ liệu chưa được sắp xếp, bạn cần sắp xếp nó trước (ví dụ bằng `Arrays.sort()` trong Java hoặc `list.sort()`/`sorted()` trong Python), hoặc sử dụng Linear Search.
* **Chọn đúng thuật toán:**
    * Dữ liệu nhỏ, không sắp xếp, hoặc chỉ tìm kiếm một lần: Linear Search có thể đủ tốt và dễ cài đặt.
    * Dữ liệu lớn, đã sắp xếp, cần tìm kiếm nhiều lần: Binary Search là lựa chọn vượt trội về hiệu năng.
    * Cần tìm kiếm cực nhanh (trung bình O(1)) và không quan tâm thứ tự: Cân nhắc Hash Table.
* **Sử dụng thư viện chuẩn:** Hầu hết các ngôn ngữ lập trình đều cung cấp các hàm tìm kiếm (và sắp xếp) được tối ưu hóa cao trong thư viện chuẩn (ví dụ: `Arrays.binarySearch()` trong Java, module `bisect` trong Python). Hãy ưu tiên sử dụng chúng trừ khi bạn có lý do cụ thể để tự cài đặt (ví dụ: học tập, cần tùy chỉnh đặc biệt).
* **Xử lý cạnh (Edge Cases) trong Binary Search:**
    * *Mảng rỗng:* Đảm bảo thuật toán xử lý đúng trường hợp mảng đầu vào không có phần tử nào.
    * *Phần tử không tồn tại:* Thuật toán phải trả về giá trị chỉ báo không tìm thấy (thường là `-1`) một cách chính xác.
    * *Tính toán `mid`:* Sử dụng `mid = low + (high - low) / 2` thay vì `mid = (low + high) / 2` để tránh nguy cơ tràn số nguyên (`integer overflow`) khi `low` và `high` đều là những số dương rất lớn.
* **Lỗi phổ biến (Common Pitfalls) trong Binary Search:**
    * *Off-by-one errors:* Lỗi sai lệch một đơn vị trong việc cập nhật `low` và `high` (ví dụ: gán `high = mid` thay vì `high = mid - 1`, hoặc `low = mid` thay vì `low = mid + 1`). Điều này có thể dẫn đến vòng lặp vô hạn hoặc bỏ sót kết quả.
    * *Điều kiện vòng lặp:* Sử dụng `while (low <= high)` thay vì `while (low < high)`. Nếu dùng `low < high`, trường hợp chỉ còn một phần tử (`low == high`) sẽ bị bỏ qua.
    * *Quên sắp xếp dữ liệu:* Áp dụng Binary Search trên dữ liệu chưa sắp xếp sẽ cho kết quả sai hoặc không đoán trước được.
* **Đệ quy vs. Lặp:**
    * Phiên bản lặp của Binary Search thường hiệu quả hơn một chút về bộ nhớ (O(1) so với O(log N) cho stack đệ quy) và có thể nhanh hơn đôi chút do tránh được chi phí gọi hàm.
    * Phiên bản đệ quy có thể trông gọn gàng và dễ đọc hơn đối với một số người, phản ánh rõ hơn bản chất "chia để trị".

## **8. Kết luận**

Tìm kiếm tuyến tính và tìm kiếm nhị phân là hai thuật toán nền tảng quan trọng mà mọi lập trình viên cần nắm vững. Tìm kiếm tuyến tính đơn giản nhưng chỉ hiệu quả với dữ liệu nhỏ hoặc chưa sắp xếp. Tìm kiếm nhị phân, với yêu cầu dữ liệu phải được sắp xếp, mang lại hiệu năng vượt trội O(log N), là lựa chọn tối ưu cho các tập dữ liệu lớn.

Hiểu rõ nguyên tắc hoạt động, độ phức tạp, ưu nhược điểm và các tình huống áp dụng của từng thuật toán sẽ giúp bạn đưa ra quyết định đúng đắn khi xây dựng các ứng dụng hiệu quả. Đồng thời, việc nhận biết các kỹ thuật tìm kiếm khác như bảng băm hay cây tìm kiếm nhị phân sẽ mở rộng thêm các công cụ giải quyết vấn đề trong kho kiến thức của bạn.
