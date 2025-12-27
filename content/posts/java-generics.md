---
title: "Java Generics: Type Safety cho Collection"
date: 2024-11-25
tags: ["Java", "Generics"]
description: "Hiểu về Generics trong Java và cách sử dụng hiệu quả"
image: "/images/blog/java-generics.png"
---

## Vấn đề trước Generics

Trước Java 5, collection không type-safe:

```java
// Trước Java 5
List list = new ArrayList();
list.add("Hello");
list.add(123);
list.add(new Date());

String text = (String) list.get(0); // OK
String num = (String) list.get(1);  // Runtime error!
```

## Generics giải quyết vấn đề

```java
// Với Generics
List<String> list = new ArrayList<>();
list.add("Hello");
list.add("World");
// list.add(123); // Compile error - Type safety!

String text = list.get(0); // Không cần cast
```

## Generic Class

### Tạo Generic class

```java
public class Box<T> {
  private T value;
  
  public void set(T value) {
    this.value = value;
  }
  
  public T get() {
    return value;
  }
}

// Sử dụng
Box<Integer> intBox = new Box<>();
intBox.set(123);
int num = intBox.get(); // Không cần cast

Box<String> strBox = new Box<>();
strBox.set("Hello");
String text = strBox.get();
```

### Multiple type parameters

```java
public class Pair<K, V> {
  private K key;
  private V value;
  
  public Pair(K key, V value) {
    this.key = key;
    this.value = value;
  }
  
  public K getKey() { return key; }
  public V getValue() { return value; }
}

// Sử dụng
Pair<String, Integer> pair = new Pair<>("Age", 22);
String key = pair.getKey();
Integer value = pair.getValue();
```

## Generic Methods

```java
public class Utils {
  // Generic method
  public static <T> void printArray(T[] array) {
    for (T element : array) {
      System.out.println(element);
    }
  }
  
  // Multiple type parameters
  public static <K, V> void printPair(K key, V value) {
    System.out.println(key + ": " + value);
  }
}

// Sử dụng
Integer[] intArray = {1, 2, 3};
String[] strArray = {"A", "B", "C"};

Utils.printArray(intArray);
Utils.printArray(strArray);
Utils.printPair("Name", "Bảo");
```

## Bounded Type Parameters

### Upper bound

```java
// Chỉ chấp nhận Number và các subclass
public class Calculator<T extends Number> {
  private T value;
  
  public Calculator(T value) {
    this.value = value;
  }
  
  public double getDoubleValue() {
    return value.doubleValue();
  }
}

Calculator<Integer> intCalc = new Calculator<>(10);
Calculator<Double> doubleCalc = new Calculator<>(10.5);
// Calculator<String> strCalc = new Calculator<>("10"); // Compile error!
```

### Multiple bounds

```java
interface Drawable {
  void draw();
}

// T phải implement cả Comparable và Drawable
public class Shape<T extends Comparable<T> & Drawable> {
  private T shape;
  
  public void compare(T other) {
    shape.compareTo(other);
  }
  
  public void display() {
    shape.draw();
  }
}
```

## Wildcards

### Unbounded wildcard (?)

```java
public static void printList(List<?> list) {
  for (Object element : list) {
    System.out.println(element);
  }
}

List<Integer> intList = Arrays.asList(1, 2, 3);
List<String> strList = Arrays.asList("A", "B", "C");

printList(intList);
printList(strList);
```

### Upper bounded wildcard (? extends)

```java
// Chấp nhận List của Number hoặc subclass
public static double sumList(List<? extends Number> list) {
  double sum = 0;
  for (Number num : list) {
    sum += num.doubleValue();
  }
  return sum;
}

List<Integer> ints = Arrays.asList(1, 2, 3);
List<Double> doubles = Arrays.asList(1.5, 2.5);

System.out.println(sumList(ints));    // 6.0
System.out.println(sumList(doubles)); // 4.0
```

### Lower bounded wildcard (? super)

```java
// Chấp nhận List của Integer hoặc superclass
public static void addIntegers(List<? super Integer> list) {
  list.add(1);
  list.add(2);
  list.add(3);
}

List<Number> numbers = new ArrayList<>();
List<Object> objects = new ArrayList<>();

addIntegers(numbers);
addIntegers(objects);
```

## PECS Principle

**Producer Extends, Consumer Super**

```java
// Producer - dùng extends
public static void copy(
  List<? extends Number> source,  // Producer
  List<? super Number> dest       // Consumer
) {
  for (Number num : source) {
    dest.add(num);
  }
}
```

## Type Erasure

Generics chỉ tồn tại ở compile-time, bị xóa ở runtime:

```java
List<String> strings = new ArrayList<>();
List<Integer> ints = new ArrayList<>();

// Runtime: cả 2 đều là ArrayList
System.out.println(strings.getClass() == ints.getClass()); // true
```

## Best Practices

1. **Dùng generics thay vì raw types**
2. **Dùng bounded types khi cần constraint**
3. **Nhớ PECS principle** khi dùng wildcards
4. **Tránh dùng generic arrays** - có thể gây vấn đề
5. **Document type parameters** rõ ràng

```java
/**
 * @param <T> the type of elements in this container
 * @param <K> the type of keys
 * @param <V> the type of values
 */
```

## Kết luận

Generics là công cụ mạnh mẽ giúp code type-safe, tái sử dụng được và ít lỗi runtime. Hiểu rõ generics là bước quan trọng để master Java!
