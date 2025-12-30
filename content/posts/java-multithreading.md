---
title: "Java: Lập Trình Đa Luồng Cơ Bản"
date: 2025-12-27
tags: ["Java", "Concurrency"]
description: "Tìm hiểu về multithreading và concurrency trong Java"
image: "https://images.unsplash.com/photo-1518773553398-650c184e0bb3?w=1200&h=600&fit=crop"
---

## Thread là gì?

Thread là đơn vị nhỏ nhất của một process, cho phép chương trình thực hiện nhiều tác vụ đồng thời.

## Tạo Thread trong Java

### Cách 1: Extends Thread

```java
class MyThread extends Thread {
  @Override
  public void run() {
    for (int i = 0; i < 5; i++) {
      System.out.println(Thread.currentThread().getName() + ": " + i);
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
    }
  }
}

// Sử dụng
MyThread thread1 = new MyThread();
MyThread thread2 = new MyThread();
thread1.start();
thread2.start();
```

### Cách 2: Implements Runnable (Recommended)

```java
class MyTask implements Runnable {
  @Override
  public void run() {
    for (int i = 0; i < 5; i++) {
      System.out.println(Thread.currentThread().getName() + ": " + i);
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
    }
  }
}

// Sử dụng
Thread thread1 = new Thread(new MyTask());
Thread thread2 = new Thread(new MyTask());
thread1.start();
thread2.start();
```

### Cách 3: Lambda Expression (Java 8+)

```java
Thread thread = new Thread(() -> {
  for (int i = 0; i < 5; i++) {
    System.out.println(Thread.currentThread().getName() + ": " + i);
    try {
      Thread.sleep(1000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }
});
thread.start();
```

## Thread Lifecycle

```
NEW → RUNNABLE → RUNNING → TERMINATED
         ↓           ↓
      BLOCKED    WAITING
```

## Thread Methods

### sleep()

```java
Thread.sleep(1000); // Sleep 1 giây
```

### join()

```java
Thread t1 = new Thread(() -> {
  System.out.println("Task 1");
});
t1.start();
t1.join(); // Chờ t1 hoàn thành
System.out.println("Task 1 completed");
```

### interrupt()

```java
Thread thread = new Thread(() -> {
  try {
    while (!Thread.interrupted()) {
      System.out.println("Working...");
      Thread.sleep(1000);
    }
  } catch (InterruptedException e) {
    System.out.println("Thread interrupted");
  }
});

thread.start();
Thread.sleep(3000);
thread.interrupt(); // Ngắt thread
```

## Synchronization

### Vấn đề Race Condition

```java
class Counter {
  private int count = 0;
  
  public void increment() {
    count++; // Không thread-safe!
  }
  
  public int getCount() {
    return count;
  }
}
```

### Giải pháp: synchronized

```java
class Counter {
  private int count = 0;
  
  public synchronized void increment() {
    count++;
  }
  
  public synchronized int getCount() {
    return count;
  }
}
```

### Synchronized Block

```java
class Counter {
  private int count = 0;
  private Object lock = new Object();
  
  public void increment() {
    synchronized(lock) {
      count++;
    }
  }
}
```

## ExecutorService (Modern Approach)

### Thread Pool

```java
ExecutorService executor = Executors.newFixedThreadPool(3);

for (int i = 0; i < 10; i++) {
  final int taskId = i;
  executor.submit(() -> {
    System.out.println("Task " + taskId + " by " + 
                      Thread.currentThread().getName());
  });
}

executor.shutdown();
```

### Callable & Future

```java
ExecutorService executor = Executors.newSingleThreadExecutor();

Callable<Integer> task = () -> {
  Thread.sleep(2000);
  return 42;
};

Future<Integer> future = executor.submit(task);
System.out.println("Doing other work...");

Integer result = future.get(); // Block cho đến khi có kết quả
System.out.println("Result: " + result);

executor.shutdown();
```

## CompletableFuture (Java 8+)

```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
  try {
    Thread.sleep(2000);
  } catch (InterruptedException e) {
    e.printStackTrace();
  }
  return "Hello";
});

future.thenApply(result -> result + " World")
      .thenAccept(System.out::println);
```

## Thread-Safe Collections

```java
// Synchronized collections
List<String> syncList = Collections.synchronizedList(new ArrayList<>());
Map<String, Integer> syncMap = Collections.synchronizedMap(new HashMap<>());

// Concurrent collections (Better performance)
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
BlockingQueue<String> queue = new LinkedBlockingQueue<>();
```

## Locks (java.util.concurrent.locks)

### ReentrantLock

```java
class Counter {
  private int count = 0;
  private Lock lock = new ReentrantLock();
  
  public void increment() {
    lock.lock();
    try {
      count++;
    } finally {
      lock.unlock(); // Luôn unlock trong finally
    }
  }
}
```

### ReadWriteLock

```java
class Cache {
  private Map<String, String> data = new HashMap<>();
  private ReadWriteLock lock = new ReentrantReadWriteLock();
  
  public String get(String key) {
    lock.readLock().lock();
    try {
      return data.get(key);
    } finally {
      lock.readLock().unlock();
    }
  }
  
  public void put(String key, String value) {
    lock.writeLock().lock();
    try {
      data.put(key, value);
    } finally {
      lock.writeLock().unlock();
    }
  }
}
```

## Best Practices

1. **Dùng ExecutorService** thay vì tạo Thread trực tiếp
2. **Tránh synchronized quá nhiều** - ảnh hưởng performance
3. **Dùng concurrent collections** khi cần thread-safety
4. **Luôn shutdown ExecutorService** sau khi dùng
5. **Cẩn thận với deadlock** - luôn lock theo thứ tự nhất quán
6. **Dùng ThreadLocal** cho thread-specific data
7. **Test kỹ concurrent code** - bugs khó reproduce

## Common Pitfalls

### Deadlock

```java
// ❌ Có thể bị deadlock
synchronized(lock1) {
  synchronized(lock2) {
    // Critical section
  }
}

// Thread khác
synchronized(lock2) {
  synchronized(lock1) {
    // Critical section
  }
}
```

### Starting Thread trong Constructor

```java
// ❌ Nguy hiểm
class MyClass extends Thread {
  public MyClass() {
    start(); // Có thể gây vấn đề
  }
}
```

## Kết luận

Multithreading giúp tận dụng tối đa CPU và cải thiện performance. Tuy nhiên, cần hiểu rõ về synchronization và concurrent programming để tránh bugs khó debug!
