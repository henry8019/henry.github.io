---
date: "2025-08-18T22:31:12+08:00"
draft: false
title: "vue-跨域"
tags: ["深入浅出", "跨域问题"]
categories: ["vue"]
---

## 一、跨域是什么

本质：浏览器基于同源策略的一种安全手段

同源策略：在同一个域

- 同源：
  - 协议相同 protocol
  - 主机相同 host
  - 端口相同 port

非同源请求时，产生跨域问题

## 二、如何解决

JSONP
CORS
Proxy

### CORS

`跨资源共享`，是一个系统，由一些列传输的 HTTP 头组成，决定浏览器是否阻止前端 JavaScript 代码获取跨域请求的响应

简而言之，前端增加一些 `HTTP` 头，让服务器能声明允许的访问来源，`后端`实现了 `CORS`，就实现了跨域

### Proxy

代理，网络代理，允许一个 （客户端）通过这个服务与另一个网络终端（服务器），进行非直接的连接。

### 一

通过 webpack 给本地服务器做一个请求的代理对象

通过服务器转发请求至目标服务器，得到的结果再转发给前端。

`vue.config.js`

```js
amodule.exports = {
	devServer: {
		host: "127.0.0.1",
		port: 8084,
		open: true, // vue项目启动时自动打开浏览器
		proxy: {
			"/api": {
				// '/api'是代理标识，用于告诉node，url前面是/api的就是使用代理的
				target: "http://xxx.xxx.xx.xx:8080", //目标地址，一般是指后台服务器地址
				changeOrigin: true, //是否跨域
				pathRewrite: {
					// pathRewrite 的作用是把实际Request Url中的'/api'用""代替
					"^/api": "",
				},
			},
		},
	},
};
```

通过 axios 发送请求，配置请求的根路径

```js
axios.defaults.baseURL = "/api";
```

### 二

通过服务器端实现代码请求转发

`express` 框架为例

```js
var express = require("express");
const proxy = require("http-proxy-middleware");
const app = express();
app.use(express.static(_dirname + "/"));
app.use(
	"/api",
	proxy({ target: "http://localhost:4000", changeOrigin: false })
);
module.exports = app;
```

### 三

配置 `nginx` 实现代理

```js
server {
    listen    80;
    # server_name www.josephxia.com;
    location / {
        root  /var/www/html;
        index  index.html index.htm;
        try_files $uri $uri/ /index.html;
    }
    location /api {
        proxy_pass  http://127.0.0.1:3000;
        proxy_redirect   off;
        proxy_set_header  Host       $host;
        proxy_set_header  X-Real-IP     $remote_addr;
        proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
    }
}
```
