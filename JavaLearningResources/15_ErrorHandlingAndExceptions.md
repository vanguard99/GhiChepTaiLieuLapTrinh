# GỠ LỖI VÀ XỬ LÝ NGOẠI LỆ

Tài liệu này cung cấp kiến thức toàn diện về hai kỹ năng thiết yếu cho mọi lập trình viên Java: gỡ lỗi (debugging) để xác định và sửa chữa các vấn đề trong mã nguồn, và xử lý ngoại lệ (exception handling) để xây dựng các ứng dụng mạnh mẽ, có khả năng phục hồi sau lỗi.

## 1\. Gỡ lỗi (Debugging) trong Java

**Debugging** là quá trình phân tích, tìm kiếm và loại bỏ các lỗi (bugs) trong mã nguồn phần mềm. Lỗi có thể xuất hiện dưới nhiều hình thức, ảnh hưởng đến tính đúng đắn, hiệu năng hoặc sự ổn định của ứng dụng.

### 1.1. Phân loại Lỗi thường gặp

Trong quá trình phát triển, lập trình viên thường đối mặt với ba loại lỗi chính:

1.  **Lỗi Cú pháp (Syntax Errors):** Đây là lỗi dễ phát hiện nhất, xảy ra khi mã nguồn vi phạm quy tắc ngữ pháp của ngôn ngữ lập trình (ví dụ: thiếu dấu chấm phẩy `;`, sai tên từ khóa, dấu ngoặc không khớp). Trình biên dịch (`compiler`) sẽ ngay lập tức báo lỗi và chỉ rõ vị trí, khiến chương trình không thể biên dịch được[cite: 4, 24].
    ```java
    // Ví dụ lỗi cú pháp: Thiếu dấu chấm phẩy
    int number = 5 // Lỗi! Thiếu ; ở cuối
    System.out.println(number);
    ```
2.  **Lỗi Thực thi (Runtime Errors):** Những lỗi này chỉ xuất hiện khi chương trình đang chạy (`runtime`), thường do các tình huống không lường trước hoặc thao tác không hợp lệ gây ra (ví dụ: chia một số cho 0, truy cập vào một đối tượng `null`, cố gắng mở một file không tồn tại). Khi xảy ra lỗi thực thi, máy ảo Java (JVM) thường sẽ dừng chương trình và hiển thị một `stack trace` để chỉ ra nguồn gốc lỗi[cite: 4, 26].
    ```java
    // Ví dụ lỗi thực thi: Chia cho 0
    int a = 10;
    int b = 0;
    int result = a / b; // Sẽ gây ra ArithmeticException khi chạy
    ```
3.  **Lỗi Logic (Logical Errors):** Đây là loại lỗi khó phát hiện nhất vì chương trình vẫn biên dịch và chạy mà không báo lỗi, nhưng kết quả đầu ra lại không đúng như mong đợi[cite: 4, 28]. Lỗi logic xuất phát từ những sai sót trong thuật toán, cách xử lý dữ liệu hoặc luồng điều khiển của chương trình[cite: 4, 29]. Việc tìm ra lỗi logic đòi hỏi sự phân tích kỹ lưỡng về cách chương trình hoạt động[cite: 4].
    ```java
    // Ví dụ lỗi logic: Tính sai diện tích hình chữ nhật
    int length = 5;
    int width = 10;
    // Sai lầm: Cộng thay vì nhân
    int area = length + width;
    System.out.println("Diện tích: " + area); // Kết quả sai (15 thay vì 50)
    ```

### 1.2. Các Phương pháp Debugging Hiệu quả

Có nhiều kỹ thuật khác nhau để tìm và sửa lỗi, việc lựa chọn phụ thuộc vào bản chất của lỗi và công cụ sẵn có:

1.  **Đọc và Phân tích Mã nguồn (Hand-tracing):** Xem xét kỹ lưỡng từng dòng mã, theo dõi luồng thực thi và sự thay đổi giá trị của các biến bằng tay (hoặc trên giấy). Phương pháp này hiệu quả với các đoạn mã nhỏ hoặc các thuật toán phức tạp cần hiểu rõ từng bước[cite: 5].
2.  **Sử dụng Câu lệnh In (Print Statements):** Chèn các lệnh như `System.out.println()` vào những vị trí quan trọng trong mã để in ra giá trị của biến, trạng thái đối tượng hoặc thông báo xác nhận luồng thực thi đã đi qua điểm đó. Đây là cách đơn giản và nhanh chóng để kiểm tra giả định và xác định vị trí lỗi, mặc dù có thể làm "rối" mã nguồn nếu lạm dụng[cite: 5].
    ```java
    public int calculateSum(int[] numbers) {
        int sum = 0;
        System.out.println("Bắt đầu tính tổng..."); // In thông báo debug
        for (int number : numbers) {
            System.out.println("Đang cộng số: " + number); // In giá trị biến
            sum += number;
            System.out.println("Tổng hiện tại: " + sum); // In giá trị biến
        }
        System.out.println("Hoàn thành tính tổng."); // In thông báo debug
        return sum;
    }
    ```
3.  **Sử dụng Debugger:** Đây là công cụ mạnh mẽ nhất, cho phép lập trình viên "dừng" chương trình tại các điểm cụ thể (`breakpoints`), thực thi từng dòng lệnh (`stepping`), kiểm tra giá trị biến và trạng thái bộ nhớ tại thời điểm đó, và xem xét `call stack` (ngăn xếp cuộc gọi hàm)[cite: 5, 6]. Hầu hết các Môi trường Phát triển Tích hợp (IDE) như IntelliJ IDEA, Eclipse, VS Code đều tích hợp sẵn debugger[cite: 5]. Java cũng cung cấp một debugger dòng lệnh là `jdb`[cite: 5].

### 1.3. Tìm hiểu về Debugger và Tính năng

Một debugger điển hình cung cấp các tính năng chính sau[cite: 6]:

  * **Breakpoints (Điểm dừng):** Đánh dấu các dòng mã cụ thể nơi chương trình sẽ tạm dừng thực thi. Điều này cho phép bạn kiểm tra trạng thái chương trình (giá trị biến, call stack) ngay tại thời điểm đó. Có nhiều loại breakpoint:
      * *Line Breakpoint:* Dừng tại một dòng cụ thể.
      * *Conditional Breakpoint:* Chỉ dừng nếu một điều kiện nào đó (biểu thức boolean) được thỏa mãn. Rất hữu ích khi lỗi chỉ xảy ra với một số giá trị cụ thể.
      * *Exception Breakpoint:* Dừng khi một loại ngoại lệ (Exception) cụ thể được ném ra, giúp xác định chính xác nơi xảy ra lỗi runtime.
  * **Stepping (Thực thi từng bước):** Sau khi dừng tại breakpoint, bạn có thể điều khiển việc thực thi tiếp theo:
      * `Step Over (F8 trong IntelliJ):` Thực thi dòng lệnh hiện tại. Nếu dòng đó là một lời gọi hàm, hàm sẽ được thực thi hoàn toàn và debugger sẽ dừng ở dòng lệnh *tiếp theo* sau lời gọi hàm đó[cite: 10]. Hữu ích khi bạn không cần đi sâu vào chi tiết của hàm được gọi.
      * `Step Into (F7 trong IntelliJ):` Nếu dòng lệnh hiện tại là một lời gọi hàm, debugger sẽ đi *vào bên trong* hàm đó và dừng ở dòng lệnh đầu tiên của hàm[cite: 10]. Hữu ích khi bạn nghi ngờ lỗi nằm trong hàm được gọi.
      * `Step Out (Shift + F8 trong IntelliJ):` Thực thi phần còn lại của hàm hiện tại và dừng ở dòng lệnh *ngay sau* lời gọi hàm này trong hàm gọi nó. Hữu ích khi bạn đã vào một hàm và muốn quay lại nhanh chóng.
      * `Resume Program (F9 trong IntelliJ):` Tiếp tục thực thi chương trình bình thường cho đến khi gặp breakpoint tiếp theo hoặc chương trình kết thúc.
  * **Variable Inspection (Kiểm tra biến):** Xem giá trị hiện tại của các biến (local, instance, static) trong phạm vi (scope) hiện tại. Bạn có thể "watch" (theo dõi) các biến cụ thể để thấy sự thay đổi của chúng qua từng bước. Một số debugger còn cho phép thay đổi giá trị biến ngay trong lúc debug để thử nghiệm các kịch bản khác nhau[cite: 6].
  * **Call Stack (Ngăn xếp cuộc gọi):** Hiển thị chuỗi các phương thức đã được gọi để đến được điểm thực thi hiện tại[cite: 6]. Phương thức được gọi gần nhất sẽ ở trên cùng. Call stack rất quan trọng để hiểu luồng chương trình và xác định cách một điểm lỗi được kích hoạt.
  * **Evaluate Expression:** Cho phép nhập và thực thi các biểu thức Java ngay trong phiên debug để kiểm tra giá trị hoặc hành vi của các đoạn mã nhỏ.

### 1.4. Phân tích Stack Trace

Khi một lỗi thực thi (runtime error) hoặc một ngoại lệ không được xử lý (uncaught exception) xảy ra, JVM sẽ in ra một `Stack Trace`. Đây là một báo cáo chi tiết về trạng thái của ngăn xếp cuộc gọi tại thời điểm xảy ra lỗi[cite: 7].

**Cấu trúc Stack Trace:**

  * Dòng đầu tiên thường chứa tên của ngoại lệ và một thông điệp mô tả lỗi.
  * Các dòng tiếp theo (`at ...`) liệt kê các phương thức trong call stack, bắt đầu từ nơi lỗi xảy ra (trên cùng) và đi ngược về phương thức `main`. Mỗi dòng chỉ rõ lớp, tên phương thức, tên file và số dòng nơi lời gọi xảy ra.

**Ví dụ Stack Trace (Lỗi chia cho 0)[cite: 9]:**

```
Exception in thread "main" java.lang.ArithmeticException: / by zero
    at com.example.Calculator.divide(Calculator.java:25) // Lỗi xảy ra tại dòng 25 của file Calculator.java, trong phương thức divide
    at com.example.MainApp.performCalculation(MainApp.java:15) // Phương thức divide được gọi từ dòng 15 của file MainApp.java
    at com.example.MainApp.main(MainApp.java:8) // Phương thức performCalculation được gọi từ dòng 8 của file MainApp.java
```

**Cách sử dụng Stack Trace[cite: 7]:**

1.  **Xác định loại lỗi:** Dòng đầu tiên cho biết loại ngoại lệ (ví dụ: `ArithmeticException`, `NullPointerException`, `ArrayIndexOutOfBoundsException`).
2.  **Xác định vị trí lỗi:** Dòng `at` đầu tiên chỉ ra chính xác nơi lỗi phát sinh trong mã nguồn của bạn.
3.  **Truy vết ngược:** Đọc các dòng `at` từ trên xuống dưới để hiểu chuỗi lời gọi hàm dẫn đến lỗi. Điều này giúp xác định bối cảnh và nguyên nhân gốc rễ.

### 1.5. Debugging với IntelliJ IDEA

IntelliJ IDEA cung cấp một bộ công cụ debugging mạnh mẽ và trực quan[cite: 10]. Các bước cơ bản:

1.  **Đặt Breakpoint:** Click vào lề trái của trình soạn thảo mã, cạnh số dòng bạn muốn chương trình dừng lại. Một chấm đỏ sẽ xuất hiện[cite: 10].
2.  **Khởi chạy chế độ Debug:** Thay vì chạy chương trình (Run), chọn chế độ Debug (thường có biểu tượng con bọ hoặc nhấn phím tắt như `Shift + F9` hoặc `^D` tùy cấu hình)[cite: 10].
3.  **Sử dụng Cửa sổ Debug:** Khi chương trình dừng tại breakpoint, cửa sổ Debug sẽ xuất hiện, hiển thị:
      * *Frames pane:* Hiển thị Call Stack.
      * *Variables pane:* Hiển thị các biến trong scope hiện tại[cite: 10].
      * *Watches pane:* Hiển thị các biến/biểu thức bạn muốn theo dõi đặc biệt.
      * *Console:* Hiển thị output của chương trình và cho phép nhập liệu (nếu cần).
4.  **Điều khiển Thực thi:** Sử dụng các nút stepping (`Step Over`, `Step Into`, `Step Out`, `Resume Program`) trên thanh công cụ Debug để kiểm soát luồng thực thi[cite: 10].

### 1.6. Mẹo Debugging và Lỗi thường gặp

  * **Hiểu rõ vấn đề:** Trước khi nhảy vào debugger, hãy cố gắng tái hiện lỗi một cách nhất quán và hiểu rõ điều kiện nào gây ra nó.
  * **Chia để trị:** Nếu lỗi xảy ra trong một đoạn mã lớn hoặc phức tạp, hãy cố gắng cô lập vấn đề bằng cách vô hiệu hóa (comment out) hoặc đơn giản hóa các phần mã không liên quan.
  * **Kiểm tra giả định:** Debugging thường là quá trình kiểm tra các giả định của bạn về cách mã hoạt động. Đừng ngại đặt breakpoint và kiểm tra giá trị biến để xác nhận chúng.
  * **Đừng bỏ qua Stack Trace:** Luôn đọc kỹ stack trace khi gặp lỗi runtime. Nó chứa thông tin vô giá.
  * **Lỗi Off-by-One:** Thường xảy ra trong vòng lặp hoặc khi truy cập mảng, ví dụ: lặp thiếu hoặc thừa một phần tử. Hãy kiểm tra kỹ điều kiện bắt đầu/kết thúc và chỉ số mảng.
  * **NullPointerException:** Một trong những lỗi phổ biến nhất trong Java. Xảy ra khi bạn cố gắng gọi phương thức hoặc truy cập thuộc tính của một biến tham chiếu đang có giá trị `null`. Luôn kiểm tra `null` trước khi sử dụng đối tượng, đặc biệt là các đối tượng trả về từ phương thức hoặc lấy từ cấu trúc dữ liệu.
  * **So sánh chuỗi sai:** Sử dụng `==` thay vì `.equals()` để so sánh nội dung của hai đối tượng `String`. `==` chỉ so sánh địa chỉ tham chiếu, trong khi `.equals()` so sánh nội dung thực tế.
  * **Rubber Duck Debugging:** Giải thích vấn đề và cách mã của bạn hoạt động (từng dòng một) cho một người khác (hoặc thậm chí một đồ vật như con vịt cao su). Quá trình giải thích này thường giúp bạn tự nhận ra lỗi logic của mình.

## 2\. Xử lý Ngoại lệ (Exception Handling) trong Java

**Ngoại lệ (Exception)** là một sự kiện xảy ra trong quá trình thực thi chương trình làm gián đoạn luồng chạy bình thường của các chỉ dẫn[cite: 21]. Java cung cấp một cơ chế mạnh mẽ để xử lý các tình huống bất thường này, giúp chương trình trở nên ổn định và đáng tin cậy hơn.

### 2.1. Tại sao cần Xử lý Ngoại lệ?

  * **Tách biệt logic xử lý lỗi:** Giữ cho mã xử lý logic chính của chương trình gọn gàng, tách biệt khỏi các đoạn mã phức tạp dùng để xử lý các tình huống lỗi có thể xảy ra.
  * **Phục hồi sau lỗi:** Cho phép chương trình bắt được lỗi, thực hiện các hành động khắc phục (ví dụ: yêu cầu người dùng nhập lại, sử dụng giá trị mặc định, ghi log lỗi) và có thể tiếp tục thực thi thay vì bị dừng đột ngột.
  * **Phân cấp và phân loại lỗi:** Cung cấp một hệ thống phân cấp các loại lỗi, cho phép xử lý cụ thể từng loại lỗi khác nhau.
  * **Truyền bá lỗi:** Cho phép một phương thức "ném" (throw) một ngoại lệ lên cho phương thức gọi nó xử lý, nếu bản thân nó không thể xử lý được lỗi đó.

### 2.2. Hệ thống phân cấp Ngoại lệ trong Java

Tất cả các lớp ngoại lệ trong Java đều kế thừa từ lớp `java.lang.Throwable`. `Throwable` có hai lớp con chính[cite: 32]:

1.  **`Error`:** Đại diện cho các vấn đề nghiêm trọng, bất thường xảy ra bên ngoài tầm kiểm soát của ứng dụng và thường không thể phục hồi (ví dụ: `OutOfMemoryError` - hết bộ nhớ, `StackOverflowError` - tràn bộ nhớ stack)[cite: 32]. Ứng dụng thường không nên cố gắng `catch` các `Error`.

2.  **`Exception`:** Đại diện cho các điều kiện lỗi mà ứng dụng *có thể* muốn bắt và xử lý[cite: 32]. `Exception` lại được chia thành hai loại chính dựa trên cách trình biên dịch xử lý chúng:

      * **Checked Exceptions (Ngoại lệ được kiểm tra):**
          * Là các ngoại lệ mà trình biên dịch *bắt buộc* bạn phải xử lý (bằng `try-catch`) hoặc khai báo ném ra (bằng `throws`)[cite: 37].
          * Thường đại diện cho các tình huống lỗi có thể lường trước và phục hồi được trong môi trường bên ngoài ứng dụng (ví dụ: `IOException` khi đọc/ghi file, `SQLException` khi tương tác database, `FileNotFoundException`)[cite: 35, 38].
          * Tất cả các lớp con trực tiếp của `Exception` (trừ `RuntimeException` và các lớp con của nó) đều là checked exceptions[cite: 38].
      * **Unchecked Exceptions (Ngoại lệ không được kiểm tra):**
          * Là các ngoại lệ mà trình biên dịch *không bắt buộc* bạn phải xử lý hoặc khai báo[cite: 40].
          * Thường đại diện cho các lỗi lập trình (logic errors) hoặc các lỗi sử dụng API không đúng cách trong chính ứng dụng (ví dụ: `NullPointerException`, `ArrayIndexOutOfBoundsException`, `ArithmeticException`, `IllegalArgumentException`)[cite: 39, 41].
          * Bao gồm lớp `RuntimeException` và tất cả các lớp con của nó[cite: 41]. Mặc dù không bắt buộc, bạn *vẫn có thể* `catch` các unchecked exceptions nếu muốn.

**Sơ đồ phân loại:**

```
                    Object
                      |
                  Throwable [cite: 32]
                  /       \
               Error      Exception [cite: 32, 33]
              /  |  \         /      \
 (OutOfMemoryError,    (IOException,   RuntimeException [cite: 32, 33]
 StackOverflowError,   SQLException,       |
   ...)              ...)              / | \
                             (NullPointerException,
                              ArrayIndexOutOfBoundsException,
                              ArithmeticException, ...) [cite: 32, 41]

          <----- Unchecked ----->   <----- Checked ----->  <-- Unchecked -->
```

### 2.3. Cơ chế Xử lý Ngoại lệ

Java cung cấp 5 từ khóa chính để làm việc với ngoại lệ: `try`, `catch`, `finally`, `throw`, `throws`.

#### 2.3.1. Khối `try-catch`

Đây là cấu trúc cơ bản nhất để xử lý ngoại lệ[cite: 42, 43].

  * **`try`:** Đặt các đoạn mã *có khả năng* ném ra ngoại lệ vào trong khối `try`[cite: 44].
  * **`catch`:** Đặt mã xử lý ngoại lệ vào trong khối `catch`. Mỗi khối `catch` được thiết kế để "bắt" một loại ngoại lệ cụ thể (hoặc các lớp con của nó)[cite: 44]. Bạn có thể có nhiều khối `catch` để xử lý các loại ngoại lệ khác nhau[cite: 51, 52].

**Cú pháp:**

```java
try {
    // Mã có thể ném ngoại lệ
    // statement1; [cite: 44]
    // statement2; [cite: 44]
    // ...
} catch (FileNotFoundException e) { // Bắt loại ngoại lệ cụ thể
    // Mã xử lý nếu FileNotFoundException xảy ra
    System.err.println("Lỗi: Không tìm thấy file. Chi tiết: " + e.getMessage());
} catch (IOException e) { // Bắt loại ngoại lệ khác (tổng quát hơn)
    // Mã xử lý nếu có lỗi IO khác xảy ra
    System.err.println("Lỗi IO: " + e.getMessage());
} catch (Exception e) { // Bắt tất cả các loại Exception khác (nên cẩn thận khi dùng) [cite: 56]
    // Mã xử lý chung cho các ngoại lệ khác
    System.err.println("Đã xảy ra lỗi không xác định: " + e.getMessage());
    e.printStackTrace(); // In stack trace để debug [cite: 72]
}
```

**Luồng thực thi[cite: 45, 47, 49, 53]:**

1.  Mã trong khối `try` được thực thi tuần tự.
2.  **Nếu không có ngoại lệ xảy ra:** Toàn bộ khối `try` thực thi xong, tất cả các khối `catch` bị bỏ qua, chương trình tiếp tục thực thi các lệnh sau khối `try-catch`[cite: 45].
3.  **Nếu một ngoại lệ xảy ra trong khối `try`:**
      * Việc thực thi trong khối `try` dừng ngay tại điểm xảy ra ngoại lệ[cite: 47, 49].
      * JVM tìm kiếm khối `catch` phù hợp (khối `catch` có kiểu ngoại lệ khớp hoặc là lớp cha của ngoại lệ được ném ra) từ trên xuống dưới.
      * Nếu tìm thấy khối `catch` phù hợp *đầu tiên*, mã trong khối `catch` đó sẽ được thực thi[cite: 53]. Sau khi khối `catch` thực thi xong, chương trình tiếp tục với lệnh *sau* toàn bộ cấu trúc `try-catch`. Các khối `catch` còn lại (nếu có) sẽ bị bỏ qua.
      * Nếu *không* tìm thấy khối `catch` nào phù hợp, ngoại lệ sẽ được "ném" ra khỏi phương thức hiện tại và truyền lên cho phương thức gọi nó xử lý (exception propagation). Nếu lên đến tận phương thức `main` mà vẫn không có gì bắt lại, chương trình sẽ kết thúc và in ra stack trace[cite: 53].

**Lưu ý về nhiều khối `catch`[cite: 51, 52, 56]:**

  * Thứ tự các khối `catch` rất quan trọng. Bạn phải đặt các khối `catch` cho các lớp ngoại lệ *cụ thể* (lớp con) *trước* các khối `catch` cho các lớp ngoại lệ *tổng quát* (lớp cha)[cite: 56]. Nếu không, khối `catch` tổng quát hơn sẽ luôn bắt được ngoại lệ trước, khiến khối `catch` cụ thể hơn không bao giờ được thực thi.
    ```java
    try {
        // ...
    } catch (Exception e) { // SAI: Bắt lớp cha trước
        System.err.println("Lỗi chung");
    } catch (FileNotFoundException e) { // Lỗi biên dịch: Khối catch này không thể đạt tới
        System.err.println("Không tìm thấy file");
    }
    ```
  * Sử dụng `catch (Exception e)` nên hạn chế vì nó che giấu các loại lỗi cụ thể, làm khó khăn cho việc xử lý chính xác và debug[cite: 56]. Chỉ dùng khi bạn thực sự muốn xử lý mọi loại `Exception` theo cùng một cách, hoặc để đảm bảo bắt được mọi lỗi không lường trước ở tầng cao nhất.

#### 2.3.2. Khối `finally`

Khối `finally` cung cấp một cơ chế để đảm bảo một đoạn mã *luôn luôn* được thực thi, bất kể có ngoại lệ xảy ra trong khối `try` hay không, và bất kể ngoại lệ đó có được bắt bởi khối `catch` hay không[cite: 57, 61].

**Cú pháp[cite: 58]:**

```java
try {
    // Mã có thể ném ngoại lệ
} catch (SpecificException e) {
    // Xử lý ngoại lệ cụ thể
} finally {
    // Mã dọn dẹp tài nguyên - LUÔN được thực thi
    // statement1; [cite: 58]
    System.out.println("Hoàn tất thao tác, đang dọn dẹp...");
}
```

**Trường hợp sử dụng chính:**

  * **Dọn dẹp tài nguyên:** Đây là mục đích phổ biến nhất của `finally`. Các tài nguyên như kết nối cơ sở dữ liệu, luồng file (file streams), kết nối mạng cần phải được đóng (`close()`) sau khi sử dụng để giải phóng bộ nhớ hệ thống và tránh rò rỉ tài nguyên. Đặt mã đóng tài nguyên vào `finally` đảm bảo chúng luôn được đóng, ngay cả khi có lỗi xảy ra.

**Luồng thực thi với `finally`[cite: 59, 61]:**

1.  Mã trong `try` thực thi.
2.  *Nếu không có ngoại lệ:* Khối `try` chạy hết -\> Khối `finally` chạy -\> Các lệnh sau `try-catch-finally` chạy.
3.  *Nếu có ngoại lệ và được `catch` bắt:* Khối `try` dừng -\> Khối `catch` phù hợp chạy -\> Khối `finally` chạy -\> Các lệnh sau `try-catch-finally` chạy.
4.  *Nếu có ngoại lệ và *không* được `catch` bắt:* Khối `try` dừng -\> Khối `finally` chạy -\> Ngoại lệ được ném ra khỏi phương thức hiện tại.
5.  *Nếu có lệnh `return`, `break`, hoặc `continue` trong `try` hoặc `catch`:* Khối `finally` vẫn sẽ được thực thi *trước khi* luồng điều khiển thực sự thoát ra khỏi cấu trúc `try-catch-finally`.

**Lưu ý:**

  * Một khối `try` phải có ít nhất một khối `catch` hoặc một khối `finally` (hoặc cả hai)[cite: 60].
  * Không thể chèn mã vào giữa `catch` và `finally`[cite: 60].

#### 2.3.3. Try-with-Resources (Từ Java 7)

Đây là một cải tiến cú pháp giúp việc quản lý tài nguyên trở nên đơn giản và an toàn hơn so với việc dùng `finally` một cách thủ công. Nó tự động đóng các tài nguyên được khai báo trong cặp ngoặc tròn `()` sau từ khóa `try`, miễn là tài nguyên đó implement giao diện `java.lang.AutoCloseable` (hầu hết các tài nguyên chuẩn như `InputStream`, `OutputStream`, `Reader`, `Writer`, `Connection`, `Statement`, `ResultSet` đều implement giao diện này).

**Cú pháp:**

```java
// Khai báo tài nguyên trong ngoặc tròn của try
try (FileInputStream fis = new FileInputStream("file.txt");
     BufferedInputStream bis = new BufferedInputStream(fis)) {

    // Sử dụng tài nguyên (fis, bis) ở đây
    int data = bis.read();
    while (data != -1) {
        System.out.print((char) data);
        data = bis.read();
    }
} catch (IOException e) {
    // Xử lý lỗi IO
    System.err.println("Lỗi đọc file: " + e.getMessage());
}
// bis và fis sẽ tự động được gọi phương thức close() tại đây,
// ngay cả khi có ngoại lệ xảy ra trong khối try hoặc không.
// Không cần khối finally để đóng tài nguyên nữa.
```

**Ưu điểm:**

  * Mã nguồn ngắn gọn, dễ đọc hơn.
  * Giảm thiểu nguy cơ quên đóng tài nguyên, tránh rò rỉ.
  * Xử lý ngoại lệ trong quá trình đóng tài nguyên tốt hơn (suppressed exceptions).

**Khuyến nghị:** Luôn ưu tiên sử dụng try-with-resources khi làm việc với các tài nguyên cần đóng.

#### 2.3.4. Từ khóa `throw`

Từ khóa `throw` được sử dụng để *chủ động* ném ra một đối tượng ngoại lệ (một instance của lớp kế thừa từ `Throwable`)[cite: 62, 65]. Điều này thường được thực hiện khi:

  * Phát hiện một tình huống lỗi logic trong phương thức mà không thể xử lý nội bộ.
  * Muốn chuyển đổi một loại ngoại lệ (ví dụ: một checked exception) thành một loại khác (ví dụ: một unchecked exception tùy chỉnh).
  * Tạo và ném ra các ngoại lệ tùy chỉnh (custom exceptions) để biểu thị các lỗi đặc thù của ứng dụng.

**Cú pháp:**

```java
public void setAge(int age) {
    if (age < 0 || age > 120) {
        // Ném ra một IllegalArgumentException nếu tuổi không hợp lệ
        throw new IllegalArgumentException("Tuổi không hợp lệ: " + age); // [cite: 62]
    }
    this.age = age;
}

public void processFile(String filename) throws CustomFileProcessingException {
    try {
        // ... mã đọc file có thể ném IOException ...
    } catch (IOException e) {
        // Chuyển đổi IOException thành ngoại lệ tùy chỉnh
        throw new CustomFileProcessingException("Lỗi khi xử lý file: " + filename, e); // Bao gồm ngoại lệ gốc (e)
    }
}
```

Khi lệnh `throw` được thực thi, luồng thực thi bình thường của phương thức sẽ dừng lại, và đối tượng ngoại lệ được ném ra sẽ được truyền đi để tìm khối `catch` phù hợp hoặc thoát ra khỏi phương thức.

#### 2.3.5. Từ khóa `throws`

Từ khóa `throws` được sử dụng trong *khai báo* (signature) của một phương thức để chỉ ra rằng phương thức đó *có thể* ném ra một hoặc nhiều loại *checked exception* cụ thể[cite: 66, 67].

**Mục đích:**

  * **Thông báo cho người gọi:** Cảnh báo cho bất kỳ ai gọi phương thức này rằng họ cần phải chuẩn bị để xử lý các loại ngoại lệ được liệt kê (bằng `try-catch`) hoặc cũng phải khai báo `throws` để "né" (propagate) ngoại lệ đó lên tầng gọi cao hơn[cite: 66, 74].
  * **Bắt buộc xử lý (đối với checked exceptions):** Trình biên dịch sẽ báo lỗi nếu một phương thức gọi một phương thức khác có khai báo `throws CheckedException` mà không xử lý hoặc khai báo `throws` tương ứng[cite: 67].

**Cú pháp:**

```java
// Phương thức này có thể ném ra IOException hoặc SQLException (đều là checked)
public void loadData(String source) throws IOException, SQLException {
    if (source.endsWith(".txt")) {
        // ... mã có thể ném IOException ...
        readFile(source);
    } else if (source.startsWith("jdbc:")) {
        // ... mã có thể ném SQLException ...
        readDatabase(source);
    } else {
        throw new IllegalArgumentException("Nguồn không được hỗ trợ: " + source); // Unchecked, không cần khai báo throws
    }
}

// Phương thức readFile (ví dụ) cũng phải khai báo throws IOException
private void readFile(String filename) throws IOException {
    // ... mã đọc file ...
}

// Phương thức gọi loadData phải xử lý hoặc khai báo throws
public void displayData() /* throws IOException, SQLException */ { // Hoặc khai báo throws
    try {
        loadData("data.txt");
    } catch (IOException | SQLException e) { // Multi-catch (từ Java 7)
        System.err.println("Không thể tải dữ liệu: " + e.getMessage());
        // Xử lý lỗi
    }
}
```

**Lưu ý:**

  * Bạn *không* cần (và thường không nên) khai báo `throws` cho các `Unchecked Exceptions` (`RuntimeException` và các lớp con của nó) hoặc `Error`[cite: 67]. Chúng có thể xảy ra ở bất cứ đâu và việc bắt buộc khai báo chúng sẽ làm mã nguồn trở nên rất dài dòng.
  * **"Né" ngoại lệ (Exception Ducking)[cite: 73, 74]:** Nếu một phương thức không muốn hoặc không thể xử lý một checked exception mà nó gặp phải (ví dụ, từ một phương thức khác mà nó gọi), nó có thể khai báo `throws` chính ngoại lệ đó để đẩy trách nhiệm xử lý lên cho phương thức gọi nó[cite: 74]. Đây là một kỹ thuật phổ biến, nhưng cần cân nhắc xem tầng nào trong ứng dụng là phù hợp nhất để xử lý một loại lỗi cụ thể.

### 2.4. Ngoại lệ Tùy chỉnh (Custom Exceptions)

Đôi khi, các lớp ngoại lệ chuẩn của Java không đủ để mô tả các tình huống lỗi đặc thù trong ứng dụng của bạn. Trong trường hợp này, bạn có thể tạo các lớp ngoại lệ tùy chỉnh bằng cách kế thừa từ `Exception` (cho checked exceptions) hoặc `RuntimeException` (cho unchecked exceptions).

**Khi nào nên tạo Custom Exception:**

  * Khi bạn muốn cung cấp thông tin lỗi chi tiết hơn hoặc ngữ cảnh cụ thể cho một loại lỗi nghiệp vụ.
  * Khi bạn muốn phân biệt các lỗi của ứng dụng/thư viện của bạn với các lỗi Java chuẩn.
  * Khi bạn muốn nhóm các loại lỗi liên quan lại với nhau dưới một lớp cha chung.

**Cách tạo:**

```java
// Custom Checked Exception
class InsufficientFundsException extends Exception {
    private double shortfall;

    public InsufficientFundsException(String message, double shortfall) {
        super(message); // Gọi constructor của lớp cha (Exception)
        this.shortfall = shortfall;
    }

    public double getShortfall() {
        return shortfall;
    }
}

// Custom Unchecked Exception
class InvalidTransactionIdException extends RuntimeException {
    public InvalidTransactionIdException(String message) {
        super(message);
    }
}

// Sử dụng Custom Exception
public void withdraw(double amount) throws InsufficientFundsException {
    if (amount < 0) {
        throw new InvalidTransactionIdException("Số tiền rút không thể âm: " + amount); // Ném Unchecked
    }
    if (this.balance < amount) {
        double needed = amount - this.balance;
        throw new InsufficientFundsException("Không đủ tiền để rút. Còn thiếu: " + needed, needed); // Ném Checked
    }
    this.balance -= amount;
}
```

### 2.5. Best Practices trong Xử lý Ngoại lệ

  * **Bắt ngoại lệ cụ thể:** Ưu tiên `catch` các lớp ngoại lệ cụ thể thay vì `catch (Exception e)` để xử lý lỗi chính xác hơn.
  * **Không "nuốt" ngoại lệ:** Tránh các khối `catch` trống hoặc chỉ in ra lỗi mà không có hành động xử lý nào khác (`e.printStackTrace()` chỉ nên dùng cho debug). Ít nhất hãy ghi log lỗi.
    ```java
    // XẤU: Nuốt ngoại lệ
    try {
        // ...
    } catch (IOException e) {
        // Trống trơn - Lỗi bị bỏ qua âm thầm!
    }

    // TỐT HƠN: Ghi log
    import java.util.logging.Logger;
    // ...
    private static final Logger LOGGER = Logger.getLogger(MyClass.class.getName());
    // ...
    try {
        // ...
    } catch (IOException e) {
        LOGGER.log(Level.SEVERE, "Lỗi IO khi thực hiện thao tác X", e);
        // Có thể ném lại một ngoại lệ khác hoặc trả về giá trị lỗi
    }
    ```
  * **Sử dụng try-with-resources:** Luôn dùng cấu trúc này để quản lý tài nguyên `AutoCloseable`.
  * **Ném lại ngoại lệ một cách có ý nghĩa:** Khi bắt một ngoại lệ cấp thấp và ném lại một ngoại lệ cấp cao hơn (thường là custom exception), hãy đảm bảo bao gồm ngoại lệ gốc (`cause`) để không làm mất thông tin debug quan trọng.
    ```java
    catch (SQLException e) {
        throw new DataAccessException("Lỗi truy cập dữ liệu người dùng", e); // Truyền e làm cause
    }
    ```
  * **Đừng dùng ngoại lệ cho luồng điều khiển thông thường:** Ngoại lệ dành cho các tình huống *bất thường*. Việc sử dụng `try-catch` để kiểm soát luồng logic thông thường (ví dụ: kiểm tra xem người dùng nhập số hay chữ) thường kém hiệu quả và làm mã khó hiểu hơn so với việc dùng các câu lệnh điều kiện (`if-else`).
  * **Tạo custom exceptions khi cần thiết:** Sử dụng chúng để làm rõ ngữ nghĩa lỗi trong domain của bạn.
  * **Ghi tài liệu (@throws):** Sử dụng Javadoc tag `@throws` để ghi lại các *checked exceptions* mà phương thức của bạn có thể ném ra (hoặc các unchecked exceptions quan trọng mà người gọi cần biết).
    ```java
    /**
     * Tải thông tin sản phẩm từ ID.
     *
     * @param productId ID sản phẩm.
     * @return Đối tượng Product.
     * @throws ProductNotFoundException Nếu không tìm thấy sản phẩm với ID đã cho.
     * @throws DataAccessException Nếu có lỗi kết nối cơ sở dữ liệu.
     */
    public Product loadProduct(int productId) throws ProductNotFoundException, DataAccessException {
        // ...
    }
    ```
  * **Xử lý ngoại lệ ở đúng tầng:** Quyết định xem tầng nào trong ứng dụng (UI, Business Logic, Data Access) là phù hợp nhất để bắt và xử lý một loại ngoại lệ cụ thể. Ví dụ, lỗi kết nối database có thể được bắt ở tầng Data Access và chuyển thành một `DataAccessException`, sau đó tầng UI có thể bắt `DataAccessException` này để hiển thị thông báo thân thiện cho người dùng.
  * 