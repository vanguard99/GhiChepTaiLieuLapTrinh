# Creational Design Patterns (Mẫu Thiết kế Khởi tạo)

## 1\. Giới thiệu

Trong kỹ thuật phần mềm, **Design Patterns (Mẫu Thiết kế)** là các giải pháp tổng quát, đã được kiểm chứng và có thể tái sử dụng cho những vấn đề thường gặp trong quá trình thiết kế kiến trúc phần mềm. Chúng không phải là một đoạn mã cụ thể hay một thuật toán chi tiết, mà là những khuôn mẫu hoặc mô tả cách giải quyết một loại vấn đề thiết kế nhất định trong nhiều tình huống khác nhau. Việc sử dụng Design Patterns giúp đẩy nhanh quá trình phát triển, nâng cao chất lượng mã nguồn, dễ bảo trì và mở rộng, đồng thời tạo điều kiện thuận lợi cho việc giao tiếp và hợp tác giữa các lập trình viên.

Các Design Patterns thường được phân thành ba nhóm chính:

  * **Creational Design Patterns (Mẫu Thiết kế Khởi tạo)**: Tập trung vào các cơ chế khởi tạo đối tượng, giúp tạo ra đối tượng một cách linh hoạt, phù hợp với từng tình huống cụ thể và che giấu sự phức tạp của quá trình khởi tạo.
  * **Structural Design Patterns (Mẫu Thiết kế Cấu trúc)**: Quan tâm đến cách tổ chức các lớp và đối tượng để hình thành nên các cấu trúc lớn hơn, phức tạp hơn nhưng vẫn đảm bảo tính linh hoạt và hiệu quả.
  * **Behavioral Design Patterns (Mẫu Thiết kế Hành vi)**: Giải quyết các vấn đề liên quan đến thuật toán và sự phân công trách nhiệm giữa các đối tượng, cũng như các mẫu giao tiếp giữa chúng.

Bài viết này sẽ tập trung phân tích sâu về nhóm **Creational Design Patterns**, cung cấp kiến thức nền tảng, ví dụ mã nguồn thực tế bằng Java, các trường hợp ứng dụng, lợi ích, hạn chế và các phương pháp thực hành tốt nhất.

## 2\. Creational Design Patterns là gì?

**Creational Design Patterns** là nhóm các mẫu thiết kế giải quyết các vấn đề liên quan đến quá trình khởi tạo đối tượng. Mục tiêu chính của chúng là làm cho hệ thống độc lập với việc các đối tượng của nó được tạo ra, kết hợp và biểu diễn như thế nào. Chúng cung cấp sự linh hoạt cao trong việc quyết định đối tượng nào cần được tạo, ai tạo ra nó, và nó được tạo ra như thế nào.

**Lợi ích chính của việc sử dụng Creational Design Patterns:**

  * **Tăng tính linh hoạt:** Cho phép thay đổi loại đối tượng được tạo ra mà không ảnh hưởng đến client code (mã nguồn sử dụng đối tượng đó).
  * **Che giấu sự phức tạp:** Quá trình khởi tạo đối tượng, đặc biệt là với các đối tượng phức tạp, có thể được đóng gói và che giấu khỏi client.
  * **Kiểm soát quá trình khởi tạo:** Cung cấp khả năng kiểm soát chặt chẽ hơn đối với việc tạo đối tượng, ví dụ như giới hạn số lượng instance hoặc tái sử dụng các đối tượng đã có.
  * **Cải thiện khả năng bảo trì:** Việc tách biệt logic khởi tạo giúp mã nguồn dễ hiểu, dễ quản lý và bảo trì hơn.

Một số Creational Design Patterns thông dụng bao gồm:

  * Singleton Pattern
  * Factory Method Pattern
  * Abstract Factory Pattern
  * Builder Pattern
  * Prototype Pattern
  * Object Pool Pattern

Trong phạm vi tài liệu này, chúng ta sẽ đi sâu vào ba mẫu phổ biến: **Singleton**, **Factory Method**, và **Object Pool**.

## 3\. Singleton Pattern

### 3.1. Khái niệm cốt lõi

**Singleton Pattern** là một mẫu thiết kế thuộc nhóm Creational, đảm bảo rằng một lớp chỉ có duy nhất một thể hiện (instance) và cung cấp một điểm truy cập toàn cục (global access point) đến thể hiện đó. Điều này hữu ích khi cần chính xác một đối tượng để điều phối các hành động trong toàn bộ hệ thống, ví dụ như quản lý kết nối cơ sở dữ liệu, quản lý cấu hình, hoặc các dịch vụ ghi log.

**Đặc điểm chính của một lớp Singleton:**

  * **Constructor riêng tư (private constructor):** Ngăn chặn việc khởi tạo đối tượng từ bên ngoài lớp thông qua toán tử `new`.
  * **Biến static private của lớp:** Lưu trữ thể hiện duy nhất của lớp.
  * **Phương thức static public (thường đặt tên là `getInstance()`):** Cung cấp điểm truy cập toàn cục đến thể hiện duy nhất. Phương thức này sẽ chịu trách nhiệm tạo thể hiện nếu nó chưa tồn tại và trả về thể hiện đó.

### 3.2. Cơ chế hoạt động bên trong

Khi phương thức `getInstance()` được gọi lần đầu tiên, nó sẽ kiểm tra xem thể hiện đã được tạo hay chưa. Nếu chưa, nó sẽ tạo một thể hiện mới và lưu vào biến static. Trong các lần gọi tiếp theo, phương thức này sẽ trả về trực tiếp thể hiện đã được tạo đó.

Có nhiều cách để triển khai Singleton, bao gồm:

  * **Eager Initialization (Khởi tạo sớm):** Thể hiện được tạo ngay khi lớp được nạp vào bộ nhớ. Cách này đơn giản nhưng có thể lãng phí tài nguyên nếu thể hiện không bao giờ được sử dụng.
  * **Static Block Initialization (Khởi tạo trong khối static):** Tương tự Eager Initialization nhưng cho phép xử lý ngoại lệ trong quá trình khởi tạo.
  * **Lazy Initialization (Khởi tạo trễ):** Thể hiện chỉ được tạo khi phương thức `getInstance()` được gọi lần đầu tiên. Cách này tiết kiệm tài nguyên nhưng cần cẩn trọng trong môi trường đa luồng.
  * **Thread-Safe Singleton (Singleton an toàn trong đa luồng):** Đảm bảo rằng chỉ một thể hiện được tạo ra ngay cả khi nhiều luồng cùng gọi `getInstance()` đồng thời. Điều này có thể đạt được bằng cách sử dụng từ khóa `synchronized` cho phương thức `getInstance()` hoặc sử dụng kỹ thuật "double-checked locking".
  * **Bill Pugh Singleton Implementation (Sử dụng lớp static bên trong):** Một cách triển khai Lazy Initialization hiệu quả và an toàn trong đa luồng mà không cần `synchronized` rõ ràng.
  * **Enum Singleton:** Một cách triển khai đơn giản và an toàn, được khuyến nghị bởi Joshua Bloch. Nó cung cấp bảo vệ ngầm chống lại việc phá vỡ Singleton thông qua reflection và serialization.

### 3.3. Ví dụ mã nguồn (Java)

#### 3.3.1. Lazy Initialization (An toàn trong đa luồng với Double-Checked Locking)

```java
public class DatabaseConnector {
    // Biến static volatile để đảm bảo tính nhất quán bộ nhớ giữa các luồng
    private static volatile DatabaseConnector instance;
    private String connectionInfo;

    // Private constructor để ngăn chặn việc tạo instance từ bên ngoài
    private DatabaseConnector() {
        // Giả lập quá trình khởi tạo kết nối tốn kém
        try {
            Thread.sleep(1000); // Giả lập độ trễ
            this.connectionInfo = "Connected to database at " + System.currentTimeMillis();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    // Phương thức public static để lấy instance
    public static DatabaseConnector getInstance() {
        // Kiểm tra lần đầu (không cần khóa) để tối ưu hiệu năng
        if (instance == null) {
            // Đồng bộ hóa chỉ khi instance chưa được tạo
            synchronized (DatabaseConnector.class) {
                // Kiểm tra lại lần nữa (trong khối đồng bộ) để tránh tạo nhiều instance
                if (instance == null) {
                    instance = new DatabaseConnector();
                }
            }
        }
        return instance;
    }

    public String getConnectionInfo() {
        return connectionInfo;
    }

    public void connect() {
        System.out.println("Connecting with: " + connectionInfo);
    }

    // Các phương thức khác liên quan đến kết nối cơ sở dữ liệu
    // ...
}

// Client code
public class SingletonDemo {
    public static void main(String[] args) {
        // Lấy instance của DatabaseConnector
        DatabaseConnector connector1 = DatabaseConnector.getInstance();
        connector1.connect();

        // Lấy lại instance (sẽ trả về cùng một đối tượng)
        DatabaseConnector connector2 = DatabaseConnector.getInstance();
        connector2.connect();

        // Kiểm tra xem hai biến có tham chiếu đến cùng một đối tượng không
        if (connector1 == connector2) {
            System.out.println("connector1 and connector2 are the same instance.");
            System.out.println("Connector 1 info: " + connector1.getConnectionInfo());
            System.out.println("Connector 2 info: " + connector2.getConnectionInfo());
        } else {
            System.out.println("connector1 and connector2 are different instances.");
        }
    }
}
```

**Giải thích mã nguồn:**

  * `private static volatile DatabaseConnector instance;`: Từ khóa `volatile` đảm bảo rằng các thay đổi đối với biến `instance` được hiển thị ngay lập tức cho tất cả các luồng.
  * `private DatabaseConnector()`: Constructor được đặt là `private` để không lớp nào khác có thể tạo đối tượng `DatabaseConnector` bằng toán tử `new`.
  * `public static DatabaseConnector getInstance()`:
      * Kiểm tra `if (instance == null)` lần đầu tiên bên ngoài khối `synchronized` để tránh chi phí không cần thiết của việc đồng bộ hóa nếu `instance` đã được khởi tạo.
      * Khối `synchronized (DatabaseConnector.class)` đảm bảo rằng chỉ một luồng có thể vào khối này tại một thời điểm, ngăn chặn việc tạo nhiều `instance` trong môi trường đa luồng.
      * Kiểm tra `if (instance == null)` lần thứ hai bên trong khối `synchronized` là một phần của kỹ thuật "double-checked locking". Điều này cần thiết vì một luồng khác có thể đã khởi tạo `instance` trong khoảng thời gian giữa lần kiểm tra đầu tiên và khi luồng hiện tại giành được khóa.

#### 3.3.2. Enum Singleton (Cách tiếp cận đơn giản và an toàn)

```java
public enum EnumDatabaseConnector {
    INSTANCE("Initial DB Connection");

    private String connectionInfo;

    // Constructor của Enum là private ngầm định
    EnumDatabaseConnector(String initialInfo) {
        // Giả lập quá trình khởi tạo
        this.connectionInfo = initialInfo + " at " + System.currentTimeMillis();
        System.out.println("EnumDatabaseConnector initialized with: " + this.connectionInfo);
    }

    public String getConnectionInfo() {
        return connectionInfo;
    }

    public void setConnectionInfo(String connectionInfo) {
        this.connectionInfo = connectionInfo;
    }

    public void connect() {
        System.out.println("Connecting via Enum with: " + connectionInfo);
    }

    // Các phương thức khác
}

// Client code
public class EnumSingletonDemo {
    public static void main(String[] args) {
        EnumDatabaseConnector connector1 = EnumDatabaseConnector.INSTANCE;
        connector1.connect();
        connector1.setConnectionInfo("Updated DB Connection");

        EnumDatabaseConnector connector2 = EnumDatabaseConnector.INSTANCE;
        connector2.connect(); // Sẽ hiển thị "Updated DB Connection"

        if (connector1 == connector2) {
            System.out.println("Enum connector1 and connector2 are the same instance.");
            System.out.println("Connector 1 info: " + connector1.getConnectionInfo());
            System.out.println("Connector 2 info: " + connector2.getConnectionInfo());
        }
    }
}
```

**Giải thích mã nguồn:**

  * `EnumDatabaseConnector.INSTANCE`: `INSTANCE` là hằng số duy nhất của enum, đại diện cho thể hiện Singleton. JVM đảm bảo rằng constructor của enum chỉ được gọi một lần.
  * Cách này tự động xử lý các vấn đề về serialization và bảo vệ chống lại việc tạo nhiều thể hiện thông qua reflection một cách hiệu quả.

### 3.4. Trường hợp sử dụng thực tế

  * **Quản lý cấu hình (Configuration Manager):** Một đối tượng duy nhất để đọc và cung cấp các thông số cấu hình cho toàn bộ ứng dụng.
  * **Dịch vụ ghi log (Logging Service):** Một logger duy nhất để ghi lại các sự kiện và lỗi từ các phần khác nhau của ứng dụng.
  * **Quản lý kết nối cơ sở dữ liệu (Database Connection Pool):** Mặc dù bản thân connection pool có thể chứa nhiều kết nối, nhưng đối tượng quản lý pool đó thường là một Singleton.
  * **Đối tượng Runtime trong Java (`Runtime.getRuntime()`):** Java cung cấp đối tượng `Runtime` như một Singleton để tương tác với môi trường thực thi.
  * **Các lớp Factory hoặc Builder có trạng thái:** Nếu một factory cần duy trì trạng thái hoặc tài nguyên chung, nó có thể được triển khai như một Singleton.
  * **Bộ đệm (Cache):** Một bộ đệm dùng chung cho toàn ứng dụng.

### 3.5. Ưu điểm và Nhược điểm

**Ưu điểm:**

  * **Đảm bảo một thể hiện duy nhất:** Kiểm soát chặt chẽ việc chỉ có một đối tượng được tạo ra.
  * **Điểm truy cập toàn cục:** Dễ dàng truy cập thể hiện duy nhất từ bất kỳ đâu trong mã nguồn.
  * **Khởi tạo trễ (Lazy Initialization):** Thể hiện chỉ được tạo khi cần thiết, giúp tiết kiệm tài nguyên (nếu được triển khai theo cách này).
  * **Quản lý tài nguyên tập trung:** Hữu ích cho việc quản lý các tài nguyên chia sẻ.

**Nhược điểm:**

  * **Vi phạm Nguyên tắc Trách nhiệm Đơn (Single Responsibility Principle - SRP):** Lớp Singleton tự chịu trách nhiệm cho cả logic nghiệp vụ của nó và việc kiểm soát quá trình khởi tạo của chính nó.
  * **Khó khăn trong việc kiểm thử đơn vị (Unit Testing):** Vì Singleton mang trạng thái toàn cục và khó thay thế bằng mock object, việc viết unit test cho các lớp phụ thuộc vào Singleton có thể trở nên phức tạp.
  * **耦合性高 (High Coupling):** Các lớp sử dụng Singleton trở nên phụ thuộc chặt chẽ vào lớp Singleton đó.
  * **Vấn đề trong môi trường đa luồng:** Cần triển khai cẩn thận để đảm bảo an toàn luồng, nếu không có thể dẫn đến việc tạo nhiều thể hiện hoặc các vấn đề về đồng bộ hóa.
  * **Có thể che giấu thiết kế không tốt:** Việc lạm dụng Singleton có thể dẫn đến việc các thành phần trong chương trình biết quá nhiều về nhau, làm giảm tính module hóa.
  * **Phá vỡ Singleton:** Có thể bị phá vỡ thông qua Reflection API hoặc quá trình Deserialization nếu không được xử lý đúng cách (trừ khi sử dụng Enum Singleton).

### 3.6. Mẹo thực chiến và Phương pháp hay nhất

  * **Cân nhắc kỹ lưỡng trước khi sử dụng:** Chỉ sử dụng Singleton khi thực sự cần thiết và chắc chắn rằng chỉ cần một thể hiện duy nhất của lớp đó trong toàn bộ vòng đời ứng dụng.
  * **Ưu tiên Dependency Injection:** Thay vì để các lớp client tự gọi `Singleton.getInstance()`, hãy cân nhắc việc inject (tiêm) thể hiện Singleton vào các lớp phụ thuộc. Điều này giúp giảm coupling và cải thiện khảibility test.
  * **Sử dụng Enum Singleton nếu có thể:** Đây là cách triển khai đơn giản, an toàn và hiệu quả nhất trong Java, đặc biệt là để chống lại Reflection và Deserialization.
  * **Cẩn thận với Lazy Initialization trong môi trường đa luồng:** Nếu không sử dụng Enum, hãy đảm bảo triển khai an toàn luồng bằng double-checked locking với `volatile` hoặc Bill Pugh Singleton.
  * **Tránh lưu trữ trạng thái toàn cục có thể thay đổi nhiều:** Nếu Singleton giữ quá nhiều trạng thái có thể thay đổi, nó có thể trở thành một "god object" và gây khó khăn cho việc quản lý và gỡ lỗi.
  * **Tài liệu hóa rõ ràng:** Ghi chú lý do tại sao một lớp được thiết kế là Singleton và cách sử dụng nó.

### 3.7. Lỗi phổ biến và Cách khắc phục

  * **Không an toàn trong đa luồng:**
      * **Vấn đề:** Trong môi trường đa luồng, nếu `getInstance()` không được đồng bộ hóa đúng cách, nhiều luồng có thể cùng lúc vượt qua kiểm tra `instance == null` và tạo ra nhiều thể hiện.
      * **Khắc phục:** Sử dụng `synchronized` cho phương thức `getInstance()`, double-checked locking với `volatile`, Bill Pugh Singleton, hoặc Enum Singleton.
  * **Phá vỡ bởi Reflection:**
      * **Vấn đề:** Reflection API có thể được sử dụng để gọi constructor `private` và tạo thêm thể hiện.
      * **Khắc phục:** Trong constructor, kiểm tra xem `instance` đã tồn tại chưa, nếu có thì ném ra `IllegalStateException`. Enum Singleton miễn nhiễm với vấn đề này.
  * **Phá vỡ bởi Deserialization:**
      * **Vấn đề:** Khi một Singleton implement `java.io.Serializable` và được deserialize, một thể hiện mới có thể được tạo ra.
      * **Khắc phục:** Implement phương thức `readResolve()` và trả về `instance` hiện có. Enum Singleton tự động xử lý việc này.
    <!-- end list -->
    ```java
    // Để bảo vệ Singleton khi Deserialization (nếu không dùng Enum)
    protected Object readResolve() {
        return getInstance();
    }
    ```
  * **Khó khăn trong kế thừa:** Vì constructor là `private`, lớp Singleton không thể dễ dàng được kế thừa. Nếu cần tính đa hình, cân nhắc các mẫu khác.

## 4\. Factory Method Pattern

### 4.1. Khái niệm cốt lõi

**Factory Method Pattern** là một mẫu thiết kế thuộc nhóm Creational, cung cấp một interface (giao diện) để tạo ra các đối tượng trong một lớp cha (superclass), nhưng cho phép các lớp con (subclasses) thay đổi loại đối tượng sẽ được tạo. Nói cách khác, Factory Method ủy thác việc khởi tạo đối tượng cho các lớp con.

Mẫu này định nghĩa một phương thức (gọi là *factory method*) mà các lớp con sẽ override để chỉ định lớp cụ thể (concrete class) của đối tượng sản phẩm (product) cần tạo. Client code sẽ làm việc với interface của sản phẩm mà không cần biết đến các lớp sản phẩm cụ thể.

**Các thành phần chính:**

  * **Product (Sản phẩm):** Định nghĩa interface cho các đối tượng mà factory method sẽ tạo ra.
  * **ConcreteProduct (Sản phẩm cụ thể):** Các lớp triển khai interface `Product`. Đây là các đối tượng thực sự được tạo ra.
  * **Creator (Người tạo):** Khai báo factory method, trả về một đối tượng kiểu `Product`. `Creator` cũng có thể định nghĩa một triển khai mặc định cho factory method. `Creator` thường chứa logic nghiệp vụ liên quan đến `Product`.
  * **ConcreteCreator (Người tạo cụ thể):** Override factory method để trả về một thể hiện của một `ConcreteProduct` cụ thể.

### 4.2. Cơ chế hoạt động bên trong

Client code gọi factory method trên một đối tượng `Creator`. Thay vì `Creator` tự khởi tạo sản phẩm trực tiếp bằng toán tử `new`, nó gọi factory method. Vì factory method này có thể được override bởi các `ConcreteCreator`, nên đối tượng sản phẩm thực sự được tạo ra sẽ phụ thuộc vào `ConcreteCreator` nào đang được sử dụng.

Điều này cho phép client code làm việc với `Creator` và `Product` ở mức trừu tượng, tách biệt khỏi các lớp cụ thể. Khi cần thêm một loại sản phẩm mới, chỉ cần tạo một lớp `ConcreteProduct` mới và một lớp `ConcreteCreator` mới (hoặc sửa đổi một `ConcreteCreator` hiện có) mà không cần thay đổi client code.

### 4.3. Ví dụ mã nguồn (Java)

Giả sử chúng ta cần tạo các loại thông báo khác nhau (Email, SMS, Push Notification).

```java
// 1. Product Interface
interface Notification {
    void send(String message);
}

// 2. ConcreteProducts
class EmailNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending Email: " + message);
    }
}

class SmsNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}

class PushNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("Sending Push Notification: " + message);
    }
}

// 3. Creator (Abstract Class or Interface)
abstract class NotificationFactory {
    // Factory Method
    public abstract Notification createNotification();

    // Một phương thức sử dụng Product
    public void sendNotification(String message) {
        Notification notification = createNotification();
        notification.send(message);
    }
}

// 4. ConcreteCreators
class EmailNotificationFactory extends NotificationFactory {
    @Override
    public Notification createNotification() {
        return new EmailNotification();
    }
}

class SmsNotificationFactory extends NotificationFactory {
    @Override
    public Notification createNotification() {
        return new SmsNotification();
    }
}

class PushNotificationFactory extends NotificationFactory {
    @Override
    public Notification createNotification() {
        return new PushNotification();
    }
}

// Client code
public class FactoryMethodDemo {
    public static void main(String[] args) {
        NotificationFactory emailFactory = new EmailNotificationFactory();
        emailFactory.sendNotification("Hello via Email!"); // Output: Sending Email: Hello via Email!

        NotificationFactory smsFactory = new SmsNotificationFactory();
        smsFactory.sendNotification("Hello via SMS!");   // Output: Sending SMS: Hello via SMS!

        NotificationFactory pushFactory = new PushNotificationFactory();
        pushFactory.sendNotification("Hello via Push Notification!"); // Output: Sending Push Notification: Hello via Push Notification!

        // Thêm một loại thông báo mới mà không cần sửa client code ở trên
        // Giả sử chúng ta tạo SlackNotification và SlackNotificationFactory
    }
}
```

**Giải thích mã nguồn:**

  * `Notification`: Interface chung cho tất cả các loại thông báo.
  * `EmailNotification`, `SmsNotification`, `PushNotification`: Các lớp cụ thể triển khai `Notification`.
  * `NotificationFactory`: Lớp trừu tượng `Creator` khai báo `createNotification()` là factory method. Nó cũng có một phương thức `sendNotification` sử dụng sản phẩm được tạo ra.
  * `EmailNotificationFactory`, `SmsNotificationFactory`, `PushNotificationFactory`: Các lớp `ConcreteCreator` override `createNotification()` để trả về loại thông báo tương ứng.
  * Client code làm việc với `NotificationFactory` và gọi `sendNotification()`. Loại thông báo thực sự được gửi đi phụ thuộc vào loại factory được sử dụng.

**Biến thể: Factory Method tĩnh (Static Factory Method)**

Đôi khi, factory method có thể được làm `static` trong một lớp tiện ích, thay vì yêu cầu một `Creator` riêng.

```java
// Product Interface và ConcreteProducts giữ nguyên

// Static Factory
class NotificationProvider {
    public static Notification getNotification(String type) {
        if ("EMAIL".equalsIgnoreCase(type)) {
            return new EmailNotification();
        } else if ("SMS".equalsIgnoreCase(type)) {
            return new SmsNotification();
        } else if ("PUSH".equalsIgnoreCase(type)) {
            return new PushNotification();
        }
        throw new IllegalArgumentException("Unknown notification type: " + type);
    }
}

// Client code
public class StaticFactoryDemo {
    public static void main(String[] args) {
        Notification emailNotif = NotificationProvider.getNotification("EMAIL");
        emailNotif.send("Static factory email.");

        Notification smsNotif = NotificationProvider.getNotification("SMS");
        smsNotif.send("Static factory SMS.");
    }
}
```

Biến thể này đơn giản hơn nhưng ít linh hoạt hơn so với việc sử dụng các lớp `Creator` riêng, vì nó không dễ dàng cho phép các lớp con tùy chỉnh hành vi tạo đối tượng thông qua kế thừa.

### 4.4. Trường hợp sử dụng thực tế

  * **Khi một lớp không thể biết trước loại đối tượng cụ thể mà nó cần tạo:** Ví dụ, một framework UI cần tạo các loại nút (`Button`, `Checkbox`) khác nhau tùy thuộc vào hệ điều hành hoặc theme hiện tại.
  * **Khi một lớp muốn ủy thác việc tạo đối tượng cho các lớp con của nó:** Các lớp con có thể quyết định loại đối tượng cụ thể nào sẽ được tạo ra.
  * **Khi bạn muốn cung cấp cho người dùng thư viện của bạn một cách để mở rộng các thành phần bên trong nó:** Người dùng có thể tạo ra các `ConcreteProduct` và `ConcreteCreator` của riêng họ.
  * **Trong các hệ thống plugin:** Mỗi plugin có thể cung cấp một factory để tạo ra các đối tượng đặc thù của plugin đó.
  * **Tạo các đối tượng Document trong một ứng dụng văn phòng:** `Application` class có thể có một `createDocument()` method, và các lớp con như `WordProcessorApplication` hay `SpreadsheetApplication` sẽ override nó để tạo `WordDocument` hoặc `SpreadsheetDocument`.
  * **Các API kết nối cơ sở dữ liệu (JDBC):** `DriverManager.getConnection()` hoạt động như một factory method để tạo ra các đối tượng `Connection` cụ thể cho từng loại database.

### 4.5. So sánh với các mẫu khác

  * **Factory Method vs. Abstract Factory:**
      * **Factory Method** sử dụng kế thừa (inheritance) để ủy thác việc tạo đối tượng cho các lớp con. Nó thường tập trung vào việc tạo một loại sản phẩm duy nhất.
      * **Abstract Factory** sử dụng composition (kết hợp đối tượng) để tạo ra các *họ* (families) của các đối tượng liên quan hoặc phụ thuộc nhau mà không cần chỉ định các lớp cụ thể của chúng. Một Abstract Factory thường có nhiều factory methods, mỗi factory method tạo một loại sản phẩm trong họ đó. Abstract Factory là "factory của các factory".
  * **Factory Method vs. Simple Factory (không phải là mẫu GoF chính thức):**
      * Simple Factory thường là một lớp có một phương thức (thường là `static`) nhận một tham số và trả về một thể hiện của một trong nhiều lớp sản phẩm cụ thể.
      * Factory Method cho phép các lớp con quyết định lớp nào sẽ được khởi tạo, trong khi Simple Factory đóng gói logic tạo đối tượng vào một chỗ duy nhất nhưng không sử dụng kế thừa theo cách của Factory Method.
  * **Factory Method vs. Builder:**
      * **Factory Method** tập trung vào việc tạo đối tượng trong một bước duy nhất.
      * **Builder** tập trung vào việc xây dựng một đối tượng phức tạp từng bước một, cho phép các biểu diễn khác nhau của cùng một đối tượng.

### 4.6. Ưu điểm và Nhược điểm

**Ưu điểm:**

  * **Giảm耦合 (Loose Coupling):** Client code chỉ làm việc với interface của `Product` và `Creator`, không phụ thuộc vào các lớp `ConcreteProduct` và `ConcreteCreator`.
  * **Tăng tính linh hoạt và khả năng mở rộng:** Dễ dàng thêm các loại sản phẩm mới và các factory tương ứng mà không cần sửa đổi client code hoặc các factory hiện có. Điều này tuân thủ **Nguyên tắc Mở/Đóng (Open/Closed Principle - OCP)**.
  * **Đóng gói logic tạo đối tượng:** Logic tạo đối tượng được tập trung trong các factory method, giúp client code gọn gàng hơn.
  * **Cho phép các lớp con tùy chỉnh việc tạo đối tượng:** Các lớp con có toàn quyền quyết định loại đối tượng nào sẽ được tạo.
  * **Cải thiện khả năng kiểm thử:** Có thể dễ dàng thay thế các `ConcreteCreator` bằng mock objects trong quá trình kiểm thử.

**Nhược điểm:**

  * **Tăng số lượng lớp:** Việc sử dụng mẫu này có thể dẫn đến việc tạo ra nhiều lớp con (`ConcreteCreator`) nếu có nhiều loại sản phẩm, làm tăng độ phức tạp của cấu trúc lớp.
  * **Client có thể cần biết `ConcreteCreator`:** Đôi khi, client vẫn cần biết nên sử dụng `ConcreteCreator` nào để khởi tạo, trừ khi có một cơ chế khác để chọn factory (ví dụ, dựa trên cấu hình).

### 4.7. Mẹo thực chiến và Phương pháp hay nhất

  * **Đặt tên rõ ràng:** Đặt tên cho factory method một cách gợi ý (ví dụ: `createProduct()`, `makeObject()`).
  * **Creator có thể là lớp trừu tượng hoặc interface:** Nếu `Creator` không có logic nghiệp vụ chung hoặc triển khai mặc định cho factory method, nó có thể là một interface.
  * **Factory method có thể có tham số:** Tham số có thể được sử dụng để quyết định loại `ConcreteProduct` nào sẽ được tạo ra bởi một `ConcreteCreator` cụ thể, hoặc để cấu hình `ConcreteProduct`.
  * **Kết hợp với các mẫu khác:** Factory Method thường được sử dụng cùng với các mẫu khác như Template Method (nếu `Creator` có các bước chung và ủy thác một số bước cho lớp con, bao gồm cả bước tạo đối tượng) hoặc Singleton (nếu `Creator` nên là duy nhất).
  * **Cân nhắc sử dụng Simple Factory cho các trường hợp đơn giản:** Nếu không cần sự linh hoạt của kế thừa hoặc không có nhiều `ConcreteCreator`, một Simple Factory (thường là một lớp với phương thức static) có thể đủ.

### 4.8. Lỗi phổ biến và Cách khắc phục

  * **Trả về `null` từ factory method:**
      * **Vấn đề:** Nếu factory method không thể tạo đối tượng (ví dụ, do tham số không hợp lệ), việc trả về `null` có thể dẫn đến `NullPointerException` ở client code.
      * **Khắc phục:** Ném ra một exception cụ thể (ví dụ: `IllegalArgumentException` hoặc một custom exception) thay vì trả về `null`. Hoặc, sử dụng mẫu Null Object.
  * **Lạm dụng tham số trong factory method:**
      * **Vấn đề:** Nếu factory method có quá nhiều tham số để quyết định loại sản phẩm hoặc cấu hình sản phẩm, nó có thể trở nên phức tạp và khó sử dụng.
      * **Khắc phục:** Cân nhắc sử dụng mẫu Builder nếu quá trình tạo đối tượng phức tạp, hoặc chia nhỏ thành nhiều factory method chuyên biệt hơn.
  * **Quên override factory method trong lớp con:**
      * **Vấn đề:** Nếu `Creator` là lớp trừu tượng và factory method là trừu tượng, trình biên dịch sẽ báo lỗi. Nhưng nếu `Creator` có triển khai mặc định và lớp con không override, sản phẩm mặc định sẽ được tạo ra, có thể không phải là điều mong muốn.
      * **Khắc phục:** Đảm bảo rằng các `ConcreteCreator` luôn override factory method để trả về đúng loại sản phẩm.

## 5\. Object Pool Pattern

### 5.1. Khái niệm cốt lõi

**Object Pool Pattern** là một mẫu thiết kế thuộc nhóm Creational, được sử dụng để quản lý một "bể chứa" (pool) các đối tượng có thể tái sử dụng. Thay vì tạo mới và hủy bỏ đối tượng mỗi khi cần, client code có thể "mượn" (acquire/checkout) một đối tượng từ pool, sử dụng nó, và sau đó "trả lại" (release/checkin) vào pool để các client khác có thể tái sử dụng.

Mẫu này đặc biệt hữu ích cho các đối tượng mà việc khởi tạo và hủy bỏ chúng tốn kém tài nguyên (thời gian CPU, bộ nhớ, kết nối mạng, v.v.), ví dụ như kết nối cơ sở dữ liệu, luồng (threads), hoặc các đối tượng đồ họa lớn.

**Các thành phần chính:**

  * **Reusable (Đối tượng có thể tái sử dụng):** Lớp của các đối tượng được quản lý trong pool.
  * **ObjectPool (Bể chứa đối tượng):** Quản lý vòng đời của các `Reusable` objects. Nó chịu trách nhiệm tạo mới đối tượng (nếu pool trống và chưa đạt giới hạn), cung cấp đối tượng cho client, và nhận lại đối tượng từ client.
  * **Client (Người dùng):** Yêu cầu một đối tượng từ `ObjectPool`, sử dụng nó, và sau đó trả lại cho pool.

### 5.2. Cơ chế hoạt động bên trong

1.  **Khởi tạo Pool:** `ObjectPool` được khởi tạo với một số lượng đối tượng ban đầu hoặc một giới hạn kích thước tối đa.
2.  **Mượn đối tượng (Acquire):**
      * Khi client cần một đối tượng, nó yêu cầu từ `ObjectPool`.
      * Pool kiểm tra xem có đối tượng nào đang "rảnh" (available) không.
      * Nếu có, pool đánh dấu đối tượng đó là "đang sử dụng" (in-use) và trả về cho client.
      * Nếu không có đối tượng rảnh và pool chưa đạt kích thước tối đa, pool có thể tạo một đối tượng mới.
      * Nếu không có đối tượng rảnh và pool đã đạt kích thước tối đa, pool có thể bắt client đợi (block) cho đến khi có đối tượng được trả lại, hoặc ném ra một exception, hoặc trả về `null`.
3.  **Sử dụng đối tượng:** Client sử dụng đối tượng đã mượn.
4.  **Trả lại đối tượng (Release):**
      * Sau khi sử dụng xong, client trả đối tượng lại cho `ObjectPool`.
      * Pool đánh dấu đối tượng đó là "rảnh" và có thể thực hiện việc "dọn dẹp" hoặc "reset" trạng thái của đối tượng để nó sẵn sàng cho lần sử dụng tiếp theo.
5.  **Dọn dẹp Pool (Shutdown):** Khi không cần thiết nữa, `ObjectPool` có thể giải phóng tất cả các đối tượng mà nó quản lý (ví dụ, đóng các kết nối cơ sở dữ liệu).

### 5.3. Ví dụ mã nguồn (Java)

Chúng ta sẽ triển khai một pool đơn giản cho các đối tượng `ExpensiveResource`.

```java
import java.util.HashSet;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Set;

// 1. Reusable Object
class ExpensiveResource {
    private static int counter = 0;
    private int id;

    public ExpensiveResource() {
        this.id = ++counter;
        // Giả lập quá trình khởi tạo tốn kém
        try {
            System.out.println("Creating ExpensiveResource " + id + "...");
            Thread.sleep(500); // Giả lập thời gian khởi tạo
            System.out.println("ExpensiveResource " + id + " created.");
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            System.err.println("Creation interrupted for resource " + id);
        }
    }

    public void use() {
        System.out.println("Using ExpensiveResource " + id);
    }

    public void reset() {
        System.out.println("Resetting ExpensiveResource " + id);
        // Logic để reset trạng thái của resource về mặc định
    }

    public int getId() {
        return id;
    }
}

// 2. ObjectPool
class ResourcePool {
    private Queue<ExpensiveResource> available; // Hàng đợi các resource rảnh
    private Set<ExpensiveResource> inUse;       // Tập hợp các resource đang được sử dụng
    private final int maxSize;
    private int currentSize = 0; // Số lượng resource đã được tạo

    public ResourcePool(int initialSize, int maxSize) {
        if (initialSize > maxSize) {
            throw new IllegalArgumentException("Initial size cannot exceed max size.");
        }
        this.maxSize = maxSize;
        this.available = new LinkedList<>();
        this.inUse = new HashSet<>();

        for (int i = 0; i < initialSize; i++) {
            ExpensiveResource resource = new ExpensiveResource();
            available.offer(resource);
            currentSize++;
        }
    }

    public synchronized ExpensiveResource acquireResource() throws InterruptedException {
        while (available.isEmpty()) {
            if (currentSize < maxSize) {
                // Tạo resource mới nếu chưa đạt giới hạn
                ExpensiveResource newResource = new ExpensiveResource();
                available.offer(newResource);
                currentSize++;
                System.out.println("Pool created new resource. Current total: " + currentSize);
            } else {
                // Đợi nếu pool rỗng và đã đạt giới hạn
                System.out.println("Pool is full and no available resources. Waiting...");
                wait(); // Chờ cho đến khi có resource được trả lại
            }
        }
        ExpensiveResource resource = available.poll();
        inUse.add(resource);
        System.out.println("Resource " + resource.getId() + " acquired. Available: " + available.size() + ", InUse: " + inUse.size());
        return resource;
    }

    public synchronized void releaseResource(ExpensiveResource resource) {
        if (resource == null) return;

        if (inUse.remove(resource)) {
            resource.reset(); // Quan trọng: Reset trạng thái resource
            available.offer(resource);
            System.out.println("Resource " + resource.getId() + " released. Available: " + available.size() + ", InUse: " + inUse.size());
            notifyAll(); // Thông báo cho các luồng đang đợi
        } else {
            System.err.println("Trying to release a resource that was not acquired from this pool or already released.");
        }
    }

    public synchronized void shutdown() {
        System.out.println("Shutting down resource pool...");
        // Logic dọn dẹp, ví dụ đóng kết nối cho từng resource
        for (ExpensiveResource resource : available) {
            // resource.close(); // Giả sử có phương thức close()
            System.out.println("Cleaning up available resource " + resource.getId());
        }
        for (ExpensiveResource resource : inUse) {
            // resource.close();
            System.out.println("Cleaning up in-use resource " + resource.getId() + " (should ideally be released first)");
        }
        available.clear();
        inUse.clear();
        currentSize = 0;
        System.out.println("Resource pool shut down.");
    }

    public synchronized int getAvailableCount() {
        return available.size();
    }

    public synchronized int getInUseCount() {
        return inUse.size();
    }
}

// 3. Client
public class ObjectPoolDemo {
    public static void main(String[] args) {
        ResourcePool pool = new ResourcePool(2, 4); // Pool ban đầu có 2, tối đa 4 resources

        Runnable task = () -> {
            ExpensiveResource resource = null;
            try {
                System.out.println(Thread.currentThread().getName() + " trying to acquire resource...");
                resource = pool.acquireResource();
                if (resource != null) {
                    System.out.println(Thread.currentThread().getName() + " acquired resource " + resource.getId());
                    resource.use();
                    Thread.sleep((long) (Math.random() * 2000)); // Giả lập thời gian sử dụng
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                System.err.println(Thread.currentThread().getName() + " was interrupted.");
            } finally {
                if (resource != null) {
                    System.out.println(Thread.currentThread().getName() + " releasing resource " + resource.getId());
                    pool.releaseResource(resource);
                }
            }
        };

        Thread t1 = new Thread(task, "Thread-1");
        Thread t2 = new Thread(task, "Thread-2");
        Thread t3 = new Thread(task, "Thread-3");
        Thread t4 = new Thread(task, "Thread-4");
        Thread t5 = new Thread(task, "Thread-5"); // Sẽ phải đợi vì pool có thể đầy

        t1.start();
        t2.start();
        t3.start();
        t4.start();
        t5.start();

        try {
            t1.join();
            t2.join();
            t3.join();
            t4.join();
            t5.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        pool.shutdown();
    }
}
```

**Giải thích mã nguồn:**

  * `ExpensiveResource`: Đối tượng mà việc tạo ra nó tốn kém. Có phương thức `reset()` để đưa về trạng thái ban đầu.
  * `ResourcePool`:
      * `available`: Một `Queue` để lưu trữ các resource đang rảnh.
      * `inUse`: Một `Set` để theo dõi các resource đang được client sử dụng.
      * `maxSize`: Giới hạn số lượng resource tối đa mà pool có thể tạo ra.
      * `acquireResource()`: Lấy một resource từ `available`. Nếu `available` rỗng, nó sẽ tạo mới nếu `currentSize < maxSize`, ngược lại nó sẽ `wait()` cho đến khi có resource được trả lại.
      * `releaseResource(ExpensiveResource resource)`: Client trả resource lại pool. Resource được reset và thêm vào `available`. `notifyAll()` được gọi để đánh thức các luồng đang `wait()`.
      * Các phương thức được `synchronized` để đảm bảo an toàn trong môi trường đa luồng.
  * `ObjectPoolDemo` (Client): Tạo nhiều luồng để mô phỏng việc nhiều client cùng yêu cầu và trả lại resource.

### 5.4. Trường hợp sử dụng thực tế

  * **Connection Pooling:** Quản lý các kết nối đến cơ sở dữ liệu (JDBC Connection Pool), kết nối mạng (HTTP Connection Pool). Đây là ứng dụng phổ biến nhất.
  * **Thread Pooling:** Quản lý một nhóm các luồng để thực thi các tác vụ (ví dụ: `java.util.concurrent.ExecutorService`).
  * **Image Pooling:** Trong các ứng dụng đồ họa, quản lý một pool các đối tượng hình ảnh lớn để tránh việc đọc từ đĩa nhiều lần.
  * **Resource-intensive objects:** Bất kỳ đối tượng nào mà việc tạo và hủy thường xuyên gây ra gánh nặng hiệu năng đáng kể.
  * **Giới hạn số lượng đối tượng:** Khi cần giới hạn số lượng instance của một lớp cụ thể đang hoạt động đồng thời.

### 5.5. Ưu điểm và Nhược điểm

**Ưu điểm:**

  * **Cải thiện hiệu năng:** Giảm đáng kể chi phí tạo và hủy đối tượng, đặc biệt với các đối tượng "đắt đỏ".
  * **Quản lý tài nguyên hiệu quả:** Tái sử dụng đối tượng giúp giảm thiểu việc sử dụng tài nguyên hệ thống.
  * **Kiểm soát số lượng đối tượng:** Có thể giới hạn số lượng đối tượng được tạo ra, tránh tình trạng cạn kiệt tài nguyên.
  * **Tăng khả năng đáp ứng của ứng dụng:** Client không phải chờ đợi lâu để một đối tượng mới được tạo ra nếu có sẵn trong pool.

**Nhược điểm:**

  * **Tăng độ phức tạp:** Việc triển khai và quản lý một Object Pool phức tạp hơn so với việc tạo đối tượng trực tiếp.
  * **Quản lý vòng đời đối tượng:** Cần xử lý cẩn thận việc reset trạng thái của đối tượng khi nó được trả lại pool để tránh rò rỉ dữ liệu hoặc trạng thái không mong muốn giữa các lần sử dụng.
  * **Khó khăn trong gỡ lỗi:** Nếu một đối tượng trong pool bị hỏng hoặc không được reset đúng cách, việc tìm ra lỗi có thể khó khăn.
  * **Overhead của pool:** Bản thân pool cũng tiêu tốn một ít bộ nhớ và CPU để quản lý các đối tượng. Cần cân nhắc xem lợi ích có vượt trội so với overhead này không.
  * **Nguy cơ "Pool Exhaustion":** Nếu tất cả đối tượng trong pool đang được sử dụng và không có cơ chế xử lý tốt (ví dụ, client phải đợi quá lâu), ứng dụng có thể bị treo.

### 5.6. Mẹo thực chiến và Phương pháp hay nhất

  * **Reset trạng thái đối tượng cẩn thận:** Đây là bước cực kỳ quan trọng. Đảm bảo rằng mọi trạng thái của đối tượng liên quan đến lần sử dụng trước đó phải được dọn dẹp hoàn toàn khi đối tượng được trả về pool.
  * **Cấu hình kích thước pool hợp lý:**
      * `minSize`/`initialSize`: Số lượng đối tượng tối thiểu được tạo sẵn.
      * `maxSize`: Số lượng đối tượng tối đa mà pool có thể quản lý.
      * Kích thước pool nên được điều chỉnh dựa trên tải của ứng dụng và tài nguyên hệ thống.
  * **Xử lý trường hợp pool cạn kiệt:**
      * **Chờ (Block):** Client đợi cho đến khi có đối tượng khả dụng. Cần có timeout để tránh chờ vô hạn.
      * **Tạo mới (nếu chưa đạt max):** Nếu pool cho phép, tạo thêm đối tượng.
      * **Ném Exception:** Báo lỗi cho client biết rằng không có đối tượng nào khả dụng.
  * **Sử dụng các thư viện có sẵn:** Thay vì tự triển khai, hãy cân nhắc sử dụng các thư viện Object Pooling đã được kiểm chứng như Apache Commons Pool, HikariCP (cho JDBC), hoặc các Thread Pool của Java Concurrency Utilities.
  * **Đảm bảo an toàn luồng:** Object Pool thường được sử dụng trong môi trường đa luồng, vì vậy việc triển khai các phương thức `acquire()` và `release()` phải là thread-safe.
  * **Giám sát pool:** Theo dõi số lượng đối tượng đang sử dụng, số lượng đối tượng rảnh, thời gian chờ đợi để có thể tinh chỉnh cấu hình pool cho phù hợp.
  * **Đối tượng trong pool nên là Singleton (nếu hợp lý):** Bản thân lớp `ObjectPool` thường được triển khai như một Singleton để đảm bảo chỉ có một pool quản lý một loại resource cụ thể.

### 5.7. Lỗi phổ biến và Cách khắc phục

  * **Không reset đối tượng đúng cách:**
      * **Vấn đề:** Dữ liệu từ lần sử dụng trước còn sót lại, gây ra hành vi không mong muốn hoặc rò rỉ thông tin.
      * **Khắc phục:** Luôn gọi một phương thức `reset()` hoặc `clear()` trên đối tượng khi nó được trả lại pool.
  * **Client không trả lại đối tượng (Resource Leak):**
      * **Vấn đề:** Client mượn đối tượng nhưng quên hoặc không thể trả lại (ví dụ, do exception không được xử lý), làm cạn kiệt pool.
      * **Khắc phục:** Sử dụng khối `try-finally` để đảm bảo đối tượng luôn được trả lại. Cân nhắc cơ chế "lease time" (thời gian thuê) và tự động thu hồi đối tượng nếu client giữ quá lâu (phức tạp hơn).
    <!-- end list -->
    ```java
    Object resource = null;
    try {
        resource = pool.acquire();
        // use resource
    } finally {
        if (resource != null) {
            pool.release(resource);
        }
    }
    ```
  * **Deadlock trong pool:**
      * **Vấn đề:** Nếu việc acquire hoặc release đối tượng liên quan đến việc khóa nhiều tài nguyên khác, có thể xảy ra deadlock.
      * **Khắc phục:** Đơn giản hóa logic khóa, đảm bảo thứ tự khóa nhất quán, hoặc sử dụng các cơ chế đồng bộ hóa cấp cao hơn.
  * **Pool được cấu hình không tối ưu:**
      * **Vấn đề:** Pool quá nhỏ gây ra chờ đợi thường xuyên; pool quá lớn lãng phí tài nguyên.
      * **Khắc phục:** Theo dõi và điều chỉnh kích thước pool dựa trên thực tế sử dụng.

## 6\. Kết luận và So sánh chung

Creational Design Patterns cung cấp các giải pháp mạnh mẽ và linh hoạt cho các vấn đề khởi tạo đối tượng trong phát triển phần mềm.

  * **Singleton Pattern** đảm bảo một lớp chỉ có một thể hiện duy nhất và cung cấp một điểm truy cập toàn cục đến nó. Nó hữu ích cho việc quản lý các tài nguyên hoặc dịch vụ cần được chia sẻ và điều phối tập trung trong toàn bộ ứng dụng. Tuy nhiên, cần cẩn trọng với các nhược điểm tiềm ẩn như vi phạm SRP và khó khăn trong kiểm thử.

  * **Factory Method Pattern** định nghĩa một interface để tạo đối tượng nhưng để các lớp con quyết định lớp cụ thể nào sẽ được khởi tạo. Nó thúc đẩy sự linh hoạt bằng cách tách biệt client code khỏi các lớp sản phẩm cụ thể, cho phép dễ dàng mở rộng hệ thống với các loại sản phẩm mới.

  * **Object Pool Pattern** quản lý một tập hợp các đối tượng có thể tái sử dụng, giúp cải thiện hiệu năng và quản lý tài nguyên hiệu quả bằng cách tránh chi phí tạo và hủy đối tượng liên tục. Nó đặc biệt hữu ích cho các đối tượng "đắt đỏ" như kết nối cơ sở dữ liệu hoặc luồng.

**Khi nào chọn mẫu nào?**

  * Cần **đảm bảo chỉ một instance** và truy cập toàn cục? → **Singleton**.
  * Cần **ủy thác việc tạo đối tượng cho các lớp con** và muốn client code làm việc với interface sản phẩm mà không biết lớp cụ thể? → **Factory Method**.
  * Cần **tái sử dụng các đối tượng đắt tiền** để cải thiện hiệu năng và quản lý tài nguyên? → **Object Pool**.

Mỗi mẫu thiết kế đều có những ưu và nhược điểm riêng, và việc lựa chọn mẫu nào phụ thuộc vào bài toán cụ thể, bối cảnh của hệ thống và các yêu cầu về hiệu năng, khả năng bảo trì, và tính linh hoạt. Hiểu rõ cơ chế hoạt động, trường hợp sử dụng, và các cân nhắc của từng mẫu sẽ giúp lập trình viên đưa ra quyết định thiết kế tốt hơn, xây dựng các hệ thống phần mềm mạnh mẽ và dễ bảo trì hơn.