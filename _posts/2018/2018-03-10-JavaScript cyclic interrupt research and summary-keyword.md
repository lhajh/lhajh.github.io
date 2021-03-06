---
layout: post
title: JavaScript 循环中断研究与总结-关键字篇
categories: [JS]
description: JavaScript 循环中断研究与总结-关键字篇
keywords: JavaScript, 循环
---

面向对象编程语法中我们会碰到 `break`, `continue`, `return` 这三个常用的关键字，那么关于这三个关键字的使用具体的操作是什么呢？我们在使用这三关键字的时候需要注意和需要理解的规则是什么呢？

具体在循环或函数中的用法请见[下篇](https://lhajh.github.io/js/2018/03/10/JavaScript-cyclic-interrupt-research-and-summary-usage.html)

## [break](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/break)

break 语句中止当前循环，[switch](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/switch) 语句或 [label](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/label) 语句，并把程序控制流转到紧接着被中止语句后面的语句。

### 语法

```js
break [label];
```

      label

      可选。与语句标签相关联的标识符。如果 break 语句不在一个循环或 switch 语句中，则该项是必须的。

### 描述

`break` 语句包含一个可选的标签，可允许程序摆脱一个被标记的语句。`break` 语句需要内嵌在引用的标签中。被标记的语句可以是任何 [块](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/block) 语句；不一定是循环语句。

### 示例

下面的函数里有个 `break` 语句，当 i 为 3 时，会中止 `while` 循环，然后返回 3 \* x 的值。

```js
function testBreak(x) {
  var i = 0

  while (i < 6) {
    if (i == 3) {
      break
    }
    i += 1
  }

  return i * x
}
```

下面的代码中一起使用 `break` 语句和被标记的块语句。一个 `break` 语句必须内嵌在它引用的标记中。注意，inner_block 内嵌在 outer_block 中。

```js
outer_block: {
  inner_block: {
    console.log('1')
    break outer_block // breaks out of both inner_block and outer_block
    console.log(':-(') // skipped
  }
  console.log('2') // skipped
}
```

下面的代码同样使用了 `break` 语句和被标记的块语句，但是产生了一个语法错误，因为它的 `break` 语句在 block_1 中，但是引用了 block_2。`break` 语句必须内嵌在它引用的标签中。

```js
block_1:{
  console.log ('1');
  break block_2; // Uncaught SyntaxError: Undefined label 'block_2'
}

block_2:{
  console.log ('2');
}
```

## [continue](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/continue)

### 概述

`continue` 语句结束当前（或标签）的循环语句的本次迭代，并继续执行循环的下一次迭代。

### 语法

```js
continue [label];
```

      label
      标识标号关联的语句

### 描述

与 `break` 语句的区别在于， `continue` 并不会终止循环的迭代，而是：

1. 在 [while](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/while) 循环中，控制流跳转回条件判断；
2. 在 [for](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for) 循环中，控制流跳转到更新语句。

`continue` 语句可以包含一个可选的标号以控制程序跳转到指定循环的下一次迭代，而非当前循环。此时要求 `continue` 语句在对应的循环内部。

### 示例

例子：在 `while` 语句中使用 `continue`

下述例子展示了 `while` 循环中 `continue` 语句的使用。当循环到 i 的值为 3 时，执行 `continue` 。 n 的值在几次迭代后分别为 1, 3, 7 和 12 ．

```js
var i = 0
var n = 0
while (i < 5) {
  i++
  if (i === 3) {
    continue
  }
  n += i
}
```

例子：使用带标号的 `continue`

在下面的例子中，被标记为 checkiandj 的语句包含一个被标记为 checkj 的语句。当遇到 `continue` 语句时，程序回到 checkj 语句的开始继续执行。每次遇到 `continue` 时，再次执行 checkj ，直到条件判断返回 false 。之后完成 checkiandj 语句剩下的部分。

但如果 `continue` 的标号被改为 checkiandj ，那程序将会从 checkiandj 语句的开始继续运行。

参考 label 。

```js
var i = 0,
  j = 8

checkiandj: while (i < 4) {
  console.log('i: ' + i)
  i += 1

  checkj: while (j > 4) {
    console.log('j: ' + j)
    j -= 1
    if (j % 2 == 0) continue checkj
    console.log(j + ' is odd.')
  }
  console.log('i = ' + i)
  console.log('j = ' + j)
}
```

输出：

```js
'i: 0'

// start checkj
'j: 8'
'7 is odd.'
'j: 7'
'j: 6'
'5 is odd.'
'j: 5'
// end checkj

'i = 1'
'j = 4'

'i: 1'
'i = 2'
'j = 4'

'i: 2'
'i = 3'
'j = 4'

'i: 3'
'i = 4'
'j = 4'
```

## [return](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/return)

`return` 语句终止函数的执行，并返回一个指定的值给函数调用者。

### 语法

```js
return [[expression]]
```

      expression
      表达式的值会被返回。如果忽略，则返回 undefined。

### 描述

当在函数体中使用 `return` 语句时，函数将会停止执行。如果指定一个值，则这个值返回给函数调用者。例如，以下函数返回其参数 x 的平方，其中 x 是数字。

```js
function square(x) {
  return x * x
}
var demo = square(3)
// demo will equal 9
```

如果省略该值，则返回 undefined。

下面的 `return` 语句都会终止函数的执行：

- return
- return true
- return false
- return x
- return x + y / 3

**自动插入分号**

[自动插入分号（ASI）](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Lexical_grammar#Automatic_semicolon_insertion)  规则
会影响 `return` 语句。在 `return` 关键字和被返回的表达式之间不允许使用行终止符。

```
return
a + b;
```

根据 ASI，被转换为：

```
return;
a + b;
```

控制台会警告 "unreachable code after return statement"。

从 Gecko 40 (Firefox 40 / Thunderbird 40 / SeaMonkey 2.37)开始，如果在一个 return 语句后发现无法访问的代码，控制台将会显示一个警告。

### 示例

中断一个函数的执行

函数将会在 `return` 语句执行后立即中止。

```js
function counter() {
  for (var count = 1; ; count++) {
    // 无限循环
    console.log(count + 'A') // 执行5次
    if (count === 5) {
      return
    }
    console.log(count + 'B') // 执行4次
  }
  console.log(count + 'C') // 永远不会执行
}

counter()

// Output:
// 1A
// 1B
// 2A
// 2B
// 3A
// 3B
// 4A
// 4B
// 5A
```

返回一个函数

另见关于[闭包](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Closures)的文章。

```js
function magic(x) {
  return function calc(x) {
    return x * 42
  }
}

var answer = magic()
answer(1337) // 56154
```
