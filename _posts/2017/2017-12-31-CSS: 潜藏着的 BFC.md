---
layout: post
title: CSS - 潜藏着的 BFC
categories: [CSS]
description: CSS - 潜藏着的 BFC
keywords: js
---

在写样式时，往往是添加了一个样式，又或者是修改了某个属性，就达到了我们的预期。
而BFC就潜藏在其中，当你修改样式时，一不小心就能触发它而毫无察觉，因此没有意识到BFC的神奇之处。

## 一、什么是 BFC（Block Formatting Context）

写 CSS 样式时，对一个元素设置 css,我们首先要知道这个元素是块级元素还是行内元素，而 BFC 就是用来格式化块级盒子的。

Formatting Context：指页面中一个渲染区域，并且拥有一套渲染规则，它决定了其子元素如何定位，以及与其他元素的相互关系和作用。

BFC 定义：块级格式化上下文，它是指一个独立的块级渲染区域，只有 Block-level Box 参与，该区域拥有一套渲染规则来约束块级盒子的布局，且与区域外部无关。

## 二、BFC 的生成

我们说到 BFC 是一块渲染区域，那么这块渲染区域到底在哪里呢，具体大小又是多少？这些都是由生成 BFC 的元素来决定的。

满足下列 CSS 声明之一的元素便会生成 BFC：

1. 根元素或其它包含它的元素
2. float 的值不为 none；
3. overflow 的值不为 visible；
4. position 的值不为 static；
5. display 的值为 inline-block、table-cell、table-caption；
6. flex boxes (元素的display: flex或inline-flex)；

注：也有人认为 display: table 能生成 BFC，我认为最主要原因是 table 会默认生成一个匿名的 table-cell，正是这个匿名的 table-cell 生成了 BFC。

## 三、BFC 的布局规则

简单归纳如下：
1. 内部的元素会在垂直方向一个接一个地排列，可以理解为是 BFC 中的一个常规流
2. 元素垂直方向的距离由 margin 决定，即属于同一个 BFC 的两个相邻盒子的 margin 可能会发生重叠
3. 每个元素的左外边距与包含块的左边界相接触(从左往右，否则相反)，即使存在浮动也是如此，这说明 BFC 中的子元素不会超出它的包含块
4. BFC 的区域不会与 float 元素区域重叠
5. 计算 BFC 的高度时，浮动子元素也参与计算
6. BFC 就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然

## 四、BFC 的应用

说了这么多，那么我们 BFC 到底有什么用呢？下面我们通过几个实例来解决一些问题：

### 实例1、解决 margin 重叠问题

玩 css 的朋友都知道 margin collapse，也就是相邻的垂直元素同时设置了 margin 后，实际 margin 值会塌陷到其中较大的那个值。

其根本原理就是它们处于同一个 BFC，符合“属于同一个 BFC 的两个相邻元素的 margin 会发生重叠”的规则。

margin重叠现象：
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>margin重叠现象</title>
    <style type="text/css">
        *{margin: 0;padding: 0;}
        .box p {
            margin: 20px 0px;
            background-color: skyblue;
        }
    </style>
</head>
<body>
    <div class="box" >
        <p>Lorem ipsum dolor sit.</p>
        <p>Lorem ipsum dolor sit.</p>
        <p>Lorem ipsum dolor sit.</p>
    </div>
</body>
</html>
```

![](/assets/images/posts/css/670723661-5a1ed95674fbf_articlex.png)

![](/assets/images/posts/css/2674931510-5a1ed9baf2ea4_articlex.png)

![](/assets/images/posts/css/925455653-5a1edab5eaab7_articlex.png)

![](/assets/images/posts/css/1161251807-5a1eda059a1c1_articlex.png)

通过实验结果我们发现，上下 margin 重叠了。

我们可以在其中一个元素外面包裹一层容器，并触发该容器生成一个 BFC。那么两个元素便属于不同的 BFC，就不会发生 margin 重叠了。

我们做如下修改：
```
<div class="box">
    <p>Lorem ipsum dolor sit.</p>
    <div style="overflow:hidden;">
        <p>Lorem ipsum dolor sit.</p>
    </div>
    <p>Lorem ipsum dolor sit.</p>
</div>
```

![](/assets/images/posts/css/928507619-5a1edc377aa86_articlex.png)

![](/assets/images/posts/css/1994306899-5a1edcadc0cf5_articlex.png)

![](/assets/images/posts/css/1587318777-5a1edccaa33ca_articlex.png)

我们使用 `overflow:hidden;` 生成了一个 BFC，成功解决了 margin 重叠问题。

### 实例2、解决浮动问题

我们知道给父元素设置 overflow:hidden 可以清除子元素的浮动，但往往都不知道原理是什么。

其实这就是应用了 BFC 的原理：当在父元素中设置 overflow:hidden 时就会触发 BFC，所以他内部的元素就不会影响外面的布局，BFC 就把浮动的子元素高度当做了自己内部的高度去处理溢出，所以外面看起来是清除了浮动。
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>BFC浮动问题</title>
    <style>
        .one {
            /* 文档流 里面的文字标签将父元素撑起来 */
            background-color: pink;
        }
        .two {
            float: left;
        }
    </style>
</head>
<body>
    <!-- 文档流 从上到下,当遇到float、position：absolute时，会离开文档流 -->
    <div class="one">
        <div class="two">Hello World!</div>
    </div>
    你好世界！
</body>
</html>
```

![](/assets/images/posts/css/2748544559-5a1ee02f6d09d_articlex.png)

![](/assets/images/posts/css/2417430225-5a1ee1655bef9_articlex.png)

我们做如下修改：
```
.one {
    background-color: pink;
    overflow: hidden;
}
```

![](/assets/images/posts/css/470587995-5a1ee085ad25a_articlex.png)

![](/assets/images/posts/css/799728265-5a1ee1498100e_articlex.png)

对比发现，当我们一个元素设置成为 BFC 之后，计算 BFC 元素高度的时候，浮动元素也参与了计算。

### 实例3、解决侵占浮动元素的问题

我们知道浮动元素会脱离文档流，然后浮盖在文档流元素上。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>BFC侵占浮动元素的问题</title>
    <style>
        .box1 {
            float: left;
            width: 100px;
            height: 100px;
            background-color: pink;
        }
        .box2 {
            width: 200px;
            height: 200px;
            background-color: skyblue;
        }
    </style>
</head>
<body>
    <div class="box1">box1</div>
    <div class="box2">box2</div>
</body>
</html>
```

![](/assets/images/posts/css/1820462065-5a1ee4c122838_articlex.png)

当一个元素浮动，另一个元素不浮动时，浮动元素因为脱离文档流就会盖在不浮动的元素上。

我们做如下修改：
```
.box2 {
    width: 200px;
    height: 200px;
    background-color: skyblue;
    overflow: hidden;
}
```
或如下修改：
```
.box2 {
    width: 200px;
    height: 200px;
    background-color: skyblue;
    /* overflow: hidden; */
    float: left;
}
```

![](/assets/images/posts/css/1604946711-5a1ee58d0ed71_articlex.png)

我们为非浮动元素建立 BFC 环境，根据 BFC 的不与 float box 重叠的规则，解决了侵占元素问题。

这一特性，我认为还是很有用的，特别是应用在两栏布局上，对比我们常规为非浮动元素或非定位元素设置 margin 来挤开的方法，其优点在于不需要去知道浮动或定位元素的宽度。

## 总结

以上就是关于 BFC 的一些分析，BFC 是一种概念，是对前端布局技术的一种理论上的总结，掌握它可以让我们在使用 CSS +DIV 进行布局时，知道一些特殊操作以及规避问题的原理。BFC 的概念比较抽象，但通过实例分析，有助于我们对 BFC 的理解。

## 参考资料

- [CSS: 潜藏着的BFC](https://segmentfault.com/a/1190000012221820)
