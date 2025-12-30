---
title: "Generics trong Java: Lợi ích và cách sử dụng tối ưu"
date: 2025-12-27
tags: ["Java", "Generics"]
description: "Hiểu về Generics trong Java và cách sử dụng hiệu quả"
image: "https://images.unsplash.com/photo-1629654297299-c8506221ca97?w=1200&h=600&fit=crop"
video: "TMnpcRvj7ow"
---

Generics được thêm vào Java 5 nhằm mục đích kiểm tra kiểu dữ liệu ngay tại thời điểm biên dịch (compile-time type checking) và loại bỏ rủi ro lỗi ClassCastException vốn thường xảy ra khi làm việc với các lớp collection. Toàn bộ collection framework cũng đã được viết lại để sử dụng Generics để tăng cường an toàn kiểu dữ liệu (type-safety).

Chúng ta hãy cùng xem cách Generics giúp sử dụng lớp collection an toàn hơn.

```java
List list = new ArrayList();
list.add("abc");
list.add(new Integer(5)); //OK

for(Object obj : list){
 //ép kiểu dẫn đến lỗi ClassCastException lúc chạy
    String str=(String) obj; 
}
```

Đoạn code ví dụ ở trên tuy biên dịch thành công nhưng sẽ gây ra lỗi ClassCastException tại thời điểm chạy (runtime). Lý do là vì ta đang cố ép kiểu (cast) một đối tượng Object trong list sang loại String, trong khi một trong số các phần tử trong đó lại thuộc kiểu Integer.

Kể từ Java 5 trở đi, cách sử dụng collection class đã thay đổi như sau.

```java
List<String> list1 = new ArrayList<String>(); // java 7 ? List<String> list1 = new ArrayList<>(); 
list1.add("abc");
//list1.add(new Integer(5)); //lỗi biên dịch

for(String str : list1){
     //không cần ép kiểu, tránh được ClassCastException
}
```

Hãy chú ý rằng, tại thời điểm khởi tạo list, ta đã chỉ định kiểu phần tử của danh sách là String. Do đó, nếu ta cố gắng thêm bất kỳ đối tượng thuộc kiểu khác vào list, chương trình sẽ báo lỗi ngay tại thời điểm biên dịch. Ngoài ra, trong vòng lặp for, ta không cần phải ép kiểu các phần tử lấy ra từ list nữa, nhờ đó tránh được lỗi ClassCastException tại thời điểm runtime.

Generics không chỉ là một tính năng nâng cao trong Java mà còn là một công cụ mạnh mẽ giúp các lập trình viên xây dựng ứng dụng an toàn, hiệu quả và dễ dàng bảo trì. Với khả năng tổng quát hóa kiểu dữ liệu, Generics mở ra cơ hội giảm thiểu lỗi runtime và tối ưu hóa hiệu suất thông qua việc kiểm tra kiểu ngay tại thời điểm biên dịch. Nếu bạn muốn phát triển các ứng dụng Java hiện đại với chất lượng mã vượt trội, việc hiểu và làm chủ Generics là điều không thể thiếu. Trong bài viết này, chúng ta sẽ cùng khám phá cốt lõi của Generics, từ các khái niệm cơ bản cho đến cách áp dụng với những ví dụ đơn giản dễ hiểu, giúp bạn tự tin triển khai trong mọi dự án Java của mình.

image.png

1. Generic trong Java là gì?
Generics là một tính năng trong Java (từ Java 5) cho phép bạn định nghĩa các lớp, phương thức, và giao diện với kiểu dữ liệu tổng quát. Điều này cung cấp sự kiểm soát kiểu mạnh mẽ, giảm lỗi tại runtime và tăng tính linh hoạt khi làm việc với dữ liệu.

1. Ví dụ không sử dụng Generic:

List list = new ArrayList();  
list.add("Apple");  
list.add(123);  // Không bị kiểm tra kiểu  
String fruit = (String) list.get(1);  // Gây lỗi ClassCastException

List không được quy định kiểu dữ liệu (Raw Type), nên có thể thêm bất kỳ loại giá trị nào (String, Integer, ...).
Khi lấy giá trị ra bằng list.get(1) và cố gắng ép kiểu sang String, giá trị thực là 123 (kiểu Integer) sẽ gây lỗi ClassCastException tại runtime.
2. Ví dụ có sử dụng Generic:

List<String> list = new ArrayList<>();  
list.add("Apple");  
// list.add(123);  // Lỗi compile-time  
String fruit = list.get(0);  // An toàn và không cần ép kiểu

Sử dụng Generics với List<String> giới hạn chỉ cho phép thêm dữ liệu kiểu String vào danh sách.
Nếu cố gắng thêm dữ liệu không hợp lệ (ví dụ 123), chương trình sẽ báo lỗi tại compile-time, ngăn ngừa lỗi trước khi chạy chương trình.
Khi lấy dữ liệu bằng list.get(0), không cần ép kiểu thủ công, mã nguồn ngắn gọn và an toàn hơn.
2. Lợi ích của việc sử dụng Generics
1. Kiểm tra kiểu dữ liệu tại thời điểm biên dịch (Type Safety):
Generics giúp đảm bảo rằng chỉ các kiểu dữ liệu hợp lệ được thêm vào cấu trúc dữ liệu, ngăn lỗi runtime do sai kiểu.

Tham khảo ví dụ ở phần 1 để hiểu rõ hơn nhé mọi người. Giờ mình sang lợi ích thứ 2

2. Loại bỏ việc ép kiểu thủ công (No Manual Casting)
Generics tự động quản lý kiểu dữ liệu, không cần sử dụng (String), (Integer)... thủ công khi truy xuất.

// Không dùng Generics
List list = new ArrayList();
list.add("Generics");
String value = (String) list.get(0); // Phải ép kiểu thủ công

// Có Generics
List<String> list = new ArrayList<>();
list.add("Generics");
String value = list.get(0); // Không cần ép kiểu, an toàn và gọn gàng

Generics tự động xác định kiểu dữ liệu, giúp mã rõ ràng và tránh sai sót do ép kiểu sai.
3. Tăng khả năng tái sử dụng mã nguồn (Code Reusability)
Một lớp hoặc phương thức Generic có thể áp dụng cho bất kỳ kiểu dữ liệu nào, thay vì viết riêng cho từng kiểu.

 // Generic class
class Box<T> {
    private T item;
    public void setItem(T item) { this.item = item; }
    public T getItem() { return item; }
}

public class Main {
    public static void main(String[] args) {
        Box<String> stringBox = new Box<>();
        stringBox.setItem("Hello Generics");
        System.out.println(stringBox.getItem()); // Hello Generics

        Box<Integer> intBox = new Box<>();
        intBox.setItem(123);
        System.out.println(intBox.getItem()); // 123
    }
}

Box<T> chỉ cần định nghĩa một lần, nhưng có thể sử dụng cho cả String, Integer, hay bất kỳ kiểu dữ liệu nào.
4. Dễ dàng bảo trì và đọc hiểu mã nguồn
Generics làm cho mã nguồn dễ hiểu hơn, giúp xác định ngay kiểu dữ liệu được sử dụng và giảm lỗi logic.

// Không dùng Generics
Map map = new HashMap();
map.put("key1", 123);
map.put(456, "value"); // Sai logic, nhưng không bị phát hiện

// Có Generics
Map<String, Integer> map = new HashMap<>();
map.put("key1", 123);
// map.put(456, "value"); // Compile-time: Lỗi, đảm bảo đúng logic

Generics giúp định nghĩa rõ ràng rằng Map<String, Integer> chỉ được phép sử dụng khóa là String và giá trị là Integer.
5. Hạn chế lỗi runtime (Reduced Runtime Errors)
Bằng cách phát hiện lỗi tại compile-time, Generics loại bỏ nhiều lỗi runtime phổ biến như ClassCastException.

// Không dùng Generics
List list = new ArrayList();
list.add(10);
list.add("Java");
for (Object obj : list) {
    Integer value = (Integer) obj; // Gây lỗi runtime nếu obj là String
    System.out.println(value);
}

// Có Generics
List<Integer> list = new ArrayList<>();
list.add(10);
// list.add("Java"); // Compile-time: Lỗi
for (Integer value : list) {
    System.out.println(value); // Không bao giờ lỗi runtime
}

Không dùng Generics: Chỉ phát hiện lỗi khi chạy chương trình.
Có Generics: Lỗi sai kiểu bị ngăn chặn ngay tại compile-time.
3. Generic Classes
Generic Class là lớp cho phép định nghĩa kiểu dữ liệu tổng quát (Generic) tại thời điểm khai báo. Với Generic Classes, bạn có thể viết mã linh hoạt, dễ tái sử dụng và đảm bảo tính an toàn kiểu.

class ClassName<T> {
    private T value;

    public void setValue(T value) {
        this.value = value;
    }

    public T getValue() {
        return value;
    }
}

T là một type parameter đại diện cho kiểu dữ liệu (ví dụ: String, Integer,...).
T có thể là bất kỳ ký tự nào, nhưng thường dùng: T, E, K, V.
Ví dụ về Generic Class:

// Định nghĩa Generic Class
class Box<T> {
    private T item;

    public void setItem(T item) {
        this.item = item;
    }

    public T getItem() {
        return item;
    }
}

// Sử dụng Generic Class
public class Main {
    public static void main(String[] args) {
        Box<String> stringBox = new Box<>();
        stringBox.setItem("Hello Generics");
        System.out.println("String Box: " + stringBox.getItem());

        Box<Integer> integerBox = new Box<>();
        integerBox.setItem(123);
        System.out.println("Integer Box: " + integerBox.getItem());
    }
}

String Box: Hello Generics
Integer Box: 123

Box<String> chỉ nhận và xử lý dữ liệu kiểu String.
Box<Integer> chỉ nhận và xử lý dữ liệu kiểu Integer.
Nhiều tham số kiểu (Multiple Type Parameters)

Bạn có thể dùng nhiều tham số kiểu trong một Generic Class.

class Pair<K, V> {
    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public V getValue() {
        return value;
    }
}

**Bounded Type Parameter**s
    

public class Main {
    public static void main(String[] args) {
        Pair<String, Integer> pair = new Pair<>("Age", 30);
        System.out.println("Key: " + pair.getKey());
        System.out.println("Value: " + pair.getValue());
    }
}

Key: Age
Value: 30

Bạn có thể giới hạn kiểu tham số của Generic Class bằng extends.
Bounded Type Parameters

Bạn có thể giới hạn kiểu tham số của Generic Class bằng extends.

class NumberBox<T extends Number> {
    private T number;

    public void setNumber(T number) {
        this.number = number;
    }

    public T getNumber() {
        return number;
    }
}

public class Main {
    public static void main(String[] args) {
        NumberBox<Integer> intBox = new NumberBox<>();
        intBox.setNumber(100);
        System.out.println("Integer: " + intBox.getNumber());

        NumberBox<Double> doubleBox = new NumberBox<>();
        doubleBox.setNumber(10.5);
        System.out.println("Double: " + doubleBox.getNumber());

        // NumberBox<String> stringBox = new NumberBox<>(); // Lỗi compile-time
    }
}

Integer: 100
Double: 10.5

<T extends Number> giới hạn T chỉ có thể là Number hoặc các lớp con của Number như Integer, Double,...
4. Generic Method
Generic Method là phương thức cho phép sử dụng kiểu dữ liệu tổng quát (Generic) thay vì kiểu dữ liệu cố định. Kiểu dữ liệu tổng quát này được định nghĩa khi phương thức được gọi.

Cách khai báo Generic Method

public <T> ReturnType methodName(T parameter) {
    // Thân phương thức
}

<T>: Khai báo một tham số kiểu dữ liệu tổng quát (Generic Type).
T parameter: Sử dụng kiểu dữ liệu Generic làm tham số.
ReturnType: Phương thức có thể trả về kiểu T hoặc các kiểu khác.
Ví dụ:

public class GenericMethodExample {

    // Generic method
    public static <T> void printArray(T[] array) {
        for (T item : array) {
            System.out.println(item);
        }
    }

    public static void main(String[] args) {
        String[] stringArray = { "Apple", "Banana", "Cherry" };
        Integer[] intArray = { 1, 2, 3 };

        // Gọi phương thức generic
        printArray(stringArray);
        printArray(intArray);
    }
}

Apple
Banana
Cherry
1
2
3

Phương thức printArray có thể in mọi kiểu dữ liệu (String, Integer,...) nhờ sử dụng T.
Phương thức generic với giá trị trả về

public class GenericReturnTypeExample {

    // Generic method
    public static <T> T getFirstElement(T[] array) {
        if (array.length > 0) {
            return array[0];
        }
        return null; // Trả về null nếu mảng rỗng
    }

    public static void main(String[] args) {
        String[] stringArray = { "Apple", "Banana", "Cherry" };
        Integer[] intArray = { 10, 20, 30 };

        System.out.println("First string: " + getFirstElement(stringArray));
        System.out.println("First integer: " + getFirstElement(intArray));
    }
}

First string: Apple
First integer: 10

Generic Method getFirstElement trả về giá trị kiểu dữ liệu của mảng được truyền vào.
Bounded Type Parameters in Generic Method

Sử dụng từ khóa extends để giới hạn kiểu dữ liệu tổng quát.

public class BoundedGenericMethodExample {

// Generic method với kiểu giới hạn
public static <T extends Number> double sum(T a, T b) {
        return a.doubleValue() + b.doubleValue();
}

    public static void main(String[] args) {
        System.out.println("Sum of 10 and 20: " + sum(10, 20));
        System.out.println("Sum of 3.5 and 4.5: " + sum(3.5, 4.5));
        // sum("Hello", "World"); // Lỗi compile-time
    }
}

Sum of 10 and 20: 30.0
Sum of 3.5 and 4.5: 8.0

<T extends Number>: Giới hạn kiểu T chỉ cho phép Number hoặc các lớp con (như Integer, Double).
Generic Method trong lớp không Generic

Bạn có thể định nghĩa phương thức generic trong một lớp không generic. public class NonGenericClass {

//Phương thức generic trong lớp không generic
public static <T> void printItem(T item) {
        System.out.println("Item: " + item);
    }

    public static void main(String[] args) {
        printItem("Hello");
        printItem(123);
    }
}

Item: Hello
Item: 123

Wildcards trong Generic Method

Dùng wildcard ? để viết phương thức generic linh hoạt hơn.

import java.util.List;

public class WildcardExample {

    // Sử dụng wildcard
    public static void printList(List<?> list) {
        for (Object item : list) {
            System.out.println(item);
        }
    }

    public static void main(String[] args) {
        List<String> stringList = List.of("Apple", "Banana");
        List<Integer> intList = List.of(1, 2, 3);

        printList(stringList);
        printList(intList);
    }
}

Apple
Banana
1
2
3

Wildcard ?: Cho phép sử dụng danh sách bất kỳ kiểu dữ liệu nào.
5. Wildcards (?)
Trong Generics của Java, ký tự đại diện (Wildcard) ? được sử dụng để biểu thị một kiểu dữ liệu chưa biết. Nó giúp Generic trở nên linh hoạt hơn, đặc biệt khi làm việc với các loại dữ liệu không xác định chính xác.

1. Unbounded Wildcard (?)
Sử dụng khi kiểu dữ liệu có thể là bất kỳ loại nào.
Cú pháp: ?
import java.util.List;

public class UnboundedWildcardExample {
    public static void printList(List<?> list) {
        for (Object item : list) {
            System.out.println(item);
        }
    }

    public static void main(String[] args) {
        List<String> stringList = List.of("Apple", "Banana");
        List<Integer> intList = List.of(1, 2, 3);

        printList(stringList);
        printList(intList);
    }
}

Apple
Banana
1
2
3

List<?> cho phép truyền vào bất kỳ kiểu nào.
Trong thân phương thức, bạn chỉ có thể sử dụng các thao tác được hỗ trợ với Object vì không biết chính xác kiểu của danh sách.
2. Upper Bounded Wildcard (<? extends T>)
Giới hạn kiểu dữ liệu là một lớp cụ thể hoặc các lớp con của nó.
Cú pháp: <? extends T>
import java.util.List;

public class UpperBoundedWildcardExample {
    public static double sumNumbers(List<? extends Number> list) {
        double sum = 0.0;
        for (Number num : list) {
            sum += num.doubleValue();
        }
        return sum;
    }

    public static void main(String[] args) {
        List<Integer> integers = List.of(1, 2, 3);
        List<Double> doubles = List.of(1.5, 2.5, 3.5);

        System.out.println("Sum of integers: " + sumNumbers(integers));
        System.out.println("Sum of doubles: " + sumNumbers(doubles));
    }
}

Sum of integers: 6.0
Sum of doubles: 7.5

<? extends Number> chỉ chấp nhận Number hoặc các lớp con như Integer, Double.
Phương thức có thể làm việc với nhiều kiểu dữ liệu liên quan đến số.
3. Lower Bounded Wildcard (<? super T>)
Giới hạn kiểu dữ liệu là một lớp cụ thể hoặc các lớp cha của nó.
Cú pháp: <? super T>
import java.util.List;
import java.util.ArrayList;

public class LowerBoundedWildcardExample {
    public static void addNumbers(List<? super Integer> list) {
        list.add(10);
        list.add(20);
    }

    public static void main(String[] args) {
        List<Number> numbers = new ArrayList<>();
        addNumbers(numbers);

        for (Object obj : numbers) {
            System.out.println(obj);
        }
    }
}

10
20

<? super Integer> chỉ chấp nhận Integer hoặc các lớp cha như Number, Object.
Có thể thêm các giá trị kiểu Integer vào danh sách.
4. Ứng dụng thực tế
Khi nào dùng <? extends T>

Khi đọc dữ liệu từ danh sách, nhưng không cần thêm mới.
Ví dụ: Tính tổng các giá trị trong danh sách.
Khi nào dùng <? super T>

Khi thêm dữ liệu vào danh sách, nhưng không quan tâm đến kiểu cụ thể của danh sách.
Ví dụ: Thêm các số vào danh sách cha (Number, Object).
Khi nào dùng <?>

Khi không cần biết hay kiểm soát kiểu dữ liệu cụ thể.
Ví dụ: In danh sách bất kỳ.
5. Lưu ý khi sử dụng Wildcards
Không thể thêm giá trị ngoài null khi dùng ? hoặc <? extends T>.
Dùng ? khi làm việc với dữ liệu mà chỉ cần truy cập, không cần thay đổi.
Wildcards làm mã tổng quát hơn, nhưng cũng có thể làm giảm an toàn kiểu nếu dùng không cẩn thận.
Wildcards là công cụ mạnh mẽ để làm cho Generics linh hoạt và mạnh mẽ hơn trong Java
6. Các tham số kiểu trong Generics
T (Type): Đại diện cho một kiểu bất kỳ. Dùng phổ biến trong các lớp, phương thức, hoặc interface tổng quát.

E (Element): Đại diện cho phần tử. Thường sử dụng trong các cấu trúc dữ liệu như danh sách hoặc tập hợp.

K (Key): Đại diện cho khóa trong cấu trúc cặp giá trị, ví dụ như Map.

V (Value): Đại diện cho giá trị tương ứng với một khóa, sử dụng với cặp K-V.

N (Number): Đại diện cho kiểu số như Integer, Double, hoặc Float.

? (Wildcard): Đại diện cho một kiểu chưa biết, giúp làm việc với các đối tượng có kiểu không xác định cụ thể.

Bounded Type Parameters (Giới hạn kiểu):

<T extends Class>: Giới hạn T là Class hoặc các lớp con.
<T extends Interface>: Giới hạn T là một interface cụ thể hoặc lớp triển khai interface đó.
7. Bảng So Sánh List, List<T>, List<Object>, List<?>, List<Student>
image.png

8. Kết bài
Như vậy, chúng ta đã cùng nhau tìm hiểu về Generics trong Java, dù ban đầu việc làm quen với Generics có thể gây đôi chút khó khăn, nhưng với sự luyện tập và áp dụng thường xuyên, bạn sẽ thấy sự khác biệt rõ ràng trong cách quản lý kiểu dữ liệu và tối ưu hóa hiệu năng mã nguồn của mình. Hãy bắt đầu từ những ứng dụng nhỏ, từng bước triển khai Generics vào các bài toán thực tế, và biến chúng thành một phần không thể thiếu trong tư duy lập trình của bạn.

Làm chủ Generics chính là một bước đi vững chắc để trở thành một lập trình viên Java chuyên nghiệp