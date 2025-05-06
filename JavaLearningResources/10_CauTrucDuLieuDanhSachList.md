# CẤU TRÚC DỮ LIỆU DANH SÁCH (LIST)

## 1\. Giới thiệu về Cấu trúc Dữ liệu Danh sách (List)

Trong khoa học máy tính, **danh sách (List)** là một trong những **Kiểu dữ liệu trừu tượng (Abstract Data Type - ADT)** cơ bản và được sử dụng rộng rãi nhất. Nó đại diện cho một tập hợp các phần tử được sắp xếp theo một thứ tự tuyến tính xác định.

### 1.1. Danh sách là gì?

Một ADT Danh sách định nghĩa một tập hợp dữ liệu và các thao tác có thể thực hiện trên tập hợp đó. Các đặc điểm chính của một danh sách bao gồm:

  * **Thứ tự (Ordered):** Các phần tử trong danh sách được lưu trữ và truy cập theo một trình tự cụ thể. Mỗi phần tử có một vị trí (index) xác định.
  * **Cho phép trùng lặp (Allows Duplicates):** Một danh sách có thể chứa các phần tử giống hệt nhau tại các vị trí khác nhau.
  * **Truy cập theo chỉ số (Indexed Access):** Các phần tử có thể được truy cập trực tiếp thông qua chỉ số (index) của chúng, thường bắt đầu từ 0.
  * **Kích thước động (Dynamic Size):** Danh sách thường có khả năng thay đổi kích thước (thêm hoặc bớt phần tử) trong quá trình thực thi.

### 1.2. Vai trò và Ứng dụng phổ biến của Danh sách

Danh sách là cấu trúc dữ liệu nền tảng cho vô số ứng dụng trong lập trình:

  * Lưu trữ và quản lý tập hợp các đối tượng: danh sách sinh viên, danh sách sản phẩm, danh sách các giao dịch,...
  * Triển khai các cấu trúc dữ liệu khác: Stack, Queue có thể được cài đặt hiệu quả bằng cách sử dụng List.
  * Làm việc với API: Nhiều API trả về hoặc nhận đầu vào là các danh sách dữ liệu.
  * Thuật toán: Danh sách là thành phần cốt lõi trong nhiều thuật toán sắp xếp, tìm kiếm và xử lý dữ liệu.

### 1.3. Giới thiệu `java.util.List` Interface trong Java Collection Framework (JCF)

**Java Collection Framework (JCF)** cung cấp một tập hợp các interface và lớp để làm việc hiệu quả với các cấu trúc dữ liệu phổ biến. `java.util.List` là interface trung tâm đại diện cho ADT Danh sách trong JCF.

  * **Kế thừa:** `List` kế thừa từ interface `java.util.Collection` và `java.lang.Iterable`. Do đó, nó thừa hưởng các phương thức cơ bản để quản lý tập hợp (như `add`, `remove`, `size`, `contains`, `clear`) và khả năng duyệt qua các phần tử (qua `iterator()`).

  * **Đặc trưng:** `List` bổ sung các phương thức quan trọng liên quan đến thứ tự và truy cập theo chỉ số:

      * `add(int index, E element)`: Chèn phần tử vào vị trí chỉ định.
      * `get(int index)`: Lấy phần tử tại vị trí chỉ định.
      * `set(int index, E element)`: Thay thế phần tử tại vị trí chỉ định.
      * `remove(int index)`: Xóa phần tử tại vị trí chỉ định.
      * `indexOf(Object o)`: Tìm chỉ số đầu tiên của phần tử.
      * `lastIndexOf(Object o)`: Tìm chỉ số cuối cùng của phần tử.
      * `listIterator()` và `listIterator(int index)`: Trả về `ListIterator` để duyệt và thao tác hai chiều.
      * `subList(int fromIndex, int toIndex)`: Trả về một "view" (không phải bản sao) của một phần danh sách.

  * **Triển khai phổ biến:** JCF cung cấp hai lớp triển khai (implementation) chính cho interface `List` là `ArrayList` và `LinkedList`, mỗi loại có ưu và nhược điểm riêng dựa trên cấu trúc lưu trữ bên trong của chúng.

## 2\. `ArrayList`: Danh sách dựa trên Mảng Động

`java.util.ArrayList` là lớp triển khai `List` sử dụng một mảng (array) động bên trong để lưu trữ các phần tử. Đây là lựa chọn mặc định và phổ biến nhất khi cần một cấu trúc dữ liệu danh sách trong Java.

### 2.1. Khái niệm và Đặc điểm cốt lõi

  * **Lưu trữ:** Sử dụng mảng `Object[]` để chứa các tham chiếu đến phần tử.
  * **Truy cập nhanh:** Cho phép truy cập ngẫu nhiên (random access) vào bất kỳ phần tử nào thông qua chỉ số với thời gian không đổi O(1).
  * **Kích thước động:** Mảng bên trong có thể tự động thay đổi kích thước khi cần thiết (thường là khi thêm phần tử vào danh sách đã đầy).
  * **Thứ tự:** Duy trì thứ tự chèn các phần tử.
  * **Null:** Cho phép lưu trữ phần tử `null`.
  * **Không đồng bộ (Not Synchronized):** `ArrayList` không thread-safe. Nếu nhiều luồng truy cập và sửa đổi `ArrayList` đồng thời, cần có cơ chế đồng bộ hóa bên ngoài.

### 2.2. Cơ chế hoạt động bên trong (Internal Mechanism)

Hiểu cách `ArrayList` hoạt động bên trong giúp sử dụng hiệu quả hơn:

  * **Mảng lưu trữ:** Một biến instance, thường là `transient Object[] elementData;`, được sử dụng để lưu trữ các phần tử. `transient` để không serialize mảng trống không cần thiết.
  * **`size` và `capacity`:**
      * `size`: Số lượng phần tử *hiện có* trong danh sách.
      * `capacity`: Kích thước của mảng `elementData` *hiện tại*. `capacity` luôn lớn hơn hoặc bằng `size`.
  * **Khởi tạo:**
      * `new ArrayList()`: Tạo `ArrayList` với mảng `elementData` rỗng ban đầu. Capacity mặc định (thường là 10) chỉ được cấp phát khi phần tử đầu tiên được thêm vào.
      * `new ArrayList(int initialCapacity)`: Tạo `ArrayList` với `capacity` ban đầu được chỉ định. Hữu ích nếu bạn biết trước số lượng phần tử ước tính để tránh việc thay đổi kích thước mảng nhiều lần.
      * `new ArrayList(Collection<? extends E> c)`: Tạo `ArrayList` chứa các phần tử từ một `Collection` khác.
  * **Cơ chế tăng kích thước (Grow Mechanism):**
      * Khi `add()` một phần tử và `size` bằng `capacity` hiện tại, `ArrayList` cần tăng kích thước mảng `elementData`.
      * Thuật toán tăng kích thước phổ biến là tăng khoảng 50% so với capacity cũ (`newCapacity = oldCapacity + (oldCapacity >> 1)`). Ví dụ: từ 10 lên 15, từ 15 lên 22,...
      * Quá trình này bao gồm: tạo một mảng mới lớn hơn, sao chép toàn bộ phần tử từ mảng cũ sang mảng mới, và sau đó `elementData` trỏ đến mảng mới.
      * Việc sao chép mảng này tốn thời gian O(n), với n là `size` hiện tại. Tuy nhiên, do việc thay đổi kích thước không xảy ra thường xuyên, chi phí này được "phân bổ" (amortized) qua các thao tác `add()`. Vì vậy, thời gian trung bình (amortized time) cho thao tác `add()` vào cuối danh sách vẫn là O(1).
  * **`trimToSize()`:** Phương thức này giảm `capacity` của mảng `elementData` xuống bằng đúng `size` hiện tại. Hữu ích để tiết kiệm bộ nhớ nếu danh sách không còn thay đổi kích thước nhiều sau đó.

### 2.3. Các Thao tác Cơ bản và Độ phức tạp Thời gian (Big O Notation)

| Thao tác              | Phương thức ví dụ             | Độ phức tạp   | Giải thích                                                    |
| :-------------------- | :---------------------------- | :------------ | :------------------------------------------------------------ |
| Truy cập theo chỉ số  | `get(index)`                  | O(1)          | Truy cập trực tiếp vào phần tử mảng.                          |
| Cập nhật theo chỉ số  | `set(index, element)`         | O(1)          | Ghi đè trực tiếp lên phần tử mảng.                            |
| Thêm vào cuối         | `add(element)`                | O(1) đến O(n) | Thường là O(1), nhưng O(n) khi cần resize mảng (*amortized*). |
| Xóa khỏi cuối         | `remove(size - 1)`            | O(1)          | Chỉ cần giảm `size`.                                          |
| **Thêm vào đầu/giữa** | `add(index, element)`         | **O(n)**      | Phải dịch chuyển tất cả phần tử từ `index` về sau.            |
| **Xóa khỏi đầu/giữa** | `remove(index)` / `remove(o)` | **O(n)**      | Phải dịch chuyển tất cả phần tử từ `index` về sau.            |
| Tìm kiếm (tuần tự)    | `contains(o)`, `indexOf(o)`   | O(n)          | Phải duyệt qua các phần tử để tìm.                            |
| Lấy kích thước        | `size()`                      | O(1)          | Trả về giá trị biến `size`.                                   |
| Kiểm tra rỗng         | `isEmpty()`                   | O(1)          | Kiểm tra `size == 0`.                                         |

### 2.4. Ví dụ Mã nguồn Sử dụng `ArrayList`

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Iterator;
import java.util.ListIterator;
import java.util.Arrays;

public class ArrayListExample {
    public static void main(String[] args) {
        // 1. Khởi tạo ArrayList
        List<String> fruits = new ArrayList<>(); // Khởi tạo rỗng
        List<String> vegetables = new ArrayList<>(Arrays.asList("Cà rốt", "Bông cải xanh")); // Từ Collection khác
        List<Integer> numbers = new ArrayList<>(20); // Với initialCapacity

        // 2. Thêm phần tử
        fruits.add("Táo"); // Thêm vào cuối - O(1) amortized
        fruits.add("Chuối");
        fruits.add(1, "Cam"); // Thêm vào index 1 - O(n)
        System.out.println("Danh sách fruits sau khi thêm: " + fruits); // Output: [Táo, Cam, Chuối]

        // 3. Truy cập phần tử - O(1)
        String secondFruit = fruits.get(1);
        System.out.println("Phần tử thứ hai: " + secondFruit); // Output: Cam

        // 4. Cập nhật phần tử - O(1)
        fruits.set(0, "Táo đỏ");
        System.out.println("Sau khi cập nhật: " + fruits); // Output: [Táo đỏ, Cam, Chuối]

        // 5. Xóa phần tử
        fruits.remove(2); // Xóa theo index - O(n)
        fruits.remove("Cam"); // Xóa theo đối tượng - O(n)
        System.out.println("Sau khi xóa: " + fruits); // Output: [Táo đỏ]

        // 6. Kích thước và kiểm tra rỗng - O(1)
        System.out.println("Kích thước: " + fruits.size()); // Output: 1
        System.out.println("Có rỗng không? " + fruits.isEmpty()); // Output: false

        // 7. Kiểm tra chứa phần tử - O(n)
        System.out.println("Có chứa 'Táo đỏ'? " + fruits.contains("Táo đỏ")); // Output: true

        // 8. Duyệt danh sách
        System.out.println("\nDuyệt bằng for-each:");
        for (String fruit : fruits) {
            System.out.print(fruit + " ");
        }
        System.out.println();

        System.out.println("Duyệt bằng Iterator:");
        Iterator<String> iterator = fruits.iterator();
        while (iterator.hasNext()) {
            String fruit = iterator.next();
            System.out.print(fruit + " ");
            // iterator.remove(); // An toàn để xóa trong lúc duyệt bằng Iterator
        }
        System.out.println();

        // Thêm lại để dùng ListIterator
        fruits.add("Xoài");
        fruits.add("Ổi");

        System.out.println("Duyệt bằng ListIterator (tiến và lùi):");
        ListIterator<String> listIterator = fruits.listIterator(fruits.size()); // Bắt đầu từ cuối
        while (listIterator.hasPrevious()) {
            System.out.print(listIterator.previous() + " "); // Duyệt lùi
        }
        System.out.println();

        // 9. Xóa toàn bộ - O(n) (để gán null cho các phần tử)
        fruits.clear();
        System.out.println("Sau khi clear: " + fruits + ", size = " + fruits.size()); // Output: [], size = 0
    }
}
```

### 2.5. Khi nào nên sử dụng `ArrayList`? (Use Cases)

`ArrayList` là lựa chọn tốt nhất khi:

  * **Truy cập ngẫu nhiên thường xuyên:** Cần lấy hoặc cập nhật phần tử tại các vị trí index cụ thể một cách nhanh chóng (`get`, `set`).
  * **Thao tác chủ yếu ở cuối danh sách:** Thêm hoặc xóa phần tử ở cuối hiệu quả.
  * **Số lượng phần tử ít thay đổi hoặc biết trước:** Giúp tối ưu `initialCapacity`.
  * **Ưu tiên hiệu năng truy cập hơn hiệu năng thêm/xóa ở đầu/giữa.**

## 3\. `LinkedList`: Danh sách Liên kết Đôi

`java.util.LinkedList` là lớp triển khai `List` (và cả `Deque` - Hàng đợi hai đầu) sử dụng cấu trúc **danh sách liên kết đôi (doubly-linked list)** để lưu trữ các phần tử.

### 3.1. Khái niệm và Đặc điểm cốt lõi

  * **Lưu trữ:** Mỗi phần tử được bao bọc trong một `Node`. Mỗi `Node` chứa dữ liệu của phần tử và hai tham chiếu: một đến `Node` đứng trước (`prev`) và một đến `Node` đứng sau (`next`).
  * **Thêm/Xóa nhanh ở đầu/cuối:** Thao tác thêm hoặc xóa phần tử ở hai đầu danh sách rất hiệu quả (O(1)) vì chỉ cần cập nhật một vài con trỏ (`first`, `last`, và các con trỏ `next`/`prev` của node liền kề).
  * **Truy cập tuần tự chậm:** Để truy cập phần tử tại một index cụ thể, `LinkedList` phải duyệt từ đầu (`first`) hoặc từ cuối (`last`) đến vị trí đó (O(n)). Nó không hỗ trợ truy cập ngẫu nhiên hiệu quả như `ArrayList`.
  * **Thứ tự:** Duy trì thứ tự chèn các phần tử.
  * **Null:** Cho phép lưu trữ phần tử `null`.
  * **Không đồng bộ (Not Synchronized):** Giống như `ArrayList`, `LinkedList` không thread-safe.

### 3.2. Cơ chế hoạt động bên trong (Internal Mechanism)

  * **Cấu trúc `Node`:** Lớp `Node` bên trong `LinkedList` thường có dạng:
    ```java
    private static class Node<E> {
        E item;         // Dữ liệu phần tử
        Node<E> next;   // Tham chiếu đến Node tiếp theo
        Node<E> prev;   // Tham chiếu đến Node trước đó

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
    ```
  * **Con trỏ đầu/cuối:** `LinkedList` duy trì hai tham chiếu quan trọng:
      * `transient Node<E> first;` (hoặc `head`)
      * `transient Node<E> last;` (hoặc `tail`)
      * Chúng trỏ đến `Node` đầu tiên và `Node` cuối cùng trong danh sách, hoặc là `null` nếu danh sách rỗng.
  * **Thao tác `addLast(E e)` (Thêm vào cuối):**
    1.  Lấy `Node` cuối hiện tại (`l = last`).
    2.  Tạo một `Node` mới (`newNode`) với `prev` là `l`, `item` là `e`, `next` là `null`.
    3.  Gán `last = newNode`.
    4.  Nếu danh sách rỗng (`l == null`), thì `first = newNode`.
    5.  Nếu không rỗng, cập nhật `l.next = newNode`.
    6.  Tăng `size`. *(Tất cả thao tác này là O(1))*
  * **Thao tác `add(int index, E element)` (Thêm vào giữa):**
    1.  Kiểm tra `index` hợp lệ.
    2.  Nếu `index == size`, gọi `addLast(element)`.
    3.  Nếu không, **duyệt** từ `first` (nếu `index < size / 2`) hoặc từ `last` (nếu `index >= size / 2`) để tìm `Node` tại vị trí `index` (`succ`). *Đây là bước O(n)*.
    4.  Lấy `Node` trước đó (`pred = succ.prev`).
    5.  Tạo `Node` mới (`newNode`) với `prev` là `pred`, `item` là `element`, `next` là `succ`.
    6.  Cập nhật `succ.prev = newNode`.
    7.  Nếu `pred == null` (chèn vào đầu), cập nhật `first = newNode`.
    8.  Nếu `pred != null`, cập nhật `pred.next = newNode`.
    9.  Tăng `size`.
  * **Thao tác `remove(int index)` (Xóa ở giữa):**
    1.  Kiểm tra `index` hợp lệ.
    2.  **Duyệt** để tìm `Node` tại vị trí `index` (`x`). *Bước O(n)*.
    3.  Lấy `Node` trước (`pred = x.prev`) và `Node` sau (`succ = x.next`).
    4.  Cập nhật con trỏ của các node xung quanh để "bỏ qua" `Node x`:
          * Nếu `pred == null` (xóa node đầu), `first = succ`.
          * Nếu `pred != null`, `pred.next = succ`.
          * Nếu `succ == null` (xóa node cuối), `last = pred`.
          * Nếu `succ != null`, `succ.prev = pred`.
    5.  Gán `x.item = null` (giúp GC).
    6.  Giảm `size`.
  * **Overhead bộ nhớ:** Mỗi phần tử trong `LinkedList` cần thêm bộ nhớ cho đối tượng `Node` và hai con trỏ `next`, `prev`, làm cho nó tốn bộ nhớ hơn `ArrayList` (chỉ tốn bộ nhớ cho mảng tham chiếu và có thể có khoảng trống chưa sử dụng).

### 3.3. Các Thao tác Cơ bản và Độ phức tạp Thời gian (Big O Notation)

| Thao tác                          | Phương thức ví dụ                      | Độ phức tạp | Giải thích                                                        |
| :-------------------------------- | :------------------------------------- | :---------- | :---------------------------------------------------------------- |
| **Thêm vào đầu**                  | `addFirst(e)`, `add(0, e)`             | **O(1)**    | Chỉ cần cập nhật con trỏ `first` và node liền kề.                 |
| **Thêm vào cuối**                 | `addLast(e)`, `add(e)`                 | **O(1)**    | Chỉ cần cập nhật con trỏ `last` và node liền kề.                  |
| **Xóa khỏi đầu**                  | `removeFirst()`, `remove(0)`           | **O(1)**    | Chỉ cần cập nhật con trỏ `first` và node liền kề.                 |
| **Xóa khỏi cuối**                 | `removeLast()`, `remove(size-1)`       | **O(1)**    | Chỉ cần cập nhật con trỏ `last` và node liền kề.                  |
| Truy cập/Cập nhật theo chỉ số     | `get(index)`, `set(index, e)`          | O(n)        | Phải duyệt từ đầu hoặc cuối đến `index`.                          |
| Thêm vào giữa (theo index)        | `add(index, e)`                        | O(n)        | O(n) để tìm vị trí, O(1) để chèn.                                 |
| Xóa khỏi giữa (theo index/object) | `remove(index)`, `remove(o)`           | O(n)        | O(n) để tìm vị trí/đối tượng, O(1) để xóa liên kết.               |
| Tìm kiếm (tuần tự)                | `contains(o)`, `indexOf(o)`            | O(n)        | Phải duyệt qua các node.                                          |
| Lấy kích thước                    | `size()`                               | O(1)        | Trả về giá trị biến `size`.                                       |
| Kiểm tra rỗng                     | `isEmpty()`                            | O(1)        | Kiểm tra `size == 0`.                                             |
| **Thêm/Xóa qua `ListIterator`**   | `iterator.add(e)`, `iterator.remove()` | **O(1)**    | Khi iterator đã ở đúng vị trí, thao tác chỉ cần cập nhật con trỏ. |

### 3.4. Ví dụ Mã nguồn Sử dụng `LinkedList`

```java
import java.util.LinkedList;
import java.util.List;
import java.util.Deque; // LinkedList implements Deque
import java.util.ListIterator;

public class LinkedListExample {
    public static void main(String[] args) {
        // 1. Khởi tạo LinkedList (có thể dùng List hoặc LinkedList)
        LinkedList<String> stations = new LinkedList<>();

        // 2. Thêm phần tử (hiệu quả ở đầu/cuối) - O(1)
        stations.addFirst("Ga Hà Nội");
        stations.addLast("Ga Sài Gòn");
        stations.add("Ga Đà Nẵng"); // Tương đương addLast
        stations.add(1, "Ga Huế"); // Thêm vào giữa - O(n)
        System.out.println("Danh sách ga: " + stations); // Output: [Ga Hà Nội, Ga Huế, Ga Đà Nẵng, Ga Sài Gòn]

        // 3. Truy cập phần tử - O(n)
        System.out.println("Ga đầu tiên: " + stations.getFirst()); // Output: Ga Hà Nội (O(1))
        System.out.println("Ga cuối cùng: " + stations.getLast()); // Output: Ga Sài Gòn (O(1))
        System.out.println("Ga tại index 2: " + stations.get(2)); // Output: Ga Đà Nẵng (O(n))

        // 4. Xóa phần tử - O(1) ở đầu/cuối, O(n) ở giữa
        stations.removeFirst();
        stations.removeLast();
        System.out.println("Sau khi xóa đầu/cuối: " + stations); // Output: [Ga Huế, Ga Đà Nẵng]

        // 5. Sử dụng như Queue/Deque (LinkedList implements Deque)
        Deque<String> taskQueue = new LinkedList<>();
        taskQueue.offer("Task 1"); // Thêm vào cuối (đuôi queue) - O(1)
        taskQueue.offer("Task 2");
        taskQueue.offerFirst("Urgent Task"); // Thêm vào đầu (đầu deque) - O(1)

        System.out.println("\nHàng đợi task: " + taskQueue);
        System.out.println("Xem task đầu tiên: " + taskQueue.peek()); // Lấy nhưng không xóa - O(1)
        System.out.println("Xử lý task: " + taskQueue.poll()); // Lấy và xóa task đầu tiên - O(1)
        System.out.println("Hàng đợi sau khi xử lý: " + taskQueue);

        // 6. Duyệt và thao tác với ListIterator
        System.out.println("\nDuyệt và sửa đổi bằng ListIterator:");
        ListIterator<String> listIterator = stations.listIterator();
        while (listIterator.hasNext()) {
            String station = listIterator.next();
            System.out.print(station + " ");
            if (station.equals("Ga Huế")) {
                listIterator.remove(); // Xóa "Ga Huế" - O(1)
                listIterator.add("Ga Vinh"); // Thêm "Ga Vinh" vào vị trí con trỏ - O(1)
            } else if (station.equals("Ga Đà Nẵng")) {
                listIterator.set("Ga Trung tâm Đà Nẵng"); // Sửa đổi - O(1)
            }
        }
        System.out.println("\nSau khi sửa đổi: " + stations);
        // Output:
        // Ga Huế Ga Đà Nẵng
        // Sau khi sửa đổi: [Ga Vinh, Ga Trung tâm Đà Nẵng]
    }
}
```

### 3.5. Khi nào nên sử dụng `LinkedList`? (Use Cases)

`LinkedList` là lựa chọn phù hợp khi:

  * **Thêm/Xóa thường xuyên ở đầu hoặc cuối danh sách:** Đây là điểm mạnh nhất của `LinkedList` với hiệu năng O(1).
  * **Triển khai `Queue` hoặc `Deque`:** `LinkedList` cung cấp sẵn các phương thức của interface `Deque` (`offer`, `poll`, `peek`, `addFirst`, `removeLast`,...) một cách hiệu quả.
  * **Cần thêm/xóa phần tử trong quá trình duyệt:** Sử dụng `ListIterator` cho phép thêm/xóa phần tử với độ phức tạp O(1) tại vị trí hiện tại của iterator.
  * **Không yêu cầu truy cập ngẫu nhiên theo index thường xuyên:** Thao tác `get(index)` và `set(index, element)` trên `LinkedList` khá chậm.

## 4\. So sánh `ArrayList` và `LinkedList`

Việc lựa chọn giữa `ArrayList` và `LinkedList` phụ thuộc chủ yếu vào các thao tác bạn dự định thực hiện thường xuyên nhất trên danh sách.

### 4.1. Bảng so sánh chi tiết

| Đặc điểm                          | `ArrayList`                                        | `LinkedList`                                       |
| :-------------------------------- | :------------------------------------------------- | :------------------------------------------------- |
| **Cấu trúc lưu trữ**              | Mảng động (`Object[]`)                             | Danh sách liên kết đôi (`Node` với `prev`, `next`) |
| **`get(index)`**                  | O(1)                                               | O(n)                                               |
| **`set(index, E)`**               | O(1)                                               | O(n)                                               |
| **`add(E)` (addLast)**            | O(1) (amortized)                                   | O(1)                                               |
| **`add(0, E)` (addFirst)**        | O(n)                                               | O(1)                                               |
| **`add(index, E)` (middle)**      | O(n)                                               | O(n)                                               |
| **`remove(size-1)` (removeLast)** | O(1)                                               | O(1)                                               |
| **`remove(0)` (removeFirst)**     | O(n)                                               | O(1)                                               |
| **`remove(index)` (middle)**      | O(n)                                               | O(n)                                               |
| **`remove(Object o)`**            | O(n)                                               | O(n)                                               |
| **`contains(Object o)`**          | O(n)                                               | O(n)                                               |
| **`ListIterator.add/remove`**     | O(n) (do dịch chuyển phần tử)                      | O(1) (chỉ cập nhật con trỏ)                        |
| **Sử dụng bộ nhớ**                | Thấp hơn (overhead mảng), có thể có vùng nhớ trống | Cao hơn (overhead cho mỗi `Node` và 2 con trỏ)     |
| **Cache Locality**                | Tốt (các phần tử liền kề trong bộ nhớ)             | Kém (các `Node` phân tán trong bộ nhớ)             |

### 4.2. Yếu tố Hiệu năng (Performance Considerations)

  * **Cache Locality:** CPU hiện đại đọc bộ nhớ theo các khối (cache lines). Do các phần tử của `ArrayList` nằm liền kề nhau trong bộ nhớ, việc duyệt `ArrayList` thường nhanh hơn `LinkedList` vì tận dụng tốt cơ chế cache của CPU (cache locality). Các `Node` của `LinkedList` có thể nằm rải rác trong bộ nhớ, gây ra cache miss thường xuyên hơn khi duyệt.
  * **Memory Overhead:** `LinkedList` tốn nhiều bộ nhớ hơn cho cùng số lượng phần tử do phải lưu thêm hai con trỏ (`prev`, `next`) và overhead của đối tượng `Node` cho mỗi phần tử. `ArrayList` chỉ có overhead của mảng tham chiếu và có thể có phần tử `null` ở cuối mảng nếu `capacity > size`.

### 4.3. Lựa chọn cấu trúc phù hợp (Decision Guide)

  * **Ưu tiên `ArrayList` trong hầu hết các trường hợp** trừ khi bạn có lý do rõ ràng để chọn `LinkedList`. `ArrayList` thường có hiệu năng tổng thể tốt hơn cho các tác vụ phổ biến.
  * **Chọn `LinkedList` nếu:**
      * Ứng dụng của bạn thực hiện **rất nhiều** thao tác thêm/xóa ở **đầu hoặc cuối** danh sách.
      * Bạn cần sử dụng danh sách như một **`Queue` hoặc `Deque`** hiệu quả.
      * Bạn thường xuyên **thêm/xóa phần tử trong khi duyệt** bằng `ListIterator`.
  * **Đo lường (Benchmark):** Nếu hiệu năng là yếu tố cực kỳ quan trọng, hãy xem xét việc đo lường hiệu năng thực tế của cả hai cấu trúc với dữ liệu và mẫu truy cập (access patterns) cụ thể của ứng dụng bạn.

## 5\. Duyệt Danh sách và `Iterator` Pattern

Java cung cấp nhiều cách để duyệt qua các phần tử của một `List`.

### 5.1. Vòng lặp `for-each` (Enhanced for loop)

Đây là cách duyệt phổ biến, ngắn gọn và dễ đọc nhất. Bên dưới, nó sử dụng `Iterator`.

```java
List<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob", "Charlie"));
for (String name : names) {
    System.out.println(name);
    // KHÔNG NÊN sửa đổi danh sách ở đây (vd: names.remove(name))
    // Sẽ gây ra ConcurrentModificationException
}
```

### 5.2. `Iterator` Interface

Cung cấp cách chuẩn để duyệt qua một collection. An toàn để xóa phần tử hiện tại trong lúc duyệt.

  * `boolean hasNext()`: Kiểm tra xem còn phần tử tiếp theo không.
  * `E next()`: Trả về phần tử tiếp theo và di chuyển con trỏ.
  * `void remove()`: Xóa phần tử *cuối cùng* được trả về bởi `next()` khỏi collection gốc. Chỉ được gọi một lần sau mỗi lần gọi `next()`.

<!-- end list -->

```java
Iterator<String> iterator = names.iterator();
while (iterator.hasNext()) {
    String name = iterator.next();
    if (name.equals("Bob")) {
        iterator.remove(); // Xóa "Bob" khỏi danh sách names một cách an toàn
    }
}
System.out.println("Names after removing Bob: " + names); // Output: [Alice, Charlie]
```

  * **Fail-fast Behavior:** Các `Iterator` trả về bởi `ArrayList` và `LinkedList` là *fail-fast*. Nếu cấu trúc của danh sách bị sửa đổi (thêm/xóa phần tử) bởi một cơ chế khác ngoài phương thức `remove()` của chính `Iterator` đó trong khi đang duyệt, `Iterator` sẽ ném ra `ConcurrentModificationException` ngay khi có thể (thường là ở lần gọi `next()` hoặc `hasNext()` tiếp theo).

### 5.3. `ListIterator` Interface

Mở rộng `Iterator`, cung cấp thêm khả năng duyệt hai chiều và sửa đổi danh sách mạnh mẽ hơn. Chỉ dành riêng cho `List`.

  * Kế thừa các phương thức của `Iterator`.
  * Thêm các phương thức duyệt ngược:
      * `boolean hasPrevious()`: Kiểm tra còn phần tử phía trước không.
      * `E previous()`: Trả về phần tử phía trước và di chuyển con trỏ lùi.
  * Thêm thông tin vị trí:
      * `int nextIndex()`: Trả về chỉ số của phần tử sẽ được trả về bởi `next()`.
      * `int previousIndex()`: Trả về chỉ số của phần tử sẽ được trả về bởi `previous()`.
  * Thêm các phương thức sửa đổi:
      * `void add(E e)`: Chèn phần tử `e` vào danh sách tại vị trí *trước* phần tử sẽ được trả về bởi `next()` (hoặc cuối danh sách nếu đang ở cuối). An toàn trong lúc duyệt.
      * `void set(E e)`: Thay thế phần tử *cuối cùng* được trả về bởi `next()` hoặc `previous()` bằng `e`.

<!-- end list -->

```java
ListIterator<String> listIterator = names.listIterator();
while (listIterator.hasNext()) {
    String name = listIterator.next();
    if (name.equals("Alice")) {
        listIterator.set("Alicia"); // Đổi tên
    } else if (name.equals("Charlie")) {
        listIterator.add("David"); // Thêm David trước phần tử tiếp theo (nếu có) hoặc cuối
    }
}
// names bây giờ là [Alicia, David, Charlie] (nếu chạy tiếp từ ví dụ trước)

// Duyệt ngược
System.out.println("Duyệt ngược:");
while (listIterator.hasPrevious()) {
    System.out.println(listIterator.previous());
}
```

### 5.4. Lựa chọn cách duyệt

  * **`for-each`:** Đơn giản nhất, dễ đọc, phù hợp khi chỉ cần đọc các phần tử.
  * **`Iterator`:** Khi cần xóa phần tử hiện tại trong lúc duyệt.
  * **`ListIterator`:** Khi cần duyệt hai chiều, lấy chỉ số, hoặc thêm/sửa đổi phần tử trong lúc duyệt.

## 6\. Generics và Type Safety trong Danh sách

**Generics** là một tính năng cực kỳ quan trọng trong Java, đặc biệt khi làm việc với Collections, giúp đảm bảo **an toàn kiểu (type safety)** tại thời điểm biên dịch.

### 6.1. Tại sao cần Generics?

Trước Java 5 (khi Generics chưa ra đời), các Collection như `ArrayList` lưu trữ mọi thứ dưới dạng `Object`.

```java
// Code kiểu cũ (không generic) - KHÔNG NÊN DÙNG
List numbers = new ArrayList();
numbers.add(1); // autoboxing thành Integer
numbers.add(2);
numbers.add("Lỗi!"); // Biên dịch vẫn OK!

// Cần ép kiểu khi lấy ra
for (int i = 0; i < numbers.size(); i++) {
    try {
        Integer num = (Integer) numbers.get(i); // Phải ép kiểu thủ công
        System.out.println(num * 2);
    } catch (ClassCastException e) {
        System.err.println("Lỗi ép kiểu tại index " + i + ": " + e.getMessage());
        // Lỗi chỉ phát hiện được tại RUNTIME khi gặp "Lỗi!"
    }
}
```

Vấn đề:

1.  **Thiếu Type Safety:** Có thể vô tình thêm sai kiểu dữ liệu vào danh sách mà trình biên dịch không báo lỗi.
2.  **Cần ép kiểu thủ công:** Phải ép kiểu (cast) khi lấy phần tử ra, dễ xảy ra `ClassCastException` tại runtime.
3.  **Code dài dòng và kém an toàn.**

### 6.2. Sử dụng Generics với `List`

Generics giải quyết các vấn đề trên bằng cách cho phép bạn khai báo kiểu dữ liệu cụ thể mà danh sách sẽ chứa.

```java
// Code sử dụng Generics (Tốt!)
List<Integer> numbers = new ArrayList<>(); // Khai báo chỉ chứa Integer

numbers.add(1);
numbers.add(2);
// numbers.add("Lỗi!"); // LỖI BIÊN DỊCH! Trình biên dịch báo lỗi ngay lập tức.

// Không cần ép kiểu khi lấy ra
for (Integer num : numbers) { // Duyệt bằng for-each tiện lợi
    System.out.println(num * 2); // Trình biên dịch đảm bảo num là Integer
}
```

Lợi ích:

1.  **Type Safety:** Lỗi kiểu dữ liệu được phát hiện tại thời điểm biên dịch.
2.  **Không cần ép kiểu:** Code gọn gàng và an toàn hơn.
3.  **Tăng khả năng đọc hiểu:** Khai báo `List<Integer>` rõ ràng hơn `List`.

### 6.3. Bounded Types (Wildcards) - Nâng cao

Wildcards (`?`) cung cấp sự linh hoạt hơn khi làm việc với Generics, đặc biệt là trong các tham số phương thức.

  * **Upper Bounded Wildcards (`<? extends Type>`):**

      * Ý nghĩa: "Một kiểu bất kỳ kế thừa từ `Type` (hoặc chính `Type`)".
      * Sử dụng: Cho phép đọc các phần tử từ collection (chúng đều là `Type`), nhưng **không cho phép thêm** phần tử mới (trừ `null`) vì không biết chính xác kiểu cụ thể là gì.
      * Thường dùng cho các tham số hoạt động như **Producer** (cung cấp dữ liệu). Nguyên tắc **PECS (Producer Extends, Consumer Super)**.
      * Ví dụ: Tính tổng các số trong danh sách `Number` hoặc subclass của nó (`Integer`, `Double`).
        ```java
        public static double sumOfList(List<? extends Number> list) {
            double sum = 0.0;
            for (Number n : list) { // Có thể đọc ra Number
                sum += n.doubleValue();
            }
            // list.add(1); // LỖI BIÊN DỊCH! Không thể thêm vào <? extends ...>
            return sum;
        }
        // Có thể gọi với List<Integer>, List<Double>, List<Number>
        List<Integer> ints = Arrays.asList(1, 2, 3);
        System.out.println("Sum = " + sumOfList(ints));
        ```

  * **Lower Bounded Wildcards (`<? super Type>`):**

      * Ý nghĩa: "Một kiểu bất kỳ là lớp cha của `Type` (hoặc chính `Type`)".
      * Sử dụng: Cho phép **thêm** các phần tử kiểu `Type` (hoặc subtype của `Type`) vào collection, nhưng khi **đọc** ra chỉ đảm bảo kiểu là `Object`.
      * Thường dùng cho các tham số hoạt động như **Consumer** (nhận dữ liệu). Nguyên tắc **PECS**.
      * Ví dụ: Thêm các số nguyên vào một danh sách có thể chứa `Integer` hoặc lớp cha của nó (`Number`, `Object`).
        ```java
        public static void addIntegers(List<? super Integer> list, int count) {
            for (int i = 1; i <= count; i++) {
                list.add(i); // Có thể thêm Integer (hoặc subtype của Integer - không có)
            }
            // Object obj = list.get(0); // Chỉ đọc ra được Object một cách an toàn
        }
        // Có thể gọi với List<Integer>, List<Number>, List<Object>
        List<Number> nums = new ArrayList<>();
        addIntegers(nums, 5);
        System.out.println("Nums: " + nums); // Output: [1, 2, 3, 4, 5]
        ```

  * **Unbounded Wildcards (`<?>`):**

      * Ý nghĩa: "Một kiểu bất kỳ không xác định". Tương đương `<? extends Object>`.
      * Sử dụng: Khi kiểu dữ liệu không quan trọng, ví dụ phương thức chỉ cần biết `size()` của list hoặc gọi `clear()`. Chỉ có thể đọc ra `Object` và chỉ thêm được `null`.
        ```java
        public static void printListSize(List<?> list) {
            System.out.println("Size: " + list.size());
            // String s = list.get(0); // LỖI BIÊN DỊCH!
            // list.add("abc"); // LỖI BIÊN DỊCH!
        }
        ```

## 7\. Các Vấn đề Nâng cao và Thực tiễn Tốt nhất

### 7.1. Đồng bộ hóa (Synchronization)

  * **Vấn đề:** `ArrayList` và `LinkedList` **không thread-safe**. Nếu nhiều luồng cùng truy cập và sửa đổi một instance của chúng, có thể xảy ra lỗi dữ liệu hoặc `ConcurrentModificationException`.
  * **Giải pháp:**
      * **Wrapper `Collections.synchronizedList()`:**
        ```java
        List<String> syncList = Collections.synchronizedList(new ArrayList<>());
        // Các thao tác trên syncList sẽ được đồng bộ hóa
        // Lưu ý: Việc duyệt vẫn cần khối synchronized thủ công trên syncList
        synchronized (syncList) {
            Iterator<String> i = syncList.iterator(); // Phải nằm trong khối synchronized
            while (i.hasNext()) {
                process(i.next());
            }
        }
        ```
        *Nhược điểm:* Hiệu năng kém do khóa (lock) toàn bộ danh sách cho mọi thao tác.
      * **Sử dụng Collections đồng bộ hóa sẵn có:**
          * `CopyOnWriteArrayList`: Thread-safe, tối ưu cho các trường hợp đọc nhiều, ghi ít. Khi có thao tác ghi (add, set, remove), nó tạo ra một bản sao mới của mảng bên trong. Việc đọc không cần khóa và luôn diễn ra trên bản sao ổn định. *Nhược điểm:* Tốn bộ nhớ khi ghi, thao tác ghi chậm.
          * `ConcurrentLinkedQueue` / `ConcurrentLinkedDeque`: Triển khai Queue/Deque thread-safe hiệu quả dựa trên danh sách liên kết.

### 7.2. Tối ưu hóa `ArrayList`

  * **`initialCapacity`:** Nếu bạn có thể ước tính kích thước tối đa của danh sách, hãy khởi tạo `ArrayList` với capacity đó: `new ArrayList<>(estimatedSize)`. Điều này giúp tránh hoặc giảm số lần phải thực hiện thao tác resize mảng tốn kém.
  * **`trimToSize()`:** Nếu danh sách đã đạt kích thước cuối cùng và không còn thêm phần tử nữa, gọi `list.trimToSize()` để giải phóng bộ nhớ thừa của mảng bên trong (phần capacity không sử dụng).

### 7.3. Các Lỗi thường gặp (Common Pitfalls)

  * **`ConcurrentModificationException`:** Sửa đổi cấu trúc danh sách (add/remove) trong khi duyệt bằng `for-each` hoặc `Iterator` (ngoại trừ dùng `iterator.remove()`). -\> Sử dụng `Iterator.remove()` hoặc `ListIterator.add/remove/set` hoặc duyệt bằng index (nếu là `ArrayList` và cẩn thận với index).
  * **Chọn sai cấu trúc:** Dùng `ArrayList` cho các tác vụ thêm/xóa đầu liên tục, hoặc dùng `LinkedList` khi cần truy cập index thường xuyên. -\> Phân tích access pattern để chọn đúng.
  * **Hiệu năng `LinkedList.get(index)`:** Quên rằng `get(index)` trên `LinkedList` là O(n) và sử dụng nó trong vòng lặp -\> hiệu năng rất tệ.
  * **Không sử dụng Generics:** Gây mất type safety, cần ép kiểu, tiềm ẩn `ClassCastException` tại runtime. -\> Luôn sử dụng Generics.
  * **Sửa đổi `subList()` ảnh hưởng list gốc:** `list.subList()` trả về một *view*, không phải bản sao. Mọi thay đổi *phi cấu trúc* (như `set`) trên sublist sẽ ảnh hưởng đến list gốc và ngược lại. Thay đổi *cấu trúc* (add/remove) trên sublist cũng ảnh hưởng list gốc, nhưng thay đổi cấu trúc trên list gốc có thể làm sublist không hợp lệ (gây `ConcurrentModificationException`).

### 7.4. Phương pháp hay nhất (Best Practices)

  * **Lập trình dựa trên Interface:** Luôn khai báo biến bằng kiểu Interface (`List`) thay vì kiểu cụ thể (`ArrayList`, `LinkedList`), trừ khi bạn cần dùng các phương thức đặc thù của lớp cụ thể.
    ```java
    List<String> users = new ArrayList<>(); // Tốt
    // ArrayList<String> users = new ArrayList<>(); // Chỉ dùng nếu cần phương thức của ArrayList
    ```
    Điều này giúp code linh hoạt hơn, dễ dàng thay đổi implementation sau này.
  * **Sử dụng Generics:** Luôn dùng Generics để đảm bảo type safety và tránh ép kiểu.
  * **Chọn Implementation phù hợp:** Hiểu rõ ưu/nhược điểm và độ phức tạp của `ArrayList` và `LinkedList` để đưa ra lựa chọn tối ưu cho use case cụ thể. `ArrayList` thường là lựa chọn mặc định tốt.
  * **Cân nhắc Thread Safety:** Nếu danh sách được chia sẻ giữa nhiều luồng, hãy sử dụng `Collections.synchronizedList` hoặc các concurrent collections như `CopyOnWriteArrayList`.
  * **Sử dụng `Iterator`/`ListIterator` đúng cách:** Tận dụng khả năng xóa an toàn của `Iterator` và khả năng duyệt/sửa đổi hai chiều của `ListIterator`.
  * **Khởi tạo Capacity cho `ArrayList`:** Nếu biết trước kích thước, cung cấp `initialCapacity`.
  * **Cẩn thận với `subList()`:** Hiểu rằng nó là view, không phải bản sao.

## 8\. Kết luận

`List` là một interface cực kỳ quan trọng trong Java Collection Framework, với hai lớp triển khai phổ biến là `ArrayList` và `LinkedList`. `ArrayList`, với cấu trúc mảng động, cung cấp hiệu năng truy cập ngẫu nhiên O(1) và thường là lựa chọn mặc định hiệu quả. `LinkedList`, với cấu trúc liên kết đôi, tỏa sáng trong các tác vụ thêm/xóa ở hai đầu (O(1)) và khi hoạt động như một `Queue`/`Deque`.

Việc hiểu rõ cơ chế hoạt động bên trong, độ phức tạp thuật toán của các thao tác, và các trường hợp sử dụng điển hình của từng loại danh sách là chìa khóa để viết mã Java hiệu quả, dễ bảo trì và tối ưu về hiệu năng. Luôn cân nhắc các yêu cầu cụ thể của bài toán và áp dụng các phương pháp hay nhất khi làm việc với danh sách trong ứng dụng của bạn.