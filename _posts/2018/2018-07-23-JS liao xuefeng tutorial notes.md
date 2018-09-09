---
layout: post
title: JS 廖雪峰教程笔记
categories: JS
description: 廖雪峰官网的 JS 教程学习笔记
keywords: js
---

廖雪峰官网的 [JS 教程学习](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000)笔记

## 数据类型

1. number：整数、浮点数、科学计数法、NaN 和 Infinity；

   **注意一：** `NaN === NaN // false`，只能通过 `isNaN()` 来判断。

   **注意二：** 浮点数相等比较不能直接 `0.1 + 0.2 === 0.3；` 而是 `Math.abs(0.1 + 0.2 - 0.3 < 0.000001)`，一般六位小数精度足够了。

2. string：字符串；
3. boolean：布尔值；

   **注意：** 控制台输入 `console.log(typeof 1 > 3); // false` 和 `console.log(typeof (1 > 3)) // boolean` 是不一样的。

4. null：空值，事实上应该是一个对象：`typeof null // object`;
5. undefined: 未定义的空值。

   **注意：** 若要对变量显式定义 undefined，应该 `var a = void 0;`

6. 数组：字面量 `[1, 2, 3]` 和 `new Array(1, 2, 3)`，一般我使用字面量方法，可读性强；

   **注意：** `typeof [1, 2] === 'object'`,`[1, 2] instanceof Array === true` 和 `[1, 2] instanceof Object === true`;

7. 对象：字面量 `{a: 1, b: '2'}`；

## 变量

使用关键字 var 声明变量，如果缺少 var，变量将自动提升为全局变量。这可能会出现问题：污染全局作用域，变量命名冲突。最好是使用严格模式或者 JS 模块化。

全局模式启用：在 js 文件第一行加上 `use strict`。

## 字符串

1. 单行字符串：使用 `''` 或 `""` 包裹内容，换行使用 `\n`；
2. 多行字符串：ES6 中，使用 \` \` 反引号可以省略换行，且与单行字符串带 `\n` 是严格相等的，如：
   ```js
   var a = `你好
   我是
   lhajh`
   var b = '你好\n我是\nlhajh'
   // (a === b) === true;
   ```
3. 模板字符串

   我们都知道使用 + 号进行字符串拼接：

   ```js
   var name = 'lhajh'
   var sayHi = 'Hi,' + name
   console.log(sayHi)
   ```

   但是如果变量较多，+ 号和 引号可能出现遗漏。在 ES6 中，使用模板字符串可以减少工作量，避免错误。

   ```js
   var newSayHi = `Hi, ${name}`
   console.log(newSayHi)
   ```

   **注意：** 模板字符串必须用反引号包裹。

4. 常用属性、方法
   - 字符串一经定义，不可局部修改：`name[1] = 'a'; // 修改无效，name === 'lhajh' // true`，下面的方法也不会修改原字符串的值，而是返回一个新的字符串；
   - string.length：获取字符串长度，可以用下标进行索引；
   - String.toUpperCase(): 将一个字符串中的英文字母全部替换为大写形式；
   - String.toLowerCase(): 将一个字符串中的英文字母全部替换为小写形式；
   - String.indexOf(substring): 搜索指定子串出现的位置。找到返回子串在字符串中出现的位置，未找到返回 -1；
   - String.substring(start,end): 截取字符串，第一个参数是起始位置，默认为 0，第二个参数是终点位置。区间表示为 [start,end)。

## 数组

1. Array.length：获取数组长度。如果修改 length 属性，数组的内容也会改变。减少 length 会截断多余的数组项，增加 length 会自动填充 undefined 值。若要修改的数组项的索引值超过 length 大小，则数组自动扩充，中间用 undefined 填充，最后一个值为新修改的项；
2. Array.indexOf()：与字符串的方法一样，返回搜索项的索引值，未找到则返回 -1；
3. Array.slice(start,end): 相当于字符串中的 substring() 方法，，截取部分元素，返回一个新的 Array 数组。不传参数会返回原数组。
   ```js
   var arr = [1, 2, 3, 4, 5]
   var copyArr = arr.slice()
   copyArr // [1, 2, 3, 4, 5]
   ```
4. push 和 pop：push 是在 Array 末尾添加元素；pop 是从末尾移除元素，并且返回被移除的元素；
5. unshift 和 shift：unshift 是在 Array 首部添加元素，shift 是删除并返回第一个元素；
   **模拟队列：** 队列是先进先出，首进尾出，因此结合 unshift 和 pop 方法可以模拟一个队列；
   **模拟堆栈：** 堆栈是先进后出，首进首出，使用 unshift 和 shift 即可。
6. Array.sort()：修改当前数组各项顺序，可以传入一个比较函数自定义比较数据。该方法效率相比较与手写排序要高很多。
7. Array.reverse(): 翻转当前数组；
8. Array.splice(): splice() 方法是修改数组的万能方法，可以从指定的索引开始删除若干元素，再从该位置添加若干元素。
   - 只删除，不添加：`arr.splice(2, 2); // 从索引 2 开始删除 2 个元素`；
   - 只添加，不删除：`arr.splice(2, 0,'first','second','third');// 从索引 2 开始添加 3 个元素`；
   - 修改某个元素：`arr.splice(2, 1,'new');// 修改索引为 2 的元素`；
   - 添加多个元素，删除多个元素： `arr.splice(2, 10,'new','new','new');// 最常用`；
9. Array.concat(): 将当前的数组与另一个数组连接，另一个数组接在当前数组的后面，返回一个新数组。事实上，concat 函数的参数并不要求必须为数组，可以传入任意个值，然后将这些值依次 push 进原数组。
10. Array.join()：将数组中各项转为字符串后用指定字符串相连接，默认为逗号 `,`；
11. 多维数组：`var arr = [[1, 2, 3], [400, 500, 600], '-'];`;

## 对象

对象由若干键值对组成，键值对间用逗号隔开，最后一个键值对后面不要加逗号。访问属性可以用 `.` 点也可以用 `[]` 中括号，中括号中一般是一个变量代表的属性名。

检测某属性是否存在于对象中，可以使用 `in` 操作符：`'name' in Person;`，属性名要带引号。如果要遍历对象中所有属性，可以利用 for...in 循环：

```js
for (var key in obj) {
  console.log(key)
}
```

如果要避免继承来的属性干扰，可以用 hasOwnProperty() 方法。

## Map 和 Set

JS 的对象是键值对的集合，相当于其他编程语言的 Map。但是对象的键必须是字符串，不能是 Number 或是其他数据类型。为了解决这个问题，ES6 引入了新的数据类型 Map。

### Map

Map 是一对键值对的集合，具有极快的查找速度。

```js
// 初始化 Map 需要一个二维数组，或者空 Map 调用 set 方法赋值
var m = new Map([['adult', 18], ['child', 12], ['elder', 60]])
// 添加或修改键值对
m.set('baby', 3)
// 检测是否存在某键
m.has('adult')
// 获取键对应的值
m.get('adult')
// 删除某个键值对
m.delete('adult')
// 获取 Map 项的数目
m.size
```

### Set

Set 只存储键，不存储值，且键唯一不重复。

初始化 Set 对象可以传入一个数组或创建空 Set。数组中重复的值会被过滤，因此可以用来**数组去重**。

Set 对象有以下几种：

1. add(): 添加键，重复添加键不影响 Set 的唯一性；
2. delete(): 删除指定键，若删除成功，返回 true；若键不存在，返回 false；
3. clear(): 清空 Set；
4. has(): 检测 Set 中是否存在某个键；
5. forEach(): 根据集合中元素的顺序，对每个元素都执行提供的 callback 函数一次。callback 有三个参数: 元素的值，元素的索引和将要遍历的集合对象。

### iterable 类型

虽然 Map 和 Set 有一些新特性，弥补 Array 的一些缺陷，但是仍然存在一个问题：无法使用下标遍历。因此，ES6 新增 iterable 类型统一 Map、Set 和 Array 类型，使用 for...of 来遍历。

```js
var a = [1, 2, 3]
var s = new Set(a)
var m = new Map([[1, 1], [2, 2], [3, 3]])

for (var x of a) {
  console.log(x)
}
for (var x of s) {
  console.log(x)
}
for (var x of m) {
  console.log(`${x[0]}:${x[1]}`)
}
```

看上去，for...of 和 for...in 很相似，但是 for...in 在遍历这些对象时，会遍历自定义属性。比如 `arr.name='lhajh'`，此时 for...in 会输出 name 属性值。但是 for...of 不会，它只循环集合本身内容。

此外，iterable 类型自带 forEach 方法，每次迭代都会执行一次回调函数。

```js
var a = [1, 2, 3]
a.forEach(function(ele, index, array) {
  /**
   * ele: 当前迭代的元素值；
   * index：当前元素索引，Set 对象中该参数与第一个参数值相同；
   * array：迭代对象本身，此处指 a 数组
   */
  console.log(ele + ':' + index)
})
```

## 函数

函数是 JS 的一等公民，可以当作参数传递。具有抽象性。

### 函数定义

1. 函数声明
   ```js
   function f(x) {
     return x
   }
   ```
2. 函数表达式声明
   `var f = function (x) { return x; }`
   如果函数不执行 return 语句，返回默认值 undefined。

### 函数传参

JS 中，调用函数时并不强制传参数量与声明时参数数量一致，如果调用时传参比声明时少，会自动传入 undefined 代替，因此在声明时要对参数进行判断。

在函数中，JS 指定了一个名叫 arguments 变量，其中包含了所有在调用函数时传递的参数。

```js
function foo(x) {
  console.log('x =' + x) // 10
  for (var i = 0; i < arguments.length; i++) {
    console.log('arg' + i + '=' + arguments[i]) // 10, 20, 30
  }
}
foo(10, 20, 30)
```

从上面的例子中可以看到 arguments 不需要声明，因为是 JS 内置的。而且它和数组相似，具有 length 属性，且通过下标可以获取元素值。但它并不是数组，是另一种对象。

```js
arguments instanceof Array // false
arguments instanceof Object // true
```

有时候，我们在函数声明时显式定义了 a、b 两个参数，再判断实际调用时是否传入 c、d、e 等参数。如果我们要将 a、b 分成一组，c、d、e 分成一组，第二组的写法可能是 `var rest = [arguments[2],arguments[3],arguments[4]];`，看上去很麻烦。

在 ES6 中，可以使用 rest 参数来改写函数：

```js
function x(a, b, ...other) {
  console.log(other)
  console.log(other.length)
  console.log(other[0])
}
x(1, 2, 3, 4, 5)
```

输出：

```js
;(3)[(3, 4, 5)]
3
3
```

因此，arguments = 显式声明形参 + rest 形参。
**注意:** rest 参数只能写在最后，前面用... 标识。

### 函数变量作用域

1. 内部作用域

   如果一个变量在函数体内部申明，则该变量的作用域为整个函数体，在函数体外不可引用该变量。但是，函数内部可以使用函数外部的变量。

   在函数中还存在变量提升的现象。指的是函数在执行前，会先扫描函数体，将所有内部声明的变量都提升到函数顶部，但不会提升变量的赋值。举个例子：

   ```js
   function foo() {
     var x = 'Hello,' + y
     console.log(x)
     var y = 'Bob'
   }
   ```

   实际上，相当于：

   ```js
   function foo() {
     var x, y
     x = 'Hello,' + y
     console.log(x)
     y = 'Bob'
   }
   ```

   这样，x 在赋值时不会报错，但是此时 y 的值还没有变成'Bob'，而是 undefined。
   为了安全起见，函数内的变量声明赋值应该在函数开头完成。

2. 全局作用域

   不在任何函数内定义的变量就具有全局作用域。实际上，JavaScript 默认有一个全局对象 window，全局作用域的变量实际上被绑定到 window 的一个属性。

   ```js
   var globalVar = 'a'
   window.globalVar === globalVar // true
   ```

   之前说过函数内部可以调用外部变量，是因为函数在搜索变量时会先在内部查找，如果没有找到则向上层函数作用域中查找。而全局作用域作为顶级作用域，如果其中也没有要查找到的变量，则查找失败，报 ReferenceError 错误。

3. 命名空间

   由于全局作用域唯一，而所有 JS 文件都可以在其中声明自己的变量，往往会导致不同 JS 文件出现命名冲突的情况。因此建议声明自己的变量和函数时，将它们绑定在一个个性化全局对象中。虽然仍有可能出现冲突，但出现率会大大减少。

   ```js
   // 唯一的全局变量 MYAPP:
   var MYAPP = {}

   // 其他变量:
   MYAPP.name = 'myapp'
   MYAPP.version = 1.0

   // 其他函数:
   MYAPP.foo = function() {
     return 'foo'
   }
   ```

4. 块级作用域

   有点 JS 基础的应该都知道，在 JS 中，if 语句、for 循环等块级代码中声明的变量实际上是声明在全局作用域中的。如果在块级代码以外调用，不会出现未声明的错误。

   这可能会导致一些问题：

   ```js
   var sum = 0
   for (var i = 0; i < 10; i++) {
     console.log(i)
   }
   i++ // 10
   ```

   在 ES6 中，有一个新的关键字：let。使用 let 声明的变量在块级代码执行后不会保留：

   ```js
   for (let i = 0; i < 10; i++) {}
   i++ // SyntaxError
   ```

5. 常量

   ES6 引入关键字 const 用来声明常量。需要注意以下几点：

   1. const 声明常量和 let、var 在声明变量后不能再使用其他两种关键字声明同名变量；
   2. const 常量声明后不得修改，不然怎么叫常量呢。但对于对象, 不能修改类型, 却可以修改里面的值

6. 解构赋值

   这也是 ES6 中的语法，用来对一组变量进行赋值。

   在 ES6 出现之前：

   ```js
   var arr = [1, 2, 3]
   var a = arr[0]
   var b = arr[1]
   var c = arr[2]
   ```

   在 ES6 中：

   ```js
   var [a, b, c] = arr
   console.log(a)
   console.log(b)
   console.log(c)
   ```

   也就是说要从数组取值时，应该将一组变量用 [] 括起。如果有多层嵌套：

   ```js
   var arr = [1, [2, 3]]
   ```

   那么解构赋值的括号嵌套也要保持一致：

   ```js
   var [a, [b, c]] = arr
   ```

   如果部分值不需要，可以忽略：

   ```js
   var [, , c] = arr
   ```

   如果从对象中取值，用 {} 括起，且变量名与属性名一致, 嵌套也要一致。下面是一个简单的例子：

   ```js
   var obj = {
     x: 1,
     y: {
       z: 3
     }
   }
   var {
     x,
     y: { z }
   } = obj
   ```

   如果存在已经声明的变量，不能直接省略 var，而是用另一种表达式：

   ```js
   var x, y
   ;({ x, y } = obj)
   ```

   解构赋值还有一个常见的用法，交换两个数的值：

   ```js
   var j = 0,
     k = 1
   ;[j, k] = [k, j]
   ```

### 方法

1. 方法是对象的函数。

   ```js
   var xiaoming = {
     name: '小明',
     birth: 1990,
     age: function() {
       var y = new Date().getFullYear()
       return y - this.birth
     }
   }

   xiaoming.age // function xiaoming.age()
   xiaoming.age() // 今年调用是 28, 明年调用就变成 29 了
   ```

   在 age 方法中我们使用了 this 关键字，该变量指向当前调用方法的对象。如果直接调用 `age();`，此时 this 指向全局对象 window；而如果在严格模式下，this 的值是 undefined。

2. apply 方法

    apply 方法可以指定方法中 this 的值。该方法接收两个参数，第一个是 this 指定的对象，另一个是传入方法的参数组成的数组。

    ```js
    function getAge() {
    var y = new Date().getFullYear()
    return y - this.birth
    }

    var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
    }

    xiaoming.age() // 28
    getAge.apply(xiaoming, []) // 28, this 指向 xiaoming, 参数为空
    ```

3. call 方法

    此外，函数还有一个 call 方法，它与 apply 方法类似，但是其不要求参数组成一个数组。

    那么，什么时候使用 apply 和 call？

    事实上，他们两个没有特别大的区别，只有当参数是数组时，apply 有天生优势。

### 高阶函数

当一个函数接受的参数中有函数变量时，该函数称为高阶函数。

JS 中常见的高阶函数有：map、reduce、filter 和 sort 等等。

1. Array.map(callback)

   map 方法可以对数组中的每一个元素执行一遍函数形参，返回一个新数组，也就是在原数组和新数组之间增加一种映射关系。

   其函数形参接收 3 个参数：n 当前迭代项的值，index 当前迭代项的索引，arr 当前迭代的数组。

   ```js
   function addTen(n, index, arr) {
     console.log(arr)
     console.log(index)
     return n + 10
   }
   var origin = [1, 2, 3, 4, 5, 6]
   var result = origin.map(addTen)
   console.log(result)
   ```

   这里有一个传参的错误实例：

   ```js
   'use strict'

   var arr = ['1', '2', '3']
   var r
   r = arr.map(parseInt)
   console.log(r)
   ```

   输出：

   ```js
   1, NaN, NaN
   ```

   原因是 parseInt(string, radix) 接收 2 个参数，第一个是要转换的字符，第二个是进制。但是 map 方法默认第二个传参为索引值。导致实际运算的真实情况是

   ```js
   parseInt('1', 0) // 1, 按十进制转换
   parseInt('2', 1) // NaN, 没有一进制
   parseInt('3', 2) // NaN, 按二进制转换不允许出现 2
   ```

2. Array.reduce(callback)

   reduce 函数的函数形参 f 接收 2 个参数，第一个参数为上一次迭代时运算的结果，第二个参数是参与本次运算的数组项。

   回调函数中可以传递 3 个参数：第一个是上一次运算的结果值，第二个是当前迭代的计算值，第三个是当前计算值的索引下标，第四个是调用函数的数组对象。
   效果如下：
   `[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4);`
   相当于:

   ```js
   function f(x, y) {
     return x + y
   }
   y1 = x1 + x2
   y2 = y1 + x3
   y3 = y2 + x4
   ```

3. Array.filter 函数

   filter(callback) 用于把 Array 的某些元素过滤掉，然后返回剩下的元素。根据数组每一项在执行回调函数后返回的布尔值确定是否保留该数组项。

   例如删除偶数：

   ```js
   var arr = [1, 2, 4, 5, 6, 9, 10, 15]
   var r = arr.filter(function(x) {
     return x % 2 !== 0
   })
   ```

   与 map 和 reduce 的回调函数相同，filter 的回调函数也有 3 个参数，意义与之前的两个方法一样。

4. Array.sort(compare)

   sort 方法可以传回调函数参数, 也可以不传。如果传入自定义的比较函数，则函数的返回值应符合如下规则：

   1. x < y: 返回负值；
   2. x = y: 返回 0；
   3. x > y: 返回正值；

   如果不传比较函数，则默认将数组项转成字符串，按 ASCII 码大小排序。

### 闭包

闭包是在函数中声明函数，声明的子函数能够使用外层函数中定义的变量。当外层函数返回内部函数时，内部函数在调用时仍保存这些外层变量。

但是被返回的函数并不是立即执行的，必须手动调用。而此时，内部保存的变量状态可能会变化。举个例子：

```js
function count () {
    var arr = [];
    for(var i = 1; i <= 3; i++){
        console.log(i);
        arr.push(function(){
            console.log(i);
            return i * i;
        });
    }
    console.log(arr);
    return arr;
}

var result = count();
var r1 = result[0];
var r2 = result[1];
var r3 = result[2];

r1();
r2();
r3();
```

由于 result 中每一项的函数中保存了变量 i，而 i 又在循环后变成了 4，所以 r1/r2/r3 三个函数最后输出的都是 4 * 4 = 16。
要修改成输出 1，4，9，应该在循环中使用 let 声明 i。每一次循环，都相当于执行了一次 `let i`，由于 let 只存在于块作用域中，所以出了本次循环后别的循环中的声明不再对它有影响。

在不支持 ES6 的浏览器中，可以使用立即执行匿名函数表达式的方法：

```js
function count() {
    var arr = [];
    for (var i = 1; i <= 3; i++) {
        arr.push((function (n) {
            return function () {
                return n * n;
            }
        })(i));
    }
    return arr;
}
```

在要返回的函数外再加一层匿名函数，并将 i 作为参数传递，同时立刻执行该匿名函数。这时，内部函数引用的是匿名函数的形参，该形参的值与 i 相等，但并不会再因为 i 改变。

匿名函数立即执行写法：`(function(){})();`，用 () 括起写成函数表达式，避免 JS 语法解析报错。

使用闭包可以实现一般面向对象中的 private 私有变量。

```js
'use strict';

function create_counter(initial) {
    var x = initial || 0;
    return {
        inc: function () {
            x += 1;
            return x;
        }
    }
}

var obj = new create_counter();
obj.x; // undefined
obj.inc(); // 1
```

可以看到 obj 是无法获取到 x 值，但是能够修改 x 的值。这样就保证了 x 变量私有化。

### 箭头函数

箭头函数是匿名函数的另一种写法：

```JavaScript
// 箭头函数
x => x * x;
// 一般写法
function(x){
    return x * x;
}
```

箭头函数相当于匿名函数，并且简化了函数定义。如果函数体中只包含一条表达式，可以省略 {} 和 return，但如果有多个语句或返回值为对象字面量时，不能省略 {} 和 return。

```js
x => {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

如果参数有 0 个或 2 个及以上，需用 () 括起。

箭头函数与匿名函数有一个区别，其内部的 this 变量作用域根据上下文确定。

举一个匿名函数中 this 指向错误的例子：

```JavaScript
var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth;
        var fn = function () {
            console.log(this.birth);
        };
        return fn();
    }
}
obj.getAge(); // undefined
```

> 虽然我不知道为什么要写成匿名函数的形式，直接在 getAge 里执行不可以吗？但是廖雪峰这么举例子，那就这样吧。

而修改成箭头函数：

```JavaScript
var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth;
        var fn = () => console.log(this.birth);
        return fn();
    }
}
obj.getAge(); // 1990
```

又因为箭头函数中的 this 已经根据上下文绑定，指向外层定义者 obj，所以使用 apply 或 call 方法都无法改变其 this 指向。

### generate 生成器

ES6 提出 generate 生成器概念，它是一种数据类型，跟函数很像：

```JavaScript
function* foo(x){
    yield x + 1;
    yield x + 2;
    return x + 3;
}
var f = foo(3);
```

与普通函数不同之处：使用 function* 定义，除了 return 语句还可以使用 yield 返回多次。上面的例子中 f 是一个 generate 对象，想要获得 foo 执行返回的值，需要使用 Generate.next() 方法，该方法返回一个对象 `{value: 值, done:true/false}`。

流程应该是：

1. 调用 f.next()，执行一遍 foo，
2. 遇到 yield 关键字，获取返回值，
3. 判断该值是否为最后一个返回值，如果是则 done 属性的值为 true，否则为 false。

除了使用 next 方法，还可以用 for...of 方法迭代 generate 对象，这时不需要我们手动判断 done 属性。

```js
for (var f of fib(10)) {
    console.log(f); // f 的值就是 value 属性的值
}
```

由此可见，generate 对象可以在要返回一组数据时，代替数组作为返回值这种方式。

此外，generate 还有优化 ajax 代码的作用，实现一个异步方法。
[Generator 函数的含义与用法](http://www.ruanyifeng.com/blog/2015/04/generator.html?20170921130219)

## 标准对象

使用 typeof 操作符区分数据类型：

```js
typeof 123; // 'number'
typeof NaN; // 'number'
typeof 'str'; // 'string'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof Math.abs; // 'function'
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'
```

可以区分的有 number、string、boolean、undefined、function 和 object 六个基本数据类型，`typeof null` 返回 'object'。

基本数据类型还有对应的封装对象类型：

```js
var n = new Number(123);
var b = new Boolean(true);
var s = new String('string');
```

这三个封装函数如果在赋值时不使用 new 关键字生成对象实例，功能会变成基本类型转换：

```js
var n = Number('123'); // 123
var b = Boolean(0); // false
var s = String(123); //'123'
```

要明确某个对象是否为数组，可以使用 Array.isArray() 方法。是数组会返回 true，不是则返回 false。

最后，虽然大多数数据类型都有 toString 方法，但是 number 数据调用 toString 需要特殊处理，否则会报错。

```js
123..toString(); // '123', 注意是两个点！
(123).toString(); // '123'
```

### Date 对象

顾名思义，Date 对象表示日期时间。创建的默认实例是代码执行的时间。

想要创建指定时间的 Date 实例，有两种方法：

```js
// 第一种方法
var date = new Date(2020, 0, 1, 0, 0, 0, 0);
// 参数依次为年、月（从 0 开始）、日、时、分、秒。

// 第二种方法
var dateStamp = Date.parse('2020-01-01-24T00:00:00.0+08:00');
// 这是用 parse 方法解析一个符合 ISO 8601 格式的字符串，返回值是一个时间戳
var date = new Date(dateStamp);
// 通过时间戳再创建 Date 实例。
```

Date 对象具有的方法如下：

```JavaScript
var date = new Date();
date.getFullYear(); // 获取年份
date.getMonth(); // 获取月份，月份并不是 1~12，而是 0~11
date.getDate(); // 获取日期
date.getDay(); // 获取星期几
date.getHours(); // 小时，24 小时制
date.getMinutes(); // 分钟
date.getSeconds(); // 秒
date.getMillseconds(); // 毫秒
date.getTime(); //number 类型时间戳
Date.now(); // 获取当前时间
```

### 正则对象 RegExp

正则表达式是匹配字符串的强大武器。

廖雪峰老师的教程讲的不是很详细，建议看看 [JavaScript 正则迷你书](https://zhuanlan.zhihu.com/p/29707385)。

### JSON 对象

json 格式与 JS 对象字面量的格式一致。目前都用 JSON 取代 XML 传输报文格式。

JSON 对象可以序列化为字符串：

```js
var json = {
    name: 'lhajh',
    age: 22,
    gender: 'male'
}
var jsonString = JSON.stringify(json);
```

JSON.stringify 接收 3 个参数：

1. value: 要序列化的值；
2. replacer：如果该参数是一个函数，则在序列化过程中，被序列化的值的每个属性都会经过该函数的转换和处理；如果该参数是一个数组，则只有包含在这个数组中的属性名才会被序列化到最终的 JSON 字符串中；如果该参数为 null 或者未提供，则对象所有的属性都会被序列化;
3. space: 指定缩进用的空白字符串，用于美化输出（pretty-print）；如果参数是个数字，它代表有多少的空格；上限为 10。该值若小于 1，则意味着没有空格；如果该参数为字符串 (字符串的前十个字母)，该字符串将被作为空格；如果该参数没有提供（或者为 null）将没有空格。

> 具体例子应该想一想就知道，我就不举例子啦~

如果我们获取的是一个 JSON 格式的字符串，可以使用 JSON.parse 方法将其解析成 JS 对象。

```js
var jsonString = '{"name": "lhajh", "age": 22}';
var json = JSON.parse(jsonString);
```

## 面向对象编程

### 创建对象

创建对象首先要有一个构造函数：

```js
function Person (name) {
    this.name = name;
}

Person.prototype.sayHello = function () {
    console.log('hello' + this.name);
}
```

一般将属性声明在构造函数内部，公用方法声明在构造函数的原型上。如果用 new 关键字调用，Person 构造函数返回的就是 this 值，而如果不用 new 关键字，返回的是 undefined。所以一定不要忘记使用 new 关键字。但是，为了防止没有 new 的出错情况，需要修改代码，提高健壮性：

```js
function person (name) {
    var obj = new Object();

    obj.name = name;
    obj.sayHello = function () {
        console.log('hello' + this.name);
    }
    return obj;
}
```

因为这不算是一个构造器函数，所以函数名首字母就不大写了。

### class 继承

ES6 中使用关键字 class 来简化类的定义。

```js
class Person {
    constructor (name) {
        this.name = name;
    }

    hello () {
        console.log(`hello ${this.name}`);
    }
}
```

此外，还是用 extends 关键字简化类的继承。

```js
class Man extends Person{
    constructor (name,age) {
        super(name); // 使用 super 调用父类构造函数
        this.age = age;
    }

    showAge () {
        console.log(`I am ${this.age} years old`);
    }
}
```

class 关键字与原型继承的实现原理相同，但是代码简化了很多。

## 浏览器

| 浏览器  | 渲染引擎 | JS 引擎                |
| ------- | -------- | ---------------------- |
| IE      | Trident  | Chakra (查克拉)        |
| Chrome  | Blink    | V8                     |
| FireFox | Gecko    | OdinMonkey             |
| Safari  | Webkit   | SquirrelFish（松鼠鱼） |

### 浏览器对象

在用户浏览网页的时候，浏览器会自动创建一些对象，这些对象存放着浏览器窗口的属性和相关信息，也就是大家熟称的 BOM。浏览器对象模型（BOM）定义了很多浏览器对象，包括 window、navigator、screen、location、document 和 history 等。

1. window 对象

   window 对象除了作为顶级全局作用域外，还保存当前浏览器窗口的部分属性。比如 `innerWidth` 和 `innerHeight` 属性，指浏览器窗口内部宽高（不包括工具栏、菜单栏等高度）。而 `outerHieght` 和 `outerWidth` 属性可以获取全部宽高。
2. navigator 对象

   navigator 对象指代浏览器的代理信息。常用属性有
   1. navigator.appName：浏览器名称；
   2. navigator.appVersion：浏览器版本；
   3. navigator.language：浏览器设置的语言；
   4. navigator.platform：操作系统类型；
   5. navigator.userAgent：浏览器设定的 User-Agent 字符串。

一般判断浏览器类型，不推荐直接根据 navigator 的属性判断，而是根据某些特定属性是否存在来判断：

```js
//IE9 以下获取浏览器宽度用 document.body.clientWidth
//IE9 以上使用 window.innerWidth
var width = window.innerWidth || document.body.clientWidth;
```

3. screen 对象

   screen 对象表示屏幕的信息，常用的属性有：

   1. screen.width：屏幕宽度，以像素为单位；
   2. screen.height：屏幕高度，以像素为单位；
   3. screen.colorDepth：返回颜色位数，如 8、16、24。

4. location 对象

   location 对象表示当前页面的 URL 信息。
   常用属性有：

   1. location.protocol: url 的传输协议, 包括 `:`；
   2. location.host: url 的域名 + 端口号；
   3. location.hostname: url 的域名；
   4. location.port: url 中的端口；
   5. location.pathname: 域名后的完整文件目录；
   6. location.search: 文件目录后的参数，包括 `?`；
   7. location.hash: `#` 后的 hash 参数。

例如，一个完整的 URL：

`http://www.example.com:8080/path/index.html?a=1&b=2#TOP`

可以用 location.href 获取。要获得 URL 各个部分的值，可以这么写：

```js
location.protocol; // 'http:'
location.hostname; // 'www.example.com'
location.port; // '8080'
location.pathname; // '/path/index.html'
location.search; // '?a=1&b=2'
location.hash; // '#TOP'`
```

要加载一个新页面，可以调用 location.assign()。如果传入的 url 不带 `http` 等协议，url 会默认连接在当前页面的 url 后。

如果要重新加载当前页面，调用 location.reload() 方法非常方便。

5. document 对象

   document 对象指代当前页面，是整个 DOM 的根节点。比如可以使用 document.title 修改当前页面的标题。还可以使用 document.querySelector() 获取元素节点。此外，document 还有 cookie 属性，获取当前页面存储在用户客户端中 的 cookie。

6. history 对象

   history 对象保存了浏览器的历史记录，JavaScript 可以调用 history 对象的 back()或 forward ()，相当于用户点击了浏览器的 “后退” 或“前进”按钮。但是现在前后端交互往往使用 Ajax 来处理，简单粗暴的 history.back 会影响用户体验。因此不建议使用 history 对象。

### 更新 DOM

HTML 文档中的节点总体上被看作一颗 DOM 树上的节点，对 DOM 的操作与对树的操作一样，有修改、删除、插入遍历等操作。当然，首先得获取要操作的节点：getElementById()、getElementsByTagName()、getElementsByClassName()、querySelector()、querySelectorAll() 等方法都可以按需使用。

1. 修改节点内容

   修改内容有两种方式，一种是修改文本：对 innerText 或 textContent 属性赋值即可；还有一种是修改内部 html 文档：修改 innerHtml 属性，将节点内的所有子结构都替换为要修改的内容。

2. 插入新节点

   在目标节点的基础上插入原始节点的第一种方法：destEle.appendChild(oriEle), 将 oriEle 节点插入到 destEle 节点中。如果 oriEle 节点已经存在于文档流中，将会被剪切到目标节点。当然也可以使用 document.createElement() 来创建一个新的节点，使用 appendChild 方法插入。这种插入方法会将子节点插入在父节点最后。

   另一种方法是使用 insertBefore() 方法，传入两个参数：插入子节点和参照物子节点。作用是将插入子节点插入在参照物子节点之前。

3. 删除节点

   删除节点只有一个方法：removeChild()。要想删除某个节点，先获取其父元素节点 Node.parentElement，然后父元素节点调用 removeChild 方法即可。这种删除方法，删除的节点仍保存在内存中，但是文档流中已经不存在其位置了。

### Ajax

Ajax 的意思是使用 JS 执行异步网络请求。核心思想是使用 XMLHttpRequest 对象来传输报文。

手写一个兼容低版本 IE 的 Ajax 代码：

```JavaScript
var request;
if (window.XMLHttpRequest) {
    request = new XMLHttpRequest();
} else {
    request = new ActiveXObject('Microsoft.XMLHTTP');
}

function success (text) {
    console.log(text);
}

function fail () {
    console.log('fail');
}

request.onreadystatechange = function () {
    if (request.readyState === 4) {
        // 请求成功
        if (request.status === 200) {
            // 200 成功
            return success(request.responseText);
        } else {
            // 失败
            return fail();
        }
    } else {
        // 继续请求。
    }
}

request.open('GET','/api/categories?id=1');
request.responseType = 'json';
request.setRequestHeader('Accept', 'application/json');
request.send();
```

上面的代码中，根据是否能够使用 XMLHttpRequest 对象，判断用户使用的是否为现代浏览器。如果 XMLHttpRequest 对象无法使用，则说明用户使用的是低版本 IE，此时用使用 ActiveXObject 对象来传输报文。

此外，还要考虑 GET 请求和 POST 请求的区别。GET 请求不需要向 send() 方法传递额外参数，POST 请求需要将 body 中的数据以字符串或 formData 对象形式传递。

```js
request.open('post','/api/categories');
request.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
request.send('id=1');
```

然后，XMLHttpRequest 对象有 5 种状态 readyState，当状态改变时触发 onreadystatechange 回调函数。

| 值  | 状态             | 描述                                              |
| --- | ---------------- | ------------------------------------------------- |
| 0   | UNSENT           | 代理被创建，但尚未调用 open() 方法。              |
| 1   | OPENED           | open() 方法已经被调用。                           |
| 2   | HEADERS_RECEIVED | send() 方法已经被调用，并且头部和状态已经可获得。 |
| 3   | LOADING          | 下载中； responseText 属性已经包含部分数据。      |
| 4   | DONE             | 下载操作已完成。                                  |

若要判断请求的数据返回成功，同时根据 readyState 和 status 两个来判断即可。status 即 http 协议的状态码。

最后是请求完成后，XMLHttpRequest 对象的 responseText 中会保存后台返回的数据，一般是一个 JSON 格式文本。

关于跨域以及解决方案，可以看看 [这篇](https://segmentfault.com/a/1190000012469713)。一般解决方案是 jsonp 和 cors。

### Promise

JS 的事件都是单线程执行，这也导致一些操作必须使用异步回调的方式。而 Promise 出现的目的就是实现异步程序的另一种解决方案，避免了回调地狱。

Promise 是一个容器，存放着某个未来才结束的事件。它提供统一的 API，对所有异步操作都使用同一种方法处理。

Promise 对象一共有 3 种状态：pending（等待异步事件执行）、fulfilled（事件执行成功）、rejected（事件执行失败）。Promise 对象的状态转换只有 p->f 和 p->r 两种，通过用户定义的 resolve 和 reject 函数 来改变状态。一旦状态转换完成，Promise 对象的状态定型，此时叫 resolved 状态。如果在控制台中打印 fulfilled 状态的 Promise 对象，其 [[PromiseStatus]] 值为 resolved。因此可以将 resolved 与 fulfilled 等价。

1. 创建 Promise 对象。

   使用 new 关键字创建 Promise 对象，传入一个函数参数，该函数带有 2 个函数参数：resolve 和 reject。resolve 函数作用是 Promise 的状态改变为 fulfilled，而 reject 函数作用是将 Promise 状态改为 rejected。

   举个例子：

   ```JavaScript
   // 创建 Promise 对象实例，此时，内部函数的语句将立刻执行。
   var promise = new Promise(function (resolve, reject) {
       // 定义异步任务 1 + 2，返回值保存在 result 变量中
       var result = 1 + 2;
       // 根据异步结果改变 Promise 状态
       if (result === 3) {
           // 异步任务执行成功，调用 resolve 函数，同时改变 Promise 状态为 fulfilled
           resolve('成功');
       } else {
           // 执行失败，调用 reject 函数，同时改变 Promise 状态为 rejected
           reject('失败');
       }
   });

   function success(text){
       console.log(text);
   }

   function fail(text){
       console.log(text);
   }
   //Promise.then 方法是用来指定异步任务执行后的回调函数。
   // 下面的代码中 success 方法和 fail 方法
   // 但他们并不对应 Promise 实例创建时传入的 resolve 和 reject 方法。
   // 只有当 resolve 方法调用后 Promise 状态发生改变，success 方法才会被调用，fail 方法同理。
   // 两个参数顺序是固定的，不可交换。
   promise.then(success, fail);
   ```

2. 使用要点

   在创建 Promise 对象时，执行 resolve() 或 reject() 函数并不会结束该函数的运行，因为它们会等当前函数中的同步任务完成后再执行。

   ```js
   new Promise((resolve, reject) => {
     resolve(1);
     console.log(2);
   }).then(r => {
     console.log(r);
   });
   // 先输出 2 再输出 1
   ```

   如果一个 Promise 对象中返回了另一个 Promise 对象，那么前一个对象的状态将由后一个对象的状态决定。

   ```js
   var p1 = new Promise(function (resolve, reject) {
       reject(new Error('p1 failed'));
   });

   var p2 = new Promise(function (resolve, reject) {
       resolve(p1);
       // 等价于 p1.then(resolve).catch(reject);
   });
   // 此时 p2 相当于 p1
   p2.then(function () {
       console.log('p2 resolved');
   }).catch(function () {
       console.log('p2 failed');
   })
   ```

3. then 方法和 catch 方法

   then 方法有 2 个参数，一个是 fulfilled 状态后的回调函数，一个是 rejected 状态后的回调函数。一旦当前 js 中的同步任务完成，Promise 对象开始调用 resolve/reject 方法，并且从 pending 状态改变为 resolved 状态，此时 then 方法获得通知，调用相应的回调函数。

   catch 方法与 then 方法本质相同，但只需要传入 rejected 状态的回调函数，相当于 then(null,reject(){});

   这两个函数都可以捕获错误。如果有多个链式调用，Error 会累积到第一个 catch 语句处理。

4. all 方法

   Promise.all([]) 方法接收一个 Promise 对象数组，如果数组项中存在不是 Promise 对象，则调用 resolve() 方法对它进行处理。

   ```js
   var all = Promise.all([p1, p2, p3, ..., pn]);
   ```

   all 方法的作用是将多个 Promise 的运行结果进行与运算，只有当数组中所有 Promise 的状态都是 fulfilled 状态，all 这个 Promise 对象的状态才是 fulfilled。而一旦其中有一个状态为 rejected，all 的状态就是 rejected。

   此外，如果数组中的子项独自声明了 catch 或 then 方法，all 的 catch 方法将不会被调用。因为子项在执行 catch 和 then 方法后会返回另一个新的 Promise 对象，状态为 resolved。

5. race 方法

   race 方法是将一组 Promise 对象竞速，谁的任务结果最先出来，就返回谁。

6. resolve 方法

   resolve 方法作用是将传入的参数转换成 Promise 对象，新对象在执行 resolve 方法时传入的参数，也会传递给 then 方法中定义的回调函数。

   ```js
   var p = new Promise(function (resolve,reject) {
       resolve(1);
   });

   Promise.resolve(p).then(function (a) {
       // a===1
       // a!==p
       console.log(a);
   })
   ```

   有四种情况需要处理：

   1. Promise 对象：如果传入的参数是 Promise 对象，则原样返回。
   2. thenable 对象：thenable 对象指的是具有 then 方法的对象。如：

   ```js
   var thenable = {
       then: function (resolve,reject) {
           resolve();
       }
   }
   ```

   resolve 方法会将该对象转换成 Promise 对象，并且立刻执行该对象的 then 方法。

   3. 其他类型：对于没有 then 方法的对象，或者根本不是对象的参数，resolve 方法将返回一个新的 Promise 对象，并且该对象状态为 fulfilled。
   4. 无参数：没有参数的 resolve 方法会返回一个新的 Promise 对象，且状态为 fulfilled。

7. reject 方法

   reject 方法会返回一个状态为 rejected 的 Promise 对象。与 resolve 方法有区别的地方，就是该方法的参数会传递给 rejected 对应的回调函数。

   ```js
   var p = new Promise(function (resolve,reject) {
       reject(1);
   });

   Promise.reject(p).catch(function (a) {
       // a!==1
       // a==p
       console.log(a);
   })
   ```

8. 改写 Ajax 方法

   使用 Promise 对象改写 Ajax 方法：

   ```js
   function ajax (url) {
       var promise = new Promise(function (resolve,reject) {
           var xhr = new XMLHttpRequest();
           xhr.open('GET',url);
           xhr.send();
           xhr.onreadystatechange = function () {
               if (xhr.readyState === 4) {
                   if (xhr.status === 200) {
                       resolve(xhr.response);
                   } else {
                       reject(new Error('get failed'));
                   }
               }
           };
           xhr.responseType = 'json';
           xhr.setRequestHeader('Accept','application/json');
       });
       return promise;
   }
   ajax('www.baidu.com').then(function (json) {
       console.log('res:'+json);
   }).catch(function (err) {
       console.log(err);
   });
   ```

Promise 也有一些缺点。首先，无法取消 Promise，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。第三，当处于 pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。
