---
layout: post
title: JavaScript 数组去重
categories: JS
description: JavaScript 数组去重
keywords: js, array
---

数组去重，一般都是在面试的时候才会碰到，一般是要求手写数组去重方法的代码。如果是被提问到，数组去重的方法有哪些？你能答出其中的 10 种，面试官很有可能对你刮目相看。

在真实的项目中碰到的数组去重，一般都是后台去处理，很少让前端处理数组去重。虽然日常项目用到的概率比较低，但还是需要了解一下，以防面试的时候可能回被问到。

## 数组去重的方法

## 一、利用 ES6 Set 去重（ES6 中最常用）

```js
function unique (arr) {
  return Array.from(new Set(arr))
}
var arr = [0, 0, 1, 1, 'true', 'true', true, true, 'false', 'false', false, false, 'undefined', 'undefined', undefined, undefined, 'null', 'null', null, null, 'NaN', 'NaN', NaN, NaN, {}, {}, [1, 2, 3], [1, 2, 3], function fun () {}, function fun () {}];

console.log(unique(arr))
// (18) [0, 1, "true", true, "false", false, "undefined", undefined, "null", null, "NaN", NaN, {…}, {…}, Array(3), Array(3), ƒ, ƒ]
```

不考虑兼容性，这种去重的方法代码最少。但这种方法无法去掉 '{}' 等对象，后面的高阶方法会添加去掉重复 '{}' 等对象的方法。

## 二、利用 for 嵌套 for，然后 splice 去重（ES5 中最常用）

```js
// ===
function unique (arr) {
  for (var i = 0; i < arr.length; i++) {
    for (var j = i + 1; j < arr.length; j++) {
      if (arr[i] === arr[j]) { // 第一个等同于第二个，splice 方法删除第二个
        arr.splice(j, 1);
        j--;
      }
    }
  }
  return arr;
}
var arr = [0, 0, 1, 1, 'true', 'true', true, true, 'false', 'false', false, false, 'undefined', 'undefined', undefined, undefined, 'null', 'null', null, null, 'NaN', 'NaN', NaN, NaN, {}, {}, [1, 2, 3], [1, 2, 3], function fun () {}, function fun () {}];

console.log(unique(arr))
// (19) [0, 1, "true", true, "false", false, "undefined", undefined, "null", null, "NaN", NaN, NaN, {…}, {…}, Array(3), Array(3), ƒ, ƒ]
// NaN 和 对象没有去重

// ==
function unique (arr) {
  for (var i = 0; i < arr.length; i++) {
    for (var j = i + 1; j < arr.length; j++) {
      if (arr[i] == arr[j]) { // 第一个等同于第二个，splice 方法删除第二个
        arr.splice(j, 1);
        j--;
      }
    }
  }
  return arr;
}
var arr = [0, 0, 1, 1, 'true', 'true', true, true, 'false', 'false', false, false, 'undefined', 'undefined', undefined, undefined, 'null', 'null', null, null, 'NaN', 'NaN', NaN, NaN, {}, {}, [1, 2, 3], [1, 2, 3], function fun () {}, function fun () {}];

console.log(unique(arr))
// (16) [0, 1, "true", "false", "undefined", undefined, "null", "NaN", NaN, NaN, {…}, {…}, Array(3), Array(3), ƒ, ƒ]
// 0 去掉 0 和 false, 1 去掉 1 和 true, undefined 去掉 undefined 和 null, NaN 和 对象没有去重
// 不建议使用 ==
```

双层循环，外层循环元素，内层循环时比较值。值相同时，则删去这个值。

## 三、利用 indexOf 去重

```js
function unique(arr) {
  if (!Array.isArray(arr)) {
    console.log('type error!')
    return
  }
  var array = [];
  for (var i = 0; i < arr.length; i++) {
    if (array.indexOf(arr[i]) === -1) {
      array.push(arr[i])
    }
  }
  return array;
}
var arr = [0, 0, 1, 1, 'true', 'true', true, true, 'false', 'false', false, false, 'undefined', 'undefined', undefined, undefined, 'null', 'null', null, null, 'NaN', 'NaN', NaN, NaN, {}, {}, [1, 2, 3], [1, 2, 3], function fun () {}, function fun () {}];

console.log(unique(arr))
// (19) [0, 1, "true", true, "false", false, "undefined", undefined, "null", null, "NaN", NaN, NaN, {…}, {…}, Array(3), Array(3), ƒ, ƒ]
//NaN 和 对象没有去重
```

新建一个空的结果数组，for 循环原数组，判断结果数组是否存在当前元素，如果有相同的值则跳过，不相同则 push 进数组。

## 四、利用 sort()

```js
function unique(arr) {
  if (!Array.isArray(arr)) {
    console.log('type error!')
    return;
  }
  arr = arr.sort()
  var arrry = [arr[0]];
  for (var i = 1; i < arr.length; i++) {
    if (arr[i] !== arr[i - 1]) {
      arrry.push(arr[i]);
    }
  }
  return arrry;
}
var arr = [0, 0, 1, 1, 'true', 'true', true, true, 'false', 'false', false, false, 'undefined', 'undefined', undefined, undefined, 'null', 'null', null, null, 'NaN', 'NaN', NaN, NaN, {}, {}, [1, 2, 3], [1, 2, 3], function fun () {}, function fun () {}];

console.log(unique(arr))
// (21) [0, 1, Array(3), Array(3), "NaN", NaN, NaN, {…}, {…}, "false", false, "false", ƒ, ƒ, null, "null", true, "true", true, "undefined", undefined]
// arr = arr.sort() ==> arr: (30) [0, 0, 1, 1, Array(3), Array(3), "NaN", "NaN", NaN, NaN, {…}, {…}, "false", false, false, "false", ƒ, ƒ, null, null, "null", "null", true, "true", "true", true, "undefined", "undefined", undefined, undefined]
// 而且改变 arr 值的顺序会得到不同的结果
// var arr = [0, 1, 'true', true, 'false', false,'undefined', undefined, 'null', null, 'NaN', NaN, {}, [1, 2, 3], function fun () {}, 0, 1, 'true', true, 'false', false, 'undefined', undefined, 'null', null, 'NaN', NaN, {}, [1, 2, 3], function fun () {}];
// arr = arr.sort() ==> arr: (30) [0, 0, 1, 1, Array(3), Array(3), NaN, "NaN", "NaN", NaN, {…}, {…}, false, "false", false, "false", ƒ, ƒ, "null", "null", null, null, true, "true", true, "true", "undefined", "undefined", undefined, undefined]
// (23) [0, 1, Array(3), Array(3), NaN, "NaN", NaN, {…}, {…}, false, "false", false, "false", ƒ, ƒ, "null", null, true, "true", true, "true", "undefined", undefined]
// sort 后 '字符串布尔值' 和 '布尔值' 顺序发生变化, 从而导致相邻元素类型不同
```

利用 sort() 排序方法，然后根据排序后的结果进行遍历及相邻元素比对。

> `**[sort()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)**` 方法用[原地算法](https://en.wikipedia.org/wiki/In-place_algorithm)对数组的元素进行排序，并返回数组。排序不一定是[稳定的](https://zh.wikipedia.org/wiki/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95#.E7.A9.A9.E5.AE.9A.E6.80.A7)。

## 五、利用对象的属性不能相同的特点进行去重

```js
function unique(arr) {
  if (!Array.isArray(arr)) {
    console.log('type error!')
    return
  }
  var arrry= [];
  var  obj = {};
  for (var i = 0; i < arr.length; i++) {
    if (!obj[arr[i]]) {
      arrry.push(arr[i])
      obj[arr[i]] = 1
    } else {
      obj[arr[i]]++
    }
  }
  console.log(obj)
  return arrry;
}
var arr = [0, 0, 1, 1, 'true', 'true', true, true, 'false', 'false', false, false, 'undefined', 'undefined', undefined, undefined, 'null', 'null', null, null, 'NaN', 'NaN', NaN, NaN, {}, {}, {a: 1}, {b: 2}, {a: 1, b: 2}, [1, 2, 3], [1, 2, 3], [11, 2, 3], [1, 22, 3], function fun () {}, function fun () {}, function f () {}, function fun () {return 1}, function fun () {return 2}]

console.log(unique(arr))
// {
//   0: 2
//   1: 2
//   1,2,3: 2
//   1,22,3: 1
//   11,2,3: 1
//   NaN: 4
//   [object Object]: 5
//   false: 4
//   function f () {}: 1
//   function fun () {return 1}: 1
//   function fun () {return 2}: 1
//   function fun () {}: 2
//   null: 4
//   true: 4
//   undefined: 4
// }
// (15) [0, 1, "true", "false", "undefined", "null", "NaN", {…} ==> {}, Array(3) ==> [1, 2, 3], Array(3) ==> [11, 2, 3], Array(3) ==> [1, 22, 3], ƒ ==> fun, ƒ ==> f, ƒ ==> fun, ƒ ==> fun]
```

NaN 和 数组/函数 等对象去重了, 但所有 Object 等去重过度了

所有 '字符串类基本类型'(如 'true') 和 '基本类型'(如 true) 均去重了, 但也是去重过度

这都是由于对象的 [] 的 key 只能是 string，你传递其他类型的 key，最后都转成了 string

## 六、利用 includes

```js
function unique (arr) {
  if (!Array.isArray(arr)) {
    console.log('type error!')
    return
  }
  var array =[];
  for(var i = 0; i < arr.length; i++) {
    if (!array.includes(arr[i])) { // includes 检测数组是否有某个值
        array.push(arr[i]);
      }
  }
  return array
}
var arr = [0, 0, 1, 1, 'true', 'true', true, true, 'false', 'false', false, false, 'undefined', 'undefined', undefined, undefined, 'null', 'null', null, null, 'NaN', 'NaN', NaN, NaN, {}, {}, {a: 1}, {b: 2}, {a: 1, b: 2}, [1, 2, 3], [1, 2, 3], [11, 2, 3], [1, 22, 3], function fun () {}, function fun () {}, function f () {}, function fun () {return 1}, function fun () {return 2}]

console.log(unique(arr))
// (26) [0, 1, "true", true, "false", false, "undefined", undefined, "null", null, "NaN", NaN, {…}, {…}, {…}, {…}, {…}, Array(3), Array(3), Array(3), Array(3), ƒ, ƒ, ƒ, ƒ, ƒ]
```

所有对象(包括数组/函数)均没有去重, 其余均正确去重

## 七、利用 hasOwnProperty

```js
function unique(arr) {
  var obj = {};
  return arr.filter(function(item, index, arr){
    return obj.hasOwnProperty(typeof item + item) ? false : (obj[typeof item + item] = true)
  })
}
var arr = [0, 0, 1, 1, 'true', 'true', true, true, 'false', 'false', false, false, 'undefined', 'undefined', undefined, undefined, 'null', 'null', null, null, 'NaN', 'NaN', NaN, NaN, {}, {}, {a: 1}, {b: 2}, {a: 1, b: 2}, [1, 2, 3], [1, 2, 3], [11, 2, 3], [1, 22, 3], function fun () {}, function fun () {}, function f () {}, function fun () {return 1}, function fun () {return 2}]

console.log(unique(arr))
// (20) [0, 1, "true", true, "false", false, "undefined", undefined, "null", null, "NaN", NaN, {…}, Array(3), Array(3), Array(3), ƒ, ƒ, ƒ, ƒ]
```

NaN 和 数组/函数 等对象去重了, 但所有 Object 等去重过度了

利用 hasOwnProperty 判断是否存在对象属性, 和利用对象的 [] 一样, 无法判断 Object, 过度去重

## 八、利用 filter

```js
function unique (arr) {
  return arr.filter (function (item, index, arr) {
    //当前元素，在原始数组中的第一个索引 == 当前索引值，否则返回当前元素
    if (arr.indexOf(item, 0) === index) {console.log(item, index)}
    return arr.indexOf(item, 0) === index;
  });
}
var arr = [0, 0, 1, 1, 'true', 'true', true, true, 'false', 'false', false, false, 'undefined', 'undefined', undefined, undefined, 'null', 'null', null, null, 'NaN', 'NaN', NaN, NaN, {}, {}, {a: 1}, {b: 2}, {a: 1, b: 2}, [1, 2, 3], [1, 2, 3], [11, 2, 3], [1, 22, 3], function fun () {}, function fun () {}, function f () {}, function fun () {return 1}, function fun () {return 2}]

console.log(unique(arr))
// (25) [0, 1, "true", true, "false", false, "undefined", undefined, "null", null, "NaN", {…}, {…}, {…}, {…}, {…}, Array(3), Array(3), Array(3), Array(3), ƒ, ƒ, ƒ, ƒ, ƒ]
```

NaN 被彻底去除了, 所有对象都没有去重

## 九、利用递归去重

```js
function unique (arr) {
  var array = arr;
  var len = array.length;

  array.sort(function (a, b) {   //排序后更加方便去重
    return a - b;
  })

  function loop (index) {
    if (index >= 1) {
      if (array[index] === array[index-1]) {
        array.splice(index,1);
      }
      loop(index - 1);    //递归loop，然后数组去重
    }
  }
  loop(len-1);
  return array;
}
var arr = [0, 0, 1, 1, 'true', 'true', true, true, 'false', 'false', false, false, 'undefined', 'undefined', undefined, undefined, 'null', 'null', null, null, 'NaN', 'NaN', NaN, NaN, {}, {}, {a: 1}, {b: 2}, {a: 1, b: 2}, [1, 2, 3], [1, 2, 3], [11, 2, 3], [1, 22, 3], function fun () {}, function fun () {}, function f () {}, function fun () {return 1}, function fun () {return 2}]

console.log(unique(arr))
// (29) [null, 0, ƒ, ƒ, "true", Array(3), Array(3), "false", false, "undefined", ƒ, ƒ, "null", 0, null, "NaN", NaN, NaN, {…}, {…}, {…}, {…}, {…}, Array(3), Array(3), true, 1, ƒ, undefined]
```

## 十、利用 Map 数据结构去重

    function arrayNonRepeatfy(arr) { let map = new Map(); let array = new Array(); // 数组用于返回结果 for (let i = 0; i < arr.length; i++) { if(map .has(arr[i])) { // 如果有该 key 值 map .set(arr[i], true); } else { map .set(arr[i], false); // 如果没有该 key 值 array .push(arr[i]); } } return array ; } var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}]; console.log(unique(arr)) //[1, "a", "true", true, 15, false, 1, {…}, null, NaN, NaN, "NaN", 0, "a", {…}, undefined]

创建一个空 Map 数据结构，遍历需要去重的数组，把数组的每一个元素作为 key 存到 Map 中。由于 Map 中不会出现相同的 key 值，所以最终得到的就是去重后的结果。

## 十一、利用 reduce+includes

    function unique(arr){ return arr.reduce((prev,cur) => prev.includes(cur) ? prev : [...prev,cur],[]); } var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}]; console.log(unique(arr)); // [1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]

## 十二、[...new Set(arr)]

    [...new Set(arr)] // 代码就是这么少 ----（其实，严格来说并不算是一种，相对于第一种方法来说只是简化了代码）

PS：有些文章提到了 foreach+indexOf 数组去重的方法，个人觉得都是大同小异，所以没有写上去。
