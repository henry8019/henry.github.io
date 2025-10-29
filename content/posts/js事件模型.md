---
date: "2025-10-29T16:41:36+08:00"
draft: true
title: "Js事件模型"
tags: ["深入浅出", "事件模型"]
categories: ["js"]
---

## 事件与事件流

DOM 是一个树结构，如果父子节点绑定事件的收获，当触发子节点的时候，存在一个顺序问题，涉及到事件流概念

事件流必经历三个阶段：

- 事件捕获阶段
- 处于目标阶段
- 事件冒泡阶段

事件冒泡：是一种从下往上的传播方式，由具体的元素逐渐向上传播到最不具体的那个节点，也就是 dom 中最高层的父节点

事件捕获与之相反，由不太具体的节点最早接受事件，最具体的节点最后接受事件

## 事件模型

分三种：

- 原始事件模型
- 标准事件模型
- IE 事件模型

### 原始事件模型

HTML 代码中直接绑定：
<input type="button" onclick="fun()">

通过 js 代码中直接绑定

```js
let btn = document.getElementById(".btn");
btn.onclick = fun;
```

`特性`:绑定速度快
只支持冒泡不支持捕获
同一个类型事件只能绑定一次

## 标准事件模型：

该模型中，一次事件共有三个模型

- 事件捕获阶段：事件从 document 一直向下传播到目标元素，以此检查经过节点是否绑定了事件监听函数，如果有则执行
- 事件处理阶段：事件到达了目标元素，触发目标元素的监听函数
- 事件冒泡阶段：事件从目标元素冒泡到 document，一次检查经过的节点是否绑定了事件监听函数，有则执行

```js
事件绑定监听函数的方法;

addEventListener(eventType, handler, useCapture);

事件移除监听函数的方法;
removeEventListener(eventType, handler, useCapture);
```

- eventType 指定事件类型(不要加 on)
- handler 是事件处理函数
- useCapture 是一个 boolean 用于指定是否在捕获阶段进行处理，一般设置为 false 与 IE 浏览器保持一致

example：

```js
const btn = document.getElementById(".btn");
btn.addEventListener("click", showMessage, false);
btn.removeEventListener("click", showMessage, false);
```

`特性`：一个 dom 元素可以绑定多个事件处理器，不会冲突

执行时机：第三个参数 useCapture 设置为 true 就在捕获过程中执行，反之在冒泡过程中执行

example：

```js
<div id='div'>
    <p id='p'>
        <span id='span'>Click Me!</span>
    </p >
</div>

var div = document.getElementById('div');
var p = document.getElementById('p');

function onClickFn (event) {
    var tagName = event.currentTarget.tagName;
    var phase = event.eventPhase;
    console.log(tagName, phase);
}

div.addEventListener('click', onClickFn, false);
p.addEventListener('click', onClickFn, false);

1 为捕获阶段
2 为事件触发阶段
3 为冒泡阶段




输出：P 3
     DIV 3

为 true 时：

输出： DIV 1
       P 1
```

## IE 事件模型

此模型有两个过程

- 事件处理阶段： 时间到达目标元素，触发目标元素的监听函数
- 事件冒泡阶段： 事件从目标元素冒泡到 document，一次检查经过的节点是否绑定了事件监听函数，有则执行

事件绑定监听函数的方法：
attchEvent（eventType，handler）

事件移除监听函数的方法：
detachEvent（eventType，handler）

example：

```js
var btn = document.getElementById('.btn');
btn.attachEvent(‘onclick’, showMessage);
btn.detachEvent(‘onclick’, showMessage);
```
