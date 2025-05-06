# THAO TÁC FILE TEXT

## **1. Giới thiệu**

Thao tác với file là một nhu cầu cơ bản và phổ biến trong hầu hết các ứng dụng phần mềm. Cho dù đó là đọc file cấu hình, ghi log hoạt động, xử lý dữ liệu đầu vào, hay lưu trữ kết quả đầu ra, việc đọc và ghi file văn bản (text file) là một kỹ năng cốt lõi đối với lập trình viên Java.

Java cung cấp một bộ thư viện I/O (Input/Output) phong phú để làm việc với nhiều loại nguồn dữ liệu khác nhau, bao gồm cả file hệ thống. Ban đầu, gói `java.io` là API chính cho các thao tác này. Tuy nhiên, kể từ Java 7, gói **NIO.2 (New I/O 2)**, chủ yếu nằm trong `java.nio.file`, đã được giới thiệu, mang lại nhiều cải tiến về hiệu năng, tính năng và cách tiếp cận hiện đại hơn so với API `java.io` truyền thống.

Hướng dẫn này sẽ cung cấp cái nhìn chi tiết về cả hai cách tiếp cận (legacy `java.io` và modern `java.nio.file`) để đọc và ghi file text trong Java. Chúng ta sẽ khám phá các khái niệm cơ bản, các lớp và phương thức quan trọng, cách xử lý lỗi, các phương pháp hay nhất (best practices), và các ví dụ mã nguồn cụ thể.

## **2. Các Khái niệm Cốt lõi về Java I/O**

### **2.1. Stream (Luồng dữ liệu)**

Trong Java I/O, **Stream** là một khái niệm trừu tượng đại diện cho một chuỗi các dữ liệu chảy từ một **nguồn (Source)** đến một **đích (Destination)**.

  * **Input Stream (Luồng vào):** Đọc dữ liệu từ một nguồn (ví dụ: file, bàn phím, network socket). Chương trình nhận dữ liệu từ luồng này.
  * **Output Stream (Luồng ra):** Ghi dữ liệu đến một đích (ví dụ: file, màn hình console, network socket). Chương trình gửi dữ liệu vào luồng này.

### **2.2. Luồng Byte (Byte Streams) vs. Luồng Ký tự (Character Streams)**

Java phân biệt hai loại luồng chính:

  * **Luồng Byte (`InputStream` / `OutputStream`):** Xử lý dữ liệu dưới dạng các byte thô (8-bit). Chúng phù hợp để làm việc với mọi loại dữ liệu, đặc biệt là dữ liệu nhị phân (binary data) như file ảnh, file thực thi, hoặc dữ liệu đã được serialization. Các lớp cơ sở là `java.io.InputStream` và `java.io.OutputStream`.
  * **Luồng Ký tự (`Reader` / `Writer`):** Xử lý dữ liệu dưới dạng các ký tự (characters - thường là 16-bit trong Java để hỗ trợ Unicode). Chúng được thiết kế đặc biệt để làm việc với dữ liệu văn bản (text data). Chúng tự động xử lý việc chuyển đổi giữa byte và ký tự dựa trên một **bộ mã hóa ký tự (character encoding)** cụ thể. Các lớp cơ sở là `java.io.Reader` và `java.io.Writer`.

*Quan trọng:* Khi làm việc với file text, **luôn ưu tiên sử dụng Luồng Ký tự (`Reader`/`Writer`)** để đảm bảo xử lý đúng các bộ mã hóa ký tự khác nhau.

### **2.3. Bộ Mã hóa Ký tự (Character Encoding)**

File text lưu trữ văn bản dưới dạng các byte. Để diễn giải các byte này thành ký tự mà con người có thể đọc được (và ngược lại), cần có một quy tắc ánh xạ gọi là **bộ mã hóa ký tự** (ví dụ: `UTF-8`, `UTF-16`, `ISO-8859-1`, `Windows-1252`).

  * **Vấn đề:** Nếu bạn đọc một file text sử dụng bộ mã hóa khác với bộ mã hóa mà file đó được ghi, bạn có thể gặp lỗi hiển thị ký tự (các ký tự lạ, dấu hỏi, ô vuông).
  * **Giải pháp:** Luôn chỉ định rõ ràng bộ mã hóa ký tự khi đọc và ghi file text. `UTF-8` là bộ mã hóa phổ biến và được khuyến nghị sử dụng trong hầu hết các trường hợp hiện đại vì khả năng biểu diễn hầu hết các ký tự trên thế giới và tính tương thích cao.
  * **Mặc định của Hệ thống:** Nếu không chỉ định bộ mã hóa, Java sẽ sử dụng bộ mã hóa mặc định của hệ điều hành, điều này có thể dẫn đến hành vi không nhất quán trên các môi trường khác nhau.

### **2.4. Gói `java.io` và `java.nio.file`**

  * **`java.io` (Legacy I/O):**
      * Cung cấp các lớp cơ bản như `File`, `FileInputStream`, `FileOutputStream`, `FileReader`, `FileWriter`, `BufferedReader`, `BufferedWriter`.
      * Lớp `java.io.File` đại diện cho một đường dẫn file hoặc thư mục nhưng có một số hạn chế (ví dụ: xử lý lỗi kém, không hỗ trợ đầy đủ thuộc tính file, liên kết tượng trưng).
      * Các thao tác I/O thường là blocking (luồng thực thi bị chặn cho đến khi thao tác hoàn tất).
  * **`java.nio.file` (NIO.2 - Modern I/O):**
      * Giới thiệu các khái niệm chính: `Path`, `Paths`, `Files`.
      * `Path`: Đại diện cho đường dẫn hệ thống file, linh hoạt và mạnh mẽ hơn `File`.
      * `Paths`: Lớp tiện ích để tạo đối tượng `Path` từ chuỗi hoặc URI.
      * `Files`: Lớp tiện ích chứa rất nhiều phương thức tĩnh hữu ích để thực hiện các thao tác file phổ biến (kiểm tra tồn tại, đọc, ghi, xóa, sao chép, di chuyển, quản lý thuộc tính, duyệt cây thư mục...) một cách ngắn gọn và hiệu quả hơn.
      * Hỗ trợ tốt hơn cho các hệ thống file khác nhau, thuộc tính file mở rộng, liên kết tượng trưng (symbolic links), và non-blocking I/O (trong gói `java.nio` nói chung, mặc dù nhiều phương thức tiện ích trong `Files` vẫn là blocking).

*Khuyến nghị:* **Ưu tiên sử dụng API `java.nio.file` (NIO.2)** cho các thao tác file trong các dự án Java hiện đại vì tính nhất quán, nhiều tính năng và xử lý lỗi tốt hơn. Tuy nhiên, hiểu biết về `java.io` vẫn cần thiết vì nhiều thư viện và mã nguồn cũ vẫn sử dụng nó.

## **3. Làm việc với Files và Thư mục**

### **3.1. Đại diện cho Đường dẫn: `File` vs. `Path`**

  * **`java.io.File` (Legacy):**
    ```java
    // Tạo đối tượng File
    java.io.File legacyFile = new java.io.File("data/input.txt");
    java.io.File legacyDir = new java.io.File("data/output");
    ```
  * **`java.nio.file.Path` (Modern):**
    ```java
    // Sử dụng Paths để lấy đối tượng Path
    java.nio.file.Path modernPath = java.nio.file.Paths.get("data/input.txt");
    java.nio.file.Path modernDir = java.nio.file.Paths.get("data", "output"); // Có thể nối các phần
    ```
    Đối tượng `Path` cung cấp nhiều phương thức hữu ích để thao tác với đường dẫn (lấy tên file, thư mục cha, nối đường dẫn, chuẩn hóa...).

### **3.2. Các Thao tác Cơ bản (Sử dụng `Files` và `Path`)**

Lớp `java.nio.file.Files` cung cấp các phương thức tĩnh tiện lợi:

```java
import java.nio.file.*;
import java.io.IOException;

public class FileOperationsNIO {
    public static void main(String[] args) {
        Path filePath = Paths.get("data/myfile.txt");
        Path dirPath = Paths.get("data/new_directory");

        try {
            // 1. Kiểm tra sự tồn tại
            boolean fileExists = Files.exists(filePath);
            System.out.println("File exists: " + fileExists); // false (nếu chưa tạo)

            boolean dirExists = Files.exists(dirPath);
            System.out.println("Directory exists: " + dirExists); // false

            // 2. Tạo thư mục
            if (!dirExists) {
                Files.createDirectories(dirPath); // Tạo thư mục, bao gồm cả thư mục cha nếu cần
                System.out.println("Directory created: " + dirPath.toAbsolutePath());
            }

            // 3. Tạo file (nếu chưa tồn tại)
            if (!fileExists) {
                Files.createFile(filePath);
                System.out.println("File created: " + filePath.toAbsolutePath());
            }

            // 4. Kiểm tra thuộc tính
            System.out.println("Is file regular file? " + Files.isRegularFile(filePath)); // true
            System.out.println("Is directory? " + Files.isDirectory(dirPath));         // true
            System.out.println("Is readable? " + Files.isReadable(filePath));       // true (thường là vậy)
            System.out.println("Is writable? " + Files.isWritable(filePath));       // true (thường là vậy)
            System.out.println("File size: " + Files.size(filePath) + " bytes");     // 0 (vì mới tạo)

            // 5. Xóa file
            // boolean deleted = Files.deleteIfExists(filePath);
            // System.out.println("File deleted: " + deleted);

        } catch (IOException e) {
            System.err.println("An I/O error occurred: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

*Lưu ý:* Các phương thức trong `Files` thường ném `IOException` (hoặc các lớp con của nó như `NoSuchFileException`, `DirectoryNotEmptyException`), cần được xử lý bằng `try-catch`.

## **4. Đọc File Text**

### **4.1. Sử dụng `java.io` (FileReader & BufferedReader)**

Đây là cách tiếp cận truyền thống, thường kết hợp `FileReader` (đọc byte từ file và chuyển thành ký tự bằng encoding mặc định) với `BufferedReader` (đọc text hiệu quả hơn nhờ bộ đệm và cung cấp phương thức `readLine()`).

**Quan trọng:** `FileReader` sử dụng bộ mã hóa mặc định của hệ thống. Để chỉ định bộ mã hóa (ví dụ: UTF-8), bạn cần dùng `InputStreamReader` kết hợp với `FileInputStream`.

```java
import java.io.*;
import java.nio.charset.StandardCharsets; // Để chỉ định UTF-8

public class ReadTextFileLegacy {
    public static void main(String[] args) {
        String filename = "data/myfile.txt"; // Giả sử file này đã có nội dung

        // Cách 1: Dùng FileReader (sử dụng encoding mặc định - không khuyến khích)
        System.out.println("--- Reading with FileReader (default encoding) ---");
        try (BufferedReader reader = new BufferedReader(new FileReader(filename))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.err.println("Error reading file with default encoding: " + e.getMessage());
        }

        // Cách 2: Dùng InputStreamReader để chỉ định Encoding (khuyến khích)
        System.out.println("\n--- Reading with InputStreamReader (UTF-8) ---");
        File file = new File(filename);
        // Sử dụng try-with-resources để đảm bảo file được đóng tự động
        try (BufferedReader reader = new BufferedReader(
                new InputStreamReader(new FileInputStream(file), StandardCharsets.UTF_8)))
        {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.err.println("Error reading file with UTF-8: " + e.getMessage());
            e.printStackTrace(); // In chi tiết lỗi
        }
    }
}
```

  * **`try-with-resources`:** Cú pháp này (từ Java 7) đảm bảo rằng các tài nguyên như `BufferedReader`, `InputStreamReader`, `FileInputStream` sẽ được đóng (`close()`) tự động sau khi khối `try` kết thúc, ngay cả khi có ngoại lệ xảy ra. Đây là cách **best practice** để quản lý tài nguyên I/O.

### **4.2. Sử dụng `java.nio.file` (Files)**

API NIO.2 cung cấp các cách đọc file text ngắn gọn và hiệu quả hơn nhiều.

```java
import java.nio.file.*;
import java.nio.charset.StandardCharsets;
import java.io.BufferedReader;
import java.io.IOException;
import java.util.List;
import java.util.stream.Stream; // Java Stream API

public class ReadTextFileNIO {
    public static void main(String[] args) {
        Path filePath = Paths.get("data", "myfile.txt"); // Giả sử file có nội dung

        // Cách 1: Đọc toàn bộ file vào List<String> (Phù hợp file nhỏ)
        System.out.println("--- Reading all lines into List (UTF-8) ---");
        try {
            List<String> lines = Files.readAllLines(filePath, StandardCharsets.UTF_8);
            for (String line : lines) {
                System.out.println(line);
            }
            // Hoặc dùng lambda: lines.forEach(System.out::println);
        } catch (IOException e) {
            System.err.println("Error reading all lines: " + e.getMessage());
        }

        // Cách 2: Sử dụng BufferedReader (Tương tự cách legacy nhưng ngắn gọn hơn)
        System.out.println("\n--- Reading with Files.newBufferedReader (UTF-8) ---");
        // try-with-resources vẫn rất quan trọng
        try (BufferedReader reader = Files.newBufferedReader(filePath, StandardCharsets.UTF_8)) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.err.println("Error reading with BufferedReader: " + e.getMessage());
        }

        // Cách 3: Sử dụng Stream API (Phù hợp xử lý file lớn, lazy loading)
        System.out.println("\n--- Reading with Files.lines (UTF-8) ---");
        // try-with-resources cho Stream<String> để đóng file handle
        try (Stream<String> stream = Files.lines(filePath, StandardCharsets.UTF_8)) {
            stream.forEach(System.out::println); // Xử lý từng dòng
            // Có thể thực hiện các thao tác stream khác: filter, map, collect...
            // long count = stream.filter(line -> line.contains("error")).count();
        } catch (IOException e) {
            System.err.println("Error reading with Stream: " + e.getMessage());
        }
    }
}
```

  * `Files.readAllLines()`: Tiện lợi nhưng đọc toàn bộ nội dung file vào bộ nhớ, không phù hợp với file quá lớn.
  * `Files.newBufferedReader()`: Cách tiếp cận cân bằng, sử dụng bộ đệm, đọc từng dòng, phù hợp nhiều kích thước file.
  * `Files.lines()`: Sử dụng Java Stream API, đọc dữ liệu một cách "lười biếng" (lazy), rất hiệu quả cho các file lớn và cho phép xử lý dữ liệu theo phong cách functional programming. Cần đóng stream bằng `try-with-resources`.

## **5. Ghi File Text**

### **5.1. Sử dụng `java.io` (FileWriter & BufferedWriter)**

Tương tự như đọc, `FileWriter` dùng encoding mặc định. Nên dùng `OutputStreamWriter` kết hợp `FileOutputStream` để chỉ định encoding. `BufferedWriter` giúp tăng hiệu quả ghi.

```java
import java.io.*;
import java.nio.charset.StandardCharsets;

public class WriteTextFileLegacy {
    public static void main(String[] args) {
        String filename = "data/output_legacy.txt";
        String content = "Đây là dòng 1 - Legacy.\nĐây là dòng 2.\n";

        // Cách 1: Dùng FileWriter (encoding mặc định - không khuyến khích)
        // FileWriter có constructor nhận tham số boolean 'append'
        // true: ghi nối vào cuối file, false (hoặc bỏ qua): ghi đè
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filename, false))) { // Ghi đè
            writer.write(content);
            writer.newLine(); // Ghi ký tự xuống dòng chuẩn của hệ điều hành
            writer.write("Dòng 3 được thêm vào.");
            System.out.println("Legacy (default): Ghi file thành công.");
        } catch (IOException e) {
            System.err.println("Error writing (default): " + e.getMessage());
        }

        // Cách 2: Dùng OutputStreamWriter (chỉ định UTF-8 - khuyến khích)
        filename = "data/output_legacy_utf8.txt";
        File file = new File(filename);
        try (BufferedWriter writer = new BufferedWriter(
                new OutputStreamWriter(new FileOutputStream(file, false), StandardCharsets.UTF_8))) // Ghi đè
        {
            writer.write("Nội dung ghi bằng UTF-8.\n");
            writer.write("Chào thế giới!");
            System.out.println("Legacy (UTF-8): Ghi file thành công.");
        } catch (IOException e) {
            System.err.println("Error writing (UTF-8): " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

  * **`BufferedWriter`:** Cung cấp phương thức `newLine()` tiện lợi để ghi ký tự xuống dòng phù hợp với hệ điều hành. Nó cũng dùng bộ đệm để ghi hiệu quả hơn (dữ liệu được ghi vào bộ đệm trước, sau đó mới thực sự ghi xuống file khi bộ đệm đầy hoặc khi `flush()` hoặc `close()` được gọi).
  * **`flush()` vs `close()`:** `flush()` đẩy dữ liệu từ bộ đệm xuống file ngay lập tức mà không đóng luồng. `close()` sẽ tự động `flush()` trước khi đóng luồng. `try-with-resources` đảm bảo `close()` được gọi.

### **5.2. Sử dụng `java.nio.file` (Files)**

NIO.2 cũng cung cấp các cách ghi file đơn giản hơn.

```java
import java.nio.file.*;
import java.nio.charset.StandardCharsets;
import java.io.BufferedWriter;
import java.io.IOException;
import java.util.Arrays;
import java.util.List;

public class WriteTextFileNIO {
    public static void main(String[] args) {
        Path filePath = Paths.get("data", "output_nio.txt");

        // Cách 1: Ghi một List<String> vào file (Ghi đè)
        System.out.println("--- Writing List<String> (UTF-8) ---");
        List<String> lines = Arrays.asList("Dòng 1 - NIO", "Dòng 2 - Viết bằng Files.write", "Kết thúc.");
        try {
            // StandardOpenOption.CREATE: Tạo file nếu chưa có
            // StandardOpenOption.WRITE: Mở file để ghi
            // StandardOpenOption.TRUNCATE_EXISTING: Xóa nội dung cũ nếu file đã tồn tại (ghi đè)
            // StandardOpenOption.APPEND: Ghi nối vào cuối file
            Files.write(filePath, lines, StandardCharsets.UTF_8,
                        StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING);
            System.out.println("NIO (List): Ghi file thành công.");
        } catch (IOException e) {
            System.err.println("Error writing List: " + e.getMessage());
        }

        // Cách 2: Sử dụng BufferedWriter (Tương tự legacy nhưng ngắn gọn hơn)
        System.out.println("\n--- Writing with Files.newBufferedWriter (UTF-8) ---");
        Path filePathAppend = Paths.get("data", "output_nio_append.txt");
        try (BufferedWriter writer = Files.newBufferedWriter(filePathAppend, StandardCharsets.UTF_8,
                                     StandardOpenOption.CREATE, StandardOpenOption.APPEND)) // Ghi nối
        {
            writer.write("Ghi thêm dòng này.");
            writer.newLine();
            writer.write("Ghi thêm dòng nữa.");
            writer.newLine();
            System.out.println("NIO (Append): Ghi file thành công.");
        } catch (IOException e) {
            System.err.println("Error writing with BufferedWriter: " + e.getMessage());
        }
         // Cách 3: Ghi một chuỗi trực tiếp (Java 11+)
        System.out.println("\n--- Writing String directly (Java 11+) ---");
         Path filePathJava11 = Paths.get("data", "output_java11.txt");
         String content = "Nội dung ghi trực tiếp từ Java 11.\nDòng thứ hai.";
         try {
            Files.writeString(filePathJava11, content, StandardCharsets.UTF_8,
                              StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING);
            System.out.println("NIO (Java 11): Ghi file thành công.");
         } catch (IOException e) {
              System.err.println("Error writing String: " + e.getMessage());
         }
    }
}
```

  * **`Files.write()` / `Files.writeString()`:** Cung cấp cách ghi đơn giản cho các trường hợp phổ biến. Tham số `OpenOption` cho phép kiểm soát chi tiết hành vi ghi (tạo mới, ghi đè, ghi nối...).
  * **`Files.newBufferedWriter()`:** Vẫn là lựa chọn tốt khi cần ghi dữ liệu lớn hoặc cần các phương thức của `BufferedWriter` như `newLine()`.

## **6. Buffering (Bộ nhớ đệm)**

  * **Tại sao cần Buffering?** Mỗi thao tác đọc/ghi trực tiếp với ổ đĩa là tương đối chậm. Buffering giảm số lượng thao tác đọc/ghi vật lý bằng cách đọc/ghi dữ liệu vào một vùng nhớ đệm (buffer) trong RAM trước. Dữ liệu chỉ được đọc/ghi thực sự với ổ đĩa khi bộ đệm đầy hoặc khi được yêu cầu `flush()` hoặc `close()`.
  * **Cách sử dụng:** Bao các `Reader`/`Writer` hoặc `InputStream`/`OutputStream` gốc bằng các lớp `Buffered*` tương ứng (`BufferedReader`, `BufferedWriter`, `BufferedInputStream`, `BufferedOutputStream`).
  * **Kích thước bộ đệm:** Mặc định thường là đủ tốt (ví dụ 8KB). Có thể tùy chỉnh kích thước nếu cần tối ưu hóa cho các trường hợp đặc biệt.
  * **Lưu ý:** Các phương thức tiện ích trong `java.nio.file.Files` (như `newBufferedReader`, `newBufferedWriter`, `readAllLines`, `write`) thường đã sử dụng buffering bên trong, bạn không cần phải bọc chúng thêm lần nữa.

## **7. Xử lý Lỗi (Error Handling)**

  * **`IOException`:** Là lớp ngoại lệ (exception) cơ sở cho hầu hết các lỗi liên quan đến I/O trong Java. Luôn cần bắt (`catch`) `IOException` hoặc khai báo ném (`throws`) nó trong phương thức của bạn khi thực hiện các thao tác I/O.
  * **Các lớp con cụ thể:** `java.io` và `java.nio.file` có nhiều lớp con cụ thể hơn của `IOException` để chỉ rõ loại lỗi (ví dụ: `FileNotFoundException`, `NoSuchFileException`, `AccessDeniedException`, `DirectoryNotEmptyException`). Bắt các ngoại lệ cụ thể này cho phép bạn xử lý từng loại lỗi một cách phù hợp hơn.
  * **`try-with-resources`:** Như đã đề cập, đây là cách tốt nhất để đảm bảo tài nguyên I/O được đóng đúng cách, giảm nguy cơ rò rỉ tài nguyên (resource leaks) và làm mã nguồn sạch sẽ hơn.
  * **Thông báo lỗi rõ ràng:** Khi bắt ngoại lệ, hãy cung cấp thông tin hữu ích (ví dụ: tên file, thao tác đang thực hiện) trong thông báo lỗi hoặc log để dễ dàng chẩn đoán vấn đề.

## **8. Các Phương pháp Hay nhất & Lỗi Phổ biến**

  * **Luôn dùng `try-with-resources`:** Để quản lý tài nguyên (`Reader`, `Writer`, `InputStream`, `OutputStream`, `Stream` từ `Files.lines`).
  * **Luôn chỉ định Bộ mã hóa Ký tự (Character Encoding):** Ưu tiên `StandardCharsets.UTF_8` trừ khi có lý do cụ thể để dùng bộ mã hóa khác. Tránh phụ thuộc vào encoding mặc định của hệ thống.
  * **Ưu tiên API `java.nio.file` (NIO.2):** Sử dụng `Path`, `Paths`, `Files` cho các thao tác file mới vì chúng mạnh mẽ, linh hoạt và nhất quán hơn `java.io.File`.
  * **Sử dụng Buffering:** Dùng `BufferedReader`, `BufferedWriter` (hoặc các phương thức NIO.2 tương đương) để tăng hiệu năng đọc/ghi file text.
  * **Xử lý `IOException` một cách hợp lý:** Cung cấp thông báo lỗi rõ ràng, hoặc thực hiện các hành động khắc phục nếu có thể.
  * **Kiểm tra sự tồn tại và quyền truy cập:** Trước khi thực hiện thao tác đọc/ghi, có thể cần kiểm tra file/thư mục có tồn tại không (`Files.exists()`) và có đủ quyền không (`Files.isReadable()`, `Files.isWritable()`).
  * **Đóng tài nguyên đúng thứ tự (nếu không dùng try-with-resources):** Nếu bạn không dùng `try-with-resources`, hãy đảm bảo đóng các luồng trong khối `finally` và theo thứ tự ngược lại với lúc mở (ví dụ: đóng `BufferedWriter` trước, rồi mới đóng `FileWriter`/`OutputStreamWriter`). *Tuy nhiên, hãy cố gắng dùng `try-with-resources`.*
  * **Cẩn thận với đường dẫn tương đối:** Đường dẫn tương đối (`"data/file.txt"`) được giải quyết dựa trên thư mục làm việc hiện tại (working directory) của ứng dụng Java, có thể thay đổi tùy thuộc vào cách ứng dụng được khởi chạy. Sử dụng đường dẫn tuyệt đối hoặc các cơ chế khác (như đọc resource từ classpath) nếu cần sự ổn định.
  * **Không đọc file lớn hoàn toàn vào bộ nhớ:** Tránh dùng `Files.readAllLines()` cho các file có kích thước rất lớn. Sử dụng `BufferedReader` hoặc `Files.lines()` để xử lý từng dòng.
  * **Nhầm lẫn giữa Luồng Byte và Luồng Ký tự:** Sử dụng `InputStream`/`OutputStream` để đọc/ghi file text mà không xử lý encoding đúng cách có thể dẫn đến lỗi dữ liệu.

## **9. Kết luận**

Thao tác với file text là một phần không thể thiếu trong lập trình Java. Việc nắm vững các khái niệm về Stream, sự khác biệt giữa luồng byte và luồng ký tự, tầm quan trọng của bộ mã hóa ký tự, và cách sử dụng hiệu quả cả API `java.io` truyền thống và đặc biệt là API `java.nio.file` hiện đại là rất quan trọng.

Bằng cách áp dụng các phương pháp hay nhất như sử dụng `try-with-resources`, chỉ định encoding rõ ràng, và tận dụng buffering, bạn có thể viết mã I/O an toàn, hiệu quả và dễ bảo trì hơn. Hiểu rõ các lựa chọn API khác nhau (`FileReader` vs `Files.newBufferedReader` vs `Files.lines`) cho phép bạn chọn công cụ phù hợp nhất cho từng tác vụ cụ thể.