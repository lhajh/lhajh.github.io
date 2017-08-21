---
layout: post
title: JavaScript toFixed() 方法
categories: JS
description: JavaScript toFixed() 方法
keywords: JavaScript, toFixed()
---

JavaScript toFixed() 方法

## 定义和用法
toFixed() 方法可把 Number 四舍五入为指定小数位数的数字。
## 语法
NumberObject.toFixed(num)
## 参数	描述
|参数|描述|
|:---:|:---|
|num 必需|规定小数的位数，是 0 ~ 20 之间的值，包括 0 和 20，有些实现可以支持更大的数值范围。如果省略了该参数，将用 0 代替。
## 返回值
返回 NumberObject 的字符串表示，不采用指数计数法，小数点后有固定的 num 位数字。如果必要，该数字会被舍入，也可以用 0 补足，以便它达到指定的长度。如果 num 大于 le+21，则该方法只调用 NumberObject.toString()，返回采用指数计数法表示的字符串。
## 问题
```
var num = 12;
alert(num.toFixed(4));
可以正常弹出
```
```
alert(12.toFixed(4));
不可以弹出
报错：Uncaught SyntaxError: Invalid or unexpected token
原因：认为12后面的点是小数点
```
```
修改
alert((12).toFixed(4));
可以正常弹出
原因：将12括起来，让其认为是一个完整的数值
```