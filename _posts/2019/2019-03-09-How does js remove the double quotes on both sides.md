---
layout: post
title: js 怎么去掉前后两边的双引号
categories: [JS]
description: js 怎么去掉前后两边的双引号
keywords: js
---

## 方法一

```js
var testStr = '"asdf"'
testStr = testStr.replace('"', '').replace('"', '')
// 如果不确定有多少个双引号：
testStr = testStr.replace(/\"/g, '')
```

这个方法有一个弊端: 只能从左到右依次替换, 如果确认只有前后两边有双引号可以使用

## 方法二

```js
var testStr = "\"dsad'''''\"asdsadf\""
var reg = /^["|'](.*)["|']$/g
testStr.replace(reg, '$1')
// 输出为: dsad'''''"asdsadf
```

利用正则匹配到前后两边是引号的字符串, `$1` 为去掉两边引号的字符串

## 方法三

```js
var testStr = '"13"23"'
testStr.replace(/^\"|\"$/g, '')
// 输出为: 13"23
```

利用正则匹配前后两边的引号并替换

## 方法四

```js
var testStr = '"testldld"'
testStr = testStr.substring(1, testStr.length - 1)
// 输出结果: testldld
```

利用 substring 截取

## 方法五

```js
var testStr = '"testldld"'
testStr = testStr.substring(testStr.indexOf("\"") + 1, testStr.lastIndexOf("\""))
// 输出结果: testldld
```

同上

## 方法六

```js
var testStr = "{\"en\":\"equip556600\", \"zh_cn\":\"\", \"zh_tw\":\"\"}"
testStr = testStr.replace(/(^\"_)|(\"_\$)/g, '')
// 输出结果: {"en":"equip556600", "zh_cn":"", "zh_tw":""}
```

## 方法七

```js
var testStr = "false"
testStr = JSON.parse(testStr)
// 输出结果为布尔值的 false

var testStr = '"false"'
testStr = JSON.parse(testStr)
// 输出结果为字符串 false

var testStr = "'false'"
testStr = JSON.parse(testStr)
// 报错: Uncaught SyntaxError: Unexpected token ' in JSON at position 0
```

利用 `JSON.parse`
