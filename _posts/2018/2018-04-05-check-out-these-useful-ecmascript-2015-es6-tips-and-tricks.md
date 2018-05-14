---
layout: post
title: 你点的 ES6 小技巧，请查收
categories: [JS]
description: 你点的 ES6 小技巧，请查收
keywords: js, es6
---

摘要：总结 ES6 新特性：默认参数、reduce、解构赋值和 Set 在使用时的一些小技巧。

ES6 出来已经有好几年了，同时很多新特性可以被巧妙地运用在项目中。我想要列下其中一些，希望它们对你有用。

## 1. 强制要求参数

ES6 提供了默认参数值机制，允许你为参数设置默认值，防止在函数被调用时没有传入这些参数。

在下面的例子中，我们写了一个 `required()` 函数作为参数 a 和 b 的默认值。这意味着如果 a 或 b 其中有一个参数没有在调用时传值，会默认调用 `required()` 函数，然后抛出错误。

```js
const required = () => {throw new Error('Missing parameter')};
const add = (a = required(), b = required()) => a + b;

add(1, 2) //3
add(1) // Error: Missing parameter.
```

## 2. 强大的 reduce

数组的 reduce 方法用途很广。它一般被用来把数组中每一项规约到单个值。但是你可以利用它做更多的事。

### 2.1 使用 reduce 同时实现 map 和 filter

假设现在有一个数列，你希望更新它的每一项（map 的功能）然后筛选出一部分（filter 的功能）。如果是先使用 map 然后 filter 的话，你需要遍历这个数组两次。

在下面的代码中，我们将数列中的值翻倍，然后挑选出那些大于 50 的数。有注意到我们是如何非常高效地使用 reduce 来同时完成 map 和 filter 方法的吗？

```js
const numbers = [10, 20, 30, 40];
const doubledOver50 = numbers.reduce((finalList, num) => {
  num = num * 2;
  if (num > 50) {
    finalList.push(num);
  }
  return finalList;
}, []);

doubledOver50; // [60, 80]
```

### 2.2 使用 reduce 取代 map 和 filter

如果你认真阅读了上面的代码，你应该能理解 reduce 是可以取代 map 和 filter 的。

### 2.3 使用 reduce 匹配圆括号

reduce 的另外一个用途是能够匹配给定字符串中的圆括号。对于一个含有圆括号的字符串，我们需要知道 `(` 和 `)` 的数量是否一致，并且 `(` 是否出现在 `)` 之前。

下面的代码中我们使用 reduce 可以轻松地解决这个问题。我们只需要先声明一个 `counter` 变量，初值为 0。在遇到 `(` 时 counter 加一，遇到 `)` 时 counter 减一。如果左右括号数目匹配，那最终结果为 0。

```js
//Returns 0 if balanced.
const isParensBalanced = str => {
  return str.split('').reduce((counter, char) => {
    if(counter < 0) { //matched ")" before "("
      return counter;
    } else if(char === '(') {
      return ++counter;
    } else if(char === ')') {
      return --counter;
    }  else { //matched some other char
      return counter;
    }

  }, 0); //<-- starting value of the counter
}

isParensBalanced('(())') // 0 <-- balanced
isParensBalanced('(asdfds)') //0 <-- balanced
isParensBalanced('(()') // 1 <-- not balanced
isParensBalanced(')(') // -1 <-- not balanced
```

### 2.4 统计数组中相同项的个数

很多时候，你希望统计数组中重复出现项的个数然后用一个对象表示。那么你可以使用 reduce 方法处理这个数组。

下面的代码将统计每一种车的数目然后把总数用一个对象表示。

```js
var cars = ['BMW','Benz', 'Benz', 'Tesla', 'BMW', 'Toyota'];
var carsObj = cars.reduce((obj, name) => {
  obj[name] = obj[name] ? ++obj[name] : 1;
  return obj;
}, {});

carsObj; // => {BMW: 2, Benz: 2, Tesla: 1, Toyota: 1}
```
reduce 的其他用处实在是太多了，我建议你阅读 [MDN 的相关代码示例](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)。

## 3. 对象解构

### 3.1 删除不需要的属性

有时候你不希望保留某些对象属性，也许是因为它们包含敏感信息或仅仅是太大了（just too big）。你可能会枚举整个对象然后删除它们，但实际上只需要简单的将这些无用属性赋值给变量，然后把想要保留的有用部分作为剩余参数就可以了。

下面的代码里，我们希望删除 `_internal` 和 `tooBig` 参数。我们可以把它们赋值给 `internal` 和 `tooBig` 变量，然后在 `cleanObject` 中存储剩下的属性以备后用。

```js
let {_internal, tooBig, ...cleanObject} = {el1: '1', _internal:"secret", tooBig:{}, el2: '2', el3: '3'};

console.log(cleanObject); // {el1: '1', el2: '2', el3: '3'}
```

### 3.2 在函数参数中解构嵌套对象

在下面的代码中，`engine` 是对象 `car` 中嵌套的一个对象。如果我们对 `engine` 的 `vin` 属性感兴趣，使用解构赋值可以很轻松地得到它。

```js
var car = {
  model: 'bmw 2018',
  engine: {
    v6: true,
    turbo: true,
    vin: 12345
  }
}
const modelAndVIN = ({model, engine: {vin}}) => {
  console.log(`model: ${model} vin: ${vin}`);
}
modelAndVIN(car); // => model: bmw 2018  vin: 12345
```

### 3.3 合并对象

ES6 带来了扩展运算符（...）。它一般被用来解构数组，但你也可以用它处理对象。

接下来，我们使用扩展运算符来展开一个新的对象，第二个对象中的属性值会改写第一个对象的属性值。比如 `object2` 的 `b` 和 `c` 就会改写 `object1` 的同名属性。

```js
let object1 = {a:1, b:2,c:3}
let object2 = {b:30, c:40, d:50}
let merged = {…object1, …object2} //spread and re-add into merged

console.log(merged) // {a:1, b:30, c:40, d:50}
```

## 4. Sets

### 4.1 使用 Set 实现数组去重

在 ES6 中，因为 Set 只存储唯一值，所以你可以使用 Set 删除重复项。

```js
let arr = [1, 1, 2, 2, 3, 3];
let deduped = [...new Set(arr)] // [1, 2, 3]
```

### 4.2 对 Set 使用数组方法

使用扩展运算符就可以简单的将 Set 转换为数组。所以你可以对 Set 使用 Array 的所有原生方法。

比如我们想要对下面的 Set 进行 filter 操作，获取大于 3 的项。

```js
let mySet = new Set([1, 2, 3, 4, 5]);
var filtered = [...mySet].filter(x => x > 3) // [4, 5]
```

## 5. 数组解构

有时候你会将函数返回的多个值放在一个数组里。我们可以使用数组解构来获取其中每一个值。

### 5.1 数值交换

```js
let param1 = 1;
let param2 = 2;
//swap and assign param1 & param2 each others values
[param1, param2] = [param2, param1];

console.log(param1) // 2
console.log(param2) // 1
```

### 5.2 接收函数返回的多个结果

在下面的代码中，我们从 `/post` 中获取一个帖子，然后在 `/comments` 中获取相关评论。由于我们使用的是 `async/await`，函数把返回值放在一个数组中。而我们使用数组解构后就可以把返回值直接赋给相应的变量。

```js
async function getFullPost(){
  return await Promise.all([
    fetch('/post'),
    fetch('/comments')
  ]);
}
const [post, comments] = getFullPost();
```

## 参考资料
- [check-out-these-useful-ecmascript-2015-es6-tips-and-tricks](https://medium.freecodecamp.org/check-out-these-useful-ecmascript-2015-es6-tips-and-tricks-6db105590377)

    作者：[rajaraodv](https://medium.freecodecamp.org/@rajaraodv)

- [你点的 ES6 小技巧，请查收](https://segmentfault.com/a/1190000013972464)