---
title: 如何去掉安卓微信浏览器里a标签的蓝边
date: 2015-08-03 15:40:48
categories:
- 前端开发
tags:
- 微信
- android
- css
---

在安卓微信中开发页面的同学都知道，点击a标签时会有一个非常难看的蓝色边框浮现出来，如下图:
![蓝色边框](/images/9YIWD]ZOAS1A_DXO7FH3.jpg "蓝色边框")

那么如何去掉这个不太招人待见的默认样式呢？代码如下：

```css
a {
    outline:none;
    -webkit-tap-highlight-color:rgba(0,0,0,0);
}
```