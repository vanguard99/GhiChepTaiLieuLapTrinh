# THUẬT TOÁN SẮP XẾP CƠ BẢN BUBBLE SORT, SELECTION SORT, INSERTION SORT

## **1. Giới thiệu**

**Sắp xếp (Sorting)** là một trong những tác vụ nền tảng và phổ biến nhất trong khoa học máy tính. Nó là quá trình tổ chức lại một tập hợp các phần tử (như số, chuỗi, đối tượng) theo một thứ tự logic nhất định, ví dụ như thứ tự tăng dần, giảm dần về giá trị số hoặc theo thứ tự từ điển.

**Tại sao sắp xếp lại quan trọng?**

  * **Tối ưu hóa tìm kiếm:** Dữ liệu đã được sắp xếp cho phép áp dụng các thuật toán tìm kiếm hiệu quả hơn nhiều (như **Tìm kiếm Nhị phân - Binary Search** với độ phức tạp O(log N)) so với tìm kiếm trên dữ liệu chưa sắp xếp (thường là **Tìm kiếm Tuyến tính - Linear Search** với độ phức tạp O(N)).
  * **Dễ đọc và phân tích:** Dữ liệu được sắp xếp giúp con người dễ dàng quan sát, phân tích và rút ra thông tin hơn.
  * **Tiền xử lý cho các thuật toán khác:** Nhiều thuật toán phức tạp hơn yêu cầu dữ liệu đầu vào phải được sắp xếp trước.
  * **Hợp nhất dữ liệu:** Sắp xếp là bước quan trọng trong các thuật toán hợp nhất (merge) dữ liệu từ nhiều nguồn.

Hướng dẫn này tập trung vào ba thuật toán sắp xếp cơ bản, thường được giới thiệu đầu tiên do tính đơn giản và dễ hiểu của chúng:

  * **Sắp xếp Nổi bọt (Bubble Sort)**
  * **Sắp xếp Chọn (Selection Sort)**
  * **Sắp xếp Chèn (Insertion Sort)**

Chúng ta sẽ đi sâu vào nguyên tắc hoạt động, cách cài đặt bằng Java, phân tích độ phức tạp, ưu nhược điểm, so sánh và các cân nhắc thực tế khi sử dụng từng thuật toán.

## **2. Các Khái niệm Cơ bản**

  * **Phần tử (Element):** Một giá trị đơn lẻ trong danh sách cần sắp xếp (ví dụ: một số nguyên, một chuỗi).
  * **Danh sách/Mảng (List/Array):** Cấu trúc dữ liệu chứa tập hợp các phần tử cần sắp xếp. Trong hướng dẫn này, chúng ta chủ yếu xét trên mảng.
  * **So sánh (Comparison):** Thao tác cơ bản để xác định thứ tự tương đối giữa hai phần tử.
  * **Hoán đổi/Tráo đổi (Swap):** Thao tác đổi vị trí của hai phần tử trong danh sách.
  * **Sắp xếp Tại chỗ (In-place Sort):** Thuật toán sắp xếp không yêu cầu hoặc yêu cầu rất ít bộ nhớ phụ (thường là O(1) hoặc O(log N)) ngoài bộ nhớ chứa danh sách đầu vào. Bubble Sort, Selection Sort, và Insertion Sort đều là các thuật toán sắp xếp tại chỗ.
  * **Sắp xếp Ổn định (Stable Sort):** Một thuật toán sắp xếp được gọi là ổn định nếu nó duy trì thứ tự tương đối của các phần tử có giá trị bằng nhau. Ví dụ, nếu có hai phần tử cùng giá trị `5`, và phần tử `5` thứ nhất xuất hiện trước phần tử `5` thứ hai trong danh sách ban đầu, thì sau khi sắp xếp ổn định, phần tử `5` thứ nhất vẫn sẽ đứng trước phần tử `5` thứ hai. Bubble Sort và Insertion Sort là ổn định, trong khi Selection Sort thì không.

## **3. Sắp xếp Nổi bọt (Bubble Sort)**

Đây là một trong những thuật toán sắp xếp đơn giản nhất về mặt ý tưởng.

### **3.1. Nguyên tắc hoạt động**

Ý tưởng chính của Bubble Sort là **lặp đi lặp lại việc duyệt qua danh sách, so sánh các cặp phần tử liền kề và hoán đổi vị trí nếu chúng sai thứ tự**. Quá trình này giống như việc các "bọt khí" nhẹ hơn (giá trị nhỏ hơn nếu sắp xếp tăng dần) nổi dần lên đầu danh sách.

**Các bước cụ thể:**

1.  Bắt đầu từ đầu danh sách, so sánh phần tử đầu tiên (`list[0]`) với phần tử thứ hai (`list[1]`). Nếu `list[0] > list[1]` (đối với sắp xếp tăng dần), hoán đổi chúng.
2.  Tiếp tục so sánh phần tử thứ hai (`list[1]`) với phần tử thứ ba (`list[2]`) và hoán đổi nếu cần.
3.  Lặp lại quá trình này cho đến khi so sánh và hoán đổi (nếu cần) cặp phần tử cuối cùng (`list[n-2]` và `list[n-1]`). Sau lượt duyệt đầu tiên này, **phần tử lớn nhất chắc chắn đã "chìm" xuống vị trí cuối cùng**.
4.  Lặp lại toàn bộ quá trình trên (từ bước 1 đến 3) cho `n-1` phần tử đầu tiên (vì phần tử cuối đã đúng vị trí). Sau lượt thứ hai, phần tử lớn thứ hai sẽ nằm ở vị trí kế cuối.
5.  Tiếp tục lặp lại các lượt duyệt, mỗi lượt giảm phạm vi xét đi 1 phần tử ở cuối, cho đến khi không còn cặp nào cần hoán đổi hoặc đã thực hiện đủ `n-1` lượt duyệt.

### **3.2. Ví dụ Minh họa**

Sắp xếp mảng `[2, 9, 5, 4, 8, 1]` tăng dần:

  * **Lượt 1:**
      * `(2, 9)` -\> giữ nguyên
      * `(9, 5)` -\> hoán đổi: `[2, 5, 9, 4, 8, 1]`
      * `(9, 4)` -\> hoán đổi: `[2, 5, 4, 9, 8, 1]`
      * `(9, 8)` -\> hoán đổi: `[2, 5, 4, 8, 9, 1]`
      * `(9, 1)` -\> hoán đổi: `[2, 5, 4, 8, 1, 9]` (Số 9 đã về cuối)
  * **Lượt 2:**
      * `(2, 5)` -\> giữ nguyên
      * `(5, 4)` -\> hoán đổi: `[2, 4, 5, 8, 1, 9]`
      * `(5, 8)` -\> giữ nguyên
      * `(8, 1)` -\> hoán đổi: `[2, 4, 5, 1, 8, 9]` (Số 8 đã về kế cuối)
  * **Lượt 3:**
      * `(2, 4)` -\> giữ nguyên
      * `(4, 5)` -\> giữ nguyên
      * `(5, 1)` -\> hoán đổi: `[2, 4, 1, 5, 8, 9]` (Số 5 đã về đúng vị trí)
  * **Lượt 4:**
      * `(2, 4)` -\> giữ nguyên
      * `(4, 1)` -\> hoán đổi: `[2, 1, 4, 5, 8, 9]` (Số 4 đã về đúng vị trí)
  * **Lượt 5:**
      * `(2, 1)` -\> hoán đổi: `[1, 2, 4, 5, 8, 9]` (Số 2 đã về đúng vị trí)
  * Mảng đã sắp xếp: `[1, 2, 4, 5, 8, 9]`

### **3.3. Cài đặt (Java)**

```java
/**
 * Sắp xếp mảng số nguyên theo thứ tự tăng dần bằng Bubble Sort.
 * Phiên bản cải tiến: dừng sớm nếu không có hoán đổi nào trong một lượt.
 *
 * @param list Mảng cần sắp xếp.
 */
public static void bubbleSort(int[] list) {
    boolean needNextPass = true; // Biến cờ kiểm tra xem có cần lượt duyệt tiếp theo không
    int n = list.length;

    // Vòng lặp ngoài quản lý số lượt duyệt tối đa (n-1 lượt)
    // và kiểm tra cờ needNextPass
    for (int k = 1; k < n && needNextPass; k++) {
        // Giả sử không cần lượt tiếp theo (mảng có thể đã được sắp xếp)
        needNextPass = false;

        // Vòng lặp trong thực hiện so sánh và hoán đổi các cặp liền kề
        // Phạm vi duyệt giảm dần sau mỗi lượt (n-k)
        for (int i = 0; i < n - k; i++) {
            if (list[i] > list[i + 1]) {
                // Hoán đổi list[i] và list[i+1]
                int temp = list[i];
                list[i] = list[i + 1];
                list[i + 1] = temp;

                // Nếu có hoán đổi, đánh dấu là cần lượt duyệt tiếp theo
                needNextPass = true;
            }
        }
        // Nếu needNextPass vẫn là false sau vòng lặp trong,
        // nghĩa là không có hoán đổi nào xảy ra -> mảng đã được sắp xếp -> thoát sớm.
    }
}

// Ví dụ sử dụng:
public static void main(String[] args) {
    int[] data = {2, 9, 5, 4, 8, 1, 6};
    System.out.println("Mảng ban đầu: " + java.util.Arrays.toString(data));
    bubbleSort(data);
    System.out.println("Mảng sau khi sắp xếp: " + java.util.Arrays.toString(data));
    // Kết quả: Mảng sau khi sắp xếp: [1, 2, 4, 5, 6, 8, 9]
}
```

### **3.4. Phân tích Độ phức tạp**

  * **Thời gian:**
      * **Trường hợp xấu nhất (Worst Case):** Mảng sắp xếp ngược thứ tự. Cần n-1 lượt duyệt, và mỗi lượt thực hiện O(n) phép so sánh và hoán đổi. Tổng độ phức tạp là O(N^2).
      * **Trường hợp tốt nhất (Best Case):** Mảng đã được sắp xếp. Với phiên bản cải tiến (sử dụng cờ `needNextPass`), chỉ cần 1 lượt duyệt để xác nhận không cần hoán đổi. Độ phức tạp là O(N). Nếu không có cải tiến, vẫn là O(N^2).
      * **Trường hợp trung bình (Average Case):** O(N^2).
  * **Không gian:** O(1) (sắp xếp tại chỗ, chỉ cần biến tạm `temp` để hoán đổi).

### **3.5. Ưu và Nhược điểm**

**Ưu điểm:**

  * **Đơn giản:** Rất dễ hiểu và dễ cài đặt.
  * **Phát hiện mảng đã sắp xếp:** Phiên bản cải tiến có thể kết thúc sớm nếu mảng đã được sắp xếp (O(N)).
  * **Ổn định (Stable):** Duy trì thứ tự tương đối của các phần tử bằng nhau.

**Nhược điểm:**

  * **Rất chậm:** Độ phức tạp O(N^2) làm cho nó không hiệu quả với các tập dữ liệu lớn.
  * **Nhiều phép hoán đổi:** Có thể thực hiện nhiều thao tác hoán đổi không cần thiết.

### **3.6. Khi nào nên sử dụng?**

  * **Mục đích học tập:** Là thuật toán tốt để bắt đầu tìm hiểu về sắp xếp.
  * **Tập dữ liệu rất nhỏ:** Khi N rất nhỏ, sự đơn giản có thể được ưu tiên.
  * **Kiểm tra mảng gần như đã sắp xếp:** Khi bạn nghi ngờ mảng gần như đã sắp xếp và muốn một thuật toán đơn giản có thể kết thúc nhanh trong trường hợp tốt nhất. *Tuy nhiên, Insertion Sort thường tốt hơn cho trường hợp này.*

## **4. Sắp xếp Chọn (Selection Sort)**

Thuật toán này cũng đơn giản nhưng có cách tiếp cận khác so với Bubble Sort.

### **4.1. Nguyên tắc hoạt động**

Ý tưởng của Selection Sort là **chia danh sách thành hai phần: phần đã sắp xếp (ban đầu rỗng, ở đầu danh sách) và phần chưa sắp xếp (ban đầu là toàn bộ danh sách)**. Thuật toán lặp đi lặp lại việc tìm phần tử nhỏ nhất (hoặc lớn nhất) trong phần chưa sắp xếp và đưa nó vào cuối phần đã sắp xếp.

**Các bước cụ thể (sắp xếp tăng dần):**

1.  Tìm phần tử có giá trị **nhỏ nhất** trong toàn bộ danh sách (từ chỉ số `0` đến `n-1`).
2.  **Hoán đổi** phần tử nhỏ nhất này với phần tử ở vị trí đầu tiên (`list[0]`). Bây giờ, phần tử `list[0]` đã nằm đúng vị trí cuối cùng của nó trong mảng đã sắp xếp. Phần đã sắp xếp là `[list[0]]`, phần chưa sắp xếp là từ `list[1]` đến `list[n-1]`.
3.  Tìm phần tử nhỏ nhất trong phần **chưa sắp xếp** (từ chỉ số `1` đến `n-1`).
4.  **Hoán đổi** phần tử nhỏ nhất này với phần tử ở đầu phần chưa sắp xếp (`list[1]`). Bây giờ, phần đã sắp xếp là `[list[0], list[1]]`, phần chưa sắp xếp là từ `list[2]` đến `list[n-1]`.
5.  Lặp lại quá trình tìm phần tử nhỏ nhất trong phần chưa sắp xếp và hoán đổi nó với phần tử đầu tiên của phần chưa sắp xếp, cho đến khi phần chưa sắp xếp chỉ còn 1 phần tử (tự động đã được sắp xếp).

### **4.2. Ví dụ Minh họa**

Sắp xếp mảng `[2, 9, 5, 4, 8, 1, 6]` tăng dần:

  * **Lượt 1 (i=0):**
      * Tìm min trong `[2, 9, 5, 4, 8, 1, 6]` -\> là `1` tại index `5`.
      * Hoán đổi `list[0]` (2) và `list[5]` (1): `[1, 9, 5, 4, 8, 2, 6]`
      * Phần đã sắp xếp: `[1]`
  * **Lượt 2 (i=1):**
      * Tìm min trong `[9, 5, 4, 8, 2, 6]` -\> là `2` tại index `5` (so với mảng gốc là index 5, nhưng trong phần chưa sắp xếp nó ở vị trí cuối).
      * Hoán đổi `list[1]` (9) và `list[5]` (2): `[1, 2, 5, 4, 8, 9, 6]`
      * Phần đã sắp xếp: `[1, 2]`
  * **Lượt 3 (i=2):**
      * Tìm min trong `[5, 4, 8, 9, 6]` -\> là `4` tại index `3`.
      * Hoán đổi `list[2]` (5) và `list[3]` (4): `[1, 2, 4, 5, 8, 9, 6]`
      * Phần đã sắp xếp: `[1, 2, 4]`
  * **Lượt 4 (i=3):**
      * Tìm min trong `[5, 8, 9, 6]` -\> là `5` tại index `3`.
      * `5` đã ở đúng vị trí (`list[3]`). Không cần hoán đổi.
      * Phần đã sắp xếp: `[1, 2, 4, 5]`
  * **Lượt 5 (i=4):**
      * Tìm min trong `[8, 9, 6]` -\> là `6` tại index `6`.
      * Hoán đổi `list[4]` (8) và `list[6]` (6): `[1, 2, 4, 5, 6, 9, 8]`
      * Phần đã sắp xếp: `[1, 2, 4, 5, 6]`
  * **Lượt 6 (i=5):**
      * Tìm min trong `[9, 8]` -\> là `8` tại index `6`.
      * Hoán đổi `list[5]` (9) và `list[6]` (8): `[1, 2, 4, 5, 6, 8, 9]`
      * Phần đã sắp xếp: `[1, 2, 4, 5, 6, 8]`
  * Phần chưa sắp xếp còn `[9]`. Kết thúc. Mảng đã sắp xếp: `[1, 2, 4, 5, 6, 8, 9]`

### **4.3. Cài đặt (Java)**

```java
/**
 * Sắp xếp mảng số nguyên theo thứ tự tăng dần bằng Selection Sort.
 *
 * @param list Mảng cần sắp xếp.
 */
public static void selectionSort(int[] list) {
    int n = list.length;

    // Vòng lặp ngoài: Duyệt qua từng vị trí cần đặt phần tử đúng (từ 0 đến n-2)
    for (int i = 0; i < n - 1; i++) {
        // Giả sử phần tử nhỏ nhất là phần tử đầu tiên của đoạn chưa sắp xếp
        int currentMin = list[i];
        int currentMinIndex = i;

        // Vòng lặp trong: Tìm phần tử nhỏ nhất thực sự trong đoạn chưa sắp xếp (từ i+1 đến n-1)
        for (int j = i + 1; j < n; j++) {
            if (list[j] < currentMin) {
                currentMin = list[j];
                currentMinIndex = j;
            }
        }

        // Nếu tìm thấy phần tử nhỏ hơn không phải là list[i]
        // thì hoán đổi nó với list[i]
        if (currentMinIndex != i) {
            list[currentMinIndex] = list[i];
            list[i] = currentMin; // Gán giá trị min đã lưu vào list[i]
        }
        // Sau vòng lặp này, list[i] đã chứa phần tử nhỏ nhất
        // trong đoạn từ i đến n-1 và nằm đúng vị trí.
    }
}

// Ví dụ sử dụng:
public static void main(String[] args) {
    int[] data = {2, 9, 5, 4, 8, 1, 6};
    System.out.println("Mảng ban đầu: " + java.util.Arrays.toString(data));
    selectionSort(data);
    System.out.println("Mảng sau khi sắp xếp: " + java.util.Arrays.toString(data));
    // Kết quả: Mảng sau khi sắp xếp: [1, 2, 4, 5, 6, 8, 9]
}
```

### **4.4. Phân tích Độ phức tạp**

  * **Thời gian:**
      * Vòng lặp ngoài chạy n-1 lần.
      * Trong mỗi lần lặp của vòng lặp ngoài (lần thứ i), vòng lặp trong chạy n-1-i lần để tìm phần tử nhỏ nhất. Số phép so sánh là (n-1) + (n-2) + ... + 1 = \\frac{(n-1)n}{2}.
      * Số phép hoán đổi tối đa là n-1 (mỗi lần lặp ngoài chỉ hoán đổi 1 lần).
      * Độ phức tạp tổng thể luôn là O(N^2) cho cả trường hợp tốt nhất, xấu nhất và trung bình, vì nó luôn phải duyệt hết phần chưa sắp xếp để tìm phần tử nhỏ nhất, bất kể mảng ban đầu như thế nào.
  * **Không gian:** O(1) (sắp xếp tại chỗ).

### **4.5. Ưu và Nhược điểm**

**Ưu điểm:**

  * **Đơn giản:** Dễ hiểu và cài đặt.
  * **Ít phép hoán đổi:** Số lần hoán đổi là tối đa N-1, ít hơn đáng kể so với Bubble Sort trong trường hợp xấu nhất. Điều này có thể hữu ích nếu chi phí ghi (hoán đổi) vào bộ nhớ là cao.
  * **Hiệu quả không phụ thuộc vào dữ liệu ban đầu:** Độ phức tạp thời gian luôn là O(N^2).

**Nhược điểm:**

  * **Chậm:** Vẫn là O(N^2), không hiệu quả cho dữ liệu lớn.
  * **Không ổn định (Not Stable):** Thao tác hoán đổi có thể thay đổi thứ tự tương đối của các phần tử bằng nhau. Ví dụ: `[5a, 5b, 1]`. Tìm min là `1`, hoán đổi với `5a` -\> `[1, 5b, 5a]`. Thứ tự của `5a` và `5b` đã bị đảo.
  * **Không tận dụng được tình trạng đã sắp xếp của mảng:** Ngay cả khi mảng đã được sắp xếp, nó vẫn thực hiện đủ O(N^2) phép so sánh.

### **4.6. Khi nào nên sử dụng?**

  * **Mục đích học tập.**
  * **Tập dữ liệu nhỏ.**
  * **Khi chi phí hoán đổi (ghi vào bộ nhớ) là yếu tố quan trọng cần tối thiểu hóa** và O(N^2) về thời gian là chấp nhận được.

## **5. Sắp xếp Chèn (Insertion Sort)**

Thuật toán này mô phỏng cách chúng ta thường sắp xếp một bộ bài trên tay.

### **5.1. Nguyên tắc hoạt động**

Giống như Selection Sort, Insertion Sort cũng **chia danh sách thành hai phần: phần đã sắp xếp (bên trái) và phần chưa sắp xếp (bên phải)**. Nó hoạt động bằng cách lặp đi lặp lại việc lấy một phần tử từ phần chưa sắp xếp và **chèn (insert)** nó vào **vị trí đúng** trong phần đã sắp xếp.

**Các bước cụ thể (sắp xếp tăng dần):**

1.  Bắt đầu với phần tử thứ hai (`list[1]`). Phần đã sắp xếp ban đầu chỉ chứa phần tử đầu tiên (`list[0]`).
2.  Lấy phần tử hiện tại từ phần chưa sắp xếp (gọi là `currentElement`, ban đầu là `list[1]`).
3.  So sánh `currentElement` với các phần tử trong phần đã sắp xếp (từ phải sang trái, tức là từ `list[0]` nếu đang xét `list[1]`).
4.  Nếu phần tử trong phần đã sắp xếp lớn hơn `currentElement`, **dịch chuyển (shift)** phần tử đó sang phải một vị trí để tạo khoảng trống.
5.  Lặp lại bước 3 và 4 cho đến khi tìm thấy một phần tử trong phần đã sắp xếp nhỏ hơn hoặc bằng `currentElement`, hoặc đã duyệt hết phần đã sắp xếp.
6.  **Chèn** `currentElement` vào vị trí trống vừa tạo ra (hoặc vị trí đầu tiên nếu nó nhỏ nhất). Bây giờ, phần đã sắp xếp đã mở rộng thêm một phần tử.
7.  Lấy phần tử tiếp theo từ phần chưa sắp xếp (`list[2]`, `list[3]`, ...) và lặp lại quá trình từ bước 3 đến 6 cho đến khi tất cả các phần tử đã được chèn vào phần đã sắp xếp.

### **5.2. Ví dụ Minh họa**

Sắp xếp mảng `[2, 9, 5, 4, 8, 1, 6]` tăng dần:

  * **i = 1 (Xét `9`):**
      * Phần đã sắp xếp: `[2]`. `currentElement = 9`.
      * `9 > 2`. Không dịch chuyển. Chèn `9` vào sau `2`.
      * Mảng: `[2, 9, 5, 4, 8, 1, 6]`
  * **i = 2 (Xét `5`):**
      * Phần đã sắp xếp: `[2, 9]`. `currentElement = 5`.
      * So sánh `5` với `9`. `9 > 5`. Dịch `9` sang phải: `[2, 9, 9, 4, 8, 1, 6]`
      * So sánh `5` với `2`. `2 < 5`. Dừng dịch chuyển.
      * Chèn `5` vào vị trí trống (index 1): `[2, 5, 9, 4, 8, 1, 6]`
  * **i = 3 (Xét `4`):**
      * Phần đã sắp xếp: `[2, 5, 9]`. `currentElement = 4`.
      * So sánh `4` với `9`. `9 > 4`. Dịch `9` sang phải: `[2, 5, 9, 9, 8, 1, 6]`
      * So sánh `4` với `5`. `5 > 4`. Dịch `5` sang phải: `[2, 5, 5, 9, 8, 1, 6]`
      * So sánh `4` với `2`. `2 < 4`. Dừng dịch chuyển.
      * Chèn `4` vào vị trí trống (index 1): `[2, 4, 5, 9, 8, 1, 6]`
  * **i = 4 (Xét `8`):**
      * Phần đã sắp xếp: `[2, 4, 5, 9]`. `currentElement = 8`.
      * So sánh `8` với `9`. `9 > 8`. Dịch `9` sang phải: `[2, 4, 5, 9, 9, 1, 6]`
      * So sánh `8` với `5`. `5 < 8`. Dừng dịch chuyển.
      * Chèn `8` vào vị trí trống (index 4): `[2, 4, 5, 8, 9, 1, 6]`
  * **i = 5 (Xét `1`):**
      * Phần đã sắp xếp: `[2, 4, 5, 8, 9]`. `currentElement = 1`.
      * Dịch `9`, `8`, `5`, `4`, `2` sang phải.
      * Chèn `1` vào đầu (index 0): `[1, 2, 4, 5, 8, 9, 6]`
  * **i = 6 (Xét `6`):**
      * Phần đã sắp xếp: `[1, 2, 4, 5, 8, 9]`. `currentElement = 6`.
      * Dịch `9`, `8` sang phải.
      * So sánh `6` với `5`. `5 < 6`. Dừng dịch chuyển.
      * Chèn `6` vào vị trí trống (index 5): `[1, 2, 4, 5, 6, 8, 9]`
  * Kết thúc. Mảng đã sắp xếp: `[1, 2, 4, 5, 6, 8, 9]`

### **5.3. Cài đặt (Java)**

```java
/**
 * Sắp xếp mảng số nguyên theo thứ tự tăng dần bằng Insertion Sort.
 *
 * @param list Mảng cần sắp xếp.
 */
public static void insertionSort(int[] list) {
    int n = list.length;

    // Vòng lặp ngoài: Duyệt qua các phần tử của phần chưa sắp xếp (từ index 1)
    for (int i = 1; i < n; i++) {
        // Lưu phần tử hiện tại cần chèn
        int currentElement = list[i];
        int k; // Chỉ số để duyệt trong phần đã sắp xếp

        // Vòng lặp trong: Dịch chuyển các phần tử lớn hơn currentElement
        // trong phần đã sắp xếp (list[0...i-1]) sang phải
        for (k = i - 1; k >= 0 && list[k] > currentElement; k--) {
            list[k + 1] = list[k]; // Dịch chuyển
        }

        // Chèn currentElement vào vị trí đúng (vị trí k+1 sau khi vòng lặp dừng)
        list[k + 1] = currentElement;

        // Sau vòng lặp này, list[0...i] đã được sắp xếp.
    }
}

// Ví dụ sử dụng:
public static void main(String[] args) {
    int[] data = {2, 9, 5, 4, 8, 1, 6};
    System.out.println("Mảng ban đầu: " + java.util.Arrays.toString(data));
    insertionSort(data);
    System.out.println("Mảng sau khi sắp xếp: " + java.util.Arrays.toString(data));
    // Kết quả: Mảng sau khi sắp xếp: [1, 2, 4, 5, 6, 8, 9]
}

```

### **5.4. Phân tích Độ phức tạp**

  * **Thời gian:**
      * **Trường hợp xấu nhất (Worst Case):** Mảng sắp xếp ngược thứ tự. Mỗi phần tử mới cần được so sánh và dịch chuyển qua toàn bộ phần đã sắp xếp trước đó. Số phép so sánh/dịch chuyển là 1 + 2 + ... + (n-1) = O(N^2).
      * **Trường hợp tốt nhất (Best Case):** Mảng đã được sắp xếp. Mỗi phần tử mới chỉ cần 1 phép so sánh với phần tử cuối cùng của phần đã sắp xếp để xác định vị trí chèn (chính là vị trí hiện tại). Độ phức tạp là O(N).
      * **Trường hợp trung bình (Average Case):** O(N^2).
  * **Không gian:** O(1) (sắp xếp tại chỗ).

### **5.5. Ưu và Nhược điểm**

**Ưu điểm:**

  * **Đơn giản:** Dễ hiểu và cài đặt.
  * **Hiệu quả với dữ liệu nhỏ:** Tương đối nhanh với N nhỏ.
  * **Hiệu quả với dữ liệu gần như đã sắp xếp:** Là một trong những thuật toán nhanh nhất cho loại dữ liệu này (tiệm cận O(N)).
  * **Ổn định (Stable):** Duy trì thứ tự tương đối của các phần tử bằng nhau.
  * **Online:** Có thể sắp xếp danh sách khi nhận từng phần tử một mà không cần biết trước toàn bộ danh sách.
  * **Tại chỗ (In-place):** Tốn ít bộ nhớ phụ.

**Nhược điểm:**

  * **Chậm với dữ liệu lớn và ngẫu nhiên:** Độ phức tạp trung bình và xấu nhất là O(N^2).
  * **Nhiều phép dịch chuyển:** Trong trường hợp xấu nhất, số lần dịch chuyển phần tử lớn.

### **5.6. Khi nào nên sử dụng?**

  * **Mục đích học tập.**
  * **Tập dữ liệu nhỏ.**
  * **Khi dữ liệu đầu vào gần như đã được sắp xếp.** Đây là trường hợp Insertion Sort tỏa sáng.
  * **Khi cần một thuật toán sắp xếp online và ổn định.**
  * Là một phần của các thuật toán sắp xếp phức tạp hơn như **Timsort** (thuật toán sắp xếp mặc định trong Python và Java cho đối tượng) hoặc **Introsort**, chúng thường sử dụng Insertion Sort cho các mảng con nhỏ.

## **6. So sánh các Thuật toán Sắp xếp Cơ bản**

| Đặc điểm                              | Bubble Sort (cải tiến)       | Selection Sort     | Insertion Sort               |
| :------------------------------------ | :--------------------------- | :----------------- | :--------------------------- |
| **Độ phức tạp (Best)**                | O(N)                         | O(N^2)             | O(N)                         |
| **Độ phức tạp (Avg)**                 | O(N^2)                       | O(N^2)             | O(N^2)                       |
| **Độ phức tạp (Worst)**               | O(N^2)                       | O(N^2)             | O(N^2)                       |
| **Độ phức tạp không gian**            | O(1)                         | O(1)               | O(1)                         |
| **Ổn định (Stable)**                  | Có                           | Không              | Có                           |
| **Tại chỗ (In-place)**                | Có                           | Có                 | Có                           |
| **Số phép so sánh**                   | O(N^2)                       | Luôn O(N^2)        | O(N) (best) - O(N^2) (worst) |
| **Số phép hoán đổi/dịch chuyển**      | O(1) (best) - O(N^2) (worst) | Tối đa O(N) (swap) | O(N) (best) - O(N^2) (shift) |
| **Hiệu quả với dữ liệu gần sắp xếp?** | Trung bình (dừng sớm)        | Kém                | **Tốt**                      |

## **7. Các Thuật toán Sắp xếp Nâng cao (So sánh Ngắn gọn)**

Mặc dù các thuật toán O(N^2) hữu ích trong một số trường hợp, đối với dữ liệu lớn, các thuật toán sắp xếp hiệu quả hơn với độ phức tạp O(N log N) thường được ưa chuộng:

  * **Merge Sort:**
      * Nguyên tắc: Chia để trị (Divide and Conquer). Chia đôi mảng, sắp xếp đệ quy hai nửa, sau đó trộn (merge) hai nửa đã sắp xếp lại.
      * Độ phức tạp: Luôn là O(N log N) (best, average, worst).
      * Ưu điểm: Đảm bảo hiệu năng O(N log N), ổn định.
      * Nhược điểm: Cần bộ nhớ phụ O(N) để trộn (không phải in-place).
  * **Quick Sort:**
      * Nguyên tắc: Chia để trị. Chọn một phần tử làm "chốt" (pivot), phân hoạch (partition) mảng thành hai phần: nhỏ hơn pivot và lớn hơn pivot, sau đó sắp xếp đệ quy hai phần.
      * Độ phức tạp: O(N log N) (best, average), O(N^2) (worst - hiếm khi xảy ra với cách chọn pivot tốt).
      * Ưu điểm: Thường nhanh hơn Merge Sort trong thực tế (hằng số thấp hơn), sắp xếp tại chỗ (thường là O(log N) không gian cho stack đệ quy).
      * Nhược điểm: Trường hợp xấu nhất là O(N^2), không ổn định.
  * **Heap Sort:**
      * Nguyên tắc: Sử dụng cấu trúc dữ liệu Heap (thường là Max Heap). Xây dựng Heap từ mảng, sau đó lặp lại việc lấy phần tử lớn nhất (ở gốc Heap) đưa về cuối mảng và hiệu chỉnh lại Heap.
      * Độ phức tạp: Luôn là O(N log N).
      * Ưu điểm: Đảm bảo O(N log N), sắp xếp tại chỗ (O(1) không gian).
      * Nhược điểm: Thường chậm hơn Quick Sort trong thực tế, không ổn định.

## **8. Thực tiễn và Tối ưu hóa**

  * **Sử dụng thư viện chuẩn:** Trong hầu hết các trường hợp thực tế, hãy sử dụng các hàm/phương thức sắp xếp có sẵn trong thư viện chuẩn của ngôn ngữ lập trình (ví dụ: `Arrays.sort()` trong Java, `sorted()` hoặc `list.sort()` trong Python). Chúng thường được triển khai bằng các thuật toán hiệu quả (O(N log N) như Timsort hoặc Introsort) và được tối ưu hóa cao.
  * **Hiểu rõ dữ liệu:** Nếu bạn biết dữ liệu của mình có đặc điểm đặc biệt (ví dụ: gần như đã sắp xếp, phạm vi giá trị nhỏ), bạn có thể chọn thuật toán phù hợp hơn (ví dụ: Insertion Sort cho dữ liệu gần sắp xếp, Counting Sort hoặc Radix Sort cho phạm vi giá trị nhỏ).
  * **Kiểu dữ liệu:** Các thuật toán so sánh (như 3 thuật toán trên) hoạt động tốt với nhiều kiểu dữ liệu, miễn là có thể định nghĩa phép so sánh giữa chúng (ví dụ: số, chuỗi, đối tượng có cài đặt `Comparable` hoặc `Comparator`).
  * **Lỗi phổ biến (Common Pitfalls):**
      * **Lỗi chỉ số (Off-by-one errors):** Sai sót trong việc xác định giới hạn của các vòng lặp (ví dụ: `< n` so với `<= n`, `n-1` so với `n`).
      * **Lỗi logic hoán đổi/dịch chuyển:** Thực hiện sai thao tác hoán đổi hoặc dịch chuyển phần tử.
      * **Quên xử lý trường hợp biên:** Mảng rỗng, mảng có 1 phần tử.
  * **Tối ưu hóa Micro (Micro-optimizations):** Đối với các thuật toán O(N^2), việc tối ưu hóa nhỏ trong vòng lặp (ví dụ: giảm số lần truy cập mảng) thường không mang lại lợi ích đáng kể so với việc chọn một thuật toán có độ phức tạp tốt hơn về mặt tiệm cận (O(N log N)).

## **9. Kết luận**

Bubble Sort, Selection Sort, và Insertion Sort là những viên gạch nền tảng quan trọng trong việc tìm hiểu về thuật toán sắp xếp. Mặc dù độ phức tạp O(N^2) khiến chúng không phù hợp cho các tập dữ liệu lớn trong hầu hết các ứng dụng thực tế, việc hiểu rõ cách chúng hoạt động, ưu nhược điểm và độ phức tạp của từng loại mang lại nhiều lợi ích:

  * Xây dựng tư duy giải thuật về sắp xếp.
  * Hiểu được các khái niệm cơ bản như so sánh, hoán đổi, ổn định, tại chỗ.
  * Biết cách lựa chọn thuật toán phù hợp cho các trường hợp đặc biệt (dữ liệu nhỏ, gần sắp xếp).
  * Làm nền tảng để hiểu các thuật toán sắp xếp phức tạp và hiệu quả hơn.