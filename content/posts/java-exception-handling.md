---
title: "Java Exception Handling: Try-Catch và Best Practices"
date: 2025-12-27
tags: ["Java", "Error Handling"]
description: "Xử lý ngoại lệ hiệu quả trong Java"
image: "/images/blog/java-exceptions.png"
---

## Exception là gì?

Exception là các sự kiện bất thường xảy ra trong quá trình chạy chương trình, làm gián đoạn luồng thực thi bình thường.

## Phân loại Exception

### 1. Checked Exception
Phải được xử lý tại compile-time:

```java
// IOException là checked exception
public void readFile(String path) throws IOException {
  FileReader file = new FileReader(path);
  BufferedReader reader = new BufferedReader(file);
  String line = reader.readLine();
  reader.close();
}
```

### 2. Unchecked Exception
Runtime exceptions, không bắt buộc phải xử lý:

```java
// NullPointerException, ArrayIndexOutOfBoundsException
String text = null;
text.length(); // NullPointerException
```

## Try-Catch-Finally

### Cú pháp cơ bản

```java
try {
  // Code có thể gây exception
  int result = 10 / 0;
} catch (ArithmeticException e) {
  // Xử lý exception cụ thể
  System.out.println("Không thể chia cho 0");
} catch (Exception e) {
  // Xử lý exception chung
  System.out.println("Có lỗi xảy ra: " + e.getMessage());
} finally {
  // Luôn thực thi, dù có exception hay không
  System.out.println("Clean up resources");
}
```

### Multi-catch (Java 7+)

```java
try {
  // Some code
} catch (IOException | SQLException e) {
  // Xử lý cả 2 loại exception
  logger.error("Database or IO error", e);
}
```

## Try-with-Resources (Java 7+)

Tự động đóng resource:

```java
// Cách cũ
BufferedReader reader = null;
try {
  reader = new BufferedReader(new FileReader("file.txt"));
  String line = reader.readLine();
} catch (IOException e) {
  e.printStackTrace();
} finally {
  if (reader != null) {
    try {
      reader.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}

// Try-with-resources (ngắn gọn hơn)
try (BufferedReader reader = new BufferedReader(new FileReader("file.txt"))) {
  String line = reader.readLine();
} catch (IOException e) {
  e.printStackTrace();
}
```

## Tạo Custom Exception

```java
public class InsufficientFundsException extends Exception {
  private double amount;
  
  public InsufficientFundsException(double amount) {
    super("Thiếu " + amount + " để thực hiện giao dịch");
    this.amount = amount;
  }
  
  public double getAmount() {
    return amount;
  }
}

// Sử dụng
public void withdraw(double amount) throws InsufficientFundsException {
  if (balance < amount) {
    throw new InsufficientFundsException(amount - balance);
  }
  balance -= amount;
}
```

## Best Practices

### 1. Catch exception cụ thể nhất

```java
// ❌ Không tốt
try {
  // code
} catch (Exception e) {
  // Quá chung chung
}

// ✅ Tốt
try {
  // code
} catch (FileNotFoundException e) {
  // Xử lý cụ thể
} catch (IOException e) {
  // Xử lý IO errors
}
```

### 2. Không bỏ qua exception

```java
// ❌ Tệ nhất
try {
  // code
} catch (Exception e) {
  // Im lặng - rất nguy hiểm!
}

// ✅ Tốt
try {
  // code
} catch (Exception e) {
  logger.error("Error occurred", e);
  // hoặc throw lại
}
```

### 3. Đóng resources

```java
// ✅ Sử dụng try-with-resources
try (Connection conn = DriverManager.getConnection(url);
     PreparedStatement stmt = conn.prepareStatement(sql)) {
  // Use connection
} catch (SQLException e) {
  logger.error("Database error", e);
}
```

### 4. Exception message có ý nghĩa

```java
// ❌ Không rõ ràng
throw new Exception("Error");

// ✅ Rõ ràng
throw new IllegalArgumentException(
  "User ID must be positive, got: " + userId
);
```

### 5. Không dùng exception cho flow control

```java
// ❌ Tệ
try {
  while (true) {
    array[index++] = getData();
  }
} catch (ArrayIndexOutOfBoundsException e) {
  // Dùng exception để thoát vòng lặp
}

// ✅ Tốt
while (index < array.length) {
  array[index++] = getData();
}
```

## Kết luận

Exception handling đúng cách giúp ứng dụng robust, dễ debug và maintain. Hãy luôn xử lý exception một cách có ý nghĩa!
