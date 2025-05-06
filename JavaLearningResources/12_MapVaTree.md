# MAP VÀ TREE

Tài liệu này cung cấp một cái nhìn toàn diện và chuyên sâu về hai cấu trúc dữ liệu quan trọng trong lập trình: **Map (Ánh xạ)** và **Tree (Cây)**, đặc biệt là **Binary Search Tree (Cây tìm kiếm nhị phân - BST)**. Chúng ta sẽ khám phá các khái niệm cơ bản, các lớp triển khai phổ biến trong Java Collections Framework, cơ chế hoạt động bên trong, các thuật toán liên quan, ứng dụng thực tế và các phương pháp hay nhất khi làm việc với chúng.

## 1\. Giới thiệu

Trong khoa học máy tính, việc tổ chức và truy cập dữ liệu hiệu quả là vô cùng quan trọng. **Map** và **Tree** là hai cách tiếp cận cấu trúc dữ liệu mạnh mẽ, phục vụ cho các nhu cầu lưu trữ và truy vấn khác nhau.

  * **Map:** Là một cấu trúc dữ liệu lưu trữ các cặp **khóa-giá trị (key-value pairs)**. Nó cho phép truy cập, cập nhật và xóa dữ liệu cực kỳ nhanh chóng thông qua khóa duy nhất. Map giống như một cuốn từ điển, nơi bạn tra cứu định nghĩa (value) bằng từ khóa (key).
  * **Tree:** Là một cấu trúc dữ liệu **phi tuyến (non-linear)**, mô phỏng một hệ thống phân cấp. Dữ liệu được tổ chức trong các **nút (nodes)** có mối quan hệ cha-con. Cấu trúc cây rất phù hợp để biểu diễn các mối quan hệ phân cấp, thực hiện các thao tác tìm kiếm và sắp xếp hiệu quả (đặc biệt với các loại cây cụ thể như Cây tìm kiếm nhị phân).

Hiểu rõ Map và Tree, cùng các biến thể của chúng, là kỹ năng thiết yếu để giải quyết nhiều vấn đề phức tạp trong lập trình.

## 2\. Cấu trúc Dữ liệu Map (Ánh xạ)

### 2.1. Khái niệm Cốt lõi (Key-Value Store)

**Map** là một kiểu dữ liệu trừu tượng (ADT) lưu trữ một tập hợp các **entry (mục)**, trong đó mỗi entry là một cặp bao gồm:

  * **Khóa (Key):** Một định danh duy nhất trong Map. Không thể có hai entry với cùng một khóa. Khóa được sử dụng để truy cập giá trị tương ứng.
  * **Giá trị (Value):** Dữ liệu được liên kết với khóa. Nhiều khóa khác nhau có thể trỏ đến cùng một giá trị.

Map **không đảm bảo thứ tự** của các entry theo mặc định (trừ các triển khai cụ thể như `LinkedHashMap` và `TreeMap`). Mục đích chính của Map là cung cấp khả năng truy xuất giá trị (lookup) cực kỳ hiệu quả dựa trên khóa.

### 2.2. Interface `java.util.Map<K, V>`

Trong Java Collections Framework, `java.util.Map<K, V>` là interface gốc định nghĩa các hành vi cơ bản của một cấu trúc dữ liệu Map. `K` đại diện cho kiểu dữ liệu của Khóa, và `V` đại diện cho kiểu dữ liệu của Giá trị.

#### 2.2.1. Các Phương thức Chính

Dưới đây là một số phương thức quan trọng và thường dùng nhất của interface `Map`:

| Phương thức                              | Mô tả                                                                                                               | Giá trị trả về                                              |
| :--------------------------------------- | :------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------- |
| `V put(K key, V value)`                  | Thêm hoặc cập nhật một cặp key-value. Nếu key đã tồn tại, giá trị cũ bị ghi đè.                                     | Giá trị cũ liên kết với `key`, hoặc `null` nếu key mới.     |
| `V get(Object key)`                      | Lấy giá trị tương ứng với `key`.                                                                                    | Giá trị liên kết với `key`, hoặc `null` nếu không tìm thấy. |
| `V remove(Object key)`                   | Xóa entry có `key` được chỉ định.                                                                                   | Giá trị đã bị xóa, hoặc `null` nếu key không tồn tại.       |
| `boolean containsKey(Object key)`        | Kiểm tra xem Map có chứa `key` hay không.                                                                           | `true` nếu chứa, `false` nếu không.                         |
| `boolean containsValue(Object value)`    | Kiểm tra xem Map có chứa ít nhất một entry với `value` hay không (thường chậm hơn `containsKey`).                   | `true` nếu chứa, `false` nếu không.                         |
| `int size()`                             | Trả về số lượng entry (cặp key-value) trong Map.                                                                    | Số lượng entry.                                             |
| `boolean isEmpty()`                      | Kiểm tra xem Map có rỗng hay không.                                                                                 | `true` nếu rỗng, `false` nếu không.                         |
| `void clear()`                           | Xóa tất cả các entry khỏi Map.                                                                                      | `void`                                                      |
| `Set<K> keySet()`                        | Trả về một `Set` chứa tất cả các khóa trong Map. Thay đổi Set này sẽ ảnh hưởng đến Map (và ngược lại).              | `Set<K>`                                                    |
| `Collection<V> values()`                 | Trả về một `Collection` chứa tất cả các giá trị trong Map. Có thể chứa giá trị trùng lặp.                           | `Collection<V>`                                             |
| `Set<Map.Entry<K, V>> entrySet()`        | Trả về một `Set` chứa tất cả các entry (`Map.Entry`) trong Map. Đây là cách hiệu quả nhất để duyệt cả key và value. | `Set<Map.Entry<K, V>>`                                      |
| `V getOrDefault(Object key, V default)`  | Lấy giá trị của `key`, hoặc trả về `defaultValue` nếu `key` không tồn tại. (Từ Java 8)                              | Giá trị của `key` hoặc `defaultValue`.                      |
| `putIfAbsent(K key, V value)`            | Chỉ thêm cặp key-value nếu `key` chưa tồn tại hoặc được map tới `null`. (Từ Java 8)                                 | Giá trị hiện tại của `key`, hoặc `null` nếu được thêm.      |
| `replace(K key, V value)`                | Chỉ thay thế giá trị nếu `key` đã tồn tại. (Từ Java 8)                                                              | Giá trị cũ, hoặc `null` nếu không thay thế.                 |
| `replace(K key, V oldValue, V newValue)` | Chỉ thay thế giá trị nếu `key` tồn tại và có giá trị là `oldValue`. (Từ Java 8)                                     | `true` nếu thay thế thành công, `false` nếu không.          |

#### 2.2.2. Interface Lồng `Map.Entry<K, V>`

Mỗi cặp key-value trong Map được biểu diễn bởi một đối tượng `Map.Entry`. Interface này cung cấp các phương thức để truy cập key và value của một entry cụ thể:

  * `K getKey()`: Trả về khóa của entry này.
  * `V getValue()`: Trả về giá trị của entry này.
  * `V setValue(V value)`: Thay thế giá trị của entry này bằng `value` mới (thao tác này cũng cập nhật giá trị trong Map gốc) và trả về giá trị cũ.

Cách duyệt Map hiệu quả nhất thường là thông qua `entrySet()`:

```java
import java.util.Map;
import java.util.HashMap;

Map<String, Integer> scores = new HashMap<>();
scores.put("Alice", 95);
scores.put("Bob", 88);

// Duyệt qua entrySet
for (Map.Entry<String, Integer> entry : scores.entrySet()) {
    String name = entry.getKey();
    int score = entry.getValue();
    System.out.println(name + ": " + score);
    // Có thể thay đổi giá trị: entry.setValue(score + 1);
}

// Duyệt qua keySet (cần gọi get() thêm một lần)
// for (String name : scores.keySet()) {
//     int score = scores.get(name);
//     System.out.println(name + ": " + score);
// }

// Duyệt qua values (chỉ lấy được giá trị)
// for (int score : scores.values()) {
//     System.out.println("Score: " + score);
// }
```

### 2.3. Các Lớp Triển khai (Implementations) Phổ biến

Java cung cấp nhiều lớp cài đặt (concrete classes) cho interface `Map`, mỗi lớp có đặc điểm và hiệu năng khác nhau. Ba lớp phổ biến nhất là `HashMap`, `LinkedHashMap`, và `TreeMap`.

#### 2.3.1. `java.util.HashMap<K, V>`

  * **Cơ chế Hoạt động:**
      * `HashMap` sử dụng một cấu trúc dữ liệu gọi là **bảng băm (hash table)**. Về cơ bản, nó là một mảng các "thùng" (buckets).
      * Khi bạn `put(key, value)`, `HashMap` tính toán một **mã băm (hash code)** từ `key` bằng cách gọi phương thức `key.hashCode()`. Mã băm này sau đó được xử lý để xác định chỉ số của bucket trong mảng nơi entry sẽ được lưu trữ.
      * **Xử lý Đụng độ (Collision Handling):** Nếu nhiều khóa khác nhau có cùng mã băm (hoặc mã băm sau khi xử lý trỏ đến cùng một bucket), một **đụng độ (collision)** xảy ra. `HashMap` trong Java xử lý đụng độ bằng kỹ thuật **chuỗi riêng biệt (Separate Chaining)**. Mỗi bucket không chỉ chứa một entry mà là một cấu trúc dữ liệu khác (thường là **danh sách liên kết - linked list** hoặc từ Java 8 là **cây đỏ-đen - red-black tree** nếu danh sách quá dài) để lưu trữ tất cả các entry có cùng chỉ số bucket.
      * Khi `get(key)`, `HashMap` lại tính mã băm của `key`, tìm đúng bucket, sau đó duyệt qua danh sách/cây trong bucket đó, sử dụng phương thức `key.equals(otherKey)` để tìm chính xác entry cần lấy.
      * **Load Factor và Rehashing:** `HashMap` có một `load factor` (hệ số tải, mặc định là 0.75) và `capacity` (sức chứa ban đầu, mặc định 16). Khi số lượng entry vượt quá `capacity * load factor`, `HashMap` sẽ tự động **tăng gấp đôi capacity** và **xây dựng lại bảng băm (rehash)**, tức là tính toán lại vị trí bucket cho tất cả các entry hiện có. Quá trình này tốn kém nhưng đảm bảo hiệu năng trung bình O(1) không bị suy giảm khi Map lớn dần.
  * **Hiệu năng:**
      * Thêm (`put`), lấy (`get`), xóa (`remove`), kiểm tra (`containsKey`): Trung bình là **O(1)** (hằng số), giả định hàm băm phân tán khóa tốt và không có quá nhiều đụng độ.
      * Trường hợp xấu nhất (worst-case): **O(n)**, xảy ra khi tất cả các khóa băm vào cùng một bucket, khiến cấu trúc bên trong bucket thoái hóa thành danh sách liên kết dài hoặc cây không cân bằng.
  * **Thứ tự:** **Không đảm bảo** bất kỳ thứ tự nào khi duyệt qua các entry, key hoặc value. Thứ tự có thể thay đổi sau khi rehash.
  * **Null:** Cho phép **một khóa `null`** và **nhiều giá trị `null`**. Khóa `null` luôn được băm vào bucket 0.
  * **Thread Safety:** **Không** thread-safe. Nếu cần sử dụng trong môi trường đa luồng, hãy dùng `Collections.synchronizedMap(new HashMap< K,V >())` hoặc tốt hơn là `java.util.concurrent.ConcurrentHashMap`.
  * **Sử dụng:** Lựa chọn mặc định khi bạn cần một Map hiệu năng cao và không quan tâm đến thứ tự duyệt. Rất phổ biến để đếm tần suất, caching đơn giản, lưu trữ dữ liệu cấu hình,...
  * **Quan trọng:** Để `HashMap` hoạt động chính xác, các đối tượng được dùng làm khóa **phải cài đặt đúng cặp phương thức `hashCode()` và `equals()`**. Quy tắc:
      * Nếu `a.equals(b)` là `true` thì `a.hashCode()` phải bằng `b.hashCode()`.
      * Nếu `a.hashCode()` khác `b.hashCode()` thì `a.equals(b)` phải là `false`.
      * `hashCode()` nên trả về cùng một giá trị cho cùng một đối tượng trong suốt thời gian chạy của ứng dụng (trừ khi thông tin dùng trong `equals` thay đổi).

**Ví dụ `HashMap`:**

```java
import java.util.HashMap;
import java.util.Map;

public class HashMapExample {
    public static void main(String[] args) {
        Map<String, String> countryCapitals = new HashMap<>();

        // Thêm entry
        countryCapitals.put("Vietnam", "Hanoi");
        countryCapitals.put("USA", "Washington D.C.");
        countryCapitals.put("Japan", "Tokyo");
        countryCapitals.put("France", "Paris");
        countryCapitals.put(null, "Unknown Capital"); // Cho phép một key null
        countryCapitals.put("Germany", null);       // Cho phép value null

        // Lấy giá trị
        System.out.println("Capital of Vietnam: " + countryCapitals.get("Vietnam")); // Hanoi
        System.out.println("Capital of Germany: " + countryCapitals.get("Germany")); // null
        System.out.println("Capital of null key: " + countryCapitals.get(null));     // Unknown Capital
        System.out.println("Capital of India: " + countryCapitals.get("India"));   // null (vì key không tồn tại)

        // Kiểm tra tồn tại
        System.out.println("Contains key 'Japan'? " + countryCapitals.containsKey("Japan")); // true
        System.out.println("Contains key 'India'? " + countryCapitals.containsKey("India")); // false

        // Kích thước
        System.out.println("Map size: " + countryCapitals.size()); // 6

        // Duyệt (thứ tự không đảm bảo)
        System.out.println("\nIterating through HashMap:");
        for (Map.Entry<String, String> entry : countryCapitals.entrySet()) {
            System.out.println("Key: " + entry.getKey() + ", Value: " + entry.getValue());
        }

        // Xóa
        countryCapitals.remove("France");
        System.out.println("\nContains key 'France' after removal? " + countryCapitals.containsKey("France")); // false
    }
}
```

#### 2.3.2. `java.util.LinkedHashMap<K, V>`

  * **Cơ chế Hoạt động:**
      * Kế thừa từ `HashMap`. Nó sử dụng **bảng băm** tương tự `HashMap` để đạt hiệu năng truy cập O(1) trung bình.
      * Điểm khác biệt chính: `LinkedHashMap` duy trì thêm một **danh sách liên kết đôi (doubly linked list)** chạy qua tất cả các entry của nó. Danh sách này xác định thứ tự duyệt.
  * **Thứ tự:**
      * **Thứ tự chèn (Insertion Order - Mặc định):** Khi duyệt (`keySet`, `values`, `entrySet`), các entry sẽ xuất hiện theo thứ tự chúng được chèn vào Map lần đầu tiên. Nếu một key được `put` lại, thứ tự của nó không thay đổi.
      * **Thứ tự truy cập (Access Order):** Có thể cấu hình `LinkedHashMap` để duy trì thứ tự truy cập. Khi chế độ này được bật (bằng cách dùng constructor `LinkedHashMap(capacity, loadFactor, true)`), mỗi lần một entry được truy cập (qua `get`, `put`, `putIfAbsent`, `compute`, `computeIfAbsent`, `computeIfPresent`, hoặc `merge`), nó sẽ được **di chuyển đến cuối** danh sách liên kết. Điều này làm cho `LinkedHashMap` trở thành lựa chọn tuyệt vời để xây dựng **bộ đệm LRU (Least Recently Used) cache**. Entry ít được truy cập nhất sẽ nằm ở đầu danh sách liên kết.
  * **Hiệu năng:** Tương tự `HashMap` (O(1) trung bình cho `put`, `get`, `remove`, `containsKey`). Tuy nhiên, có chi phí bộ nhớ và thời gian cao hơn một chút do phải duy trì danh sách liên kết đôi. Việc duyệt tốn O(n).
  * **Null:** Giống `HashMap`, cho phép một khóa `null` và nhiều giá trị `null`.
  * **Thread Safety:** **Không** thread-safe. Dùng `Collections.synchronizedMap` hoặc `ConcurrentHashMap` nếu cần.
  * **Sử dụng:**
      * Khi bạn cần hiệu năng của `HashMap` nhưng đồng thời muốn **duy trì thứ tự chèn** các phần tử.
      * Xây dựng **LRU cache** đơn giản bằng cách sử dụng chế độ thứ tự truy cập và ghi đè phương thức `removeEldestEntry()`.

**Ví dụ `LinkedHashMap`:**

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class LinkedHashMapExample {
    public static void main(String[] args) {
        // 1. Thứ tự chèn (Mặc định)
        System.out.println("--- Insertion Order ---");
        Map<Integer, String> insertionOrderMap = new LinkedHashMap<>();
        insertionOrderMap.put(3, "Three");
        insertionOrderMap.put(1, "One");
        insertionOrderMap.put(2, "Two");
        insertionOrderMap.put(1, "Ein"); // Cập nhật key 1, không thay đổi thứ tự

        System.out.println("Insertion Order Map: " + insertionOrderMap);
        // Output: {3=Three, 1=Ein, 2=Two} (theo thứ tự put)

        // 2. Thứ tự truy cập (LRU Cache)
        System.out.println("\n--- Access Order (LRU Cache Example) ---");
        // true để bật accessOrder
        // Ghi đè removeEldestEntry để giới hạn kích thước cache
        final int MAX_ENTRIES = 3;
        Map<String, Integer> lruCache = new LinkedHashMap<>(MAX_ENTRIES, 0.75f, true) {
            @Override
            protected boolean removeEldestEntry(Map.Entry<String, Integer> eldest) {
                // Xóa entry cũ nhất (ít truy cập nhất) nếu size vượt quá MAX_ENTRIES
                return size() > MAX_ENTRIES;
            }
        };

        lruCache.put("A", 1);
        lruCache.put("B", 2);
        lruCache.put("C", 3);
        System.out.println("Initial Cache: " + lruCache); // {A=1, B=2, C=3}

        // Truy cập B -> B di chuyển lên cuối
        lruCache.get("B");
        System.out.println("After accessing B: " + lruCache); // {A=1, C=3, B=2}

        // Thêm D -> A (cũ nhất) bị xóa
        lruCache.put("D", 4);
        System.out.println("After adding D: " + lruCache); // {C=3, B=2, D=4}
    }
}
```

#### 2.3.3. `java.util.TreeMap<K, V>`

  * **Cơ chế Hoạt động:**
      * `TreeMap` cài đặt interface `SortedMap` và `NavigableMap`. Nó lưu trữ các entry được **sắp xếp theo thứ tự tự nhiên của khóa** hoặc theo một `Comparator` được cung cấp khi tạo Map.
      * Bên trong, `TreeMap` sử dụng một **cây đỏ-đen (Red-Black Tree)**, một loại cây tìm kiếm nhị phân tự cân bằng (self-balancing binary search tree). Cấu trúc cây này đảm bảo các thao tác cơ bản (`put`, `get`, `remove`, `containsKey`) luôn có độ phức tạp **O(log n)**.
  * **Thứ tự:** Các entry luôn được sắp xếp theo khóa. Khi duyệt (`keySet`, `values`, `entrySet`), chúng sẽ xuất hiện theo thứ tự tăng dần của khóa (dựa trên `Comparable` hoặc `Comparator`).
  * **Sắp xếp:**
      * **Thứ tự Tự nhiên:** Nếu các khóa (`K`) cài đặt interface `java.lang.Comparable`, `TreeMap` sẽ sử dụng phương thức `compareTo` của chúng để sắp xếp.
      * **Thứ tự Tùy chỉnh:** Bạn có thể cung cấp một `java.util.Comparator<K>` trong constructor của `TreeMap` để định nghĩa một thứ tự sắp xếp khác.
  * **Hiệu năng:** `put`, `get`, `remove`, `containsKey`: Đảm bảo **O(log n)** trong mọi trường hợp (worst-case, average, best-case). Chậm hơn O(1) trung bình của `HashMap` nhưng ổn định hơn (không có O(n) worst-case cho các thao tác này) và cung cấp khả năng duyệt có thứ tự.
  * **Null:**
      * **Khóa (Key):** **Không** cho phép khóa `null` nếu sử dụng thứ tự tự nhiên (vì `null` không thể so sánh). Có thể cho phép nếu `Comparator` được cung cấp có khả năng xử lý `null`.
      * **Giá trị (Value):** Cho phép giá trị `null`.
  * **Thread Safety:** **Không** thread-safe. Dùng `Collections.synchronizedSortedMap(new TreeMap< K,V >())` hoặc `java.util.concurrent.ConcurrentSkipListMap` (là một `SortedMap` thread-safe hiệu năng cao).
  * **Sử dụng:**
      * Khi bạn cần duyệt Map theo **thứ tự sắp xếp của khóa**.
      * Khi cần các thao tác của `NavigableMap` như tìm kiếm theo khoảng (range queries), tìm khóa gần nhất (`floorKey`, `ceilingKey`), lấy các sub-map (`headMap`, `tailMap`, `subMap`).
      * Khi cần đảm bảo độ phức tạp O(log n) ổn định cho các thao tác, tránh O(n) worst-case của `HashMap`.

**Ví dụ `TreeMap`:**

```java
import java.util.TreeMap;
import java.util.Map;
import java.util.Comparator;

public class TreeMapExample {
    public static void main(String[] args) {
        // 1. Sắp xếp theo thứ tự tự nhiên của khóa (String)
        System.out.println("--- Natural Order (String keys) ---");
        TreeMap<String, Integer> naturalOrderMap = new TreeMap<>();
        naturalOrderMap.put("Charlie", 30);
        naturalOrderMap.put("Alice", 25);
        naturalOrderMap.put("Bob", 35);
        // naturalOrderMap.put(null, 0); // Sẽ ném NullPointerException

        System.out.println("Natural Order Map: " + naturalOrderMap);
        // Output: {Alice=25, Bob=35, Charlie=30} (sắp xếp theo tên A->B->C)

        // 2. Sắp xếp theo Comparator (ví dụ: theo độ dài của String key)
        System.out.println("\n--- Custom Order (String length) ---");
        Comparator<String> lengthComparator = Comparator.comparingInt(String::length);
        // Hoặc: Comparator<String> lengthComparator = (s1, s2) -> Integer.compare(s1.length(), s2.length());

        TreeMap<String, Integer> customOrderMap = new TreeMap<>(lengthComparator);
        customOrderMap.put("Apple", 1); // length 5
        customOrderMap.put("Banana", 2); // length 6
        customOrderMap.put("Kiwi", 3); // length 4

        System.out.println("Custom Order Map: " + customOrderMap);
        // Output: {Kiwi=3, Apple=1, Banana=2} (sắp xếp theo độ dài key 4->5->6)

        // 3. NavigableMap features
        System.out.println("\n--- NavigableMap Features ---");
        System.out.println("First Key: " + naturalOrderMap.firstKey()); // Alice
        System.out.println("Last Key: " + naturalOrderMap.lastKey());   // Charlie
        System.out.println("Key <= 'Bob': " + naturalOrderMap.floorKey("Bob"));     // Bob
        System.out.println("Key >= 'Betty': " + naturalOrderMap.ceilingKey("Betty")); // Bob
        System.out.println("Head Map (before Bob): " + naturalOrderMap.headMap("Bob")); // {Alice=25}
        System.out.println("Tail Map (from Bob): " + naturalOrderMap.tailMap("Bob")); // {Bob=35, Charlie=30}
    }
}
```

### 2.4. Chọn Lựa Giữa Các Implementations

| Đặc điểm              | `HashMap`                     | `LinkedHashMap`                      | `TreeMap`                           |
| :-------------------- | :---------------------------- | :----------------------------------- | :---------------------------------- |
| **Cơ chế**            | Bảng băm                      | Bảng băm + Linked List               | Cây Đỏ-Đen                          |
| **Hiệu năng (Avg)**   | O(1)                          | O(1)                                 | O(log n)                            |
| **Hiệu năng (Worst)** | O(n)                          | O(n)                                 | O(log n)                            |
| **Thứ tự duyệt**      | Không đảm bảo                 | Thứ tự Chèn / Truy cập               | Thứ tự Sắp xếp Khóa                 |
| **Khóa `null`**       | Cho phép (1)                  | Cho phép (1)                         | Không (trừ khi có Comparator)       |
| **Giá trị `null`**    | Cho phép                      | Cho phép                             | Cho phép                            |
| **Thread-safe**       | Không                         | Không                                | Không                               |
| **Khi nào dùng?**     | Nhanh nhất, không cần thứ tự. | Cần thứ tự chèn/truy cập, LRU Cache. | Cần sắp xếp theo khóa, range query. |

### 2.5. Map Best Practices & Pitfalls

  * **`hashCode()` và `equals()` Contract:** Cực kỳ quan trọng đối với `HashMap` và `LinkedHashMap`. Đảm bảo cài đặt đúng cho các đối tượng key tùy chỉnh. Sử dụng các lớp immutable (bất biến) làm key là một ý tưởng tốt để tránh thay đổi `hashCode` sau khi đã đưa vào Map.
  * **Khóa Bất biến (Immutable Keys):** Nên ưu tiên sử dụng các lớp bất biến (như `String`, `Integer`, hoặc các lớp tùy chỉnh bất biến) làm khóa cho Map. Nếu một khóa bị thay đổi sau khi đã được thêm vào `HashMap`/`LinkedHashMap` theo cách làm thay đổi `hashCode` của nó, bạn có thể không tìm lại được entry đó nữa.
  * **Null Keys/Values:** `HashMap` và `LinkedHashMap` cho phép một khóa `null` và nhiều giá trị `null`. `TreeMap` không cho phép khóa `null` (trừ khi có `Comparator` đặc biệt). Luôn kiểm tra `null` khi lấy giá trị từ Map nếu `null` là một giá trị hợp lệ.
  * **Thread Safety:** Các lớp Map cơ bản (`HashMap`, `LinkedHashMap`, `TreeMap`) **không** thread-safe. Trong môi trường đa luồng:
      * Sử dụng `java.util.concurrent.ConcurrentHashMap` thay cho `HashMap`. Nó cung cấp hiệu năng tốt và an toàn luồng.
      * Sử dụng `java.util.concurrent.ConcurrentSkipListMap` thay cho `TreeMap`.
      * Hoặc dùng các wrapper `Collections.synchronizedMap()` / `Collections.synchronizedSortedMap()`, nhưng chúng thường có hiệu năng kém hơn các lớp trong `java.util.concurrent` do khóa toàn bộ Map cho mỗi thao tác.
  * **Iteration:** Cách hiệu quả nhất để duyệt qua cả khóa và giá trị là dùng `entrySet()`. Tránh lấy `keySet()` rồi gọi `get()` cho từng key bên trong vòng lặp, vì nó kém hiệu quả hơn (đặc biệt với `HashMap`, mỗi lần `get` lại tốn thêm thao tác băm và tìm kiếm).
  * **Capacity và Load Factor:** Đối với `HashMap` và `LinkedHashMap`, việc chọn `initialCapacity` phù hợp nếu bạn biết trước kích thước ước lượng có thể giúp giảm số lần rehash, cải thiện hiệu năng.

## 3\. Cấu trúc Dữ liệu Tree (Cây)

### 3.1. Khái niệm Cốt lõi

**Tree** là một cấu trúc dữ liệu phân cấp, phi tuyến, bao gồm các **nút (nodes)** chứa dữ liệu và các **cạnh (edges)** kết nối các nút đó.

  * **Nút Gốc (Root):** Nút cao nhất trong cây, không có nút cha. Mỗi cây chỉ có một nút gốc.
  * **Nút Cha (Parent):** Một nút có các nút con kết nối trực tiếp bên dưới nó.
  * **Nút Con (Child):** Một nút được kết nối trực tiếp bên dưới một nút cha.
  * **Nút Lá (Leaf):** Một nút không có bất kỳ nút con nào.
  * **Nút Anh em (Sibling):** Các nút có cùng nút cha.
  * **Cây Con (Subtree):** Một nút và tất cả các hậu duệ của nó tạo thành một cây con.
  * **Cạnh (Edge):** Liên kết giữa một nút cha và một nút con.

### 3.2. Thuật ngữ Tree

  * **Đường đi (Path):** Một chuỗi các nút và cạnh nối từ một nút đến một hậu duệ.
  * **Độ dài Đường đi (Path Length):** Số lượng cạnh trên một đường đi.
  * **Chiều sâu (Depth) của Nút:** Độ dài đường đi từ nút gốc đến nút đó. Chiều sâu của gốc là 0.
  * **Chiều cao (Height) của Nút:** Độ dài đường đi dài nhất từ nút đó đến một nút lá trong cây con của nó. Chiều cao của nút lá là 0.
  * **Chiều cao (Height) của Cây:** Chiều cao của nút gốc (hoặc độ dài đường đi dài nhất từ gốc đến một lá). Chiều cao cây rỗng thường là -1.
  * **Mức (Level) của Nút:** Thường được định nghĩa là `chiều sâu + 1`. Gốc ở mức 1.
  * **Bậc (Degree) của Nút:** Số lượng cây con (hoặc số cạnh đi ra) của nút đó.
  * **Bậc (Degree) của Cây:** Bậc lớn nhất của tất cả các nút trong cây.

### 3.3. Cây Nhị phân (Binary Tree)

**Cây nhị phân** là một loại cây đặc biệt trong đó mỗi nút có **tối đa hai nút con**, thường được gọi là **con trái (left child)** và **con phải (right child)**.

  * Cây con gốc tại con trái được gọi là **cây con trái (left subtree)**.
  * Cây con gốc tại con phải được gọi là **cây con phải (right subtree)**.

#### 3.3.1. Các loại Cây Nhị phân (Mở rộng)

  * **Cây Nhị phân Đầy đủ (Full Binary Tree):** Mỗi nút có 0 hoặc 2 con.
  * **Cây Nhị phân Hoàn chỉnh (Complete Binary Tree):** Tất cả các mức, trừ có thể là mức cuối cùng, đều được lấp đầy hoàn toàn, và tất cả các nút ở mức cuối cùng được dồn hết về bên trái. Cấu trúc này quan trọng cho việc triển khai Heap.
  * **Cây Nhị phân Hoàn hảo (Perfect Binary Tree):** Tất cả các nút trong (internal nodes) đều có 2 con và tất cả các nút lá đều ở cùng một mức.

### 3.4. Cây Tìm kiếm Nhị phân (Binary Search Tree - BST)

**Cây tìm kiếm nhị phân (BST)** là một cây nhị phân có thêm các ràng buộc về giá trị dữ liệu lưu trữ trong các nút, cho phép tìm kiếm hiệu quả:

  * Đối với mỗi nút `N`:
      * Tất cả các giá trị trong **cây con trái** của `N` đều **nhỏ hơn** (hoặc bằng, tùy định nghĩa) giá trị của `N`.
      * Tất cả các giá trị trong **cây con phải** của `N` đều **lớn hơn** (hoặc bằng, tùy định nghĩa) giá trị của `N`.
  * Cả cây con trái và cây con phải cũng phải là các cây tìm kiếm nhị phân.

**Lưu ý:** Để thuộc tính này có nghĩa, các giá trị (element) lưu trong cây phải có khả năng so sánh được (ví dụ: cài đặt `Comparable` hoặc có `Comparator`).

#### 3.4.1. Triển khai Cơ bản (Node)

Một nút trong BST thường chứa:

  * Dữ liệu (element).
  * Tham chiếu đến nút con trái (`left`).
  * Tham chiếu đến nút con phải (`right`).
  * (Tùy chọn) Tham chiếu đến nút cha (`parent`).

<!-- end list -->

```java
// Định nghĩa lớp Node cơ bản cho BST
class TreeNode<E> {
    protected E element;
    protected TreeNode<E> left;
    protected TreeNode<E> right;
    // protected TreeNode<E> parent; // Tùy chọn

    public TreeNode(E e) {
        element = e;
        left = null;
        right = null;
        // parent = null;
    }

    @Override
    public String toString() {
        // toString đơn giản để dễ hình dung
        return "Node(" + element + ")";
    }
}
```

#### 3.4.2. Các Thao tác trên BST

Giả định `E` cài đặt `Comparable<E>`.

**a) Tìm kiếm (Search)**

  * **Thuật toán:**
    1.  Bắt đầu từ nút gốc (`root`).
    2.  So sánh giá trị cần tìm (`element`) với giá trị của nút hiện tại (`current.element`).
    3.  Nếu bằng nhau: Tìm thấy, trả về `true`.
    4.  Nếu `element` nhỏ hơn `current.element`: Đi sang cây con trái (`current = current.left`).
    5.  Nếu `element` lớn hơn `current.element`: Đi sang cây con phải (`current = current.right`).
    6.  Lặp lại bước 2-5 cho đến khi tìm thấy hoặc `current` trở thành `null` (không tìm thấy).
  * **Độ phức tạp:**
      * Trung bình (cây cân bằng): O(log n).
      * Xấu nhất (cây bị lệch/thoái hóa thành danh sách): O(n).
  * **Mã nguồn (trong lớp BST):**

<!-- end list -->

```java
public boolean search(E element) {
    TreeNode<E> current = root; // root là biến thành viên của lớp BST

    while (current != null) {
        int compareResult = ((Comparable<E>) element).compareTo(current.element);

        if (compareResult < 0) {
            current = current.left; // Đi sang trái
        } else if (compareResult > 0) {
            current = current.right; // Đi sang phải
        } else {
            // compareResult == 0
            return true; // Tìm thấy
        }
    }
    return false; // Không tìm thấy
}
```

**b) Chèn (Insertion)**

  * **Thuật toán:**
    1.  Nếu cây rỗng, tạo nút mới làm gốc.
    2.  Bắt đầu từ gốc, tìm vị trí thích hợp để chèn (giống như tìm kiếm). Duy trì một con trỏ `parent` trỏ đến nút cha của nút `current`.
    3.  So sánh giá trị cần chèn (`element`) với `current.element`.
    4.  Nếu `element` nhỏ hơn: Gán `parent = current`, đi sang trái (`current = current.left`).
    5.  Nếu `element` lớn hơn: Gán `parent = current`, đi sang phải (`current = current.right`).
    6.  Nếu bằng: Không chèn (hoặc cập nhật giá trị nếu cho phép trùng khóa - thường BST không cho trùng). Trả về `false`.
    7.  Lặp lại bước 3-6 cho đến khi `current` là `null`.
    8.  Tại vị trí `current == null`, tạo nút mới với `element`.
    9.  Nối nút mới vào `parent`: Nếu `element` nhỏ hơn `parent.element`, gán `parent.left = newNode`. Ngược lại, gán `parent.right = newNode`. Tăng kích thước (`size++`). Trả về `true`.
  * **Độ phức tạp:** Tương tự tìm kiếm: O(log n) trung bình, O(n) xấu nhất.
  * **Mã nguồn (trong lớp BST):**

<!-- end list -->

```java
public boolean insert(E element) {
    if (root == null) {
        root = new TreeNode<>(element); // Cây rỗng, tạo gốc mới
        size++;
        return true;
    }

    TreeNode<E> current = root;
    TreeNode<E> parent = null;
    Comparable<E> comparableElement = (Comparable<E>) element;

    while (current != null) {
        parent = current;
        int compareResult = comparableElement.compareTo(current.element);

        if (compareResult < 0) {
            current = current.left;
        } else if (compareResult > 0) {
            current = current.right;
        } else {
            // element đã tồn tại trong cây
            return false;
        }
    }

    // Tạo nút mới và nối vào parent
    TreeNode<E> newNode = new TreeNode<>(element);
    int parentCompare = comparableElement.compareTo(parent.element);

    if (parentCompare < 0) {
        parent.left = newNode;
    } else {
        parent.right = newNode;
    }
    // newNode.parent = parent; // Nếu cần tham chiếu cha

    size++;
    return true;
}
```

**c) Xóa (Deletion)**

Đây là thao tác phức tạp nhất trong BST vì cần bảo toàn thuộc tính tìm kiếm. Có 3 trường hợp chính khi xóa nút `N`:

1.  **`N` là nút lá (không có con):** Đơn giản chỉ cần ngắt kết nối `N` khỏi nút cha của nó (gán `parent.left` hoặc `parent.right` thành `null`).
2.  **`N` có một con:** Thay thế `N` bằng nút con duy nhất của nó. Nối nút con đó trực tiếp vào nút cha của `N`.
3.  **`N` có hai con:** Trường hợp phức tạp nhất.
      * Tìm **nút thay thế** cho `N`. Có hai lựa chọn phổ biến:
          * **Phần tử lớn nhất trong cây con trái (inorder predecessor):** Đi sang trái một bước (`N.left`), sau đó đi sang phải liên tục cho đến hết (`rightmost node`).
          * **Phần tử nhỏ nhất trong cây con phải (inorder successor):** Đi sang phải một bước (`N.right`), sau đó đi sang trái liên tục cho đến hết (`leftmost node`).
      * **Sao chép giá trị:** Copy giá trị của nút thay thế vào nút `N`.
      * **Xóa nút thay thế:** Xóa nút thay thế khỏi vị trí ban đầu của nó. Lưu ý rằng nút thay thế này sẽ rơi vào trường hợp 1 (nút lá) hoặc trường hợp 2 (có một con - con trái nếu là successor, con phải nếu là predecessor), nên có thể xóa bằng cách đệ quy hoặc lặp lại quy trình xóa đơn giản hơn.

<!-- end list -->

  * **Độ phức tạp:** Tương tự tìm kiếm và chèn: O(log n) trung bình, O(n) xấu nhất.
  * **Mã nguồn:** Mã nguồn cho việc xóa khá dài và cần xử lý cẩn thận các trường hợp biên (xóa gốc, nối lại con trỏ...). Thường được cài đặt bằng phương thức trợ giúp đệ quy. (Xem thêm trong các tài liệu chuẩn về BST để có mã hoàn chỉnh).

#### 3.4.3. Duyệt Cây (Tree Traversal)

Duyệt cây là quá trình thăm (visit) tất cả các nút trong cây theo một thứ tự có hệ thống. Mỗi nút được thăm đúng một lần.

**a) Duyệt theo Chiều sâu (Depth-First Search - DFS)**

Thăm các nút càng sâu càng tốt trước khi quay lại. Có 3 kiểu DFS chính:

1.  **Inorder (Trung tự): Left -\> Node -\> Right**

      * Thăm cây con trái -\> Thăm nút hiện tại -\> Thăm cây con phải.
      * **Kết quả:** Với BST, duyệt Inorder sẽ cho ra các phần tử theo **thứ tự tăng dần**.
      * **Ứng dụng:** Lấy danh sách phần tử đã sắp xếp từ BST.
      * **Mã đệ quy:**
        ```java
        protected void inorder(TreeNode<E> node) {
            if (node == null) return;
            inorder(node.left);
            System.out.print(node.element + " "); // Thăm nút
            inorder(node.right);
        }
        public void inorderTraversal() {
            inorder(root);
            System.out.println();
        }
        ```
      * **Mã không đệ quy (dùng Stack):**
        ```java
        public void inorderIterative() {
            if (root == null) return;
            Deque<TreeNode<E>> stack = new ArrayDeque<>();
            TreeNode<E> current = root;
            while (current != null || !stack.isEmpty()) {
                // Đi hết sang trái và đẩy vào stack
                while (current != null) {
                    stack.push(current);
                    current = current.left;
                }
                // Pop từ stack, thăm nút, đi sang phải
                current = stack.pop();
                System.out.print(current.element + " "); // Thăm nút
                current = current.right;
            }
            System.out.println();
        }
        ```

2.  **Preorder (Tiền tự): Node -\> Left -\> Right**

      * Thăm nút hiện tại -\> Thăm cây con trái -\> Thăm cây con phải.
      * **Ứng dụng:** Sao chép cây (copy tree), tạo biểu thức tiền tố (prefix notation).
      * **Mã đệ quy:**
        ```java
        protected void preorder(TreeNode<E> node) {
            if (node == null) return;
            System.out.print(node.element + " "); // Thăm nút
            preorder(node.left);
            preorder(node.right);
        }
        public void preorderTraversal() {
            preorder(root);
            System.out.println();
        }
        ```
      * **Mã không đệ quy (dùng Stack):**
        ```java
        public void preorderIterative() {
            if (root == null) return;
            Deque<TreeNode<E>> stack = new ArrayDeque<>();
            stack.push(root);
            while (!stack.isEmpty()) {
                TreeNode<E> current = stack.pop();
                System.out.print(current.element + " "); // Thăm nút
                // Đẩy con phải vào trước (để xử lý sau)
                if (current.right != null) {
                    stack.push(current.right);
                }
                // Đẩy con trái vào sau (để xử lý trước)
                if (current.left != null) {
                    stack.push(current.left);
                }
            }
             System.out.println();
        }
        ```

3.  **Postorder (Hậu tự): Left -\> Right -\> Node**

      * Thăm cây con trái -\> Thăm cây con phải -\> Thăm nút hiện tại.
      * **Ứng dụng:** Xóa cây (delete tree - cần xóa con trước khi xóa cha), tạo biểu thức hậu tố (postfix notation).
      * **Mã đệ quy:**
        ```java
        protected void postorder(TreeNode<E> node) {
            if (node == null) return;
            postorder(node.left);
            postorder(node.right);
            System.out.print(node.element + " "); // Thăm nút
        }
        public void postorderTraversal() {
            postorder(root);
            System.out.println();
        }
        ```
      * **Mã không đệ quy (dùng 2 Stack hoặc 1 Stack + xử lý phức tạp hơn):** (Thường ít dùng bản không đệ quy hơn Inorder/Preorder).

**b) Duyệt theo Chiều rộng (Breadth-First Search - BFS) / Level Order Traversal**

  * Thăm các nút theo từng mức (level), từ trái sang phải.
  * **Thuật toán:** Sử dụng một **Queue**.
    1.  Thêm `root` vào queue.
    2.  Trong khi queue không rỗng:
          * Lấy nút `current` từ đầu queue (`dequeue`/`poll`).
          * Thăm nút `current`.
          * Nếu `current.left` tồn tại, thêm vào cuối queue.
          * Nếu `current.right` tồn tại, thêm vào cuối queue.
  * **Ứng dụng:** Tìm đường đi ngắn nhất trong cây không trọng số, các bài toán xử lý theo level.
  * **Mã nguồn (dùng Queue):**

<!-- end list -->

```java
import java.util.Queue;
import java.util.LinkedList; // Hoặc ArrayDeque

public void levelOrderTraversal() {
    if (root == null) return;
    Queue<TreeNode<E>> queue = new LinkedList<>(); // Hoặc ArrayDeque
    queue.offer(root); // Thêm gốc vào queue

    while (!queue.isEmpty()) {
        TreeNode<E> current = queue.poll(); // Lấy nút đầu queue
        System.out.print(current.element + " "); // Thăm nút

        // Thêm con trái (nếu có)
        if (current.left != null) {
            queue.offer(current.left);
        }
        // Thêm con phải (nếu có)
        if (current.right != null) {
            queue.offer(current.right);
        }
    }
    System.out.println();
}
```

#### 3.4.4. Cây Tìm kiếm Nhị phân Cân bằng (Balanced BST) (Mở rộng)

  * **Vấn đề của BST:** Trong trường hợp xấu nhất (ví dụ: chèn các phần tử đã sắp xếp), BST có thể bị **lệch (skewed)** và thoái hóa thành một danh sách liên kết. Khi đó, chiều cao cây trở thành O(n), và các thao tác tìm kiếm, chèn, xóa mất O(n) thay vì O(log n).
  * **Giải pháp:** Sử dụng **Cây Tìm kiếm Nhị phân Tự Cân bằng (Self-Balancing BSTs)**. Các cấu trúc cây này thực hiện các thao tác **xoay cây (rotations)** và/hoặc thay đổi màu nút (trong cây đỏ-đen) trong quá trình chèn và xóa để đảm bảo chiều cao của cây luôn được giữ ở mức O(log n).
  * **Các loại phổ biến:**
      * **AVL Tree:** Cây cân bằng nghiêm ngặt nhất, đảm bảo chiều cao của hai cây con của mọi nút chỉ chênh lệch tối đa là 1. Việc chèn/xóa có thể cần nhiều phép xoay hơn.
      * **Red-Black Tree (Cây Đỏ-Đen):** Cân bằng ít nghiêm ngặt hơn AVL tree nhưng vẫn đảm bảo chiều cao O(log n). Thường yêu cầu ít phép xoay hơn khi chèn/xóa so với AVL. **Đây là cấu trúc được sử dụng bên trong `java.util.TreeMap` và `java.util.TreeSet` của Java.**
  * Việc cài đặt cây tự cân bằng phức tạp hơn BST thông thường đáng kể. Thông thường, lập trình viên sẽ sử dụng các lớp có sẵn như `TreeMap` thay vì tự cài đặt lại.

#### 3.4.5. Ứng dụng của BST

  * **Tìm kiếm hiệu quả:** Nhanh chóng kiểm tra sự tồn tại hoặc lấy dữ liệu liên kết với một khóa.
  * **Sắp xếp:** Duyệt inorder BST cho ra dãy đã sắp xếp.
  * **Implement Set và Map:** `TreeSet` và `TreeMap` trong Java sử dụng Red-Black Tree (một dạng BST cân bằng) để lưu trữ các phần tử/entry có thứ tự và cung cấp các thao tác O(log n).
  * **Indexing trong Cơ sở Dữ liệu:** Các chỉ mục (indexes) trong CSDL thường sử dụng các cấu trúc dựa trên cây (như B-Tree, B+ Tree, là các biến thể của cây tìm kiếm nhưng tối ưu cho việc truy cập đĩa) để tăng tốc độ truy vấn.

### 3.5. Tree Best Practices & Pitfalls

  * **Cân bằng là Chìa khóa:** Đối với các ứng dụng cần hiệu năng đảm bảo, BST không cân bằng có thể trở thành thảm họa (O(n)). Hãy xem xét sử dụng các cấu trúc cây tự cân bằng có sẵn (`TreeMap`, `TreeSet`) hoặc các thư viện cung cấp chúng.
  * **Đệ quy và Độ sâu:** Các thao tác trên cây thường được cài đặt bằng đệ quy rất tự nhiên. Tuy nhiên, cẩn thận với **StackOverflowError** nếu cây quá sâu (đặc biệt là cây bị lệch). Cân nhắc sử dụng phiên bản lặp (iterative) với Stack hoặc Queue nếu độ sâu là vấn đề.
  * **Complexity:** Luôn ghi nhớ độ phức tạp thời gian O(log n) (trung bình/cân bằng) và O(n) (xấu nhất/lệch) của các thao tác BST.
  * **Comparable/Comparator:** Đảm bảo các phần tử có thể so sánh được một cách nhất quán khi sử dụng BST (hoặc `TreeMap`/`TreeSet`).

## 4\. Tổng kết

  * **Map** là cấu trúc dữ liệu key-value tối ưu cho việc truy xuất nhanh chóng dựa trên khóa. Java cung cấp `HashMap` (nhanh nhất, không thứ tự), `LinkedHashMap` (duy trì thứ tự chèn/truy cập), và `TreeMap` (duy trì thứ tự sắp xếp khóa). Việc chọn đúng implementation phụ thuộc vào yêu cầu về hiệu năng và thứ tự.
  * **Tree** là cấu trúc dữ liệu phân cấp. **Binary Search Tree (BST)** cho phép tìm kiếm, chèn, xóa hiệu quả (O(log n) trung bình) nếu cây cân bằng. Duyệt cây (Inorder, Preorder, Postorder, Level Order) cung cấp các cách khác nhau để xử lý dữ liệu. **Cây tự cân bằng** (như Red-Black Tree trong `TreeMap`) là cần thiết để đảm bảo hiệu năng O(log n) trong mọi trường hợp.

Việc nắm vững các khái niệm, cách hoạt động, và các lớp cài đặt của Map và Tree trong Java là nền tảng quan trọng để xây dựng các ứng dụng hiệu quả và có cấu trúc tốt.