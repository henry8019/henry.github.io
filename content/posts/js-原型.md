---
date: "2025-08-18T22:31:12+08:00"
draft: false
title: "js-原型"
tags: ["深入浅出", "原型"]
categories: ["js"]
---

## 一、原型

### `javascript` 被描述为一种基于原型的语言，即 每一个对象都有一个原型对象

当访问一个对象的属性时，不仅对该对象上搜寻，还会搜寻该对象的原型，以及对象原型的原型，依次层层向上搜寻，直到找到一个名字匹配的属性或着到达`原型链`的末尾

eg:

```javascript
function fn()
log(fu.prototype)
```

控制台输出

```js
{
constructor: ƒ doSomething(),
    __proto__: {
        constructor: ƒ Object(),
        hasOwnProperty: ƒ hasOwnProperty(),
        isPrototypeOf: ƒ isPrototypeOf(),
        propertyIsEnumerable: ƒ propertyIsEnumerable(),
        toLocaleString: ƒ toLocaleString(),
        toString: ƒ toString(),
        va
        lueOf: ƒ valueOf()
    }
}
```

以上，为原型对象
原型对象有一个 constructor,该属性指向该函数

## 二、原型链

    1、原型对象也可能有原型，并继承其方法和属性，一层一层这种关系被称作原型链
    2、对象实例和它的构造器之间建立了一个链接   _proto_ 属性 是从构造函数的prototype属性派生
