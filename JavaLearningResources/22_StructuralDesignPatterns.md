# Structural Design Patterns (Mẫu Thiết kế Cấu trúc)

## 1\. Giới thiệu

Trong lĩnh vực phát triển phần mềm, việc tổ chức các lớp và đối tượng một cách hiệu quả là yếu tố then chốt để xây dựng nên những hệ thống lớn, phức tạp nhưng vẫn đảm bảo tính linh hoạt, dễ bảo trì và mở rộng. **Structural Design Patterns (Mẫu Thiết kế Cấu trúc)** chính là nhóm các giải pháp đã được kiểm chứng, tập trung vào cách các lớp và đối tượng được kết hợp và cấu thành để hình thành nên các cấu trúc lớn hơn. Chúng giúp đơn giản hóa việc thiết kế các mối quan hệ phức tạp giữa các thực thể, làm cho mã nguồn trở nên dễ hiểu, hiệu quả và có khả năng thích ứng cao hơn.

Không giống như Creational Patterns tập trung vào *quá trình khởi tạo đối tượng*, Structural Patterns quan tâm đến *cách cấu trúc các đối tượng và lớp*. Việc áp dụng các mẫu này giúp đảm bảo rằng nếu một phần của hệ thống thay đổi, toàn bộ hệ thống không cần phải thay đổi theo.

Bài viết này sẽ đi sâu vào các Structural Design Patterns phổ biến, giải thích khái niệm, cơ chế hoạt động, cung cấp ví dụ mã nguồn bằng Java, phân tích ưu nhược điểm, các trường hợp ứng dụng thực tế và những phương pháp thực hành tốt nhất.

## 2\. Structural Design Patterns là gì?

**Structural Design Patterns** là tập hợp các kỹ thuật thiết kế nhằm đơn giản hóa việc xây dựng mối quan hệ giữa các thực thể (lớp, đối tượng) trong một hệ thống phần mềm. Mục tiêu chính của chúng là xác định những cách đơn giản để hiện thực hóa các mối quan hệ giữa các thực thể, đồng thời tăng cường tính linh hoạt và hiệu quả của cấu trúc tổng thể.

**Lợi ích chính của việc sử dụng Structural Design Patterns:**

  * **Đơn giản hóa cấu trúc:** Giúp tổ chức các lớp và đối tượng một cách rõ ràng, dễ hiểu, ngay cả trong các hệ thống phức tạp.
  * **Tăng tính linh hoạt:** Cho phép thay đổi hoặc mở rộng cấu trúc hệ thống mà ít ảnh hưởng đến các phần khác.
  * **Tái sử dụng mã nguồn:** Tạo điều kiện thuận lợi cho việc sử dụng lại các lớp và thành phần hiện có.
  * **Cải thiện khả năng bảo trì:** Mã nguồn có cấu trúc tốt sẽ dễ dàng hơn trong việc sửa lỗi, nâng cấp và bảo trì.
  * **Tối ưu hóa tài nguyên:** Một số mẫu (như Flyweight) giúp giảm thiểu việc sử dụng bộ nhớ.

Các Structural Design Patterns thông dụng bao gồm:

  * Adapter Pattern
  * Facade Pattern
  * Proxy Pattern
  * Decorator Pattern
  * Bridge Pattern
  * Composite Pattern
  * Flyweight Pattern

Chúng ta sẽ tìm hiểu chi tiết về Adapter, Facade và Proxy, sau đó là tổng quan về các mẫu còn lại.

## 3\. Adapter Pattern (Mẫu Bộ Chuyển Đổi)

### 3.1. Khái niệm cốt lõi

**Adapter Pattern** là một mẫu thiết kế cấu trúc cho phép các interface (giao diện) không tương thích của các lớp có thể làm việc cùng nhau. Nó hoạt động như một cầu nối trung gian, chuyển đổi interface của một lớp (Adaptee - lớp cần được thích ứng) thành một interface khác mà client (máy khách) mong đợi (Target - mục tiêu).

Hãy tưởng tượng bạn có một đầu cắm điện kiểu Mỹ (Adaptee) nhưng ổ cắm trên tường lại là kiểu Châu Âu (Target). Một bộ chuyển đổi (Adapter) sẽ giúp bạn kết nối chúng. Trong phần mềm, Adapter Pattern giải quyết vấn đề tương tự: làm cho các lớp vốn không thể làm việc trực tiếp với nhau do sự khác biệt về interface có thể hợp tác một cách trơn tru.

Mẫu này đặc biệt hữu ích khi bạn muốn sử dụng một lớp hiện có nhưng interface của nó không khớp với phần còn lại của hệ thống, hoặc khi bạn muốn tạo một lớp có thể tái sử dụng để hợp tác với các lớp không lường trước được hoặc không liên quan mà có thể có các interface khác nhau.

**Các thành phần chính:**

  * **Target (Mục tiêu):** Interface mà client mong đợi hoặc tương tác.
  * **Adaptee (Đối tượng cần thích ứng):** Lớp hiện có với một interface không tương thích. Đây là lớp mà chúng ta muốn "thích ứng".
  * **Adapter (Bộ chuyển đổi):** Lớp triển khai interface `Target` và bao bọc một đối tượng `Adaptee`. Nó nhận các yêu cầu từ client thông qua interface `Target` và chuyển tiếp chúng tới đối tượng `Adaptee` bằng cách gọi các phương thức tương ứng trên `Adaptee`.
  * **Client (Máy khách):** Lớp tương tác với `Adapter` thông qua interface `Target`.

Có hai cách chính để triển khai Adapter Pattern:

1.  **Object Adapter (Adapter Đối tượng):** Sử dụng kỹ thuật composition (kết hợp đối tượng). Adapter chứa một tham chiếu đến một instance của Adaptee.
2.  **Class Adapter (Adapter Lớp):** Sử dụng kỹ thuật inheritance (kế thừa). Adapter kế thừa từ cả `Target` (thường là một interface) và `Adaptee` (thường là một lớp cụ thể). Cách này yêu cầu đa kế thừa, nên trong Java, `Target` phải là interface và `Adaptee` là lớp, hoặc Adapter kế thừa từ `Adaptee` và implement `Target`.

Object Adapter thường được ưa chuộng hơn vì tính linh hoạt cao hơn (ví dụ, có thể thay đổi Adaptee lúc runtime).

### 3.2. Cơ chế hoạt động bên trong

1.  Client gọi một phương thức trên `Adapter` thông qua interface `Target`.
2.  `Adapter` nhận yêu cầu này.
3.  `Adapter` dịch hoặc chuyển đổi yêu cầu này thành một hoặc nhiều lời gọi phương thức tới đối tượng `Adaptee` mà nó bao bọc (đối với Object Adapter) hoặc kế thừa (đối với Class Adapter).
4.  `Adaptee` thực hiện công việc.
5.  Nếu cần, `Adapter` có thể xử lý kết quả trả về từ `Adaptee` trước khi trả lại cho Client.

### 3.3. Ví dụ mã nguồn (Java) - Object Adapter

Giả sử chúng ta có một `LegacyRectangle` (Adaptee) với phương thức `display(x1, y1, x2, y2)` để vẽ hình chữ nhật bằng tọa độ hai góc đối diện. Tuy nhiên, hệ thống mới của chúng ta sử dụng interface `Shape` (Target) với phương thức `draw(x, y, width, height)`.

```java
// Adaptee - Lớp hiện có với interface không tương thích
class LegacyRectangle {
    public void display(int x1, int y1, int x2, int y2) {
        System.out.println("LegacyRectangle: display with coordinates (" + x1 + "," + y1 + ") to (" + x2 + "," + y2 + ")");
        // Giả sử đây là logic vẽ phức tạp
    }
}

// Target - Interface mà client mong đợi
interface Shape {
    void draw(int x, int y, int width, int height);
    String getDescription();
}

// Adapter - Lớp chuyển đổi LegacyRectangle sang Shape
class RectangleAdapter implements Shape {
    private LegacyRectangle legacyRectangle; // Tham chiếu đến Adaptee

    public RectangleAdapter(LegacyRectangle legacyRectangle) {
        this.legacyRectangle = legacyRectangle;
    }

    @Override
    public void draw(int x, int y, int width, int height) {
        int x1 = x;
        int y1 = y;
        int x2 = x + width;
        int y2 = y + height;
        System.out.println("RectangleAdapter: converting draw(x,y,width,height) to display(x1,y1,x2,y2)");
        legacyRectangle.display(x1, y1, x2, y2);
    }

    @Override
    public String getDescription() {
        return "Adapter for LegacyRectangle";
    }
}

// Client
public class AdapterDemo {
    public static void main(String[] args) {
        // Tạo một đối tượng LegacyRectangle
        LegacyRectangle legacyRect = new LegacyRectangle();

        // Tạo Adapter để bao bọc LegacyRectangle
        Shape adaptedRectangle = new RectangleAdapter(legacyRect);

        System.out.println("Client is using the adapted rectangle:");
        // Client sử dụng interface Shape để vẽ
        adaptedRectangle.draw(10, 20, 50, 30);
        System.out.println(adaptedRectangle.getDescription());

        // Một ví dụ khác với hình tròn (không cần adapter)
        Shape circle = new Circle(); // Giả sử có lớp Circle implement Shape
        System.out.println("\nClient is using a circle:");
        circle.draw(100, 100, 40, 40); // (x, y, radius, radius)
        System.out.println(circle.getDescription());
    }
}

// Lớp Circle mẫu để hoàn thiện ví dụ
class Circle implements Shape {
    @Override
    public void draw(int x, int y, int width, int height) {
        // Giả sử width và height ở đây là đường kính cho đơn giản, hoặc bán kính
        System.out.println("Circle: drawing at (" + x + "," + y + ") with radius " + width/2);
    }
    @Override
    public String getDescription() {
        return "A simple Circle";
    }
}
```

**Giải thích mã nguồn:**

  * `LegacyRectangle`: Là `Adaptee`, có phương thức `display()` với các tham số khác với những gì client mong đợi.
  * `Shape`: Là `Target` interface, định nghĩa phương thức `draw()` mà client sẽ sử dụng.
  * `RectangleAdapter`: Là `Adapter`. Nó implement `Shape` và chứa một instance của `LegacyRectangle`. Trong phương thức `draw()`, nó nhận các tham số theo kiểu `Shape` và chuyển đổi chúng thành các tham số phù hợp để gọi phương thức `display()` của `LegacyRectangle`.
  * `AdapterDemo` (Client): Tạo một `LegacyRectangle` và một `RectangleAdapter` tương ứng. Sau đó, client tương tác với `adaptedRectangle` thông qua interface `Shape` mà không cần biết về `LegacyRectangle`.

### 3.4. Trường hợp sử dụng thực tế

  * **Tích hợp thư viện của bên thứ ba:** Khi một thư viện bên ngoài cung cấp chức năng hữu ích nhưng có interface không tương thích với hệ thống hiện tại.
  * **Làm việc với hệ thống cũ (legacy systems):** Khi cần tích hợp các thành phần từ hệ thống cũ vào hệ thống mới mà không muốn thay đổi mã nguồn của hệ thống cũ.
  * **Tạo wrapper cho các API khác nhau:** Ví dụ, một ứng dụng cần đọc dữ liệu từ nhiều định dạng file (CSV, XML, JSON). Mỗi định dạng có thể có một thư viện đọc riêng. Adapter có thể cung cấp một interface chung để đọc dữ liệu, bất kể định dạng file.
  * **Trong Java:** Các lớp như `java.io.InputStreamReader` (chuyển đổi một `InputStream` thành `Reader`) và `java.io.OutputStreamWriter` (chuyển đổi `OutputStream` thành `Writer`) là ví dụ về Adapter Pattern. `Arrays.asList()` cũng có thể được coi là một dạng adapter, nó chuyển đổi một mảng thành một `List`.

### 3.5. Ưu điểm và Nhược điểm

**Ưu điểm:**

  * **Tăng khả năng tái sử dụng:** Cho phép sử dụng lại các lớp hiện có mà không cần sửa đổi mã nguồn của chúng.
  * **Tăng tính linh hoạt:** Dễ dàng thay thế hoặc thêm mới các `Adaptee` mà không ảnh hưởng đến client, miễn là có `Adapter` tương ứng.
  * **Giảm sự phụ thuộc:** Client chỉ phụ thuộc vào interface `Target`, không phụ thuộc trực tiếp vào các lớp `Adaptee` cụ thể.
  * **Tuân thủ Nguyên tắc Mở/Đóng (Open/Closed Principle):** Có thể giới thiệu các adapter mới để làm việc với các adaptee mới mà không cần sửa đổi mã nguồn hiện có của client hoặc target.

**Nhược điểm:**

  * **Tăng độ phức tạp:** Thêm một lớp `Adapter` vào hệ thống có thể làm tăng số lượng lớp và độ phức tạp tổng thể của mã nguồn.
  * **Overhead tiềm ẩn:** Việc chuyển đổi dữ liệu hoặc lời gọi phương thức qua Adapter có thể gây ra một chút overhead về hiệu năng, mặc dù thường không đáng kể.
  * **Có thể che giấu vấn đề thiết kế của Adaptee:** Nếu `Adaptee` có thiết kế tồi, việc "bọc" nó bằng Adapter chỉ giải quyết vấn đề tương thích interface chứ không giải quyết các vấn đề tiềm ẩn bên trong `Adaptee`.

### 3.6. Mẹo thực chiến và Phương pháp hay nhất

  * **Giữ Adapter đơn giản:** Mục tiêu chính của Adapter là chuyển đổi interface. Tránh thêm quá nhiều logic nghiệp vụ không liên quan vào Adapter.
  * **Thiết kế interface `Target` tốt:** `Target` nên là một interface rõ ràng, dễ hiểu và đáp ứng đúng nhu cầu của client.
  * **Sử dụng Object Adapter thường xuyên hơn Class Adapter:** Object Adapter linh hoạt hơn, cho phép thay đổi Adaptee lúc runtime và có thể adapt một lớp con bất kỳ của Adaptee. Class Adapter bị giới hạn bởi cơ chế kế thừa của ngôn ngữ.
  * **Cân nhắc Two-Way Adapter:** Một Two-Way Adapter có thể hoạt động như một adapter cho cả hai chiều, tức là nó có thể làm cho Adaptee trông giống Target và Target trông giống Adaptee. Điều này phức tạp hơn và ít phổ biến hơn.
  * **Test kỹ lưỡng:** Đảm bảo Adapter hoạt động đúng như mong đợi, bao gồm việc chuyển đổi dữ liệu và xử lý lỗi một cách chính xác.

### 3.7. Lỗi phổ biến và Cách khắc phục

  * **Adapter làm quá nhiều việc:**
      * **Vấn đề:** Adapter không chỉ chuyển đổi interface mà còn thực hiện thêm nhiều logic nghiệp vụ phức tạp, vi phạm Nguyên tắc Trách nhiệm Đơn (SRP).
      * **Khắc phục:** Tách logic nghiệp vụ ra các lớp riêng và giữ cho Adapter chỉ tập trung vào việc chuyển đổi interface.
  * **Lộ chi tiết của Adaptee cho Client:**
      * **Vấn đề:** Client vô tình hoặc cố ý truy cập trực tiếp vào Adaptee thay vì thông qua Target interface, làm mất đi lợi ích của Adapter.
      * **Khắc phục:** Đảm bảo Adapter đóng gói hoàn toàn Adaptee và Client chỉ tương tác thông qua Target.
  * **Không xử lý ngoại lệ từ Adaptee:**
      * **Vấn đề:** Adaptee có thể ném ra các ngoại lệ mà Target interface hoặc Client không mong đợi.
      * **Khắc phục:** Adapter nên bắt các ngoại lệ từ Adaptee và chuyển đổi chúng thành các ngoại lệ phù hợp với Target interface hoặc xử lý chúng một cách thích hợp.

## 4\. Facade Pattern (Mẫu Mặt Tiền)

### 4.1. Khái niệm cốt lõi

**Facade Pattern** là một mẫu thiết kế cấu trúc cung cấp một interface đơn giản, thống nhất cho một tập hợp các interface phức tạp trong một hệ thống con (subsystem). Thay vì client phải tương tác trực tiếp với nhiều thành phần khác nhau của hệ thống con, Facade đứng ra làm "mặt tiền", che giấu sự phức tạp bên trong và cung cấp một cách sử dụng dễ dàng hơn.

Hãy tưởng tượng việc khởi động một hệ thống giải trí tại gia phức tạp (TV, amply, đầu đĩa, loa...). Thay vì phải bật từng thiết bị và cài đặt từng thứ một, bạn có một chiếc điều khiển từ xa "thông minh" (Facade) với một nút "Xem Phim". Nhấn nút này, điều khiển sẽ tự động thực hiện tất cả các thao tác cần thiết.

Mục tiêu chính của Facade Pattern là giảm sự phụ thuộc của client vào các chi tiết triển khai phức tạp của một hệ thống con, từ đó làm cho hệ thống dễ sử dụng, dễ hiểu và dễ bảo trì hơn.

**Các thành phần chính:**

  * **Facade (Mặt tiền):** Lớp cung cấp interface đơn giản hóa cho hệ thống con. Nó biết những lớp nào trong hệ thống con chịu trách nhiệm cho một yêu cầu và ủy thác các yêu cầu của client đến các đối tượng hệ thống con thích hợp.
  * **Subsystem classes (Các lớp hệ thống con):** Các lớp triển khai chức năng của hệ thống con. Chúng thực hiện công việc thực sự. Các lớp này không biết về sự tồn tại của Facade và có thể được sử dụng trực tiếp bởi các client "cao cấp" nếu cần truy cập sâu hơn.
  * **Client (Máy khách):** Lớp sử dụng Facade để tương tác với hệ thống con.

### 4.2. Cơ chế hoạt động bên trong

1.  Client gửi một yêu cầu đến `Facade` thông qua interface đơn giản mà `Facade` cung cấp.
2.  `Facade` nhận yêu cầu này và điều phối công việc bằng cách gọi các phương thức thích hợp trên các lớp thuộc hệ thống con.
3.  Các lớp trong hệ thống con thực hiện các tác vụ được giao.
4.  `Facade` có thể thực hiện thêm một số công việc tổng hợp hoặc xử lý kết quả trước khi trả về cho client.

Client không cần biết về sự phức tạp của các tương tác giữa các thành phần bên trong hệ thống con. Facade đóng vai trò như một điểm vào duy nhất (hoặc ít nhất là một điểm vào chính) cho hệ thống con.

### 4.3. Ví dụ mã nguồn (Java)

Giả sử chúng ta có một hệ thống xử lý đơn hàng phức tạp bao gồm các dịch vụ: `InventoryService` (kiểm tra tồn kho), `PaymentService` (xử lý thanh toán), và `ShippingService` (vận chuyển).

```java
// Subsystem classes
class InventoryService {
    public boolean isAvailable(String productId, int quantity) {
        System.out.println("InventoryService: Checking availability for " + quantity + " of product " + productId);
        // Logic kiểm tra tồn kho phức tạp
        return quantity < 10; // Giả sử luôn có sẵn nếu số lượng < 10
    }
}

class PaymentService {
    public boolean processPayment(String userId, double amount) {
        System.out.println("PaymentService: Processing payment of $" + amount + " for user " + userId);
        // Logic xử lý thanh toán phức tạp
        return true; // Giả sử thanh toán luôn thành công
    }
}

class ShippingService {
    public void shipProduct(String productId, String address) {
        System.out.println("ShippingService: Shipping product " + productId + " to " + address);
        // Logic vận chuyển phức tạp
    }
}

// Facade
class OrderFacade {
    private InventoryService inventoryService;
    private PaymentService paymentService;
    private ShippingService shippingService;

    public OrderFacade() {
        this.inventoryService = new InventoryService();
        this.paymentService = new PaymentService();
        this.shippingService = new ShippingService();
    }

    // Phương thức đơn giản hóa việc đặt hàng
    public boolean placeOrder(String userId, String productId, int quantity, double price, String shippingAddress) {
        System.out.println("\nOrderFacade: Processing order for user " + userId + ", product " + productId);

        // 1. Kiểm tra tồn kho
        if (!inventoryService.isAvailable(productId, quantity)) {
            System.out.println("OrderFacade: Product " + productId + " is out of stock for quantity " + quantity);
            return false;
        }

        // 2. Xử lý thanh toán
        if (!paymentService.processPayment(userId, price * quantity)) {
            System.out.println("OrderFacade: Payment failed for user " + userId);
            return false;
        }

        // 3. Vận chuyển
        shippingService.shipProduct(productId, shippingAddress);

        System.out.println("OrderFacade: Order placed successfully!");
        return true;
    }
}

// Client
public class FacadeDemo {
    public static void main(String[] args) {
        OrderFacade orderFacade = new OrderFacade();

        // Client chỉ cần gọi một phương thức đơn giản trên Facade
        boolean order1Result = orderFacade.placeOrder("user123", "productABC", 2, 50.0, "123 Main St, Anytown");
        System.out.println("Order 1 processing result: " + (order1Result ? "Success" : "Failed"));

        boolean order2Result = orderFacade.placeOrder("user456", "productXYZ", 15, 20.0, "456 Oak Ave, Otherville");
        System.out.println("Order 2 processing result: " + (order2Result ? "Success" : "Failed"));
    }
}
```

**Giải thích mã nguồn:**

  * `InventoryService`, `PaymentService`, `ShippingService`: Là các lớp của hệ thống con, mỗi lớp thực hiện một phần của quy trình đặt hàng.
  * `OrderFacade`: Là Facade. Nó cung cấp một phương thức `placeOrder()` duy nhất, đơn giản hóa cho client. Bên trong `placeOrder()`, Facade điều phối các lời gọi đến các service của hệ thống con.
  * `FacadeDemo` (Client): Client chỉ cần tương tác với `OrderFacade` để đặt hàng mà không cần biết về sự tồn tại hay cách hoạt động của `InventoryService`, `PaymentService`, hay `ShippingService`.

### 4.4. Trường hợp sử dụng thực tế

  * **Đơn giản hóa việc sử dụng các thư viện hoặc framework phức tạp:** Cung cấp một API tiện lợi hơn cho các tác vụ phổ biến.
  * **Cấu trúc một hệ thống thành các lớp (layers):** Facade có thể là điểm vào cho một lớp trong kiến trúc phân lớp (layered architecture).
  * **Bao bọc các hệ thống cũ:** Khi tích hợp với các hệ thống cũ có API phức tạp hoặc khó sử dụng.
  * **Giảm sự phụ thuộc giữa client và các lớp hệ thống con:** Client chỉ phụ thuộc vào Facade. Nếu các lớp hệ thống con thay đổi, chỉ cần cập nhật Facade mà không ảnh hưởng đến client (miễn là interface của Facade không đổi).
  * **API cho các ứng dụng đa nền tảng:** Một Facade có thể cung cấp một API chung, bên dưới nó là các triển khai cụ thể cho từng nền tảng.
  * **Thư viện `java.net.URL`:** Khi bạn sử dụng lớp `URL` để lấy nội dung từ một địa chỉ web, nó hoạt động như một Facade, che giấu sự phức tạp của việc thiết lập kết nối, gửi yêu cầu HTTP, và nhận phản hồi.

### 4.5. Ưu điểm và Nhược điểm

**Ưu điểm:**

  * **Đơn giản hóa việc sử dụng hệ thống con:** Client chỉ cần biết về Facade, không cần quan tâm đến sự phức tạp bên trong.
  * **Giảm sự phụ thuộc (Decoupling):** Tách biệt client khỏi các thành phần của hệ thống con. Điều này giúp hệ thống con có thể phát triển và thay đổi độc lập hơn.
  * **Cải thiện khả năng đọc và bảo trì mã nguồn:** Mã nguồn của client trở nên gọn gàng và dễ hiểu hơn.
  * **Tăng tính module hóa:** Khuyến khích việc chia hệ thống thành các subsystem nhỏ hơn, dễ quản lý.

**Nhược điểm:**

  * **Facade có thể trở thành "God Object":** Nếu Facade cố gắng bao bọc quá nhiều chức năng và trở nên quá lớn, nó có thể vi phạm Nguyên tắc Trách nhiệm Đơn và trở nên khó bảo trì.
  * **Không che giấu hoàn toàn sự phức tạp:** Đối với các client "cao cấp" cần truy cập sâu hơn vào các chức năng của hệ thống con, Facade có thể không đủ. Tuy nhiên, Facade không ngăn cản việc truy cập trực tiếp này nếu cần.
  * **Thêm một lớp trung gian:** Giống như Adapter, Facade thêm một lớp nữa vào hệ thống.

### 4.6. So sánh với các mẫu khác

  * **Facade vs. Adapter:**
      * Mục đích của **Adapter** là thay đổi một interface hiện có thành một interface khác mà client mong đợi. Nó giải quyết vấn đề không tương thích interface.
      * Mục đích của **Facade** là cung cấp một interface đơn giản hơn cho một hệ thống con phức tạp. Nó không nhất thiết phải thay đổi interface, mà là đơn giản hóa nó.
      * Adapter thường "bọc" một đối tượng, Facade thường "bọc" một tập hợp các đối tượng (hệ thống con).
  * **Facade vs. Mediator:**
      * **Facade** đơn giản hóa interface của một hệ thống con, client vẫn gọi Facade.
      * **Mediator** quản lý sự tương tác phức tạp giữa một nhóm các đối tượng (colleagues), các colleagues không giao tiếp trực tiếp với nhau mà thông qua Mediator. Facade không ngăn cản các thành phần subsystem giao tiếp với nhau.
  * **Facade vs. Abstract Factory:**
      * **Facade** tập trung vào việc đơn giản hóa việc sử dụng một hệ thống con (các hành vi).
      * **Abstract Factory** tập trung vào việc tạo ra các họ đối tượng liên quan (khởi tạo).

### 4.7. Mẹo thực chiến và Phương pháp hay nhất

  * **Xác định rõ phạm vi của Facade:** Facade nên cung cấp các chức năng mà đa số client cần. Tránh làm Facade quá ôm đồm.
  * **Không bắt buộc Client phải dùng Facade:** Client vẫn có thể truy cập trực tiếp các lớp của hệ thống con nếu cần các chức năng nâng cao mà Facade không cung cấp. Facade chỉ là một lựa chọn tiện lợi.
  * **Facade không cần quản lý vòng đời của các đối tượng hệ thống con:** Thông thường, Facade sẽ tạo hoặc được cung cấp các instance của các lớp hệ thống con mà nó sử dụng.
  * **Có thể có nhiều Facade:** Nếu một hệ thống con quá lớn, có thể chia nhỏ nó và cung cấp nhiều Facade chuyên biệt hơn.
  * **Facade có thể là Singleton:** Nếu trạng thái của Facade không thay đổi hoặc không cần nhiều instance, nó có thể được triển khai như một Singleton.

### 4.8. Lỗi phổ biến và Cách khắc phục

  * **Facade quá lớn (God Object Facade):**
      * **Vấn đề:** Facade cố gắng cung cấp quá nhiều phương thức, bao trùm mọi khía cạnh của hệ thống con, trở nên cồng kềnh và khó quản lý.
      * **Khắc phục:** Chia Facade thành nhiều Facade nhỏ hơn, mỗi Facade tập trung vào một nhóm chức năng cụ thể của hệ thống con.
  * **Rò rỉ chi tiết triển khai của hệ thống con:**
      * **Vấn đề:** Interface của Facade để lộ các kiểu dữ liệu hoặc khái niệm từ bên trong hệ thống con, làm giảm mức độ trừu tượng.
      * **Khắc phục:** Thiết kế interface của Facade sử dụng các kiểu dữ liệu và khái niệm độc lập với hệ thống con. Facade chịu trách nhiệm chuyển đổi nếu cần.
  * **Facade làm thay đổi hành vi của hệ thống con:**
      * **Vấn đề:** Facade không chỉ đơn giản hóa mà còn thêm vào logic mới làm thay đổi cách hệ thống con hoạt động cơ bản.
      * **Khắc phục:** Mục tiêu chính của Facade là đơn giản hóa *việc sử dụng*. Nếu cần thay đổi hành vi, cân nhắc các mẫu khác hoặc điều chỉnh trực tiếp hệ thống con.

## 5\. Proxy Pattern (Mẫu Đại Diện)

### 5.1. Khái niệm cốt lõi

**Proxy Pattern** là một mẫu thiết kế cấu trúc cung cấp một đối tượng thay thế hoặc đại diện (surrogate or placeholder) cho một đối tượng khác để kiểm soát quyền truy cập vào đối tượng đó. Proxy có cùng interface với đối tượng thật (Real Subject), cho phép client tương tác với Proxy như thể nó đang tương tác trực tiếp với đối tượng thật.

Proxy đứng giữa client và đối tượng thật, có thể thực hiện các hành động bổ sung trước hoặc sau khi ủy thác yêu cầu cho đối tượng thật. Điều này cho phép Proxy quản lý vòng đời, kiểm soát truy cập, thực hiện lazy loading, logging, caching, v.v.

**Các thành phần chính:**

  * **Subject (Chủ thể):** Interface chung cho cả `RealSubject` và `Proxy`. Client sẽ tương tác thông qua interface này.
  * **RealSubject (Chủ thể thật):** Đối tượng thực sự mà Proxy đại diện. Đây là đối tượng chứa logic nghiệp vụ chính.
  * **Proxy (Đại diện):** Lớp có cùng interface với `RealSubject`. Nó giữ một tham chiếu đến `RealSubject` (có thể được tạo khi cần). `Proxy` có thể thực hiện các tác vụ bổ sung trước/sau khi gọi các phương thức của `RealSubject`.
  * **Client (Máy khách):** Tương tác với đối tượng thông qua interface `Subject`, không biết mình đang làm việc với `Proxy` hay `RealSubject`.

### 5.2. Các loại Proxy phổ biến

Có nhiều loại Proxy khác nhau, tùy thuộc vào mục đích sử dụng:

  * **Virtual Proxy (Proxy ảo):** Trì hoãn việc tạo hoặc khởi tạo đối tượng `RealSubject` cho đến khi nó thực sự cần thiết (lazy loading). Hữu ích cho các đối tượng tốn kém tài nguyên để tạo.
  * **Protection Proxy (Proxy bảo vệ):** Kiểm soát quyền truy cập vào `RealSubject` dựa trên các quyền hạn của client.
  * **Remote Proxy (Proxy từ xa):** Đại diện cho một đối tượng nằm ở một không gian địa chỉ khác (ví dụ, trên một máy chủ từ xa). Proxy quản lý việc giao tiếp mạng. (Ví dụ: RMI của Java).
  * **Logging Proxy (Proxy ghi log):** Ghi lại thông tin về các lời gọi phương thức đến `RealSubject`.
  * **Caching Proxy (Proxy bộ đệm):** Lưu trữ kết quả của các yêu cầu tốn kém và trả về kết quả đã lưu nếu yêu cầu được lặp lại.
  * **Smart Reference Proxy (Proxy tham chiếu thông minh):** Thực hiện các hành động bổ sung khi đối tượng được truy cập, ví dụ như đếm số lượng tham chiếu.

### 5.3. Cơ chế hoạt động bên trong

1.  Client yêu cầu một thao tác trên đối tượng `Subject`. Thực tế, client đang gọi phương thức trên `Proxy`.
2.  `Proxy` nhận yêu cầu.
3.  Tùy thuộc vào loại Proxy và logic của nó, `Proxy` có thể:
      * Tạo/khởi tạo `RealSubject` nếu chưa có (ví dụ: Virtual Proxy).
      * Kiểm tra quyền truy cập của client (ví dụ: Protection Proxy).
      * Ghi log lời gọi (ví dụ: Logging Proxy).
      * Kiểm tra cache trước khi gọi `RealSubject` (ví dụ: Caching Proxy).
      * Chuyển tiếp yêu cầu qua mạng (ví dụ: Remote Proxy).
4.  Nếu cần, `Proxy` ủy thác yêu cầu cho `RealSubject`.
5.  `RealSubject` thực hiện công việc.
6.  `Proxy` có thể thực hiện thêm các thao tác sau khi `RealSubject` hoàn thành (ví dụ, ghi log kết quả, cập nhật cache).
7.  `Proxy` trả kết quả (nếu có) cho client.

### 5.4. Ví dụ mã nguồn (Java) - Virtual Proxy

Chúng ta sẽ tạo một Virtual Proxy cho việc tải và hiển thị hình ảnh. `RealImage` là đối tượng tốn kém để tải từ đĩa. `ImageProxy` sẽ chỉ tải hình ảnh khi phương thức `display()` được gọi.

```java
// Subject Interface
interface Image {
    void display();
    String getFileName();
}

// RealSubject - Đối tượng thật, tốn kém để tạo
class RealImage implements Image {
    private String fileName;

    public RealImage(String fileName) {
        this.fileName = fileName;
        loadFromDisk(); // Thao tác tốn kém
    }

    private void loadFromDisk() {
        System.out.println("RealImage: Loading image '" + fileName + "' from disk...");
        // Giả lập độ trễ khi tải ảnh
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("RealImage: Image '" + fileName + "' loaded.");
    }

    @Override
    public void display() {
        System.out.println("RealImage: Displaying image '" + fileName + "'.");
    }

    @Override
    public String getFileName() {
        return fileName;
    }
}

// Proxy - Virtual Proxy
class ImageProxy implements Image {
    private RealImage realImage; // Tham chiếu đến RealSubject
    private String fileName;

    public ImageProxy(String fileName) {
        this.fileName = fileName;
        System.out.println("ImageProxy: Created for '" + fileName + "'. Real image not loaded yet.");
    }

    @Override
    public void display() {
        if (realImage == null) {
            System.out.println("ImageProxy: RealImage is null. Initializing RealImage for '" + fileName + "'.");
            realImage = new RealImage(fileName); // Lazy loading
        }
        System.out.println("ImageProxy: Delegating display call to RealImage for '" + fileName + "'.");
        realImage.display();
    }

    @Override
    public String getFileName() {
        // Có thể trả về fileName trực tiếp hoặc ủy thác cho realImage nếu nó đã được tạo
        // và fileName có thể thay đổi hoặc cần được lấy từ realImage.
        // Ở đây, fileName là cố định khi Proxy được tạo.
        return fileName;
    }
}

// Client
public class ProxyDemo {
    public static void main(String[] args) {
        System.out.println("Creating proxy for image1.jpg...");
        Image image1 = new ImageProxy("image1.jpg"); // RealImage chưa được tải

        System.out.println("\nCreating proxy for image2.png...");
        Image image2 = new ImageProxy("image2.png"); // RealImage chưa được tải

        // Client thao tác với image1
        System.out.println("\nClient requests to display image1.jpg:");
        image1.display(); // Lúc này RealImage cho image1.jpg mới được tải và hiển thị

        System.out.println("\nClient requests to display image1.jpg again:");
        image1.display(); // RealImage đã được tải, chỉ cần hiển thị

        // Client thao tác với image2
        System.out.println("\nClient requests to display image2.png:");
        image2.display(); // Lúc này RealImage cho image2.png mới được tải và hiển thị
        
        System.out.println("\nAccessing file name for image1: " + image1.getFileName());
    }
}
```

**Giải thích mã nguồn:**

  * `Image`: Là `Subject` interface, định nghĩa các thao tác chung.
  * `RealImage`: Là `RealSubject`. Constructor của nó gọi `loadFromDisk()`, một thao tác tốn kém.
  * `ImageProxy`: Là `Proxy`. Nó cũng implement `Image`. Nó chứa một tham chiếu `realImage` (ban đầu là `null`) và `fileName`. Khi phương thức `display()` được gọi lần đầu, `ImageProxy` mới tạo instance của `RealImage` (lazy loading) rồi mới ủy thác việc hiển thị. Các lần gọi `display()` sau đó sẽ sử dụng `realImage` đã được tạo.
  * `ProxyDemo` (Client): Tạo các `ImageProxy`. `RealImage` chỉ được tải khi `display()` được gọi, giúp tiết kiệm tài nguyên nếu hình ảnh không bao giờ được hiển thị.

### 5.5. Trường hợp sử dụng thực tế

  * **Virtual Proxy:**
      * Tải hình ảnh hoặc tài liệu lớn chỉ khi chúng thực sự được hiển thị hoặc truy cập.
      * Khởi tạo các đối tượng phức tạp hoặc tốn nhiều tài nguyên một cách trì hoãn.
  * **Protection Proxy:**
      * Kiểm soát quyền truy cập vào các đối tượng dựa trên vai trò hoặc quyền của người dùng trong các hệ thống quản lý.
      * Ví dụ, một nhân viên chỉ có thể xem thông tin của mình, trong khi quản lý có thể xem thông tin của nhiều nhân viên.
  * **Remote Proxy:**
      * Trong các hệ thống phân tán như RMI (Remote Method Invocation) của Java, CORBA. Client tương tác với một Proxy cục bộ, Proxy này xử lý giao tiếp mạng để gọi phương thức trên đối tượng ở máy chủ từ xa.
  * **Logging Proxy:**
      * Ghi lại các lời gọi API, tham số và kết quả để phục vụ cho việc gỡ lỗi hoặc kiểm toán (auditing).
  * **Caching Proxy:**
      * Lưu trữ kết quả của các truy vấn cơ sở dữ liệu hoặc các lời gọi dịch vụ web tốn kém để tăng tốc độ phản hồi cho các yêu cầu lặp lại.
  * **Java API:**
      * `java.lang.reflect.Proxy`: Một lớp tiện ích trong Java cho phép tạo các đối tượng proxy động tại runtime cho một tập hợp các interface nhất định. Thường được sử dụng trong AOP (Aspect-Oriented Programming) và các framework như Spring.
      * Các đối tượng lazy-loaded trong các ORM framework (như Hibernate) thường sử dụng cơ chế tương tự Virtual Proxy.

### 5.6. Ưu điểm và Nhược điểm

**Ưu điểm:**

  * **Kiểm soát truy cập:** Cho phép quản lý và kiểm soát cách client tương tác với đối tượng thật.
  * **Lazy Loading:** Cải thiện hiệu năng bằng cách trì hoãn việc tạo các đối tượng tốn kém.
  * **Tách biệt các mối quan tâm:** Các logic bổ sung (logging, caching, security) có thể được thực hiện trong Proxy mà không làm thay đổi mã nguồn của `RealSubject`.
  * **Tính linh hoạt:** Có thể thay đổi đối tượng Proxy mà không ảnh hưởng đến Client hoặc `RealSubject` (miễn là interface `Subject` không đổi).
  * **Hỗ trợ các tác vụ ẩn:** Như giao tiếp mạng trong Remote Proxy.

**Nhược điểm:**

  * **Tăng độ phức tạp:** Thêm một lớp Proxy nữa vào hệ thống.
  * **Overhead tiềm ẩn:** Lời gọi phương thức qua Proxy có thể chậm hơn một chút so với gọi trực tiếp `RealSubject` do có thêm lớp trung gian.
  * **Interface của Proxy có thể khác RealSubject (ít phổ biến):** Mặc dù Proxy thường implement cùng interface với `RealSubject`, trong một số trường hợp hiếm, Proxy có thể cung cấp một interface hơi khác hoặc mở rộng. Điều này cần được cân nhắc cẩn thận.

### 5.7. So sánh với các mẫu khác

  * **Proxy vs. Decorator:**
      * Cả hai đều "bọc" một đối tượng và có cùng interface.
      * **Decorator** thêm các *trách nhiệm/hành vi mới* cho một đối tượng một cách linh hoạt. Bạn có thể "trang trí" một đối tượng với nhiều Decorator.
      * **Proxy** *kiểm soát quyền truy cập* vào một đối tượng hoặc quản lý vòng đời của nó. Proxy thường quản lý đối tượng mà nó đại diện và không được thiết kế để client thêm/bớt các proxy một cách linh hoạt như Decorator.
      * Mục đích khác nhau: Decorator là để mở rộng chức năng, Proxy là để kiểm soát hoặc quản lý.
  * **Proxy vs. Adapter:**
      * **Adapter** thay đổi interface của một đối tượng để làm cho nó tương thích với client.
      * **Proxy** cung cấp cùng một interface với đối tượng thật nhưng kiểm soát quyền truy cập hoặc thêm các hành vi phụ trợ.
  * **Proxy vs. Facade:**
      * **Facade** cung cấp một interface đơn giản cho một *hệ thống con* phức tạp (nhiều đối tượng).
      * **Proxy** đại diện cho *một đối tượng* duy nhất.

### 5.8. Mẹo thực chiến và Phương pháp hay nhất

  * **Giữ Proxy nhẹ nhàng:** Logic trong Proxy nên tập trung vào mục đích chính của nó (ví dụ: lazy loading, kiểm soát truy cập).
  * **Sử dụng `java.lang.reflect.Proxy` cho Dynamic Proxies:** Nếu bạn cần tạo Proxy tại runtime cho nhiều interface khác nhau mà không muốn viết các lớp Proxy tĩnh, Dynamic Proxy của Java là một công cụ mạnh mẽ.
  * **Cẩn thận với vòng đời của RealSubject:** Đặc biệt với Virtual Proxy, đảm bảo `RealSubject` được tạo và quản lý đúng cách.
  * **Truyền tải ngoại lệ một cách minh bạch:** Proxy nên xử lý hoặc truyền tải các ngoại lệ từ `RealSubject` một cách phù hợp.
  * **Cân nhắc hiệu năng:** Mặc dù Proxy có thể cải thiện hiệu năng (ví dụ, qua lazy loading hoặc caching), bản thân lớp Proxy cũng thêm một mức độ gián tiếp. Đánh giá tác động hiệu năng trong các tình huống cụ thể.

### 5.9. Lỗi phổ biến và Cách khắc phục

  * **Proxy quá "thông minh" hoặc quá phức tạp:**
      * **Vấn đề:** Proxy chứa quá nhiều logic không liên quan đến việc kiểm soát truy cập hoặc đại diện, làm cho nó khó hiểu và bảo trì.
      * **Khắc phục:** Tách các trách nhiệm không cốt lõi ra các lớp khác. Giữ Proxy tập trung.
  * **Lazy initialization không an toàn trong đa luồng:**
      * **Vấn đề:** Nếu Virtual Proxy được sử dụng trong môi trường đa luồng, việc khởi tạo `RealSubject` có thể không an toàn, dẫn đến nhiều instance được tạo.
      * **Khắc phục:** Sử dụng các kỹ thuật đồng bộ hóa (ví dụ: `synchronized`, double-checked locking với `volatile`) khi khởi tạo `RealSubject` trong Proxy.
  * **Client bỏ qua Proxy và truy cập RealSubject trực tiếp:**
      * **Vấn đề:** Nếu client có cách để lấy tham chiếu trực tiếp đến `RealSubject`, các cơ chế kiểm soát của Proxy sẽ bị vô hiệu hóa.
      * **Khắc phục:** Thiết kế hệ thống sao cho client chỉ có thể truy cập `RealSubject` thông qua Proxy (ví dụ, bằng cách sử dụng Factory để tạo Proxy và không để lộ constructor của `RealSubject`).

## 6\. Các Structural Design Patterns Khác (Tổng quan)

Ngoài Adapter, Facade và Proxy, có một số Structural Patterns quan trọng khác:

### 6.1. Decorator Pattern (Mẫu Trang Trí)

  * **Mục đích:** Cho phép thêm các trách nhiệm/chức năng mới vào một đối tượng một cách linh hoạt tại runtime, mà không cần sửa đổi mã nguồn của đối tượng đó hoặc tạo ra nhiều lớp con.
  * **Cách hoạt động:** Decorator "bọc" (wraps) đối tượng gốc. Cả Decorator và đối tượng gốc đều implement cùng một interface. Decorator ủy thác các yêu cầu cho đối tượng được bọc và có thể thực hiện các hành vi bổ sung trước hoặc sau lời gọi đó. Có thể xếp chồng nhiều Decorator.
  * **Ví dụ:** Thêm thanh cuộn, đường viền vào một cửa sổ giao diện người dùng; thêm các topping khác nhau vào một ly cà phê (đối tượng gốc là cà phê, các topping là decorator). Các lớp `java.io` (như `BufferedInputStream` bọc một `InputStream`) là ví dụ điển hình.
  * **Khi nào dùng:** Khi bạn muốn thêm chức năng cho đối tượng một cách linh hoạt và tránh việc tạo ra quá nhiều lớp con do các tổ hợp tính năng.

### 6.2. Bridge Pattern (Mẫu Cầu Nối)

  * **Mục đích:** Tách một lớp trừu tượng (abstraction) khỏi các triển khai (implementation) của nó để cả hai có thể thay đổi một cách độc lập. "Ưu tiên composition hơn inheritance".
  * **Cách hoạt động:** Tạo hai hệ thống phân cấp riêng biệt: một cho abstraction và một cho implementation. Abstraction chứa một tham chiếu đến một đối tượng Implementor. Client tương tác với Abstraction, Abstraction sẽ ủy thác công việc cho Implementor.
  * **Ví dụ:** Một lớp `Shape` (Abstraction) có thể có các lớp con như `Circle`, `Square`. Các `Shape` này có thể được vẽ bằng các API đồ họa khác nhau (Implementation) như `VectorDrawingAPI`, `RasterDrawingAPI`. Thay vì tạo `VectorCircle`, `RasterCircle`, `VectorSquare`, `RasterSquare`, Bridge cho phép `Circle` và `Square` tham chiếu đến một `DrawingAPI` và thay đổi `DrawingAPI` độc lập.
  * **Khi nào dùng:** Khi bạn muốn tránh việc số lượng lớp tăng theo cấp số nhân do có nhiều chiều biến thể (ví dụ: loại hình và loại API vẽ); khi bạn muốn thay đổi implementation lúc runtime.

### 6.3. Composite Pattern (Mẫu Hỗn Hợp/Thành Phần)

  * **Mục đích:** Cho phép client xử lý các đối tượng riêng lẻ (leaf) và các nhóm đối tượng (composite) một cách thống nhất. Tạo ra một cấu trúc cây để biểu diễn các hệ thống phân cấp một phần - toàn thể (part-whole hierarchies).
  * **Cách hoạt động:** Định nghĩa một interface `Component` chung cho cả đối tượng lá (`Leaf`) và đối tượng phức hợp (`Composite`). `Composite` chứa một tập hợp các `Component` con (có thể là `Leaf` hoặc `Composite` khác) và ủy thác các thao tác cho các con của nó.
  * **Ví dụ:** Biểu diễn cấu trúc thư mục và file (thư mục là Composite, file là Leaf); các thành phần trong giao diện người dùng (một panel có thể chứa các button, text field, hoặc các panel khác).
  * **Khi nào dùng:** Khi bạn muốn biểu diễn một cấu trúc cây và muốn client có thể đối xử với các đối tượng đơn lẻ và các nhóm đối tượng theo cùng một cách.

### 6.4. Flyweight Pattern (Mẫu Đối Tượng Nhẹ)

  * **Mục đích:** Giảm thiểu việc sử dụng bộ nhớ bằng cách chia sẻ càng nhiều dữ liệu càng tốt với các đối tượng tương tự khác. Nó tập trung vào việc giảm số lượng đối tượng được tạo ra.
  * **Cách hoạt động:** Tách trạng thái của đối tượng thành hai phần:
      * **Intrinsic state (trạng thái nội tại):** Là phần dữ liệu không đổi, có thể chia sẻ giữa nhiều đối tượng (ví dụ: font chữ, màu sắc của một ký tự).
      * **Extrinsic state (trạng thái ngoại tại):** Là phần dữ liệu thay đổi theo ngữ cảnh, được client cung cấp khi cần (ví dụ: vị trí của một ký tự trên màn hình).
        Một Factory sẽ quản lý và trả về các đối tượng Flyweight đã được chia sẻ (chứa intrinsic state).
  * **Ví dụ:** Trong một trình soạn thảo văn bản, mỗi ký tự (ví dụ: 'A' với font Arial, size 12) có thể là một Flyweight. Thay vì tạo hàng ngàn đối tượng ký tự riêng biệt nếu có nhiều ký tự 'A' giống hệt nhau, chỉ một đối tượng Flyweight 'A' được chia sẻ. Vị trí của mỗi ký tự 'A' là extrinsic state.
  * **Khi nào dùng:** Khi ứng dụng cần tạo ra một số lượng rất lớn các đối tượng nhỏ, và nhiều đối tượng trong số đó có thể chia sẻ một phần trạng thái chung để tiết kiệm bộ nhớ.

## 7\. Kết luận

Structural Design Patterns cung cấp những giải pháp đã được chứng minh để giải quyết các vấn đề phổ biến liên quan đến cách cấu trúc các lớp và đối tượng trong phần mềm. Chúng giúp tạo ra các hệ thống linh hoạt, dễ bảo trì, dễ mở rộng và dễ hiểu hơn bằng cách quản lý các mối quan hệ và sự phụ thuộc một cách hiệu quả.

  * **Adapter** giúp các interface không tương thích làm việc cùng nhau.
  * **Facade** cung cấp một interface đơn giản cho một hệ thống con phức tạp.
  * **Proxy** kiểm soát quyền truy cập vào một đối tượng khác.
  * **Decorator** thêm chức năng cho đối tượng một cách linh hoạt.
  * **Bridge** tách biệt abstraction khỏi implementation của nó.
  * **Composite** cho phép xử lý các đối tượng đơn lẻ và nhóm đối tượng một cách thống nhất.
  * **Flyweight** tối ưu hóa việc sử dụng bộ nhớ bằng cách chia sẻ đối tượng.

Việc hiểu rõ và áp dụng đúng các mẫu thiết kế này sẽ giúp lập trình viên xây dựng các ứng dụng có chất lượng cao hơn. Tuy nhiên, điều quan trọng là phải nhận biết được khi nào nên và không nên sử dụng một mẫu cụ thể, tránh việc áp dụng một cách máy móc có thể dẫn đến sự phức tạp không cần thiết. Luôn cân nhắc bối cảnh cụ thể của bài toán và các yêu cầu của hệ thống để đưa ra lựa chọn thiết kế phù hợp nhất.