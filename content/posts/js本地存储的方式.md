---
date: '2025-10-31T16:23:06+08:00'
draft: true
title: 'Js本地存储的方式'
tags: ['深入浅出', 'Js本地存储的方式']
categories: ['js']
---

## 方式

以下四种
`cookie`,
`sessionStorage`,
`localStorage`,
`indexedDB`

## cookie

cookie 其类型为小型文本文件，一般为辨别用户身份 `存储在用户本地终端上的` 数据 为了解决 http 无状态导致的问题

一段一般不超过 4KB 的`小型文本数据`，它由一个名称（Name）、一个值（Value）和其它几个用于控制 cookie 有效期、安全性、使用范围的可选属性组成

cookie 在`每次请求中都会被发送`，如果不使用 HTTPS 并对其加密，其保存的信息很容易被窃取，

## localStorage

特点：

-   生命周期：持久化存储，除非主动删除，否则数据不会过期
-   存储的信息在同一域是共享的
-   页面增删改 了 loaclstorage 的时候，本页面不会触发 storage 事件
-   loacalstorage 本质上是对字符串的读取，存储过多会消耗内存空间
-   受同源策略影响

use localStorage:

```js
//setting
localStorage.setItem('username', 'cfangxu')

//get
localStorage.getItem('username')

//get keys
localStorage.key(0) //获取第一个键名

//delete
localStorage.removeItem('username')

//clear all  setting
localStorage.clear()
```

## sessionStorage

sessionStorage 和 localStorage 使用方法基本一致，唯一不同的是生命周期，一旦页面（会话）关闭，sessionStorage 将会删除数据

## 区别

关于 cookie、sessionStorage、localStorage 三者的区别主要如下：

-   存储大小：cookie 数据大小不能超过 4k，sessionStorage 和 localStorage 虽然也有存储大小的限制，但比 cookie 大得多，可以达到 5M 或更大

-   有效时间：localStorage 存储持久数据，浏览器关闭后数据不丢失除非主动删除数据； sessionStorage 数据在当前浏览器窗口关闭后自动删除；cookie 设置的 cookie 过期时间之前一直有效，即使窗口或浏览器关闭

-   数据与服务器之间的交互方式，cookie 的数据会自动的传递到服务器，服务器端也可以写 cookie 到客户端； sessionStorage 和 localStorage 不会自动把数据发给服务器，仅在本地保存

# #应用场景

-   标记用户与跟踪用户行为的情况，推荐使用 cookie
-   适合长期保存在本地的数据（令牌），推荐使用 localStorage
-   敏感账号一次性登录，推荐使用 sessionStorage
-   存储大量数据的情况、在线文档（富文本编辑器）保存编辑历史的情况，推荐使用 indexedDB
