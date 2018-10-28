---
layout: post
title: ES6 新特性的注意点
categories: JS
description: ES6 新特性的注意点
keywords: js, ES6
---

## 一、块级作用域与函数声明

在 ES5 中，块级作用域中是不能写函数声明的。

```js
if (false) {
  function f() {
    console.log('123')
  }
}
```

上面的例子在 ES5 中是非法的。但是浏览器并不一定会报错，因为没有完全执行这个规范。

而在 ES6 中，块级作用域里的函数声明相当于使用了 let 关键字，在块级作用域外是无法调用的。

举个例子：

```js
function f() {
  console.log(123)
}
{
  if (false) {
    function f() {
      console.log(321)
    }
  }
  f()
}
```

上面的代码，在 ES5 中执行会打印 321，因为浏览器对这段代码的解析是：

```js
{
  function f() {
    console.log(321)
  }
  if (false) {
  }
  f()
}
```

而在 ES6 的浏览器中，虽然按规范应该打印外层作用域的 123，但实际上会报错。因为浏览器为了兼容老版本，仍然不遵守 ES6 的规范，使用的是下面的规则：

1. 允许在块级作用域内声明函数。
2. 函数声明类似于 var，即会提升到全局作用域或函数作用域的头部。
3. 同时，函数声明还会提升到所在的块级作用域的头部。

所以，ES6 浏览器中的实际代码是这样的：

```js
{
  var f = undefined
  if (false) {
    f = function() {
      console.log(321)
    }
  }
  f()
}
```

这才导致实际结果是 `f is not a function` 错误。

因此，虽然 ES6 支持块级作用域声明函数，但还是尽量不要这么做。

## 二、对象的解构赋值究竟把值赋给了谁？

ES6 出现了一个很方便的特性：解构赋值。它的写法如下：

```js
let [x, y, z] = [1, 2, 3]
let { a, b, c } = { a: 1, b: 2, c: 3 }
// 这时，x=a=1,y=b=2,z=c=3
```

例子中对象的解构赋值写法是一种简写，实际上应该是：

```js
let {a:a,b:b,c:c} = {a:1,b:2,c:3}；
```

分析 `{key:value}` 形式，key 是用来匹配右侧对象的键名，而 value 才是真正被赋值的变量。

## 三、字符串的新方法

ES6 中，对字符串扩展了新的方法：includes()、startsWith() 和 endsWith()。

1. includes()：返回布尔值，表示是否找到了参数字符串。
2. startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
3. endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。
   这三个方法都接收第二个参数，表示开始搜索的位置。而 endsWith() 的第二个参数表示的是在前 n 个字符组成的字符串中搜索。

此外还有 repeat(n) 方法，用来将字符串重复 n 次。如果 n 不是整数，会使用 Math.floor() 方法取整。如果是 NaN，当作 0 处理，其他类型先转成 number 类型，使用的是 parseInt 方法。

还有两个实用的方法是 padStart(length,str) 和 padEnd(length,str)，能够在字符串前或尾填充指定字符串达到指定长度。如果省略第二个参数，默认为空格。

## 四、模板字符串

ES6 中使用反引号强化字符串的表现，这被成为模板字符串。在模板字符串中，空格、换行都会得到保留。而且还能调用变量：`${变量名}`。

同时，模板字符串还能用来当作函数参数。

```
function hello(msg){
    console.log("hello"+msg);
}
hello`whiteyin`;//hello whiteyin
```

不过，如果模板字符串当作函数参数时有使用到变量，函数实际接收到的参数就不是一个转换后的字符串。以上面的例子来说：

```
var name = "whiteyin";
hello` 我是 ${name}。`;
// 相当于
hello(['我是','。'],name);
```

也就是说，模板字符串中的变量在计算后会依次被添加到最后一个参数上，而其他非变量的字符串会按顺序插入一个数组，这个数组始终是函数的第一个参数。

## 五、正则表达式的新修饰符

### u 修饰符

为了匹配超过 \ uFFFF 的四字节 unicode 编码，可以使用 u 修饰符。

```
/𠮷{2}/.test('𠮷𠮷') // false
/𠮷{2}/u.test('𠮷𠮷') // true
```

### y 修饰符

y 修饰符也称为粘连修饰符，与 g 修饰符功能类似，但是比它严格。

```
var s = 'aaa_aa_a';
var r1 = /a+/g;
var r2 = /a+/y;

r1.exec(s) // ["aaa"]
r2.exec(s) // ["aaa"]

r1.exec(s) // ["aa"]
r2.exec(s) // null
```

例子中，使用 y 修饰符匹配时只会匹配第一组 `aaa`，再次匹配剩余字符串 `_aa_a` 时，因为第一个位置是 `_` 所以匹配失败。而 g 修饰符会将剩余字符串当作新串，仍能匹配到 `aa`。

实际上，y 修饰符的设计目的就是使 `^` 匹配符全局有效。

## 六、对有穷和 NaN 判断的优化

ES6 新增两个 Number 的方法 isFinite 和 isNaN。之前已经有两个同名方法注册在全局下，我们用 window.isFinite 和 window.isNaN 来表示。

```
isFinite("123");//true
Number.isFinite("123");//false

isNaN("a");//true
Number.isNaN("a");//false
```

相比较与 window.isFinite 和 window.isNaN，Number.isFinite 和 Number.isNaN 会对所有不是 number 类型参数返回 false。

## 七、函数参数默认值

ES5 中想要给函数参数设置默认值，可以使用 || 运算符：

```
function f(x,y){
    x = x||1;
    y = y||"2";
    console.log(x,y);
}
```

但是这样会导致一个问题，如果 x 或 y 的值是 falsy 值，则会取值错误：

```
f(3,"");// 输出 3,"2"
```

而在 ES6 中，可以使用默认参数语法：

```
function f(x=1,y="2"){
    console.log(x,y);
}
```

这样的写法很方便，也很容易懂。但是会有一些规则要遵守：

1. 不能在函数体中使用 let 或 const 再次声明与形参同名的变量：
   ```
   function f(x=1){
       let x;
   }
   f();//Identifier 'x' has already been declared
   ```
2. 使用默认参数时不能有多个同名形参：
   ```
   function f(x=1,x,y){}//Duplicate parameter name not allowed in this context
   ```
3. 如果有默认值的形参后面没有设置默认值的参数，那么调用函数时不能省略这种参数：
   ```
   function f(x,y=1,z){}
   f(1,,3);// 报错
   f(1,undefined,3);// 没问题
   ```

使用默认参数会对函数的 length 属性有影响。因为 length 的值会变成没有默认值的参数个数。比如：

```
function f(a,b,c=1){}
f.length;//2
```

如果一个设置默认值的参数不是尾参数，那么它后面的所有参数都不会计入 length 中。

```
function f(c=1,a,b){}
f.length;//0
```

如果默认参数值是一个函数，这个函数可能形成了一个闭包，它记住的是函数所在的作用域。

```js
var a = 1
function f() {
  var a = 2
  function g(
    h = () => {
      console.log(a)
    }
  ) {
    var a = 3
    h()
  }
  g()
}
f() // 输出 2
```

还有个更复杂的例子：

```js
function f(
  x,
  y = () => {
    x = 3
  }
) {
  x = 1
  y()
  console.log(x)
}
f() //3
```

应该说，f() 这个圆括号内形成一个作用域:

```js
f(
    {// 块作用域
        let x;
        let y=function(){
            x = 3;
        };
        return [x,y];
    }
)
```

大概是这样吧，y 相当于一个闭包函数，记住了 x 的引用，所以后面调用时会修改这个引用。

不过这种规则确实违反了我的正常认知，所以我觉得还是不要这么写比较好。

## 八、函数的名字

ES6 的函数新增了一个 name 属性，用来读取函数的名字。

```js
// 具名函数返回函数名
function f() {}
f.name //f
// 匿名函数赋给变量
var f = function() {}
f.name //f
// 具名函数赋给变量
var g = function f() {}
g.name //f
// 变量函数赋给变量
var f = function() {}
var g = f
g.name //f
```

总结一下就是 name 属性应该是跟函数地址绑定，如果这个地址的函数没有名字，那么在声明或赋值语句执行后会设置 name 属性值，此后不管是什么变量指向这个地址，都不会改变 name 属性的值。

## 九、数组空位

如果我们使用 `new Array(10);` 来创建一个数组，返回的是一个有 10 个空位的数组，length=10。这里空位的概念并不等同于 undefined，因为空位没有值，而 undefined 是有值的。

在 ES5 中，数组方法对空位的处理是不一样的，可以总结如下：

1. forEach(), filter(), reduce(), every() 和 some() 都会跳过空位。

   ```
   // forEach 方法
   [,'a'].forEach((x,i) => console.log(i)); // 1

   // filter 方法
   ['a',,'b'].filter(x => true) // ['a','b']

   // every 方法
   [,'a'].every(x => x==='a') // true

   // reduce 方法
   [1,,2].reduce((x,y) => return x+y) // 3

   // some 方法
   [,'a'].some(x => x !== 'a') // false
   ```

2. map() 会跳过空位，但会保留这个值
   ```
   // map 方法
   [,'a'].map(x => 1) // [,1]
   ```
3. join() 和 toString() 会将空位视为 undefined，而 undefined 和 null 会被处理成空字符串。

   ```
   // join 方法
   [,'a',undefined,null].join('#') // "#a##"

   // toString 方法
   [,'a',undefined,null].toString() // ",a,,"
   ```

   在 ES6 新增的方法中，空位会被处理成 undefined。尽管如此，还是应该拒绝出现空位的情况。

## 十、Object.is()

在 ES6 之前，比较两个值相等只能使用 `==` 和 `===`，这两种方法各有各的缺点。
首先，== 会判断左右两边值的类型，如果不同则会类型转换。而 `===` 在判断 - 0 和 + 0 比较时会返回 true，而 `NaN===NaN` 会返回 false。

为了能够得到更符合人直觉的严格比较，ES6 提出新的比较方法 Object.is()，它与 `===` 表现类似，但是弥补了上述的两个缺点。如果浏览器没有支持这个方法，可以使用下面的代码 polyfill：

```js
Object.defineProperty(Object, 'is', {
  value: function(x, y) {
    if (x === y) {
      return x !== 0 || 1 / x === 1 / y
    }
    return x !== x && y !== y
  },
  configurable: true,
  enumerable: true,
  writable: true
})
```

## 十一、super 的 this 绑定

ES6 中，super 关键字指向当前对象的原型对象，同时它只能在对象的方法中调用，不能在其他地方使用。

如果要调用原型对象的属性：

```
var proto = {
    foo:1
}
var obj = {
    foo(){
        console.log(super.foo);
    }
}
Object.setPrototypeOf(obj,proto);
obj.foo();//1
```

但是如果要调用原型对象的方法，那会有一个 this 指向的问题：

```js
var proto = {
  foo: 1,
  bar() {
    console.log(this.foo)
  }
}
var obj = {
  foo: 2,
  bar() {
    super.bar()
  }
}
Object.setPrototypeOf(obj, proto)
obj.bar() //2
```

也就是说，super.bar 实质上是 `Object.getPrototypeOf(this).bar.call(this);` 从而 this 仍指向当前对象。

## 十二、Symbol 与 Symbol.for

Symbol 是 ES6 中新增的原始类型，用来表示独一无二的值。使用方法是 `const s = Symbol("123");`。一般都要给 Symbol 函数传递一个字符串用来标记该 Symbol 变量的名字。如果传入的是对象，根据 toPrimitive 原则要调用 toString 方法。

尽管传入参数可以标记不同名的 Symbol 变量，但是同名的 Symbol 变量，相互之间并不是等价的。

```js
const s1 = Symbol('1')
const newS1 = Symbol('1')
s1 === newS1 //false
```

也就是说，这种方法创建的 Symbol 并不是唯一命名的。
如果想要创建一个 Symbol，并且如果之前已经有相同命名的 Symbol 则使用已有的 Symbol，否则重新创建一个。可以使用 Symbol.for()。

```js
const s1 = Symbol('123')
const s2 = Symbol.for('123')
const s3 = Symbol.for('123')

s1 === s2 //false
s2 === s3 //true
```

这个例子表明，s2 和 s3 这两个使用 Symbol.for() 创建的 Symbol 变量实质上是等价的。原理就是 Symbol() 创建的变量不会在全局中登记，而 Symbol.for() 创建的变量会在全局中登记，当下次调用 Symbol.for() 时，会检测是否登记过该变量。

```js
const s1 = Symbol() //Symbol()
const s2 = Symbol.for() //Symbol(undefined)
```

另外，如果不给这两个函数传参，得到的值也是不一样的。

如果要获取某个登记过的 Symbol 值，可以使用 Symbol.keyFor() 方法。

```
const s = Symbol.for("123");
Symbol.keyFor(s);//123

const s2 = Symbol("123");
Symbol.keyFor(s2);//undefined
```

对于没有登记过的 Symbol 会返回 undefined。传入其他类型参数会报错 TypeError。

## 十三、Set 和 Map

Set 中没有重复值，事实上这是因为 Set 的键名就是值。

```
const arr = [1,"2",{},[],null,undefined,true];
const set = new Set(arr);
set.forEach((key,value)=>console.log(key,"+",value));
/* 输出
//1":"1
//1:1
//{}:{}
//[]:[]
//null":"null
//undefined":"undefined
//true":"true
*/
```

另一方面，Set 中的键值对的顺序与添加时的顺序一致。

Map 是对原始对象的升级，因为原始对象的属性名都是字符串类型，也就是 `字符串：值`，而 Map 的属性名可以是任意值，也就是 `值：值`。

比较 Set 和 Map 的 CURD 接口：
类型 | Set|Map
--|--|--
新增 / 修改 | add|set
删除 | delete|delete 和 clear
查找 | 没有 | get
检测存在 | has|has
默认遍历方法 | values|entries
元素个数 | size 属性 | size 属性
ES6 给出的 Set 接口中并没有获取某一特定值的方法，感觉也没有这么个需求。毕竟键值相同，如果知道要找什么值，那为什么还要遍历 Set 去找这个值呢？反正就是没有这个需要。

### 关于 WeakSet 和 WeakMap

这两个是弱引用的 Set 或 Map，成员的键名只能是对象，因为 WeakSet 的键与值一样，所以它的成员只能是对象。如果这些对象在外部引用消除，垃圾回收了，WeakSet 或 WeakMap 里的引用也会消除。也就是说，这两个数据类型里的引用会被垃圾回收机制忽视。由于不知道什么时候会垃圾回收，所以这两个数据结构可能每次返回值都不一样。因此，没有给他们遍历接口，也没有 size 属性。而利用这个特性，一旦键名对象不再被外部需要，WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。

## 十四、生成器 Generator

Generator 是 ES6 提出的一种异步解决方案，也可以看成是一个状态机，可以生成一个遍历器对象。声明使用 `function*` 表达形式，内部的状态跳转使用 yield 关键字。

```js
// 定义一个 Generator 函数
function* gen() {
  console.log('第一次运行')
  console.log('第一次中断')
  yield 1
  console.log('第二次运行')
  console.log('第二次中断')
  yield 2
  console.log('第三次运行')
  console.log('运行结束')
  return 3
}
// 执行该函数，返回一个迭代器对象
const g = gen()
console.log(g)
//{[[GeneratorStatus]]: "suspended"}，这里 GeneratorStatus 是该状态机当前的状态
// 遍历方式一：执行 next
g.next() // 第一次运行 第一次中断 {value: 1, done: false}
g.next() // 第二次运行 第二次中断 {value: 2, done: false}
g.next() // 第三次运行 运行结束 {value: 3, done: true}

// 遍历方式二：for...of
for (let obj of g) {
  console.log(obj)
}
/* 第一次运行
第一次中断
1
第二次运行
第二次中断
2
第三次运行
运行结束 */

console.log(g)
//{[[GeneratorStatus]]: "closed"}，最后状态改变了
```

可以看出几个特点：

1. 每遍历一次，返回一个对象，有两个属性：value 和 done。value 代表 yield 或 return 的返回值；done 代表迭代器是否结束迭代。
2. Generator 返回的迭代器对象有一个内部属性 GeneratorStatus，初始值为 suspended，而在 done 属性为 true 后会被置为 closed。
3. 用 for...of 遍历迭代器对象会忽略最后 return 的返回值，或是说成 done 为 true 的对象的 value 值。
   Generate 有三种方法：next()、throw() 和 return()。在我看来，后两种方法效果类似。
   首先，next() 方法让 Generate 状态机的状态向前移动一步，可以传入一个参数，相当于上一个状态返回的结果，而这可以作为下一个状态的输入值。next() 的返回值是一个对象。

```js
function* gen() {
  var y = yield 1
  console.log(y)
}
var g = gen()
g.next()

// 如果第二次不传参数
g.next() //undefined

// 如果第二次传入参数
g.next('我是 y') // 我是 y
```

这是因为，next 方法执行后，遇到 yield 关键字，会执行它后面的表达式 `1`。而 `var y = ...` 赋值语句并不会执行，而是等到下次调用时在赋值。但是等到下一次调用 next 时，因为不传参数就是传入 undefined，相当于 `var y = undefined`，所以打印出来的就是 undefined。如果我们传入一个字符串 "我是 y"，那么相当于 `var y = "我是 y";`，也就打印出响应值。
我感觉 Generator 每一次状态转换都是对一个执行过程的分段与冻结，第一次调用 next 时在 `var y = ...` 处冻结，将 `yield 1` 的状态封存，1 这个返回值由于没有变量接收所以无法引用。而第二次调用 next 时，从冻结处开始运行，原本 `var y = ...` 希望右边是 1，但是因为这是上一个状态的事，所以在这个状态无法获取到。因此，需要 next 传入这个值。
可以理解成 next 是上一个状态与下一个状态的邮差，其参数是上一个状态要发送给下一个状态的邮件，大概就是这样。

接下来是 throw()，调用 throw 方法后，会手动抛出一个错误，同时迭代器 g 的 GeneratorStatus 会被修改为 closed。

```js
function* gen() {
  console.log('start')
  yield 1
  console.log('end')
  yield 2
}

const g = gen()
g.next() //{value:1,done:false}
console.log(g) //{[[GeneratorStatus]]: "suspended"}
g.throw(new Error('出错了')) //Uncaught Error 出错了
console.log(g) // 这一步没有打印，实际上应该是 {[[GeneratorStatus]]: "closed"}
```

之所以最后一步执行，是因为上一步抛出错误，且没有 catch 语句捕获错误，所以程序中断。如果我们在 Generator 函数中加入 try...catch 语句：

```js
function* gen() {
  console.log('start')
  yield 1
  console.log('second')
  try {
    yield 2
  } catch (e) {
    console.log('catch')
  }
  yield 3
  console.log('end')
}

const g = gen()
g.next()
//start
//{value:1}
g.next()
//second
//{value:2}
g.throw(new Error('hi'))
//catch
//{value:3}
console.log(g)
//{[[GeneratorStatus]]: "suspended"}
g.next()
//end
//{value:undefined,done:true}
```

当 g.throw() 被调用时，刚好是 try 块里的 yield 语句，这里抛出的错误 `new Error("hi")` 被 catch 捕获，因此程序没有中断，并且继续向下执行到 `yield 3`，返回 3，同时也没有设置 `{[[GeneratorStatus]]: "closed"}`。
看上去 throw 方法会在 Generator 下一个状态的开始处抛出一个错误，如果被捕获则继续执行，否则报错终止运行。

最后，return() 方法是让 Generator 函数强制执行 return，也就是相当于将 yield 替换成 return 关键字。当然，与 return 关键字的作用一样，return() 方法会将迭代器的 GeneratorStatus 设置为 closed。

```
function* gen(){
    yield console.log("start");
    yield console.log("second");
    yield console.log("end");
}
const g = gen();
g.next();
//start
//{done:false}
g.return("hi");
//{value:"hi",done:true}
console.log(g);
//{[GeneratorStatus]:"closed"}
g.next();
//{value: undefined, done: true}
```

这里的 return() 方法，会将 Generator 函数的后续状态全部抹除，跳到一个全新的终态。这也能解释为什么 second、end 字符串没有打印出来，而且迭代器的状态被修改为 closed。

最后的最后，如果要将一个 Generator 插入另一个 Generator，需要使用 yield \* 表达式。
总结一下，我认为理解 Generator 需要理解 DFA 有穷自动机的概念，这样有很多特性就能想清楚了。

## 十五、Iterator

一个对象具有 Iterator 接口有以下几个条件：

1. 有一个 [Symbol.iterator] 属性，该属性值为一个函数；
2. 该函数返回值是一个对象，该对象具有 next 属性，值为一个函数；
3. 该 next 函数有一个返回值，形式为 {value:...,next:...}。

或是使用 Array.from 将类数组对象转成数组。又或是将一个 Generator 函数赋值给 [Symbol.iterator]。
