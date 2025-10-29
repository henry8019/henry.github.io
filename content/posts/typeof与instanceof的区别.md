---
date: "2025-10-29T17:55:35+08:00"
draft: true
title: "typeof与instanceof的区别"
tags: ["深入浅出", "typeof与instanceof的区别"]
categories: ["js"]
---

## typeof

其返回一个字符串，表示未经计算的操作数的类型

example：

```js
typeof 1; //number
typeof "1"; // 'string'
typeof undefined; // 'undefined'
typeof true; // 'boolean'
typeof Symbol(); // 'symbol'
typeof null; // 'object'   老 bug
typeof []; // 'object'
typeof {}; // 'object'
typeof console; // 'object'
typeof console.log; // 'function'
```

判断一个变量是否存在可以用 typeof

## instanceof

其 用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上

返回的值是一个布尔值

object instanceof constructor

```js
// 定义构建函数
let Car = function () {};
let benz = new Car();
benz instanceof Car; // true
let car = new String("xxx");
car instanceof String; // true
let str = "xxx";
str instanceof String; // false
```

instanceof 的实现原理：大概是顺着原型链去找，直到找到相同的原型对象
