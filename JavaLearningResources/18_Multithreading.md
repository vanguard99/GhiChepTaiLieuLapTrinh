# LẬP TRÌNH ĐA LUỒNG (THREADING)

Hướng dẫn này cung cấp một cái nhìn toàn diện và chuyên sâu về lập trình đa luồng trong Java, từ các khái niệm cơ bản đến các kỹ thuật nâng cao, ví dụ thực tế và các phương pháp tốt nhất.

## 1\. Giới thiệu về Đa nhiệm và Đa luồng

Trong thế giới điện toán hiện đại, khả năng thực thi nhiều tác vụ đồng thời là điều cần thiết để tối ưu hóa hiệu suất và cải thiện trải nghiệm người dùng.

  * **Đa nhiệm (Multi-tasking)**: Là khả năng của một hệ điều hành cho phép chạy nhiều chương trình (tiến trình - process) độc lập cùng một lúc. Ví dụ, bạn có thể vừa duyệt web bằng Chrome, vừa soạn thảo văn bản bằng Word, và nghe nhạc bằng Spotify trên cùng một máy tính. Mỗi chương trình này là một tiến trình riêng biệt.
  * **Đa luồng (Multi-threading)**: Là khả năng của một tiến trình (chương trình) có thể thực thi đồng thời nhiều luồng (thread) xử lý bên trong nó. Mỗi luồng thực hiện một phần công việc cụ thể của chương trình đó. Ví dụ, trong một trình soạn thảo văn bản, một luồng có thể xử lý việc nhập liệu từ người dùng, một luồng khác thực hiện kiểm tra lỗi chính tả, và một luồng nữa tự động lưu tài liệu.

**Lợi ích của Đa luồng:**

  * **Tăng độ phản hồi (Responsiveness):** Giúp ứng dụng (đặc biệt là ứng dụng có giao diện người dùng - GUI) luôn phản hồi nhanh chóng với tương tác của người dùng, ngay cả khi đang thực hiện các tác vụ nặng ở chế độ nền.
  * **Tăng hiệu suất (Performance):** Tận dụng tối đa khả năng xử lý của CPU đa lõi bằng cách thực thi song song các phần công việc độc lập.
  * **Chia sẻ tài nguyên hiệu quả:** Các luồng trong cùng một tiến trình chia sẻ chung không gian bộ nhớ (heap), giúp việc trao đổi dữ liệu giữa các luồng trở nên dễ dàng và ít tốn kém hơn so với giao tiếp giữa các tiến trình (Inter-Process Communication - IPC).
  * **Đơn giản hóa mô hình hóa các tác vụ đồng thời:** Cho phép mô hình hóa các tác vụ phức tạp thành các đơn vị công việc nhỏ hơn, dễ quản lý hơn.

**Khác biệt cốt lõi giữa Tiến trình (Process) và Luồng (Thread):**

| Đặc điểm           | Tiến trình (Process)                   | Luồng (Thread)                                  |
| :----------------- | :------------------------------------- | :---------------------------------------------- |
| **Đơn vị**         | Đơn vị thực thi của chương trình       | Đơn vị thực thi nhỏ nhất trong một tiến trình   |
| **Độc lập**        | Hoạt động độc lập với nhau             | Phụ thuộc vào tiến trình cha                    |
| **Không gian nhớ** | Có không gian bộ nhớ riêng biệt        | Chia sẻ không gian bộ nhớ (heap) của tiến trình |
| **Tạo mới**        | Tốn kém tài nguyên (thời gian, bộ nhớ) | Ít tốn kém tài nguyên hơn                       |
| **Giao tiếp**      | Phức tạp hơn (IPC)                     | Đơn giản hơn (qua biến chia sẻ)                 |

## 2\. Khái niệm Cốt lõi về Thread trong Java

  * **Thread (Luồng)**: Là đường dẫn thực thi (path of execution) độc lập bên trong một chương trình Java. Mỗi chương trình Java khi khởi chạy sẽ có ít nhất một luồng chính (main thread) do Máy ảo Java (JVM) tạo ra. Từ luồng chính này, các luồng khác có thể được tạo ra để thực hiện các công việc đồng thời.
  * **JVM và Quản lý Thread:** JVM chịu trách nhiệm quản lý vòng đời và lập lịch (scheduling) cho các luồng. Cơ chế lập lịch (ví dụ: preemptive - ưu tiên, time-slicing - chia sẻ thời gian) có thể khác nhau tùy thuộc vào JVM và hệ điều hành bên dưới. Lập trình viên thường không kiểm soát trực tiếp việc lập lịch này mà chỉ có thể đưa ra gợi ý (ví dụ: thông qua độ ưu tiên - priority).

## 3\. Tạo và Quản lý Thread trong Java

Java cung cấp hai cách chính để định nghĩa và tạo luồng:

### 3.1. Cách 1: Kế thừa (Extend) từ lớp `java.lang.Thread`

Đây là cách trực tiếp nhất nhưng kém linh hoạt hơn do Java không hỗ trợ đa kế thừa lớp.

  * **Bước 1:** Tạo một lớp mới kế thừa từ `java.lang.Thread`.
  * **Bước 2:** Ghi đè (override) phương thức `run()`. Logic xử lý của luồng sẽ được đặt bên trong phương thức này.
  * **Bước 3:** Tạo một đối tượng từ lớp vừa định nghĩa và gọi phương thức `start()` của nó để yêu cầu JVM cấp phát tài nguyên và lập lịch thực thi cho luồng mới. **Lưu ý:** Không bao giờ gọi trực tiếp phương thức `run()`. Việc gọi `run()` sẽ chỉ thực thi logic trong luồng hiện tại mà không tạo ra luồng mới.

**Ví dụ:**

```java
// Bước 1 & 2: Định nghĩa lớp Thread
class MyThread extends Thread {
    private String threadName;

    public MyThread(String name) {
        this.threadName = name;
        System.out.println("Creating " +  threadName );
    }

    @Override
    public void run() { // Logic của luồng
        System.out.println("Running " +  threadName );
        try {
            for(int i = 4; i > 0; i--) {
                System.out.println("Thread: " + threadName + ", " + i);
                // Dừng luồng trong 50ms
                Thread.sleep(50);
            }
        } catch (InterruptedException e) {
            System.out.println("Thread " +  threadName + " interrupted.");
        }
        System.out.println("Thread " +  threadName + " exiting.");
    }
}

public class TestThread {
   public static void main(String args[]) {
      // Bước 3: Tạo và khởi chạy Thread
      MyThread t1 = new MyThread("Thread-1");
      t1.start(); // Yêu cầu JVM bắt đầu luồng

      MyThread t2 = new MyThread("Thread-2");
      t2.start(); // Yêu cầu JVM bắt đầu luồng
   }
}
```

### 3.2. Cách 2: Triển khai (Implement) interface `java.lang.Runnable`

Đây là cách được **khuyến khích** vì nó linh hoạt hơn, cho phép lớp của bạn kế thừa từ một lớp khác nếu cần (tách biệt logic tác vụ khỏi cơ chế tạo luồng).

  * **Bước 1:** Tạo một lớp mới triển khai interface `java.lang.Runnable`.
  * **Bước 2:** Cung cấp cài đặt (implement) cho phương thức `run()` duy nhất của interface này. Đây là nơi chứa logic của tác vụ.
  * **Bước 3:**
      * Tạo một đối tượng từ lớp vừa định nghĩa (đại diện cho tác vụ - task).
      * Tạo một đối tượng `Thread`, truyền đối tượng `Runnable` vào hàm khởi tạo của `Thread`.
      * Gọi phương thức `start()` trên đối tượng `Thread` để khởi chạy.

**Ví dụ (Viết lại ví dụ PrintChar và PrintNum từ slide):**

```java
// Tác vụ in một ký tự nhiều lần
class PrintChar implements Runnable { // Bước 1
    private char charToPrint;
    private int times;

    public PrintChar(char c, int t) {
        this.charToPrint = c;
        this.times = t;
    }

    @Override
    public void run() { // Bước 2: Logic tác vụ
        for (int i = 0; i < times; i++) {
            System.out.print(charToPrint);
            // Có thể thêm Thread.yield() hoặc Thread.sleep() để dễ quan sát sự xen kẽ
        }
    }
}

// Tác vụ in số từ 1 đến n
class PrintNum implements Runnable { // Bước 1
    private int lastNum;

    public PrintNum(int n) {
        this.lastNum = n;
    }

    @Override
    public void run() { // Bước 2: Logic tác vụ
        for (int i = 1; i <= lastNum; i++) {
            System.out.print(" " + i);
             // Có thể thêm Thread.yield() hoặc Thread.sleep() để dễ quan sát sự xen kẽ
        }
    }
}


public class TaskThreadDemo {
    public static void main(String[] args) {
        // Bước 3: Tạo các đối tượng Runnable (tác vụ)
        Runnable printA = new PrintChar('a', 100);
        Runnable printB = new PrintChar('b', 100);
        Runnable print100 = new PrintNum(100);

        // Tạo các đối tượng Thread từ Runnable
        Thread thread1 = new Thread(printA);
        Thread thread2 = new Thread(printB);
        Thread thread3 = new Thread(print100);

        // Khởi chạy các luồng
        System.out.println("Starting threads...");
        thread1.start();
        thread2.start();
        thread3.start();
        System.out.println("\nThreads started.");
        // Luồng main có thể kết thúc trước các luồng con
    }
}
```

### 3.3. So sánh `Thread` vs `Runnable`

| Tiêu chí           | Kế thừa `Thread`                      | Triển khai `Runnable`                                          | Khuyến nghị |
| :----------------- | :------------------------------------ | :------------------------------------------------------------- | :---------- |
| **Bản chất**       | Lớp *là một* Thread (`is-a`)          | Lớp *có một* tác vụ (`has-a` task)                             | `Runnable`  |
| **Đa kế thừa**     | Không thể kế thừa lớp khác            | Có thể kế thừa lớp khác                                        | `Runnable`  |
| **Tính linh hoạt** | Kém linh hoạt hơn                     | Linh hoạt hơn, tách biệt tác vụ và luồng                       | `Runnable`  |
| **Chia sẻ tác vụ** | Khó chia sẻ cùng 1 đối tượng `Thread` | Dễ dàng chia sẻ cùng 1 đối tượng `Runnable` cho nhiều `Thread` | `Runnable`  |
| **Thiết kế**       | Hướng đối tượng kém hơn               | Hướng đối tượng tốt hơn (Composition)                          | `Runnable`  |

**Kết luận:** Nên **ưu tiên sử dụng `Runnable`** thay vì kế thừa `Thread` trừ khi bạn cần sửa đổi hành vi cơ bản của chính lớp `Thread`.

### 3.4. Sử dụng Lớp Nặc danh (Anonymous Class)

Để tạo nhanh các luồng hoặc tác vụ đơn giản mà không cần định nghĩa lớp riêng.

**Với `Thread`:**

```java
new Thread() {
    @Override
    public void run() {
        System.out.println("Anonymous Thread running...");
        // Logic xử lý
    }
}.start();
```

**Với `Runnable`:**

```java
Runnable myRunnable = new Runnable() {
    @Override
    public void run() {
        System.out.println("Anonymous Runnable running...");
        // Logic xử lý
    }
};
new Thread(myRunnable).start();

// Hoặc viết gọn:
new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("Inline Anonymous Runnable running...");
    }
}).start();
```

  * **Ưu điểm:** Ngắn gọn cho các tác vụ đơn giản.
  * **Nhược điểm:** Khó đọc và bảo trì nếu logic phức tạp, không thể tái sử dụng. Thường được thay thế bằng biểu thức Lambda trong Java 8+.

### 3.5. Biểu thức Lambda (Java 8+)

Cách viết gọn gàng nhất để tạo `Runnable` (vì `Runnable` là một functional interface).

```java
// Tương đương với việc tạo Runnable bằng lớp nặc danh
Runnable r1 = () -> System.out.println("Lambda Runnable 1 running...");
new Thread(r1).start();

// Thậm chí gọn hơn
new Thread(() -> System.out.println("Lambda Runnable 2 running...")).start();

// Ví dụ với logic phức tạp hơn
new Thread(() -> {
    for (int i = 0; i < 5; i++) {
        System.out.println("Lambda loop: " + i);
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt(); // Khôi phục trạng thái ngắt
        }
    }
}).start();
```

## 4\. Vòng đời của Thread (Thread Lifecycle)

Một luồng trong Java trải qua các trạng thái khác nhau trong suốt vòng đời của nó. Việc hiểu rõ các trạng thái này rất quan trọng để gỡ lỗi và quản lý luồng hiệu quả.

  * **NEW:** Luồng vừa được tạo bằng toán tử `new` nhưng phương thức `start()` chưa được gọi. Nó chưa được cấp phát tài nguyên hệ thống và chưa thể chạy.
  * **RUNNABLE (Ready to run):** Sau khi `start()` được gọi, luồng chuyển sang trạng thái `RUNNABLE`. Nó đã sẵn sàng để chạy và đang chờ bộ lập lịch (thread scheduler) của JVM cấp phát CPU. Một luồng đang thực sự chạy trên CPU cũng được coi là ở trạng thái `RUNNABLE`.
  * **RUNNING:** (Không phải là trạng thái chính thức trong `Thread.State` enum nhưng hữu ích về mặt khái niệm) Luồng đang được CPU thực thi mã trong phương thức `run()` của nó.
  * **BLOCKED (Chặn):** Luồng đang tạm dừng hoạt động vì đang chờ đợi một **monitor lock** (khóa màn hình) để vào một khối mã `synchronized` hoặc một phương thức `synchronized` mà đang bị một luồng khác chiếm giữ.
  * **WAITING (Chờ đợi):** Luồng đang tạm dừng vô thời hạn, chờ một luồng khác thực hiện một hành động cụ thể. Điều này xảy ra khi luồng gọi một trong các phương thức sau mà không có timeout:
      * `Object.wait()`
      * `Thread.join()`
      * `LockSupport.park()`
        Luồng chỉ thoát khỏi trạng thái `WAITING` khi nhận được thông báo thích hợp (ví dụ: `Object.notify()`, `Object.notifyAll()`) hoặc bị ngắt (`interrupt()`). 
  * **TIMED\_WAITING (Chờ đợi có thời hạn):** Luồng đang tạm dừng trong một khoảng thời gian xác định. Nó sẽ quay lại trạng thái `RUNNABLE` khi hết thời gian chờ, hoặc khi nhận được thông báo thích hợp, hoặc bị ngắt. Điều này xảy ra khi luồng gọi các phương thức:
      * `Thread.sleep(long millis)`
      * `Object.wait(long timeout)`
      * `Thread.join(long millis)`
      * `LockSupport.parkNanos(long nanos)`
      * `LockSupport.parkUntil(long deadline)`
        
  * **TERMINATED (Dead - Kết thúc):** Luồng đã hoàn thành việc thực thi phương thức `run()` của nó hoặc đã bị dừng vì một lý do nào đó (ví dụ: một ngoại lệ không được xử lý). Luồng không thể được khởi động lại.

**Sơ đồ chuyển đổi trạng thái (Tương tự Image 2 ):**

```
NEW 
  └── start() ──► RUNNABLE
        ├── CPU cấp phát ──► RUNNING
        │       ├── Hết time slice / yield() ──► RUNNABLE
        │       ├── Hoàn thành run() / Exception ──► TERMINATED
        │       ├── Chờ monitor lock ──► BLOCKED
        │       │      └── Nhận được monitor lock ──► RUNNABLE
        │       ├── wait() / join() / park() ──► WAITING
        │       │      ├── notify() / notifyAll() / unpark() / Luồng join kết thúc ──► RUNNABLE
        │       │      └── interrupt() ──► RUNNABLE
        │       └── sleep(t) / wait(t) / join(t) / parkNanos(t) / parkUntil(t) ──► TIMED_WAITING
        │              ├── Hết timeout / notify() / notifyAll() / unpark() / Luồng join kết thúc ──► RUNNABLE
        │              └── interrupt() ──► RUNNABLE
        └── interrupt() trong RUNNABLE (không đang chạy) ──► RUNNABLE (chỉ set cờ interrupt)
```

## 5\. Các Phương thức Quan trọng của Lớp `Thread`

Lớp `java.lang.Thread` cung cấp nhiều phương thức để quản lý và tương tác với luồng:

  * `void start()`: Khởi động luồng, yêu cầu JVM gọi phương thức `run()`.
  * `void run()`: Chứa mã thực thi của luồng. Được gọi tự động bởi JVM sau khi `start()` được gọi.
  * `static void sleep(long millis)`: Tạm dừng luồng **hiện tại** trong một khoảng thời gian tính bằng mili giây. Luồng chuyển sang trạng thái `TIMED_WAITING`. Ném ra `InterruptedException` nếu luồng bị ngắt trong khi đang ngủ.
  * `static void yield()`: Gợi ý cho bộ lập lịch rằng luồng **hiện tại** sẵn sàng nhường quyền sử dụng CPU cho các luồng khác có cùng độ ưu tiên. Bộ lập lịch có thể bỏ qua gợi ý này. Luồng vẫn ở trạng thái `RUNNABLE`.
  * `void join()`: Chờ cho luồng **mà phương thức này được gọi trên đó** kết thúc (chuyển sang `TERMINATED`). Luồng **hiện tại** (luồng gọi `join()`) sẽ chuyển sang `WAITING` (hoặc `TIMED_WAITING` nếu dùng `join(long millis)`). Ném ra `InterruptedException`. Rất hữu ích để đảm bảo các luồng con hoàn thành trước khi luồng cha tiếp tục.
  * `void interrupt()`: Gửi một yêu cầu ngắt (interrupt request) đến luồng **mà phương thức này được gọi trên đó**. Nó *không* ép buộc luồng dừng lại ngay lập tức. Thay vào đó, nó thiết lập cờ ngắt (interrupt status flag) của luồng đó. Nếu luồng đang ở trạng thái `WAITING`, `TIMED_WAITING` (ví dụ: trong `sleep()`, `wait()`, `join()`), nó sẽ nhận được một `InterruptedException` và cờ ngắt sẽ bị xóa. Nếu luồng đang ở trạng thái `RUNNABLE`, cờ ngắt sẽ được đặt, và luồng cần phải tự kiểm tra cờ này (dùng `isInterrupted()` hoặc `Thread.interrupted()`) để quyết định có nên dừng hay không.
  * `boolean isInterrupted()`: Kiểm tra xem luồng có bị ngắt hay không **mà không xóa** cờ ngắt.
  * `static boolean interrupted()`: Kiểm tra xem luồng **hiện tại** có bị ngắt hay không **và xóa** cờ ngắt sau khi kiểm tra.
  * `boolean isAlive()`: Kiểm tra xem luồng đã được khởi động (`start()` đã được gọi) và chưa kết thúc (`TERMINATED`) hay chưa.
  * `void setPriority(int newPriority)`: Đặt độ ưu tiên cho luồng (từ `Thread.MIN_PRIORITY`=1 đến `Thread.MAX_PRIORITY`=10, mặc định `Thread.NORM_PRIORITY`=5). Đây chỉ là gợi ý cho bộ lập lịch, không đảm bảo thứ tự thực thi.
  * `int getPriority()`: Lấy độ ưu tiên hiện tại của luồng.
  * `long getId()`: Trả về ID duy nhất của luồng.
  * `Thread.State getState()`: Trả về trạng thái hiện tại của luồng (là một enum `Thread.State`).
  * `static Thread currentThread()`: Trả về tham chiếu đến đối tượng `Thread` đang thực thi mã lệnh hiện tại.

## 6\. Đồng bộ hóa (Synchronization) - Giải quyết Vấn đề Truy cập Đồng thời

Khi nhiều luồng cùng truy cập và sửa đổi các tài nguyên chia sẻ (shared resources), chẳng hạn như các biến instance, biến static, hoặc các đối tượng dùng chung, có thể xảy ra các vấn đề nghiêm trọng gọi là **Race Condition** (Tranh chấp Điều kiện).

**Ví dụ về Race Condition (Minh họa qua ví dụ `AccountWithoutSync`):**

Hãy xem xét phương thức `deposit` trong lớp `Account`:

```java
private static class Account {
    private int balance = 0;

    public int getBalance() {
        return balance;
    }

    // PHIÊN BẢN CÓ VẤN ĐỀ (KHÔNG ĐỒNG BỘ)
    public void deposit(int amount) {
        int newBalance = balance + amount; // (1) Đọc balance
        try {
            Thread.sleep(5); // (2) Giả lập thời gian xử lý, tăng khả năng xảy ra lỗi
        } catch (InterruptedException ex) {}
        balance = newBalance; // (3) Ghi balance mới
    }
}
```

Giả sử `balance` ban đầu là 0 và có hai luồng (T1, T2) cùng gọi `deposit(1)`:

1.  T1 đọc `balance` (0), tính `newBalance` = 1. 
2.  T1 bị tạm dừng (ví dụ: hết time slice, hoặc do `sleep(5)`). 
3.  T2 đọc `balance` (vẫn là 0), tính `newBalance` = 1. 
4.  T2 ghi `balance` = 1. 
5.  T2 kết thúc.
6.  T1 hoạt động trở lại và ghi `balance` = 1 (giá trị `newBalance` nó đã tính trước đó). 

**Kết quả:** `balance` cuối cùng là 1, trong khi đáng lẽ phải là 2. Đây là Race Condition. Dữ liệu đã bị sai lệch do sự truy cập không được kiểm soát vào biến `balance`.

**Giải pháp: Đồng bộ hóa (Synchronization)**

Đồng bộ hóa là cơ chế kiểm soát việc truy cập vào các tài nguyên chia sẻ, đảm bảo rằng tại một thời điểm, chỉ có một luồng duy nhất có thể truy cập vào phần mã hoặc tài nguyên quan trọng (gọi là **vùng tranh chấp** - critical section).

Java cung cấp cơ chế đồng bộ hóa tích hợp sẵn dựa trên khái niệm **monitor** (màn hình giám sát) và từ khóa `synchronized`.

  * Mỗi đối tượng trong Java đều có một *monitor lock* (còn gọi là intrinsic lock hoặc mutex) đi kèm.
  * Khi một luồng muốn thực thi một khối mã hoặc phương thức được đánh dấu `synchronized`, nó phải giành được (acquire) *monitor lock* của đối tượng liên quan.
  * Tại một thời điểm, chỉ có một luồng có thể giữ *monitor lock* của một đối tượng cụ thể. Các luồng khác cố gắng giành lock đó sẽ bị chặn (`BLOCKED`) cho đến khi luồng đang giữ lock giải phóng nó (release). Lock được tự động giải phóng khi luồng thoát khỏi khối mã/phương thức `synchronized` (kể cả khi thoát do ném ra ngoại lệ).

### 6.1. Đồng bộ hóa Phương thức (Synchronized Methods)

Áp dụng từ khóa `synchronized` vào khai báo phương thức.

  * **Phương thức instance `synchronized`:** Luồng phải giành được lock của đối tượng instance (`this`) trước khi thực thi phương thức.
  * **Phương thức static `synchronized`:** Luồng phải giành được lock của đối tượng `Class` đại diện cho lớp đó.

**Ví dụ (Sửa lỗi ví dụ `Account`):**

```java
private static class Account {
    private int balance = 0;

    public int getBalance() {
        // Không cần synchronized vì chỉ đọc giá trị nguyên thủy (atomic read)
        // Tuy nhiên, nếu cần đảm bảo đọc giá trị mới nhất sau khi ghi,
        // có thể cần volatile hoặc synchronized write/read.
        return balance;
    }

    // ĐỒNG BỘ HÓA PHƯƠNG THỨC INSTANCE
    public synchronized void deposit(int amount) {
        int newBalance = balance + amount;
        // try { Thread.sleep(5); } catch (InterruptedException ex) {} // Bỏ sleep trong thực tế
        balance = newBalance;
        // Lock của 'this' (đối tượng Account) được giải phóng khi thoát khỏi phương thức
    }
}
```

Với `synchronized void deposit(...)`, khi một luồng (ví dụ T1) gọi `deposit`, nó sẽ giành lock của đối tượng `Account`. Nếu T2 cũng gọi `deposit` trên *cùng* đối tượng `Account` đó, T2 sẽ bị chặn cho đến khi T1 hoàn thành và giải phóng lock.

### 6.2. Đồng bộ hóa Khối lệnh (Synchronized Blocks)

Cho phép đồng bộ hóa chỉ một phần của phương thức thay vì toàn bộ, giúp giảm phạm vi khóa và tăng khả năng thực thi đồng thời. Cú pháp:

```java
synchronized (objectReference) {
   // Khối mã cần được bảo vệ (critical section)
   // Chỉ một luồng có thể thực thi mã này tại một thời điểm
   // trên cùng một đối tượng được tham chiếu bởi objectReference
}
```

  * `objectReference`: Là đối tượng mà luồng cần giành lock của nó.
      * Thường là `this` để khóa trên đối tượng hiện tại (tương đương phương thức instance synchronized).
      * Có thể là một đối tượng lock riêng biệt (ví dụ: `private final Object lock = new Object();`).
      * Có thể là `MyClass.class` để khóa trên đối tượng Class (tương đương phương thức static synchronized).

**Viết lại phương thức `deposit` dùng synchronized block:**

```java
private static class Account {
    private int balance = 0;
    private final Object lock = new Object(); // Lock riêng biệt (tùy chọn)

    public int getBalance() {
        // Có thể cần đồng bộ nếu balance không phải kiểu nguyên thủy hoặc cần visibility guarantee
        synchronized(lock) { // Hoặc synchronized(this)
             return balance;
        }
    }

    public void deposit(int amount) {
        int tempBalance; // Biến cục bộ, không cần trong synchronized block

        // Các thao tác không cần khóa có thể đặt ở đây

        synchronized(lock) { // Khóa trên đối tượng lock riêng biệt (hoặc this)
             // --- Critical Section START ---
             int newBalance = balance + amount;
             balance = newBalance;
             // --- Critical Section END ---
        }
        // Lock được giải phóng

        // Các thao tác khác không cần khóa
    }
}
```

**Khi nào dùng Synchronized Block?**

  * Khi chỉ cần bảo vệ một phần nhỏ của phương thức.
  * Khi cần khóa trên một đối tượng khác `this` hoặc `MyClass.class`.
  * Khi cần thực hiện các thao tác không cần khóa trước hoặc sau vùng tranh chấp để tối ưu hiệu suất.

**Cảnh báo:** Việc sử dụng `synchronized` không đúng cách có thể dẫn đến **Deadlock** (Khóa chết).

## 7\. Các Vấn đề Phổ biến trong Lập trình Đa luồng

### 7.1. Race Condition (Đã đề cập)

Xảy ra khi nhiều luồng truy cập tài nguyên chia sẻ và kết quả cuối cùng phụ thuộc vào thứ tự thực thi không thể đoán trước của các luồng. Giải pháp: Đồng bộ hóa.

### 7.2. Deadlock (Khóa chết)

Xảy ra khi hai hoặc nhiều luồng bị chặn vĩnh viễn, mỗi luồng đang giữ một tài nguyên và chờ đợi tài nguyên mà luồng khác đang giữ.

**Ví dụ kinh điển:**

  * Luồng A khóa Tài nguyên 1, sau đó cố gắng khóa Tài nguyên 2.
  * Luồng B khóa Tài nguyên 2, sau đó cố gắng khóa Tài nguyên 1.

Nếu Luồng A khóa được Tài nguyên 1 và Luồng B khóa được Tài nguyên 2 cùng lúc, cả hai sẽ bị chặn mãi mãi khi cố gắng khóa tài nguyên còn lại.

**Cách phòng tránh Deadlock:**

  * **Thứ tự khóa (Lock Ordering):** Đảm bảo tất cả các luồng luôn yêu cầu khóa theo một thứ tự cố định. Ví dụ, luôn khóa Tài nguyên 1 trước Tài nguyên 2.
  * **Timeout khi yêu cầu khóa:** Sử dụng các cơ chế khóa nâng cao (ví dụ: `Lock.tryLock(timeout)`) cho phép luồng từ bỏ yêu cầu khóa sau một khoảng thời gian chờ nhất định, thay vì chờ đợi vô hạn.
  * **Giảm phạm vi khóa:** Giữ khóa trong thời gian ngắn nhất có thể.
  * **Tránh khóa lồng nhau không cần thiết.**

### 7.3. Livelock

Xảy ra khi các luồng không bị chặn, nhưng chúng liên tục thay đổi trạng thái để phản ứng với hành động của nhau mà không bao giờ tiến triển được công việc thực sự. Giống như hai người cố gắng tránh nhau trong hành lang hẹp, cùng bước sang một bên rồi lại cùng bước sang bên kia.

**Giải pháp:** Đưa yếu tố ngẫu nhiên vào hành động lặp lại (ví dụ: thời gian chờ ngẫu nhiên trước khi thử lại).

### 7.4. Starvation (Đói tài nguyên)

Xảy ra khi một hoặc nhiều luồng không bao giờ có cơ hội được cấp phát tài nguyên (CPU, lock...) để thực thi, thường do các luồng có độ ưu tiên cao hơn hoặc các luồng "tham lam" liên tục chiếm giữ tài nguyên.

**Giải pháp:** Sử dụng cơ chế lập lịch công bằng (fair scheduling - ví dụ `ReentrantLock(true)`), quản lý độ ưu tiên hợp lý.

### 7.5. Vấn đề về Bộ nhớ (Memory Consistency Errors / Visibility Issues)

Xảy ra khi các luồng không nhìn thấy được những thay đổi mới nhất đối với các biến chia sẻ do cơ chế caching của CPU hoặc tối ưu hóa của trình biên dịch. Một luồng có thể đang làm việc với một bản sao cũ (stale copy) của dữ liệu.

**Từ khóa `volatile`:**

  * Đảm bảo rằng mọi thao tác đọc/ghi biến `volatile` sẽ được thực hiện trực tiếp trên bộ nhớ chính, bỏ qua cache cục bộ của CPU.
  * Đảm bảo "happens-before" relationship: Việc ghi vào biến `volatile` xảy ra trước bất kỳ thao tác đọc biến đó sau này.
  * **Lưu ý:** `volatile` chỉ đảm bảo *visibility* (tính nhìn thấy) và *ordering* (thứ tự), nó **không** đảm bảo *atomicity* (tính nguyên tố) cho các thao tác phức hợp (như `count++`, vốn bao gồm đọc-sửa-ghi). Chỉ sử dụng `volatile` khi một luồng ghi và các luồng khác đọc, hoặc khi các cập nhật không phụ thuộc vào giá trị hiện tại.

**Giải pháp khác:** Sử dụng `synchronized` (đảm bảo cả atomicity và visibility) hoặc các lớp trong `java.util.concurrent.atomic` (ví dụ `AtomicInteger`, `AtomicBoolean`).

## 8\. Khung Executor (Executor Framework) và Thread Pools

Việc tạo và hủy luồng trực tiếp bằng `new Thread().start()` có thể tốn kém tài nguyên, đặc biệt khi cần xử lý nhiều tác vụ ngắn hạn. **Thread Pool** (Bể luồng) là một giải pháp hiệu quả:

  * Quản lý một tập hợp các luồng worker đã được tạo sẵn.
  * Khi có tác vụ cần thực thi, nó sẽ được gán cho một luồng rảnh rỗi trong pool.
  * Sau khi hoàn thành, luồng không bị hủy mà quay trở lại pool để sẵn sàng nhận tác vụ tiếp theo.

**Lợi ích:**

  * **Giảm chi phí tạo/hủy luồng.**
  * **Kiểm soát số lượng luồng:** Ngăn chặn việc tạo quá nhiều luồng gây cạn kiệt tài nguyên hệ thống.
  * **Quản lý tác vụ dễ dàng hơn.**

**Khung Executor (`java.util.concurrent`):**

Cung cấp một cách tiếp cận cấp cao, linh hoạt và mạnh mẽ để quản lý và thực thi các tác vụ bất đồng bộ (thường là các `Runnable` hoặc `Callable`).

  * **`Executor` interface:** Interface cơ bản, chỉ có phương thức `execute(Runnable command)`.
  * **`ExecutorService` interface:** Mở rộng `Executor`, cung cấp các phương thức quản lý vòng đời của executor (ví dụ: `shutdown()`, `awaitTermination()`) và khả năng xử lý `Callable` (trả về kết quả) thông qua `submit()`.
  * **`Executors` class:** Lớp tiện ích cung cấp các phương thức factory để tạo các loại `ExecutorService` phổ biến:
      * `newFixedThreadPool(int nThreads)`: Tạo pool với số lượng luồng cố định. Nếu tất cả luồng đều bận, tác vụ mới sẽ được đưa vào hàng đợi.
      * `newCachedThreadPool()`: Tạo pool có thể thay đổi kích thước. Nó sẽ tạo luồng mới nếu cần và hủy các luồng không hoạt động sau một khoảng thời gian. Thích hợp cho nhiều tác vụ ngắn hạn, nhưng cần cẩn thận vì có thể tạo ra rất nhiều luồng.
      * `newSingleThreadExecutor()`: Tạo executor chỉ sử dụng một luồng duy nhất. Đảm bảo các tác vụ được thực thi tuần tự theo thứ tự chúng được submit.
      * `newScheduledThreadPool(int corePoolSize)`: Tạo pool hỗ trợ lập lịch thực thi tác vụ sau một khoảng thời gian trễ hoặc thực thi định kỳ.

**Ví dụ sử dụng `ExecutorService` (Cải tiến ví dụ `AccountWithoutSync`):**

```java
import java.util.concurrent.*;
import java.util.concurrent.atomic.AtomicInteger; // Sử dụng AtomicInteger thay vì Account tự quản lý

public class AccountWithExecutor {

    // Sử dụng AtomicInteger để đảm bảo tính nguyên tố và visibility
    private static AtomicInteger balance = new AtomicInteger(0);

    // Tác vụ thêm 1 penny (sử dụng AtomicInteger)
    private static class AddAPennyTask implements Runnable {
        @Override
        public void run() {
            balance.incrementAndGet(); // Thao tác nguyên tố, thread-safe
        }
    }

    public static void main(String[] args) {
        // Tạo một Thread Pool với số luồng cố định (ví dụ: 10)
        ExecutorService executor = Executors.newFixedThreadPool(10);

        System.out.println("Initial Balance: " + balance.get());

        // Gửi 1000 tác vụ vào Thread Pool
        for (int i = 0; i < 1000; i++) {
            executor.execute(new AddAPennyTask());
        }

        // Yêu cầu ExecutorService ngừng nhận tác vụ mới và hoàn thành các tác vụ đang chờ
        executor.shutdown();

        try {
            // Chờ tối đa 1 phút để tất cả các tác vụ hoàn thành
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                 System.err.println("Tasks did not complete within the timeout period.");
                 // Có thể gọi executor.shutdownNow() để cố gắng dừng các tác vụ đang chạy
            }
        } catch (InterruptedException e) {
            System.err.println("Executor termination interrupted.");
            executor.shutdownNow(); // Cố gắng dừng ngay lập tức
            Thread.currentThread().interrupt(); // Khôi phục trạng thái ngắt
        }

        System.out.println("Final Balance: " + balance.get()); // Kết quả mong đợi: 1000
    }
}
```

### 8.1. `Callable` và `Future`

  * **`Callable<V>`:** Tương tự `Runnable`, nhưng phương thức `call()` của nó có thể trả về một giá trị kiểu `V` và ném ra exception.
  * **`Future<V>`:** Đại diện cho kết quả của một phép tính bất đồng bộ. Khi bạn `submit()` một `Callable` vào `ExecutorService`, nó sẽ trả về một `Future`.
      * `isDone()`: Kiểm tra xem tác vụ đã hoàn thành chưa.
      * `get()`: Lấy kết quả. Phương thức này sẽ **block** (chặn) luồng hiện tại cho đến khi kết quả sẵn sàng. Có phiên bản `get(long timeout, TimeUnit unit)` để chờ có thời hạn.
      * `cancel(boolean mayInterruptIfRunning)`: Cố gắng hủy tác vụ.

**Ví dụ:**

```java
import java.util.concurrent.*;

public class CallableDemo {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executor = Executors.newSingleThreadExecutor();

        // Định nghĩa một Callable trả về String
        Callable<String> task = () -> {
            System.out.println("Callable task started...");
            Thread.sleep(2000); // Giả lập công việc tốn thời gian
            System.out.println("Callable task finished.");
            return "Result from Callable";
        };

        System.out.println("Submitting Callable task...");
        Future<String> future = executor.submit(task);

        System.out.println("Task submitted. Main thread continues execution...");

        // Làm việc khác trong khi task chạy ngầm...
        Thread.sleep(500);
        System.out.println("Main thread doing other work...");

        // Lấy kết quả (sẽ block nếu task chưa xong)
        System.out.println("Trying to get the result...");
        String result = future.get(); // Blocking call
        System.out.println("Result received: " + result);

        executor.shutdown();
    }
}
```

## 9\. Các Cơ chế Đồng bộ hóa Nâng cao (`java.util.concurrent.locks`)

Gói `java.util.concurrent.locks` cung cấp các cơ chế khóa linh hoạt và mạnh mẽ hơn so với `synchronized`.

  * **`Lock` interface:** Interface cơ bản cho các đối tượng khóa.

      * `lock()`: Giành khóa (block nếu khóa đang bị giữ).
      * `unlock()`: Giải phóng khóa. **Quan trọng:** Phải luôn gọi `unlock()` trong khối `finally` để đảm bảo khóa được giải phóng ngay cả khi có exception xảy ra.
      * `tryLock()`: Thử giành khóa. Trả về `true` nếu thành công, `false` nếu khóa đang bị giữ (không block).
      * `tryLock(long time, TimeUnit unit)`: Thử giành khóa trong một khoảng thời gian chờ.

  * **`ReentrantLock` class:** Một triển khai của `Lock` cung cấp các tính năng tương tự `synchronized` (khả năng tái nhập - reentrancy: một luồng đã giữ khóa có thể giành lại khóa đó nhiều lần) cộng thêm:

      * Khả năng tạo khóa công bằng (fair lock - ưu tiên luồng chờ lâu nhất) hoặc không công bằng (non-fair lock - mặc định, hiệu năng thường tốt hơn).
      * Khả năng `tryLock()` có timeout.
      * Khả năng `lockInterruptibly()`: Giành khóa nhưng cho phép luồng bị ngắt trong khi chờ.

**Mẫu sử dụng `ReentrantLock`:**

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class Counter {
    private final Lock lock = new ReentrantLock(); // Tạo lock
    private int count = 0;

    public void increment() {
        lock.lock(); // Giành khóa
        try {
            // --- Critical Section ---
            count++;
            // --- End Critical Section ---
        } finally {
            lock.unlock(); // **Luôn giải phóng khóa trong finally**
        }
    }

    public int getCount() {
        lock.lock();
        try {
            return count;
        } finally {
            lock.unlock();
        }
    }
}
```

  * **`ReadWriteLock` interface:** Cho phép nhiều luồng đọc đồng thời nhưng chỉ một luồng ghi tại một thời điểm. Hữu ích khi số lượng thao tác đọc nhiều hơn đáng kể so với thao tác ghi.
      * `readLock()`: Trả về `Lock` cho thao tác đọc.
      * `writeLock()`: Trả về `Lock` cho thao tác ghi.
  * **`ReentrantReadWriteLock` class:** Triển khai của `ReadWriteLock`.

## 10\. Các Lớp Tiện ích Đồng bộ khác (`java.util.concurrent`)

  * **`Semaphore`:** Giới hạn số lượng luồng có thể truy cập đồng thời vào một tài nguyên hoặc thực hiện một hành động cụ thể. Giống như một tập hợp các giấy phép (permits). `acquire()` để lấy giấy phép (block nếu hết), `release()` để trả lại.
  * **`CountDownLatch`:** Cho phép một hoặc nhiều luồng chờ đợi cho đến khi một tập hợp các thao tác đang thực hiện trong các luồng khác hoàn thành. Đếm ngược từ một giá trị ban đầu. `await()` để chờ đếm về 0, `countDown()` để giảm bộ đếm. Không thể tái sử dụng sau khi đếm về 0.
  * **`CyclicBarrier`:** Cho phép một nhóm các luồng chờ đợi lẫn nhau tại một điểm hẹn chung (barrier point) trước khi tất cả cùng tiếp tục. Có thể tái sử dụng sau khi các luồng vượt qua điểm hẹn. Có thể tùy chọn chạy một hành động (barrier action) khi tất cả luồng đã đến điểm hẹn.
  * **`Phaser`:** Một cơ chế đồng bộ hóa linh hoạt hơn `CountDownLatch` và `CyclicBarrier`, cho phép số lượng luồng tham gia thay đổi động.

## 11\. Bộ sưu tập Đồng bộ (Concurrent Collections)

Gói `java.util.concurrent` cung cấp các phiên bản thread-safe của các Collection API tiêu chuẩn, được tối ưu hóa cho hiệu năng trong môi trường đa luồng. Nên **ưu tiên sử dụng chúng** thay vì các collection cũ được bao bọc bởi `Collections.synchronizedMap()`, `Collections.synchronizedList()`, v.v. (vì các lớp cũ thường khóa toàn bộ collection cho mọi thao tác, gây tắc nghẽn).

  * **`ConcurrentHashMap`:** Phiên bản thread-safe và hiệu năng cao của `HashMap`. Sử dụng kỹ thuật locking tinh vi hơn (lock striping) cho phép nhiều luồng sửa đổi map đồng thời trên các phần khác nhau.
  * **`CopyOnWriteArrayList`:** Phiên bản thread-safe của `ArrayList`. Mọi thao tác sửa đổi (add, set, remove) sẽ tạo ra một bản sao mới của mảng bên trong. Thích hợp cho các danh sách mà số lượng thao tác đọc **vượt trội** so với thao tác ghi. Iterator của nó không bao giờ ném `ConcurrentModificationException`.
  * **`CopyOnWriteArraySet`:** Tương tự `CopyOnWriteArrayList` nhưng dành cho `Set`.
  * **Blocking Queues:** Các interface và lớp Queue được thiết kế cho môi trường producer-consumer:
      * `BlockingQueue<E>` interface: Mở rộng `Queue`, cung cấp các thao tác `put(E e)` (block nếu queue đầy) và `take()` (block nếu queue rỗng).
      * `ArrayBlockingQueue<E>`: Queue chặn dựa trên mảng, kích thước cố định.
      * `LinkedBlockingQueue<E>`: Queue chặn dựa trên danh sách liên kết, kích thước tùy chọn (có thể không giới hạn).
      * `PriorityBlockingQueue<E>`: Queue chặn không giới hạn, các phần tử được sắp xếp theo thứ tự ưu tiên.
      * `SynchronousQueue<E>`: Queue chặn có kích thước bằng 0. Mỗi thao tác `put` phải chờ một thao tác `take` tương ứng và ngược lại. Hữu ích cho việc chuyển giao trực tiếp giữa các luồng.

## 12\. Xử lý Ngoại lệ trong Thread

Ngoại lệ không được bắt (uncaught exception) trong phương thức `run()` của một luồng sẽ khiến luồng đó kết thúc (`TERMINATED`). Nó không tự động lan truyền đến luồng đã tạo ra nó. Để xử lý các ngoại lệ này:

1.  **Sử dụng `try-catch` bên trong `run()`:** Cách phổ biến nhất.
2.  **Sử dụng `Thread.UncaughtExceptionHandler`:**
      * Cài đặt một trình xử lý mặc định cho tất cả các luồng: `Thread.setDefaultUncaughtExceptionHandler(...)`.
      * Cài đặt một trình xử lý riêng cho một luồng cụ thể: `thread.setUncaughtExceptionHandler(...)`.
        Trình xử lý này sẽ được gọi khi một luồng kết thúc do có ngoại lệ không được bắt.
3.  **Khi sử dụng `ExecutorService` và `Future`:** Nếu tác vụ ( `Runnable` hoặc `Callable`) ném ra ngoại lệ, ngoại lệ đó sẽ được gói trong một `ExecutionException` và được ném ra khi bạn gọi phương thức `Future.get()`.

## 13\. Các Phương pháp Tốt nhất (Best Practices) và Mẹo

  * **Ưu tiên `Runnable`/`Callable` và `ExecutorService`:** Sử dụng khung Executor thay vì quản lý luồng thủ công bằng `new Thread()`. Nó linh hoạt, hiệu quả và dễ quản lý hơn.
  * **Sử dụng `java.util.concurrent`:** Tận dụng các lớp tiện ích đồng bộ hóa (Locks, Semaphores, Latches, Barriers) và bộ sưu tập đồng bộ (Concurrent Collections) vì chúng thường hiệu quả và an toàn hơn các cơ chế cũ.
  * **Giảm thiểu phạm vi đồng bộ hóa:** Chỉ khóa những phần mã thực sự cần thiết (critical sections) để tối đa hóa khả năng thực thi song song. Tránh khóa toàn bộ phương thức nếu không cần thiết.
  * **Sử dụng khóa riêng (Private Locks):** Cân nhắc sử dụng các đối tượng `Object` riêng biệt làm lock thay vì khóa trên `this` hoặc đối tượng `Class` để tránh các vấn đề không mong muốn nếu các lớp khác cũng khóa trên cùng đối tượng đó.
  * **Tránh Deadlock:** Tuân thủ thứ tự khóa nhất quán, sử dụng `tryLock()` với timeout nếu có thể.
  * **Xử lý `InterruptedException` đúng cách:** Khi bắt `InterruptedException` (thường từ `sleep()`, `wait()`, `join()`), đừng bỏ qua nó một cách im lặng. Ít nhất hãy khôi phục trạng thái ngắt bằng cách gọi `Thread.currentThread().interrupt()` để mã cấp cao hơn có thể biết luồng đã bị yêu cầu dừng.
  * **Cung cấp cơ chế dừng luồng (Graceful Shutdown):** Tránh sử dụng phương thức `Thread.stop()` (đã bị deprecated và không an toàn). Thay vào đó, sử dụng một biến cờ `volatile boolean` hoặc cơ chế ngắt (`interrupt()`) để báo hiệu cho luồng rằng nó nên dừng lại một cách an toàn (dọn dẹp tài nguyên nếu cần trước khi thoát khỏi `run()`). `ExecutorService.shutdown()` và `shutdownNow()` cung cấp cách tốt để dừng các luồng trong pool.
  * **Đặt tên cho luồng:** Sử dụng hàm khởi tạo `Thread(Runnable target, String name)` hoặc `thread.setName()` để đặt tên có ý nghĩa cho luồng. Điều này cực kỳ hữu ích khi gỡ lỗi và phân tích stack trace hoặc dump luồng.
  * **Cẩn thận với `volatile`:** Hiểu rõ giới hạn của `volatile` (không đảm bảo atomicity cho thao tác phức hợp). Sử dụng các lớp `Atomic*` hoặc `synchronized`/`Lock` khi cần tính nguyên tố.
  * **Sử dụng Thread Pool đúng cách:** Chọn loại Thread Pool phù hợp (fixed, cached, scheduled) và cấu hình kích thước pool hợp lý dựa trên loại tác vụ (CPU-bound hay I/O-bound) và tài nguyên hệ thống.
  * **Cân nhắc Thread Safety của thư viện bên ngoài:** Khi sử dụng thư viện của bên thứ ba trong môi trường đa luồng, hãy kiểm tra tài liệu của chúng để đảm bảo chúng là thread-safe hoặc áp dụng các biện pháp đồng bộ hóa cần thiết.

## 14\. Gỡ lỗi (Debugging) và Phân tích Luồng

  * **Stack Traces:** Khi có lỗi xảy ra, stack trace sẽ chỉ ra luồng nào gây ra lỗi và vị trí trong mã nguồn.
  * **Debuggers:** Hầu hết các IDE (IntelliJ IDEA, Eclipse) đều hỗ trợ gỡ lỗi đa luồng mạnh mẽ, cho phép bạn tạm dừng/tiếp tục từng luồng, kiểm tra trạng thái biến của từng luồng, phát hiện deadlock.
  * **Thread Dumps:** Là ảnh chụp nhanh (snapshot) trạng thái của tất cả các luồng trong JVM tại một thời điểm. Rất hữu ích để phân tích deadlock, livelock, hoặc các vấn đề về hiệu năng. Có thể tạo thread dump bằng các công cụ như `jstack` (đi kèm JDK), JVisualVM, hoặc tính năng tích hợp trong IDE.
  * **Logging:** Ghi log chi tiết với thông tin về ID hoặc tên luồng (`Thread.currentThread().getName()`) giúp theo dõi luồng thực thi và xác định vấn đề.
