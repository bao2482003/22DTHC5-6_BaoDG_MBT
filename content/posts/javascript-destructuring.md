---
title: "JavaScript Destructuring: Giải nén dữ liệu dễ dàng"
date: 2024-11-28
tags: ["JavaScript", "ES6"]
description: "Destructuring assignment trong JavaScript ES6 và các use cases thực tế"
image: "/images/blog/js-destructuring.png"
---

## Destructuring là gì?

Destructuring cho phép "giải nén" giá trị từ arrays hoặc properties từ objects thành các biến riêng biệt.

## Array Destructuring

### Cú pháp cơ bản

```javascript
// Cách cũ
const arr = [1, 2, 3];
const first = arr[0];
const second = arr[1];

// Destructuring
const [first, second, third] = [1, 2, 3];
console.log(first);  // 1
console.log(second); // 2
console.log(third);  // 3
```

### Skip elements

```javascript
const [first, , third] = [1, 2, 3];
console.log(first); // 1
console.log(third); // 3
```

### Default values

```javascript
const [a = 1, b = 2, c = 3] = [10, 20];
console.log(a); // 10
console.log(b); // 20
console.log(c); // 3 (default)
```

### Rest operator

```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
console.log(first); // 1
console.log(rest);  // [2, 3, 4, 5]
```

### Swap variables

```javascript
let a = 1;
let b = 2;

// Không cần biến tạm
[a, b] = [b, a];
console.log(a); // 2
console.log(b); // 1
```

## Object Destructuring

### Cú pháp cơ bản

```javascript
const user = {
  name: 'Bảo',
  age: 22,
  email: 'bao@example.com'
};

// Cách cũ
const name = user.name;
const age = user.age;

// Destructuring
const { name, age, email } = user;
console.log(name);  // 'Bảo'
console.log(age);   // 22
console.log(email); // 'bao@example.com'
```

### Đổi tên biến

```javascript
const { name: userName, age: userAge } = user;
console.log(userName); // 'Bảo'
console.log(userAge);  // 22
```

### Default values

```javascript
const { name, age, country = 'Vietnam' } = user;
console.log(country); // 'Vietnam'
```

### Nested destructuring

```javascript
const user = {
  name: 'Bảo',
  address: {
    city: 'TP.HCM',
    district: 'Quận 1'
  }
};

const {
  name,
  address: { city, district }
} = user;

console.log(city);     // 'TP.HCM'
console.log(district); // 'Quận 1'
```

### Rest operator

```javascript
const user = {
  name: 'Bảo',
  age: 22,
  email: 'bao@example.com',
  phone: '0123456789'
};

const { name, ...otherInfo } = user;
console.log(name);      // 'Bảo'
console.log(otherInfo); // { age: 22, email: '...', phone: '...' }
```

## Use Cases thực tế

### 1. Function parameters

```javascript
// ❌ Khó nhớ thứ tự tham số
function createUser(name, age, email, phone, address) {
  // ...
}
createUser('Bảo', 22, 'bao@...', '012...', 'TP.HCM');

// ✅ Dễ đọc hơn
function createUser({ name, age, email, phone, address }) {
  // ...
}
createUser({
  name: 'Bảo',
  email: 'bao@...',
  age: 22,
  phone: '012...',
  address: 'TP.HCM'
});
```

### 2. React props

```javascript
// Component với destructuring
function UserCard({ name, age, avatar }) {
  return (
    <div className="user-card">
      <img src={avatar} alt={name} />
      <h3>{name}</h3>
      <p>{age} tuổi</p>
    </div>
  );
}
```

### 3. API response

```javascript
async function getUser() {
  const response = await fetch('/api/user');
  const { data, status, message } = await response.json();
  
  if (status === 'success') {
    return data;
  }
  throw new Error(message);
}
```

### 4. Array methods

```javascript
const users = [
  { id: 1, name: 'An' },
  { id: 2, name: 'Bình' },
  { id: 3, name: 'Chi' }
];

// Destructuring trong map
const names = users.map(({ name }) => name);
console.log(names); // ['An', 'Bình', 'Chi']

// Destructuring trong forEach
users.forEach(({ id, name }) => {
  console.log(`${id}: ${name}`);
});
```

### 5. Importing modules

```javascript
// Named imports sử dụng destructuring
import { useState, useEffect } from 'react';
import { sum, multiply } from './utils';

// Rename khi import
import { sum as add } from './utils';
```

## Best Practices

1. **Đặt default values** để tránh undefined
2. **Đổi tên biến** khi bị conflict
3. **Không destructure quá sâu** - khó đọc
4. **Kết hợp với rest operator** để lấy phần còn lại

## Kết luận

Destructuring giúp code ngắn gọn, dễ đọc và dễ maintain hơn. Đây là một tính năng không thể thiếu trong JavaScript hiện đại!
