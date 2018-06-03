---
layout: post
title: js 严格模式简谈
categories: [JS]
description: js 严格模式简谈
keywords: strict
---

## 前言

严格模式是 ES5 中引入的概念，使用严格模式，可以在函数内部选择进行较为严格的全局或局部错误条件检测。使用严格模式的好处是可以提早知道代码中存在的错误，及时捕获一些可能导致编程错误的 ECMAScript 行为。

## 一、使用严格模式

字符串 "use strict" 是一个使用严格模式的编译指示，支持严格模式的引擎将会启动严格模式，不支持的则当作遇到一个未赋值的字符串字面量。

既可以在全局中声明使用严格模式，也可以在函数中声明使用，两者情况下的作用域不同。

## 二、对变量的限制

首先，不允许意外创建全局变量。即
```js
message = 'hello world';
```
这行代码在非严格模式下将 message 定义为全局属性，而严格模式则抛出 ReferenceError 错误。
其次，不能对变量调用 delete 操作符。
```js
delete message;
```
非严格模式下，会返回 false 静默错误。而严格模式则抛出 ReferenceError 错误。

最后，严格模式还不允许变量使用 implements、interface、let、package 等保留字。

### 三、操作对象

1. 为只读属性赋值会抛出 TypeError；
2. 对不可配置的属性调用 delete 操作符会抛出 TypeError；
3. 对不可扩展的对象添加属性会抛出 TypeError。
4. 对象的属性名必须唯一：

```js
var person = {
  name: "abc",
  name: "def"
}
```
非严格模式中，最后声明的 name 属性值生效，而严格模式中这种代码会导致语法错误。

## 四、函数

1. 函数形参必须唯一，不能重名。
    ```js
    function add(num,num){
      // something
    }
    ```
    非严格模式中，重名形参只能访问到最后一个，而严格模式下会报语法错误。
2. 严格模式修改形参无法反映到 arguments 对象。
3. 禁用 arguments.callee 和 arguments.caller
4. 保留字无法用来命名函数
5. if 语句中无法命名函数：
```js
if(1){
  function add(a,b){
    // something
  }
}
```
非严格模式下会将 add 函数提升到 if 语句外，而严格模式报语法错误。

## 五、eval()

eval() 不再能够创建变量：
```js
eval(var x = 10);
alert(x);
```
非严格模式下，alert 正常运行；严格模式下报错。

## 六、this 的正确指向
```js
var color = 'red';
function display(){
  alert(this.color);
}
display.call(null);
```
以上代码在非严格模式中，值为 null 的 this 会转换为 window，因此能够正常弹出对话框 red，而严格模式下，this 的值为 null，报错。

## 七、其他变化

1. 抛弃 with 语句。
2. 0 开头的八进制字面量无效。
3. 若将八进制字面量传入 parseInt()，会被当作 0 开头的十进制数处理。

## 总结

严格模式如其名，提供了更加规范也更加合理的代码标准，理解严格模式也是非常重要的。