---
layout: post
title: (a ==1 && a== 2 && a==3) 有可能是 true 吗？
categories: [JS]
description: (a ==1 && a== 2 && a==3) 有可能是 true 吗？
keywords: ==
---

一个有趣的问题：
在 JavaScript 中， (a ==1 && a== 2 && a==3) 是否有可能为 true ？

这是一道我被某科技公司问到的面试题。发生在两周之前，我仍然在努力寻找答案。

我知道我们从来不会在日常工作中写出这样的代码，但我对问题的答案仍然十分很好奇。

## 解法一：

### 修改 toString()

利用松散相等运算符 == 的工作原理，你可以简单地创建一个带有自定义 toString (或者 valueOf ) 函数的对象，在每一次使用它时候改变它所的返回值，使其满足所有三个条件。
```js
const a = {
  i: 1,
  toString: function () {
    return a.i++;
  }
}
if(a == 1 && a == 2 && a == 3) {
  console.log('Hello World!');
}
// Hello World!
```
之所以会得到如此结果，是由于表达式中使用了松散相等的运算符 ==。使用松散相等时，如果其中一个操作数与另一个类型不同，则 JS 引擎将尝试将一个操作转换为另一个类型。在左边对象、右边的数字的情况下，它会尝试将对象转换为一个数，首先通过调用 valueOf (如果是可调用的)。否则，它会调用 toString 方法。我使用 toString 仅仅是因为它是我的第一反应，valueOf 会更合理。如果我不从 toString 返回一个字符串（而是返回数字），JS 引擎会尝试将字符串转换为一个数字，虽然有一个稍长的路径，但它仍然会给我们同样的结果。

### 利用 toString 调用 .join 

== 调用 toString 调用 .join 数组：
```js
a = [1,2,3];
a.join = a.shift;
console.log(a == 1 && a == 2 && a == 3);
```

### Symbol.toPrimitive

使用 Symbol.toPrimitive 它是一个 ES6 相当于 toString/valueOf：
```js
let a = {[Symbol.toPrimitive]: ((i) => () => ++i) (0)};
console.log(a == 1 && a == 2 && a == 3);
```

### 正则 + valueOf

```js
var a = {
  r: /\d/g, 
  valueOf: function(){
    return this.r.exec(123)[0]
  }
}
if (a == 1 && a == 2 && a == 3) {
    console.log("!")
}
```
它的工作原理: valueOf 是当 Object 与 primitive（比如 Number）比较时调用的自定义方法; 一个正则表达式对象会记住它匹配的最后一个索引，exec再次调用将从该索引开始搜索。

主要的诀窍是 a.valueOf 每次都返回新的值，因为它调用 exec 带有 g 标志 lastIndex 的正则表达式，每当找到匹配时就会更新该正则表达式。所以第一次 this.r.lastIndex == 0，它匹配到 1 和更新 lastIndex：this.r.lastIndex == 1，所以下一次正则表达式将匹配 2 等等。

## 解法二：

我不可否认——其他答案无疑是正确的，但你真的不能过错下面的代码：
```js
var aﾠ = 1;
var a = 2;
var ﾠa = 3;
if(aﾠ==1 && a== 2 &&ﾠa==3) {
    console.log("Why hello there!")
}
```
请注意 if 语句中的奇怪间距。它是半宽度韩文 =,=。这是一个 Unicode 空格字符，但是 ECMAScript 不将其解释为一个空格 —— 这意味着它是一个有效的标识符。因此有三个完全不同的变量，一个是 a 后加半宽度韩文，一个是 a， 一个是 a 前加半宽度韩文。。。

用下划线 _ 替代半宽度韩文，增加可读性，相同的代码看起来像这样：
```js
var a_ = 1;
var a = 2;
var _a = 3;
if(a_==1 && a== 2 &&_a==3) {
    console.log("Why hello there!")
}
```
类似的还有：
```js
var a = 1;
var ａ = 2;
var а = 3;
if(a == 1 && ａ == 2 && а == 3) {
    console.log("Why hello there!")
}
```
你可能会注意到与第二个的差异，但第一个和第三个与肉眼是一样的。所有3个是不同的字符：

`a`- 拉丁文小写字母A 
`ａ`- 全宽拉丁文小写字母A 
`а`- 西里尔文小写字母A

通用术语是 `homoglyphs`：不同的 unicode 字符看起来是一样的。通常很难得到完全没有区别的三个，但在某些情况下，你可以有幸得到。A，Α，А 和 Ꭺ 会更好地完成上面的工作（拉丁语-A，希腊阿尔法，西里尔-A ，以及切诺基-A;不幸的是，希腊和切诺基小写字母是和拉丁文太不一样了a：α，ꭺ）。

这里有一整套的同形异义攻击（Homoglyph Attacks），通常是伪造的域名（例如 `wikipediа.org`（Cyrillic）vs `wikipedia.org`（Latin）），但也可以用代码表示。

还有这样的：
```js
var a = 1;
var ﾠ1 = a;
var ﾠ2 = a;
var ﾠ3 = a;
console.log( a ==ﾠ1 && a ==ﾠ2 && a ==ﾠ3 );
```
这是一个倒置的版本,  其中隐藏字符（U + 115F，U + 1160或U + 3164）用于创建看起来像变量1，2和3。

## 解法三：

这是完全可能的！
```js
var val = 0;
Object.defineProperty(window, 'a', {
  get: function() {
    return ++val;
  }
});
if (a == 1 && a == 2 && a == 3) {
  console.log('yay');
}
```
或者这样：
```js
var i = 0;
with({
  get a() {
    return ++i;
  }
}) {
  if (a == 1 && a == 2 && a == 3)
    console.log("wohoo");
}
```
使用一个 get，让 a 的返回值为三个不同的值。然而这并不意味着我们应该在真正的代码中使用。。。

## 利用 JavaScript 的 Number

在 JavaScript 中，没有[整数](https://stackoverflow.com/questions/33773296/is-there-or-isnt-there-an-integer-type-in-javascript/33774009#33774009)，但只有 Number，这是作为双精度浮点数实现的。

这意味着如果一个数字 a 足够大，它可以被认为等于 3 个连续的整数：
```js
a = 100000000000000000
if (a == a+1 && a == a+2 && a == a+3){
  console.log("Precision loss!");
}
```
诚然，这不是面试官所要求的（它不适用 a=0），但它不涉及隐藏的函数或操作符重载的任何技巧。

## 总结

1. 利用松散相等运算符 == 的原理，自定义 toString 和 valueOf 返回对应值
2. 利用半宽度韩文等特殊字符，玩“障眼法”，本质上其实并没有做到题设
3. 劫持 JS 对象的 getter，不过这种方式对于严格相等 === 同样有效
4. 利用 JavaScript 的 Number，不过有点不符合题设，但也可参考

## 参考资料

- [can-a-1-a-2-a-3-ever-evaluate-to-true](https://stackoverflow.com/questions/48270127/can-a-1-a-2-a-3-ever-evaluate-to-true)
- [【译】 (a ==1 && a== 2 && a==3) 有可能是 true 吗？](http://elevenbeans.github.io/2018/01/23/nothing-is-impossible-for-javascript/)
