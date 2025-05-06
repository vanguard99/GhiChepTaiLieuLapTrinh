# KIỂM SOÁT QUYỀN TRUY CẬP

## 1. Giới thiệu về Kiểm soát Truy cập và Tổ chức Mã nguồn

Trong quá trình phát triển phần mềm bằng Java, việc viết mã chỉ là một phần. Hai khía cạnh cực kỳ quan trọng khác là **kiểm soát truy cập** (хác định thành phần nào của mã nguồn có thể được truy cập từ đâu) và **tổ chức mã nguồn** (sắp xếp các lớp và giao diện một cách logic). Nắm vững các cơ chế này giúp xây dựng hệ thống an toàn hơn, dễ bảo trì, dễ mở rộng và dễ hiểu hơn.

Java cung cấp các cơ chế mạnh mẽ để thực hiện điều này, bao gồm:

* **Access Modifiers:** Các từ khóa quy định mức độ hiển thị của lớp và các thành viên của nó (`public`, `protected`, `default`, `private`).
* **Từ khóa `static`:** Phân biệt giữa thành viên thuộc về lớp và thành viên thuộc về đối tượng cụ thể.
* **Packages:** Cơ chế nhóm các lớp và giao diện liên quan lại với nhau, tương tự như các thư mục trong hệ thống tệp.
* **Nested Classes (Lớp Lồng Nhau):** Cho phép định nghĩa lớp bên trong một lớp khác, tăng cường tính đóng gói và tổ chức.

Hướng dẫn này sẽ đi sâu vào từng khái niệm, giải thích cách thức hoạt động, lý do sử dụng và cung cấp các ví dụ thực tế cùng phương pháp hay nhất.

## 2. Ôn tập: Kiểu Dữ liệu Nguyên thủy và Tham chiếu

Trước khi đi vào access modifiers và `static`, cần phân biệt rõ cách Java xử lý hai loại kiểu dữ liệu cơ bản, vì điều này ảnh hưởng đến cách chúng được lưu trữ và truyền vào phương thức.

### 2.1. Biến Nguyên thủy (Primitive Variables)

* Lưu trữ **giá trị thực tế** của dữ liệu trực tiếp trong vùng nhớ được cấp phát cho biến đó (thường là trên **Stack** đối với biến cục bộ).
* Java có 8 kiểu nguyên thủy: `byte`, `short`, `int`, `long`, `float`, `double`, `boolean`, `char`.
* Khi gán một biến nguyên thủy cho biến khác, **giá trị được sao chép**.

```java
int a = 100;
int b = a; // Giá trị 100 được sao chép từ a sang b

b = 200;   // Thay đổi b không ảnh hưởng đến a

System.out.println("a = " + a); // Output: a = 100
System.out.println("b = " + b); // Output: b = 200
```

### 2.2. Biến Tham chiếu (Reference Variables)

* Không lưu trữ đối tượng thực tế mà lưu trữ **địa chỉ bộ nhớ** (tham chiếu) nơi đối tượng đó được lưu trữ trên **Heap**.
* Áp dụng cho tất cả các kiểu không phải nguyên thủy: các lớp (ví dụ: `String`, `ArrayList`, các lớp do bạn tự định nghĩa), mảng, interfaces.
* Khi gán một biến tham chiếu cho biến khác, **địa chỉ (tham chiếu) được sao chép**. Cả hai biến sẽ cùng trỏ đến **một đối tượng duy nhất** trên Heap.

```java
StringBuilder sb1 = new StringBuilder("Hello");
StringBuilder sb2 = sb1; // Địa chỉ của đối tượng StringBuilder mà sb1 trỏ tới được sao chép cho sb2

sb2.append(" World"); // Thay đổi đối tượng thông qua sb2

System.out.println(sb1); // Output: Hello World (sb1 cũng thấy sự thay đổi vì cùng trỏ đến 1 đối tượng)
System.out.println(sb2); // Output: Hello World
```

### 2.3. Stack và Heap

* **Stack:** Vùng nhớ có tốc độ truy cập nhanh, quản lý các lời gọi phương thức (method calls) và các biến cục bộ (local variables), bao gồm cả biến nguyên thủy và biến tham chiếu. Dữ liệu trên Stack có vòng đời ngắn, bị hủy khi phương thức kết thúc.
* **Heap:** Vùng nhớ lớn hơn, nơi các đối tượng (được tạo bằng `new`) được cấp phát. Dữ liệu trên Heap có vòng đời dài hơn, tồn tại miễn là còn ít nhất một tham chiếu trỏ đến nó. Bộ dọn rác (Garbage Collector) của Java chịu trách nhiệm thu hồi bộ nhớ của các đối tượng không còn được tham chiếu trên Heap.

### 2.4. Truyền Tham số vào Phương thức (Pass-by-Value)

Một điểm quan trọng cần nhớ là Java **luôn luôn** sử dụng cơ chế **truyền giá trị (pass-by-value)** khi gọi phương thức:

* **Đối với kiểu nguyên thủy:** Giá trị của biến nguyên thủy được **sao chép** vào tham số của phương thức. Thay đổi giá trị của tham số bên trong phương thức không ảnh hưởng đến biến gốc bên ngoài.

    ```java
    public static void modifyPrimitive(int value) {
        value = 50; // Chỉ thay đổi bản sao 'value' bên trong phương thức
    }

    public static void main(String[] args) {
        int original = 10;
        modifyPrimitive(original);
        System.out.println(original); // Output: 10 (không thay đổi)
    }
    ```
* **Đối với kiểu tham chiếu:** Giá trị của biến tham chiếu (tức là **địa chỉ** của đối tượng) được **sao chép** vào tham số của phương thức. Cả biến gốc và tham số bây giờ cùng trỏ đến **một đối tượng duy nhất** trên Heap.
    * Do đó, nếu phương thức **thay đổi trạng thái nội tại của đối tượng** thông qua tham số, biến gốc bên ngoài cũng sẽ "thấy" sự thay đổi đó (vì chúng cùng trỏ đến một đối tượng).
    * Tuy nhiên, nếu phương thức **gán lại tham số để trỏ đến một đối tượng hoàn toàn mới**, điều này chỉ ảnh hưởng đến tham số bên trong phương thức, không ảnh hưởng đến biến tham chiếu gốc bên ngoài.

    ```java
    class MyData {
        public int value;
        public MyData(int v) { this.value = v; }
    }

    public static void modifyReference(MyData dataRef) {
        // Thay đổi trạng thái đối tượng gốc -> Ảnh hưởng bên ngoài
        dataRef.value = 99;

        // Gán lại tham số -> KHÔNG ảnh hưởng bên ngoài
        // dataRef = new MyData(1000);
    }

    public static void main(String[] args) {
        MyData myObj = new MyData(5);
        modifyReference(myObj);
        System.out.println(myObj.value); // Output: 99
    }
    ```

## 3. Access Modifiers: Kiểm soát Quyền Truy cập

Access Modifiers là các từ khóa trong Java xác định phạm vi truy cập (khả năng hiển thị) của một lớp, constructor, thuộc tính hoặc phương thức. Việc sử dụng chúng hợp lý là nền tảng của **tính đóng gói (Encapsulation)**.

### 3.1. Tại sao cần Kiểm soát Truy cập?

* **Bảo mật:** Ngăn chặn truy cập trái phép hoặc sửa đổi dữ liệu quan trọng từ bên ngoài.
* **Tính đóng gói:** Che giấu chi tiết triển khai nội bộ, chỉ cung cấp giao diện công khai cần thiết.
* **Dễ bảo trì:** Giảm sự phụ thuộc giữa các phần của mã. Khi thay đổi chi tiết nội bộ (private), các phần khác không bị ảnh hưởng miễn là giao diện công khai (public) không đổi.
* **Thiết kế API rõ ràng:** Giúp người dùng thư viện/framework biết được thành phần nào dành cho họ sử dụng.

### 3.2. Các Mức Truy cập

Java cung cấp 4 mức truy cập, từ rộng nhất đến hạn chế nhất:

1.  **`public`**:
    * **Ý nghĩa:** Có thể truy cập từ bất kỳ lớp nào, trong bất kỳ package nào.
    * **Phạm vi:** Rộng nhất.
    * **Sử dụng:** Cho các lớp, phương thức, constructor, thuộc tính muốn công khai hoàn toàn (ví dụ: API chính của thư viện, các phương thức tiện ích chung).

2.  **`protected`**:
    * **Ý nghĩa:** Có thể truy cập bởi các lớp trong **cùng package**, HOẶC bởi các **lớp con (subclasses)** của nó, ngay cả khi lớp con đó nằm ở package khác.
    * **Phạm vi:** Rộng hơn `default`, hẹp hơn `public`.
    * **Sử dụng:** Cho các thành viên mà lớp cha muốn lớp con có thể truy cập hoặc ghi đè, nhưng không muốn công khai hoàn toàn cho "thế giới bên ngoài". Thường dùng trong thiết kế kế thừa.

3.  **`default` (Package-Private)**:
    * **Ý nghĩa:** Khi **không có** access modifier nào được chỉ định rõ ràng (không có từ khóa). Thành viên hoặc lớp chỉ có thể được truy cập bởi các lớp khác **trong cùng một package**.
    * **Phạm vi:** Hẹp hơn `protected` và `public`, rộng hơn `private`.
    * **Sử dụng:** Cho các lớp hoặc thành viên tiện ích chỉ phục vụ nội bộ trong một package, không dành cho bên ngoài package sử dụng.

4.  **`private`**:
    * **Ý nghĩa:** Chỉ có thể truy cập từ **bên trong chính lớp** khai báo nó.
    * **Phạm vi:** Hẹp nhất.
    * **Sử dụng:** **Mặc định nên dùng cho thuộc tính (fields)** để đảm bảo tính đóng gói. Cũng dùng cho các phương thức hoặc constructor helper chỉ dùng nội bộ trong lớp.

### 3.3. Phạm vi áp dụng

* **Cho Lớp (Top-level Class):** Chỉ có thể là `public` hoặc `default`.
    * Lớp `public` có thể được truy cập từ mọi nơi. Tên file `.java` phải trùng với tên lớp `public`.
    * Lớp `default` chỉ có thể được truy cập bởi các lớp khác trong cùng package.
* **Cho Thành viên (Thuộc tính, Phương thức, Constructor, Nested Class):** Có thể sử dụng cả 4 loại: `public`, `protected`, `default`, `private`.

### 3.4. Bảng Tổng hợp

| Modifier    | Trong cùng Lớp | Trong cùng Package (khác lớp) | Lớp con (Khác Package) | Thế giới bên ngoài (Khác Package, không phải lớp con) |
| :---------- | :------------: | :---------------------------: | :--------------------: | :---------------------------------------------------: |
| `public`    |       ✅        |               ✅               |           ✅            |                           ✅                           |
| `protected` |       ✅        |               ✅               |           ✅            |                           ❌                           |
| `default`   |       ✅        |               ✅               |           ❌            |                           ❌                           |
| `private`   |       ✅        |               ❌               |           ❌            |                           ❌                           |

**Giải thích chi tiết:**

* **Trong cùng Lớp:** Mọi thành viên luôn truy cập được từ bên trong lớp chứa nó.
* **Trong cùng Package (khác lớp):** `private` không truy cập được. `default`, `protected`, `public` đều truy cập được.
* **Lớp con (Khác Package):** Chỉ `public` và `protected` mới truy cập được từ lớp con ở package khác. `default` và `private` không được. *Lưu ý: Lớp con truy cập thành viên `protected` của lớp cha thông qua kế thừa, không phải qua việc tạo một đối tượng lớp cha thông thường.*
* **Thế giới bên ngoài:** Chỉ có `public` mới truy cập được.

### 3.5. Ví dụ Minh họa Đa Package

Giả sử cấu trúc thư mục:

```
src/
├── com/
│   └── example/
│       ├── entity/
│       │   └── Product.java
│       └── service/
│           └── ProductService.java
└── main/
    └── MainApp.java
```

**com/example/entity/Product.java:**

```java
package com.example.entity;

public class Product {
    public String publicName;       // Truy cập mọi nơi
    protected String protectedCode; // Truy cập trong com.example.entity, com.example.service và lớp con (nếu có)
    String defaultDescription;      // Chỉ truy cập trong com.example.entity
    private double privatePrice;    // Chỉ truy cập trong lớp Product

    public Product(String name, String code, String desc, double price) {
        this.publicName = name;
        this.protectedCode = code;
        this.defaultDescription = desc;
        this.privatePrice = price;
    }

    public void displayInternal() {
        System.out.println("--- Internal Access ---");
        System.out.println("Name: " + this.publicName);
        System.out.println("Code: " + this.protectedCode);
        System.out.println("Desc: " + this.defaultDescription);
        System.out.println("Price: " + this.privatePrice); // OK - trong cùng lớp
        privateHelperMethod(); // OK
        System.out.println("-----------------------");
    }

    private void privateHelperMethod() {
        System.out.println("Private helper executed.");
    }

    protected void protectedMethod() {
        System.out.println("Protected method executed.");
    }

     void defaultMethod() {
         System.out.println("Default method executed.");
     }
}
```

**com/example/service/ProductService.java:**

```java
package com.example.service;

import com.example.entity.Product; // Cần import vì khác package

public class ProductService {

    public void processProduct(Product product) {
        System.out.println("--- Access from com.example.service ---");
        System.out.println("Name: " + product.publicName);       // OK - public
        // System.out.println("Code: " + product.protectedCode); // Lỗi! Khác package và không phải lớp con trực tiếp
        // System.out.println("Desc: " + product.defaultDescription); // Lỗi! Khác package
        // System.out.println("Price: " + product.privatePrice); // Lỗi! private

        product.displayInternal(); // OK - phương thức public
        // product.protectedMethod(); // Lỗi!
        // product.defaultMethod(); // Lỗi!
        // product.privateHelperMethod(); // Lỗi!
        System.out.println("--------------------------------------");
    }

    // Giả sử có lớp con trong cùng package dịch vụ
    static class SpecialProductService extends ProductService {
         public void processSpecialProduct(Product p) {
             // Vẫn không truy cập được protected/default/private của Product
             // vì ProductService không phải lớp con của Product
         }
    }
}

// Lớp con của Product trong package khác
class DiscountedProduct extends Product {
     public DiscountedProduct(String name, String code, String desc, double price) {
         super(name, code, desc, price);
     }

     public void applyDiscount() {
         System.out.println("--- Access from Subclass (com.example.service) ---");
         System.out.println("Name: " + this.publicName);       // OK - public
         System.out.println("Code: " + this.protectedCode);    // OK - protected từ lớp con khác package
         // System.out.println("Desc: " + this.defaultDescription); // Lỗi! Khác package
         // System.out.println("Price: " + this.privatePrice); // Lỗi! private

         this.displayInternal(); // OK - public
         this.protectedMethod(); // OK - protected từ lớp con khác package
         // this.defaultMethod(); // Lỗi!
         // this.privateHelperMethod(); // Lỗi!
         System.out.println("------------------------------------------------");
     }
}
```

**main/MainApp.java:**

```java
package main;

import com.example.entity.Product;
import com.example.service.ProductService;
// import com.example.service.DiscountedProduct; // Nếu DiscountedProduct là default thì không import được

public class MainApp {
    public static void main(String[] args) {
        Product p1 = new Product("Laptop", "LP123", "Gaming Laptop", 1200.0);
        ProductService service = new ProductService();

        System.out.println("--- Access from main (different package) ---");
        System.out.println("Name: " + p1.publicName); // OK - public
        // System.out.println("Code: " + p1.protectedCode); // Lỗi!
        // System.out.println("Desc: " + p1.defaultDescription); // Lỗi!
        // System.out.println("Price: " + p1.privatePrice); // Lỗi!

        p1.displayInternal(); // OK - public method
        // p1.protectedMethod(); // Lỗi!
        // p1.defaultMethod(); // Lỗi!
        // p1.privateHelperMethod(); // Lỗi!
        System.out.println("-----------------------------------------");

        service.processProduct(p1);

        // Nếu DiscountedProduct là public
        // DiscountedProduct dp = new DiscountedProduct("Mouse", "M456", "Wireless Mouse", 25.0);
        // dp.applyDiscount();
    }
}
```

### 3.6. Access Modifiers và Kế thừa

* Lớp con **không thể** truy cập thành viên `private` của lớp cha.
* Lớp con (kể cả ở package khác) **có thể** truy cập thành viên `protected` của lớp cha.
* Lớp con ở **cùng package** với lớp cha có thể truy cập thành viên `default` và `protected` của lớp cha.
* Khi ghi đè (override) một phương thức từ lớp cha, phương thức trong lớp con **không được phép** có mức truy cập **hạn chế hơn** mức truy cập của phương thức trong lớp cha (ví dụ: không thể ghi đè phương thức `public` thành `protected`). Có thể giữ nguyên hoặc mở rộng (`protected` thành `public`).

## 4. Từ khóa `static`: Thành viên của Lớp

Từ khóa `static` dùng để khai báo các thành viên (thuộc tính và phương thức) thuộc về **chính bản thân lớp đó**, chứ không thuộc về bất kỳ **đối tượng cụ thể (instance)** nào của lớp.

### 4.1. Phân biệt Thành viên Instance và Thành viên Lớp (`static`)

| Đặc điểm             | Thành viên Instance (Không `static`)                | Thành viên Lớp (`static`)                                                        |
| :------------------- | :-------------------------------------------------- | :------------------------------------------------------------------------------- |
| **Thuộc về**         | Từng đối tượng cụ thể (instance)                    | Bản thân lớp                                                                     |
| **Số lượng bản sao** | Mỗi đối tượng có bản sao riêng                      | Chỉ **một** bản sao duy nhất, chia sẻ bởi mọi đối tượng                          |
| **Cách truy cập**    | Qua biến tham chiếu đối tượng (`objectName.member`) | Qua tên lớp (`ClassName.staticMember`) (Hoặc qua đối tượng - không khuyến khích) |
| **Khởi tạo**         | Khi đối tượng được tạo (`new`)                      | Khi lớp được nạp (load) vào JVM lần đầu tiên                                     |
| **Liên kết `this`**  | Có tham chiếu `this` đến đối tượng hiện tại         | **Không** có tham chiếu `this`                                                   |
| **Ví dụ**            | `name`, `age` của `Person`; `deposit()`             | `Math.PI`, `Math.sqrt()`, `main()` method                                        |

### 4.2. Thuộc tính Tĩnh (`static` field / Class Variable)

Một biến được chia sẻ bởi tất cả các instance của lớp.

* **Cú pháp:** `[modifier] static dataType variableName [= initialValue];`
* **Đặc điểm:**
    * Chỉ có một giá trị duy nhất tồn tại trong bộ nhớ cho dù có bao nhiêu đối tượng được tạo ra.
    * Thay đổi giá trị của biến `static` qua một đối tượng hoặc qua tên lớp sẽ ảnh hưởng đến tất cả các đối tượng khác.
* **Trường hợp sử dụng:**
    * **Hằng số (Constants):** Thường kết hợp với `final` (`public static final`). Ví dụ: `Math.PI`, `Integer.MAX_VALUE`. Đặt tên theo quy ước `ALL_CAPS_WITH_UNDERSCORES`.
    * **Bộ đếm (Counters):** Đếm số lượng đối tượng đã được tạo.
    * **Cấu hình chung:** Lưu trữ giá trị cấu hình áp dụng cho toàn bộ lớp.
* **Cách truy cập:** `ClassName.staticFieldName` (khuyến khích) hoặc `objectName.staticFieldName` (không khuyến khích vì gây nhầm lẫn).

```java
public class AppConfig {
    public static final String DEFAULT_LANGUAGE = "en"; // Hằng số
    private static int connectionTimeout = 5000; // Cấu hình chung (private static)
    private static int activeInstances = 0;      // Bộ đếm

    public AppConfig() {
        activeInstances++;
    }

    public static int getConnectionTimeout() { // Getter tĩnh
        return connectionTimeout;
    }

    public static void setConnectionTimeout(int timeout) { // Setter tĩnh
        if (timeout > 0) {
            connectionTimeout = timeout;
        }
    }

    public static int getActiveInstances() { // Getter tĩnh cho bộ đếm
        return activeInstances;
    }
}

// Sử dụng
System.out.println("Default Lang: " + AppConfig.DEFAULT_LANGUAGE);
AppConfig.setConnectionTimeout(10000);
System.out.println("Timeout: " + AppConfig.getConnectionTimeout());

AppConfig config1 = new AppConfig();
AppConfig config2 = new AppConfig();
System.out.println("Instances: " + AppConfig.getActiveInstances()); // Output: Instances: 2
```

### 4.3. Phương thức Tĩnh (`static` method / Class Method)

Một phương thức thuộc về lớp, có thể được gọi mà không cần tạo đối tượng.

* **Cú pháp:** `[modifier] static returnType methodName([parameterList]) { ... }`
* **Đặc điểm & Ràng buộc:**
    * Được gọi trực tiếp bằng tên lớp: `ClassName.staticMethodName()`.
    * **Không thể** sử dụng từ khóa `this` hoặc `super`.
    * **Không thể** truy cập trực tiếp các thành viên *instance* (thuộc tính hoặc phương thức không `static`) của lớp. Lý do: phương thức `static` không gắn với đối tượng cụ thể nào, nên không biết `this` là ai để truy cập thành viên instance.
    * **Có thể** truy cập các thành viên `static` khác (thuộc tính hoặc phương thức) của lớp.
    * **Có thể** tạo đối tượng mới của lớp bên trong phương thức `static` và gọi phương thức instance trên đối tượng đó.
* **Trường hợp sử dụng:**
    * **Phương thức tiện ích (Utility Methods):** Các hàm thực hiện các thao tác chung, không phụ thuộc vào trạng thái của đối tượng cụ thể (ví dụ: các phương thức trong lớp `Math`, `Arrays`, `Collections`).
    * **Phương thức Factory (Factory Methods):** Một cách thay thế constructor để tạo đối tượng. Cung cấp sự linh hoạt hơn (có thể trả về subclass, có thể không cần tạo đối tượng mới mỗi lần gọi, có thể đặt tên rõ ràng hơn constructor).
    * **Phương thức `main`:** Điểm khởi đầu của mọi ứng dụng Java là một phương thức `static`.

```java
import java.util.Arrays;
import java.util.List;

public class StringUtils {
    // Phương thức tiện ích tĩnh
    public static boolean isNullOrEmpty(String str) {
        return str == null || str.trim().isEmpty();
    }

    // Phương thức factory tĩnh (ví dụ đơn giản)
    public static List<String> createListFromCommaString(String commaSeparated) {
        if (isNullOrEmpty(commaSeparated)) {
            return Collections.emptyList(); // Có thể trả về instance đặc biệt
        }
        String[] items = commaSeparated.split(",");
        return Arrays.asList(items); // Trả về một List
    }
}

// Sử dụng:
String name = "";
if (StringUtils.isNullOrEmpty(name)) {
    System.out.println("Name is empty.");
}

List<String> items = StringUtils.createListFromCommaString("apple,banana,orange");
System.out.println(items);
```

### 4.4. Khối Khởi tạo Tĩnh (`static` Initializer Block)

Một khối mã được đánh dấu `static` dùng để thực hiện các thao tác khởi tạo phức tạp cho các thuộc tính `static` khi lớp được nạp lần đầu tiên.

* **Cú pháp:** `static { ... }`
* **Thứ tự thực thi:**
    1.  Các khối khởi tạo `static` và việc khởi tạo các biến `static` được thực thi theo thứ tự xuất hiện trong mã nguồn.
    2.  Việc này xảy ra **chỉ một lần** duy nhất khi lớp được ClassLoader nạp vào JVM, thường là trước khi đối tượng đầu tiên của lớp được tạo hoặc trước khi một thành viên `static` được truy cập lần đầu.

```java
import java.util.Properties;
import java.io.InputStream;

public class AppSettings {
    private static Properties settings = new Properties();
    private static boolean initializationFailed = false;

    // Khối khởi tạo tĩnh để đọc file cấu hình
    static {
        System.out.println("Static initializer block running...");
        try (InputStream input = AppSettings.class.getClassLoader().getResourceAsStream("config.properties")) {
            if (input == null) {
                System.err.println("Sorry, unable to find config.properties");
                initializationFailed = true;
            } else {
                settings.load(input);
                System.out.println("Configuration loaded successfully.");
            }
        } catch (Exception ex) {
            System.err.println("Error loading configuration: " + ex.getMessage());
            initializationFailed = true;
        }
    }

    public static String getSetting(String key) {
        if (initializationFailed) {
            return null; // Hoặc trả về giá trị mặc định/ném lỗi
        }
        return settings.getProperty(key);
    }

     public static boolean didInitFail() {
         return initializationFailed;
     }
}

// Sử dụng (Giả sử có file config.properties trong classpath):
// System.out.println("Is init failed? " + AppSettings.didInitFail()); // Sẽ trigger static block nếu chưa chạy
// String dbUrl = AppSettings.getSetting("database.url");
// System.out.println("DB URL: " + dbUrl);
```

## 5. Tổ chức Mã nguồn với Packages và Import

Packages là cơ chế cơ bản của Java để nhóm các lớp và interface có liên quan lại với nhau, giúp quản lý mã nguồn hiệu quả.

### 5.1. Packages

* **Mục đích:**
    * **Tổ chức logic:** Nhóm các lớp có cùng chức năng hoặc liên quan (ví dụ: `java.util`, `java.net`, `com.mycompany.model`, `com.mycompany.controller`).
    * **Tránh xung đột tên (Naming Conflicts):** Hai lớp có thể có cùng tên miễn là chúng nằm trong các package khác nhau (ví dụ: `java.util.Date` và `java.sql.Date`). Tên đầy đủ (Fully Qualified Name - FQN) của lớp bao gồm cả tên package (ví dụ: `com.example.entity.Product`).
    * **Kiểm soát truy cập:** Kết hợp với access modifier `default` (package-private) để hạn chế truy cập chỉ trong nội bộ package.
* **Quy ước đặt tên:**
    * Sử dụng chữ thường.
    * Thường sử dụng tên miền internet đảo ngược của tổ chức để đảm bảo tính duy nhất toàn cầu (ví dụ: `com.google.common`, `org.apache.commons`).
    * Các cấp con được phân tách bằng dấu chấm (`.`).
* **Cách khai báo:**
    * Câu lệnh `package packageName;` phải là **dòng mã đầu tiên** (không tính comment) trong file `.java`.
    * Một file chỉ có thể thuộc về **một package**. Nếu không có khai báo `package`, lớp sẽ thuộc về *default package* (không khuyến khích sử dụng cho các dự án lớn).
* **Ánh xạ thư mục:** Cấu trúc thư mục trong hệ thống tệp **phải khớp chính xác** với cấu trúc package.
    * Ví dụ: Lớp trong package `com.example.util` phải nằm trong thư mục `.../com/example/util/`.

### 5.2. Import

Từ khóa `import` cho phép bạn sử dụng các lớp hoặc interface từ các package khác mà không cần phải gõ tên đầy đủ (FQN) mỗi lần.

* **Cách khai báo:** Đặt các câu lệnh `import` **sau** câu lệnh `package` và **trước** khai báo lớp.
* **Các loại import:**
    1.  **Import lớp/interface cụ thể (Single Type Import):**
        ```java
        import java.util.ArrayList; // Chỉ import lớp ArrayList
        import java.io.File;

        public class MyClass {
            ArrayList list; // OK
            File file;     // OK
        }
        ```
        Đây là cách **khuyến khích** vì nó rõ ràng nhất về các lớp đang được sử dụng.
    2.  **Import theo yêu cầu / Wildcard (Type-Import-on-Demand):**
        ```java
        import java.util.*; // Import tất cả các lớp và interface public trong package java.util

        public class MyClass {
            ArrayList list; // OK
            HashMap map;    // OK
            Date date;     // OK (nếu chỉ có java.util.Date được import)
        }
        ```
        * *Lưu ý:* Không import các lớp trong các package con (subpackages). `import java.util.*` không import `java.util.regex.*`.
        * *Cân nhắc:* Có thể làm mã khó đọc hơn một chút vì không rõ ngay lập tức lớp nào đang được dùng. Một số tranh cãi về ảnh hưởng nhỏ đến hiệu năng *biên dịch* (không phải runtime). IDE hiện đại thường quản lý import cụ thể tốt hơn.
    3.  **Import thành viên tĩnh (Static Import):** Cho phép truy cập trực tiếp các thành viên `static` (thuộc tính và phương thức) của một lớp mà không cần tên lớp đứng trước.
        ```java
        import static java.lang.Math.PI;
        import static java.lang.Math.sqrt;
        import static java.util.Arrays.asList;

        public class MyCalc {
            public double circleArea(double radius) {
                return PI * radius * radius; // Dùng PI trực tiếp thay vì Math.PI
            }

            public double calculateHypotenuse(double a, double b) {
                return sqrt(a*a + b*b); // Dùng sqrt trực tiếp thay vì Math.sqrt
            }

             public void showList() {
                 List<String> names = asList("A", "B", "C"); // Dùng asList thay vì Arrays.asList
                 System.out.println(names);
             }
        }
        ```
        * *Cân nhắc:* Sử dụng cẩn thận vì có thể làm giảm tính rõ ràng của mã nguồn nếu lạm dụng (khó biết `PI` hay `sqrt` đến từ đâu nếu không xem import). Thường hữu ích cho các hằng số hoặc phương thức tiện ích được dùng rất thường xuyên.
        * Có thể import tất cả thành viên tĩnh: `import static java.lang.Math.*;`.

### 5.3. Xung đột tên và Fully Qualified Name (FQN)

Nếu bạn import hai lớp có cùng tên từ hai package khác nhau (ví dụ: `import java.util.Date;` và `import java.sql.Date;`), trình biên dịch sẽ báo lỗi mơ hồ (ambiguous). Trong trường hợp này, bạn phải sử dụng tên đầy đủ (FQN) cho ít nhất một trong số chúng:

```java
import java.util.Date; // Import java.util.Date

public class DateTest {
    public static void main(String[] args) {
        Date utilDate = new Date(); // Sử dụng tên ngắn cho java.util.Date
        java.sql.Date sqlDate = new java.sql.Date(utilDate.getTime()); // Phải dùng FQN cho java.sql.Date

        System.out.println("Util Date: " + utilDate);
        System.out.println("SQL Date: " + sqlDate);
    }
}
```

## 6. Getter, Setter và Tính Đóng gói (Ôn tập & Nâng cao)

Như đã đề cập trong phần Access Modifiers, đóng gói là nguyên tắc cốt lõi, và getters/setters là công cụ phổ biến để thực hiện nó.

* **Recap:** Thuộc tính nên là `private`. Cung cấp phương thức `public` getter (`getProperty()`, `isProperty()`) để đọc và `public` setter (`setProperty(value)`) để ghi (nếu cần).
* **Validation trong Setters:** Setter là nơi lý tưởng để kiểm tra tính hợp lệ của dữ liệu trước khi gán cho thuộc tính.

    ```java
    public class User {
        private int age;

        public int getAge() { return age; }

        public void setAge(int age) {
            if (age > 0 && age < 120) { // Validation
                this.age = age;
            } else {
                throw new IllegalArgumentException("Tuổi không hợp lệ: " + age);
            }
        }
    }
    ```
* **Read-Only / Write-Only Fields:**
    * **Read-Only:** Chỉ cung cấp getter, không cung cấp setter. Giá trị thường được thiết lập trong constructor và không đổi sau đó (tiến gần đến tính bất biến).
    * **Write-Only:** Chỉ cung cấp setter, không cung cấp getter. Ít phổ biến, có thể dùng cho các thuộc tính như mật khẩu (chỉ cho phép set, không cho phép đọc lại).
* **Getters/Setters và Tính Bất biến (Immutability):**
    * Một đối tượng bất biến là đối tượng mà trạng thái của nó không thể thay đổi sau khi được tạo.
    * Nếu thuộc tính là kiểu tham chiếu (ví dụ: `List`, `Date`, object khác), việc chỉ trả về tham chiếu gốc trong getter có thể vô tình cho phép bên ngoài thay đổi trạng thái nội tại của đối tượng đó, phá vỡ tính bất biến.
    * **Cách xử lý:**
        * Trong getter, trả về một bản sao (copy) của đối tượng tham chiếu (defensive copying).
        * Sử dụng các collection không thể thay đổi (unmodifiable collections) từ `java.util.Collections` hoặc các thư viện như Guava.

    ```java
    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.List;
    import java.util.Date;

    public final class ImmutableData { // Lớp final để không bị kế thừa
        private final String name;     // final primitive
        private final List<String> items; // final reference (nhưng List có thể thay đổi)
        private final Date creationDate; // final reference (Date có thể thay đổi)

        public ImmutableData(String name, List<String> items, Date date) {
            this.name = name;
            // Tạo bản sao phòng thủ cho List
            this.items = new ArrayList<>(items);
            // Tạo bản sao phòng thủ cho Date
            this.creationDate = new Date(date.getTime());
        }

        public String getName() { return name; }

        // Getter cho List: trả về bản sao không thể thay đổi
        public List<String> getItems() {
            // return new ArrayList<>(items); // Cách 1: Trả về bản sao mới
            return Collections.unmodifiableList(items); // Cách 2: Trả về view không sửa được
        }

        // Getter cho Date: trả về bản sao
        public Date getCreationDate() {
            return new Date(creationDate.getTime()); // Luôn trả về bản sao
        }
    }
    ```

## 7. Lớp Lồng Nhau (Nested Classes)

Java cho phép bạn định nghĩa một lớp bên trong một lớp khác. Có 4 loại lớp lồng nhau:

### 7.1. Giới thiệu và Mục đích

* **Nhóm các lớp liên quan logic:** Khi một lớp chỉ được sử dụng bởi một lớp khác, việc lồng nó vào giúp mã nguồn gọn gàng hơn.
* **Tăng tính đóng gói:** Lớp lồng nhau có thể được khai báo là `private`, `protected`, `default` hoặc `public`, kiểm soát khả năng truy cập của nó. Lớp lồng nhau có quyền truy cập đặc biệt vào các thành viên (kể cả `private`) của lớp bao ngoài (tùy loại).
* **Cải thiện khả năng đọc và bảo trì:** Đặt các lớp helper nhỏ gần nơi chúng được sử dụng.

### 7.2. Lớp Lồng Nhau Tĩnh (`static` Nested Class)

* **Đặc điểm:**
    * Được khai báo với từ khóa `static` bên trong lớp ngoài.
    * Giống như một lớp top-level bình thường được đặt tên bên trong lớp khác.
    * **Không** có tham chiếu ngầm đến bất kỳ đối tượng nào của lớp bao ngoài.
    * **Không thể** truy cập trực tiếp các thành viên *instance* (không `static`) của lớp bao ngoài.
    * **Có thể** truy cập các thành viên `static` (kể cả `private`) của lớp bao ngoài.
* **Cách sử dụng & Khởi tạo:**
    ```java
    public class Outer {
        private static String outerStaticField = "Outer Static";
        private String outerInstanceField = "Outer Instance";

        static class StaticNested {
             private static int nestedStaticCounter = 0;
             private int nestedInstanceVar;

             public StaticNested(int val) { this.nestedInstanceVar = val;}

            void display() {
                System.out.println("Outer Static Field: " + outerStaticField); // OK
                // System.out.println("Outer Instance Field: " + outerInstanceField); // Lỗi!
                System.out.println("Nested Static Counter: " + nestedStaticCounter);
                System.out.println("Nested Instance Var: " + nestedInstanceVar);
            }
        }
    }

    // Khởi tạo từ bên ngoài:
    Outer.StaticNested nestedObj = new Outer.StaticNested(10);
    nestedObj.display();
    ```
* **Trường hợp sử dụng:** Khi lớp lồng nhau không cần truy cập trạng thái của đối tượng bao ngoài. Thường dùng cho các lớp tiện ích, builder, hoặc các implementation private static. **Nên ưu tiên dùng static nested class hơn inner class nếu có thể.**

### 7.3. Lớp Nội (Inner Class - non-static)

* **Đặc điểm:**
    * Không có từ khóa `static`.
    * Mỗi đối tượng inner class **luôn liên kết** với một đối tượng (instance) của lớp bao ngoài.
    * **Có thể** truy cập tất cả các thành viên (kể cả `private`, `static` và non-static) của đối tượng lớp bao ngoài.
    * **Không thể** khai báo bất kỳ thành viên `static` nào bên trong inner class (trừ các hằng số `static final`).
* **Cách sử dụng & Khởi tạo:** Phải có một đối tượng của lớp ngoài để tạo đối tượng inner class.
    ```java
    public class OuterWithInner {
        private static String outerStaticField = "Outer Static";
        private String outerInstanceField = "Outer Instance Value";

        class Inner {
            private String innerField = "Inner Value";

            void display() {
                System.out.println("--- Inner Display ---");
                System.out.println("Outer Static: " + outerStaticField);         // OK
                System.out.println("Outer Instance: " + outerInstanceField);     // OK - Truy cập trực tiếp
                System.out.println("Outer Instance (this): " + OuterWithInner.this.outerInstanceField); // Cách truy cập tường minh `this` của lớp ngoài
                System.out.println("Inner Field: " + this.innerField);
                System.out.println("---------------------");
            }
        }

        public void createAndShowInner() {
             Inner innerObj = new Inner(); // Tạo từ bên trong phương thức instance của Outer
             innerObj.display();
        }
    }

    // Khởi tạo từ bên ngoài:
    OuterWithInner outerObj = new OuterWithInner();
    OuterWithInner.Inner innerObjFromOutside = outerObj.new Inner(); // Cú pháp đặc biệt
    innerObjFromOutside.display();

    outerObj.createAndShowInner(); // Cách khác
    ```
* **Shadowing:** Nếu inner class khai báo biến trùng tên với lớp ngoài, biến của inner class sẽ che (shadow) biến của lớp ngoài. Dùng `OuterClassName.this.variableName` để truy cập biến bị che.
* **Trường hợp sử dụng:** Khi lớp lồng nhau cần truy cập chặt chẽ vào trạng thái của đối tượng lớp ngoài. Ví dụ phổ biến là implement các event listener trong GUI, iterator, các lớp helper phụ thuộc vào trạng thái đối tượng chính.

### 7.4. Lớp Địa phương (Local Class)

* **Đặc điểm:**
    * Được khai báo bên trong một khối mã (phương thức, constructor, hoặc khối `{}`).
    * Phạm vi truy cập bị giới hạn trong khối đó. Không thể sử dụng access modifier (`public`, `private`, etc.) vì phạm vi đã được xác định bởi khối chứa nó.
    * Có thể truy cập thành viên của lớp bao ngoài (giống inner class).
    * **Chỉ có thể truy cập các biến cục bộ (local variables) của khối chứa nó nếu các biến đó là `final` hoặc *effectively final***. Effectively final nghĩa là biến đó không bị thay đổi giá trị sau khi được khởi tạo.
* **Cách sử dụng:**
    ```java
    public class DataProcessor {
        public void processData(final String category) { // category là final
            int counter = 0; // counter là effectively final nếu không bị sửa đổi sau dòng này
            final int limit = 100; // limit là final

            // Biến này không effectively final
            // String status = "Processing";
            // status = "Checking"; // Nếu có dòng này, status không effectively final

            class LocalHelper {
                void performAction(String data) {
                    System.out.println("Processing '" + data + "' in category: " + category); // OK
                    System.out.println("Counter value: " + counter); // OK (vì counter effectively final)
                    System.out.println("Limit: " + limit); // OK
                    // System.out.println(status); // Lỗi nếu status không final/effectively final
                }
            }

            LocalHelper helper = new LocalHelper();
            helper.performAction("Sample Data 1");
            helper.performAction("Sample Data 2");

            // counter++; // Nếu có dòng này sau khi LocalHelper được định nghĩa, counter sẽ không còn effectively final -> lỗi biên dịch trong LocalHelper
        }
    }
    ```
* **Trường hợp sử dụng:** Ít phổ biến hơn hai loại trên. Dùng khi bạn cần một lớp chỉ cho mục đích sử dụng một lần, rất cục bộ trong một phương thức và cần truy cập cả biến cục bộ của phương thức đó.

### 7.5. Lớp Vô danh (Anonymous Class)

* **Đặc điểm:**
    * Là một lớp **không có tên**, được khai báo và khởi tạo **đồng thời** tại một điểm duy nhất.
    * Thường được dùng để tạo nhanh một đối tượng implement một interface hoặc extend một lớp (thường là lớp trừu tượng) với một số lượng nhỏ phương thức cần ghi đè.
    * Giống như local class, nó có thể truy cập thành viên của lớp bao ngoài và các biến local `final` hoặc effectively final.
* **Cú pháp:**
    ```java
    new InterfaceOrSuperclassName() {
        // Khai báo thuộc tính (nếu cần)
        // Ghi đè phương thức của interface/superclass
        // Định nghĩa phương thức mới (chỉ gọi được bên trong anonymous class)
    }; // Kết thúc bằng dấu chấm phẩy
    ```
* **Trường hợp sử dụng:**
    * **Event Listeners (GUI):** Rất phổ biến trong Swing, AWT (phiên bản Java cũ hơn).
    * **Tạo đối tượng `Runnable`, `Callable`:** Để thực thi trong luồng mới.
    * **Tạo đối tượng `Comparator`, `Predicate`, etc.:** Để dùng với Collections hoặc Streams.
* **Ví dụ:**
    ```java
    import java.util.Arrays;
    import java.util.Comparator;

    public class AnonymousDemo {
        public static void main(String[] args) {
            String[] names = {"Charlie", "Alice", "Bob"};

            // Sử dụng Anonymous Class để tạo Comparator sắp xếp theo độ dài chuỗi
            Arrays.sort(names, new Comparator<String>() {
                @Override
                public int compare(String s1, String s2) {
                    return Integer.compare(s1.length(), s2.length());
                }
            });

            System.out.println("Sorted by length: " + Arrays.toString(names));

            // Sử dụng Anonymous Class để tạo Runnable
            Runnable myTask = new Runnable() {
                private int count = 0; // Có thể có thuộc tính

                @Override
                public void run() {
                    count++;
                    System.out.println("Task executed! Count: " + count + " on thread " + Thread.currentThread().getName());
                }
            };

            Thread t1 = new Thread(myTask);
            Thread t2 = new Thread(myTask); // Mỗi thread có đối tượng Runnable riêng (dù mã nguồn giống nhau)
            t1.start();
            t2.start();
        }
    }
    ```
* **So sánh với Lambda Expressions (Java 8+):** Đối với các interface chỉ có **một phương thức trừu tượng duy nhất** (functional interfaces như `Runnable`, `Comparator`, `ActionListener`), Lambda Expressions cung cấp cú pháp ngắn gọn và rõ ràng hơn nhiều so với anonymous classes. Anonymous classes vẫn cần thiết khi bạn cần implement interface có nhiều phương thức hoặc extend một class, hoặc khi cần duy trì trạng thái riêng (có thuộc tính instance).

    ```java
    // Comparator dùng Lambda
    Arrays.sort(names, (s1, s2) -> Integer.compare(s1.length(), s2.length()));

    // Runnable dùng Lambda
    Runnable myLambdaTask = () -> {
        // Không thể có thuộc tính instance riêng biệt như anonymous class ở trên
        System.out.println("Lambda Task executed on thread " + Thread.currentThread().getName());
    };
    new Thread(myLambdaTask).start();
    ```

## 8. Thực hành Tốt nhất và Troubleshooting

### 8.1. Best Practices

* **Nguyên tắc Quyền Tối thiểu (Principle of Least Privilege):** Luôn bắt đầu với mức truy cập hạn chế nhất (`private`) và chỉ mở rộng khi thực sự cần thiết (`default` -> `protected` -> `public`).
* **Đóng gói là Mặc định:** Thuộc tính (fields) nên luôn là `private` trừ khi có lý do rất đặc biệt. Sử dụng getters/setters để kiểm soát truy cập.
* **Tổ chức bằng Packages:** Sử dụng cấu trúc package hợp lý, theo tên miền đảo ngược, để tránh xung đột và phân loại mã nguồn rõ ràng.
* **Ưu tiên `static nested class`:** Nếu lớp lồng nhau không cần truy cập thành viên instance của lớp ngoài, hãy làm cho nó `static`. Nó đơn giản hơn và không giữ tham chiếu không cần thiết đến đối tượng lớp ngoài.
* **Hằng số dùng `public static final`:** Đây là quy ước chuẩn cho hằng số trong Java.
* **Cân nhắc `static factory method`:** Đôi khi chúng tốt hơn constructor `public` vì có thể đặt tên rõ ràng, trả về subtype, hoặc không nhất thiết phải tạo đối tượng mới (ví dụ: trả về đối tượng đã cache).
* **Tránh `import *` nếu có thể:** Import cụ thể làm mã rõ ràng hơn, mặc dù IDE hiện đại giúp quản lý việc này.
* **Cẩn thận với `protected`:** Chỉ dùng khi bạn thiết kế cho kế thừa và hiểu rõ hệ quả. Lạm dụng `protected` có thể làm lộ chi tiết triển khai không mong muốn.
* **Sử dụng Lambda thay Anonymous Class:** Khi làm việc với functional interfaces để mã ngắn gọn hơn.

### 8.2. Troubleshooting / Common Pitfalls

* **Lỗi truy cập (Compiler Error):** "Cannot access member ...", "... has private access in ...", "... is not public in ...; cannot be accessed from outside package". -> Nguyên nhân: Vi phạm quy tắc của Access Modifier. -> *Giải pháp:* Kiểm tra lại modifier của thành viên và vị trí bạn đang cố truy cập nó, điều chỉnh modifier hoặc cách truy cập (ví dụ: dùng getter thay vì truy cập field trực tiếp).
* **`NullPointerException` với thành viên `static`?:** Mặc dù phương thức `static` thuộc về lớp, nếu bạn gọi nó thông qua một biến tham chiếu đang là `null` (`nullObject.staticMethod()`), bạn **vẫn** gặp `NullPointerException` tại điểm gọi, không phải vì bản thân phương thức `static` cần `this`. -> *Giải pháp:* Luôn gọi phương thức `static` qua tên lớp (`ClassName.staticMethod()`).
* **Nhầm lẫn `static` và `this`:** Cố gắng truy cập thành viên instance (dùng `this` ngầm định hoặc tường minh) từ bên trong phương thức `static`. -> *Giải pháp:* Nhớ rằng `static` không có `this`. Nếu cần dữ liệu của đối tượng, hãy truyền đối tượng đó vào làm tham số cho phương thức `static`, hoặc làm cho phương thức đó thành instance method nếu nó thực sự thuộc về hành vi của đối tượng.
* **Lỗi biên dịch khi Local/Anonymous class truy cập biến non-final:** "... local variables referenced from a ... inner class must be final or effectively final". -> *Giải pháp:* Đảm bảo biến cục bộ không bị gán lại giá trị sau khi nó được khai báo và trước khi lớp lồng nhau sử dụng nó, hoặc khai báo biến đó là `final`.
* **Khó khăn khi khởi tạo Inner Class:** Quên cú pháp `outerObject.new InnerClass()`. -> *Giải pháp:* Nhớ rằng inner class cần một instance của outer class để tồn tại.
* **Cấu trúc Package không khớp thư mục:** Lỗi "Could not find or load main class ..." hoặc lỗi biên dịch liên quan đến package. -> *Giải pháp:* Đảm bảo đường dẫn thư mục từ `src` hoặc `classpath root` khớp chính xác với khai báo `package` trong file `.java`.

## 9. Kết luận

Việc kiểm soát truy cập và tổ chức mã nguồn là những kỹ năng nền tảng để viết mã Java chuyên nghiệp. Access Modifiers (`public`, `protected`, `default`, `private`) cho phép bạn thực thi tính đóng gói, bảo vệ dữ liệu và định nghĩa các API rõ ràng. Từ khóa `static` giúp phân biệt giữa những gì thuộc về lớp và những gì thuộc về đối tượng, rất quan trọng cho việc tạo hằng số, phương thức tiện ích và quản lý trạng thái chung. Packages cung cấp cơ chế thiết yếu để tổ chức các dự án lớn, tránh xung đột tên và kiểm soát phạm vi hiển thị. Cuối cùng, Nested Classes mang lại sự linh hoạt trong việc cấu trúc các lớp liên quan chặt chẽ, tăng cường tính đóng gói và khả năng đọc mã.