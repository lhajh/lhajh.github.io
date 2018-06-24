---
layout: post
title: 5 分钟掌握 JavaScript 实用窍门
categories: [JS]
description: 5 分钟掌握 JavaScript 实用窍门
keywords: js, tricks
---

一开始 JavaScript 只是为网页增添一些实时动画效果，现在 JS 已经能做到前后端通吃了，而且还是年度流行语言。本文分享几则 JS 小窍门，可以让你事半功倍 ~

## 1. 删除数组尾部元素

一个简单方法就是改变数组的 length 值:
```js
const arr = [11, 22, 33, 44, 55, 66];
// truncanting
arr.length = 3;
console.log(arr); //=> [11, 22, 33]
// clearing
arr.length = 0;
console.log(arr); //=> []
console.log(arr[2]); //=> undefined
```

## 2. 使用对象解构（object destructuring）来模拟命名参数

如果需要将一系列可选项作为参数传入函数，你很可能会使用对象（Object）来定义配置（Config）。
```js
doSomething({foo: 'Hello', bar: 'Hey!', baz: 42});

function doSomething(config) {
  const foo = config.foo !== undefined ? config.foo : 'Hi';
  const bar = config.bar !== undefined ? config.bar : 'Yo!';
  const baz = config.baz !== undefined ? config.baz : 13;
  // ...
}
```
不过这是一个比较老的方法了，它模拟了 JavaScript 中的命名参数。

在 ES 2015 中，你可以直接使用对象解构：
```js
function doSomething({foo = 'Hi', bar = 'Yo!', baz = 13}) {
  // ...
}
```
让参数可选也很简单：
```js
function doSomething({foo = 'Hi', bar = 'Yo!', baz = 13} = {}) {
  // ...
}
```

## 3. 使用对象解构来处理数组

可以使用对象解构的语法来获取数组的元素：
```js
const csvFileLine = '1997,John Doe,US,john@doe.com,New York';
const {2: country, 4: state} = csvFileLine.split(',');

console.log(country, state); // 'US', 'New York'
```

## 4. 在 Switch 语句中使用范围值

可以这样写满足范围值的语句：
```js
function getWaterState(tempInCelsius) {
  let state;

  switch (true) {
    case (tempInCelsius <= 0):
      state = 'Solid';
      break;
    case (tempInCelsius> 0 && tempInCelsius < 100):
      state = 'Liquid';
      break;
    default:
      state = 'Gas';
  }
  return state;
}
```

## 5. await 多个 async 函数

在使用 async/await 的时候，可以使用 Promise.all 来 await 多个 async 函数
```js
await Promise.all([anAsyncCall(), thisIsAlsoAsync(), oneMore()])
```

## 6. 创建 pure objects

你可以创建一个 100% pure object，它不从 Object 中继承任何属性或则方法（比如 constructor, toString() 等）
```js
const pureObject = Object.create(null);
console.log(pureObject); //=> {}
console.log(pureObject.constructor); //=> undefined
console.log(pureObject.toString); //=> undefined
console.log(pureObject.hasOwnProperty); //=> undefined
```

## 7. 格式化 JSON 代码

[JSON.stringify()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 除了可以将一个对象字符化，还可以格式化输出 JSON 对象
```js
const obj = {
  foo: {bar: [11, 22, 33, 44], baz: { bing: true, boom: 'Hello' } }
};
// The third parameter is the number of spaces used to
// beautify the JSON output.
JSON.stringify(obj, null, 4);
// =>"{
// =>    "foo": {
// =>        "bar": [
// =>            11,
// =>            22,
// =>            33,
// =>            44
// =>        ],
// =>        "baz": {
// =>            "bing": true,
// =>            "boom": "Hello"
// =>        }
// =>    }
// =>}"
```

## 8. 从数组中移除重复元素

通过使用集合语法和 Spread 操作，可以很容易将重复的元素移除：
```js
const removeDuplicateItems = arr => [...new Set(arr)];
removeDuplicateItems([42, 'foo', 42, 'foo', true, true]);
//=> [42, "foo", true]
```

## 9. 平铺多维数组

使用 Spread 操作平铺嵌套多维数组：
```js
const arr = [11, [22, 33], [44, 55], 66];
const flatArr = [].concat(...arr); //=> [11, 22, 33, 44, 55, 66]
```
不过上面的方法仅适用于二维数组，但是通过递归，就可以平铺任意维度的嵌套数组了：
```js
function flattenArray(arr) {
  const flattened = [].concat(...arr);
  return flattened.some(item => Array.isArray(item)) ?
    flattenArray(flattened) : flattened;
}

const arr = [11, [22, 33], [44, [55, 66, [77, [88]], 99]]];
const flatArr = flattenArray(arr);
//=> [11, 22, 33, 44, 55, 66, 77, 88, 99]
```
希望这些小技巧能帮助你写好 JavaScript ~

## 参考资料

- [Learn these neat JavaScript tricks in less than 5 minutes](https://link.zhihu.com/?target=https%3A//medium.freecodecamp.org/9-neat-javascript-tricks-e2742f2735c3)
- [5 分钟掌握 JavaScript 实用窍门](https://zhuanlan.zhihu.com/p/37493249)