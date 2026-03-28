---
title: JavaScript中split()方法
date: 2024-01-25 10:44:00
tags:
---

大白话，主要用来`字符串`转`数组`

`split()` 方法是一个非常有用的字符串方法，它用于将字符串分割成子字符串数组，然后返回这个新的数组。

**直接上例子**：

1. **基本分割**：用特定字符分割字符串。

   ```javascript
   var str = "apple,banana,orange";
   var fruits = str.split(','); 
   // fruits 的值为 ["apple", "banana", "orange"]
   ```

2. **使用空字符串分割**：将每个字符分开。

   ```javascript
   var str = "hello";
   var chars = str.split(''); 
   // chars 的值为 ["h", "e", "l", "l", "o"]
   ```

3. **使用正则表达式分割**：根据匹配的正则表达式来分割字符串。

   ```javascript
   var str = "The quick brown fox jumps";
   var words = str.split(/\s/); 
   // words 的值为 ["The", "quick", "brown", "fox", "jumps"]
   ```

4. **限制分割后数组的大小**：指定返回数组的最大长度。

   ```javascript
   var str = "one,two,three,four";
   var limited = str.split(',', 2); 
   // limited 的值为 ["one", "two"]
   ```
