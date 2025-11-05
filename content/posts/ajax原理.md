---
date: '2025-10-30T10:08:00+08:00'
draft: true
title: 'Ajax原理'
tags: ['深入浅出', 'Ajax原理']
categories: ['网页技术']
---

## Ajax

全称：Async Javascript and XML

原理：通过 XmlHttpRequest 对象向服务器发起异步请求，从服务器获取数据，然后用 javascript 来操作更新 demo

领导要小李来汇报工作，委托秘书去叫小李，自己接着做其他事情，直到秘书告诉领导小李到了，最后小李跟领导汇报工作

秘书：XMLHttpRequest 对象

领导：浏览器

响应数据：小李

## 实现过程

实现 Ajax 异步交换需要无夫妻逻辑进行配合

1 创建 Ajax 的核心对象 XMLHttpRequest 对象

2 通过 XmlHttpRequest 对象的 open（）方法与服务端建立连接

3 构建请求所需的数据内容，并通过 XMLHttpRequest 对象的 send（）方法发送给服务器端

4 通过 XmlHttpRequest 对象提供的 onreadystatechange 事件监听服务器端你的通信状态

5 接受并处理服务器端向客户端响应的数据结果

6 将处理结果更新到 HTML 页面中

```javascript
//创建 XmlHttpRequest对象
const xhr=new XMLHttpRequest()

//与服务器建立连接
xhr.open(method,url,[async],[user],[password])

method：表示当前的请求方式，常见的有GET、POST

url：服务端地址

async：布尔值，表示是否异步执行操作，默认为true

user: 可选的用户名用于认证用途；默认为 null

password: 可选的密码用于认证用途，默认为 null

给服务器发送数据：

xhr.send([body])

body: 在 XHR 请求中要发送的数据体，如果不传递数据则为 null

如果使用GET请求发送数据的时候，需要注意如下：

将请求数据添加到open()方法中的url地址中
发送请求数据中的send()方法中参数设置为null
```

### 绑定 onreadystatechange 事件

该事件用于监听服务器端的通信状态，监听的属性为 XMLHttpRequest.readyState

example:

```javascript
const request = new XMLHttpRequest()
request.onreadystatechange = function (e) {
	if (request.readyState === 4) {
		// 整个请求过程完毕
		if (request.status >= 200 && request.status <= 300) {
			console.log(request.responseText) // 服务端返回的结果
		} else if (request.status >= 400) {
			console.log('错误信息：' + request.status)
		}
	}
}
request.open('POST', 'http://xxxx')
request.send()
```

## 封装

封转一个简单的 ajax 请求

```javascript
function ajax(options){
    //创建 XmlHttpRequest对象
    const xhr=new XMLHttpRequest()

    options=options||{}
    options.type=(options.type||'Get').toUpperCase()
    options.dataType=options.dataType||'json'
    const params=options.data

    //发送请求
    if(options.type==='GET'){
       xhr.open('GET', options.url + '?' + params, true)
        xhr.send(null)
    } else if (options.type === 'POST') {
        xhr.open('POST', options.url, true)
        xhr.send(params)

    //接收请求
    xhr.onreadystatechange = function () {
        if (xhr.readyState === 4) {
            let status = xhr.status
            if (status >= 200 && status < 300) {
                options.success && options.success(xhr.responseText, xhr.responseXML)
            } else {
                options.fail && options.fail(status)
            }
        }
    }

}
```

使用方法：

```js
ajax({
	type: 'post',
	dataType: 'json',
	data: {},
	url: 'https://xxxx',
	success: function (text, xml) {
		//请求成功后的回调函数
		console.log(text)
	},
	fail: function (status) {
		////请求失败后的回调函数
		console.log(status)
	},
})
```
