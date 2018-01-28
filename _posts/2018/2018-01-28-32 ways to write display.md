---
layout: post
title: display 的 32 种写法
categories: [CSS]
description: display 的 32 种写法
keywords: display
---

你知道`『回』`字有四种写法，但你知道 display 有 32 种写法吗？今天我们一一道来，让你一次性完全掌握 display，从此再也不用对它发愁。

从大的分类来讲，display 的 32 种写法可以分为 6 个大类，再加上 1 个全局类，一共是 7 大类：

- [外部值](#外部值)
- [内部值](#内部值)
- [列表值](#列表值)
- [属性值](#属性值)
- [显示值](#显示值)
- [混合值](#混合值)
- [全局值](#全局值)

## 外部值

所谓外部值，就是说这些值只会直接影响一个元素的外部表现，而不影响元素里面的儿子级孙子级元素的表现。

### display: block;

这个值大家不陌生，我们最熟悉的 `<div>` 缺省就是这个值，最基本的块级元素，属于 css 入门初学者都知道的概念，只要是容器类型的元素基本都是这个值。除 `<div>` 之外，还有 `<h1>` 到 `<h6>`，`<p>`，`<form>`，`<header>`，`<footer>`，`<section>`，`<article>` 天生都是这个值。

### display: inline;

这个值大家也不陌生，行内元素嘛，只要是个行内元素都是这个值，最典型的是 `<span>`，还有 `<a>`，`<img>`，以及古代html语言当中的 `<b>`，`<i>` 都属于这一类型。

### display: run-in;

这个值有点奇怪，通常没人用它，但你可以知道它。因为除了 IE 和 Opera 支持它以外，其他所有主流浏览器包括 Chrome, Safari, Firefox 全都对它置若罔闻。这东西说白了也没什么神秘，它的意思就是说如果我们命令一个元素 `run-in`，中文意思就是`『闯入』`！那么这个元素就直接闯入下一行。比如说这样：

![](/assets/images/posts/css/3151940790-5a5a0dfc0948d_articlex.png)

写起来大概就是这样：
```
<div class="a">aaa</div>
<div class="b">bbb</div>
.a {
  font-size: 36px;
  display: run-in;
}
```
这有什么用呢？我们拿 `span` 设置 `font-size` 一样可以实现这个效果，就让 IE 自己跟自己玩去吧！说实话，在人力资源如此宝贵的今天，IE 的产品经理不知脑子是不是进水了，不派工程师去实现那么多比这重要的多得多的特性，却花时间做这么个没用的玩意儿，难道工程师的时间不是金钱吗？难怪市场占有率连年下滑。

## 内部值

谈完了外部值，我们来看看内部值。这一组值比较有意思了，在 css3 如火如荼的今天，你要玩不转这些值，怕是哪儿也找不到工作的。内部值主要是用来管束自己下属的儿子级元素的排布的，规定它们或者排成 S 形，或者排成 B 形这样的。

### display: flow;

含义不清，实验室阶段产品，Chrome 不支持。如果还不够说服你暂时不要碰它的话，试着理解以下英文原文：

> If its outer display type is inline or run-in, and it is participating in a block or inline formatting context, then it generates an inline box. Otherwise it generates a block container box.

### display: flow-root;

不同于刚才谈到的 `flow`，现在用` flow-root` 的渐渐多起来了，因为它可以撑起被你 `float` 掉的块级元素的高度。外容器本来是有高度的，就像这样：

![](/assets/images/posts/css/3516068194-5a5a15f72191f_articlex.png)

```
<div class="container container1">
  <div class="item"></div>
  Example one
</div>
.container {
  border: 2px solid #3bc9db;
  border-radius: 5px;
  background-color: #e3fafc;
  width: 400px;
  padding: 5px;
}
.item {
  height: 100px;
  width: 100px;
  background-color: #1098ad;
  border: 1px solid #0b7285;
  border-radius: 5px;
}
```
结果因为你想让那一行字上去，于是你给 `.item` 加了一个 `float: left;` 结果就成这样了，外容器高度掉了，这不是很多人常犯的错误吗？

![](/assets/images/posts/css/2178139767-5a5a166b16c9b_articlex.png)

现在我们给 `.container` 加上 `display: flow-root;` 再看一下：

![](/assets/images/posts/css/4119238615-5a5a169b489ce_articlex.png)

喏，外容器高度又回来了，这效果是不是杠杠的？

那位同学说，我们用 `clear: both;` 不是一样可以达到这效果吗？
```
.container::after {
  content: '';
  clear: both;
  display: table;
}
```
小明，请你出去！我们在讲 `display: flow-root;`，不是在讲` clear: both;`！

### display: table;

这一个属性，以及下面的另外 8 个与 `table` 相关的属性，都是用来控制如何把 `div` 显示成 `table` 样式的，因为我们不喜欢 `<table>` 这个标签嘛，所以我们想把所有的 `<table>` 标签都换成 `<div>` 标签。`<div>` 有什么好？无非就是能自动换行而已，但其实你完全可以做一个 `<table><tr><td>` 标签，把它全都替换成 `display: block;` 也可以自动折行，只不过略微麻烦而已。

关于 `display: table;` 的详细用法，大家可以参考[这篇文章](http://www.cnblogs.com/haoqipeng/p/html-display-table.html)，这里就不细说了。

### display: flex;

敲黑板，划重点！作为新一代的前端工程师，这个属性你必须烂熟于胸衣中，哦，错了，是胸中。`display: flex;` 以及与它相关联的一系列属性：`flex-direction`, `flex-wrap`, `flex-flow`, `justify-content`, `align-items`, `align-content`，并且包括所有这些属性的取值，都是你需要反复研磨的。2009 年诞生的这个属性可以说是不亚于 css 界一场蒸汽机诞生一样的工业革命，它的诞生标志着马车一样的 `float` 被彻底抛进历史的垃圾堆。

关于它的详情，会中文的可以参考阮一峰的[这篇文章](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)，但我认为，格式编排的更好还是 csstrick 上的[这篇文章](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)。没有一张图能完整地展现 `flex` 的神韵，就放这张我比较喜欢的图片吧：

![](/assets/images/posts/css/355930984-5848bb40460ed_articlex.png)

### display: grid;

会 `flex` 很吊吗？会 `grid` 更吊哦！也许这就是下次前端面试的重点哦！

![](/assets/images/posts/css/2523208566-5a5ac74bd288c_articlex.png)

`grid` 布局，中文翻译为网格布局。学习 `grid` 布局有两个重点：一个重点是 `grid` 布局引入了一个全新的单位：fr，它是fraction（分数）的缩写，所以从此以后，你的兵器库里除了 px, em, rem, 百分比这些常见兵器以及 vw, vh 这些新式武器之外，又多了一样旁门暗器 fr，要想用好 `grid`，必须充分掌握 fr。另一个重点是斜杠操作符，这可不是分数哦。它表示的是起始位置和结束位置。比如说 3 / 4，这可不是四分之三的意思，这是指一个元素从第 3 行开始，到第 4 行结束，但又不包括第 4行。

同样，与 `grid` 相关联的也有一大堆旁门属性，是在学习 `display: grid;` 的同时必须掌握的。包括 `grid`, `grid-column-start`, `grid-column-end`, `grid-row-start`, `grid-row-end`, `grid-template`, `grid-template-columns`, `grid-template-rows`, `grid-template-areas`, `grid-gap`, `grid-column-gap`, `grid-row-gap`, `grid-auto-columns`,` grid-auto-rows`, `grid-auto-flow`, `grid-column`, `grid-row`。不能详述，关于这个写起来又是一大篇文章。详情还是参考 csstrick 上[这篇文章](https://css-tricks.com/snippets/css/complete-guide-grid/)，讲得非常细致非常清楚。

### display: ruby;

`ruby` 这个取值对于我们亚洲人来说其实是非常有用的一个东西，但是目前除了 Firefox `以外其它浏览器对它的支持都不太好。简而言之，display: ruby;` 的作用就是可以做出下面这样的东西：

![](/assets/images/posts/css/214122309-5a5ac84bde635_articlex.png)

很好的东西，对吧？如果可以用的话，对我国的小学教育可以有极大的促进。但可惜我们现在暂时还用不了。

`ruby` 这个词在英语里的意思是红宝石，但在日语里是 `ルビ`，翻译成中文是旁注标记的意思，我们中文的旁注标记就是汉语拼音。可以想见，这个标准的制定者肯定是日本人，如果是我们中国人的话，那这个标签就不是 `ruby`，而是 `pinyin` 了。还有一个 `ruby` 语言，发明者也是一个日本人，和 `html` 里这个 `ruby` 是两码事，不要搞混了。

`ruby` 的语法大致如下：

![](/assets/images/posts/css/2284248556-5a5ac8fc9979e_articlex.png)

### display: subgrid;

2015 年 8 月 6 日，W3C 的级联样式单（CSS）工作组（Cascading Style Sheets Working Group）发布了 CSS 网格布局模块第一级（CSS Grid Layout Module Level 1）的工作草案。在这个草案里规定了上一节我们讲到的 `display: grid;` 的方案。而` display: subgrid;` 是属于 2017 年 11 月 9 日发布的非正式的 [CSS 网格布局模块第二级](https://drafts.csswg.org/css-grid-2/)的内容。所以这是一个非常新的草案，并且围绕它的争议从来也没有断过。

subgrid 总的思想是说大网格里还可以套小网格，互相不影响。但如果 grid 里可以再套 subgrid 的话，那我 subgrid 里还想再套 subgrid 怎么办？subsubgrid 吗？况且，到底是 grid: subgrid; 还是 display: subgrid; 这个也没有达成共识，关于此一系列的争议，感兴趣的同学可以看看[这篇文章](https://www.w3cplus.com/css3/why-display-contents-is-not-css-grid-layout-subgrid.html)，英语好的可以看[这篇](https://blogs.igalia.com/mrego/2016/02/12/subgrids-thinking-out-loud/)。

## 列表值

### display: list-item;

`display: list-item;` 和 `display: table;` 一样，也是一帮痛恨各种 `html` 标签，而希望只使用 `<div>` 来写遍一切 `html` 的家伙搞出来的鬼东西，实际使用极少，效果就是这样：

![](/assets/images/posts/css/1935635266-5a5aeb6132487_articlex.png)

看，你用 `<ul><li>` 能实现的效果，他可以用 `<div>`实现出来，就是这个作用。