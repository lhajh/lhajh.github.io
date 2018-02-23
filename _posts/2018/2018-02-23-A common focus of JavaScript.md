---
layout: post
title: JavaScript 的常见注意点
categories: [JS]
description: JavaScript 的常见注意点
keywords: JavaScript
---

[上篇](https://lhajh.github.io/js/2017/12/31/JavaScript-array-operation-attention-point.html)说了一些 JS 中数组操作的常见误区，这次来总结一下初学者常见的其他易错点。

## 写立即执行函数时前置 void

立即执行函数（IIFE）在 JS 非常常用，作用就是构造一个函数级的变量作用域。常见的写法如下：
```
(function () {
  // code
})()
```
这样写可能会被 JS 理解成为一个函数调用
```
var a = 1
(function () { // Uncaught TypeError: 1 is not a function
})()
```
从今天改变习惯，这样写：
```
void function () {
  // code
}()
```
有些人喜欢以 `!` 打头，个人习惯问题。

在 [standardjs](https://standardjs.com/readme-zhcn.html) 规范日益流行的今天，忽略行尾分号成为了主流，更要改变这个习惯

注：standardjs 本身禁止行首括号

## 检查一个变量是否为对象之前，首先判断其值是否为 null

虽然不愿承认，JS 标准说：
```
typeof null === 'object' // true
```
毋庸置疑的，null 不具备作为对象类型的基本特征，是原始类型。这是一个广为人知的 JS 的 bug，，它从 JS 诞生开始就存在，从未、而且永远不会被修复

我们不必去探究它的黑历史，但是我们写代码时判断一个变量的类型时，首先需要判断它是否为 null
```
if (someVal !== null && typeof someVal === 'object') {
  // someVal 是一个对象
}
```

## 做数值计算时，注意 JS 数值类型的精度

在 JS 里，所有的 number 原始值都是一个双精度浮点数，对应 Java 的 double 类型，对应标准 IEEE754。小心它的精度问题。

## 做整数处理时，注意数值的大小

JS 最大可存储的安全整数（不存在精度问题）为 9007199254740991 (16位，Number.MAX_SAFE_INTEGER )，注意比 Java 的 long 类型最大整数 9223372036854775807 (19位) 小几个数量级，所以有时 JS 的 number 类型是不能精确存储 Java 的整数的（当然通常情况下不是问题）。

问题通常出在前后端数据传输上。数据库中的主键通常是一个自增长的长整型数，有可能会超出 JS 的安全整数范围，这时请考虑使用字符串传输。

## 做小数计算时，注意浮点数的精度问题

例如：0.1 + 0.2 => 0.30000000000000004，0.4 - 0.3 => 0.10000000000000003

将小数转化为字符串时，永远记得使用 toFixed 取小数点后若干位数字：
```
(0.1 + 0.2).toFixed(2) === '0.30'
```
比较小数相等时，切记不要直接使用 ===，而要使用相减取绝对值的方式（表示两数相差在一定范围内即认为他们相等）。
```
0.1 + 0.2 === 0.3 // false
Math.abs(0.1 + 0.2 - 0.3) <= 1e-10 // true
```

## NaN !== NaN

[NaN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN) 之所以 NB，因为它有一个独一无二的特性。对！独一无二！那就是：
```
NaN === NaN // false
var a = NaN; a === a // false
```
NaN 不等于它自己。你可以使用这个特性判断一个变量是否为 NaN，一个变量如果不等于它自己，这个变量一定是 NaN。

还有一个方式是使用 Number.isNaN。注意如果不已知这个变量的类型是数字时，不要使用 isNaN 做判断，因为 [isNaN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN#Description) 有个很诡异的特性：它会先将待判断的变量转换为数值类型。
```
isNaN('abc') // true
isNaN('123') // false
isNaN('') // false
isNaN([]) // false
isNaN({}) // true
```
永远不要写 someVal === NaN

## 正确使用 parseInt

首先 parseInt 接受两个参数，第一个参数为待 parse 的字符串（如果不是字符串则会首先转换为字符串）；第二个参数为使用的进制数。

如果不传第二个参数，则进制由第一个参数决定。什么意思呢？比如以 0x 开头的字符串，会被解析为 16 进制数。

我们知道以数字 0 开头的数字为 8 进制数（非严格模式），比如 011 === 9，0 本身也是 8 进制数。那么问题来了， parseInt('011') = ?

答案是看浏览器。目前绝大多数浏览器都会作为 10 进制数解析，结果为 11。但是还有一些老旧的浏览器以 8 进制数解析（例如 IE8 和一批老 Android 浏览器）

![](/assets/images/posts/js/2624446038-5a50a565c05e8_articlex.png)

所以如果你非要用 parseInt：

### parseInt 使用规则一：请传入第二个参数

回到 parseInt 本身的含义。顾名思义这个函数是在 parse，被 parse 的一定是个字符串。如果第一个参数不是字符串，那么会首先被转换为字符串。

问：parseInt(0.0000000008) =?

答：

1. String(0.0000000008) => '8e-10'
2. parseInt('8e-10') => 8

自己打开调试器去试

### parseInt 使用规则二：永远不要使用 parseInt 给小数取整

建议对于数值转换一概使用强制转换函数 Number，如果你 JS 用 6 了可以使用 +（正号）。

如果需要对某个数字取整，建议使用 [Math.trunc](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/trunc)。如果你能确定数值在 32 位以内，可以使用 x | 0 或 ~~x 等方式

![](/assets/images/posts/js/1047484547-5a50bd32b05ba_articlex.png)

parseInt 的用处在于转换一些 CSS 里带单位的值：parseInt('10px', 10) => 10。但这里建议使用 parseFloat，可以解析小数又没有进制问题。

## 除了用于比较 null 或 undefined，永远不要使用非严格相等 ==

绝不要简单的把非严格相等 == 理解为两者表示的数字一样，它有一套非常复杂的转换规则：它会先将 %% 转换为 @@，然后把 !! 转换为 **，如果 %% 是 ?? 类型，还会 xx 一把……

看不懂对吧，我相信你就算看懂了也记不住的。不然请问：
```
'true' == true // => false
'true' == false // => false
[] == {} // => false
[] == [] // => false
```
关于非严格相等，你只需要记住这个规则：
```
null == null // => true
undefined == undefined  // => true
null == undefined // => true
x == null // => false (x 非 null 或 undefined）
x == undefined // => false (x 非 null 或 undefined）
```
简言之：
```
x == null // 或 x == undefined
```
是最简单的判断 x 为 null 或 undefined 的方式，相对应的
```
x != null // 或 x != undefined
```
是最简单的判断 x 非 null 和 undefined 的方式。这就是 == 存在的唯一意义。

## 日期处理

### new Date(year, month, day) 注意其参数的数值范围

由于可能的历史传承原因，JS 内置对象 Date 的构造函数比较特殊。

1. 如果 year 是 0 ~ 99 之间，year 默认加 1900。比如 1 代表公元 1901 年，99 代表公元 1999 年，100 代表公元 100 年。（你问 -1 是几？公元前 1 年。。。）
2. month 从 0 开始算。0 代表一月，1 代表二月，以此类推。12 代表下一年的一月（自动进位）

第一点不知道也没什么，毕竟一般不会操作公元 99 年之前的时间。但第二点就很容易出错，切记它是以 0 开始的数字。

这样得到的日期对象是本地时间（采用客户端时区）

### new Date(dateString) 注意浏览器时区问题以及浏览器兼容性

时常有后端接口返回一个日期字符串的情况：
```
new Date('2018-01-01') // => "2018/1/1 08:00:00" 新版浏览器，IE 11
new Date('2018-01-01') // => "2018/1/1 00:00:00" 某些旧版安卓
new Date('2018-01-01') // => "Invalid Date" IE 8（这个忽略。。。）
```
可以看到，浏览器基本都是把日期字符串当做 UTC 时间处理的。而
```
new Date('2018/01/01') // => "2018/1/1 00:00:00" 包括 IE 8 在内所有浏览器
```
所以对于日期字符串，请注意字符串中是使用横杠还是斜杠。对于横杠可以考虑将 - 替换成 /，或者补全完整的带时区的 ISO8601 字符串。考虑到负数时区的问题，不推荐将小时数清零的做法。

注：[关于横杠和斜杠的区别](https://lhajh.github.io/js/2017/08/07/date-use-the-slash-point-and-the-minus-sign-separation.html)

PS：将日期对象取当天 0 点为 date.setHours(0, 0, 0, 0)
PS2：取当前时间的 Unix 时间戳可以 Date.now()

![](/assets/images/posts/js/1557773151-5a50ca7930892_articlex.png)

## 慎用 || 填充默认值

这反而是 JS 老鸟更容易犯的错误。给用户传入的对象填充默认值是很常见的行为，他们总是随手就写：
```
config.prop1 = config.prop1 || 233;
config.prop2 = config.prop2 || 'balabala';
```
expr1 || expr2 的意思是：如果 expr1 能转换成 true 则返回 expr1，否则返回 expr2
```
expr1 || expr2 <=> Boolean(expr1) ? expr1 : expr2
```
哪些值不能转换为 true 呢？

- null
- undefined
- NaN
- 0 !!!
- 空字符串（''） !!!

如果用户指定了传入参数的值为 0 或者是空字符串的配置项，它的值就会被强制替换为默认值，然而实际上只有 undefined 应该被认为是用户没有指定其值（语义上可以这样理解：null 表示 用户让你给他把这个位置空着；而 undefined 表示 用户没发表意见）

所以就应该是这样：
```
config.prop1 = config.prop1 !== undefined ? config.prop1 : 233;
config.prop2 = config.prop2 !== undefined ? config.prop2 : 'balabala';
```
很长。。。你可以搞个全局的函数简化这一操作，或者考虑使用 lodash 的 [defaults](https://lodash.com/docs/4.17.5#defaults) 方法

## 参考资料

- [给初学者：JavaScript 的常见注意点](https://segmentfault.com/a/1190000012730162)
