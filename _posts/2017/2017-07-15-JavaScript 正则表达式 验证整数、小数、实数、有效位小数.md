---
layout: post
title: JavaScript正则表达式验证整数、小数、实数、有效位小数
categories: JS
description: JavaScript正则表达式验证整数、小数、实数、有效位小数
keywords: JS, 正则
---

JavaScript 正则表达式 验证整数、小数、实数、有效位小数

```
<!DOCTYPE html> 
<html> 
<head> 
<title> 验证数字最简单正则表达式大全 </title> 
</head> 
<body> 
<h3>输入完按回车后即可验证！</h3> 
正整数:    <input type="text" size="20" onkeydown="if(event.keyCode == 13) alert(/^\d+$/.test(this.value));" /> 
<br> 
负整数:    <input type="text" size="20" onkeydown="if(event.keyCode == 13) alert(/^-\d+$/.test(this.value));" /> 
<br> 
整　数:    <input type="text" size="20" onkeydown="if(event.keyCode == 13) alert(/^-?\d+$/.test(this.value));" /> 
<br> 
正小数:    <input type="text" size="20" onkeydown="if(event.keyCode == 13) alert(/^\d+\.\d+$/.test(this.value));" /> 
<br> 
负小数:    <input type="text" size="20" onkeydown="if(event.keyCode == 13) alert(/^-\d+\.\d+$/.test(this.value));" /> 
<br> 
小　数:    <input type="text" size="20" onkeydown="if(event.keyCode == 13) alert(/^-?\d+\.\d+$/.test(this.value));" /> 
<br> 
非负数:    <input type="text" size="20" onkeydown="if(event.keyCode == 13) alert(/^\d+(\.\d+)?$/.test(this.value));" /> 
<br> 
实　数:    <input type="text" size="20" onkeydown="if(event.keyCode == 13) alert(/^-?\d+\.?\d*$/.test(this.value));" /> 
<br> 
保留1位小数:<input type="text" size="20" onkeydown="if(event.keyCode == 13) alert(/^-?\d+\.?\d{0,1}$/.test(this.value));" /> 
<br> 
保留2位小数:<input type="text" size="20" onkeydown="if(event.keyCode == 13) alert(/^-?\d+\.?\d{0,2}$/.test(this.value));" /> 
<br> 
保留3位小数:<input type="text" size="20" onkeydown="if(event.keyCode == 13) alert(/^-?\d+\.?\d{0,3}$/.test(this.value));" /> 
<br> 
</body> 
</html>
\d：表示任意一个数字
\w：表示任意一个单词字符（只能是 数字，字母，下划线）
\s：表示任意一个空白字符
\D：表示任意一个非数字字符
\W：表示任意一个非单词字符
\S：表示任意一个非空白字符
"[]"用来描述单一字符，方括号内部可以定义这个字符的内容，也可以描述一个范围,例如:[1,2,3],表示该字符只能是1或2或3
"+"：表示内容可以连续出现至少1次以上
"*"：表示内容出现0-若干次
"?"：表示内容出现0-1次
{n}：表示内容必须出现n次
{n,m}：表示内容出现n-m次
{n,}：表示内容出现至少n次
```