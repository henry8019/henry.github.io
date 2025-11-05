---
date: '2025-11-04T10:32:40+08:00'
draft: true
title: 'Vue 实例挂载过程'
tags: ['Vue', '实例挂载']
categories: ['vue']
---

## 分析

-   new Vue 的时候调用会调用\_init 方法
    -   定义 $set、$get 、$delete、$watch 等方法
    -   定义 $on、$off、$emit、$off 等事件
-   定义 \_update、$forceUpdate、$destroy 生命周期
-   调用$mount 进行页面的挂载

-   挂载的时候主要是通过 mountComponent 方法

-   定义 updateComponent 更新函数

-   执行 render 生成虚拟 DOM

-   \_update 将虚拟 DOM 生成真实 DOM 结构，并且渲染到页面中
