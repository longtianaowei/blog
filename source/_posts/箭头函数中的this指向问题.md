---
title: 箭头函数中的this指向问题
date: 2024-07-30 11:18:24
tags:
---
箭头函数（Arrow Function）是ES6引入的一种简洁的函数定义方式。与传统的函数声明或函数表达式不同，箭头函数没有自己的 `this` 绑定。相反，箭头函数会捕获其定义时的上下文（即定义时的 `this` 值），并将这个值作为其 `this` 值。

**为什么？**

这是因为箭头函数的设计目的是为了简化上下文（context）的处理，特别是在需要嵌套回调函数的场景中，比如在回调函数中使用 `this` 时。

### 普通函数的 `this` 绑定

在普通的函数中，`this` 的值是在函数被调用时确定的，而不是在函数定义时确定的。也就是说，`this` 的值取决于函数的调用方式。常见的调用方式包括：

1. **全局调用**：在非严格模式下，`this` 指向全局对象（`window`）。在严格模式下，`this` 是 `undefined`。

   ```javascript
   function traditionalFunction() {
     console.log(this);
   }
   traditionalFunction(); // 在非严格模式下，输出 window 或 global。在严格模式下，输出 undefined。
   ```

2. **方法调用**：当函数作为对象的方法调用时，`this` 指向调用该方法的对象。

   ```javascript
   const obj = {
     method: function() {
       console.log(this);
     }
   };
   obj.method(); // 输出 obj 对象。
   ```

3. **构造函数调用**：当函数作为构造函数被调用时（使用 `new` 操作符），`this` 指向新创建的对象。

   ```javascript
   function ConstructorFunction() {
     console.log(this);
   }
   const instance = new ConstructorFunction(); // 输出新创建的 instance 对象。
   ```

4. **`call` 和 `apply` 调用**：使用 `call` 或 `apply` 方法调用函数时，`this` 的值由第一个参数显式指定。

   ```javascript
   function traditionalFunction() {
     console.log(this);
   }
   const obj = {};
   traditionalFunction.call(obj); // 输出 obj 对象。
   ```

### 箭头函数的 `this` 绑定

箭头函数没有自己的 `this` 绑定，它会继承定义时外层上下文的 `this`。具体来说，箭头函数的 `this` 值是由其词法作用域（lexical scope）决定的，即定义箭头函数时所在的那个作用域的 `this`。

```javascript
const obj = {
  name: 'Alice',
  regularFunction: function() {
    console.log(this); // 输出 obj
    return function() {
      console.log(this); // 输出全局对象（非严格模式）或 undefined（严格模式）
    };
  },
  arrowFunction: function() {
    console.log(this); // 输出 obj
    return () => {
      console.log(this); // 输出 obj，因为箭头函数继承了 arrowFunction 的 this
    };
  }
};

const regular = obj.regularFunction();
regular(); // 调用返回的函数

const arrow = obj.arrowFunction();
arrow(); // 调用返回的箭头函数
```

在上述例子中，`regularFunction` 中返回的普通函数其 `this` 是在调用时确定的，因此在全局环境中调用时会指向全局对象。而 `arrowFunction` 中返回的箭头函数其 `this` 是在定义时确定的，因此始终指向定义它时的 `obj`。

### 为什么箭头函数设计成这样的 `this` 绑定

这种设计使得箭头函数在编写需要嵌套回调的代码时更加简洁，不需要使用 `self = this` 经常可以在代码中看到 `that = this` 一样的，或者 `.bind(this)` 等技巧来保持上下文的正确性。举例来说：

**使用箭头函数**

```javascript
function Timer() {
  this.seconds = 0;
  setInterval(() => {
    this.seconds++;
    console.log(this.seconds);
  }, 1000);
}

const timer = new Timer();
```

使用普通函数

```javascript
方法一：保存 this 的引用
function Timer() {
  this.seconds = 0;
  const that = this; // 保存 this 的引用
  setInterval(function() {
    that.seconds++;
    console.log(self.seconds);
  }, 1000);
}

const timer = new Timer();

方法二：使用 bind 方法
function Timer() {
  this.seconds = 0;
  setInterval(function() {
    this.seconds++;
    console.log(this.seconds);
  }.bind(this), 1000); // 将 this 绑定到回调函数
}

const timer = new Timer();
```

在上述代码中，如果使用普通函数，则需要额外的步骤来确保 `this` 指向 `Timer` 实例。使用箭头函数可以自动捕获外层函数的 `this`，避免了这种额外的复杂性。



总结箭头函数this关键字的指向是在定义时就确定了，普通函数的this指向是在调用时确定的。