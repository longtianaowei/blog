---
title: javascript之数组的各种操作
date: 2024-01-25 10:28:31
tags:
---

# javascript之数组的各种操作

**1.forEach**

用途：用于遍历数组的每个元素，**不会修改原始数组**

```javascript
const array1 = ['a', 'b', 'c'];
array1.forEach(element => console.log(element));
// 输出: "a" "b" "c"
```

**2.slice**

用途：返回数组的一部分，**不会修改原始数组**。

即数组切片

```javascript
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];
// 从数组索引2开始到数组结束（包含结束的元素）
// 输出: ["camel", "duck", "elephant"] 
console.log(animals.slice(2));
//从数组索引2开始到数组索引4（不包含索引4的元素）
// 输出: ["camel", "duck"] 
console.log(animals.slice(2, 4)); 
```

**3.splice**

用途：splice 方法通过删除、替换或添加新元素来修改数组，**会修改原始数组**

```javascript
const fruits = ['Banana', 'Orange', 'Apple', 'Mango'];
//从索引2开始，删除的元素个数为0（如果不填第二个参数则默认删除从指定索引开始后面的全部元素,ps：最好显示的指出要删除的个数)
const removed = fruits.splice(2, 0, 'Lemon', 'Kiwi'); 
// fruits = ['Banana', 'Orange', 'Lemon', 'Kiwi', 'Apple','Mango']
// removed = []，因为没有元素被删除

const removed1 = fruits.splice(2, 1, 'Lemon', 'Kiwi'); //从索引2开始，删除的元素个数为1
// fruits = ['Banana', 'Orange', 'Lemon', 'Kiwi','Mango']
// removed = ['Apple']，//删除了apple
```

**splice的错误用法**

大多数人理解下面这段代码为从数组索引2开始删除后面全部的元素并添加'Lemon', 'Kiwi'，

```javascript
const fruits = ['Banana', 'Orange', 'Apple', 'Mango'];
const removed = fruits.splice(2, 'Lemon', 'Kiwi'); 
```

预期结果为：

` fruits =['Banana','Orange','Lemon','Kiwi']` 

`removed = ['Apple', 'Mango']`

**大错特错** xxx

实际结果为：

`fruits =['Banana','Orange','Kiwi','Mango'] `

`removed = []`

原因：系统认为第二个参数为`Lemon`，而不是我们想的2后面为省略的参数，会执行删除后面的全部元素，事实上将Lemon作为了第二个参数，但是`Lemon`是字符串类型，JavaScript 会尝试将这个字符串转换为数字。由于 `'Lemon'` 不能被转换为一个合理的数字，它被转换为 `NaN`（Not a Number）。在 `splice` 方法中，如果 第二个参数是 `NaN`，它通常被视为 `0`所以删除了0个元素。

**4.reduce**

用途：将数组中的所有元素求和，**不会修改原始数组**

```javascript
const array1 = [1, 2, 3, 4];

// 0 + 1 + 2 + 3 + 4
const initialValue = 0;
const sum = array1.reduce(
  (accumulator, currentValue) => accumulator + currentValue,
);
console.log(sum);
// 输出: 10
```

`accumulator`

上一次调用 `callbackFn` 的结果。在第一次调用时，如果指定了 `initialValue` 则为指定的值，否则为 `array[0]` 的值。

`currentValue`

当前元素的值。在第一次调用时，如果指定了 `initialValue`，则为 `array[0]` 的值，否则为 `array[1]`。

**5.filter**

用途：用于过滤，创建一个新数组，包含通过提供的函数实现的测试的所有元素，**不会修改原始数组**

```javascript
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];
const result = words.filter(word => word.length > 6);
console.log(result); // 输出: ["exuberant", "destruction", "present"]
```

**6.map**

用途：创建一个新数组，其结果是该数组中的每个元素调用一次提供的函数后的返回值。**不会修改原始数组**

```javascript
const array1 = [1, 4, 9, 16];
const map1 = array1.map(x => x * 2);
console.log(map1); // 输出: [2, 8, 18, 32]
```

