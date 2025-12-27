---
title: "Java Collections Framework: ArrayList vs LinkedList"
date: 2024-12-15
tags: ["Java", "Data Structures"]
description: "So sánh chi tiết giữa ArrayList và LinkedList trong Java Collections Framework"
image: "/images/blog/java-collections.png"
---

## Giới thiệu

Java Collections Framework cung cấp nhiều cấu trúc dữ liệu khác nhau, trong đó ArrayList và LinkedList là hai lựa chọn phổ biến nhất cho danh sách động.

## ArrayList

ArrayList sử dụng mảng động bên trong để lưu trữ phần tử:

```java
ArrayList<String> list = new ArrayList<>();
list.add("Java");
list.add("Python");
list.add("JavaScript");
```

### Ưu điểm:
- Truy cập nhanh theo index O(1)
- Tiết kiệm bộ nhớ hơn
- Hiệu quả cho việc duyệt qua các phần tử

### Nhược điểm:
- Chèn/xóa phần tử ở giữa chậm O(n)
- Phải tái cấp phát bộ nhớ khi vượt quá capacity

## LinkedList

LinkedList sử dụng cấu trúc danh sách liên kết đôi:

```java
LinkedList<String> list = new LinkedList<>();
list.add("Java");
list.addFirst("Python");
list.addLast("JavaScript");
```

### Ưu điểm:
- Chèn/xóa phần tử nhanh O(1) khi biết vị trí
- Không cần tái cấp phát bộ nhớ
- Hỗ trợ thao tác đầu/cuối danh sách hiệu quả

### Nhược điểm:
- Truy cập theo index chậm O(n)
- Tốn bộ nhớ hơn (lưu trữ thêm con trỏ)

## Khi nào dùng gì?

- **ArrayList**: Khi cần truy cập ngẫu nhiên nhiều, ít thao tác chèn/xóa
- **LinkedList**: Khi thường xuyên chèn/xóa phần tử, ít truy cập theo index

## Kết luận

Hiểu rõ đặc điểm của từng loại sẽ giúp bạn chọn đúng cấu trúc dữ liệu, tối ưu hiệu suất ứng dụng Java.
