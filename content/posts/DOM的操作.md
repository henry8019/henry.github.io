---
date: '2025-10-31T15:53:32+08:00'
draft: true
title: 'DOM的操作'
tags: ['深入浅出', 'DOM']
categories: ['DOM']
---

## 常见操作

-   创建节点
-   查询节点
-   更新节点
-   添加节点
-   删除节点

```js

const divEl = document.createElement("div");

const divEl = document.createElement("div");

//创建一个文档碎片
const fragment = document.createDocumentFragment();

//创建属性节点
const dataAttribute = document.createAttribute('custom');

//获取节点
document.querySelector('.element')
document.querySelector('#element')
document.querySelector('div')
document.querySelector('[name="username"]')
document.querySelector('div + p > span')

document.getElementById('id属性值');
//返回拥有指定id的对象的引用

document.getElementsByClassName('class属性值');
<!-- 返回拥有指定class的对象集合 -->

document.getElementsByTagName('标签名');
<!-- 返回拥有指定标签名的对象集合 -->

document.getElementsByName('name属性值');
<!-- 返回拥有指定名称的对象结合 -->

document/element.querySelector('CSS选择器');
<!-- 仅返回第一个匹配的元素 -->

document/element.querySelectorAll('CSS选择器');
<!-- 返回所有匹配的元素 -->

document.documentElement;
<!-- 获取页面中的HTML标签 -->

document.body;
<!-- 获取页面中的BODY标签 -->

document.all[''];
<!-- 获取页面中的所有元素节点的对象集合型 -->



--- 
//更新节点
innerHTML()

//添加节点
appendChild()

//
删除节点
removeChild()


```
