---
layout: post
title: ES5 和 ES6 中, 都怎么判断变量是否为数组
categories:
  - js
description: ES5 和 ES6 中, 都怎么判断变量是否为数组
keywords: js
---

JS 的弱类型机制导致判断变量类型是初级前端开发者面试时的必考题

# 在 ES5 中判断变量是否为数组

在 ES5 中, 我们至少有如下 5 种方式去判断一个值是否数组:

```javascript
var a = []
// 1\. 基于 instanceof
a instanceof Array
// 2\. 基于 constructor
a.constructor === Array
// 3\. 基于 Object.prototype.isPrototypeOf
Array.prototype.isPrototypeOf(a)
// 4\. 基于 getPrototypeOf
Object.getPrototypeOf(a) === Array.prototype
// 5\. 基于 Object.prototype.toString
Object.prototype.toString.apply(a) === '[object Array]'
```

以上, 除了 `Object.prototype.toString` 外, 其它方法都不能正确判断变量的类型.

要知道, 代码的运行环境十分复杂, 一个变量可能使用浑身解数去迷惑它的创造者. 且看:

```javascript
var a = {
  __proto__: Array.prototype
}
// 分别在控制台试运行以下代码
// 1\. 基于 instanceof
a instanceof Array // => true
// 2\. 基于 constructor
a.constructor === Array // => true
// 3\. 基于 Object.prototype.isPrototypeOf
Array.prototype.isPrototypeOf(a) // => true
// 4\. 基于 getPrototypeOf
Object.getPrototypeOf(a) === Array.prototype // => true
```

以上, 4 种方法将全部返回 `true` , 为什么呢? 我们只是手动指定了某个对象的 `__proto__` 属性为 `Array.prototype` , 便导致了该对象继承了 `Array` 对象, 这种毫不负责任的继承方式, 使得基于继承的判断方案瞬间土崩瓦解.

不仅如此, 我们还知道, `Array` 是堆数据, 变量指向的只是它的引用地址, 因此每个页面的 `Array` 对象引用的地址都是不一样的. `iframe` 中声明的数组, 它的构造函数是 `iframe` 中的 `Array` 对象. 如果在 `iframe` 声明了一个数组 `x` , 将其赋值给父页面的变量 `y` , 那么在父页面使用 `y instanceof Array` , 结果一定是 `false` 的. 而最后一种返回的是字符串, 不会存在引用问题. 实际上, 多页面或系统之间的交互只有字符串能够畅行无阻.

鉴于上述的两点原因, 故笔者推荐使用最后一种方法去撩面试官(别提是我说的), 如果你还不信, 这里恰好有篇文章跟我持有相同的观点: [Determining with absolute accuracy whether or not a JavaScript object is an array](http://web.mit.edu/jwalden/www/isArray.html).

## 在 ES6 中判断变量是否为数组

鉴于数组的常用性, 在 ES6 中新增了 `Array.isArray` 方法, 使用此方法判断变量是否为数组, 则非常简单, 如下:

```javascript
Array.isArray([]) // => true
Array.isArray({
  0: 'a',
  length: 1
}) // => false
```

实际上, 通过 `Object.prototype.toString` 去判断一个值的类型, 也是各大主流库的标准. 因此 `Array.isArray` 的 polyfill 通常长这样:

```javascript
if (!Array.isArray) {
  Array.isArray = function (arg) {
    return Object.prototype.toString.call(arg) === '[object Array]'
  }
}
```

# 参考资料

- [ES5和ES6中, 都怎么判断变量是否为数组? - 掘金](https://juejin.im/post/5ced146ef265da1bc07e1a3c)
