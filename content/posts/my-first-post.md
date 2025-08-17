---
title: "My First Post"
date: 2025-08-17T12:00:00+08:00
draft: false
tags: ["demo", "博客搭建"]
categories: ["通用技术"]
---

## 这是我的第一篇文章

这是我用 **Hugo + PaperMod** 发布的第一篇文章。

- 支持 Markdown
- 支持代码高亮
- 支持图片插入

```css
body {
  font-family: 'JetBrains Mono', monospace;
}
h1, h2, h3 {
  color: #333;
}
code {
  background-color: #f4f4f4;
  padding: 5px;
  border-radius: 3px;
}
```


## 理解

题意比较简单，时间长没有碰数学者或许需要一两分钟回想一下矩阵的乘法。

可以看一下同济大学的线代教材中给出的矩阵的相乘的公式，

![](https://i.postimg.cc/KYT3BXGx/image.png)

看一下输入和输出，

**输入**

这里有个术语：EBNF，这是编译原理里面的一个很简单的一个范式，上过课的应该都有印象，如果需要，再去翻一下书即可。

注意这个条件，

```txt
Expression = Matrix | "(" Expression Expression ")"
```

可以推出，括号里面只可以出现一对单独的矩阵，也就是两个单独的矩阵，e.g. `(AB)`，或者，一个矩阵加上另一对括号括起来的矩阵，e.g. `(A(AB))`，以此类推。因此，我们可以在遇到右括号的时候，一次出栈两个栈中的元素，

```cpp
else if (expr[i] == ')') {
  Matrix m2 = s.top();
  s.pop();
  Matrix m1 = s.top();
  s.pop();
  ...
}
```

**输出**

理解一下题目中的例子即可。

## 代码

```cpp
#include <cstdio>
#include <cctype>
#include <stack>
#include <iostream>
#include <string>

using namespace std;

struct Matrix {
  int a, b;
  Matrix(int a = 0, int b = 0) : a(a), b(b) {}
} m[26];

stack<Matrix> s;

int main(int argc, char *argv[]) {
  int n;
  cin >> n;
  for (int i = 0; i < n; i++) {
    string name;
    cin >> name;
    int k = name[0] - 'A';
    cin >> m[k].a >> m[k].b;
  }
  string expr;
  while (cin >> expr) {
    int len = expr.length();
    bool error = false;
    int ans = 0;
    for (int i = 0; i < len; i++) {
      if (isalpha(expr[i]))
        s.push(m[expr[i] - 'A']);
      else if (expr[i] == ')') {
        Matrix m2 = s.top();
        s.pop();
        Matrix m1 = s.top();
        s.pop();
        if (m1.b != m2.a) {
          error = true;
          break;
        }
        ans += m1.a * m2.a * m2.b;
        s.push(Matrix(m1.a, m2.b));
      }
    }
    if (error)
      cout << "error\n";
    else
      cout << ans << "\n";
  }
  return 0;
}
```

![](https://i.postimg.cc/Bbz12JKZ/image.png)

或许也可以让 chatgpt 的访问更加丝滑。

起因是在看 react 的官方教程时，发现有一张图片无法访问，对，就是下面这张，

![](https://i.imgur.com/yXOvdOSs.jpg)

其实我最近两年的 imgur 的访问一直比较有问题，有时候需要换很多个节点才能尝试出来一个可以正常加载 imgur 图片的 ip，而有时候甚至会全军覆没。

今天实在忍不了了，所以，就在网上找了一下有没有好的方法可以实现 imgur 的图片的正常访问，因为这个图床(图片托管网站)用到的地方挺多的，比如，像上面的 reactjs 的官网都会用到，还有 v 站也是默认会渲染 imgur 的图片，我自己的博客之前也是用 imgur 比较多，现在则换成了访问更加顺畅的 postimage，不过，我还有很多图片没有迁移过去，所以，解决 imgur 这个图床的问题迫在眉睫。找了一圈，发现还是用 warp 套一层比较合适。

那么，具体是怎么操作呢？

经过我的尝试，发现使用第三方开源的 [cli](https://github.com/ViRb3/wgcf) 而非是 cloudflare 的官方 cli 在 Arch 上的体验是更加丝滑的，具体可以参考 Reddit 的这个帖子，

<https://www.reddit.com/r/CloudFlare/comments/q0hqj4/warpcli_is_not_working/>

具体来讲，就是先安装几个依赖，