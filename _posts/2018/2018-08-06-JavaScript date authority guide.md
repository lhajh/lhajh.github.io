---
layout: post
title: JavaScript 日期权威指南
categories: JS
description: JavaScript 日期权威指南
keywords: js, array
---

JavaScript 通过强大的对象为我们提供日期处理功能：Date。

## Date 对象

Date 对象实例表示单个时间点。

尽管被命名为 Date，它也处理时间。

## 初始化 Date 对象

我们使用初始化 Date 对象

```js
new Date()
```

这将创建一个指向当前时刻的 Date 对象。

在内部，日期以 1970 年 1 月 1 日（UTC）以来的毫秒数表示。这个日期很重要，因为就计算机而言，这就是一切开始的地方。

您可能熟悉 UNIX 时间戳：它表示自该著名日期以来经过的 seconds 数。

重要：UNIX 时间戳的原因以秒为单位。JavaScript 以毫秒为单位记录原因。

如果我们有 UNIX 时间戳，我们可以使用实例化 JavaScript Date 对象

```js
const timestamp = 1530826365
new Date(timestamp * 1000)
```

如果我们传递 0，我们将得到一个 Date 对象，表示 1970 年 1 月 1 日（UTC）的时间：

```js
new Date(0)
```

如果我们传递一个字符串而不是一个数字，那么 Date 对象使用 parse 方法来确定您传递的日期。例子：

```js
new Date('2018-07-22')
new Date('2018-07') //July 1st 2018, 00:00:00
new Date('2018') //Jan 1st 2018, 00:00:00
new Date('07/22/2018')
new Date('2018/07/22')
new Date('2018/7/22')
new Date('July 22, 2018')
new Date('July 22, 2018 07:22:13')
new Date('2018-07-22 07:22:13')
new Date('2018-07-22T07:22:13')
new Date('25 March 2018')
new Date('25 Mar 2018')
new Date('25 March, 2018')
new Date('March 25, 2018')
new Date('March 25 2018')
new Date('March 2018') //Mar 1st 2018, 00:00:00
new Date('2018 March') //Mar 1st 2018, 00:00:00
new Date('2018 MARCH') //Mar 1st 2018, 00:00:00
new Date('2018 march') //Mar 1st 2018, 00:00:00
```

这里有很多灵活性。您可以在几个月或几天内添加或省略前导零。

> 小心月 / 日的位置，或者你可能最终将月份误解为当天。

你也可以使用 Date.parse：

```js
Date.parse('2018-07-22')
Date.parse('2018-07') //July 1st 2018, 00:00:00
Date.parse('2018') //Jan 1st 2018, 00:00:00
Date.parse('07/22/2018')
Date.parse('2018/07/22')
Date.parse('2018/7/22')
Date.parse('July 22, 2018')
Date.parse('July 22, 2018 07:22:13')
Date.parse('2018-07-22 07:22:13')
Date.parse('2018-07-22T07:22:13')
```

Date.parse 将返回一个时间戳（以毫秒为单位）而不是 Date 对象。

您还可以传递一组代表日期各部分的有序值：年，月（从 0 开始），日，小时，分钟，秒和毫秒：

```js
new Date(2018, 6, 22, 7, 22, 13, 0)
new Date(2018, 6, 22)
```

最小值应该是 3 个参数，但是大多数 JavaScript 引擎的解释都比这些少：

```js
new Date(2018, 6) //Sun Jul 01 2018 00:00:00 GMT+0200 (Central European Summer Time)
new Date(2018) //Thu Jan 01 1970 01:00:02 GMT+0100 (Central European Standard Time)
```

在任何这些情况下，生成的日期都相对于计算机的时区。这意味着两台不同的计算机可能会为同一日期对象输出不同的值。

JavaScript 没有任何关于时区的信息，会将日期视为 UTC，并自动执行到当前计算机时区的转换。

因此，总结一下，您可以通过 4 种方式创建新的 Date 对象

  * 不传参数，创建一个表示 “现在” 的 Date 对象
  * 传递 number，表示从格林威治标准时间 1970 年 1 月 1 日 00:00 开始的毫秒数
  * 传递一个字符串，代表一个日期
  * 传递一组参数，它们代表日期的不同部分

## 时区

初始化日期时，您可以传递时区，因此日期不会被假定为 UTC，然后转换为您当地的时区。

您可以通过以 + HOURS 格式添加时区来指定时区，或者通过添加括在括号中的时区名称来指定时区：

```js
new Date('July 22, 2018 07:22:13 +0700')
new Date('July 22, 2018 07:22:13 (CET)')
```

如果在括号中指定了错误的时区名称，则 JavaScript 将默认为 UTC 而不会报错。

如果您指定了错误的数字格式，JavaScript 将报 “无效日期” 的错误。

## 日期转换和格式设置

给定 Date 对象，有很多方法将从该日期生成一个字符串：

```js
const date = new Date('July 22, 2018 07:22:13')

date.toString() // "Sun Jul 22 2018 07:22:13 GMT+0200 (Central European Summer Time)"
date.toTimeString() //"07:22:13 GMT+0200 (Central European Summer Time)"
date.toUTCString() //"Sun, 22 Jul 2018 05:22:13 GMT"
date.toDateString() //"Sun Jul 22 2018"
date.toISOString() //"2018-07-22T05:22:13.000Z" (ISO 8601 format)
date.toLocaleString() //"22/07/2018, 07:22:13"
date.toLocaleTimeString()    //"07:22:13"
date.getTime() //1532236933000
date.getTime() //1532236933000
```

## Date 对象的 getter 方法

Date 对象提供了几种检查其值的方法。这些都取决于计算机的当前时区：

```js
const date = new Date('July 22, 2018 07:22:13')

date.getDate() //22
date.getDay() //0 (0 means sunday, 1 means monday..)
date.getFullYear() //2018
date.getMonth() //6 (starts from 0)
date.getHours() //7
date.getMinutes() //22
date.getSeconds() //13
date.getMilliseconds() //0 (not specified)
date.getTime() //1532236933000
date.getTimezoneOffset() //-120 (will vary depending on where you are and when you check - this is CET during the summer). Returns the timezone difference expressed in minutes
```

这些方法有等效的 UTC 版本，它们返回 UTC 值而不是适合您当前时区的值：

```js
date.getUTCDate() //22
date.getUTCDay() //0 (0 means sunday, 1 means monday..)
date.getUTCFullYear() //2018
date.getUTCMonth() //6 (starts from 0)
date.getUTCHours() //5 (not 7 like above)
date.getUTCMinutes() //22
date.getUTCSeconds() //13
date.getUTCMilliseconds() //0 (not specified)
```

## 编辑日期

Date 对象提供了几种编辑日期值的方法：

```js
const date = new Date('July 22, 2018 07:22:13')

date.setDate(newValue)
date.setDay(newValue)
date.setFullYear(newValue) //note: avoid setYear(), it's deprecated
date.setMonth(newValue)
date.setHours(newValue)
date.setMinutes(newValue)
date.setSeconds(newValue)
date.setMilliseconds(newValue)
date.setTime(newValue)
date.setTimezoneOffset(newValue)
```

setDay 和 setMonth 从 0 开始编号，因此例如 March 是 2 月。

你可以在 setHours（）中添加多个参数来设置分钟，秒和毫秒：setHours（0,0,0,0） - 这同样适用于 setMinutes 和 setSeconds。

至于 get_，set_方法也有 UTC 等价物：

```js
const date = new Date('July 22, 2018 07:22:13')

date.setUTCDate(newalue)
date.setUTCDay(newValue)
date.setUTCFullYear(newValue)
date.setUTCMonth(newValue)
date.setUTCHours(newValue)
date.setUTCMinutes(newValue)
date.setUTCSeconds(newValue)
date.setUTCMilliseconds(newValue)
```

## 获取当前时间戳

如果要以毫秒为单位获取当前时间戳，可以使用速记

```js
Date.now()
```

代替

```js
new Date().getTime()
```

## JavaScript 关于日期的容错处理

请注意。如果您使用天数计算超过一个月，则不会出现错误，日期将转到下个月：

```js
new Date(2018, 6, 40) //Thu Aug 09 2018 00:00:00 GMT+0200 (Central European Summer Time)
```

数月，小时，分钟，秒和毫秒都是如此。

## 根据区域设置格式化日期

现代浏览器中的支持良好国际化 API（值得注意的例外：UC 浏览器）允许您翻译日期。

它是由 Intl Object 暴露出来的，这也有助于本地化数字，字符串。

我来看看 Intl.DateTimeFormat（）。

以下是如何使用它。

根据计算机默认区域设置格式化日期：

```js
// "12/22/2017"
const date = new Date('July 22, 2018 07:22:13')
new Intl.DateTimeFormat().format(date) //"22/07/2018" in my locale
```

根据不同的区域设置格式化日期：

```js
new Intl.DateTimeFormat('en-US').format(date) //"7/22/2018"

Intl.DateTimeFormat方法采用可选参数，允许您自定义输出显示小时，分钟和秒：

const options = {
  year: 'numeric',
  month: 'numeric',
  day: 'numeric',
  hour: 'numeric',
  minute: 'numeric',
  second: 'numeric'
}

new Intl.DateTimeFormat('en-US', options).format(date) //"7/22/2018, 7:22:13 AM"
new Intl.DateTimeFormat('it-IT', options2).format(date) //"22/7/2018, 07:22:13"
```

[这里是您可以使用的所有属性的参考](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/DateTimeFormat)。

## 比较两个日期

您可以使用 Date.getTime（）计算两个日期之间的差异：

```js
const date1 = new Date('July 10, 2018 07:22:13')
const date2 = new Date('July 22, 2018 07:22:13')
const diff = date2.getTime() - date1.getTime() //difference in milliseconds
```

以同样的方式，您可以检查两个日期是否相等：

```js
const date1 = new Date('July 10, 2018 07:22:13')
const date2 = new Date('July 10, 2018 07:22:13')
if (date2.getTime() === date1.getTime()) {
  //dates are equal
}
```

请记住，getTime（）返回的毫秒数，因此您需要在比较中考虑时间因素。2018 年 7 月 10 日 07:22:13 不等于 2018 年 7 月 10 日。在这种情况下，您可以使用 setHours（0,0,0,0）重置时间。

## 参考资料

- [THE DEFINITIVE GUIDE TO JAVASCRIPT DATES](https://flaviocopes.com/javascript-dates/)
