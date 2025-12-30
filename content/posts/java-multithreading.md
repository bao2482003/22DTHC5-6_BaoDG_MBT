---
title: "Java: Lập Trình Đa Luồng Cơ Bản"
date: 2025-12-27
tags: ["Java", "Concurrency"]
description: "Tìm hiểu về multithreading và concurrency trong Java"
image: "https://images.unsplash.com/photo-1518773553398-650c184e0bb3?w=1200&h=600&fit=crop"
video: "_SWK7pZQzIg"
---

Lập trình đa luồng luôn là một chủ đề được nhiều lập trình viên chú ý trong quá trình phát triển phần mềm, phải làm sao để chương trình của mình chạy trơn tru nhất, nhẹ nhàng nhất để người dùng có thiện cảm tốt với sản phẩm của mình. Trong bài viết dưới đây, chúng ta sẽ cùng tìm hiểu về lập trình đa luồng và các thực hiện với ngôn ngữ Java.

1. Đa luồng là gì ?
Thread(luồng) là một tiến trình con(sub-process), một đơn vị nhỏ nhất của máy tính có thể thực hiện một công việc riêng biệt. Đa tiến trình và đa luồng là các cách để đạt được đa nhiệm. Trong Java, các luồng được quản lý bởi máy ảo Java(JVM)

Lợi ích của đa luồng
Đa luồng không chặn người dùng vì các luồng là độc lập và bạn có thể thực hiện nhiều hành động tại một thời điểm

Có thể tiết kiệm thời gian vì có thể thực hiện nhiều hành động đồng thời

Các luồng độc lập nên nó sẽ không gây ảnh hưởng tới các luồng khác nếu exception xảy ra tại một luồng

Các luồng có thể dùng chung và chia sẻ nguồn tài nguyên trong quá trình chạy, tuy nhiên các luồng vẫn sẽ hoạt động độc lập với nhau

Nhược điểm của đa luồng
Xử lý chương trình phức tạp

Xử lý vấn đề tranh chấp bộ nhớ, đồng bộ dữ liệu khá phức tạp

Cần phát hiện các luồng bị chặn, block(dead lock) để tránh gây lỗi cho hệ thống

2. Đa nhiệm là gì ?
Đa nhiệm là khả năng chạy đồng thời một hay nhiều chương trình cùng lúc trên một hệ điều hành. Chúng ta dùng đa nhiệm để tối ưu việc sử dụng tài nguyên CPU. Đa nhiệm có thể đạt được qua hai cách:

Đa nhiệm dựa trên đa tiến trình(Multiprocessing)
Mỗi tiến trình có một địa chỉ trong bộ nhớ, mỗi tiến trình có một bộ nhớ riêng biệt

Một tiến trình là nặng

Sự giao tiếp giữa các tiến trình có chi phí cao

Sự chuyển đổi giữa một tiến trình sang tiến trình khác đòi hỏi thời gian cho việc lưu và tải các vùng đăng ký bộ nhớ, danh sách, ...

Đa nhiệm dựa trên đa luồng(Multithreading)
Luồng sử dụng chung vùng nhớ

Luồng là nhẹ

Chi phí giao tiếp giữa các luồng là không cao

3. Vòng đời của một Thread trong Java
Một Thread sẽ trải qua năm trạng thái trong vòng đời của nó. Vòng đời của luồng được quản lý bởi JVM bao gồm có:

NEW : Đây là trạng thái khi luồng vừa được khởi tạo bằng phương thức khởi tạo của lớp Thread nhưng chưa được start(). Ở trạng thái này, luồng được tạo ra nhưng chưa được cấp phát tài nguyên và cũng chưa chạy. Nếu luồng đang ở trạng thái này mà ta gọi các phương thức ép buộc stop,resume,suspend … sẽ là nguyên nhân sảy ra ngoại lệ IllegalThreadStateException .

RUNNABLE : Sau khi gọi phương thức start() thì luồng test đã được cấp phát tài nguyên và các lịch điều phối CPU cho luồng test cũng bắt đầu có hiệu lực. Ở đây, chúng ta dùng trạng thái là Runnable chứ không phải Running, vì luồng không thực sự luôn chạy mà tùy vào hệ thống mà có sự điều phối CPU khác nhau.

WAITING : Thread chờ không giới hạn cho đến khi một luồng khác đánh thức nó.

TIMED_WAITING : Thread chờ trong một thời gian nhất định, hoặc là có một luồng khác đánh thức nó.

BLOCKED: Đây là 1 dạng của trạng thái “Not Runnable”, là trạng thái khi Thread vẫn còn sống, nhưng hiện tại không được chọn để chạy. Thread chờ một monitor để unlock một đối tượng mà nó cần.

TERMINATED : Một thread ở trong trạng thái terminated hoặc dead khi phương thức run() của nó bị thoát.

4. Lớp Thread trong Java
Trong Java, chúng ta có thể tạo ra một luồng bằng cách tạo một đối tượng từ một lớp kế thừa lớp Thread. Để khởi tạo một luồng bằng cách này, chúng ta sẽ phải thực hiện các bước sau:

Khai báo một lớp kế thừa lớp Thread

Override lại phương thức run() của lớp này, các dòng lệnh trong phương thức này sẽ được thực thi khi luồng bắt đầu chạy. Sau khi luồng đã thực hiện xong các dòng code trong phương thức run thì luồng sẽ chuyển sang trạng thái hủy.

Tạo một đối tượng của lớp vừa khai báo.

Gọi phương thức start() để bắt đầu thực thi luồng.

```java
public class ThreadSample extends Thread{

    @Override
    public void run() {
        System.out.println("This is a thread, myThreadName: " + this.getName());
    }
}
```

Tại sao gọi phương thức start() chứ không phải run() để thực thi luồng?

Phương thức start() là phương thức đặc biệt được Oracle xây dựng trong lớp Thread, phương thức này sẽ cấp phát tài nguyên cho luồng rồi mới thực thi phương thức run() ở luồng này.

Do đó, nếu ta gọi phương thức run() thì cũng tương đương với việc gọi một phương thức ở một lớp bình thường và nó sẽ chạy trên luồng thực hiện gọi phương thức chứ không sinh ra một luồng mới.

Thêm một lưu ý nữa, bạn không thể gọi phương thức start() của Thread hai lần, nếu không sẽ có ngoại lệ IllegalThreadStateException xảy ra.

5. Interface Runnable trong Java
Ngoài cách sử kế thừa lớp Thread trong Java, bạn hoàn toàn có thể sử dụng interface Runnable trong Java để khởi tạo một luồng mới. Để tạo luồng bằng cách sử dụng Runnable, ta phải làm các công việc sau:

Khai báo một lớp implement Runnable

Override lại phương thức run() trong lớp này. Việc này tương tự với việc override lại phương thức run() trong cách sử dụng lớp Thread

Tạo một đối tượng của lớp vừa khai báo

Tạo một đối tượng Thread bằng constructor: Thread(Runnable runnable). Điều thú vị ở đây là, mặc dù chúng ta vẫn cần một đối tượng Thread để khởi tạo luồng mới, tuy nhiên thay vì việc override lại phương thức run() của nó thì chúng ta lại truyền vào đây một Runnable với các dòng code thực thi chương trình đã được thêm vào trong phương thức run()

Gọi phương thức start() để bắt đầu thực thi luồng

```java
public class PingWorker implements Runnable {

    private static final Logger LOGGER = LogManager.getLogger("PingWorker");
    private static final long TIMEOUT_PING = 30 * 1000L;

    @Override
    public void run() {
        LOGGER.info("Start ping worker !");
        while (true) {
            Packet pingPacket = new Packet();
            pingPacket.setServiceType(Constants.PacketServiceType.PING);
            pingPacket.setBody(new JSONObject());
            List<Connection> allConnections = ConnectionManager.getInstance().getAllConnections();
            for (Connection connection : allConnections) {
                connection.sendPacket(pingPacket);
            }

            try {
                TimeUnit.SECONDS.sleep(30);
            } catch (Exception ex) {
                LOGGER.error("Error  when trying to sleep ", ex);
            }
        }
    }
}
```

Khi nào thì nên dùng Runnable
Vì Runnable là một interface nên nó có thể giúp chúng ta giải quyết bài toán đa kế thừa. Ngoài ra, Thread Pool cũng chấp nhận các đối tượng để nó quản lý là Runnable, việc sử dụng Thread Pool mang lại nhiều hiệu quả trong việc quản lý các luồng

Trong các trường hợp còn lại, bạn hoàn toàn có thể sử dụng Thread

6. Ví dụ minh họa sử dụng đa luồng
Tạo luồng với Thread

```java
public class ThreadSample extends Thread{

    @Override
    public void run() {
        System.out.println("This is a thread, myThreadName: " + this.getName());
    }

    public static void main(String[] args) {
        ThreadSample thread = new ThreadSample();
        ThreadSample thread2 = new ThreadSample();
        ThreadSample thread3 = new ThreadSample();

        System.out.println("CurrentThread " + Thread.currentThread().getName());

        thread.start();
        thread2.start();
        thread3.start();
    }
}
```

**Output chương trình:**
```
CurrentThread main
This is a thread, myThreadName: Thread-0
This is a thread, myThreadName: Thread-2
This is a thread, myThreadName: Thread-1
```
Tạo luồng với interface Runnable

```java
public class RunnableSample implements Runnable {

    @Override
    public void run() {
        long timestamp = System.currentTimeMillis();
        System.out.printf("[%s]Checking timestamp%n", Thread.currentThread().getName());
        System.out.printf("[%s]My timestamp: %d%n", Thread.currentThread().getName(), timestamp);
    }

    public static void main(String[] args) {
        Thread thread1 = new Thread(new RunnableSample());
        Thread thread2 = new Thread(new RunnableSample());
        Thread thread3 = new Thread(new RunnableSample());

        thread1.start();
        thread2.start();
        thread3.start();
    }
}
```

**Output chương trình:**
```
[Thread-2]Checking timestamp
[Thread-0]Checking timestamp
[Thread-0]My timestamp: 1687795285576
[Thread-1]Checking timestamp
[Thread-1]My timestamp: 1687795285576
[Thread-2]My timestamp: 1687795285576
```
7. Các phương thức của lớp Thread thường được hay sử dụng

| Kiểu trả về | Tên phương thức | Mô tả |
|-------------|----------------|-------|
| void | start() | Được sử dụng để bắt đầu thực thi một luồng |
| void | run() | Được sử dụng để chạy hành động của một luồng |
| static void | sleep() | Nghỉ một luồng trong một khoảng thời gian |
| static Thread | currentThread() | Trả về tham chiếu tới luồng đang thực thi đối tượng Thread |
| void | join() | Chờ một thread hoàn thành vòng đời của nó |
| int | getPriority() | Trả về giá trị ưu tiên của luồng |
| void | setPriority(int) | Thay đổi giá trị ưu tiên của luồng |
| String | getName() | Trả về tên của luồng |
| void | setName(String) | Thay đổi tên của luồng |
| long | getId() | Trả về id của luồng |
| boolean | isAlive() | Cho biết thread còn sống hay không |
| static void | yield() | Làm cho luồng hiện tại tạm dừng và cho phép các thread khác thực hiện trong một khoảng thời gian |
| void | suspend() | Được sử dụng để tạm dừng một luồng |
| void | resume() | Được sử dụng để tiếp tục luồng đã bị tạm dừng |
| void | stop() | Được dùng để dừng thread |
| void | destroy() | Dùng để phá hủy các nhóm luồng và các nhóm con của nó |
| boolean | isDaemon() | Kiểm tra xem luồng này có phải là daemon thread hay không |
| void | setDaemon(boolean) | Thay đổi việc luồng có phải là daemon hay không |
| void | interrupt() | Làm gián đoạn một luồng trong java. Nếu thread nằm trong trạng thái sleep hoặc wait, nghĩa là sleep() hoặc wait() được gọi ra. Việc gọi phương thức interrupt() trên thread đó sẽ phá vỡ trạng thái sleep hoặc wait và ném ra ngoại lệ InterruptedException. Nếu thread không ở trong trạng thái sleep hoặc wait, việc gọi phương thức interrupt() thực hiện hành vi bình thường và không làm gián đoạn thread nhưng đặt cờ interrupt thành true. |
| boolean | isInterrupted() | Kiểm tra xem luồng có bị interrupt hay không |
| static boolean | interrupted() | Kiểm tra xem luồng hiện tại có bị interrupt hay chưa |


**Kết luận**

Lập trình đa luồng Java mang lại nhiều lợi ích như tăng hiệu suất và tận dụng tài nguyên hệ thống. Tuy nhiên, việc quản lý và đồng bộ hóa luồng đòi hỏi kỹ năng chính xác và phải tránh các vấn đề như data race và deadlock. Trong bài viết này, Stringee đã giúp các bạn có được các kiến thức về lập trình đa luồng với Java, trong các bài viết tiếp theo của series lập trình đa luồng, chúng ta sẽ tìm hiểu về cách phòng tránh deadlock và data race cũng như các kỹ thuật lập trình đa luồng khác.