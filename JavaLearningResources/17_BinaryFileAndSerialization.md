# THAO TÁC FILE NHỊ PHÂN VÀ SERIALIZATION 

## **1. Giới thiệu**

Tài liệu này cung cấp một cái nhìn sâu sắc về cách thức làm việc với file nhị phân và Serialization trong Java. Việc hiểu rõ các khái niệm này rất quan trọng đối với các lập trình viên Java, vì nó cho phép lưu trữ và truy xuất dữ liệu một cách hiệu quả, cũng như truyền tải các đối tượng (object) giữa các môi trường khác nhau.

## **2. Thao tác với File nhị phân**

### **2.1. Khái niệm về Dòng Byte (Byte Stream)**

Trong Java, dữ liệu nhị phân được xử lý thông qua **dòng byte** (byte stream)[cite: 3, 4].  Dòng byte cho phép đọc và ghi dữ liệu ở dạng thô, tức là các đơn vị 8-bit (byte)[cite: 4, 5]. Điều này trái ngược với các dòng ký tự (character stream), được thiết kế để xử lý dữ liệu văn bản.

Ví dụ, khi số nguyên `5` được lưu vào file thông qua dòng byte, nó sẽ được biểu diễn thành chuỗi bit `101` trong hệ nhị phân[cite: 5]. Các file được tạo bằng dòng byte được gọi là **file nhị phân**[cite: 5]. Để đọc được nội dung của file nhị phân, chúng ta cần các chương trình có khả năng "giải mã" dữ liệu nhị phân thành định dạng mà con người có thể hiểu được[cite: 6].

### **2.2.  `InputStream` và `OutputStream`**

Các lớp để xử lý dữ liệu nhị phân trong Java đều bắt nguồn từ hai lớp trừu tượng `InputStream` và `OutputStream`[cite: 7].

  * `InputStream`:  Cung cấp các phương thức để đọc dữ liệu thô dưới dạng byte từ một nguồn nào đó (ví dụ: file, mạng)[cite: 7, 8].
  * `OutputStream`: Cung cấp các phương thức để ghi dữ liệu thô dưới dạng byte đến một đích nào đó (ví dụ: file, mạng)[cite: 7, 8].

Hai lớp này chỉ định nghĩa các phương thức cơ bản nhất để đọc và ghi byte. Để làm việc với các kiểu dữ liệu phức tạp hơn, chúng ta sử dụng các lớp con của `InputStream` và `OutputStream`[cite: 9].

### **2.3. Các lớp quan trọng để thao tác File nhị phân**

Java cung cấp một số lớp mạnh mẽ để làm việc với file nhị phân:

  * **`FileInputStream` / `FileOutputStream`**:  Đây là các dòng kết nối trực tiếp đến file nhị phân trên ổ cứng, cho phép đọc/ghi dữ liệu tuần tự[cite: 11].  `FileInputStream` được dùng để đọc dữ liệu từ file, còn `FileOutputStream` dùng để ghi dữ liệu vào file[cite: 11, 14].
  * **`ObjectInputStream` / `ObjectOutputStream`**:  Đây là các dòng đối tượng, thường được "bọc" bên ngoài một `InputStream` hoặc `OutputStream` khác (ví dụ: `FileInputStream`, `FileOutputStream`). Chúng cho phép đọc/ghi toàn bộ đối tượng Java[cite: 12].  `ObjectInputStream` dùng để đọc các đối tượng từ một stream, và `ObjectOutputStream` dùng để ghi các đối tượng xuống stream[cite: 12].

### **2.4.  `FileInputStream` và `FileOutputStream` chi tiết**

#### **2.4.1. Cấu trúc lớp**

Dưới đây là cấu trúc của lớp `FileInputStream` và `FileOutputStream`:

```
java.io.InputStream
    java.io.FileInputStream

        + FileInputStream(file: File)
        + FileInputStream(filename: String)

java.io.OutputStream
    java.io.FileOutputStream

        + FileOutputStream(file: File)
        + FileOutputStream(filename: String)
        + FileOutputStream(file: File, append: boolean)
        + FileOutputStream(filename: String, append: boolean)
```

#### **2.4.2. Xử lý ngoại lệ (Exception Handling)**

Các phương thức đọc/ghi trong gói `java.io` có thể phát sinh ngoại lệ `java.io.IOException`[cite: 15]. Do đó, bạn cần có cơ chế để bắt và xử lý ngoại lệ này (ví dụ: dùng `try-catch` hoặc `throws`)[cite: 16, 17, 18].

```java
public static void main(String[] args) throws IOException {
    // Thực hiện các thao tác I/O
}

// Hoặc

public static void main(String[] args) {
    try {
        // Thực hiện các thao tác I/O
    } catch (IOException ex) {
        ex.printStackTrace(); // In ra thông tin lỗi
    }
}
```

### **2.5. Ví dụ sử dụng `FileOutputStream` và `FileInputStream`**

#### **2.5.1. Ghi dữ liệu với `FileOutputStream` và `DataOutputStream`**

Ví dụ sau ghi một mảng các số nguyên vào file "output.dat":

```java
import java.io.*;

public class TestDataOutputStream {
    public static void main(String[] args) {
        int[] a = {2, 3, 5, 7, 11};
        try (FileOutputStream fout = new FileOutputStream("output.dat");
             DataOutputStream dout = new DataOutputStream(fout)) { // Bọc FileOutputStream trong DataOutputStream
            for (int i = 0; i < a.length; i++) {
                dout.writeInt(a[i]); // Ghi từng số nguyên vào stream
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

**Giải thích:**

1.  Chúng ta tạo một `FileOutputStream` để mở file "output.dat" ở chế độ ghi.
2.  Chúng ta "bọc" `FileOutputStream` trong một `DataOutputStream`. `DataOutputStream` cung cấp các phương thức tiện lợi để ghi các kiểu dữ liệu nguyên thủy (int, double, boolean, v.v.) vào stream[cite: 20, 21].
3.  Vòng lặp `for` duyệt qua mảng `a` và ghi từng phần tử vào file bằng phương thức `writeInt()`.
4.  Khối `try-with-resources` tự động đóng các stream khi kết thúc, đảm bảo giải phóng tài nguyên.

#### **2.5.2. Đọc dữ liệu với `FileInputStream` và `DataInputStream`**

Ví dụ sau đọc các số nguyên từ file "output.dat" đã tạo ở trên:

```java
import java.io.*;

public class TestDataInputStream {
    public static void main(String[] args) {
        try (FileInputStream fin = new FileInputStream("output.dat");
             DataInputStream din = new DataInputStream(fin)) { // Bọc FileInputStream trong DataInputStream
            while (true) {
                System.out.println(din.readInt()); // Đọc và in số nguyên từ stream
            }
        } catch (EOFException e) {
            // Bắt EOFException để kết thúc vòng lặp khi hết dữ liệu
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

**Giải thích:**

1.  Chúng ta tạo một `FileInputStream` để mở file "output.dat" ở chế độ đọc.
2.  Chúng ta "bọc" `FileInputStream` trong một `DataInputStream`. `DataInputStream` cung cấp các phương thức tương ứng để đọc các kiểu dữ liệu nguyên thủy từ stream[cite: 22, 23].
3.  Vòng lặp `while(true)` đọc liên tục các số nguyên từ file bằng phương thức `readInt()`.
4.  `EOFException` (End of File Exception) được ném ra khi đọc đến cuối file. Chúng ta bắt ngoại lệ này để thoát khỏi vòng lặp `while`.

### **2.6.  Các trường hợp sử dụng File nhị phân**

File nhị phân thường được sử dụng trong các tình huống sau:

  * **Lưu trữ dữ liệu có cấu trúc phức tạp:** Khi bạn cần lưu trữ các cấu trúc dữ liệu phức tạp (ví dụ: đồ thị, cây) hoặc dữ liệu từ các ứng dụng chuyên dụng (ví dụ: hình ảnh, âm thanh, video), file nhị phân hiệu quả hơn file văn bản.
  * **Tối ưu hóa hiệu suất:** Đọc và ghi file nhị phân thường nhanh hơn so với file văn bản, vì không cần thực hiện các chuyển đổi định dạng phức tạp.
  * **Tiết kiệm dung lượng:** File nhị phân thường chiếm ít dung lượng hơn file văn bản, đặc biệt khi lưu trữ các dữ liệu số.

### **2.7.  Mẹo thực chiến và Best Practices**

  * **Sử dụng `BufferedInputStream` / `BufferedOutputStream`:** Để tăng hiệu suất, hãy "bọc" các `FileInputStream` / `FileOutputStream` trong `BufferedInputStream` / `BufferedOutputStream`. Các lớp Buffered hỗ trợ buffering dữ liệu, giảm số lần truy cập đĩa cứng.
  * **Xử lý ngoại lệ cẩn thận:** Luôn nhớ xử lý các ngoại lệ `IOException` khi làm việc với file.
  * **Đóng stream đúng cách:** Đảm bảo đóng tất cả các stream sau khi sử dụng để giải phóng tài nguyên hệ thống.  Sử dụng `try-with-resources` để tự động đóng stream.
  * **Chọn kiểu dữ liệu phù hợp:** Sử dụng các phương thức đọc/ghi tương ứng với kiểu dữ liệu bạn đang xử lý (ví dụ: `writeInt()`, `readDouble()`, `writeUTF()`).

## **3. Serialization**

### **3.1. Khái niệm Serialization**

**Serialization** là quá trình chuyển đổi một đối tượng Java thành một chuỗi các byte[cite: 25]. Chuỗi byte này chứa thông tin về kiểu dữ liệu của đối tượng, trạng thái của nó (giá trị của các thuộc tính), và cấu trúc của nó[cite: 26, 27]. Quá trình ngược lại, tái tạo lại đối tượng từ chuỗi byte, được gọi là **Deserialization**[cite: 28].

### **3.2.  Mục đích của Serialization**

Serialization cho phép:

  * **Lưu trữ đối tượng xuống file hoặc database:** Bạn có thể lưu trạng thái của một đối tượng vào ổ cứng và sau đó khôi phục nó khi cần.
  * **Truyền tải đối tượng qua mạng:** Các byte được tạo ra từ quá trình Serialization có thể được gửi qua mạng đến một máy khác, nơi chúng được Deserialization để tạo lại đối tượng ban đầu[cite: 27].  Điều này rất quan trọng trong các ứng dụng phân tán (distributed applications).
  * **Giao tiếp giữa các JVM:** Serialization cho phép các máy ảo Java (JVM) khác nhau trao đổi các đối tượng với nhau, ngay cả khi chúng chạy trên các máy tính khác nhau[cite: 27].

### **3.3.  `java.io.Serializable` Interface**

Để một đối tượng Java có thể được Serialization, lớp của đối tượng đó phải triển khai (implement) interface `java.io.Serializable`[cite: 30].

```java
public class MyObject implements Serializable {
    // Các thuộc tính và phương thức của lớp
}
```

`Serializable` là một **marker interface**, tức là nó không định nghĩa bất kỳ phương thức nào[cite: 31].  Chức năng duy nhất của nó là "đánh dấu" cho JVM biết rằng các đối tượng của lớp này có thể được Serialization.

### **3.4.  `ObjectOutputStream` và `ObjectInputStream`**

Java cung cấp hai lớp `ObjectOutputStream` và `ObjectInputStream` để thực hiện Serialization và Deserialization[cite: 31, 32, 33].

  * **`ObjectOutputStream`**:  Chứa phương thức `writeObject(Object obj)` để Serialization một đối tượng và ghi nó vào một `OutputStream`[cite: 32, 35].
  * **`ObjectInputStream`**: Chứa phương thức `readObject()` để đọc một đối tượng đã được Serialization từ một `InputStream` và Deserialization nó[cite: 33, 38].  Phương thức `readObject()` trả về một `Object`, do đó bạn cần ép kiểu (cast) nó về kiểu dữ liệu thực tế của đối tượng[cite: 34].

### **3.5.  Ví dụ Serialization và Deserialization**

#### **3.5.1.  Serialization đối tượng**

Ví dụ sau Serialization một chuỗi, một số thực, và một đối tượng `Date` vào file "object.dat":

```java
import java.io.*;
import java.util.Date;

public class TestObjectOutputStream {
    public static void main(String[] args) throws IOException {
        try (ObjectOutputStream output = new ObjectOutputStream(new FileOutputStream("object.dat"))) {
            output.writeUTF("John"); // Ghi chuỗi
            output.writeDouble(85.5); // Ghi số thực
            output.writeObject(new Date()); // Ghi đối tượng Date
        }
    }
}
```

**Lưu ý:** Các lớp `String`, `Double`, và `Date` đã triển khai interface `java.io.Serializable`[cite: 36].

#### **3.5.2. Deserialization đối tượng**

Ví dụ sau Deserialization các đối tượng từ file "object.dat" đã tạo ở trên:

```java
import java.io.*;
import java.util.Date;

public class TestObjectInputStream {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        try (ObjectInputStream input = new ObjectInputStream(new FileInputStream("object.dat"))) {
            String name = input.readUTF(); // Đọc chuỗi
            double score = input.readDouble(); // Đọc số thực
            Date date = (Date) input.readObject(); // Đọc đối tượng Date (cần ép kiểu)
            System.out.println(name + " " + score + " " + date);
        }
    }
}
```

**Chú ý:** Phương thức `readObject()` có thể ném ra ngoại lệ `ClassNotFoundException` nếu không tìm thấy định nghĩa của lớp của đối tượng được Deserialization.  Do đó, bạn cần phải xử lý ngoại lệ này[cite: 39, 40, 41].

### **3.6.  Các trường hợp sử dụng Serialization**

Serialization được sử dụng rộng rãi trong các tình huống sau:

  * **Lưu trữ tạm thời trạng thái ứng dụng:** Serialization có thể được dùng để lưu trạng thái của ứng dụng, ví dụ như khi người dùng tạm dừng công việc và muốn khôi phục lại sau đó.
  * **Remote Method Invocation (RMI):** Serialization là cơ chế cốt lõi trong RMI, cho phép các đối tượng trên các máy khác nhau gọi phương thức của nhau.
  * **Enterprise JavaBeans (EJB):** Serialization được sử dụng trong EJB để truyền tải các đối tượng giữa các thành phần khác nhau của ứng dụng doanh nghiệp.
  * **Caching:** Serialization có thể được dùng để lưu trữ các đối tượng vào cache (bộ nhớ đệm) để tăng tốc độ truy cập.

### **3.7.  Các vấn đề cần cân nhắc khi sử dụng Serialization**

  * **Tính tương thích:** Nếu bạn thay đổi cấu trúc của một lớp (ví dụ: thêm hoặc xóa thuộc tính), các đối tượng đã được Serialization trước đó có thể không còn tương thích với phiên bản mới của lớp.  Java cung cấp cơ chế `serialVersionUID` để quản lý tính tương thích này, nhưng đòi hỏi sự cẩn trọng.
  * **Bảo mật:** Deserialization có thể tạo ra các lỗ hổng bảo mật nếu dữ liệu đầu vào không được kiểm tra cẩn thận.  Việc Deserialization dữ liệu không tin cậy có thể dẫn đến việc thực thi mã độc.
  * **Hiệu suất:** Serialization và Deserialization có thể tốn kém về hiệu suất, đặc biệt đối với các đối tượng lớn.

### **3.8.  Best Practices cho Serialization**

  * **Chỉ Serialization các thuộc tính cần thiết:** Đánh dấu các thuộc tính không cần thiết cho quá trình Serialization bằng từ khóa `transient`.
  * **Quản lý `serialVersionUID`:** Nếu bạn dự định phát triển ứng dụng trong thời gian dài, hãy khai báo rõ ràng `serialVersionUID` để đảm bảo tính tương thích.
  * **Cẩn trọng với Deserialization:** Chỉ Deserialization dữ liệu từ các nguồn tin cậy.  Cân nhắc sử dụng các cơ chế bảo mật bổ sung (ví dụ: chữ ký số) để xác thực dữ liệu.
  * **Đánh giá hiệu suất:** Nếu hiệu suất là một mối quan tâm lớn, hãy cân nhắc các giải pháp thay thế cho Serialization của Java (ví dụ: các thư viện Serialization khác như Protocol Buffers, Apache Avro, JSON).

## **4. So sánh File nhị phân và Serialization**

| Tính năng         | File nhị phân                                       | Serialization                                        |
| :---------------- | :-------------------------------------------------- | :--------------------------------------------------- |
| Mục đích sử dụng  | Lưu trữ dữ liệu thô, tối ưu hiệu suất và dung lượng | Lưu trữ và truyền tải đối tượng Java                 |
| Đơn vị xử lý      | Byte                                                | Đối tượng                                            |
| Khả năng đọc hiểu | Khó đọc trực tiếp (cần chương trình chuyên dụng)    | Khó đọc trực tiếp (dạng byte)                        |
| Tính linh hoạt    | Phù hợp với dữ liệu có cấu trúc đơn giản hoặc thô   | Phù hợp với dữ liệu có cấu trúc phức tạp (đối tượng) |
| Ví dụ             | Lưu trữ ảnh, âm thanh,                              |