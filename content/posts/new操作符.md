---
date: '2025-10-29T18:21:09+08:00'
draft: true
title: 'new操作符'
tags: ['深入浅出', 'new操作符']
categories: ['js']
---

## new 操作符

用于创建一个给定构造函数的实例对象

example:

```js
function Person(name, age) {
	this.name = name
	this.age = age
}
Person.prototype.sayName = function () {
	console.log(this.name)
}
const person1 = new Person('Tom', 20)
console.log(person1) // Person {name: "Tom", age: 20}
t.sayName() //Tom
```

-   new 通过构造函数 Person 创建出来的实例可以访问到构造函数中的属性
-   new 通过构造函数 Person 创建出来的实例可以访问到构造函数原型链中的属性（即实例与构造函数通过原型链连接了起来）

## 流程

-   创建一个新的对象 obj

-   将对象与构建函数通过原型链连接起来

-   将构建函数中的 this 绑定到新建的对象 obj 上

-   根据构建函数返回类型作判断，如果是原始值则被忽略，如果是返回对象，需要正常处理

## 手写 new 操作符

```js
function mynew(Func, ...args) {
	// 1.创建一个新对象
	const obj = {}
	// 2.新对象原型指向构造函数原型对象
	obj.__proto__ = Func.prototype
	// 3.将构建函数的this指向新对象
	let result = Func.apply(obj, args)
	// 4.根据返回值判断
	return result instanceof Object ? result : obj
}
```

```js
function mynew(func, ...args) {
	const obj = {}
	obj.__proto__ = func.prototype
	let result = func.apply(obj, args)
	return result instanceof Object ? result : obj
}
function Person(name, age) {
	this.name = name
	this.age = age
}
Person.prototype.say = function () {
	console.log(this.name)
}

let p = mynew(Person, 'huihui', 123)
console.log(p) // Person {name: "huihui", age: 123}
p.say() // huihui
```
