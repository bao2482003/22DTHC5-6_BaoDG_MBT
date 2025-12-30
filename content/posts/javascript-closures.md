---
title: "JavaScript: Closures và Quản Lý Bộ Nhớ"
date: 2025-12-27
tags: ["JavaScript", "Advanced"]
description: "Closure là gì và tại sao nó quan trọng trong JavaScript"
image: "https://images.unsplash.com/photo-1605379399642-870262d3d051?w=1200&h=600&fit=crop"
---

## Closure là gì?

Closure là một function có thể truy cập biến từ scope bên ngoài của nó, ngay cả sau khi function bên ngoài đã return.

```javascript
function outer() {
  const message = 'Hello';
  
  function inner() {
    console.log(message); // Truy cập được biến message
  }
  
  return inner;
}

const myFunc = outer();
myFunc(); // 'Hello' - closure giữ reference đến message
```

## Tại sao Closures quan trọng?

### 1. Data Privacy / Encapsulation

```javascript
function createCounter() {
  let count = 0; // Private variable
  
  return {
    increment() {
      count++;
      return count;
    },
    decrement() {
      count--;
      return count;
    },
    getCount() {
      return count;
    }
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
// count không thể truy cập trực tiếp từ bên ngoài
```

### 2. Function Factories

```javascript
function createMultiplier(multiplier) {
  return function(number) {
    return number * multiplier;
  };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

### 3. Event Handlers

```javascript
function setupButton(buttonId, message) {
  const button = document.getElementById(buttonId);
  
  button.addEventListener('click', function() {
    // Closure giữ reference đến message
    alert(message);
  });
}

setupButton('btn1', 'Button 1 clicked!');
setupButton('btn2', 'Button 2 clicked!');
```

## Closure trong Loops - Vấn đề phổ biến

### ❌ Vấn đề với var

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i); // 3, 3, 3 (không phải 0, 1, 2)
  }, 1000);
}
```

### ✅ Giải pháp 1: IIFE

```javascript
for (var i = 0; i < 3; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j); // 0, 1, 2
    }, 1000);
  })(i);
}
```

### ✅ Giải pháp 2: let (ES6)

```javascript
for (let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i); // 0, 1, 2
  }, 1000);
}
```

## Use Cases thực tế

### 1. Module Pattern

```javascript
const Calculator = (function() {
  // Private variables
  let result = 0;
  
  // Private function
  function log(operation, value) {
    console.log(`${operation}: ${value}`);
  }
  
  // Public API
  return {
    add(num) {
      result += num;
      log('Add', num);
      return this;
    },
    subtract(num) {
      result -= num;
      log('Subtract', num);
      return this;
    },
    getResult() {
      return result;
    },
    reset() {
      result = 0;
      return this;
    }
  };
})();

Calculator.add(10).subtract(3).add(5);
console.log(Calculator.getResult()); // 12
```

### 2. Memoization (Caching)

```javascript
function memoize(fn) {
  const cache = {};
  
  return function(...args) {
    const key = JSON.stringify(args);
    
    if (cache[key]) {
      console.log('From cache');
      return cache[key];
    }
    
    console.log('Calculating...');
    const result = fn(...args);
    cache[key] = result;
    return result;
  };
}

const fibonacci = memoize(function(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
});

console.log(fibonacci(10)); // Calculating...
console.log(fibonacci(10)); // From cache
```

### 3. Debounce

```javascript
function debounce(func, delay) {
  let timeoutId;
  
  return function(...args) {
    clearTimeout(timeoutId);
    
    timeoutId = setTimeout(() => {
      func.apply(this, args);
    }, delay);
  };
}

// Sử dụng
const searchInput = document.getElementById('search');
const handleSearch = debounce(function(event) {
  console.log('Searching for:', event.target.value);
}, 500);

searchInput.addEventListener('input', handleSearch);
```

### 4. Partial Application

```javascript
function partial(fn, ...fixedArgs) {
  return function(...remainingArgs) {
    return fn(...fixedArgs, ...remainingArgs);
  };
}

function greet(greeting, name) {
  return `${greeting}, ${name}!`;
}

const sayHello = partial(greet, 'Hello');
const sayHi = partial(greet, 'Hi');

console.log(sayHello('Bảo')); // 'Hello, Bảo!'
console.log(sayHi('An'));     // 'Hi, An!'
```

## Memory Considerations

Closures giữ reference đến outer scope, có thể gây memory leak nếu không cẩn thận:

```javascript
// ❌ Memory leak
function setupButton() {
  const data = new Array(1000000).fill('data');
  
  document.getElementById('btn').addEventListener('click', function() {
    console.log('Clicked');
    // Closure giữ reference đến data dù không dùng
  });
}

// ✅ Tốt hơn
function setupButton() {
  const data = new Array(1000000).fill('data');
  const dataLength = data.length; // Chỉ giữ những gì cần
  
  document.getElementById('btn').addEventListener('click', function() {
    console.log('Clicked, data length:', dataLength);
  });
}
```

## Best Practices

1. **Chỉ closure những gì cần thiết** - tránh giữ reference không cần
2. **Dùng let/const trong loops** thay vì var
3. **Clean up event listeners** để tránh memory leaks
4. **Document closure dependencies** rõ ràng

## Kết luận

Closures là một trong những khái niệm quan trọng nhất trong JavaScript. Hiểu rõ closures giúp bạn viết code mạnh mẽ, maintainable và tận dụng được sức mạnh của JavaScript!
