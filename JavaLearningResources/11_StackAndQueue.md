# STACK (NGĂN XẾP) VÀ QUEUE (HÀNG ĐỢI)

Tài liệu này cung cấp cái nhìn chuyên sâu về hai cấu trúc dữ liệu cơ bản và cực kỳ quan trọng trong khoa học máy tính và lập trình: **Stack (Ngăn xếp)** và **Queue (Hàng đợi)**. Chúng ta sẽ khám phá khái niệm, các cách triển khai phổ biến, ứng dụng thực tế, và những phương pháp tốt nhất khi làm việc với chúng, đặc biệt trong môi trường Java.

## 1\. Giới thiệu

Stack và Queue là các **Kiểu dữ liệu trừu tượng (Abstract Data Types - ADTs)** tuyến tính, định nghĩa một tập hợp các thao tác logic trên một tập hợp dữ liệu có thứ tự mà không cần quan tâm đến cách triển khai cụ thể bên dưới. Sự khác biệt cốt lõi nằm ở **nguyên tắc truy cập và quản lý phần tử**:

  * **Stack:** Tuân theo nguyên tắc **LIFO (Last-In, First-Out)** - Phần tử được thêm vào sau cùng sẽ là phần tử được lấy ra đầu tiên. Hãy tưởng tượng một chồng đĩa, bạn chỉ có thể đặt thêm đĩa lên trên cùng và lấy ra chiếc đĩa trên cùng.
  * **Queue:** Tuân theo nguyên tắc **FIFO (First-In, First-Out)** - Phần tử được thêm vào đầu tiên sẽ là phần tử được lấy ra đầu tiên. Giống như một hàng người đang chờ đợi, người đến trước sẽ được phục vụ trước.

Việc hiểu rõ và sử dụng thành thạo Stack và Queue là nền tảng thiết yếu cho mọi lập trình viên.

## 2\. Stack (Ngăn xếp)

### 2.1. Khái niệm Cốt lõi (LIFO)

Như đã đề cập, Stack hoạt động theo nguyên tắc **Last-In, First-Out**. Mọi thao tác thêm (push) và xóa (pop) đều diễn ra ở một đầu duy nhất của cấu trúc, thường được gọi là **đỉnh (top)** của stack.

### 2.2. Các Thao tác Cơ bản (ADT Operations)

Một ADT Stack điển hình định nghĩa các thao tác sau:

  * `push(element)`: Thêm một phần tử `element` vào đỉnh stack.
  * `pop()`: Loại bỏ và trả về phần tử ở đỉnh stack. Nếu stack rỗng, hành vi có thể là ném ra lỗi (exception) hoặc trả về một giá trị đặc biệt (như `null`).
  * `peek()` (hoặc `top()`): Trả về phần tử ở đỉnh stack mà **không** loại bỏ nó. Nếu stack rỗng, hành vi tương tự như `pop()`.
  * `isEmpty()`: Kiểm tra xem stack có rỗng hay không. Trả về `true` nếu rỗng, `false` nếu ngược lại.
  * `size()`: Trả về số lượng phần tử hiện có trong stack.

### 2.3. Các Cách Triển khai Phổ biến

Stack có thể được triển khai bằng nhiều cấu trúc dữ liệu cơ bản khác nhau.

#### 2.3.1. Triển khai bằng Mảng (Array-based Implementation)

Đây là một cách tiếp cận phổ biến, sử dụng một mảng để lưu trữ các phần tử. Một biến (ví dụ: `top` hoặc `count`) được dùng để theo dõi chỉ số của phần tử trên cùng.

  * **Push:** Tăng biến `top` lên 1 và gán phần tử mới vào vị trí `array[top]`.
  * **Pop:** Trả về phần tử tại `array[top]`, sau đó giảm `top` đi 1.
  * **Peek:** Trả về phần tử tại `array[top]` mà không thay đổi `top`.

**Vấn đề kích thước mảng:** Mảng có kích thước cố định. Nếu số lượng phần tử vượt quá kích thước ban đầu, cần phải thực hiện **mở rộng mảng (dynamic resizing)**. Điều này thường bao gồm việc tạo một mảng mới lớn hơn (thường gấp đôi kích thước cũ), sao chép tất cả phần tử từ mảng cũ sang mảng mới, và sau đó cập nhật tham chiếu để trỏ đến mảng mới.

**Ví dụ Mã nguồn (Java):**

```java
import java.util.Arrays;
import java.util.EmptyStackException;

public class ArrayStack<E> {
    private static final int DEFAULT_CAPACITY = 10;
    private Object[] elements; // Sử dụng Object[] và ép kiểu khi lấy ra
    private int size = 0; // Số lượng phần tử thực tế

    public ArrayStack() {
        elements = new Object[DEFAULT_CAPACITY];
    }

    public ArrayStack(int initialCapacity) {
        if (initialCapacity <= 0) {
            throw new IllegalArgumentException("Initial capacity must be positive");
        }
        elements = new Object[initialCapacity];
    }

    // Thêm phần tử vào đỉnh stack
    public void push(E element) {
        ensureCapacity(); // Đảm bảo đủ chỗ trước khi thêm
        elements[size++] = element;
    }

    // Lấy và xóa phần tử khỏi đỉnh stack
    @SuppressWarnings("unchecked") // Bỏ qua cảnh báo ép kiểu không an toàn
    public E pop() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        // Lấy phần tử trên cùng
        E element = (E) elements[size - 1];
        // Giúp GC dọn dẹp, tránh memory leak nếu E là kiểu tham chiếu
        elements[size - 1] = null;
        size--;
        // Có thể cân nhắc giảm kích thước mảng nếu size quá nhỏ so với capacity
        // shrinkIfNeeded();
        return element;
    }

    // Xem phần tử ở đỉnh stack (không xóa)
    @SuppressWarnings("unchecked")
    public E peek() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        return (E) elements[size - 1];
    }

    // Kiểm tra stack rỗng
    public boolean isEmpty() {
        return size == 0;
    }

    // Lấy số lượng phần tử
    public int size() {
        return size;
    }

    // Đảm bảo mảng đủ sức chứa
    private void ensureCapacity() {
        if (size == elements.length) {
            int newCapacity = elements.length * 2; // Gấp đôi dung lượng
            // Hoặc có thể dùng: newCapacity = elements.length + (elements.length >> 1); // Tăng 50%
            elements = Arrays.copyOf(elements, newCapacity);
            // System.out.println("Resized to: " + newCapacity); // Để debug
        }
    }

    // (Tùy chọn) Giảm kích thước mảng nếu cần
    private void shrinkIfNeeded() {
        // Ví dụ: nếu size chỉ bằng 1/4 capacity và capacity lớn hơn mức mặc định
        if (size > DEFAULT_CAPACITY && size <= elements.length / 4) {
            int newCapacity = Math.max(DEFAULT_CAPACITY, elements.length / 2);
            elements = Arrays.copyOf(elements, newCapacity);
            // System.out.println("Shrank to: " + newCapacity); // Để debug
        }
    }

    @Override
    public String toString() {
        if (isEmpty()) {
            return "[]";
        }
        StringBuilder sb = new StringBuilder();
        sb.append("[");
        for (int i = 0; i < size - 1; i++) {
            sb.append(elements[i]).append(", ");
        }
        sb.append(elements[size - 1]).append("] (top)");
        return sb.toString();
    }

    public static void main(String[] args) {
        ArrayStack<String> bookStack = new ArrayStack<>(2); // Khởi tạo với capacity nhỏ để test resize
        System.out.println("Is empty? " + bookStack.isEmpty()); // true

        bookStack.push("Lập trình Java");
        bookStack.push("Cấu trúc dữ liệu");
        System.out.println("Stack: " + bookStack);
        System.out.println("Size: " + bookStack.size()); // 2
        System.out.println("Peek: " + bookStack.peek()); // Cấu trúc dữ liệu

        bookStack.push("Giải thuật"); // Sẽ trigger resize
        System.out.println("Stack after push: " + bookStack);
        System.out.println("Size: " + bookStack.size()); // 3

        String poppedBook = bookStack.pop();
        System.out.println("Popped: " + poppedBook); // Giải thuật
        System.out.println("Stack after pop: " + bookStack); // [Lập trình Java, Cấu trúc dữ liệu] (top)
        System.out.println("Size: " + bookStack.size()); // 2

        System.out.println("Popped: " + bookStack.pop()); // Cấu trúc dữ liệu
        System.out.println("Popped: " + bookStack.pop()); // Lập trình Java
        System.out.println("Is empty? " + bookStack.isEmpty()); // true

        // bookStack.pop(); // Sẽ ném ra EmptyStackException
    }
}
```

#### 2.3.2. Triển khai bằng Danh sách Liên kết (Linked List-based Implementation)

Sử dụng danh sách liên kết (đơn hoặc đôi) cũng là một cách hiệu quả. Thường thì **đầu (head)** của danh sách liên kết được chọn làm **đỉnh (top)** của stack.

  * **Push:** Thêm một node mới vào đầu danh sách. Thao tác này có độ phức tạp O(1).
  * **Pop:** Xóa node ở đầu danh sách và trả về giá trị của nó. Thao tác này cũng có độ phức tạp O(1).
  * **Peek:** Trả về giá trị của node ở đầu danh sách mà không xóa. Thao tác này có độ phức tạp O(1).

Ưu điểm so với mảng là không cần lo lắng về việc thay đổi kích thước (resizing), bộ nhớ được cấp phát động theo nhu cầu.

#### 2.3.3. Sử dụng các Lớp có sẵn trong Java

  * **`java.util.Stack<E>`:**

      * Đây là lớp cài đặt Stack cổ điển trong Java.
      * **Tuy nhiên, không nên sử dụng `java.util.Stack` trong các dự án mới.** Lý do chính là nó kế thừa từ `java.util.Vector`, một lớp collection cũ, được **đồng bộ hóa (synchronized)** tất cả các phương thức. Việc đồng bộ hóa này gây ra chi phí hiệu năng không cần thiết trong môi trường đơn luồng hoặc khi việc đồng bộ hóa được quản lý ở cấp độ cao hơn. Hơn nữa, việc kế thừa từ `Vector` cho phép truy cập các phần tử ở giữa stack bằng chỉ số (ví dụ `get(index)`), vi phạm nguyên tắc LIFO của Stack.
      * Các phương thức chính: `push()`, `pop()`, `peek()`, `empty()`, `search()`.

  * **`java.util.Deque<E>` (Recommended Interface):**

      * `Deque` (Double-Ended Queue) là một interface linh hoạt hơn, có thể hoạt động như cả Stack (LIFO) và Queue (FIFO).
      * Nó cung cấp các phương thức chuyên biệt cho hoạt động kiểu Stack: `push()` (tương đương `addFirst()`), `pop()` (tương đương `removeFirst()`), `peek()` (tương đương `peekFirst()`).
      * **`java.util.ArrayDeque<E>` (Recommended Implementation):**
          * Đây là lớp cài đặt `Deque` sử dụng **mảng động (resizable array)**.
          * Nó **không được đồng bộ hóa**, do đó nhanh hơn `Stack` trong môi trường đơn luồng.
          * Nó hiệu quả hơn `LinkedList` cho các thao tác ở cả hai đầu khi dùng làm Stack hoặc Queue.
          * **Đây là lựa chọn được khuyến nghị hàng đầu khi bạn cần một cài đặt Stack (LIFO) trong Java hiện đại.**
          * *Lưu ý:* `ArrayDeque` không cho phép lưu trữ phần tử `null`.

  * **`java.util.LinkedList<E>`:**

      * `LinkedList` cũng cài đặt interface `Deque`, nên nó cũng có thể được sử dụng như một Stack thông qua các phương thức `push()`, `pop()`, `peek()`.
      * Cho phép lưu trữ phần tử `null`.
      * Tuy nhiên, `ArrayDeque` thường có hiệu năng tốt hơn cho các thao tác stack do sử dụng mảng (cache locality tốt hơn) so với các node phân tán của `LinkedList`.

**Ví dụ sử dụng `ArrayDeque` làm Stack:**

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class ArrayDequeAsStack {
    public static void main(String[] args) {
        // Khai báo biến với kiểu interface Deque, khởi tạo bằng ArrayDeque
        Deque<Integer> numberStack = new ArrayDeque<>();

        // Sử dụng các phương thức của Deque như một Stack
        numberStack.push(10); // Thêm vào "đỉnh" (đầu deque)
        numberStack.push(20);
        numberStack.push(30);

        System.out.println("Stack: " + numberStack); // [30, 20, 10] (Đỉnh là 30)
        System.out.println("Peek: " + numberStack.peek()); // 30 (Xem đỉnh)

        int topElement = numberStack.pop(); // Lấy và xóa đỉnh
        System.out.println("Popped: " + topElement); // 30
        System.out.println("Stack after pop: " + numberStack); // [20, 10]

        System.out.println("Is empty? " + numberStack.isEmpty()); // false
        System.out.println("Size: " + numberStack.size()); // 2

        numberStack.pop();
        numberStack.pop();

        System.out.println("Is empty? " + numberStack.isEmpty()); // true

        // numberStack.pop(); // Sẽ ném NoSuchElementException nếu rỗng
        // System.out.println(numberStack.peek()); // Sẽ trả về null nếu rỗng
    }
}
```

### 2.4. Các Trường hợp Sử dụng (Use Cases) Thực tế

  * **Quản lý Lịch sử (Undo/Redo):** Trong các trình soạn thảo văn bản, phần mềm đồ họa, thao tác "Undo" lấy lại trạng thái trước đó bằng cách pop trạng thái hiện tại ra khỏi stack "undo". Thao tác "Redo" thì ngược lại, push trạng thái từ stack "redo" vào stack "undo".
  * **Nút "Back" trong Trình duyệt Web:** Mỗi URL bạn truy cập có thể được push vào một stack. Khi nhấn nút "Back", URL hiện tại được pop ra (và có thể push vào stack "forward").
  * **Call Stack (Ngăn xếp Lời gọi Hàm):** Đây là ứng dụng nền tảng trong thực thi chương trình. Mỗi khi một hàm được gọi, một "khung ngăn xếp" (stack frame) chứa thông tin về lời gọi đó (tham số, biến cục bộ, địa chỉ trả về) được push vào call stack. Khi hàm kết thúc, khung của nó được pop ra, và chương trình quay lại nơi nó được gọi. Hiểu về call stack rất quan trọng để gỡ lỗi (debugging), đặc biệt là lỗi tràn bộ nhớ stack (StackOverflowError).
  * **Đánh giá Biểu thức (Expression Evaluation):**
      * **Chuyển đổi Infix sang Postfix/Prefix:** Stack được dùng để tạm thời lưu trữ các toán tử và dấu ngoặc trong quá trình chuyển đổi biểu thức từ dạng trung tố (con người dễ đọc) sang hậu tố (máy tính dễ xử lý).
      * **Tính toán Biểu thức Postfix:** Duyệt qua biểu thức hậu tố, khi gặp toán hạng thì push vào stack, khi gặp toán tử thì pop hai toán hạng ra, thực hiện phép tính, rồi push kết quả trở lại stack.
  * **Thuật toán Duyệt Đồ thị theo Chiều sâu (Depth-First Search - DFS):** Mặc dù DFS thường được cài đặt đệ quy (sử dụng call stack ngầm), phiên bản khử đệ quy (iterative) của DFS sử dụng một stack tường minh để lưu trữ các đỉnh cần thăm.
  * **Thuật toán Quay lui (Backtracking):** Các thuật toán giải quyết vấn đề bằng cách thử các lựa chọn và quay lui nếu lựa chọn đó không dẫn đến giải pháp (ví dụ: giải mê cung, bài toán N-Queens) thường sử dụng stack (có thể là call stack của đệ quy hoặc stack tường minh) để ghi nhớ các trạng thái/lựa chọn đã thử.
  * **Kiểm tra Cú pháp (Syntax Parsing):** Ví dụ kiểm tra tính hợp lệ của dấu ngoặc trong biểu thức: `{ [ ( ) ] }`. Khi gặp dấu mở, push vào stack. Khi gặp dấu đóng, pop từ stack và kiểm tra xem có khớp với dấu mở tương ứng không.

### 2.5. Mẹo Thực chiến & Lỗi Phổ biến

  * **Luôn kiểm tra rỗng trước khi `pop()` hoặc `peek()`:** Đây là lỗi phổ biến nhất, dẫn đến `EmptyStackException` (với `java.util.Stack`) hoặc `NoSuchElementException` (với `ArrayDeque.pop()`) hoặc trả về `null` (với `ArrayDeque.peek()`) có thể gây `NullPointerException` sau đó nếu không xử lý.
    ```java
    if (!myStack.isEmpty()) {
        Element top = myStack.pop();
        // process top
    }
    ```
  * **Cẩn thận với `java.util.Stack`:** Như đã giải thích, hãy ưu tiên sử dụng `Deque` và `ArrayDeque` vì lý do hiệu năng và thiết kế.
  * **Tràn bộ nhớ Stack (StackOverflowError):** Thường xảy ra với các lời gọi đệ quy quá sâu (vượt quá dung lượng của call stack) hoặc khi push quá nhiều phần tử vào một stack tường minh mà không có giới hạn. Cần xem xét lại logic đệ quy hoặc sử dụng phiên bản khử đệ quy nếu cần.
  * **Memory Leak trong Array-based Stack:** Khi pop một đối tượng khỏi stack cài đặt bằng mảng, nên gán `null` cho vị trí đó trong mảng (`elements[size - 1] = null;`). Điều này giúp bộ thu gom rác (Garbage Collector - GC) có thể thu hồi bộ nhớ của đối tượng đó nếu không còn tham chiếu nào khác trỏ tới nó.
  * **Hiệu năng:** Thao tác `push`, `pop`, `peek` trên `ArrayDeque` và `LinkedList` đều có độ phức tạp thời gian trung bình là O(1). Tuy nhiên, `ArrayDeque` thường nhanh hơn do tận dụng tốt hơn bộ nhớ cache của CPU. Việc thay đổi kích thước của `ArrayDeque` (resizing) có độ phức tạp O(n) nhưng xảy ra không thường xuyên (amortized O(1)).

### 2.6. Best Practices

  * **Khai báo bằng Interface:** Sử dụng interface `Deque` thay vì lớp cụ thể `ArrayDeque` hoặc `LinkedList` khi khai báo biến. Điều này giúp mã nguồn linh hoạt hơn, dễ dàng thay đổi cách triển khai sau này nếu cần.
    ```java
    Deque<String> names = new ArrayDeque<>(); // Tốt
    // ArrayDeque<String> names = new ArrayDeque<>(); // Ít linh hoạt hơn
    ```
  * **Chọn Cài đặt Phù hợp:**
      * Ưu tiên `ArrayDeque` cho hầu hết các trường hợp cần Stack trong Java hiện đại.
      * Sử dụng `LinkedList` nếu bạn cần lưu trữ giá trị `null` (hiếm khi cần thiết trong stack).
      * Tránh `java.util.Stack`.
  * **Xem xét Giới hạn Kích thước (Bounded Stack):** Nếu bạn biết trước giới hạn tối đa số phần tử hoặc muốn ngăn chặn việc sử dụng bộ nhớ quá mức, bạn có thể cài đặt một stack có giới hạn kích thước (bounded stack) và xử lý trường hợp stack đầy (ví dụ: ném lỗi, ghi đè phần tử cũ nhất).

## 3\. Queue (Hàng đợi)

### 3.1. Khái niệm Cốt lõi (FIFO)

Queue hoạt động theo nguyên tắc **First-In, First-Out**. Phần tử được thêm vào ở một đầu (thường gọi là **đuôi - rear** hoặc **tail**) và được lấy ra từ đầu kia (thường gọi là **đầu - front** hoặc **head**).

### 3.2. Các Thao tác Cơ bản (ADT Operations)

Một ADT Queue điển hình định nghĩa các thao tác:

  * `enqueue(element)` (hoặc `add`, `offer`): Thêm một phần tử `element` vào cuối hàng đợi (rear/tail).
  * `dequeue()` (hoặc `remove`, `poll`): Loại bỏ và trả về phần tử ở đầu hàng đợi (front/head). Nếu queue rỗng, hành vi có thể là ném lỗi hoặc trả về giá trị đặc biệt.
  * `peek()` (hoặc `element`, `front`): Trả về phần tử ở đầu hàng đợi mà **không** loại bỏ nó. Nếu queue rỗng, hành vi tương tự như `dequeue()`.
  * `isEmpty()`: Kiểm tra xem queue có rỗng hay không.
  * `size()`: Trả về số lượng phần tử hiện có trong queue.

### 3.3. Các Cách Triển khai Phổ biến

#### 3.3.1. Triển khai bằng Danh sách Liên kết (Linked List-based Implementation)

Đây là cách triển khai tự nhiên và phổ biến cho Queue. Cần duy trì hai con trỏ: `front` (trỏ đến node đầu) và `rear` (trỏ đến node cuối).

  * **Enqueue:** Tạo một node mới, thêm vào sau node `rear`, và cập nhật `rear` để trỏ đến node mới này. Nếu queue rỗng, cả `front` và `rear` đều trỏ đến node mới. Thao tác này là O(1).
  * **Dequeue:** Lấy giá trị từ node `front`, cập nhật `front` để trỏ đến node tiếp theo (`front.next`). Nếu sau khi xóa, queue trở nên rỗng, cần cập nhật cả `rear` thành `null`. Thao tác này là O(1).
  * **Peek:** Trả về giá trị của node `front`. Thao tác này là O(1).

**Ví dụ Mã nguồn (Java - Tương tự slide):**

```java
import java.util.LinkedList; // Sử dụng LinkedList có sẵn của Java
import java.util.NoSuchElementException;

public class LinkedListQueue<E> {
    // Sử dụng LinkedList làm cấu trúc lưu trữ nền tảng
    // LinkedList hỗ trợ hiệu quả việc thêm/xóa ở cả hai đầu
    private LinkedList<E> list = new LinkedList<>();

    // Thêm phần tử vào cuối hàng đợi (rear)
    public void enqueue(E element) {
        list.addLast(element); // addLast là O(1) cho LinkedList
    }

    // Lấy và xóa phần tử khỏi đầu hàng đợi (front)
    public E dequeue() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        return list.removeFirst(); // removeFirst là O(1) cho LinkedList
    }

    // Xem phần tử ở đầu hàng đợi (không xóa)
    public E peek() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        return list.getFirst(); // getFirst là O(1) cho LinkedList
    }

    // Kiểm tra hàng đợi rỗng
    public boolean isEmpty() {
        return list.isEmpty();
    }

    // Lấy số lượng phần tử
    public int size() {
        return list.size();
    }

    @Override
    public String toString() {
       return "Queue (front): " + list.toString();
    }

    public static void main(String[] args) {
        LinkedListQueue<String> customerQueue = new LinkedListQueue<>();

        customerQueue.enqueue("Alice");
        customerQueue.enqueue("Bob");
        customerQueue.enqueue("Charlie");

        System.out.println(customerQueue); // Queue (front): [Alice, Bob, Charlie]
        System.out.println("Size: " + customerQueue.size()); // 3
        System.out.println("Front: " + customerQueue.peek()); // Alice

        String served = customerQueue.dequeue();
        System.out.println("Served: " + served); // Alice
        System.out.println("Queue after dequeue: " + customerQueue); // Queue (front): [Bob, Charlie]

        System.out.println("Served: " + customerQueue.dequeue()); // Bob
        System.out.println("Served: " + customerQueue.dequeue()); // Charlie

        System.out.println("Is empty? " + customerQueue.isEmpty()); // true

        // customerQueue.dequeue(); // Sẽ ném NoSuchElementException
    }
}
```

#### 3.3.2. Triển khai bằng Mảng (Array-based Implementation - Circular Buffer)

Triển khai Queue bằng mảng đơn giản (luôn thêm vào cuối và xóa ở đầu) sẽ không hiệu quả, vì việc xóa ở đầu (vị trí 0) đòi hỏi phải dịch chuyển tất cả các phần tử còn lại sang trái (O(n)), làm chậm thao tác `dequeue`.

Giải pháp hiệu quả hơn là sử dụng **Mảng Vòng (Circular Array / Ring Buffer)**. Chúng ta sử dụng hai chỉ số (index): `front` (chỉ vị trí của phần tử đầu tiên) và `rear` (chỉ vị trí *tiếp theo* sẽ được thêm phần tử vào).

  * **Enqueue:** Thêm phần tử vào `array[rear]`, sau đó tăng `rear` lên 1. Nếu `rear` đạt đến cuối mảng, nó sẽ "quay vòng" về 0.
  * **Dequeue:** Lấy phần tử từ `array[front]`, sau đó tăng `front` lên 1. Nếu `front` đạt đến cuối mảng, nó cũng "quay vòng" về 0.
  * **Điều kiện Rỗng:** Queue rỗng khi `front == rear` (trong một số cách cài đặt, hoặc khi `size == 0`).
  * **Điều kiện Đầy:** Queue đầy khi vị trí tiếp theo của `rear` chính là `front`. Tức là `(rear + 1) % capacity == front`. Để phân biệt trạng thái đầy và rỗng (cả hai đều có thể có `front == rear` trong một số cách cài đặt), thường người ta giới hạn số phần tử tối đa là `capacity - 1` hoặc dùng thêm biến `size`.
  * **Tính toán chỉ số:** Sử dụng toán tử modulo (`%`) để thực hiện việc quay vòng: `rear = (rear + 1) % capacity;` và `front = (front + 1) % capacity;`.

Circular Buffer hiệu quả vì cả `enqueue` và `dequeue` đều là O(1) (không cần dịch chuyển phần tử), nhưng vẫn cần xử lý việc thay đổi kích thước (resizing) nếu cần một queue động.

#### 3.3.3. Sử dụng các Lớp/Interface có sẵn trong Java

  * **`java.util.Queue<E>` (Interface):**

      * Đây là interface chính định nghĩa các hành vi cơ bản của một hàng đợi FIFO trong Java Collections Framework.
      * Nó cung cấp hai bộ phương thức cho mỗi thao tác chính:
          * **Ném Exception nếu thất bại:** `add(e)` (ném `IllegalStateException` nếu queue đầy/không thêm được), `remove()` (ném `NoSuchElementException` nếu queue rỗng), `element()` (ném `NoSuchElementException` nếu queue rỗng).
          * **Trả về giá trị đặc biệt:** `offer(e)` (trả về `false` nếu không thêm được), `poll()` (trả về `null` nếu queue rỗng), `peek()` (trả về `null` nếu queue rỗng).
      * **Nên ưu tiên sử dụng các phương thức trả về giá trị đặc biệt (`offer`, `poll`, `peek`)** vì chúng linh hoạt hơn trong việc xử lý các trường hợp biên (rỗng, đầy) mà không cần dùng `try-catch`.

  * **`java.util.LinkedList<E>` (Implementation):**

      * Cài đặt interface `Queue`.
      * Hoạt động tốt như một Queue đa dụng.
      * Cho phép phần tử `null`.

  * **`java.util.ArrayDeque<E>` (Recommended Implementation):**

      * Cũng cài đặt interface `Queue`.
      * Thường **hiệu quả hơn `LinkedList`** cho các thao tác `offer`, `poll`, `peek` do sử dụng mảng vòng (circular array) bên trong.
      * **Đây là lựa chọn khuyến nghị hàng đầu khi bạn cần một cài đặt Queue (FIFO) thông thường trong Java hiện đại.**
      * Không cho phép phần tử `null`.

  * **`java.util.PriorityQueue<E>` (Implementation):**

      * Đây là một loại Queue đặc biệt sẽ được thảo luận riêng ở phần sau. Nó **không tuân thủ nghiêm ngặt FIFO** mà dựa trên thứ tự ưu tiên của các phần tử.

**Ví dụ sử dụng `ArrayDeque` làm Queue:**

```java
import java.util.ArrayDeque;
import java.util.Queue; // Sử dụng interface Queue

public class ArrayDequeAsQueue {
    public static void main(String[] args) {
        // Khai báo bằng interface Queue, khởi tạo bằng ArrayDeque
        Queue<String> taskQueue = new ArrayDeque<>();

        // Sử dụng các phương thức offer, poll, peek
        System.out.println("Offer task 1: " + taskQueue.offer("Send email")); // true
        System.out.println("Offer task 2: " + taskQueue.offer("Process payment")); // true
        System.out.println("Offer task 3: " + taskQueue.offer("Generate report")); // true

        System.out.println("Queue: " + taskQueue); // [Send email, Process payment, Generate report] (Đầu là Send email)
        System.out.println("Peek: " + taskQueue.peek()); // Send email (Xem phần tử đầu)

        String nextTask = taskQueue.poll(); // Lấy và xóa phần tử đầu
        System.out.println("Processing: " + nextTask); // Send email
        System.out.println("Queue after poll: " + taskQueue); // [Process payment, Generate report]

        System.out.println("Is empty? " + taskQueue.isEmpty()); // false
        System.out.println("Size: " + taskQueue.size()); // 2

        taskQueue.poll();
        taskQueue.poll();

        System.out.println("Is empty? " + taskQueue.isEmpty()); // true
        System.out.println("Peek on empty queue: " + taskQueue.peek()); // null
        System.out.println("Poll on empty queue: " + taskQueue.poll()); // null

        // taskQueue.remove(); // Sẽ ném NoSuchElementException nếu rỗng
    }
}
```

### 3.4. Các Trường hợp Sử dụng (Use Cases) Thực tế

  * **Lập lịch Tác vụ (Task Scheduling):** Hệ điều hành sử dụng queue để quản lý các tiến trình đang chờ được cấp CPU (ví dụ: thuật toán Round Robin).
  * **Quản lý Yêu cầu (Request Handling):** Web server thường dùng queue để chứa các yêu cầu HTTP đến trước khi chúng được xử lý bởi các luồng worker. Tương tự cho hàng đợi in ấn (print queue).
  * **Truyền Thông Bất đồng bộ (Asynchronous Communication):** Trong các hệ thống phân tán, message queue (như RabbitMQ, Kafka, SQS) được dùng để các thành phần giao tiếp với nhau mà không cần chờ đợi trực tiếp, giúp tăng khả năng chịu lỗi và mở rộng.
  * **Đệm Dữ liệu (Buffering):** Khi đọc/ghi dữ liệu từ I/O (đĩa, mạng), queue được dùng làm bộ đệm để làm mượt sự chênh lệch tốc độ giữa nơi sản xuất dữ liệu và nơi tiêu thụ dữ liệu.
  * **Thuật toán Duyệt Đồ thị theo Chiều rộng (Breadth-First Search - BFS):** BFS sử dụng queue để lưu trữ các đỉnh cần thăm. Đỉnh được lấy ra từ đầu queue để xử lý, và các hàng xóm chưa thăm của nó được thêm vào cuối queue.
  * **Mô phỏng (Simulations):** Mô phỏng các hệ thống hàng đợi thực tế như quầy thanh toán siêu thị, tổng đài điện thoại,...
  * **Quản lý Tài nguyên Chia sẻ:** Cấp phát quyền truy cập vào một tài nguyên giới hạn (ví dụ: connection pool trong cơ sở dữ liệu) theo thứ tự yêu cầu.

### 3.5. Mẹo Thực chiến & Lỗi Phổ biến

  * **Luôn kiểm tra rỗng hoặc sử dụng `poll`/`peek`:** Tương tự như Stack, việc gọi `remove()` hoặc `element()` trên queue rỗng sẽ ném `NoSuchElementException`. Sử dụng `poll()` và `peek()` an toàn hơn vì chúng trả về `null` khi rỗng, nhưng bạn phải kiểm tra giá trị `null` sau đó.
    ```java
    String data = myQueue.poll();
    if (data != null) {
        // process data
    }
    ```
  * **Chọn đúng phương thức (`offer` vs `add`, `poll` vs `remove`, `peek` vs `element`):** Hiểu rõ sự khác biệt giữa các cặp phương thức (ném exception vs trả về giá trị đặc biệt) để chọn cách xử lý lỗi phù hợp với logic ứng dụng của bạn. Ưu tiên `offer`, `poll`, `peek`.
  * **Cẩn thận với Queue có giới hạn (Bounded Queue):** Khi sử dụng queue có kích thước cố định (như `ArrayBlockingQueue`), thao tác `add()` có thể ném exception nếu queue đầy, trong khi `offer()` sẽ trả về `false` (hoặc có thể chờ đợi trong một khoảng thời gian nhất định với phiên bản có timeout).
  * **Hiệu năng:** Tương tự Stack, `ArrayDeque` thường là lựa chọn hiệu quả nhất cho Queue FIFO thông thường (O(1) cho các thao tác cơ bản). `LinkedList` cũng là O(1) nhưng có thể chậm hơn một chút do quản lý node.

### 3.6. Best Practices

  * **Khai báo bằng Interface:** Sử dụng `Queue<E>` khi khai báo biến.
    ```java
    Queue<Customer> waitingLine = new LinkedList<>(); // Tốt
    Queue<Request> requestPool = new ArrayDeque<>(); // Tốt
    ```
  * **Chọn Cài đặt Phù hợp:**
      * Ưu tiên `ArrayDeque` cho Queue FIFO hiệu năng cao, không cần `null`.
      * Sử dụng `LinkedList` nếu cần Queue FIFO và cho phép `null`.
      * Sử dụng các lớp `BlockingQueue` (như `ArrayBlockingQueue`, `LinkedBlockingQueue`) trong môi trường đa luồng khi cần cơ chế chờ đợi an toàn giữa producer và consumer.
      * Sử dụng `PriorityQueue` khi cần xử lý phần tử theo thứ tự ưu tiên.

## 4\. Priority Queue (Hàng đợi Ưu tiên)

### 4.1. Khái niệm

**Priority Queue** là một kiểu dữ liệu trừu tượng giống Queue, nhưng khác biệt ở chỗ: thay vì tuân thủ nghiêm ngặt FIFO, nó lấy ra phần tử có **độ ưu tiên cao nhất** (hoặc thấp nhất, tùy thuộc vào cách định nghĩa thứ tự).

Các thao tác chính tương tự Queue:

  * `add(element)` hoặc `offer(element)`: Thêm phần tử vào hàng đợi.
  * `remove()` hoặc `poll()`: Lấy và xóa phần tử có **ưu tiên cao nhất**.
  * `element()` hoặc `peek()`: Xem phần tử có **ưu tiên cao nhất** (không xóa).

### 4.2. Cơ chế Hoạt động và Triển khai

Priority Queue thường được cài đặt bằng cấu trúc dữ liệu **Heap** (thường là Min-Heap hoặc Max-Heap). Heap là một cấu trúc dữ liệu dạng cây (thường là cây nhị phân) thỏa mãn **tính chất Heap (Heap Property)**:

  * **Min-Heap:** Giá trị của mọi node cha luôn nhỏ hơn hoặc bằng giá trị của các node con. Do đó, phần tử nhỏ nhất (ưu tiên cao nhất nếu nhỏ là ưu tiên) luôn nằm ở gốc (root).
  * **Max-Heap:** Giá trị của mọi node cha luôn lớn hơn hoặc bằng giá trị của các node con. Phần tử lớn nhất (ưu tiên cao nhất nếu lớn là ưu tiên) luôn nằm ở gốc.

Trong Java, `java.util.PriorityQueue` được cài đặt sử dụng **Min-Heap**.

  * **Thêm phần tử (`offer`)**: O(log n) - Phần tử mới được thêm vào cuối heap (mức thấp nhất, ngoài cùng bên trái) rồi "nổi lên" (bubble up / sift up) để đảm bảo tính chất heap.
  * **Lấy phần tử ưu tiên nhất (`poll`)**: O(log n) - Phần tử ở gốc (nhỏ nhất) được lấy ra. Phần tử cuối cùng của heap được chuyển lên gốc, sau đó "chìm xuống" (bubble down / sift down) để khôi phục tính chất heap.
  * **Xem phần tử ưu tiên nhất (`peek`)**: O(1) - Chỉ cần trả về phần tử ở gốc.

### 4.3. Thứ tự Ưu tiên: `Comparable` và `Comparator`

Để Priority Queue biết phần tử nào có "ưu tiên cao nhất" (nhỏ nhất theo mặc định của `PriorityQueue` trong Java), các phần tử phải có khả năng so sánh được với nhau. Có hai cách để định nghĩa thứ tự này:

1.  **Thứ tự Tự nhiên (`Comparable`):**

      * Lớp của đối tượng cần cài đặt (implements) interface `java.lang.Comparable<E>`.
      * Interface này yêu cầu cài đặt phương thức `int compareTo(E other)`.
      * Phương thức này trả về:
          * Số âm: nếu đối tượng hiện tại (`this`) nhỏ hơn (ưu tiên cao hơn) `other`.
          * Số không: nếu `this` bằng `other`.
          * Số dương: nếu `this` lớn hơn (ưu tiên thấp hơn) `other`.
      * `PriorityQueue` sẽ sử dụng thứ tự này mặc định nếu các phần tử là `Comparable`.

    <!-- end list -->

    ```java
    // Ví dụ lớp Task có thứ tự tự nhiên dựa trên priority (số nhỏ hơn ưu tiên hơn)
    class Task implements Comparable<Task> {
        String description;
        int priority; // Lower value means higher priority

        public Task(String description, int priority) {
            this.description = description;
            this.priority = priority;
        }

        @Override
        public int compareTo(Task other) {
            // So sánh dựa trên priority
            return Integer.compare(this.priority, other.priority);
            // Hoặc: return this.priority - other.priority; (cẩn thận tràn số nếu priority quá lớn/nhỏ)
        }

        @Override
        public String toString() {
            return "Task('" + description + "', priority=" + priority + ")";
        }
    }
    ```

2.  **Thứ tự Tùy chỉnh (`Comparator`):**

      * Nếu lớp không cài đặt `Comparable` hoặc bạn muốn một thứ tự khác với thứ tự tự nhiên, bạn có thể cung cấp một `java.util.Comparator<E>` khi khởi tạo `PriorityQueue`.
      * Interface `Comparator` yêu cầu cài đặt phương thức `int compare(E o1, E o2)`.
      * Phương thức này trả về:
          * Số âm: nếu `o1` nhỏ hơn (ưu tiên cao hơn) `o2`.
          * Số không: nếu `o1` bằng `o2`.
          * Số dương: nếu `o1` lớn hơn (ưu tiên thấp hơn) `o2`.

    <!-- end list -->

    ```java
    import java.util.Comparator;
    import java.util.PriorityQueue;

    class Job {
        String name;
        int complexity; // Higher value might mean lower priority

        public Job(String name, int complexity) {
            this.name = name;
            this.complexity = complexity;
        }
         @Override
        public String toString() {
            return "Job('" + name + "', complexity=" + complexity + ")";
        }
    }

    // Comparator để ưu tiên Job có complexity THẤP HƠN
    class JobComplexityComparator implements Comparator<Job> {
        @Override
        public int compare(Job j1, Job j2) {
            return Integer.compare(j1.complexity, j2.complexity);
        }
    }

    // Hoặc dùng Lambda expression (ngắn gọn hơn)
    // Comparator<Job> jobComparator = (j1, j2) -> Integer.compare(j1.complexity, j2.complexity);
    // Comparator<Job> reverseComplexityComparator = (j1, j2) -> Integer.compare(j2.complexity, j1.complexity); // Ưu tiên complexity CAO HƠN
    ```

### 4.4. Ví dụ Sử dụng `PriorityQueue`

```java
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.Queue;

public class PriorityQueueExample {

    // Lớp Task đã định nghĩa ở trên (Comparable)
    static class Task implements Comparable<Task> {
        String description;
        int priority; // Lower value means higher priority

        public Task(String description, int priority) { /* constructor */
            this.description = description;
            this.priority = priority;
        }
        @Override
        public int compareTo(Task other) { /* compareTo */
            return Integer.compare(this.priority, other.priority);
        }
        @Override
        public String toString() { /* toString */
             return "Task('" + description + "', priority=" + priority + ")";
         }
    }

    // Lớp Job đã định nghĩa ở trên
     static class Job {
        String name;
        int complexity; /* constructor + toString */
        public Job(String name, int complexity) { this.name = name; this.complexity = complexity;}
        @Override
        public String toString() { return "Job('" + name + "', complexity=" + complexity + ")";}
    }


    public static void main(String[] args) {
        System.out.println("--- PriorityQueue with Comparable (Task by priority) ---");
        // Sử dụng thứ tự tự nhiên (Comparable) của Task
        Queue<Task> taskQueue = new PriorityQueue<>();
        taskQueue.offer(new Task("Write report", 3));
        taskQueue.offer(new Task("Fix critical bug", 1)); // Ưu tiên cao nhất
        taskQueue.offer(new Task("Attend meeting", 2));
        taskQueue.offer(new Task("Refactor code", 3));

        System.out.println("Tasks in queue (order might not be sorted): " + taskQueue);

        System.out.println("Processing tasks by priority:");
        while (!taskQueue.isEmpty()) {
            System.out.println("Processing: " + taskQueue.poll());
        }
        // Output: Fix critical bug (1), Attend meeting (2), Write report (3), Refactor code (3)
        // Thứ tự giữa các task cùng priority (3) không được đảm bảo

        System.out.println("\n--- PriorityQueue with Comparator (Job by complexity) ---");
        // Sử dụng Comparator để sắp xếp Job theo complexity tăng dần
        Comparator<Job> jobComparator = (j1, j2) -> Integer.compare(j1.complexity, j2.complexity);
        Queue<Job> jobQueue = new PriorityQueue<>(jobComparator); // Cung cấp Comparator

        // Hoặc có thể khởi tạo Comparator riêng:
        // Queue<Job> jobQueue = new PriorityQueue<>(new JobComplexityComparator());

        jobQueue.offer(new Job("Data Analysis", 8));
        jobQueue.offer(new Job("UI Design", 5)); // Complexity thấp nhất -> ưu tiên cao nhất
        jobQueue.offer(new Job("Database Migration", 10));
        jobQueue.offer(new Job("Testing", 5));

         System.out.println("Jobs in queue (order might not be sorted): " + jobQueue);

        System.out.println("Processing jobs by complexity (lowest first):");
        while(!jobQueue.isEmpty()){
            System.out.println("Processing: " + jobQueue.poll());
        }
        // Output: UI Design (5), Testing (5), Data Analysis (8), Database Migration (10)
    }
}
```

### 4.5. Các Trường hợp Sử dụng (Use Cases) Thực tế

  * **Lập lịch Sự kiện (Event Scheduling):** Trong các hệ thống mô phỏng sự kiện rời rạc, Priority Queue dùng để lưu các sự kiện tương lai, sắp xếp theo thời gian xảy ra. Hệ thống luôn xử lý sự kiện có thời gian xảy ra sớm nhất.
  * **Thuật toán Tìm đường đi Ngắn nhất (Shortest Path Algorithms):**
      * **Dijkstra:** Sử dụng Min-Priority Queue để lưu các đỉnh chưa thăm, ưu tiên đỉnh có khoảng cách tạm thời ngắn nhất từ nguồn.
      * **A\* Search:** Sử dụng Priority Queue để lưu các node cần khám phá, ưu tiên node có giá trị hàm đánh giá (f = g + h) thấp nhất.
  * **Thuật toán Cây Khung Nhỏ nhất (Minimum Spanning Tree - MST):**
      * **Prim:** Sử dụng Min-Priority Queue để lưu các cạnh nối từ cây đang xây dựng đến các đỉnh chưa thuộc cây, ưu tiên cạnh có trọng số nhỏ nhất.
  * **Nén Dữ liệu (Data Compression):** Thuật toán **Huffman Coding** sử dụng Min-Priority Queue để xây dựng cây mã hóa dựa trên tần suất xuất hiện của các ký tự.
  * **Hợp nhất K danh sách đã sắp xếp (Merge K Sorted Lists):** Dùng Min-Priority Queue để lấy ra phần tử nhỏ nhất từ đầu của K danh sách một cách hiệu quả.
  * **Tìm Top K phần tử:** Tìm K phần tử lớn nhất/nhỏ nhất trong một tập dữ liệu lớn mà không cần sắp xếp toàn bộ. Sử dụng Min-Heap để tìm K phần tử lớn nhất hoặc Max-Heap để tìm K phần tử nhỏ nhất.

## 5\. So sánh Stack và Queue

| Đặc điểm          | Stack (Ngăn xếp)                       | Queue (Hàng đợi)                          | Priority Queue                         |
| :---------------- | :------------------------------------- | :---------------------------------------- | :------------------------------------- |
| **Nguyên tắc** | LIFO (Last-In, First-Out)              | FIFO (First-In, First-Out)                | Ưu tiên cao nhất ra trước              |
| **Thêm phần tử** | `push` (Thêm vào đỉnh)                 | `enqueue` / `offer` (Thêm vào cuối)     | `add` / `offer`                      |
| **Xóa phần tử** | `pop` (Xóa khỏi đỉnh)                  | `dequeue` / `poll` (Xóa khỏi đầu)        | `remove` / `poll` (Xóa phần tử ưu tiên) |
| **Xem phần tử** | `peek` / `top` (Xem đỉnh)              | `peek` / `element` / `front` (Xem đầu) | `peek` / `element` (Xem phần tử ưu tiên) |
| **Cài đặt Java** | `ArrayDeque`, `LinkedList` (via Deque) | `ArrayDeque`, `LinkedList` (via Queue)  | `PriorityQueue`                        |
| **Độ phức tạp (Avg)** | O(1) (cho ArrayDeque/LinkedList)   | O(1) (cho ArrayDeque/LinkedList)    | O(log n) (thêm/xóa), O(1) (xem) |
| **Ứng dụng chính**| Call stack, Undo/Redo, Backtracking, Đánh giá biểu thức | BFS, Lập lịch, Buffering, Quản lý yêu cầu | Dijkstra, Prim, A\*, Lập lịch sự kiện, Top K |

## 6\. Tổng kết

Stack và Queue là những viên gạch nền tảng trong cấu trúc dữ liệu và giải thuật.

  * **Stack (LIFO)** rất hữu ích cho các bài toán liên quan đến việc đảo ngược thứ tự, xử lý đệ quy, quay lui, và các thao tác cần truy cập phần tử mới nhất.
  * **Queue (FIFO)** phù hợp cho việc xử lý các tác vụ theo thứ tự đến, duyệt đồ thị theo chiều rộng, và quản lý tài nguyên/yêu cầu một cách công bằng.
  * **Priority Queue** mở rộng khái niệm Queue bằng cách cho phép xử lý các phần tử dựa trên độ ưu tiên, là công cụ mạnh mẽ cho nhiều thuật toán tối ưu hóa và lập lịch.

Trong Java, **`ArrayDeque`** thường là lựa chọn tốt nhất cho cả Stack và Queue FIFO thông thường nhờ hiệu năng và thiết kế hiện đại. **`PriorityQueue`** là lựa chọn chuẩn cho hàng đợi ưu tiên. Việc hiểu rõ cách chúng hoạt động, các cách triển khai khác nhau, và khi nào nên sử dụng chúng sẽ giúp bạn viết mã hiệu quả, rõ ràng và dễ bảo trì hơn.