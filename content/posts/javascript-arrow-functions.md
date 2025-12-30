---
title: "JavaScript: Arrow Functions và This Binding"
date: 2025-12-27
tags: ["JavaScript", "ES6"]
description: "Tìm hiểu về Arrow Functions và cách xử lý this binding trong JavaScript"
image: "https://images.unsplash.com/photo-1607799279861-4dd421887fb3?w=1200&h=600&fit=crop"
---

## Arrow Functions là gì?

Arrow Functions (hàm mũi tên) là cú pháp ngắn gọn để định nghĩa hàm trong ES6:

```javascript
// Cú pháp truyền thống
const sum = function(a, b) {
  return a + b;
};

// Arrow function
const sum = (a, b) => a + b;
```

## Đặc điểm nổi bật

### 1. Cú pháp ngắn gọn

```javascript
// Một tham số, không cần ngoặc đơn
const square = x => x * x;

// Nhiều tham số
const multiply = (a, b) => a * b;

// Nhiều dòng code, cần return
const calculate = (a, b) => {
  const sum = a + b;
  return sum * 2;
};
```

### 2. This Binding

Arrow function **không** có `this` riêng, nó kế thừa `this` từ context bên ngoài:

```javascript
class Counter {
  constructor() {
    this.count = 0;
  }
  
  // Arrow function - this trỏ đến Counter
  increment = () => {
    this.count++;
  }
  
  // Regular function - this có thể bị thay đổi
  incrementOld() {
    this.count++;
  }
}

const counter = new Counter();
setTimeout(counter.increment, 1000); // OK
setTimeout(counter.incrementOld, 1000); // Lỗi!
```

## Khi nào KHÔNG nên dùng Arrow Function?

### 1. Methods trong object

```javascript
// ❌ Không nên
const obj = {
  value: 10,
  getValue: () => this.value // undefined
};

// ✅ Nên dùng
const obj = {
  value: 10,
  getValue() { return this.value; }
};
```

### 2. Event handlers cần `this` của element

```javascript
// ❌ Không nên
button.addEventListener('click', () => {
  this.classList.toggle('active'); // this không phải button
});

// ✅ Nên dùng
button.addEventListener('click', function() {
  this.classList.toggle('active');
});
```

## Kết luận

Arrow functions giúp code ngắn gọn và giải quyết vấn đề `this` binding trong nhiều trường hợp. Tuy nhiên, cần hiểu rõ cách hoạt động để sử dụng đúng lúc!
