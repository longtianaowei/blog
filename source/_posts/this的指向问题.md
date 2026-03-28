---
title: this的指向问题
date: 2024-02-29 19:57:31
tags:
---

# this的指向问题

##  1.全局执行环境

### 	1.1非严格模式 为window

```javascript
console.log(this)
```

###      1.2严格模式 为window   

```javascript
 'use strict'
console.log(this)
```

##  2.直接调用

###    2.1非严格模式 为window

```javascript
function func() {
	console.log(this);
}
func() 
```

### 	2.2严格模式 为undefind

```javascript
 function func() {
 'use strict'
 	console.log(this)
 }
 func() 
```

## 3.对象调用

### 3.1非严格模式 为调用的对象

```javascript
const food = {
	name:'小面',
	eat() {
		console.log(this)
	}
}
food.eat()
```

### 3.2严格模式 为调用的对象

```javascript
const food = {
	name:'小面',
	eat() {
    'use strict'
		console.log(this)
	}
}
food.eat()
```
