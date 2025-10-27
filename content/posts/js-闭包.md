---
date: "2025-08-18T22:31:12+08:00"
draft: false
title: "js-闭包与作用域"
tags: ["深入浅出", "闭包", "作用域"]
categories: ["js"]
---

## 闭包

    一个函数对其周围状态的引用，捆绑在一起。或者说函数被引用包围，这样的组合就是闭包

```js
function outer() {
	let a = 10;
	return function fn() {
		console.log(a);
	};
}

const fn = outer();
```

- 封装以及数据的私有化
- 可能会造成内存泄露

---

```js
function makeAdder(x) {
	return function (y) {
		return x + y;
	};
}
const add5 = makeAdder(5);
const add7 = makeAdder(7);
log(add5(3)); //8
log(add7(5)); //12
```

- 共享相同的函数体，保存不同的词法环境

---

## 作用域

    程序中变量或函数名称的有效可见范围

1. 全局作用域 2.模块作用域 3.函数作用域
