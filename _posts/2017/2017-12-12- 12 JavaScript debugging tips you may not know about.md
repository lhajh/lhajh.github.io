---
layout: post
title: 你可能不知道的 12 个 JavaScript 调试技巧
categories: [JS, Google]
description: 你可能不知道的 12 个 JavaScript 调试技巧
keywords: JavaScript, js, 调试技巧
---

了解你的工具可以极大的帮助你完成任务。尽管 JavaScript 的调试非常麻烦，但在掌握了技巧 (tricks) 的情况下，你依然可以用尽量少的的时间解决这些错误 (errors) 和问题 (bugs) 。

我会列出 12 个你可能不知道的调试技巧, 但是一旦知道了，你就会迫不及待的想在下次需要调试 JavaScript 代码的时候使用它们！

虽然许多技巧也可以用在别的检查工具上，但大部分的技巧是用在 Chrome Inspector 和 Firefox 上的。

## debugger;

`debugger` 是 `console.log` 之外我最喜欢的调试工具，简单暴力。只要把它写到代码里，Chrome 调试模式运行的时候就会自动停在那。你甚至可以用条件语句把它包裹起来，这样就可以在需要的时候才执行它。

```js
if (thisThing) {
  debugger;
}
```

## console

### 把 objects 输出成表格

有时候你可能有一堆对象需要查看。你可以用 `console.log` 把每一个对象都输出出来，你也可以用 `console.table` 语句把它们直接输出为一个表格！

```js
var animals = [
  {animal: 'Horse', name: 'Henry', age: 43},
  {animal: 'Dog', name: 'Fred', age: 13},
  {animal: 'Cat', name: 'Frodo', age: 18}
];
console.table(animals);
```

输出结果：

![](/assets/images/posts/js/3fdR5.png)

### 用 console.time() 和 console.timeEnd() 测试循环耗时

当你想知道某些代码的执行时间的时候这个工具将会非常有用，特别是当你定位很耗时的循环的时候。你甚至可以通过标签来设置多个 timer 。demo 如下:

```js
console.time('Timer1');

var items = [];

for(var i = 0; i < 100000; i++){
  items.push({index: i});
}

console.timeEnd('Timer1');
```

运行结果：

![](/assets/images/posts/js/20171118091302.png)

### 获取函数的堆栈轨迹信息

你可能知道 JavaScript 框架会产生很多的代码 -- 迅速的。

它创建视图触发事件而且你最终会想知道函数调用是怎么发生的。

因为 JavaScript 不是一个很结构化的语言，有时候很难完整的了解到底发生了什么以及什么时候发生的。 这个时候就轮到 `console.trace`（在终端的话就只有 trace ）出场来调试 JavaScript 了 。

假设你现在想看 car 实例在调用 funcZ 函数的完整堆栈轨迹信息:

```js
var car;
var func1 = function() {
	func2();
}

var func2 = function() {
	func4();
}
var func3 = function() {
}

var func4 = function() {
	car = new Car();
	car.funcX();
}
var Car = function() {
	this.brand = 'volvo';
	this.color = 'red';
	this.funcX = function() {
		this.funcY();
	}

	this.funcY = function() {
		this.funcZ();
	}

	this.funcZ = function() {
		console.trace('trace car')
	}
}
func1();
```

`console.trace` 会输出：

![](/assets/images/posts/js/20171118093054.png)

你可以看到 `func1` 调用了 `func2`, `func2` 又调用了 `func4`。`func4` 创建了 `Car` 的实例，然后调用了方法 `car.funcX`，等等。

尽管你感觉你对自己的脚本代码非常了解，这种分析依然是有用的。 比如你想优化你的代码。 获取到堆栈轨迹信息和一个所有相关函数的列表。每一行都是可点击的，你可以在他们中间前后穿梭。 这感觉就像特地为你准备的菜单。

### 在复杂的调试过程中寻找重点

在更复杂的调试中，我们有时需要输出很多行。你可以做的事情就是保持良好的输出结构，使用更多控制台函数，例如 `console.log`，`console.debug`，`console.warn`，`console.info`，`console.error` 等等。然后，你可以在控制台中快速浏览。但有时候，某些 JavaScrip 调试信息并不是你需要的。现在，可以自己美化调试信息了。在调试 JavaScript 时，可以使用 CSS 并自定义控制台信息.

例如：

在 `console.log()` 中， 可以用 `%s` 设置字符串，`%i` 或 `%d` 设置数字，`%f` 设置浮点数，`%o` 设置对象，`%c` 设置自定义样式等等，还有很多更好的 `console.log()` 使用方法。如果使用的是单页应用框架，可以为视图（view）消息创建一个样式，为模型（models），集合（collections），控制器（controllers）等创建另一个样式。也许还可以像 wlog，clog 和 mlog 一样发挥你的想象力！

```js
console.todo = function(msg) {
	console.log('%c%s%s%s', 'color: yellow; background-color: black;', '–', msg, '–');
}

console.important = function(msg) {
	console.log('%c%s%s%s', 'color: brown; font-weight: bold; text-decoration: underline;', '–', msg, '–');
}

console.todo('This is something that' s need to be fixed');
console.important('This is an important message');
```

输出：

![](/assets/images/posts/js/20171118100604.png)

### 信息分组

`console.group` 可以将数据分组，来更好显示数据

```js
console.group('第一组信息');
  console.log('第一组第一条: 我的名字：lhajh');
  console.log('第一组第二条: 我的博客：lhajh.github.io');
console.groupEnd();

console.group('第二组信息');
  console.log('第二组第一条: 欢迎你加入');
  console.log('第二组第二条: just enjoy yourself');
console.groupEnd();
```

输出：

![](/assets/images/posts/js/20171126111228.png)

### 查看对象的信息

`console.dir()` 可以显示一个对象所有的属性和方法。

```js
var info = {
  name: 'lhajh',
  blog: 'lhajh.github.io',
  sayName: function () {
    console.log('my name is lhajh')
  }
};
console.dir(info);
```

输出：

![](/assets/images/posts/js/20171126114018.png)

![](/assets/images/posts/js/20171126114837.png)

![](/assets/images/posts/js/20171126115144.png)

### 显示某个节点的内容

`console.dirxml()` 用来显示网页的某个节点（node）所包含的 html/xml 代码。

![](/assets/images/posts/js/20171126114345.png)

![](/assets/images/posts/js/20171126114631.png)

![](/assets/images/posts/js/20171126114736.png)

三者区别：如果输出的是一个对象，基本没区别；但如果是一个节点，dir 仍输出节点的属性和方法，而 log 和 dirxml 则输出节点

### 条件输出

当你想代码满足某些条件时才输出信息到控制台，那么你大可不必写 if 或者三元表达式来达到目的，`cosole.assert` 便是这样场景下一种很好的工具，它会先对传入的表达式进行断言，只有表达式为假时才输出相应信息到控制台。

![](/assets/images/posts/js/20150113142705_397.jpg)

### 统计函数执行次数

除了条件输出的场景，还有常见的场景是计数。

当你想统计某段代码执行了多少次时也大可不必自己去写相关逻辑，内置的 `console.count` 可以很好地胜任这样的任务。

![](/assets/images/posts/js/20150113142706_297.jpg)

### [QQ 空间自动删除所有说说](http://www.meiriyixue.cn/post/968.html)

这算一个敲门砖吧, 感觉只要是网页重复性的操作, 都可以在这里完成

## 试遍所有的尺寸

虽然把各种各样的手机都摆在桌子上看起来很酷，但这却很不现实。为什么不选择直接调整界面大小呢? Chrome 浏览器提供了你所需要的一切。 进入检查面板点击 ` 切换设备模式 ` 按钮，这样你就可以调整视窗的大小了！

![](/assets/images/posts/js/2896879.png)

## $

在 Chrome 的控制台里，`$` 用处还真是蛮多且方便的。

### 返回最近一次表达式执行结果

`$_` 命令返回最近一次表达式执行的结果，功能跟按向上的方向键再回车是一样的，但它可以做为一个变量使用在你接下来的表达式中：

![](/assets/images/posts/js/20150113142706_234.jpg)

### 快速定位 DOM 元素

在元素面板上标记一个 DOM 元素并在 console 中使用它。Chrome Inspector 的历史记录保存最近选取的五个元素，最后被标记的元素记为 `$0`，倒数第二个被标记的记为 `$1`，以此类推。

什么意思？在页面右击选择审查元素，然后在弹出来的 DOM 节点树上面随便点选，这些被点过的节点会被记录下来，而 `$0` 会返回最近一次点选的 DOM 节点，以此类推，`$1` 返回的是上上次点选的 DOM 节点，最多保存了 5 个，如果不够 5 个，则返回 `undefined`。

![](/assets/images/posts/js/20150113142706_291.gif)

如果你像下面那样把元素按顺序标记为'item-4', 'item-3', 'item-2', 'item-1', 'item-0' ，你就可以在 console 中获取到 DOM 节点:

![](/assets/images/posts/js/46782fg.png)

### jQuery

另外值得一赞的是，Chrome 控制台中原生支持类 jQuery 的选择器，也就是说你可以用 `$` 加上熟悉的 css 选择器来选择 DOM 节点，多么滴熟悉。

![](/assets/images/posts/js/20150113142706_364.jpg)

`$(selector)` 返回的是满足选择条件的首个 DOM 元素。剥去她伪善的外衣，其实 `$(selector)` 是原生 JavaScript `document.querySelector()` 的封装。同时另一个命令 `$$(selector)` 返回的是所有满足选择条件的元素的一个集合，是对 `document.querySelectorAll()` 的封装。

![](/assets/images/posts/js/20150113142707_140.jpg)

## 格式化代码使调试 JavaScript 变得容易

有时候你发现产品有一个问题，而 source map 并没有部署到服务器。不要害怕。Chrome 可以格式化 JavaScript 文件，使之易读。格式化出来的代码在可读性上可能不如源代码 —— 但至少你可以观察到发生的错误。点击源代码查看器下面的美化代码按钮 {} 即可。

格式化之前：

![](/assets/images/posts/js/20171118094138.png)

格式化之后：

![](/assets/images/posts/js/20171118094230.png)

## 快速找到调试函数

来看看怎么在函数中设置断点。

通常情况下有两种方法：

1.  在查看器中找到某行代码并在此添加断点
2.  在脚本中添加 debugger

这两种方法都必须在文件中找到需要调试的那一行。

使用控制台是不太常见的方法。在控制台中使用 debug(funcName)，代码会在进入这里指定的函数时停止。

这个操作很快，但它不能用于局部函数或匿名函数。不过如果不是这两种情况下，这可能是调试函数最快的方法。（注意：这里并不是在调用 console.debug 函数）。

```js
var func1 = function() {
	func2();
};

var Car = function() {
	this.funcX = function() {
		this.funcY();
	}

	this.funcY = function() {
		this.funcZ();
	}
}

var car = new Car();
```

在控制台中输入 debug(car.funcY)，脚本会在调试模式下，进入 car.funcY 的时候停止运行：

![](/assets/images/posts/js/143724.png)

而 undebug 则是解除该断点。

## keys & values

这是一对基友。前者返回传入对象所有属性名组成的数组，后者返回所有属性值组成的数组。具体请看下面的例子：

![](/assets/images/posts/js/20171215-170957@2x.png)

[5 分钟彻底理解 Object.keys](https://github.com/berwin/Blog/issues/24)

## monitor & unmonitor

monitor(function)，它接收一个函数名作为参数，比如 `function a` , 每次 a 被执行了，都会在控制台输出一条信息，里面包含了函数的名称 a 及执行时所传入的参数。而 unmonitor(function) 便是用来停止这一监听。

![](/assets/images/posts/js/20171215-171835@2x.png)

```js
var func1 = function(x, y, z) {
  // ....
};
```

然后输出：

![](/assets/images/posts/js/20171118101107.png)

这是查看将哪些参数传递到函数的一种很好的方法。但我必须说，如果控制台能够告诉我们需要多少参数，那就好了。在上面的例子中，函数 1 期望 3 个参数，但是只有 2 个参数被传入。如果代码没有在代码中处理，它可能会导致一个 bug 。

这里有一个特殊的术语：Arity。Arity 指的是一个函数声明的形参数量。你可能需要在程序运行时获取函数的 Arity，使用函数的 length 属性即可。

```js
function foo(x,y,z) {
	// ..
}
foo.length;				// 3
```

## 屏蔽不相关代码

如今，经常在应用中引入多个库或框架。其中大多数都经过良好的测试且相对没有缺陷。但是，调试器仍然会进入与此调试任务无关的文件。解决方案是将不需要调试的脚本屏蔽掉。当然这也可以包括你自己的脚本。 [点此阅读更多关于调试不相关代码。](http://raygun.com/blog/javascript-debugging-with-black-box/)

![](/assets/images/posts/js/183650_yO4h_2896879.png)

## 在控制台中快速访问元素

在控制台中执行 querySelector 一种更快的方法是使用美元符。$('css-selector') 将会返回第一个匹配的 CSS 选择器。$$('css-selector') 将会返回所有。如果你使用一个元素超过一次，它就值会被作为一个变量。

![](/assets/images/posts/js/143940_Y4fd_2896879.png)

## Postman 很棒（但 Firefox 更快）

很多开发人员都使用 Postman 来处理 Ajax 请求。Postman 真不错，但每次都需要打开新的浏览器窗口，新写一个请求对象来测试。这确实有点儿烦人。

有时候直接在浏览器中使用会更容易。

这样的话，如果你想请求一个通过密码保证安全的页面时，就不再需要担心验证 Cookie 的问题。这就是 Firefox 中编辑并重新发送请求的方式。

打开探查器并进入网络页面，右键单击要处理的请求，选择编辑并重新发送。现在你想怎么改就怎么改。可以修改头信息，也可以编辑参数，然后点击重新发送即可。

现在我发送了两次同一个请求，但使用了不同的参数：

![](/assets/images/posts/js/144016_m5gf_2896879.png)

## 节点变化时中断

DOM 是个有趣的东西。有时候它发生了变化，但你却并不知道为什么会这样。不过，如果你需要调试 JavaScript，Chrome 可以在 DOM 元素发生变化的时候暂停处理。你甚至可以监控它的属性。在 Chrome 探查器上，右键点击某个元素，并选择中断（Break on）选项来使用：

![](/assets/images/posts/js/144035_hst5_2896879.png)

## 参考资料

- [The 14 JavaScript debugging tips you probably didn't know](https://raygun.com/javascript-debugging-tips)
- [[译]14 个你可能不知道的 JavaScript 调试技巧](https://segmentfault.com/a/1190000011857058)
- [Chrome 控制台不完全指南](http://www.open-open.com/lib/view/open1421130008671.html)
