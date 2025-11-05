---
date: '2025-10-30T15:54:53+08:00'
draft: true
title: 'Bind和call和apple的区别'
tags: ['深入浅出', 'bind，acll，apply']
categories: ['js']
---

## 作用

都是改变函数执行时的上下文 == 改变函数运行时的 this 指向

```js
var name = 'lucy'
var obj = {
	name: 'martin',
	say: function () {
		console.log(this.name)
	},
}
obj.say() // martin，this 指向 obj 对象
setTimeout(obj.say, 0) // lucy，this 指向 window 对象
```

如果要按照理想情况输入
需要改变 this 的指向
setTimeout(obj.say.bind(obj),0);

## 区别

**apply**

接受两个参数，第一个参数是 this 的指向，第二个参数是函数接受的参数，以数组的形式传入

```js
fn.apply(obj,[1,2]); // this会变成传入的obj，传入的参数必须是一个数组；
fn(1,2) // this指向window

//当第一个参数为null、undefined的时候，默认指向window(在浏览器中)

fn.apply(null,[1,2]); // this指向window
fn.apply(undefined,[1,2]); // this指向window
```

**call**

call 方法的第一个参数也是 this 指向，后面传入的是一个参数列表

跟 apply 一样，改变 this 指向后原函数会立即执行，此方法只是临时改变this 指向一次
```js
function fn(...args){
    console.log(this,args);
}
let obj = {
    myname:"张三"
}

fn.call(obj,1,2); // this会变成传入的obj，传入的参数必须是一个数组；
fn(1,2) // this指向window

//当第一个参数为null、undefined的时候，默认指向window(在浏览器中)
fn.call(null,[1,2]); // this指向window
fn.call(undefined,[1,2]); // this指向window
```

**bind**

和 call 类似，第一参数也是 this 指向，后面传入一个参数列表（ 这个参数列表可以分多次传入 ）

改变 this 指向后不会立即执行，返回一个永久改变 this 指向的函数

```js
function fn(...args){
    console.log(this,args);
}
let obj = {
    myname:"张三"
}

const bindFn = fn.bind(obj); // this 也会变成传入的obj ，bind不是立即执行需要执行一次
bindFn(1,2) // this指向obj
fn(1,2) // this指向window
```

## 小结

区别：

- 三者都可以改变 this 的指向
- 三者第一个参数都是 this 要指向的对象，非指向的对象则默认指向全局 window
- 都可以传参 ， apply 为数组  call 为列表   都是一次性传入      bind 可以分多次传入
- bind 是返回绑定后的参数，  apply 和 call 是立即执行
