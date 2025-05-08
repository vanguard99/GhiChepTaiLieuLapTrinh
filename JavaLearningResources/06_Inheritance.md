# KẾ THỪA

Hướng dẫn này cung cấp kiến thức toàn diện về cơ chế **kế thừa (Inheritance)** trong Java, một trong những trụ cột của lập trình hướng đối tượng (Object-Oriented Programming - OOP). Chúng ta sẽ khám phá cách kế thừa hoạt động, cách triển khai, các khái niệm liên quan như ghi đè phương thức, đa hình, và các phương pháp thực hành tốt nhất.

## 1. Giới thiệu về Kế thừa

**Kế thừa** là một cơ chế nền tảng trong OOP cho phép một lớp mới (gọi là **lớp con** - `subclass` hoặc `child class`) tự động sở hữu (thừa hưởng) các thuộc tính (fields) và phương thức (methods) đã được định nghĩa trong một lớp khác (gọi là **lớp cha** - `superclass` hoặc `parent class`).

**Mục đích chính của kế thừa:**

* **Tái sử dụng mã (Code Reusability):** Tránh việc phải viết lại mã giống nhau ở nhiều lớp. Các đặc điểm chung được định nghĩa một lần ở lớp cha và được các lớp con sử dụng lại.
* **Tạo cấu trúc phân cấp (Hierarchical Structure):** Phản ánh mối quan hệ tự nhiên giữa các đối tượng trong thế giới thực hoặc trong miền vấn đề (problem domain), tạo ra một cấu trúc lớp có tổ chức và dễ quản lý.
* **Mở rộng chức năng (Extensibility):** Lớp con có thể thêm các thuộc tính và phương thức mới hoặc thay đổi (ghi đè) hành vi của phương thức kế thừa từ lớp cha để phù hợp với nhu cầu cụ thể của nó.

## 2. Mối quan hệ "IS-A" (Là một)

Kế thừa biểu diễn một mối quan hệ **"IS-A"** giữa lớp con và lớp cha. Điều này có nghĩa là một đối tượng của lớp con *cũng là* một đối tượng của lớp cha.

* Ví dụ:
    * Một `Dog` **IS-A** `Animal`.
    * Một `Car` **IS-A** `Vehicle`.
    * Một `Rectangle` **IS-A** `GeometricObject`.

Mối quan hệ này là cơ sở cho nhiều khái niệm quan trọng khác như **đa hình (Polymorphism)**.

```java
// Ví dụ minh họa quan hệ IS-A
class Animal {
    void eat() {
        System.out.println("This animal eats food.");
    }
}

class Dog extends Animal { // Dog IS-A Animal
    void bark() {
        System.out.println("The dog barks.");
    }
}

public class TestInheritance {
    public static void main(String[] args) {
        Dog myDog = new Dog();
        myDog.eat();  // Gọi phương thức kế thừa từ Animal
        myDog.bark(); // Gọi phương thức riêng của Dog
    }
}
```

## 3. Triển khai Kế thừa trong Java

Java sử dụng từ khóa `extends` để thiết lập mối quan hệ kế thừa giữa hai lớp.

**Cú pháp:**

```java
access_modifier class SubClassName extends SuperClassName {
    // Thân lớp con:
    // - Các thuộc tính và phương thức mới
    // - Các phương thức ghi đè (overridden methods)
}
```

* `SubClassName`: Tên của lớp con (lớp kế thừa).
* `SuperClassName`: Tên của lớp cha (lớp được kế thừa).

**Ví dụ cơ bản:**

```java
// Lớp cha
class Vehicle {
    String brand;
    int year;

    void displayInfo() {
        System.out.println("Brand: " + brand + ", Year: " + year);
    }
}

// Lớp con kế thừa từ Vehicle
class Car extends Vehicle {
    int numberOfDoors;

    void honk() {
        System.out.println("Beep beep!");
    }
}

public class Main {
    public static void main(String[] args) {
        Car myCar = new Car();
        myCar.brand = "Toyota"; // Thuộc tính kế thừa từ Vehicle
        myCar.year = 2023;      // Thuộc tính kế thừa từ Vehicle
        myCar.numberOfDoors = 4;// Thuộc tính riêng của Car

        myCar.displayInfo();    // Phương thức kế thừa từ Vehicle
        myCar.honk();           // Phương thức riêng của Car
    }
}
```

## 4. Các Thành phần được Kế thừa (và không được kế thừa)

Khi một lớp con kế thừa từ lớp cha, nó sẽ nhận được:

* **Thuộc tính và Phương thức `public`:** Truy cập được từ bất cứ đâu.
* **Thuộc tính và Phương thức `protected`:** Truy cập được bên trong lớp con, các lớp cùng package, và các lớp con khác (ngay cả khi ở package khác). Đây là mức truy cập thường dùng để cho phép lớp con tương tác với thành viên của lớp cha mà không cần công khai chúng hoàn toàn.
* **Thuộc tính và Phương thức với mức truy cập mặc định (package-private):** Chỉ truy cập được nếu lớp con nằm cùng `package` với lớp cha.

**Những gì KHÔNG được kế thừa:**

* **Thuộc tính và Phương thức `private`:** Các thành viên `private` của lớp cha chỉ có thể được truy cập bên trong chính lớp cha đó. Lớp con không thể truy cập trực tiếp chúng. *Tuy nhiên, lớp con vẫn có thể tương tác gián tiếp với các thành viên `private` thông qua các phương thức `public` hoặc `protected` (ví dụ: getters/setters) mà lớp cha cung cấp.*
* **Constructors (Hàm khởi tạo):** Lớp con không kế thừa constructors của lớp cha. Tuy nhiên, lớp con *phải* gọi một constructor của lớp cha (ngầm định hoặc tường minh) trong constructor của chính nó bằng từ khóa `super()`. Nếu không gọi tường minh, trình biên dịch Java sẽ tự động thêm một lời gọi `super()` (không tham số) vào đầu constructor của lớp con. Nếu lớp cha không có constructor không tham số, bạn *bắt buộc* phải gọi `super()` với tham số phù hợp một cách tường minh.

## 5. Ghi đè Phương thức (Method Overriding)

**Method Overriding** là một tính năng mạnh mẽ của kế thừa, cho phép lớp con cung cấp một **triển khai cụ thể** (specific implementation) cho một phương thức đã được định nghĩa ở lớp cha.

**Mục đích:**

* Thay đổi hoặc mở rộng hành vi của phương thức kế thừa để phù hợp với đặc thù của lớp con.

**Quy tắc ghi đè:**

1.  **Tên phương thức:** Phải giống hệt tên phương thức ở lớp cha.
2.  **Tham số:** Danh sách, kiểu dữ liệu, và thứ tự các tham số phải giống hệt (signature).
3.  **Kiểu trả về:** Phải giống hệt hoặc là một *kiểu con* (subtype) của kiểu trả về ở lớp cha (còn gọi là *covariant return type*).
4.  **Access Modifier:** Không được yếu hơn access modifier của phương thức ở lớp cha (ví dụ: nếu ở cha là `protected`, ở con có thể là `protected` hoặc `public`, nhưng không thể là `private` hoặc default).
5.  **Exceptions:** Phương thức ghi đè không được `throw` các checked exceptions mới hoặc rộng hơn những exceptions được `throw` bởi phương thức ở lớp cha. Nó có thể `throw` ít hơn hoặc hẹp hơn, hoặc không `throw` gì cả.
6.  **`static` và `final`:** Phương thức `static` không thể bị ghi đè (chỉ có thể bị *che giấu* - method hiding). Phương thức `final` không thể bị ghi đè.

**Annotation `@Override`:**

Nên sử dụng annotation `@Override` ngay phía trên phương thức ghi đè trong lớp con.

* **Mục đích:** Giúp trình biên dịch kiểm tra xem bạn có thực sự đang ghi đè một phương thức từ lớp cha hay không (đúng tên, đúng signature). Nếu không, trình biên dịch sẽ báo lỗi. Điều này giúp tránh các lỗi tinh vi do gõ sai tên hoặc tham số.
* **Tăng tính rõ ràng:** Làm cho mã nguồn dễ đọc hơn, rõ ràng rằng đây là một phương thức được ghi đè.

**Ví dụ:**

```java
class Shape {
    // Phương thức gốc ở lớp cha
    public void draw() {
        System.out.println("Drawing a generic shape.");
    }

    public double calculateArea() {
        System.out.println("Cannot calculate area for a generic shape.");
        return 0.0;
    }
}

class Circle extends Shape {
    private double radius;

    public Circle(double radius) { this.radius = radius; }

    // Ghi đè phương thức draw()
    @Override
    public void draw() {
        System.out.println("Drawing a circle with radius: " + radius);
    }

    // Ghi đè phương thức calculateArea()
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }

    // Phương thức riêng của Circle
    public double getRadius() {
        return radius;
    }
}

public class OverridingDemo {
    public static void main(String[] args) {
        Shape genericShape = new Shape();
        Circle myCircle = new Circle(5.0);

        genericShape.draw(); // Output: Drawing a generic shape.
        System.out.println("Area: " + genericShape.calculateArea()); // Output: Cannot calculate area... Area: 0.0

        myCircle.draw();     // Output: Drawing a circle with radius: 5.0
        System.out.println("Area: " + myCircle.calculateArea()); // Output: Area: 78.539...
    }
}
```

## 6. Từ khóa `super`

Từ khóa `super` được sử dụng bên trong lớp con để tham chiếu đến các thành viên của lớp cha ngay trước nó trong cây kế thừa.

**Các trường hợp sử dụng chính:**

1.  **Gọi Constructor của Lớp cha:**
    * Sử dụng `super()` hoặc `super(arguments)` để gọi constructor tương ứng của lớp cha.
    * Lời gọi `super(...)` **phải** là câu lệnh đầu tiên trong constructor của lớp con.
    * Nếu bạn không gọi `super(...)` tường minh, Java sẽ tự động chèn `super()` (không tham số). Nếu lớp cha không có constructor không tham số mặc định hoặc được định nghĩa, bạn sẽ gặp lỗi biên dịch.

    ```java
    class Person {
        String name;
        int age;

        // Constructor lớp cha
        Person(String name, int age) {
            System.out.println("Person constructor called.");
            this.name = name;
            this.age = age;
        }
    }

    class Employee extends Person {
        String employeeId;

        // Constructor lớp con gọi constructor lớp cha
        Employee(String name, int age, String employeeId) {
            super(name, age); // Phải là dòng đầu tiên
            System.out.println("Employee constructor called.");
            this.employeeId = employeeId;
        }

        void displayDetails() {
             System.out.println("Name: " + name + ", Age: " + age + ", ID: " + employeeId);
             // name và age được kế thừa và khởi tạo bởi super(name, age)
        }
    }

    public class SuperConstructorDemo {
        public static void main(String[] args) {
            Employee emp = new Employee("Alice", 30, "E123");
            emp.displayDetails();
            // Output:
            // Person constructor called.
            // Employee constructor called.
            // Name: Alice, Age: 30, ID: E123
        }
    }
    ```

2.  **Truy cập Phương thức bị Ghi đè của Lớp cha:**
    * Sử dụng `super.methodName(arguments)` để gọi phiên bản phương thức của lớp cha từ bên trong phương thức ghi đè ở lớp con. Điều này hữu ích khi bạn muốn mở rộng chức năng của lớp cha thay vì thay thế hoàn toàn.

    ```java
    class Vehicle {
        void startEngine() {
            System.out.println("Engine started.");
        }
    }

    class ElectricCar extends Vehicle {
        @Override
        void startEngine() {
            super.startEngine(); // Gọi phương thức gốc của Vehicle
            System.out.println("Electric systems activated."); // Thêm hành vi riêng
        }
    }

    public class SuperMethodDemo {
         public static void main(String[] args) {
             ElectricCar myECar = new ElectricCar();
             myECar.startEngine();
             // Output:
             // Engine started.
             // Electric systems activated.
         }
    }
    ```

3.  **Truy cập Thuộc tính của Lớp cha:**
    * Sử dụng `super.fieldName` để truy cập thuộc tính của lớp cha, đặc biệt hữu ích khi lớp con có một thuộc tính cùng tên (mặc dù đây là tình huống nên tránh để mã rõ ràng hơn - còn gọi là *variable shadowing*).

## 7. Lớp `Object` - Lớp Tổ tiên Chung

Trong Java, mọi lớp (class) đều ngầm định hoặc tường minh kế thừa từ lớp `java.lang.Object`. Điều này có nghĩa là `Object` là lớp gốc (root class) của toàn bộ hệ thống phân cấp lớp trong Java.

Do đó, mọi đối tượng trong Java đều có các phương thức cơ bản được định nghĩa trong lớp `Object`, bao gồm:

* **`toString()`:** Trả về một biểu diễn chuỗi (String representation) của đối tượng. Mặc định, nó trả về tên lớp và mã hash của đối tượng (`ClassName@hashCode`). Thường được ghi đè (`@Override`) trong các lớp con để cung cấp thông tin mô tả hữu ích hơn.
* **`equals(Object obj)`:** So sánh đối tượng hiện tại với một đối tượng khác xem chúng có "bằng nhau" hay không. Mặc định, `equals()` của `Object` chỉ kiểm tra xem hai tham chiếu có trỏ đến cùng một đối tượng trong bộ nhớ hay không (giống toán tử `==`). Các lớp con (như `String`, `Integer`, hoặc các lớp custom) thường ghi đè `equals()` để thực hiện so sánh dựa trên nội dung hoặc trạng thái của đối tượng.
* **`hashCode()`:** Trả về một giá trị `int` là mã băm (hash code) của đối tượng. Có một "hợp đồng" quan trọng giữa `equals()` và `hashCode()`: nếu hai đối tượng bằng nhau theo `equals()`, chúng phải có cùng `hashCode()`. Quy tắc này rất quan trọng khi sử dụng đối tượng làm key trong các cấu trúc dữ liệu dựa trên hash như `HashMap`, `HashSet`.
* **`getClass()`:** Trả về đối tượng `Class` đại diện cho lớp của đối tượng tại thời điểm chạy (runtime).
* `clone()`, `finalize()`, `wait()`, `notify()`, `notifyAll()`: Các phương thức khác liên quan đến sao chép đối tượng, dọn dẹp bộ nhớ, và đồng bộ hóa luồng (thread synchronization).

**Ghi đè `toString()` hiệu quả:**

Việc ghi đè `toString()` là một thực hành rất phổ biến và hữu ích để gỡ lỗi (debugging) và logging.

```java
class Point {
    private int x;
    private int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    // Ghi đè toString() để cung cấp thông tin hữu ích
    @Override
    public String toString() {
        return "Point[x=" + x + ", y=" + y + "]";
    }
}

public class ToStringDemo {
    public static void main(String[] args) {
        Point p1 = new Point(10, 20);
        System.out.println(p1); // Tự động gọi p1.toString()
        // Output: Point[x=10, y=20] (thay vì Point@<some_hash_code>)
    }
}
```

## 8. Tính Đa hình (Polymorphism)

**Polymorphism** (từ tiếng Hy Lạp nghĩa là "nhiều hình thái") là khả năng của một biến tham chiếu (reference variable) thuộc kiểu lớp cha có thể trỏ đến (tham chiếu đến) một đối tượng thuộc bất kỳ lớp con nào của nó.

**Khái niệm cốt lõi:**

* Một đối tượng có thể thể hiện nhiều "hình thái" khác nhau tùy thuộc vào ngữ cảnh (context) mà nó được sử dụng, cụ thể là kiểu của biến tham chiếu đang giữ nó.
* Cho phép viết mã tổng quát hơn, xử lý các đối tượng thuộc các lớp con khác nhau thông qua một giao diện chung (phương thức được định nghĩa ở lớp cha hoặc interface).

**Ví dụ thực tế:**

Hãy tưởng tượng bạn có một mảng các `Shape` (hình dạng), trong đó có thể chứa cả `Circle`, `Rectangle`, `Triangle`. Bạn có thể duyệt qua mảng này và gọi phương thức `draw()` trên từng phần tử mà không cần biết chính xác đó là loại hình nào.

```java
public class PolymorphismDemo {

    // Phương thức này chấp nhận bất kỳ đối tượng nào IS-A Shape
    public static void renderShape(Shape shape) {
        System.out.print("Rendering: ");
        shape.draw(); // -> Dynamic Binding sẽ quyết định gọi draw() của lớp nào
        System.out.println("Calculated Area: " + shape.calculateArea());
        System.out.println("---");
    }

    public static void main(String[] args) {
        Shape[] shapes = new Shape[3];
        shapes[0] = new Circle(5.0);       // Upcasting (tự động)
        shapes[1] = new Rectangle(4.0, 6.0); // Upcasting
        shapes[2] = new Shape();           // Đối tượng lớp cha

        for (Shape currentShape : shapes) {
            renderShape(currentShape); // Tính đa hình hoạt động ở đây!
        }
    }
}

// Giả sử lớp Rectangle cũng kế thừa Shape và ghi đè draw() và calculateArea()
class Rectangle extends Shape {
    private double width, height;
    public Rectangle(double w, double h) { this.width = w; this.height = h; }

    @Override
    public void draw() {
        System.out.println("Drawing a rectangle with width=" + width + ", height=" + height);
    }
    @Override
    public double calculateArea() { return width * height; }
}

/* Output của PolymorphismDemo:

Rendering: Drawing a circle with radius: 5.0
Calculated Area: 78.53981633974483
---
Rendering: Drawing a rectangle with width=4.0, height=6.0
Calculated Area: 24.0
---
Rendering: Drawing a generic shape.
Cannot calculate area for a generic shape.
Calculated Area: 0.0
---

*/
```

Trong ví dụ trên, lời gọi `shape.draw()` và `shape.calculateArea()` bên trong `renderShape` sẽ thực thi phiên bản phương thức của đối tượng *thực tế* (`Circle`, `Rectangle`, hay `Shape`) mà biến `shape` đang trỏ tới tại thời điểm đó. Đây chính là sức mạnh của đa hình và liên kết động.

## 9. Liên kết Động (Dynamic Binding / Late Binding)

**Dynamic Binding** (hay còn gọi là Late Binding hoặc Runtime Binding) là cơ chế mà **Máy ảo Java (JVM)** sử dụng để xác định xem phiên bản phương thức nào (của lớp cha hay lớp con) sẽ được thực thi tại **thời điểm chạy (runtime)**, dựa trên **kiểu thực tế (actual type)** của đối tượng mà biến tham chiếu đang trỏ tới, chứ không phải dựa trên **kiểu khai báo (declared type)** của biến tham chiếu đó.

* **Kiểu khai báo (Declared Type):** Là kiểu dữ liệu được chỉ định khi khai báo biến (ví dụ: `Shape s`). Trình biên dịch sử dụng kiểu này để kiểm tra cú pháp và tính hợp lệ của các lời gọi phương thức tại thời điểm biên dịch (compile-time).
* **Kiểu thực tế (Actual Type):** Là kiểu của đối tượng thực sự được tạo ra và gán cho biến tham chiếu (ví dụ: `new Circle()`). JVM sử dụng kiểu này tại runtime để quyết định phương thức nào sẽ được gọi.

**Ví dụ:**

```java
Shape myShape = new Circle(10); // Declared Type: Shape, Actual Type: Circle
Object obj = new Rectangle(2, 3); // Declared Type: Object, Actual Type: Rectangle

myShape.draw(); // JVM tại runtime thấy myShape đang giữ Circle -> gọi Circle.draw()
obj.toString(); // JVM tại runtime thấy obj đang giữ Rectangle -> gọi Rectangle.toString() (nếu có override)
                // hoặc Shape.toString() hoặc Object.toString() tùy vào cây kế thừa và override.
```

Dynamic Binding chỉ áp dụng cho các **phương thức instance** (non-static, non-final, non-private). Các phương thức `static`, `final`, `private` được xử lý bằng **Static Binding** (Early Binding) tại thời điểm biên dịch.

## 10. Ép kiểu Đối tượng (Object Casting)

**Casting** trong ngữ cảnh đối tượng là việc chuyển đổi một biến tham chiếu từ kiểu này sang kiểu khác trong cùng một cây kế thừa.

Có hai loại ép kiểu chính:

1.  **Ép kiểu Ngầm định (Implicit Casting / Upcasting):**
    * Tự động xảy ra khi bạn gán một đối tượng của lớp con cho một biến tham chiếu của lớp cha (hoặc interface mà lớp con implements).
    * Luôn an toàn vì lớp con chắc chắn "IS-A" lớp cha, nên nó có tất cả các phương thức/thuộc tính mà kiểu cha yêu cầu.
    * Ví dụ: `Shape s = new Circle();` hoặc `Object o = new String("hello");`

2.  **Ép kiểu Tường minh (Explicit Casting / Downcasting):**
    * Chuyển đổi một biến tham chiếu từ kiểu lớp cha xuống kiểu lớp con.
    * Phải được thực hiện một cách tường minh bằng cách đặt tên kiểu con trong cặp dấu ngoặc đơn `()` trước biến tham chiếu.
    * **Không phải lúc nào cũng an toàn.** Cần thực hiện khi bạn chắc chắn rằng đối tượng mà biến kiểu cha đang trỏ tới *thực sự* là một instance của kiểu con đó (hoặc một lớp con của kiểu con đó).
    * Nếu ép kiểu sai (đối tượng thực tế không phải là instance của kiểu đích), chương trình sẽ ném ra một ngoại lệ **`ClassCastException`** tại runtime.

    ```java
    Object obj = "Hello"; // Upcasting ngầm định String -> Object

    // Downcasting tường minh Object -> String (An toàn vì obj thực sự là String)
    String str = (String) obj;
    System.out.println(str.toUpperCase()); // OK

    Object numObj = Integer.valueOf(100); // Upcasting ngầm định Integer -> Object

    // Downcasting tường minh Object -> String (KHÔNG an toàn!)
    try {
        String failedStr = (String) numObj; // Sẽ ném ClassCastException ở đây
        System.out.println(failedStr);
    } catch (ClassCastException e) {
        System.err.println("Error: Cannot cast Integer to String. " + e.getMessage());
    }
    ```

**Toán tử `instanceof`:**

Để tránh `ClassCastException` khi thực hiện downcasting, hãy sử dụng toán tử `instanceof` để kiểm tra kiểu thực tế của đối tượng trước khi ép kiểu.

```java
public static void processObject(Object obj) {
    if (obj instanceof String) {
        // An toàn để ép kiểu sang String
        String s = (String) obj;
        System.out.println("It's a String: " + s.toLowerCase());
    } else if (obj instanceof Integer) {
        // An toàn để ép kiểu sang Integer
        Integer i = (Integer) obj;
        System.out.println("It's an Integer: " + i * 2);
    } else if (obj instanceof Circle) {
         // An toàn để ép kiểu sang Circle
        Circle c = (Circle) obj;
        System.out.println("It's a Circle with radius: " + c.getRadius()); // Giả sử Circle có getRadius()
    } else {
        System.out.println("Unknown object type: " + obj.toString());
    }
}

public static void main(String[] args) {
     processObject("Example");
     processObject(Integer.valueOf(50));
     processObject(new Circle(2.5));
     processObject(new java.util.Date()); // Kiểu không xác định trong hàm processObject
}
/* Output:
It's a String: example
It's an Integer: 100
It's a Circle with radius: 2.5
Unknown object type: Sun Apr 13 23:13:56 ICT 2025 // Tùy ngày giờ hiện tại
*/
```

*Lưu ý từ Java 14 trở đi:* Pattern Matching for `instanceof` giúp mã gọn hơn:
```java
if (obj instanceof String s) { // Vừa kiểm tra vừa khai báo biến s kiểu String
    System.out.println("It's a String: " + s.toLowerCase());
} else if (obj instanceof Integer i) { // Vừa kiểm tra vừa khai báo biến i kiểu Integer
    System.out.println("It's an Integer: " + i * 2);
} //...
```

## 11. Từ khóa `final` trong Kế thừa

Từ khóa `final` có thể được áp dụng cho lớp, phương thức, và biến, với ý nghĩa khác nhau, ảnh hưởng đến khả năng kế thừa và ghi đè:

1.  **`final` class:**
    * Một lớp được khai báo là `final` **không thể** bị kế thừa. Không lớp nào có thể `extends` từ một `final` class.
    * Ví dụ: `public final class String { ... }`. Lớp `String` của Java là `final`, bạn không thể tạo lớp con của `String`.
    * Sử dụng khi bạn muốn đảm bảo rằng hành vi và cấu trúc của lớp là bất biến, không thể bị thay đổi hoặc mở rộng bởi các lớp con (thường vì lý do bảo mật hoặc thiết kế).

    ```java
    public final class ImmutablePoint { // Lớp này không thể bị kế thừa
        private final int x; // Biến final
        private final int y;

        public ImmutablePoint(int x, int y) { this.x = x; this.y = y; }
        // Chỉ có getters, không có setters
        public int getX() { return x; }
        public int getY() { return y; }
    }

    // class MyPoint extends ImmutablePoint {} // -> Lỗi biên dịch!
    ```

2.  **`final` method:**
    * Một phương thức instance được khai báo là `final` **không thể** bị ghi đè (`@Override`) bởi các lớp con.
    * Sử dụng khi bạn muốn đảm bảo rằng hành vi cụ thể của một phương thức không bị thay đổi trong chuỗi kế thừa.

    ```java
    class BaseAlgorithm {
        // Phương thức này đảm bảo các bước cốt lõi không bị thay đổi
        public final void executeTemplate() {
            step1();
            step2_hook(); // Cho phép lớp con tùy chỉnh bước này
            step3();
        }

        private void step1() { System.out.println("Executing mandatory step 1."); }
        protected void step2_hook() { /* Default implementation, can be overridden */ }
        private void step3() { System.out.println("Executing mandatory step 3."); }

        // Phương thức này có thể bị ghi đè
        public void optionalOperation() { System.out.println("Base optional operation."); }
    }

    class ConcreteAlgorithm extends BaseAlgorithm {
        @Override
        protected void step2_hook() {
            System.out.println("Executing custom step 2 for ConcreteAlgorithm.");
        }

        // @Override
        // public final void executeTemplate() {} // -> Lỗi biên dịch! Không thể ghi đè final method.

        @Override
        public void optionalOperation() { System.out.println("Concrete optional operation."); }
    }
    ```

3.  **`final` variable:** (Không trực tiếp liên quan đến kế thừa nhưng quan trọng)
    * Một biến `final` chỉ có thể được gán giá trị một lần (tại thời điểm khai báo hoặc trong constructor). Sau khi được gán, giá trị của nó không thể thay đổi. Nó trở thành một hằng số (constant).

## 12. Các Loại Hình Kế thừa (và Hạn chế trong Java)

Có nhiều cấu trúc kế thừa khác nhau:

* **Đơn Kế thừa (Single Inheritance):** Một lớp con chỉ kế thừa từ một lớp cha duy nhất. **Đây là loại hình kế thừa lớp (class inheritance) duy nhất mà Java hỗ trợ.**
    ```
      A
      |
      B
    ```
* **Kế thừa Đa Cấp (Multilevel Inheritance):** Một lớp kế thừa từ một lớp con khác, tạo thành một chuỗi kế thừa. Java hỗ trợ loại hình này.
    ```
      A
      |
      B
      |
      C // C kế thừa B, B kế thừa A
    ```
* **Kế thừa Thứ Bậc (Hierarchical Inheritance):** Nhiều lớp con cùng kế thừa từ một lớp cha duy nhất. Java hỗ trợ loại hình này.
    ```
        A
       / \
      B   C
    ```
* **Đa Kế thừa (Multiple Inheritance):** Một lớp con kế thừa từ nhiều lớp cha cùng lúc. **Java KHÔNG hỗ trợ đa kế thừa cho các lớp (classes).**
    ```
       A   B
        \ /
         C // Không được phép trong Java cho class
    ```
    Lý do chính Java không hỗ trợ đa kế thừa lớp là để tránh **"Vấn đề Kim Cương" (Diamond Problem)**. Nếu lớp `C` kế thừa từ `A` và `B`, và cả `A` và `B` đều có một phương thức cùng tên (ví dụ, kế thừa từ một lớp `D` nào đó hoặc tự định nghĩa), thì trình biên dịch không biết phải sử dụng phiên bản phương thức nào khi gọi từ đối tượng `C`.

* **Kế thừa Lai (Hybrid Inheritance):** Kết hợp của các loại hình trên (ví dụ: kết hợp Hierarchical và Multilevel). Java hỗ trợ miễn là không tạo ra đa kế thừa lớp.

**Giải pháp cho Đa kế thừa trong Java:**

Mặc dù không hỗ trợ đa kế thừa *lớp*, Java cho phép một lớp **implement (thực thi)** nhiều **interfaces**. Interface chỉ định các phương thức trừu tượng (contract) mà lớp phải cung cấp triển khai. Điều này cho phép một lớp "thừa hưởng" *kiểu* (type) và *hợp đồng* (contract) từ nhiều nguồn mà không gặp phải Diamond Problem về mặt triển khai (implementation).

## 13. Ưu điểm và Nhược điểm của Kế thừa

**Ưu điểm:**

* **Tái sử dụng mã:** Giảm thiểu sự trùng lặp mã, dễ bảo trì hơn khi chỉ cần sửa lỗi hoặc cập nhật ở một nơi (lớp cha).
* **Tổ chức code:** Tạo ra cấu trúc phân cấp rõ ràng, phản ánh mối quan hệ giữa các khái niệm.
* **Tính đa hình:** Cho phép viết mã linh hoạt và tổng quát, dễ dàng mở rộng hệ thống bằng cách thêm các lớp con mới mà không cần sửa đổi mã hiện có (tuân thủ Open/Closed Principle).
* **Mở rộng:** Dễ dàng thêm chức năng mới vào lớp con.

**Nhược điểm:**

* **Kopplung Chặt (Tight Coupling):** Lớp con bị phụ thuộc chặt chẽ vào lớp cha. Bất kỳ thay đổi nào trong lớp cha (đặc biệt là thay đổi signature phương thức hoặc xóa bỏ thành viên) có thể làm hỏng các lớp con.
* **Tính đóng gói bị phá vỡ (Encapsulation Breakage):** Nếu lớp con truy cập trực tiếp vào các thành viên `protected` của lớp cha, nó sẽ biết quá nhiều về chi tiết triển khai bên trong của lớp cha, làm giảm tính đóng gói.
* **Hệ thống phân cấp phức tạp:** Cây kế thừa quá sâu hoặc quá rộng có thể trở nên khó hiểu và khó quản lý.
* **Lạm dụng kế thừa:** Đôi khi lập trình viên sử dụng kế thừa trong khi **Composition (Thành phần)** lại là lựa chọn tốt hơn và linh hoạt hơn (xem mục tiếp theo).

## 14. Composition vs Inheritance ("HAS-A" vs "IS-A")

Một nguyên tắc thiết kế hướng đối tượng quan trọng là **"Ưu tiên Composition hơn Inheritance" (Favor Composition over Inheritance)**.

* **Inheritance (IS-A):** Biểu thị mối quan hệ "là một". Lớp con *là một loại* của lớp cha. Sử dụng khi có mối quan hệ phân loại rõ ràng và lớp con thực sự cần thừa hưởng phần lớn hành vi của lớp cha.
* **Composition (HAS-A):** Biểu thị mối quan hệ "có một" hoặc "sử dụng một". Một lớp chứa một tham chiếu đến một đối tượng của lớp khác để sử dụng chức năng của nó. Lớp chứa (container) *ủy thác* (delegates) các tác vụ cho đối tượng thành phần (component).

**Khi nào dùng Composition?**

* Khi bạn chỉ cần *sử dụng* chức năng của một lớp khác mà không cần phải *là* một loại của lớp đó.
* Khi bạn muốn thay đổi hành vi được sử dụng tại runtime (bằng cách thay đổi đối tượng thành phần).
* Khi bạn muốn tránh sự kopplung chặt chẽ của kế thừa.
* Khi mối quan hệ không phải là "IS-A" mà là "HAS-A". Ví dụ: Một `Car` **HAS-A** `Engine` (Xe hơi *có một* động cơ), chứ không phải `Car` **IS-A** `Engine`.

**Ví dụ Composition:**

```java
// Thành phần Engine
class Engine {
    void start() { System.out.println("Engine starts."); }
    void stop() { System.out.println("Engine stops."); }
}

// Lớp Car sử dụng Engine thông qua Composition
class Car {
    private Engine engine; // Car HAS-A Engine (Composition)
    private String model;

    // Inject Engine qua constructor (Dependency Injection)
    public Car(String model, Engine engine) {
        this.model = model;
        this.engine = engine; // Giữ tham chiếu đến đối tượng Engine
    }

    // Ủy thác hành vi cho đối tượng Engine
    public void startCar() {
        System.out.print(model + ": ");
        engine.start();
    }

    public void stopCar() {
        System.out.print(model + ": ");
        engine.stop();
    }
}

// Các loại Engine khác nhau
class ElectricEngine extends Engine {
    @Override void start() { System.out.println("Silent electric engine starts."); }
    @Override void stop() { System.out.println("Electric engine stops."); }
}

class PetrolEngine extends Engine {
     @Override void start() { System.out.println("Vroom! Petrol engine starts."); }
     @Override void stop() { System.out.println("Petrol engine sputters to stop."); }
}


public class CompositionDemo {
    public static void main(String[] args) {
        Engine electric = new ElectricEngine();
        Engine petrol = new PetrolEngine();

        Car tesla = new Car("Tesla Model S", electric);
        Car ford = new Car("Ford Mustang", petrol);

        tesla.startCar(); // Tesla Model S: Silent electric engine starts.
        ford.startCar();  // Ford Mustang: Vroom! Petrol engine starts.

        tesla.stopCar();  // Tesla Model S: Electric engine stops.
        ford.stopCar();   // Ford Mustang: Petrol engine sputters to stop.
    }
}
```

Composition thường dẫn đến thiết kế linh hoạt, dễ kiểm thử (testable) và ít phụ thuộc hơn so với Inheritance.

## 15. Thực hành Tốt nhất (Best Practices) & Mẹo Gỡ lỗi

* **Tuân thủ quan hệ "IS-A":** Chỉ sử dụng kế thừa khi lớp con thực sự *là một loại* của lớp cha. Nếu không, hãy cân nhắc Composition.
* **Giữ hệ thống phân cấp nông (Shallow Hierarchy):** Tránh tạo cây kế thừa quá sâu (nhiều hơn 2-3 cấp thường là dấu hiệu cần xem xét lại thiết kế).
* **Sử dụng `protected` một cách cẩn thận:** Chỉ `protected` những thành viên mà bạn thực sự muốn lớp con truy cập và có lý do chính đáng. Ưu tiên sử dụng `private` và cung cấp các phương thức `public/protected` để tương tác.
* **Luôn dùng `@Override`:** Giúp tránh lỗi và làm rõ ý định ghi đè phương thức.
* **Cẩn thận khi thay đổi lớp cha:** Nhận thức rằng thay đổi lớp cha có thể ảnh hưởng đến tất cả các lớp con.
* **Sử dụng `final` class/method khi thích hợp:** Ngăn chặn kế thừa hoặc ghi đè khi cần thiết để đảm bảo tính toàn vẹn hoặc bảo mật.
* **Hiểu rõ `super()`:** Đảm bảo constructor của lớp con gọi đúng constructor của lớp cha (tường minh hoặc ngầm định). Lỗi liên quan đến `super()` thường xảy ra khi lớp cha không có constructor không tham số.
* **Tránh `ClassCastException`:** Luôn sử dụng `instanceof` (hoặc pattern matching) trước khi thực hiện ép kiểu tường minh (downcasting).
* **Hiểu về `Object`:** Ghi đè `toString()`, `equals()`, và `hashCode()` một cách nhất quán và đúng đắn khi cần thiết, đặc biệt khi làm việc với Collections.
* **Gỡ lỗi (Debugging):**
    * Sử dụng debugger để theo dõi giá trị biến và dòng thực thi, đặc biệt khi làm việc với đa hình để xem phương thức của lớp nào thực sự được gọi.
    * In ra thông tin đối tượng (sử dụng `toString()` đã ghi đè) để kiểm tra trạng thái.
    * Kiểm tra kỹ các lỗi `NullPointerException` (khi gọi phương thức trên biến tham chiếu `null`) và `ClassCastException`.