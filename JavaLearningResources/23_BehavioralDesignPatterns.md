# Behavioral Design Patterns (Mẫu Thiết kế Hành vi)

## 1\. Giới thiệu

Trong kỹ thuật phần mềm, việc các đối tượng tương tác và giao tiếp với nhau một cách hiệu quả là yếu tố then chốt để xây dựng nên các hệ thống phức tạp và linh hoạt. **Behavioral Design Patterns (Mẫu Thiết kế Hành vi)** tập trung vào việc xác định các mẫu giao tiếp phổ biến giữa các đối tượng, cách chúng cộng tác và phân chia trách nhiệm để hoàn thành một tác vụ nào đó. Những mẫu này không chỉ giúp đơn giản hóa luồng điều khiển phức tạp mà còn tăng cường tính linh hoạt và khả năng tái sử dụng của mã nguồn.

Khác với Creational Patterns (tập trung vào việc khởi tạo đối tượng) và Structural Patterns (tập trung vào cấu trúc lớp và đối tượng), Behavioral Patterns quan tâm đến *thuật toán và sự phân công trách nhiệm giữa các đối tượng*. Chúng mô tả cách các đối tượng khác nhau tương tác để đạt được một mục tiêu chung, đồng thời cố gắng giảm thiểu sự phụ thuộc chặt chẽ (tight coupling) giữa chúng.

Bài viết này sẽ đi sâu vào các Behavioral Design Patterns phổ biến, đặc biệt là Command, Observer, Template Method và Strategy. Chúng ta sẽ khám phá khái niệm, cơ chế hoạt động, cung cấp ví dụ mã nguồn bằng Java, phân tích ưu nhược điểm, các trường hợp ứng dụng thực tế và những phương pháp thực hành tốt nhất.

## 2\. Behavioral Design Patterns là gì?

**Behavioral Design Patterns** là nhóm các mẫu thiết kế liên quan đến thuật toán và việc gán trách nhiệm giữa các đối tượng. Chúng mô tả các mẫu truyền thông tin phức tạp giữa các đối tượng và cách các đối tượng hợp tác để thực hiện các nhiệm vụ. Mục tiêu chính của các mẫu này là tăng tính linh hoạt trong việc thực hiện các hành động này.

**Lợi ích chính của việc sử dụng Behavioral Design Patterns:**

  * **Tăng tính linh hoạt:** Dễ dàng thay đổi thuật toán hoặc cách các đối tượng tương tác mà không ảnh hưởng lớn đến các phần khác của hệ thống.
  * **Giảm sự phụ thuộc (Loose Coupling):** Các đối tượng tương tác với nhau một cách lỏng lẻo hơn, giúp hệ thống dễ bảo trì và mở rộng.
  * **Đóng gói hành vi:** Các hành vi hoặc thuật toán có thể được đóng gói thành các đối tượng riêng biệt, giúp quản lý và tái sử dụng dễ dàng hơn.
  * **Cải thiện khả năng giao tiếp:** Cung cấp một vốn từ vựng chung và các giải pháp đã được kiểm chứng cho các vấn đề giao tiếp phổ biến giữa các đối tượng.
  * **Phân chia trách nhiệm rõ ràng:** Giúp xác định rõ ràng đối tượng nào chịu trách nhiệm cho hành vi nào.

Một số Behavioral Design Patterns thông dụng bao gồm:

  * Command Pattern
  * Observer Pattern
  * Strategy Pattern
  * Template Method Pattern
  * Chain of Responsibility Pattern
  * Iterator Pattern
  * Mediator Pattern
  * Memento Pattern
  * State Pattern
  * Visitor Pattern

Chúng ta sẽ tập trung vào bốn mẫu đầu tiên, sau đó là tổng quan về một số mẫu khác.

## 3\. Command Pattern (Mẫu Lệnh)

### 3.1. Khái niệm cốt lõi

**Command Pattern** là một mẫu thiết kế hành vi, trong đó một yêu cầu (request) được đóng gói thành một đối tượng độc lập (`Command object`). Điều này cho phép bạn tham số hóa các client với các yêu cầu khác nhau, xếp hàng các yêu cầu, ghi log các yêu cầu, cũng như hỗ trợ các thao tác có thể hoàn tác (undoable operations).

Về cơ bản, Command Pattern tách biệt đối tượng gửi yêu cầu (Invoker) khỏi đối tượng thực sự thực hiện yêu cầu (Receiver). Invoker không cần biết gì về hoạt động cụ thể được yêu cầu hay đối tượng Receiver. Nó chỉ cần biết cách kích hoạt một Command.

**Các thành phần chính:**

  * **Command:** Interface hoặc lớp trừu tượng định nghĩa một phương thức (thường là `execute()`) để thực hiện một hành động. Đây là đối tượng lưu giữ yêu cầu và có thể cả trạng thái liên quan tại một thời điểm.
  * **ConcreteCommand (Lệnh cụ thể):** Lớp triển khai interface `Command`. Nó liên kết một đối tượng `Receiver` với một hành động cụ thể trên `Receiver` đó. Khi phương thức `execute()` được gọi, `ConcreteCommand` sẽ gọi phương thức tương ứng trên `Receiver`. Nó cũng có thể lưu trữ các tham số cần thiết cho hành động.
  * **Client (Máy khách):** Tạo ra một đối tượng `ConcreteCommand` và thiết lập `Receiver` cho nó (nếu cần).
  * **Invoker (Người gọi/Kích hoạt):** Đối tượng yêu cầu `Command` thực hiện một yêu cầu. `Invoker` không biết về `Receiver` cụ thể, chỉ biết về interface `Command`. Nó có thể lưu trữ một hoặc nhiều `Command` và quyết định khi nào thực hiện chúng.
  * **Receiver (Người nhận):** Đối tượng thực sự thực hiện công việc khi phương thức `execute()` của `Command` được gọi. Nó chứa logic nghiệp vụ thực tế.

### 3.2. Cơ chế hoạt động bên trong

1.  Client tạo một đối tượng `ConcreteCommand` và cung cấp cho nó một `Receiver` (đối tượng sẽ thực hiện hành động) cùng với các tham số cần thiết.
2.  Client chuyển đối tượng `Command` này cho `Invoker`.
3.  Vào một thời điểm thích hợp, `Invoker` gọi phương thức `execute()` trên đối tượng `Command`.
4.  Đối tượng `Command` (cụ thể là `ConcreteCommand`) sau đó gọi một hoặc nhiều phương thức trên `Receiver` của nó để thực hiện yêu cầu.

### 3.3. Ví dụ mã nguồn (Java)

Hãy xem xét một ví dụ về điều khiển từ xa (Remote Control) cho các thiết bị điện tử.

```java
// Receiver interface (optional, receivers can be any class)
interface ElectronicDevice {
    void turnOn();
    void turnOff();
    void volumeUp();
    void volumeDown();
}

// Concrete Receivers
class Television implements ElectronicDevice {
    private String location;
    public Television(String location) { this.location = location; }

    @Override public void turnOn() { System.out.println(location + " Television is ON"); }
    @Override public void turnOff() { System.out.println(location + " Television is OFF"); }
    @Override public void volumeUp() { System.out.println(location + " Television Volume UP"); }
    @Override public void volumeDown() { System.out.println(location + " Television Volume DOWN"); }
}

class StereoSystem implements ElectronicDevice {
    private String location;
    public StereoSystem(String location) { this.location = location; }

    @Override public void turnOn() { System.out.println(location + " Stereo is ON"); }
    @Override public void turnOff() { System.out.println(location + " Stereo is OFF"); }
    @Override public void volumeUp() { System.out.println(location + " Stereo Volume UP"); }
    @Override public void volumeDown() { System.out.println(location + " Stereo Volume DOWN"); }
}

// Command interface
interface Command {
    void execute();
    void undo(); // For supporting undo operations
}

// ConcreteCommands
class TurnOnCommand implements Command {
    private ElectronicDevice device;
    public TurnOnCommand(ElectronicDevice device) { this.device = device; }
    @Override public void execute() { device.turnOn(); }
    @Override public void undo() { device.turnOff(); } // Undo for turning on is turning off
}

class TurnOffCommand implements Command {
    private ElectronicDevice device;
    public TurnOffCommand(ElectronicDevice device) { this.device = device; }
    @Override public void execute() { device.turnOff(); }
    @Override public void undo() { device.turnOn(); }
}

class VolumeUpCommand implements Command {
    private ElectronicDevice device;
    public VolumeUpCommand(ElectronicDevice device) { this.device = device; }
    @Override public void execute() { device.volumeUp(); }
    @Override public void undo() { device.volumeDown(); }
}

// Null Object Pattern for empty slots (optional but good practice)
class NoCommand implements Command {
    @Override public void execute() { System.out.println("No command assigned."); }
    @Override public void undo() { System.out.println("No command to undo."); }
}


// Invoker
class RemoteControl {
    private Command[] onCommands;
    private Command[] offCommands;
    private Command undoCommand; // To store the last executed command

    public RemoteControl() {
        onCommands = new Command[7]; // 7 slots for ON commands
        offCommands = new Command[7]; // 7 slots for OFF commands
        Command noCommand = new NoCommand();
        for (int i = 0; i < 7; i++) {
            onCommands[i] = noCommand;
            offCommands[i] = noCommand;
        }
        undoCommand = noCommand;
    }

    public void setCommand(int slot, Command onCommand, Command offCommand) {
        if (slot >= 0 && slot < 7) {
            onCommands[slot] = onCommand;
            offCommands[slot] = offCommand;
        } else {
            System.out.println("Invalid slot number.");
        }
    }

    public void onButtonWasPushed(int slot) {
        if (slot >= 0 && slot < 7) {
            onCommands[slot].execute();
            undoCommand = onCommands[slot];
        } else {
            System.out.println("Invalid slot number for ON button.");
        }
    }

    public void offButtonWasPushed(int slot) {
         if (slot >= 0 && slot < 7) {
            offCommands[slot].execute();
            undoCommand = offCommands[slot];
        } else {
            System.out.println("Invalid slot number for OFF button.");
        }
    }

    public void undoButtonWasPushed() {
        System.out.print("Undo: ");
        undoCommand.undo();
        undoCommand = new NoCommand(); // Reset undo command after use
    }

    @Override
    public String toString() {
        StringBuilder stringBuff = new StringBuilder();
        stringBuff.append("\n------ Remote Control -------\n");
        for (int i = 0; i < onCommands.length; i++) {
            stringBuff.append("[slot " + i + "] " + onCommands[i].getClass().getSimpleName())
                .append("    " + offCommands[i].getClass().getSimpleName() + "\n");
        }
        stringBuff.append("[undo] " + undoCommand.getClass().getSimpleName() + "\n");
        stringBuff.append("---------------------------\n");
        return stringBuff.toString();
    }
}

// Client
public class RemoteControlDemo {
    public static void main(String[] args) {
        RemoteControl remote = new RemoteControl();

        // Create devices (Receivers)
        Television livingRoomTV = new Television("Living Room");
        StereoSystem kitchenStereo = new StereoSystem("Kitchen");

        // Create commands
        Command livingRoomTVOn = new TurnOnCommand(livingRoomTV);
        Command livingRoomTVOff = new TurnOffCommand(livingRoomTV);
        Command livingRoomTVVolumeUp = new VolumeUpCommand(livingRoomTV);

        Command kitchenStereoOn = new TurnOnCommand(kitchenStereo);
        Command kitchenStereoOff = new TurnOffCommand(kitchenStereo);
        // Command kitchenStereoVolumeUp = new VolumeUpCommand(kitchenStereo); // Example unused

        // Set commands to remote slots
        remote.setCommand(0, livingRoomTVOn, livingRoomTVOff);
        remote.setCommand(1, kitchenStereoOn, kitchenStereoOff);
        remote.setCommand(2, livingRoomTVVolumeUp, new NoCommand()); // Example of using NoCommand

        System.out.println(remote);

        // Press buttons
        remote.onButtonWasPushed(0); // Living Room TV ON
        remote.offButtonWasPushed(0); // Living Room TV OFF
        remote.undoButtonWasPushed(); // Undo TV OFF -> TV ON

        remote.onButtonWasPushed(1); // Kitchen Stereo ON
        remote.undoButtonWasPushed(); // Undo Stereo ON -> Stereo OFF

        remote.onButtonWasPushed(2); // Living Room TV Volume UP
        remote.undoButtonWasPushed(); // Undo Volume UP -> Volume DOWN
    }
}
```

**Giải thích mã nguồn:**

  * `ElectronicDevice`: Interface (tùy chọn) cho các thiết bị.
  * `Television`, `StereoSystem`: Các lớp `Receiver` cụ thể.
  * `Command`: Interface `Command` với phương thức `execute()` và `undo()`.
  * `TurnOnCommand`, `TurnOffCommand`, `VolumeUpCommand`: Các lớp `ConcreteCommand`. Mỗi lớp giữ một tham chiếu đến một `ElectronicDevice` và triển khai `execute()` bằng cách gọi phương thức tương ứng trên device đó. `undo()` thực hiện hành động ngược lại.
  * `NoCommand`: Một ví dụ về Null Object Pattern, dùng cho các slot không có lệnh.
  * `RemoteControl`: Lớp `Invoker`. Nó có các "slot" để gán các `Command`. Khi một nút được nhấn (`onButtonWasPushed`, `offButtonWasPushed`), nó gọi `execute()` trên `Command` tương ứng và lưu lại command đó để có thể `undo`.
  * `RemoteControlDemo`: Client tạo các `Receiver`, `Command`, và `Invoker`, sau đó thiết lập và sử dụng `Invoker`.

### 3.4. Trường hợp sử dụng thực tế

  * **Triển khai chức năng Hoàn tác/Làm lại (Undo/Redo):** Bằng cách lưu trữ một lịch sử các đối tượng `Command` đã thực thi.
  * **Hệ thống menu đồ họa (GUI Menus):** Mỗi mục menu có thể là một `Invoker` kích hoạt một `Command`.
  * **Xếp hàng tác vụ (Task Queuing):** Các `Command` có thể được đưa vào một hàng đợi và được xử lý bởi một worker thread hoặc một service nền.
  * **Giao dịch (Transactions):** Một chuỗi các `Command` có thể được nhóm lại thành một giao dịch. Nếu một `Command` thất bại, tất cả các `Command` trước đó trong giao dịch có thể được hoàn tác.
  * **Wizard (Trình hướng dẫn từng bước):** Mỗi bước trong wizard có thể được biểu diễn bằng một `Command`.
  * **Macro recording:** Ghi lại một chuỗi các hành động của người dùng thành các `Command` và sau đó phát lại chúng.
  * **Các tác vụ cần thực hiện từ xa hoặc theo lịch trình.**

### 3.5. Ưu điểm và Nhược điểm

**Ưu điểm:**

  * **Tách biệt Invoker và Receiver:** `Invoker` không cần biết về `Receiver` cụ thể hoặc cách hành động được thực hiện.
  * **Dễ dàng thêm Command mới:** Có thể thêm các `ConcreteCommand` mới mà không cần thay đổi mã nguồn của `Invoker` hoặc `Receiver` hiện có (tuân thủ Open/Closed Principle).
  * **Hỗ trợ Undo/Redo:** Việc lưu trữ và gọi `undo()` trên các `Command` giúp triển khai chức năng này một cách tự nhiên.
  * **Hỗ trợ các thao tác phức hợp (Composite Commands):** Một `Command` có thể bao gồm một tập hợp các `Command` khác.
  * **Có thể trì hoãn hoặc xếp hàng việc thực thi:** Vì yêu cầu là một đối tượng, nó có thể được lưu trữ và thực thi sau.

**Nhược điểm:**

  * **Tăng số lượng lớp:** Mỗi hành động cụ thể có thể yêu cầu một lớp `ConcreteCommand` riêng, dẫn đến tăng số lượng lớp trong hệ thống.
  * **Độ phức tạp có thể tăng:** Nếu có quá nhiều `Command` nhỏ, việc quản lý có thể trở nên phức tạp.
  * **Mỗi `Command` có thể chỉ liên kết với một `Receiver` cụ thể:** (Mặc dù `Receiver` có thể được tham số hóa).

### 3.6. Mẹo thực chiến và Phương pháp hay nhất

  * **Sử dụng Command cho các hành động có thể hoàn tác hoặc cần được xếp hàng.**
  * **`Invoker` nên giữ một tham chiếu đến `Command` trừu tượng, không phải `ConcreteCommand`.**
  * **Cân nhắc sử dụng Null Object Pattern** cho các `Command` rỗng hoặc không xác định để tránh kiểm tra `null`.
  * **`Command` có thể lưu trạng thái cần thiết** để thực hiện hành động hoặc hoàn tác nó.
  * **`Receiver` có thể là bất kỳ lớp nào** có logic nghiệp vụ cần được kích hoạt. Không nhất thiết phải có một interface `Receiver` chung.
  * **Đối với các hành động đơn giản không cần undo hoặc queuing, Command Pattern có thể là quá mức cần thiết.**

### 3.7. Lỗi phổ biến và Cách khắc phục

  * **Lớp `Command` quá lớn hoặc làm quá nhiều việc:**
      * **Vấn đề:** Một `ConcreteCommand` chứa logic nghiệp vụ phức tạp thay vì chỉ ủy thác cho `Receiver`.
      * **Khắc phục:** Logic nghiệp vụ chính nên nằm trong `Receiver`. `Command` chỉ nên đóng gói lời gọi đến `Receiver`.
  * **Khó khăn trong việc quản lý trạng thái cho Undo:**
      * **Vấn đề:** Việc khôi phục trạng thái trước đó của `Receiver` để thực hiện `undo` có thể phức tạp.
      * **Khắc phục:** `Command` cần lưu trữ đủ thông tin (có thể là một đối tượng Memento) để `Receiver` có thể khôi phục trạng thái.
  * **Lạm dụng Pattern:**
      * **Vấn đề:** Sử dụng Command Pattern cho mọi tương tác phương thức đơn giản, làm tăng sự phức tạp không cần thiết.
      * **Khắc phục:** Chỉ áp dụng khi các lợi ích (decoupling, undo, queuing) thực sự cần thiết.

## 4\. Observer Pattern (Mẫu Quan Sát)

### 4.1. Khái niệm cốt lõi

**Observer Pattern** là một mẫu thiết kế hành vi, trong đó một đối tượng, được gọi là **Subject (Chủ thể)**, duy trì một danh sách các đối tượng phụ thuộc nó, được gọi là **Observers (Người quan sát)**, và tự động thông báo cho chúng về bất kỳ thay đổi trạng thái nào, thường bằng cách gọi một trong các phương thức của chúng.

Mẫu này thiết lập một mối quan hệ một-nhiều (one-to-many) giữa các đối tượng. Khi trạng thái của đối tượng "một" (Subject) thay đổi, tất cả các đối tượng "nhiều" (Observers) phụ thuộc vào nó sẽ được cập nhật tự động.

**Các thành phần chính:**

  * **Subject (Chủ thể):** Interface hoặc lớp trừu tượng định nghĩa các phương thức để đăng ký (`attach` hoặc `register`), hủy đăng ký (`detach` hoặc `unregister`), và thông báo (`notify`) cho các Observer.
  * **ConcreteSubject (Chủ thể cụ thể):** Lớp triển khai interface `Subject`. Nó lưu trữ trạng thái quan tâm của các `Observer` và gửi thông báo cho các `Observer` khi trạng thái của nó thay đổi. Nó cũng duy trì danh sách các `Observer`.
  * **Observer (Người quan sát):** Interface hoặc lớp trừu tượng định nghĩa một phương thức cập nhật (`update()`) mà `Subject` sẽ gọi khi trạng thái của nó thay đổi.
  * **ConcreteObserver (Người quan sát cụ thể):** Lớp triển khai interface `Observer`. Mỗi `ConcreteObserver` đăng ký với một `ConcreteSubject` để nhận thông báo. Khi phương thức `update()` được gọi, nó có thể truy vấn `Subject` để lấy trạng thái mới và thực hiện các hành động tương ứng. Nó duy trì một tham chiếu đến `ConcreteSubject` và lưu trữ trạng thái cần thiết để đồng bộ với `Subject`.

### 4.2. Cơ chế hoạt động bên trong

1.  Các `ConcreteObserver` đăng ký (attach) với một `ConcreteSubject`.
2.  Khi trạng thái của `ConcreteSubject` thay đổi (thường thông qua một lời gọi phương thức làm thay đổi trạng thái, ví dụ `setState()`), `ConcreteSubject` sẽ gọi phương thức `notify()` của nó.
3.  Phương thức `notify()` duyệt qua danh sách tất cả các `Observer` đã đăng ký và gọi phương thức `update()` trên mỗi `Observer`.
4.  Trong phương thức `update()`, mỗi `ConcreteObserver` có thể lấy trạng thái mới từ `Subject` (pull model) hoặc nhận trạng thái mới như một phần của thông báo (push model) và thực hiện các hành động cập nhật cần thiết.

**Push Model vs. Pull Model:**

  * **Push Model:** `Subject` gửi chi tiết về sự thay đổi (hoặc toàn bộ trạng thái mới) cho `Observer` như một phần của phương thức `update()`. Điều này có thể hiệu quả nếu `Observer` luôn cần thông tin đó, nhưng có thể gửi quá nhiều thông tin nếu `Observer` chỉ quan tâm đến một phần nhỏ.
  * **Pull Model:** `Subject` chỉ thông báo rằng có sự thay đổi. `Observer` sau đó tự "kéo" (pull) dữ liệu cần thiết từ `Subject` bằng cách gọi các phương thức `getState()` trên `Subject`. Điều này linh hoạt hơn cho `Observer` nhưng có thể kém hiệu quả hơn nếu mỗi `Observer` phải thực hiện nhiều lời gọi để lấy trạng thái.

### 4.3. Ví dụ mã nguồn (Java)

Ví dụ về một trạm thời tiết (`WeatherStation` - Subject) thông báo cho các thiết bị hiển thị khác nhau (`DisplayDevice` - Observers) khi nhiệt độ thay đổi.

```java
import java.util.ArrayList;
import java.util.List;

// Observer interface
interface WeatherObserver {
    void update(float temperature, float humidity, float pressure);
}

// Subject interface
interface WeatherSubject {
    void registerObserver(WeatherObserver observer);
    void removeObserver(WeatherObserver observer);
    void notifyObservers();
}

// ConcreteSubject
class WeatherStation implements WeatherSubject {
    private List<WeatherObserver> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherStation() {
        observers = new ArrayList<>();
    }

    @Override
    public void registerObserver(WeatherObserver observer) {
        if (!observers.contains(observer)) {
            observers.add(observer);
        }
    }

    @Override
    public void removeObserver(WeatherObserver observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers() {
        // Tạo bản sao để tránh ConcurrentModificationException nếu observer hủy đăng ký trong lúc update
        List<WeatherObserver> observersCopy = new ArrayList<>(observers);
        for (WeatherObserver observer : observersCopy) {
            observer.update(temperature, humidity, pressure); // Push model
        }
    }

    // Phương thức này được gọi khi có dữ liệu thời tiết mới
    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        System.out.println("\nWeatherStation: New measurements set - Temp: " + temperature +
                           "C, Humidity: " + humidity + "%, Pressure: " + pressure + " hPa");
        measurementsChanged();
    }

    private void measurementsChanged() {
        notifyObservers();
    }

    // Getters (cho Pull model, không dùng trong ví dụ push này nhưng hữu ích)
    public float getTemperature() { return temperature; }
    public float getHumidity() { return humidity; }
    public float getPressure() { return pressure; }
}

// ConcreteObservers
class CurrentConditionsDisplay implements WeatherObserver {
    private float temperature;
    private float humidity;
    private WeatherSubject weatherStation; // Để có thể hủy đăng ký

    public CurrentConditionsDisplay(WeatherSubject weatherStation) {
        this.weatherStation = weatherStation;
        weatherStation.registerObserver(this);
    }

    @Override
    public void update(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        display();
    }

    public void display() {
        System.out.println("CurrentConditionsDisplay: Temp = " + temperature + "C, Humidity = " + humidity + "%");
    }

    public void deregister() {
        weatherStation.removeObserver(this);
        System.out.println("CurrentConditionsDisplay has deregistered.");
    }
}

class StatisticsDisplay implements WeatherObserver {
    private List<Float> temperatureReadings = new ArrayList<>();
    private WeatherSubject weatherStation;

    public StatisticsDisplay(WeatherSubject weatherStation) {
        this.weatherStation = weatherStation;
        weatherStation.registerObserver(this);
    }

    @Override
    public void update(float temperature, float humidity, float pressure) {
        temperatureReadings.add(temperature);
        display();
    }

    public void display() {
        float sum = 0;
        for (float temp : temperatureReadings) {
            sum += temp;
        }
        float avgTemp = temperatureReadings.isEmpty() ? 0 : sum / temperatureReadings.size();
        System.out.println("StatisticsDisplay: Avg Temp = " + String.format("%.1f", avgTemp) + "C (based on " + temperatureReadings.size() + " readings)");
    }
     public void deregister() {
        weatherStation.removeObserver(this);
        System.out.println("StatisticsDisplay has deregistered.");
    }
}


// Client
public class WeatherStationDemo {
    public static void main(String[] args) {
        WeatherStation weatherStation = new WeatherStation(); // ConcreteSubject

        CurrentConditionsDisplay currentDisplay = new CurrentConditionsDisplay(weatherStation);
        StatisticsDisplay statisticsDisplay = new StatisticsDisplay(weatherStation);
        // Có thể thêm ForecastDisplay, v.v.

        weatherStation.setMeasurements(25.5f, 65.0f, 1012.5f);
        weatherStation.setMeasurements(26.0f, 70.0f, 1010.0f);

        // Hủy đăng ký một observer
        System.out.println("\nCurrentConditionsDisplay is deregistering...");
        currentDisplay.deregister();

        weatherStation.setMeasurements(24.8f, 60.0f, 1015.0f);
    }
}
```

**Giải thích mã nguồn:**

  * `WeatherObserver`: Interface cho các `Observer`.
  * `WeatherSubject`: Interface cho `Subject`.
  * `WeatherStation`: `ConcreteSubject`, quản lý danh sách `Observer` và thông báo cho họ khi `setMeasurements()` được gọi. Ví dụ này sử dụng Push Model. Trong `notifyObservers`, một bản sao của danh sách observers được tạo ra để tránh `ConcurrentModificationException` nếu một observer cố gắng hủy đăng ký trong quá trình lặp.
  * `CurrentConditionsDisplay`, `StatisticsDisplay`: Các `ConcreteObserver`, đăng ký với `WeatherStation` và cập nhật hiển thị của chúng khi nhận được thông báo.
  * `WeatherStationDemo`: Client tạo `Subject` và các `Observer`, sau đó thay đổi trạng thái của `Subject` để xem các `Observer` được cập nhật.

**Java có sẵn hỗ trợ:**
Trước Java 9, Java cung cấp interface `java.util.Observer` và lớp `java.util.Observable`. Tuy nhiên, `Observable` là một lớp (không phải interface), gây khó khăn cho kế thừa, và nó bị đánh dấu là `deprecated` từ Java 9. Các lựa chọn hiện đại hơn bao gồm sử dụng `java.beans.PropertyChangeListener` hoặc các thư viện Reactive Streams (ví dụ: RxJava, Project Reactor).

### 4.4. Trường hợp sử dụng thực tế

  * **Giao diện người dùng (GUI):** Các thành phần GUI (ví dụ: nút bấm, danh sách) là `Subject`, và các phần khác của ứng dụng (ví dụ: model dữ liệu, các view khác) là `Observer` lắng nghe sự kiện từ các thành phần đó.
  * **Hệ thống quản lý sự kiện (Event Management Systems):** Khi một sự kiện xảy ra, các listener (Observers) đã đăng ký sẽ được thông báo.
  * **Spreadsheet:** Các ô là `Subject`. Khi giá trị của một ô thay đổi, các ô khác phụ thuộc vào nó (Observers) sẽ được tính toán lại.
  * **RSS Feeds/Thông báo tin tức:** Người dùng (Observers) đăng ký nhận thông báo từ một nguồn tin (Subject).
  * **Mô hình Model-View-Controller (MVC) / Model-View-Presenter (MVP) / Model-View-ViewModel (MVVM):** Model là `Subject`, View là `Observer`. Khi Model thay đổi, View được thông báo để tự cập nhật.
  * **Chat rooms:** Server chat là `Subject`, các client là `Observer`.

### 4.5. Ưu điểm và Nhược điểm

**Ưu điểm:**

  * **Giảm sự phụ thuộc (Loose Coupling):** `Subject` và `Observer` không cần biết chi tiết về nhau, chỉ cần tuân theo interface chung. `Subject` chỉ biết rằng `Observer` có phương thức `update()`. `Observer` có thể được thêm hoặc bớt mà không ảnh hưởng đến `Subject`.
  * **Khả năng tái sử dụng cao:** Cả `Subject` và `Observer` đều có thể được tái sử dụng độc lập.
  * **Hỗ trợ broadcast communication:** Một `Subject` có thể thông báo cho nhiều `Observer` cùng lúc.
  * **Tính linh hoạt động:** Các `Observer` có thể được đăng ký hoặc hủy đăng ký tại runtime.

**Nhược điểm:**

  * **Thứ tự thông báo không được đảm bảo:** `Subject` thường thông báo cho các `Observer` theo một thứ tự không xác định (ví dụ, thứ tự chúng được thêm vào danh sách). Nếu thứ tự quan trọng, cần có logic bổ sung.
  * **Vấn đề "Lapsed listener" hoặc "Memory Leaks":** Nếu `Observer` không hủy đăng ký khi không còn cần thiết (và `Subject` vẫn giữ tham chiếu mạnh đến nó), `Observer` có thể không được giải phóng bởi garbage collector, gây rò rỉ bộ nhớ. Sử dụng weak references có thể giúp giải quyết vấn đề này.
  * **Cập nhật không mong muốn (Unexpected updates) / Cascading updates:** Một thay đổi nhỏ ở `Subject` có thể gây ra một chuỗi các cập nhật ở nhiều `Observer`, và những cập nhật này có thể lại kích hoạt các cập nhật khác, dẫn đến hiệu năng kém hoặc hành vi khó lường nếu không được quản lý cẩn thận.
  * **Khó gỡ lỗi:** Luồng điều khiển có thể trở nên phức tạp để theo dõi do tính chất động và phân tán của các thông báo.

### 4.6. Mẹo thực chiến và Phương pháp hay nhất

  * **Chọn giữa Push và Pull Model cẩn thận:**
      * **Push:** `Subject` chủ động gửi dữ liệu. Đơn giản hơn cho `Observer` nhưng `Subject` cần biết `Observer` cần gì.
      * **Pull:** `Observer` chủ động lấy dữ liệu. Linh hoạt hơn cho `Observer` nhưng có thể tạo thêm gánh nặng cho `Subject` nếu có nhiều `Observer` cùng lấy dữ liệu.
  * **Sử dụng Weak References cho Observers:** Để tránh memory leaks, đặc biệt khi `Subject` có vòng đời dài hơn `Observer`.
  * **Đảm bảo `Observer` hủy đăng ký (deregister) khi không còn cần thiết:** Thường thực hiện trong phương thức `dispose()` hoặc một phương thức hủy đăng ký rõ ràng của `Observer`.
  * **Cung cấp cơ chế lọc sự kiện:** Nếu `Observer` chỉ quan tâm đến một số loại thay đổi nhất định từ `Subject`, `Subject` có thể cung cấp cách để `Observer` đăng ký chỉ nhận các thông báo liên quan.
  * **Cẩn thận với đồng bộ hóa trong môi trường đa luồng:** Việc đăng ký, hủy đăng ký và thông báo `Observer` cần được thực hiện một cách an toàn nếu `Subject` được truy cập bởi nhiều luồng. Tạo bản sao của danh sách observers trước khi lặp để thông báo là một cách tốt để tránh `ConcurrentModificationException`.

### 4.7. Lỗi phổ biến và Cách khắc phục

  * **Quên hủy đăng ký Observer:**
      * **Vấn đề:** Gây memory leak vì `Subject` vẫn giữ tham chiếu đến `Observer`.
      * **Khắc phục:** Luôn có cơ chế để `Observer` tự hủy đăng ký, hoặc `Subject` sử dụng weak references.
  * **Sửa đổi danh sách Observer trong khi đang duyệt (ví dụ, trong `notifyObservers()`):**
      * **Vấn đề:** Có thể gây ra `ConcurrentModificationException` hoặc hành vi không mong muốn.
      * **Khắc phục:** Tạo một bản sao của danh sách `Observer` trước khi duyệt để thông báo, hoặc sử dụng các cấu trúc dữ liệu thread-safe cho phép sửa đổi trong khi duyệt (ví dụ: `CopyOnWriteArrayList`).
  * **Observer thực hiện các tác vụ tốn thời gian trong phương thức `update()`:**
      * **Vấn đề:** Có thể làm chặn luồng thông báo của `Subject`, ảnh hưởng đến các `Observer` khác.
      * **Khắc phục:** Nếu `Observer` cần thực hiện tác vụ dài, hãy thực hiện nó trong một luồng riêng (asynchronously) thay vì trực tiếp trong `update()`.

## 5\. Template Method Pattern (Mẫu Phương thức Khuôn mẫu)

### 5.1. Khái niệm cốt lõi

**Template Method Pattern** là một mẫu thiết kế hành vi, trong đó một phương thức trừu tượng định nghĩa "khung sườn" (skeleton) của một thuật toán trong một lớp cha (superclass), nhưng để các lớp con (subclasses) tự định nghĩa lại (override) một số bước cụ thể của thuật toán đó mà không làm thay đổi cấu trúc tổng thể của thuật toán.

Nói cách khác, lớp cha xác định các bước chung của một quy trình và thứ tự thực hiện chúng, trong khi các bước chi tiết có thể được tùy chỉnh bởi các lớp con.

**Các thành phần chính:**

  * **AbstractClass (Lớp trừu tượng):**
      * Định nghĩa một `templateMethod()` (phương thức khuôn mẫu) - đây là phương thức chính định nghĩa bộ khung của thuật toán. Phương thức này thường là `final` để các lớp con không thể override nó và thay đổi cấu trúc thuật toán.
      * `templateMethod()` sẽ gọi một loạt các phương thức khác (primitive operations hoặc hook operations) theo một thứ tự nhất định để thực hiện thuật toán.
      * Khai báo các phương thức trừu tượng (`abstract methods`) mà các lớp con phải triển khai (đây là các bước cụ thể của thuật toán).
      * Có thể cung cấp các phương thức cụ thể (concrete methods) là các phần chung của thuật toán mà các lớp con có thể sử dụng hoặc không cần override.
      * Có thể cung cấp "hook methods" - các phương thức có triển khai mặc định (thường là rỗng) mà các lớp con có thể tùy chọn override để " móc nối" (hook) hành vi tùy chỉnh vào thuật toán.
  * **ConcreteClass (Lớp cụ thể):**
      * Kế thừa từ `AbstractClass`.
      * Triển khai các phương thức trừu tượng được định nghĩa trong `AbstractClass` để cung cấp chi tiết cho các bước cụ thể của thuật toán.
      * Có thể override các hook methods nếu cần.

### 5.2. Cơ chế hoạt động bên trong

1.  Client gọi `templateMethod()` trên một instance của `ConcreteClass`.
2.  `templateMethod()` trong `AbstractClass` (mà `ConcreteClass` kế thừa) sẽ được thực thi.
3.  `templateMethod()` sẽ lần lượt gọi các phương thức (trừu tượng, cụ thể, hoặc hook) theo thứ tự đã định sẵn.
4.  Nếu một phương thức được gọi là trừu tượng hoặc được override trong `ConcreteClass`, phiên bản của `ConcreteClass` sẽ được thực thi. Nếu là phương thức cụ thể không bị override, phiên bản của `AbstractClass` sẽ được thực thi.

### 5.3. Ví dụ mã nguồn (Java)

Ví dụ về quy trình xây dựng một ngôi nhà. Các bước chung là đặt móng, xây tường, lợp mái, trang trí nội thất. Một số bước có thể khác nhau tùy loại nhà.

```java
// AbstractClass
abstract class HouseBuilder {

    // Template Method - final để lớp con không override
    public final void buildHouse() {
        buildFoundation(); // Bước chung
        buildPillars();    // Bước chung, có thể override bởi lớp con (hook)
        buildWalls();      // Bước cụ thể, lớp con phải triển khai
        buildWindows();    // Bước chung
        buildRoof();       // Bước cụ thể, lớp con phải triển khai
        paintHouse();      // Hook method, tùy chọn override
        decorateHouse();   // Bước cụ thể, lớp con phải triển khai
        System.out.println("House is built!");
    }

    // Các bước cụ thể (primitive operations) - abstract, lớp con phải triển khai
    protected abstract void buildWalls();
    protected abstract void buildRoof();
    protected abstract void decorateHouse();

    // Các bước chung (concrete methods) - đã có triển khai
    private void buildFoundation() {
        System.out.println("Building foundation with cement, iron rods and sand.");
    }

    protected void buildPillars() { // Có thể là hook nếu lớp con muốn thay đổi
        System.out.println("Building pillars with concrete and steel.");
    }

    private void buildWindows() {
        System.out.println("Installing glass windows.");
    }

    // Hook method - có triển khai mặc định, lớp con có thể override
    protected void paintHouse() {
        System.out.println("Painting house with default white color.");
    }
}

// ConcreteClass 1
class WoodenHouse extends HouseBuilder {
    @Override
    protected void buildWalls() {
        System.out.println("Building wooden walls.");
    }

    @Override
    protected void buildRoof() {
        System.out.println("Building wooden shingle roof.");
    }

    @Override
    protected void decorateHouse() {
        System.out.println("Decorating with rustic wooden furniture.");
    }

    // Override hook method
    @Override
    protected void paintHouse() {
        System.out.println("Painting house with light brown varnish.");
    }
     @Override
    protected void buildPillars() { // Override bước chung (nếu muốn)
        System.out.println("Building wooden support pillars.");
    }
}

// ConcreteClass 2
class GlassHouse extends HouseBuilder {
    @Override
    protected void buildWalls() {
        System.out.println("Building glass walls.");
    }

    @Override
    protected void buildRoof() {
        System.out.println("Building glass dome roof.");
    }

    @Override
    protected void decorateHouse() {
        System.out.println("Decorating with modern minimalist furniture.");
    }
    // Không override paintHouse(), sẽ dùng triển khai mặc định từ HouseBuilder
}


// Client
public class ConstructionDemo {
    public static void main(String[] args) {
        System.out.println("Building a Wooden House:");
        HouseBuilder woodenHouse = new WoodenHouse();
        woodenHouse.buildHouse();

        System.out.println("\nBuilding a Glass House:");
        HouseBuilder glassHouse = new GlassHouse();
        glassHouse.buildHouse();
    }
}
```

**Giải thích mã nguồn:**

  * `HouseBuilder`: Lớp `AbstractClass` định nghĩa `buildHouse()` là `templateMethod()`. Nó chứa các bước chung (`buildFoundation`, `buildWindows`) và các bước trừu tượng (`buildWalls`, `buildRoof`, `decorateHouse`) mà lớp con phải triển khai. `paintHouse()` là một hook method. `buildPillars()` cũng có thể coi là một hook hoặc một bước chung có thể tùy chỉnh.
  * `WoodenHouse`, `GlassHouse`: Các lớp `ConcreteClass` triển khai các bước xây dựng cụ thể cho từng loại nhà và tùy chọn override hook method.
  * `ConstructionDemo`: Client chỉ cần tạo instance của `ConcreteClass` và gọi `buildHouse()`.

### 5.4. Trường hợp sử dụng thực tế

  * **Frameworks:** Nhiều framework sử dụng Template Method để cho phép người dùng tùy chỉnh các phần cụ thể của một quy trình mà vẫn giữ nguyên cấu trúc chung (ví dụ: trong các framework web, vòng đời xử lý request; trong các framework kiểm thử, các phương thức `setUp()`, `tearDown()`).
  * **Xử lý tài liệu:** Một quy trình chung để mở, đọc, xử lý, và đóng tài liệu, trong đó bước "xử lý" có thể khác nhau tùy loại tài liệu.
  * **Các thuật toán có các bước chung nhưng một vài bước thay đổi:** Ví dụ, các thuật toán sắp xếp, nén dữ liệu có thể có cấu trúc chung nhưng các hàm so sánh hoặc mã hóa cụ thể có thể thay đổi.
  * **Trong `java.io.InputStream`:** Phương thức `read(byte b[], int off, int len)` là một template method. Nó gọi các phương thức trừu tượng như `read()` (đọc một byte) mà các lớp con như `FileInputStream` phải triển khai.
  * **Trong `javax.servlet.http.HttpServlet`:** Các phương thức `doGet()`, `doPost()` được gọi bởi phương thức `service()` (template method) của lớp cha.

### 5.5. Ưu điểm và Nhược điểm

**Ưu điểm:**

  * **Tái sử dụng mã nguồn:** Logic chung của thuật toán được đặt trong lớp cha và được tái sử dụng bởi tất cả các lớp con.
  * **Kiểm soát cấu trúc thuật toán:** Lớp cha kiểm soát toàn bộ cấu trúc của thuật toán, các lớp con không thể thay đổi nó.
  * **Mở rộng hành vi dễ dàng:** Các lớp con chỉ cần tập trung vào việc triển khai các bước cụ thể.
  * **Tuân thủ "Hollywood Principle" ("Don't call us, we'll call you"):** Lớp cha (framework) gọi các phương thức của lớp con (custom code), chứ không phải ngược lại.

**Nhược điểm:**

  * **Hạn chế về cấu trúc thuật toán:** Các lớp con bị ràng buộc bởi cấu trúc thuật toán đã được định nghĩa trong `templateMethod()`.
  * **Số lượng phương thức có thể tăng:** Nếu thuật toán có nhiều bước tùy chỉnh, lớp trừu tượng có thể có nhiều phương thức trừu tượng hoặc hook.
  * **Khó khăn trong việc hiểu rõ luồng thực thi:** Có thể cần phải xem xét cả lớp cha và lớp con để hiểu đầy đủ thuật toán.

### 5.6. Mẹo thực chiến và Phương pháp hay nhất

  * **Xác định rõ các bước cố định và các bước biến thể** của thuật toán. Các bước cố định nên nằm trong `templateMethod()` hoặc là các phương thức `private` / `final` trong lớp cha. Các bước biến thể là các phương thức `abstract` hoặc hook.
  * **Sử dụng access modifiers hợp lý:**
      * `templateMethod()` thường là `public final`.
      * Các phương thức trừu tượng mà lớp con phải triển khai thường là `protected abstract`.
      * Các hook methods thường là `protected` với triển khai rỗng hoặc mặc định.
      * Các bước chung chỉ dùng nội bộ trong `templateMethod()` có thể là `private`.
  * **Tài liệu hóa rõ ràng** các bước nào lớp con cần hoặc có thể override.
  * **Hook methods cung cấp tính linh hoạt cao hơn** so với việc bắt buộc lớp con phải triển khai tất cả các bước biến thể.

### 5.7. Lỗi phổ biến và Cách khắc phục

  * **Lớp con override `templateMethod()`:**
      * **Vấn đề:** Nếu `templateMethod()` không được đánh dấu là `final`, lớp con có thể vô tình hoặc cố ý override nó, làm thay đổi cấu trúc thuật toán.
      * **Khắc phục:** Luôn đánh dấu `templateMethod()` là `final`.
  * **Lạm dụng hook methods:**
      * **Vấn đề:** Quá nhiều hook methods có thể làm cho `AbstractClass` trở nên phức tạp và khó hiểu mục đích của từng hook.
      * **Khắc phục:** Chỉ tạo hook khi thực sự cần thiết cho việc tùy chỉnh.
  * **Các bước trong `templateMethod()` quá phụ thuộc lẫn nhau một cách không tường minh:**
      * **Vấn đề:** Nếu một bước được override bởi lớp con làm ảnh hưởng đến điều kiện tiên quyết của một bước khác trong lớp cha, có thể gây lỗi.
      * **Khắc phục:** Thiết kế các bước sao cho độc lập nhất có thể hoặc tài liệu hóa rõ ràng các phụ thuộc.

## 6\. Strategy Pattern (Mẫu Chiến lược)

### 6.1. Khái niệm cốt lõi

**Strategy Pattern** là một mẫu thiết kế hành vi cho phép bạn định nghĩa một họ các thuật toán, đóng gói mỗi thuật toán vào một lớp riêng biệt, và làm cho các đối tượng thuật toán này có thể hoán đổi cho nhau tại runtime. Điều này cho phép client chọn một thuật toán cụ thể để sử dụng mà không cần biết chi tiết triển khai của nó, và thuật toán có thể thay đổi độc lập với client sử dụng nó.

Nói cách khác, Strategy Pattern cho phép bạn chọn "chiến lược" thực hiện một công việc nào đó một cách linh hoạt.

**Các thành phần chính:**

  * **Strategy (Chiến lược):** Interface chung cho tất cả các thuật toán (chiến lược) được hỗ trợ. Nó khai báo một phương thức mà tất cả các `ConcreteStrategy` phải triển khai.
  * **ConcreteStrategy (Chiến lược cụ thể):** Các lớp triển khai interface `Strategy`, mỗi lớp cung cấp một thuật toán (chiến lược) cụ thể.
  * **Context (Ngữ cảnh):** Lớp sử dụng một đối tượng `Strategy`. `Context` duy trì một tham chiếu đến một đối tượng `Strategy` và giao tiếp với nó thông qua interface `Strategy`. `Context` không biết về lớp `ConcreteStrategy` cụ thể. `Context` có thể cung cấp một phương thức để client thay đổi `Strategy` đang được sử dụng.

### 6.2. Cơ chế hoạt động bên trong

1.  `Context` được cấu hình với một đối tượng `ConcreteStrategy`.
2.  Client yêu cầu `Context` thực hiện một hành động.
3.  `Context` ủy thác hành động đó cho đối tượng `Strategy` mà nó đang giữ, bằng cách gọi phương thức đã được định nghĩa trong interface `Strategy`.
4.  Đối tượng `ConcreteStrategy` thực hiện thuật toán của nó.
5.  Client có thể thay đổi `Strategy` của `Context` tại runtime.

### 6.3. Ví dụ mã nguồn (Java)

Ví dụ về việc sắp xếp một danh sách số, với các chiến lược sắp xếp khác nhau (Bubble Sort, Quick Sort).

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Collections; // Import Collections for sorting

// Strategy interface
interface SortStrategy {
    void sort(List<Integer> list);
}

// ConcreteStrategies
class BubbleSortStrategy implements SortStrategy {
    @Override
    public void sort(List<Integer> list) {
        System.out.println("Sorting using Bubble Sort");
        // Basic Bubble Sort implementation
        int n = list.size();
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - i - 1; j++) {
                if (list.get(j) > list.get(j + 1)) {
                    // Swap
                    int temp = list.get(j);
                    list.set(j, list.get(j + 1));
                    list.set(j + 1, temp);
                }
            }
        }
    }
}

class QuickSortStrategy implements SortStrategy {
    @Override
    public void sort(List<Integer> list) {
        System.out.println("Sorting using Quick Sort (using Collections.sort for simplicity)");
        // For a real QuickSort, you'd implement the algorithm here.
        // Using Java's built-in sort which is typically a highly optimized quicksort/mergesort.
        Collections.sort(list);
    }
}

// Context
class SortedList {
    private List<Integer> list;
    private SortStrategy strategy; // Reference to the strategy

    public SortedList(List<Integer> initialList) { // Changed parameter name for clarity
        this.list = new ArrayList<>(initialList); // Create a mutable copy
    }

    public void setSortStrategy(SortStrategy strategy) {
        this.strategy = strategy;
    }

    public void performSort() {
        if (strategy == null) {
            System.out.println("No sort strategy set. Using default (Java's Collections.sort).");
            Collections.sort(list);
            return;
        }
        strategy.sort(list);
    }

    public void display() {
        System.out.println(list);
    }
}

// Client
public class StrategyDemo {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(5, 1, 9, 3, 7, 2, 8, 6, 4);

        SortedList sortedList = new SortedList(numbers);

        System.out.println("Original list:");
        sortedList.display();

        // Use Bubble Sort
        System.out.println("\nSetting Bubble Sort Strategy:");
        sortedList.setSortStrategy(new BubbleSortStrategy());
        sortedList.performSort();
        System.out.println("List after Bubble Sort:");
        sortedList.display();

        // Re-initialize or use a new list for fair comparison if strategies modify in place
        // For this demo, we'll re-create to show another strategy
        SortedList sortedList2 = new SortedList(Arrays.asList(50, 10, 90, 30, 70, 20, 80, 60, 40));
        System.out.println("\nOriginal list 2:");
        sortedList2.display();

        // Use Quick Sort
        System.out.println("\nSetting Quick Sort Strategy:");
        sortedList2.setSortStrategy(new QuickSortStrategy());
        sortedList2.performSort();
        System.out.println("List after Quick Sort:");
        sortedList2.display();
    }
}
```

**Giải thích mã nguồn:**

  * `SortStrategy`: Interface `Strategy` định nghĩa phương thức `sort()`.
  * `BubbleSortStrategy`, `QuickSortStrategy`: Các lớp `ConcreteStrategy` triển khai các thuật toán sắp xếp khác nhau.
  * `SortedList`: Lớp `Context`. Nó chứa một danh sách và một tham chiếu đến `SortStrategy`. Phương thức `performSort()` ủy thác việc sắp xếp cho `strategy` hiện tại. Client có thể thay đổi chiến lược bằng `setSortStrategy()`.
  * `StrategyDemo`: Client tạo `Context`, thiết lập các `Strategy` khác nhau và yêu cầu `Context` thực hiện sắp xếp.

### 6.4. Trường hợp sử dụng thực tế

  * **Thay đổi thuật toán tại runtime:** Khi một ứng dụng cần chọn một trong nhiều biến thể của một thuật toán tại thời điểm chạy (ví dụ: dựa trên dữ liệu đầu vào, cấu hình người dùng, hoặc điều kiện hệ thống).
  * **Loại bỏ các câu lệnh điều kiện phức tạp:** Thay vì sử dụng nhiều câu lệnh `if-else` hoặc `switch` để chọn hành vi, mỗi nhánh có thể trở thành một `ConcreteStrategy`.
  * **Validation:** Các quy tắc validation khác nhau có thể được triển khai như các `Strategy`.
  * **Nén file:** Các thuật toán nén khác nhau (ZIP, RAR, GZIP) có thể là các `Strategy`.
  * **Xử lý thanh toán:** Các cổng thanh toán khác nhau (PayPal, Stripe, Credit Card) có thể được triển khai như các `Strategy`.
  * **Trong Java Collections:** Phương thức `Collections.sort(List<T> list, Comparator<? super T> c)` chấp nhận một `Comparator` (một dạng Strategy) để định nghĩa cách so sánh các phần tử, từ đó thay đổi hành vi sắp xếp.

### 6.5. Ưu điểm và Nhược điểm

**Ưu điểm:**

  * **Linh hoạt và dễ mở rộng:** Dễ dàng thêm các thuật toán (chiến lược) mới mà không cần sửa đổi `Context` hoặc các `Strategy` khác (Open/Closed Principle).
  * **Tách biệt thuật toán khỏi Context:** Logic của thuật toán nằm trong các lớp `Strategy` riêng biệt, `Context` chỉ quan tâm đến việc sử dụng chúng thông qua interface chung.
  * **Thay thế kế thừa bằng composition:** Thay vì `Context` kế thừa từ một lớp trừu tượng có nhiều biến thể thuật toán, nó sử dụng composition để tham chiếu đến một `Strategy`.
  * **Giảm các câu lệnh điều kiện:** Loại bỏ nhu cầu sử dụng các khối `if-else` hoặc `switch` lớn để chọn thuật toán.
  * **Tăng khả năng kiểm thử:** Mỗi `Strategy` có thể được kiểm thử độc lập.

**Nhược điểm:**

  * **Tăng số lượng lớp/đối tượng:** Mỗi thuật toán yêu cầu một lớp `ConcreteStrategy` riêng, có thể làm tăng số lượng đối tượng trong hệ thống.
  * **Client cần biết về các Strategy khác nhau:** Client thường phải nhận thức được sự tồn tại của các `ConcreteStrategy` khác nhau để có thể chọn và cung cấp chúng cho `Context`. (Điều này có thể được giảm thiểu bằng cách sử dụng Factory để tạo `Strategy`).
  * **Overhead giao tiếp:** Có một mức độ gián tiếp khi `Context` gọi `Strategy`. Đối với các thuật toán rất đơn giản, overhead này có thể đáng chú ý (mặc dù hiếm).

### 6.6. So sánh với các mẫu khác

  * **Strategy vs. State:**
      * Cả hai đều thay đổi hành vi của `Context` dựa trên một đối tượng khác. Cấu trúc UML của chúng rất giống nhau.
      * **Strategy** thường xử lý *cách* một đối tượng thực hiện một tác vụ cụ thể; các chiến lược thường do client cung cấp cho `Context`. `Context` không tự thay đổi `Strategy`.
      * **State** xử lý việc một đối tượng *nên làm gì* trong các trạng thái khác nhau; sự thay đổi trạng thái thường được quản lý bên trong `Context` hoặc bởi chính các đối tượng `State`. `Context` có thể tự động chuyển đổi giữa các `State`.
  * **Strategy vs. Template Method:**
      * **Template Method** sử dụng *kế thừa* để thay đổi một phần của thuật toán. Cấu trúc thuật toán là cố định.
      * **Strategy** sử dụng *composition* để thay đổi toàn bộ thuật toán. Client có thể cung cấp các đối tượng `Strategy` khác nhau cho `Context`.
  * **Strategy vs. Command:**
      * **Command** đóng gói một *hành động* hoặc *yêu cầu* thành một đối tượng.
      * **Strategy** đóng gói một *thuật toán* hoặc *cách thực hiện* một hành động.

### 6.7. Mẹo thực chiến và Phương pháp hay nhất

  * **Interface `Strategy` nên nhỏ gọn và tập trung** vào một hành vi duy nhất.
  * **Các `ConcreteStrategy` nên là stateless (không trạng thái)** nếu có thể, để chúng có thể được chia sẻ và tái sử dụng. Trạng thái cần thiết cho thuật toán nên được truyền từ `Context` vào phương thức của `Strategy`.
  * **Sử dụng Dependency Injection** để cung cấp `Strategy` cho `Context`.
  * **Cân nhắc sử dụng Lambda Expressions (trong Java 8+)** cho các `Strategy` đơn giản, thay vì tạo các lớp `ConcreteStrategy` riêng biệt.
  * **Cung cấp một `Strategy` mặc định** trong `Context` nếu hợp lý.

### 6.8. Lỗi phổ biến và Cách khắc phục

  * **Interface `Strategy` quá lớn hoặc không rõ ràng:**
      * **Vấn đề:** Interface `Strategy` định nghĩa quá nhiều phương thức, làm cho việc triển khai các `ConcreteStrategy` trở nên phức tạp.
      * **Khắc phục:** Chia nhỏ thành các interface `Strategy` chuyên biệt hơn.
  * **`ConcreteStrategy` có trạng thái (stateful):**
      * **Vấn đề:** Nếu `ConcreteStrategy` lưu trữ trạng thái qua các lần gọi và được chia sẻ giữa nhiều `Context` hoặc được sử dụng lại, có thể dẫn đến hành vi không mong muốn.
      * **Khắc phục:** Thiết kế `Strategy` là stateless. Mọi dữ liệu cần thiết cho thuật toán nên được `Context` truyền vào khi gọi phương thức của `Strategy`.
  * **Lạm dụng Pattern cho các biến thể logic quá đơn giản:**
      * **Vấn đề:** Sử dụng Strategy khi một vài câu lệnh `if-else` đơn giản là đủ, làm tăng sự phức tạp không cần thiết.
      * **Khắc phục:** Đánh giá độ phức tạp và khả năng thay đổi của các thuật toán trước khi quyết định sử dụng Strategy.

## 7\. Các Behavioral Design Patterns Khác (Tổng quan)

Ngoài bốn mẫu đã thảo luận chi tiết, còn nhiều Behavioral Patterns hữu ích khác:

### 7.1. Chain of Responsibility Pattern (Mẫu Chuỗi Trách Nhiệm)

  * **Mục đích:** Tránh việc gắn kết trực tiếp người gửi yêu cầu với người nhận yêu cầu bằng cách cho nhiều hơn một đối tượng cơ hội xử lý yêu cầu đó. Các đối tượng nhận được liên kết thành một chuỗi và yêu cầu được truyền dọc theo chuỗi cho đến khi một đối tượng xử lý nó.
  * **Ví dụ:** Hệ thống xử lý sự kiện click chuột trong GUI (sự kiện có thể được xử lý bởi button, sau đó là panel chứa button, rồi đến window); hệ thống lọc request trong web server (authentication filter, logging filter, etc.). `javax.servlet.Filter#doFilter()` là một ví dụ.

### 7.2. Iterator Pattern (Mẫu Bộ Lặp)

  * **Mục đích:** Cung cấp một cách để truy cập tuần tự các phần tử của một đối tượng tập hợp (collection) mà không cần phơi bày cấu trúc bên trong của nó (ví dụ: list, stack, tree).
  * **Ví dụ:** Tất cả các Collection Framework trong Java (`java.util.Iterator`, `java.util.ListIterator`).

### 7.3. Mediator Pattern (Mẫu Trung Gian)

  * **Mục đích:** Định nghĩa một đối tượng (`Mediator`) đóng gói cách một tập hợp các đối tượng (`Colleagues`) tương tác. Mediator thúc đẩy sự ghép nối lỏng lẻo bằng cách giữ cho các `Colleague` không tham chiếu trực tiếp đến nhau, mà chỉ thông qua `Mediator`.
  * **Ví dụ:** Một phòng chat (Mediator) nơi những người tham gia (Colleagues) gửi tin nhắn cho phòng chat, và phòng chat sẽ phân phối tin nhắn đó cho những người khác; kiểm soát không lưu (Air Traffic Controller). `java.util.Timer` (các phương thức `scheduleXXX()`) sử dụng một dạng của Mediator.

### 7.4. Memento Pattern (Mẫu Lưu Trữ)

  * **Mục đích:** Không vi phạm đóng gói, thu thập và ngoại hóa trạng thái bên trong của một đối tượng để đối tượng đó có thể được khôi phục về trạng thái này sau đó. Thường dùng để triển khai chức năng undo/redo.
  * **Ví dụ:** Một trình soạn thảo văn bản lưu trạng thái của tài liệu để có thể hoàn tác các thay đổi. Các đối tượng `java.io.Serializable` có thể mô phỏng Memento.

### 7.5. State Pattern (Mẫu Trạng Thái)

  * **Mục đích:** Cho phép một đối tượng thay đổi hành vi của nó khi trạng thái bên trong của nó thay đổi. Đối tượng sẽ có vẻ như thay đổi lớp của nó.
  * **Ví dụ:** Trạng thái của một kết nối mạng (Connecting, Connected, Disconnected); trạng thái của một đơn hàng (New, Processing, Shipped, Delivered). Hành vi của đối tượng (ví dụ, các thao tác có thể thực hiện) sẽ khác nhau tùy thuộc vào trạng thái hiện tại.

### 7.6. Visitor Pattern (Mẫu Khách)

  * **Mục đích:** Đại diện cho một thao tác được thực hiện trên các phần tử của một cấu trúc đối tượng. Visitor cho phép bạn định nghĩa một thao tác mới mà không cần thay đổi các lớp của các phần tử mà nó hoạt động trên.
  * **Ví dụ:** Một trình biên dịch có thể sử dụng Visitor để thực hiện các phân tích khác nhau (type checking, code generation) trên cây cú pháp trừu tượng (AST).

## 8\. Kết luận

Behavioral Design Patterns cung cấp các giải pháp đã được kiểm chứng cho các vấn đề phổ biến liên quan đến thuật toán và sự tương tác giữa các đối tượng. Chúng giúp quản lý sự phức tạp, tăng cường tính linh hoạt, và thúc đẩy việc thiết kế các hệ thống dễ bảo trì và mở rộng hơn.

  * **Command** đóng gói một yêu cầu thành một đối tượng.
  * **Observer** cho phép các đối tượng đăng ký và nhận thông báo về sự thay đổi trạng thái của một đối tượng khác.
  * **Template Method** định nghĩa bộ khung của một thuật toán, để các lớp con tùy chỉnh các bước cụ thể.
  * **Strategy** cho phép chọn lựa thuật toán tại runtime.

Việc hiểu rõ mục đích, cấu trúc, ưu nhược điểm và các tình huống áp dụng của từng mẫu sẽ giúp lập trình viên đưa ra các quyết định thiết kế sáng suốt, từ đó xây dựng nên những phần mềm chất lượng cao, đáp ứng tốt các yêu cầu thay đổi và phát triển trong tương lai. Cũng như các loại design pattern khác, việc áp dụng chúng cần được cân nhắc kỹ lưỡng để tránh làm phức tạp hóa vấn đề một cách không cần thiết.