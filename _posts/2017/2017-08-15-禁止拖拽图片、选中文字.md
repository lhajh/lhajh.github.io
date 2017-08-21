---
layout: post
title: 禁止拖拽图片、选中文字
categories: CSS
description: 禁止拖拽图片、选中文字
keywords: 禁止拖拽图片, 选中文字
---

利用CSS禁止拖拽图片、选中文字

## 禁止鼠标拖动图片、选中文字
```
<img src="logo.gif" ondragstart="return false;">
oncontextmenu="return false;" //禁止鼠标右键
ondragstart="return false;" //禁止鼠标拖动
onselectstart="return false;"//文字禁止鼠标选中
onselect="document.selection.empty();"//禁止复制文本
```
## CSS3禁止网页中文本被选中代码
### 浏览器支持
目前，只有Gecko和webkit支持该属性，包括基本上所有版本的Firefox/Chrome/Safari，IE10中也将支持该属性。当然，各个浏览器都必须加上私有前缀。Opera尚不支持。
### user-select的介绍
user-select是在css3 UI规范中新增的一个功能，用来控制内容的可选择性。
语法 : `user-select:value;`
可选参数
auto——默认值，用户可以选中元素中的内容
none——用户不能选择元素中的任何内容
text——用户可以选择元素中的文本
element——文本可选，但仅限元素的边界内(只有IE和FF支持)
all——在编辑器内，如果双击/上下文点击发生在子元素上，改值的最高级祖先元素将被选中。
-moz-none——firefox私有，元素和子元素的文本将不可选，但是，子元素可以通过text重设回可选。
### 代码
```
选择器 {
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}
```