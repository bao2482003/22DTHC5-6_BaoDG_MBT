---
title: "Java Stream API: Xử lý Collection hiện đại"
date: 2024-12-08
tags: ["Java", "Stream API"]
description: "Làm chủ Stream API để xử lý dữ liệu hiệu quả trong Java"
image: "/images/blog/java-stream.png"
---

## Stream API là gì?

Stream API (từ Java 8) cho phép xử lý collection theo phong cách functional programming:

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Cách cũ
int sum = 0;
for (int num : numbers) {
  if (num % 2 == 0) {
    sum += num * 2;
  }
}

// Với Stream API
int sum = numbers.stream()
  .filter(n -> n % 2 == 0)
  .map(n -> n * 2)
  .reduce(0, Integer::sum);
```

## Các thao tác phổ biến

### 1. Filter - Lọc phần tử

```java
List<String> names = Arrays.asList("An", "Bình", "Chi", "Dung");

List<String> filtered = names.stream()
  .filter(name -> name.length() > 2)
  .collect(Collectors.toList());
// Kết quả: ["Bình", "Dung"]
```

### 2. Map - Biến đổi phần tử

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4);

List<Integer> squared = numbers.stream()
  .map(n -> n * n)
  .collect(Collectors.toList());
// Kết quả: [1, 4, 9, 16]
```

### 3. Reduce - Tổng hợp kết quả

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

int sum = numbers.stream()
  .reduce(0, (a, b) -> a + b);
// Kết quả: 15

Optional<Integer> max = numbers.stream()
  .reduce(Integer::max);
```

### 4. Collect - Thu thập kết quả

```java
List<String> names = Arrays.asList("An", "Bình", "Chi");

// Thành List
List<String> list = names.stream()
  .collect(Collectors.toList());

// Thành Set
Set<String> set = names.stream()
  .collect(Collectors.toSet());

// Thành Map
Map<String, Integer> map = names.stream()
  .collect(Collectors.toMap(
    name -> name,
    String::length
  ));
```

## Parallel Stream

Xử lý song song để tăng hiệu suất:

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

int sum = numbers.parallelStream()
  .filter(n -> n % 2 == 0)
  .mapToInt(Integer::intValue)
  .sum();
```

## Lưu ý quan trọng

1. **Stream chỉ dùng 1 lần**: Không thể reuse stream đã xử lý
2. **Lazy evaluation**: Chỉ thực thi khi gặp terminal operation
3. **Parallel không phải lúc nào cũng nhanh hơn**: Chỉ hiệu quả với data lớn

## Kết luận

Stream API giúp code ngắn gọn, dễ đọc và dễ maintain hơn nhiều so với vòng lặp truyền thống!
