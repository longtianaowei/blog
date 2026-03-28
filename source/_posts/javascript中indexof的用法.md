---
title: javascript中indexof的用法
date: 2024-10-23 11:31:09
tags:
---

# javascript中indexof的用法

`indexOf` 是 JavaScript 字符串对象的一个方法，用于查找指定子字符串在字符串中首次出现的位置。如果未找到该子字符串，则返回 `-1`。它的基本语法如下：

```
str.indexOf(searchValue, fromIndex)
```

- **`searchValue`**：必需。要查找的子字符串。
- **`fromIndex`**：可选。从字符串中的哪个位置开始查找。默认值为 `0`。如果 `fromIndex` 为负数，则会被视为 `0`。

1. **基本用法**

   ```js
   const str = "Hello, World!";
   const index = str.indexOf("World");
   console.log(index); // 输出: 7
   
   const index2 = str.indexOf("Wo");
   console.log(index2); // 输出: 7
   ```

2. **指定起始位置**：

   ```js
   const str = "Hello, World!";
   const index = str.indexOf("o", 5);
   console.log(index); // 输出: 8
   ```

3. **未找到子字符串**：

   ```js
   const str = "Hello, World!";
   const index = str.indexOf("Universe");
   console.log(index); // 输出: -1
   ```

4. **处理负数索引**：

   ```js
   const str = "Hello, World!";
   const index = str.indexOf("o", -3);
   console.log(index); // 输出: 4
   ```