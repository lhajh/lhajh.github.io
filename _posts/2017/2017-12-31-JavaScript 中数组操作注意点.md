---
layout: post
title: JavaScript 中数组操作注意点
categories: [JS]
description: JavaScript 中数组操作注意点
keywords: js
---

JavaScript 中数组操作注意点

## 不要用 for_in 遍历数组

这是 JavaScript 初学者常见的误区。for_in 用于遍历对象中包括原型链上的所有可枚举的（enumerable）的 key，本来不是为遍历数组而存在。

使用 for_in 遍历数组有三点问题：

1. 遍历顺序不固定

	JavaScript 引擎不保证对象的遍历顺序。当把数组作为普通对象遍历时同样不保证遍历出的索引顺序。
2. 会遍历出对象原型链上的值。

	如果你改变了数组的原型对象（比如 polyfill）而没有将其设为 enumerable: false，for_in 会把这些东西遍历出来。
3. 运行效率低下。

	尽管理论上 JavaScript 使用对象的形式储存数组，JavaScript 引擎还是会对数组这一非常常用的内置对象特别优化。
	
	[可以看到使用 for_in 遍历数组要比使用下标遍历数组慢 50 倍以上](ttps://jsperf.com/for-in-vs-for-of-vs-foreach)

PS：你可能是想找 [for_of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)

## 不要用 JSON.parse(JSON.stringify()) 深拷贝数组

有人使用 JSON 中深拷贝对象或数组。这虽然在多数情况是个简单方便的手段，但也可能引发未知 bug，因为：

1. 会使某些特定值转换为 null

	NaN, undefined, Infinity 对于 JSON 中不支持的这些值，会在序列化 JSON 时被转换为 null，反序列化回来后自然也就是 null

2. 会丢失值为 undefined 的键值对

	JSON 序列化时会忽略值为 undefined 的 key，反序列化回来后自然也就丢失了

3. 会将 Date 对象转换为字符串

	JSON 不支持对象类型，对于 JS 中 Date 对象的处理方式为转换为 ISO8601 格式的字符串。然而反序列化并不会把时间格式的字符串转化为 Date 对象

4. 运行效率低下。

	作为原生函数，JSON.stringify 和 JSON.parse 自身操作 JSON 字符串的速度是很快的。然而为了深拷贝数组把对象序列化成 JSON 再反序列化回来完全没有必要。

我花了一些时间写了一个简单的深拷贝数组或对象的函数，测试发现运行速度差不多是使用 JSON 中转的 6 倍左右，顺便还支持了 TypedArray、RegExp 的对象的复制

[深拷贝](https://jsperf.com/deep-clone-array-using-native-json-and-custom-deepclone)

## 不要用 arr.find 代替 arr.some

Array.prototype.find 是 ES2015 中新增的数组查找函数，与 Array.prototype.some 有相似之处，但不能替代后者。

Array.prototype.find 返回第一个符合条件的值，直接拿这个值做 if 判断是否存在，如果这个符合条件的值恰好是 0 怎么办？

arr.find 是找到数组中的值后对其进一步处理，一般用于对象数组的情况；arr.some 才是检查存在性；两者不可混用。

## 不要用 arr.map 代替 arr.forEach

也是一个 JavaScript 初学者常常犯的错误，他们往往并没有分清 Array.prototype.map 和 Array.prototype.forEach 的实际含义。

map 中文叫做 映射，它通过将某个序列依次执行某个函数导出另一个新的序列。这个函数通常是不含副作用的，更不会修改原始的数组（所谓纯函数）。

forEach 就没有那么多说法，它就是简单的把数组中所有项都用某个函数处理一遍。由于 forEach 没有返回值（返回 undefined），所以它的回调函数通常是包含副作用的，否则这个 forEach 写了毫无意义。

确实 map 比 forEach 更加强大，但是 map 会创建一个新的数组，占用内存。如果你不用 map 的返回值，那你就应当使用 forEach

## 补：forEach 与 break

ES6 以前，遍历数组主要就是两种方法：手写循环用下标迭代，使用 Array.prototype.forEach。前者万能，效率最高，可就是写起来比较繁琐——它不能直接获取到数组中的值。

笔者个人是喜欢后者的：可以直接获取到迭代的下标和值，而且函数式风格（注意 FP 注重的是不可变数据结构，forEach 天生为副作用存在，所以只有 FP 的形而没有神）写起来爽快无比。但是！不知各位同学注意过没有：forEach 一旦开始就停不下来了。。。

forEach 接受一个回调函数，你可以提前 return，相当于手写循环中的 continue。但是你不能 break——因为回调函数中没有循环让你去 break：
```
[1, 2, 3, 4, 5].forEach(x => {
  console.log(x);
  if (x === 3) {
    break;  // SyntaxError: Illegal break statement
  }
});
```
解决方案还是有的。其他函数式编程语言例如 scala 就遇到了类似问题，它提供了一个函数
break，作用是抛出一个异常。

![](/assets/images/posts/js/2120325863-5a3690a81290b_articlex.png)

我们可以仿照这样的做法，来实现 arr.forEach 的 break：
```
try {
  [1, 2, 3, 4, 5].forEach(x => {
    console.log(x);
    if (x === 3) {
      throw 'break';
    }
  });
} catch (e) {
  if (e !== 'break') throw e; // 不要勿吞异常。。。
}
```
恶心的一Ｂ对不对。还有其他方法，比如用 Array.prototype.some 代替 Array.prototype.forEach。

考虑 [Array.prototype.some](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/some#Description) 的特性，当 some 找到一个符合条件的值（回调函数返回 true）时会立即终止循环，利用这样的特性可以模拟 break：
```
[1, 2, 3, 4, 5].some(x => {
  console.log(x);
  if (x === 3) {
    return true; // break
  }
  // return undefined; 相当于 false
});
```
some 的返回值被忽略掉了，它已经脱离了判断数组中是否有元素符合给出的条件这一原始的含义。

在 ES6 前，笔者主要使用该法（其实因为 Babel 代码膨胀的缘故，现在也偶尔使用），ES6 不一样了，我们有了 [for...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)。for...of 是真正的循环，可以 break：
```
for (const x of [1, 2, 3, 4, 5]) {
  console.log(x);
  if (x === 3) {
    break;
  }
}
```
但是有个问题，for...of 似乎拿不到循环的下标。其实 JavaScript 语言制定者想到了这个问题，可以如下解决：
```
for (const [index, value] of [1, 2, 3, 4, 5].entries()) {
  console.log(`arr[${index}] = ${value}`);
}
```
[Array.prototype.entries](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/entries)

[for...of 和 forEach 的性能测试](https://jsperf.com/array-foreach-vs-for-of-entries/1) Chrome 中 for...of 要快一些哦

## 参考资料

- [给初学者：JavaScript 中数组操作注意点](https://segmentfault.com/a/1190000012463583)