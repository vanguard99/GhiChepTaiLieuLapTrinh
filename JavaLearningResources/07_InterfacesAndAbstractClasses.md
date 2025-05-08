# INTERFACE VÀ ABSTRACT CLASS

## 1\. Giới thiệu

Trong lập trình hướng đối tượng (OOP), **tính trừu tượng (abstraction)** là một khái niệm nền tảng, cho phép chúng ta mô hình hóa thế giới thực vào mã nguồn bằng cách tập trung vào các đặc tính thiết yếu và bỏ qua các chi tiết không cần thiết. Java cung cấp hai cơ chế chính để đạt được tính trừu tượng ở cấp độ lớp: **Lớp trừu tượng (Abstract Class)** và **Giao diện (Interface)**.

Tài liệu này sẽ đi sâu vào bản chất, cách sử dụng, và sự khác biệt giữa `abstract class` và `interface` trong Java. Mục tiêu là cung cấp cho lập trình viên Java một hiểu biết toàn diện, từ khái niệm cơ bản đến các ứng dụng thực tế, giúp bạn đưa ra quyết định thiết kế đúng đắn và xây dựng mã nguồn linh hoạt, dễ bảo trì.

## 2\. Lớp trừu tượng (Abstract Class)

### 2.1. Khái niệm cơ bản

Một **lớp trừu tượng (abstract class)** là một lớp được khai báo với từ khóa `abstract`. Nó đóng vai trò như một *khuôn mẫu* hoặc *lớp cơ sở* cho các lớp khác kế thừa. Điểm đặc biệt là bạn **không thể tạo ra đối tượng (instance)** trực tiếp từ một abstract class.

Mục đích chính của abstract class là:

1.  **Định nghĩa trạng thái và hành vi chung:** Cung cấp các thuộc tính (biến thành viên) và phương thức (cụ thể hoặc trừu tượng) mà các lớp con có thể thừa hưởng và sử dụng lại.
2.  **Buộc các lớp con phải cài đặt các hành vi cụ thể:** Thông qua các phương thức trừu tượng.

<!-- end list -->

```java
// Ví dụ về khai báo một abstract class
public abstract class HinhHoc {
    private String mauSac;

    // Constructor (có thể có trong abstract class)
    protected HinhHoc(String mauSac) {
        this.mauSac = mauSac;
    }

    // Phương thức cụ thể (có phần thân)
    public String getMauSac() {
        return mauSac;
    }

    public void setMauSac(String mauSac) {
        this.mauSac = mauSac;
    }

    // Phương thức trừu tượng (sẽ được định nghĩa ở lớp con)
    public abstract double tinhDienTich();
    public abstract double tinhChuVi();

    // Một phương thức cụ thể khác
    public void hienThiThongTin() {
        System.out.println("Màu sắc: " + mauSac);
        System.out.println("Diện tích: " + tinhDienTich());
        System.out.println("Chu vi: " + tinhChuVi());
    }
}
```

### 2.2. Phương thức trừu tượng (Abstract Method)

Một **phương thức trừu tượng (abstract method)** là một phương thức được khai báo với từ khóa `abstract` và **không có phần thân (implementation)**, chỉ có chữ ký (signature).

```java
public abstract double tinhDienTich(); // Chỉ khai báo, không có dấu {}
```

Mục đích của abstract method là định nghĩa một "hợp đồng" mà *mọi lớp con không trừu tượng* kế thừa từ abstract class này *bắt buộc phải cung cấp cài đặt cụ thể* cho phương thức đó. Điều này đảm bảo rằng tất cả các "hình học" cụ thể (như hình tròn, hình chữ nhật) đều phải biết cách tính diện tích và chu vi của riêng chúng.

### 2.3. Khi nào sử dụng Abstract Class?

Sử dụng `abstract class` khi:

  * Bạn muốn **chia sẻ mã nguồn** (các phương thức đã được cài đặt hoặc các biến thành viên) giữa các lớp con có liên quan chặt chẽ.
  * Các lớp con có chung một **bản chất cơ sở**, thể hiện mối quan hệ **"is-a"** (ví dụ: `HinhTron` *is a* `HinhHoc`, `HinhChuNhat` *is a* `HinhHoc`).
  * Bạn muốn định nghĩa các **biến thành viên không phải là `static` và `final`**. Interface chỉ cho phép các hằng số `public static final`.
  * Bạn cần khai báo các phương thức có phạm vi truy cập (access modifier) khác `public` (ví dụ: `protected abstract`). Interface (trước Java 9) chỉ cho phép `public abstract` methods.
  * Bạn muốn cung cấp một số phương thức đã được cài đặt sẵn, đồng thời bắt buộc các lớp con phải cài đặt các phương thức trừu tượng khác (Template Method Pattern là một ví dụ điển hình).

### 2.4. Ví dụ thực tế về Abstract Class

Hãy xem xét lớp `HinhHoc` ở trên và các lớp con cụ thể:

```java
// Lớp con kế thừa từ HinhHoc
public class HinhTron extends HinhHoc {
    private double banKinh;

    public HinhTron(String mauSac, double banKinh) {
        super(mauSac); // Gọi constructor của lớp cha
        this.banKinh = banKinh;
    }

    public double getBanKinh() {
        return banKinh;
    }

    public void setBanKinh(double banKinh) {
        this.banKinh = banKinh;
    }

    // Bắt buộc phải @Override các abstract method từ lớp cha
    @Override
    public double tinhDienTich() {
        return Math.PI * banKinh * banKinh;
    }

    @Override
    public double tinhChuVi() {
        return 2 * Math.PI * banKinh;
    }

    // Có thể có thêm phương thức riêng của HinhTron
    public double tinhDuongKinh() {
        return 2 * banKinh;
    }
}

public class HinhChuNhat extends HinhHoc {
    private double chieuDai;
    private double chieuRong;

    public HinhChuNhat(String mauSac, double chieuDai, double chieuRong) {
        super(mauSac);
        this.chieuDai = chieuDai;
        this.chieuRong = chieuRong;
    }

    // Getters và Setters cho chieuDai, chieuRong...

    @Override
    public double tinhDienTich() {
        return chieuDai * chieuRong;
    }

    @Override
    public double tinhChuVi() {
        return 2 * (chieuDai + chieuRong);
    }
}

// Sử dụng
public class Main {
    public static void main(String[] args) {
        // Không thể tạo đối tượng trực tiếp từ HinhHoc
        // HinhHoc hinh = new HinhHoc("Xanh"); // Lỗi biên dịch!

        HinhHoc hinhTron = new HinhTron("Đỏ", 5.0);
        HinhHoc hinhChuNhat = new HinhChuNhat("Xanh", 4.0, 6.0);

        hinhTron.hienThiThongTin();
        System.out.println("---");
        hinhChuNhat.hienThiThongTin();

        // Tính đa hình (polymorphism)
        HinhHoc[] danhSachHinh = {hinhTron, hinhChuNhat};
        for (HinhHoc hinh : danhSachHinh) {
             System.out.println("Diện tích của hình màu " + hinh.getMauSac() + " là: " + hinh.tinhDienTich());
        }
    }
}
```

**Giải thích:**

  * `HinhHoc` định nghĩa cấu trúc chung (màu sắc) và các hành vi bắt buộc (`tinhDienTich`, `tinhChuVi`).
  * `HinhTron` và `HinhChuNhat` kế thừa `HinhHoc`, sử dụng lại `mauSac` và cung cấp cài đặt cụ thể cho các phương thức trừu tượng.
  * Không thể tạo `new HinhHoc()`.
  * Có thể khai báo biến kiểu `HinhHoc` nhưng tham chiếu đến đối tượng của lớp con (`HinhTron`, `HinhChuNhat`), thể hiện tính đa hình.

### 2.5. Các quy tắc và lưu ý quan trọng

  * Một lớp chứa ít nhất một `abstract method` thì *bắt buộc* phải được khai báo là `abstract class`.
  * Một `abstract class` *không nhất thiết* phải có `abstract method`. Nó có thể chỉ đơn thuần là không cho phép tạo đối tượng.
  * Nếu một lớp con kế thừa từ một `abstract class` nhưng *không* cài đặt *tất cả* các `abstract method` của lớp cha, thì lớp con đó cũng *phải* được khai báo là `abstract`.
  * `abstract class` không thể được khai báo là `final` (vì mục đích của nó là để được kế thừa).
  * `abstract method` không thể được khai báo là `final` (vì chúng phải được override), `static` (vì chúng thuộc về đối tượng, không phải lớp), hoặc `private` (vì chúng phải được nhìn thấy và cài đặt bởi lớp con).

## 3\. Giao diện (Interface)

### 3.1. Khái niệm cơ bản

Một **giao diện (interface)** trong Java là một kiểu tham chiếu hoàn toàn trừu tượng, tương tự như `abstract class`, nhưng với mục đích chính là định nghĩa một **"hợp đồng" (contract)** về các hành vi mà một lớp phải tuân thủ nếu muốn triển khai (implement) interface đó.

Interface được khai báo bằng từ khóa `interface`. Theo truyền thống (trước Java 8), một interface chỉ chứa:

1.  **Hằng số (Constants):** Các biến được ngầm định là `public static final`.
2.  **Phương thức trừu tượng (Abstract Methods):** Các phương thức được ngầm định là `public abstract`.

<!-- end list -->

```java
// Ví dụ về khai báo interface
public interface DongVatAnThit {
    // Hằng số (ngầm định là public static final)
    String LOAI_THUC_AN = "Thịt";

    // Phương thức trừu tượng (ngầm định là public abstract)
    void sanMoi();
    void an(); // Có thể bỏ qua public abstract
}

public interface CoTheBay {
    int TOC_DO_TOI_DA = 100; // public static final

    void catCanh();        // public abstract
    void haCanh();         // public abstract
}
```

Từ **Java 8** trở đi, interface có thể chứa thêm:

3.  **Phương thức mặc định (Default Methods):** Các phương thức có từ khóa `default` và có phần thân cài đặt sẵn. Lớp triển khai có thể sử dụng cài đặt này hoặc `@Override` nó.
4.  **Phương thức tĩnh (Static Methods):** Các phương thức có từ khóa `static` và có phần thân. Chúng thuộc về interface và được gọi trực tiếp qua tên interface.

Từ **Java 9** trở đi, interface còn có thể chứa:

5.  **Phương thức riêng tư (Private Methods):** Bao gồm cả `private` và `private static` methods, dùng để chia sẻ mã giữa các `default` hoặc `static` methods bên trong cùng interface đó.

### 3.2. Triển khai Interface (Implementing Interfaces)

Một lớp sử dụng từ khóa `implements` để "ký hợp đồng" với một hoặc nhiều interface. Khi một lớp `implements` một interface, nó phải:

1.  Cung cấp cài đặt cụ thể cho *tất cả* các phương thức trừu tượng được định nghĩa trong interface đó (và các interface cha của nó).
2.  Hoặc, nếu lớp không muốn/thể cài đặt hết các phương thức trừu tượng, lớp đó phải được khai báo là `abstract`.

Một điểm mạnh của interface là một lớp có thể `implements` **nhiều interface**, giúp giải quyết vấn đề "đa kế thừa" (Java chỉ cho phép đơn kế thừa lớp - `extends` chỉ một lớp).

```java
// Lớp Ho kế thừa lớp DongVat và triển khai 2 interface
public class Ho extends DongVat implements DongVatAnThit, CoTheChay { // Giả sử có interface CoTheChay

    public Ho(String ten) {
        super(ten);
    }

    @Override
    public void sanMoi() {
        System.out.println(getTen() + " đang rình mồi...");
    }

    @Override
    public void an() {
        System.out.println(getTen() + " đang ăn " + LOAI_THUC_AN); // Sử dụng hằng số từ interface
    }

    @Override
    public void chay(int tocDo) {
        System.out.println(getTen() + " đang chạy với tốc độ " + tocDo + " km/h");
    }

    // Override phương thức tu DongVat (lớp cha trừu tượng)
    @Override
    public void keu() {
       System.out.println(getTen() + " gầm!");
    }
}

// Lớp ChimDaiBang kế thừa DongVat và triển khai nhiều interface
public class ChimDaiBang extends DongVat implements DongVatAnThit, CoTheBay {

    public ChimDaiBang(String ten) {
        super(ten);
    }

    // Implement các phương thức của DongVatAnThit
    @Override
    public void sanMoi() {
        System.out.println(getTen() + " đang bay lượn tìm mồi...");
    }

    @Override
    public void an() {
        System.out.println(getTen() + " đang ăn thịt tươi.");
    }

    // Implement các phương thức của CoTheBay
    @Override
    public void catCanh() {
        System.out.println(getTen() + " dang sải cánh bay lên bầu trời.");
    }

    @Override
    public void haCanh() {
        System.out.println(getTen() + " đang hạ cánh xuống cành cây.");
    }

     // Override phương thức tu DongVat
    @Override
    public void keu() {
       System.out.println(getTen() + " kêu!");
    }
}
```

### 3.3. Kế thừa Interface (Extending Interfaces)

Một interface có thể kế thừa từ một hoặc nhiều interface khác bằng cách sử dụng từ khóa `extends`. Interface con sẽ thừa hưởng tất cả các hằng số và phương thức (abstract, default, static) từ các interface cha.

```java
// Interface CoTheBayNhanh kế thừa CoTheBay và thêm phương thức mới
public interface CoTheBayNhanh extends CoTheBay {
    void baySieuThanh(); // Thêm phương thức trừu tượng mới
}

// Một lớp triển khai interface con phải implement tất cả phương thức
// từ cả interface con và cha (trừ default/static methods)
public class MayBayChienDau implements CoTheBayNhanh, CoVuKhi { // Giả sử có interface CoVuKhi

    @Override
    public void catCanh() { System.out.println("Máy bay cất cánh khỏi đường băng."); }

    @Override
    public void haCanh() { System.out.println("Máy bay hạ cánh."); }

    @Override
    public void baySieuThanh() { System.out.println("Vượt tốc độ âm thanh!"); }

    @Override
    public void khaiHoa() { System.out.println("Bắn tên lửa!"); }
}
```

### 3.4. Khi nào sử dụng Interface?

Sử dụng `interface` khi:

  * Bạn muốn định nghĩa một **hợp đồng** về các hành vi mà các lớp (có thể không liên quan trực tiếp đến nhau) phải thực hiện. Thể hiện mối quan hệ **"can-do"** hoặc **"has-behavior"** (ví dụ: `Bird` *can* `Fly`, `Plane` *can* `Fly`).
  * Bạn muốn tận dụng **đa kế thừa kiểu (type inheritance)**. Một lớp có thể thuộc về nhiều "kiểu" (interfaces) khác nhau.
  * Bạn muốn thiết kế **API linh hoạt, tách rời (decoupled)**. Các thành phần giao tiếp với nhau thông qua interface thay vì các lớp cụ thể, giúp dễ dàng thay đổi hoặc mở rộng cài đặt. (Ví dụ: Strategy Pattern, Observer Pattern, Dependency Injection).
  * Bạn chỉ cần định nghĩa các hành vi mà không cần chia sẻ trạng thái (biến thành viên non-static/non-final) hoặc mã nguồn cài đặt chung (trừ khi sử dụng `default` methods).
  * Bạn muốn tạo ra các **functional interfaces** để sử dụng với Lambda Expressions (từ Java 8).

### 3.5. Ví dụ thực tế về Interface (với Java 8+)

```java
// Functional Interface (chỉ có một abstract method)
@FunctionalInterface
public interface SapXep {
    void sapXepMang(int[] arr);
}

// Interface với default và static method
public interface MayTinh {
    double PI = 3.14159; // public static final

    // Abstract method
    double tinhToan(double a, double b);

    // Default method (Java 8+)
    default void gioiThieu() {
        System.out.println("Đây là một máy tính cơ bản.");
        inPhienBan(); // Gọi private method (Java 9+)
    }

    // Static method (Java 8+)
    static void thongTinHang() {
        System.out.println("Sản xuất bởi Java Corp.");
    }

    // Private static method (Java 9+) - chỉ dùng nội bộ
    private static void kiemTraNguon() {
        System.out.println("Kiểm tra nguồn điện...");
    }

    // Private method (Java 9+) - chỉ dùng nội bộ
    private void inPhienBan() {
       System.out.println("Phiên bản 2.0");
    }
}

// Lớp triển khai
public class MayTinhCongTru implements MayTinh {
    @Override
    public double tinhToan(double a, double b) {
        return a + b; // Cài đặt phép cộng
    }

    // Có thể @Override default method nếu muốn
    @Override
    public void gioiThieu() {
        System.out.println("Máy tính Cộng/Trừ Siêu Cấp");
    }
}

public class Main {
    public static void main(String[] args) {
        // Sử dụng static method
        MayTinh.thongTinHang();

        MayTinh mayCong = new MayTinhCongTru();
        System.out.println("5 + 3 = " + mayCong.tinhToan(5, 3));
        mayCong.gioiThieu(); // Gọi phương thức đã override

        // Sử dụng interface làm kiểu dữ liệu
        MayTinh mayNhan = new MayTinh() { // Anonymous class triển khai interface
             @Override
             public double tinhToan(double a, double b) {
                 return a * b;
             }
             // Có thể override default method ở đây nếu muốn
        };
        System.out.println("5 * 3 = " + mayNhan.tinhToan(5, 3));
        mayNhan.gioiThieu(); // Gọi default method từ interface

        // Sử dụng functional interface với Lambda Expression (Java 8+)
        SapXep bubbleSort = (arr) -> {
            // Code thuật toán bubble sort ...
            System.out.println("Đã sắp xếp mảng bằng Bubble Sort.");
        };
        int[] data = {5, 1, 4, 2, 8};
        bubbleSort.sapXepMang(data);
    }
}
```

**Giải thích:**

  * `SapXep` là Functional Interface, cho phép dùng Lambda `(arr) -> {...}`.
  * `MayTinh` minh họa hằng số, abstract, default, static, và private methods.
  * Lớp `MayTinhCongTru` implement abstract method và override default method.
  * Anonymous class được dùng để tạo nhanh một cài đặt cho `MayTinh` (phép nhân).
  * `static method` được gọi trực tiếp qua tên interface. `default method` được gọi qua đối tượng.

### 3.6. Các quy tắc và lưu ý quan trọng

  * Tất cả các biến khai báo trong interface mặc định là `public static final` (hằng số). Bạn không cần viết tường minh các từ khóa này.
  * Tất cả các phương thức trừu tượng (không có `default` hoặc `static`) mặc định là `public abstract`. Bạn không cần viết tường minh.
  * `default methods` cho phép thêm chức năng vào interface mà không phá vỡ các lớp đã triển khai interface đó trước đây. Tuy nhiên, lạm dụng có thể làm interface trở nên giống `abstract class`, mất đi tính "hợp đồng thuần túy".
  * `static methods` trong interface thường dùng cho các phương thức tiện ích (utility methods) liên quan đến interface đó.
  * Một lớp có thể `implements` nhiều interface, nhưng nếu các interface đó có `default methods` với cùng chữ ký, lớp triển khai *bắt buộc phải `@Override`* phương thức đó để giải quyết xung đột (ambiguity). Tương tự nếu một lớp kế thừa một lớp cha và implement một interface có default method trùng tên.

## 4\. So sánh Abstract Class và Interface

| Đặc điểm               | Abstract Class (`abstract class`)                                                    | Interface (`interface`)                                                                                                     |
| ---------------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| **Mục đích chính**     | Cung cấp khuôn mẫu, chia sẻ code (is-a relationship)                                 | Định nghĩa hợp đồng hành vi (can-do relationship)                                                                           |
| **Khởi tạo đối tượng** | **Không thể**                                                                        | **Không thể**                                                                                                               |
| **Constructor**        | **Có thể có** (thường là `protected`)                                                | **Không có**                                                                                                                |
| **Biến thành viên**    | Bất kỳ loại biến nào (instance, static, final...)                                    | Chỉ `public static final` (hằng số)                                                                                         |
| **Phương thức**        | Abstract methods; Concrete methods (đã cài đặt); Có thể có `static`, `final` methods | Abstract methods (ngầm định `public`); `default` methods (Java 8+); `static` methods (Java 8+); `private` methods (Java 9+) |
| **Từ khóa triển khai** | `extends` (chỉ một lớp)                                                              | `implements` (nhiều interface)                                                                                              |
| **Đa kế thừa**         | Không hỗ trợ đa kế thừa lớp                                                          | Hỗ trợ "đa kế thừa kiểu" qua `implements` nhiều interface                                                                   |
| **Quan hệ điển hình**  | **Is-a** (là một loại của)                                                           | **Can-do / Has-behavior** (có khả năng làm gì đó)                                                                           |
| **Khi nào chọn?**      | Cần chia sẻ code cài đặt hoặc trạng thái; Các lớp con có quan hệ cha-con rõ ràng     | Chỉ cần định nghĩa hợp đồng; Cần đa kế thừa kiểu; Các lớp không liên quan cần chung hành vi                                 |



**Tóm tắt:**

  * **Chọn `abstract class`** nếu bạn đang tạo một lớp cơ sở mà các lớp con có quan hệ "is-a" chặt chẽ và bạn muốn chia sẻ mã nguồn cài đặt hoặc trạng thái (biến thành viên) giữa chúng.
  * **Chọn `interface`** nếu bạn chỉ muốn định nghĩa một "hợp đồng" về hành vi mà các lớp (có thể không liên quan) phải tuân theo (quan hệ "can-do"), hoặc khi bạn cần một lớp có thể đảm nhiệm nhiều vai trò (đa kế thừa kiểu). **Ưu tiên sử dụng interface nếu chỉ cần định nghĩa hợp đồng.**

## 5\. Lớp Nặc danh (Anonymous Class)

### 5.1. Khái niệm và Cú pháp

**Lớp nặc danh (anonymous class)** là một lớp không có tên, được khai báo và khởi tạo đối tượng ngay tại thời điểm sử dụng. Nó thường được dùng để tạo nhanh một đối tượng "dùng một lần" với một cài đặt riêng biệt cho một lớp (thường là abstract class) hoặc một interface.

**Cú pháp:**

  * **Triển khai Interface:**
    ```java
    InterfaceName obj = new InterfaceName() {
        // Cài đặt các phương thức của interface
        @Override
        public void method1() { /* ... */ }
        // ...
    };
    ```
  * **Kế thừa Lớp (thường là Abstract Class):**
    ```java
    ClassName obj = new ClassName(constructor_args) {
        // Override các phương thức (thường là abstract)
        @Override
        public void method2() { /* ... */ }
        // ...
        // Có thể thêm phương thức/biến riêng (ít dùng)
    };
    ```

### 5.2. Trường hợp sử dụng

Anonymous class rất hữu ích trong các trường hợp:

  * **Xử lý sự kiện (Event Handling):** Ví dụ, tạo các `ActionListener`, `MouseListener` trong lập trình GUI Swing/AWT.
  * **Tạo các đối tượng `Runnable`, `Callable`:** Để thực thi trong các luồng (threads).
  * **Callbacks:** Cung cấp một cài đặt cụ thể cho một phương thức yêu cầu một đối tượng của interface/abstract class làm tham số.
  * **Tạo nhanh đối tượng với cài đặt đơn giản:** Khi việc tạo một lớp con riêng biệt đầy đủ là không cần thiết và làm mã nguồn dài dòng.

### 5.3. Ví dụ

```java
import javax.swing.*;
import java.awt.event.*;

public class AnonymousDemo {
    public static void main(String[] args) {
        // 1. Sử dụng với Interface (Runnable)
        Runnable task = new Runnable() {
            @Override
            public void run() {
                System.out.println("Công việc đang chạy trong luồng nặc danh!");
            }
        };
        new Thread(task).start();

        // 2. Sử dụng với Interface (ActionListener trong Swing)
        JFrame frame = new JFrame("Demo");
        JButton button = new JButton("Click Me");

        button.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                JOptionPane.showMessageDialog(frame, "Bạn đã click nút!");
            }
        });

        // 3. Sử dụng với Abstract Class (giả định)
        abstract class Greeter {
            abstract void greet();
        }

        Greeter vietnameseGreeter = new Greeter() {
            @Override
            void greet() {
                System.out.println("Xin chào!");
            }
        };
        vietnameseGreeter.greet();


        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.add(button);
        frame.pack();
        frame.setVisible(true);
    }
}
```

### 5.4. Hạn chế và Thay thế (Lambda Expressions)

**Hạn chế:**

  * **Cú pháp dài dòng:** Đặc biệt với các interface chỉ có một phương thức (functional interfaces).
  * **Khó tái sử dụng:** Vì chúng không có tên.
  * **Giới hạn truy cập:** Chỉ có thể truy cập các biến `final` hoặc *effectively final* (không thay đổi giá trị sau khi khởi tạo) từ phạm vi bao quanh nó.

**Thay thế với Lambda Expressions (Java 8+):**

Đối với các **functional interfaces** (interface chỉ có một phương thức trừu tượng), Lambda Expressions cung cấp cú pháp ngắn gọn và rõ ràng hơn nhiều so với anonymous class.

```java
// Anonymous Class
Runnable taskAnon = new Runnable() {
    @Override
    public void run() {
        System.out.println("Chạy bằng Anonymous Class");
    }
};

// Lambda Expression
Runnable taskLambda = () -> System.out.println("Chạy bằng Lambda Expression");

// Tương tự với ActionListener
button.addActionListener(e -> JOptionPane.showMessageDialog(frame, "Click bằng Lambda!"));
```

**Khi nào vẫn dùng Anonymous Class?**

  * Khi cần triển khai interface có nhiều hơn một phương thức trừu tượng.
  * Khi cần kế thừa từ một abstract class (hoặc lớp cụ thể) và override phương thức.
  * Khi cần tạo một đối tượng có trạng thái riêng (biến thành viên) bên trong lớp nặc danh (dù ít phổ biến).

## 6\. Nguyên tắc Thiết kế Hướng Đối tượng Liên quan

Khi làm việc với `abstract class` và `interface`, việc tuân thủ các nguyên tắc thiết kế tốt là rất quan trọng:

  * **Tính Gắn kết (Cohesion):** Mỗi lớp hoặc interface nên có một trách nhiệm rõ ràng và duy nhất. Tránh tạo ra các lớp/interface "làm tất cả". Ví dụ, interface `Flyable` chỉ nên định nghĩa hành vi bay, không nên thêm các phương thức về ăn uống.
  * **Tính Bao đóng (Encapsulation):** Che giấu chi tiết cài đặt bên trong. Sử dụng `private` cho các biến thành viên trong abstract class và cung cấp getters/setters khi cần thiết. Interface vốn đã trừu tượng hóa cài đặt.
  * **Tính Nhất quán (Consistency):** Tuân thủ các quy ước đặt tên của Java (ví dụ: tên interface thường là tính từ - `Runnable`, `Comparable`; tên abstract class thường là danh từ - `AbstractList`, `NumberFormat`). Giữ cấu trúc mã nhất quán.
  * **Tính Rõ ràng (Clarity):** Viết mã dễ đọc, dễ hiểu. Sử dụng tên biến, tên phương thức có ý nghĩa. Cung cấp tài liệu (Javadoc) cho các API công khai (public methods/interfaces/classes).
  * **Ưu tiên Kết tập hơn Kế thừa (Composition over Inheritance):** Mặc dù kế thừa (đặc biệt với abstract class) rất hữu ích, nhưng lạm dụng có thể tạo ra hệ thống phân cấp cứng nhắc, khó thay đổi. Hãy cân nhắc sử dụng **kết tập (aggregation)** hoặc **bao hàm (composition)** - một lớp *chứa* (has-a) tham chiếu đến đối tượng của lớp khác - để đạt được sự linh hoạt cao hơn. Ví dụ, thay vì `Sloth extends Animal`, có thể `Animal` có một tham chiếu đến một đối tượng `MovementStrategy` (có thể là interface với các cài đặt `WalkSlowly`, `FlyFast`).
  * **Nguyên tắc Phân tách Interface (Interface Segregation Principle - ISP):** Client không nên bị buộc phải phụ thuộc vào các phương thức interface mà chúng không sử dụng. Hãy tạo ra các interface nhỏ, tập trung thay vì một interface lớn, cồng kềnh.

## 7\. Best Practices và Các Lỗi Thường Gặp (Pitfalls)

### 7.1. Best Practices

  * **Ưu tiên Interface:** Nếu bạn chỉ cần định nghĩa một hợp đồng về hành vi mà không cần chia sẻ mã cài đặt hoặc trạng thái, hãy ưu tiên sử dụng `interface`. Nó linh hoạt hơn do hỗ trợ đa kế thừa kiểu.
  * **Sử dụng Abstract Class cho Mã Chung:** Chọn `abstract class` khi bạn có một nhóm lớp liên quan chặt chẽ cần chia sẻ các phương thức đã cài đặt hoặc các biến thành viên non-static/non-final.
  * **Giữ Interface Nhỏ và Tập trung (ISP):** Chia nhỏ các interface lớn thành các interface nhỏ hơn, mỗi cái định nghĩa một tập hợp hành vi liên quan.
  * **Sử dụng `@Override`:** Luôn sử dụng chú thích `@Override` khi cài đặt phương thức từ interface hoặc abstract class. Nó giúp trình biên dịch kiểm tra lỗi (ví dụ: gõ sai tên phương thức) và làm mã nguồn rõ ràng hơn.
  * **Cung cấp Javadoc:** Viết tài liệu rõ ràng cho các abstract class, interface và các phương thức trừu tượng/công khai của chúng để người khác dễ dàng hiểu và sử dụng API của bạn.
  * **Cẩn trọng với `default` Methods:** `default` methods rất hữu ích để phát triển interface mà không phá vỡ tương thích ngược, nhưng đừng lạm dụng chúng để biến interface thành một "abstract class trá hình". Giữ mục đích chính của interface là định nghĩa hợp đồng.
  * **Xem xét Functional Interfaces và Lambdas:** Nếu interface chỉ có một phương thức trừu tượng, hãy đánh dấu nó bằng `@FunctionalInterface` và tận dụng cú pháp Lambda ngắn gọn khi sử dụng.

### 7.2. Lỗi Thường Gặp (Pitfalls)

  * **Quên Implement Methods:** Lỗi biên dịch phổ biến nhất là một lớp cụ thể `implements` interface hoặc `extends` abstract class nhưng quên cài đặt *tất cả* các phương thức trừu tượng bắt buộc.
  * **Chọn Sai Công Cụ:** Sử dụng `abstract class` khi `interface` là đủ (và ngược lại), dẫn đến thiết kế kém linh hoạt hoặc phức tạp không cần thiết.
  * **Vi phạm Liskov Substitution Principle (LSP):** Khi override phương thức trong lớp con (từ abstract class), đảm bảo rằng lớp con vẫn tuân thủ "hợp đồng" của lớp cha, không làm thay đổi hành vi một cách bất ngờ khiến nó không thể thay thế được cho lớp cha.
  * **Hệ thống Kế thừa Quá Sâu:** Tạo ra chuỗi kế thừa quá dài (`A extends B extends C extends D...`) làm hệ thống trở nên cứng nhắc, khó hiểu và khó bảo trì. Ưu tiên cấu trúc phẳng hơn hoặc sử dụng composition.
  * **Xung đột `default` Method:** Khi một lớp implement nhiều interface có default methods trùng tên, hoặc kế thừa lớp cha và implement interface có default method trùng tên, bạn sẽ gặp lỗi biên dịch nếu không tự mình `@Override` để giải quyết sự nhập nhằng.
  * **Nhầm lẫn `extends` và `implements`:** Nhớ rằng lớp `extends` lớp, lớp `implements` interface, interface `extends` interface.

## 8\. Kết luận

`Abstract Class` và `Interface` là hai công cụ mạnh mẽ trong Java để đạt được tính trừu tượng và xây dựng các hệ thống phần mềm linh hoạt, có khả năng mở rộng và bảo trì.

  * **Abstract Class** phù hợp khi bạn cần định nghĩa một khuôn mẫu chung với một số cài đặt sẵn và trạng thái cho một nhóm lớp con có quan hệ "is-a" chặt chẽ.
  * **Interface** là lựa chọn lý tưởng để định nghĩa các "hợp đồng" hành vi (quan hệ "can-do"), cho phép đa kế thừa kiểu và tạo ra các thiết kế tách rời, linh hoạt.