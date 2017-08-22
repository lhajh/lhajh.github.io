---
layout: post
title: CSS cursor 属性
categories: CSS
description: CSS cursor 属性
keywords: CSS, cursor
---

CSS cursor 属性

## 定义和用法
cursor 属性规定要显示的光标的类型（形状）。
该属性定义了鼠标指针放在一个元素边界范围内时所用的光标形状（不过 CSS2.1 没有定义由哪个边界确定这个范围）。

| 值 fdsaf |描述 |
| :---: | :--- |
| url	| 需使用的自定义光标的 URL。注释：请在此列表的末端始终定义一种普通的光标，以防没有由 URL 定义的可用光标。
| default	| 默认光标（通常是一个箭头）
|auto	|默认。浏览器设置的光标。
|crosshair	|光标呈现为十字线。
|pointer	|光标呈现为指示链接的指针（一只手）
|move	|此光标指示某对象可被移动。
|e-resize	|此光标指示矩形框的边缘可被向右（东）移动。
|ne-resize	|此光标指示矩形框的边缘可被向上及向右移动（北/东）。
|nw-resize	|此光标指示矩形框的边缘可被向上及向左移动（北/西）。
|n-resize	|此光标指示矩形框的边缘可被向上（北）移动。
|se-resize	|此光标指示矩形框的边缘可被向下及向右移动（南/东）。
|sw-resize	|此光标指示矩形框的边缘可被向下及向左移动（南/西）。
|s-resize	|此光标指示矩形框的边缘可被向下移动（南）。
|w-resize	|此光标指示矩形框的边缘可被向左移动（西）。
|text	|此光标指示文本。
|wait	|此光标指示程序正忙（通常是一只表或沙漏）。
|help	|此光标指示可用的帮助（通常是一个问号或一个气球）。
|not-allowed  |此光标指示禁用样式
| no-drop  |此光标指示禁用样式

注：not-allowed跟pointer-events同时使用会影响cursor的形状，比如`pointer-events:none;cursor:not-allowed;`后,cursor:not-allowed的鼠标形状不会改变为红色的失效图型，而只是默认的鼠标图形了。