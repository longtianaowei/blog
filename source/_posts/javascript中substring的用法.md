---
title: javascript中substring的用法
date: 2024-10-23 11:31:46
tags:
---

# javascript中substring的用法

`substring` 是 JavaScript 字符串对象的一个方法，用于提取字符串中的一部分并返回一个新的字符串。它的基本语法如下：

```js
str.substirng(start,end)
```

- **`start`**：必需。提取的起始位置（包括该位置的字符）。如果 `start` 为负数，则会被视为 `0`。
- **`end`**：可选。提取的结束位置（**不包括该位置的字符**）。如果 `end` 为负数，则会被视为 `0`。如果省略 `end`，则提取从 `start` 到字符串末尾的所有字符。

#### 用法示例

1. **基本用法**

   ```js
   const str = "Hello, World!";
   const result = str.substring(7, 12); // 从索引 7 开始，到索引 12 结束（不包括索引 12）
   console.log(result); // 输出: "World"
   ```

2. **省略 `end` 参数**：

   ```js
   const str = "Hello, World!";
   const result = str.substring(7); // 从索引 7 开始，到字符串末尾
   console.log(result); // 输出: "World!"
   ```

3. **处理负数索引**：

   ```js
   const str = "Hello, World!";
   const result = str.substring(-3, 5); // 负数索引被视为 0
   console.log(result); // 输出: "Hello"
   ```





