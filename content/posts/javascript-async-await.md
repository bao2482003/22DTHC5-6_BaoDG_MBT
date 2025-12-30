---
title: "JavaScript: Async/Await và Xử Lý Bất Đồng Bộ"
date: 2025-12-27
tags: ["JavaScript", "Async"]
description: "Xử lý bất đồng bộ trong JavaScript với async/await"
image: "https://images.unsplash.com/photo-1523961131990-5ea7c61b2107?w=1200&h=600&fit=crop"
---

## Vấn đề với Callback Hell

Trước đây, xử lý bất đồng bộ trong JavaScript rất khó đọc:

```javascript
getData(function(a) {
  getMoreData(a, function(b) {
    getMoreData(b, function(c) {
      getMoreData(c, function(d) {
        console.log(d);
      });
    });
  });
});
```

## Promise giải quyết phần nào

```javascript
getData()
  .then(a => getMoreData(a))
  .then(b => getMoreData(b))
  .then(c => getMoreData(c))
  .then(d => console.log(d))
  .catch(error => console.error(error));
```

## Async/Await - Giải pháp tối ưu

Async/await giúp code bất đồng bộ trông như đồng bộ:

```javascript
async function fetchData() {
  try {
    const a = await getData();
    const b = await getMoreData(a);
    const c = await getMoreData(b);
    const d = await getMoreData(c);
    console.log(d);
  } catch (error) {
    console.error(error);
  }
}
```

## Cú pháp cơ bản

### Khai báo async function

```javascript
// Function declaration
async function myFunction() {
  return "Hello";
}

// Arrow function
const myFunction = async () => {
  return "Hello";
};

// Method
class MyClass {
  async myMethod() {
    return "Hello";
  }
}
```

### Sử dụng await

```javascript
async function getUser() {
  const response = await fetch('/api/user');
  const data = await response.json();
  return data;
}
```

## Xử lý nhiều Promise song song

### ❌ Chậm - Chờ tuần tự

```javascript
async function getData() {
  const user = await fetchUser();      // 2s
  const posts = await fetchPosts();    // 2s
  const comments = await fetchComments(); // 2s
  // Tổng: 6s
}
```

### ✅ Nhanh - Chạy song song

```javascript
async function getData() {
  const [user, posts, comments] = await Promise.all([
    fetchUser(),
    fetchPosts(),
    fetchComments()
  ]);
  // Tổng: 2s (thời gian của request chậm nhất)
}
```

## Error Handling

### Try/Catch

```javascript
async function fetchData() {
  try {
    const data = await fetch('/api/data');
    return data.json();
  } catch (error) {
    console.error('Lỗi:', error);
    return null;
  }
}
```

### Higher-order function

```javascript
const asyncHandler = (fn) => async (req, res, next) => {
  try {
    await fn(req, res, next);
  } catch (error) {
    next(error);
  }
};

// Sử dụng
app.get('/users', asyncHandler(async (req, res) => {
  const users = await User.find();
  res.json(users);
}));
```

## Best Practices

1. **Luôn dùng try/catch** hoặc `.catch()` để xử lý lỗi
2. **Chạy song song khi có thể** với `Promise.all()`
3. **Không quên await** - sẽ trở thành Promise chưa resolve
4. **Cẩn thận với forEach** - dùng `for...of` thay vì `forEach`

```javascript
// ❌ Không hoạt động như mong đợi
async function processArray(array) {
  array.forEach(async item => {
    await processItem(item);
  });
}

// ✅ Đúng cách
async function processArray(array) {
  for (const item of array) {
    await processItem(item);
  }
}
```

## Kết luận

Async/await làm cho code bất đồng bộ dễ đọc, dễ debug và dễ maintain hơn rất nhiều so với callback và promise chains!
