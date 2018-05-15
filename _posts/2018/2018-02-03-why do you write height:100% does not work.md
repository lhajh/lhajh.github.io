---
layout: post
title: 为什么你写的 height:100% 不起作用
categories: [CSS]
description: 为什么你写的 height:100% 不起作用
keywords: height
---

这个知识不算冷门的，但是用的时候可能还是会有些懵逼，不能生效时搜一搜就能找到答案了，但是你真的懂了吗？为什么想要设置一个全屏元素的时候，高度不受%的控制？

## 1.百分比宽高的设定

按照 w3c 中的 width 和 height 属性，可以明确 % 设定宽高是根据父元素的宽高来的：

- [CSS width 属性](http://www.w3school.com.cn/cssref/pr_dim_width.asp)
- [CSS height 属性](http://www.w3school.com.cn/cssref/pr_dim_height.asp)

![](/assets/images/posts/css/20180203-114028@2x.png)

## 2.width:100%;

我们写下这样一段代码，随意设置一个背景色便于观察元素
```
<body>
  <div style="width:100%;height:100%;background-color:blueviolet;">
    width:100%;height:100%;
  </div>
</body>
//宽100%，我们现在看到的高是属于font-size的，而不是100%；
```
<div style="width:100%;height:100%;background-color:blueviolet;">
  width:100%;height:100%;
</div>


```
<body>
  <div style="width:100%;height:200px;background-color:blueviolet;">
    width:100%;height:200px;
  </div>
</body>
//效果如下
```
<div style="width:100%;height:200px;background-color:blueviolet;">
  width:100%;height:200px;
</div>

可以看到基本上宽的 100% 很容易就实现的，但是这里的 height 却不能设置成 % 的（该元素会消失看不见），这是为什么呢？

## 3.浏览器是如何计算高度和宽度的

Web 浏览器在计算有效宽度时会考虑浏览器窗口的打开宽度。如果你不给宽度设定任何缺省值，对于块元素，那浏览器会自动将页面内容平铺填满整个横向宽度。即我们不设置宽，会自动填满整个横向宽度，如下：

```
<div style="height:100%;background-color:blueviolet;">
  height:100%;
</div>
```

<div style="height:100%;background-color:blueviolet;">
  height:100%;
</div>


但是高度的计算方式完全不一样。事实上，浏览器根本就不计算内容的高度，除非内容超出了视窗范围(导致滚动条出现)。或者你给整个页面设置一个绝对高度。否则，浏览器就会简单的让内容往下堆砌，页面的高度根本就无需考虑。

因为页面并没有缺省的高度值，所以，当你让一个元素的高度设定为百分比高度时，无法根据获取父元素的高度，也就无法计算自己的高度。

即父元素的高度只是一个缺省值：`height: auto;` 我们设置 `height：100%` 时，是要求浏览器根据这样一个缺省值来计算百分比高度时，只能得到 undefined 的结果。也就是一个 null 值，浏览器不会对这个值有任何的反应。

各个浏览器对于宽高的解析也不相同，大家可以自己搜索一下。

## 4.如何解决

现在你知道了吧，% 是一个相对父元素计算得来的高度，要想使他有效，我们需要设置父元素的 height;

要特别注意的一点是，在 `<body>` 之中的元素的父元素并不仅仅只是 `<body>`，还包括了 `<html>`。

所以我们要同时设置这两者的 height，只设置其中一个是不行的：
```css
html,body{
  height: 100%;
  margin: 0;
  padding: 0;
}
```

![](/assets/images/posts/css/4261529069-5a4e0f2045bb0_articlex.png)

## 5.关于line-height居中的一点误解？

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    html,body{
      height: 100%;
      margin: 0;
      padding: 0;
    }
    div {
      color: white;
      text-align: center;
      font-size: 30px;
      line-height: 100%;
      background-color: blueviolet;
    }
  </style>
</head>

<body>
  <div style="height:100%;">height:100%;</div>
</body>

</html>
```

![](/assets/images/posts/css/20180203-120509@2x.png)

全部代码如上，可以看到设置了 line-height 为 100% 没有居中，这是为什么呢，因为这时候的 % 是相对于字体尺寸的.所以直接作用于没有绝对高度的元素是不行的。

-[CSS line-height 属性](http://www.w3school.com.cn/cssref/pr_dim_line-height.asp)

![](/assets/images/posts/css/20180203-120754@2x.png)

这时候要想居中，可以如下，做一个 div 嵌套，一个负责高度，一个负责居中，虽然感觉并不会这样用到，但是居中还是很灵验的~

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    html,
    body {
      height: 100%;
      margin: 0;
      padding: 0;
    }

    .div1 {
      background-color: blueviolet;
      position: relative;
    }

    .div2 {
      font-size: 30px;    
      color: white;
      text-align: center;                    
      width: 400px;
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translateX(-50%)  translateY(-50%);
    }
  </style>
</head>

<body>
  <div style="height:100%;" class="div1">
    <div class="div2">height:100%;</div>
  </div>
</body>

</html>
```

![](/assets/images/posts/css/1018014712-5a4e19270155d_articlex.png)