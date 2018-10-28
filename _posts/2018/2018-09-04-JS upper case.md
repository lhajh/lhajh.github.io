---
layout: post
title: JavaScript 字符串单词首字母大写
categories: [JS, tools]
description: JavaScript 字符串单词首字母大写
keywords: js, upper
---

首字母大写和驼峰命名法这么常见的写法, 在 js 中竟然没有相应的方法, 没办法, 只能自己写一个了

## replace + 正则

```js
function firstUpperCase(str) {
  return str.replace(/\b(\w)(\w*)/g, function($0, $1, $2) {
    return $1.toUpperCase() + $2.toLowerCase()
  })
}
```

利用正则 `/\b(\w)(\w*)/g` 先匹配出每个单词边界, 再将单词第一个字母大写, 其余字母全部小写(这个看情况, 如果不需要修改, 直接返回 `$2` 即可)

```js
function firstUpperCase(str) {
  return str.toLowerCase().replace(/( |^)[a-z]/g, function(s) {
    return s.toUpperCase()
  })
}
```

```js
function firstUpperCase(str) {
  return str.toLowerCase().replace(/(^|\s)[a-z]/g, function(s) {
    return s.toUpperCase()
  })
}
```

```js
function firstUpperCase(str) {
  return str.toLowerCase().replace(/\b[a-z]/g, function(s) {
    return s.toUpperCase()
  })
}
```

正则表达式 `\b` 会把英文缩写，譬如 `i'm` 拆分成两个部分，导致输出为 `I'M`

```js
function firstUpperCase(str) {
  return str.replace(/^\S/, function(s) {
    return s.toUpperCase()
  })
}
```

```js
str[0].toUpperCase() + str.slice(1)
```

```js
str.charAt(0).toUpperCase() + str.slice(1)
```

只大写第一个单词的首字母

## 拆分 + replace

```js
function firstUpperCase(str) {
  //将字符串转化为小写，并拆分成单词
  str = str.toLowerCase().split(' ')
  //循环将每个单词的首字母大写
  for (var i = 0; i < str.length; i++) {
    //选取首个字符
    var char = str[i].charAt(0)
    //将单子首字符替换为大写
    str[i] = str[i].replace(char, function(s) {
      return s.toUpperCase()
    })
  }
  //拼合数组
  str = str.join(' ')
  return str
}
```

for + replace()

```js
const firstUpperCase = str => {
  const splits = str.toLowerCase().split(' ')
  return splits.map(x => x.replace(/[a-z]/, y => y.toUpperCase())).join(' ')
}
```

map() + replace()

```js
function firstUpperCase(str) {
  str = str.toLowerCase().split(' ')
  for (var i in str) {
    str[i] = str[i].replace(str[i].charAt(0), str[i].charAt(0).toUpperCase())
  }
  return str.join(' ')
}
```

for ··· in + replace()

```js
function titleCase(str) {
  return str
    .toLowerCase()
    .split(' ')
    .map(function(word) {
      return word.charAt(0).toUpperCase() + word.slice(1)
    })
    .join(' ')
}
```

map() + slice()

## ES6

```js
let firstUpperCase = ([first, ...rest]) => first.toUpperCase() + rest.join('')
```

## CSS

```css
text-transform: capitalize;
```

如果只是需要在页面显示, 可以直接使用 css 的样式
