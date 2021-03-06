---
layout: post
title: 用 CSS 让你的文字更有文艺范
categories: CSS
tags: [CSS]
description: 用 CSS 让你的文字更有文艺范
keywords: css 文字
---

透明文字，模糊文字，镂空文字，渐变文字，图片背景文字，用 CSS 让你的文字也有 freestyle～

## 前言

我们做页面涉及字体的时候，最多就是换个 color 换个 font-family，总是觉得没体现出你内心的文艺范，这时是不是抱怨 CSS 并没有给文字设置什么样式，抱怨是没用哒，我们自己动手，看看能不能“创造”出一些 CSS 字体样式呢～

## 透明文字

1. 用 rgba 调整透明度
```
div{
    background-color: pink;
    color: rgba(0, 0, 0, 0.1);
}
```

![](/assets/images/posts/css/06B76BD6-3B89-4D38-8FB9-D8C10EA2D031.png)

2. 用 opacity 调整遮罩
```
div{
    background-color: pink;
    opacity: 0.4;
}
```

![](/assets/images/posts/css/4ADBA9CC-0D62-4DD7-8B18-6C6DAF5C7708.png)

两种区别是用 rgba 只是对文字有透明度，而 opacity 对整个 div 都有遮罩影响，对比其两个 div 的背景颜色即可发现区别。

## 模糊文字

在 css 中并没有指定文字模糊的样式，但是可以用 `text-shadow` 和 `-webkit-text-fill-color` 组合，得出模糊文字，即用 `text-shadow` 制造底层模糊文字，用 `-webkit-text-fill-color` 填充颜色为透明，例如：
```
div{
    text-shadow: 0 0 5px red;
    -webkit-text-fill-color: transparent;
}
```
这里的 `text-shadow` 将 x，y 偏移量设置为 0，也就是不偏移，设置为 5px 的模糊程度，重点是下面的 `fill-color` 设置为 transparent 透明，这样就把底层的模糊字体体现出来，效果如下：

![](/assets/images/posts/css/B6BC0F0C-C50B-4EA1-B462-F634F6391365.png)

## 镂空文字

这里我们用到 `-webkit-text-stroke` 来给文字外围描边，然后再把文字的填充颜色设置为透明，这样就能只显示出文字的外围的描边，也就是我们所说的镂空文字。
```
div{
    font-size: 40px;
    -webkit-text-stroke: 1px red;
    -webkit-text-fill-color: transparent;
}
```

![](/assets/images/posts/css/0764C528-A3A2-4025-999C-3A2F8C2E91E6.png)

## 渐变文字

这是个比较有趣的组合方式，CSS 中并没有给我们提供文字的渐变，但是我们的 `background` 可以做到渐变颜色，那怎么让文字渐变呢，我们上面的一个属性是让文字透明，这样文字底下的东西我们就可以看的到，那我们试想，如果让文字下面的渐变背景颜色显示出来，这样不就是相当于文字有了背景颜色嘛，我们先试试：
```
div{
    font-size: 40px;
    background: linear-gradient(to bottom,white,black);
    -webkit-text-fill-color: transparent;
}
```
然而效果却是：

![](/assets/images/posts/css/0A5C29AF-C9BA-4826-8084-3B98AFBF78BA.png)

这里虽然背景有了渐变，但文字直接成了透明，那么我们怎么做到文字外围的背景去除，文字中的背景显示出来，我们知道 `background-clip` 是用来设置背景图片在哪个区域显示，如果它能让文字中的底下显示，那我们岂不是就能做到我们希望的效果～，没错 `-webkit-background-clip:text` 的效果就是指定背景只在文字底下显示，我们再试试：
```
div{
    font-size: 40px;
    background: linear-gradient(to bottom,white,black);
    -webkit-text-fill-color: transparent;
    -webkit-background-clip: text;
}
```
文字渐变效果：

![](/assets/images/posts/css/B5FE66FB-CF92-49EF-8ACD-B194E667D790.png)

## 图片背景文字

我们经常在网上看到类似于这样酷炫的图片：

![](/assets/images/posts/css/2430F35B-80B8-4A42-AEDF-D554399E7AD7.png)

作为一个爱折腾的前端狗，做页面的时候能用 CSS 达到的效果绝对不求美工给我们高清大图贴页面，自己动手～

我们都知道 `background-clip` 是用来设置背景图片的显示位置，如果要用到只在图片上显示背景位置，我们在这里用到了上面说的 `-webkit-background-clip: text`,这个属性能让背景只在文字底下显示，如果文字设置为透明的，那我们就能透过文字，透过文字看到背景图片，这是一个能媲美 PS 效果的利器属性，代码：
```
div{
	/*背景样式*/
	height: 300px;
	width: 500px;
	background-size: contain;
	background-repeat: no-repeat;
	background-image: url(bg.jpg);
	/*文字样式*/
	font-size: 70px;
	font-weight: bold;
	text-align: center;
	line-height: 300px;
	/*图片文字样式*/
	-webkit-text-fill-color: transparent;
	-webkit-background-clip: text;
}
```

![](/assets/images/posts/css/62713D66-A2D6-4E52-BF6C-A61C8936CFD8.png)

## 总结

单个 CSS 看起来似乎只是个简单的样式功能，但是用多个 CSS 样式组合，能给我们一些惊喜的效果，这篇短文只是简单的列出几个效果，还有更多酷炫的效果等着小伙伴们去挖掘～
