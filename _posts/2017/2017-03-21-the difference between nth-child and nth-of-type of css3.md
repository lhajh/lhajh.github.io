---
layout: post
title: CSS3的nth-child和nth-of-type的区别
categories: CSS
description: CSS3的nth-child和nth-of-type的区别
keywords: CSS3, nth-child, nth-of-type
---

对于初学者来说，区别 `nth-child` 和 `nth-of-type` 是一个比较头疼的问题，为了更好帮助大家区别两者使用方法，特在此加以区分。

首先创建一个HTML结构。

```
<div class="post">
	<p>我是文章的第一个段落</p>
	<p>我是文章的第二个段落</p>
</div>
```

接下来,使用 `:nth-child` 和 `:nth-of-type` 选择段落并改变其文字颜色。

```
.post>p:nth-child(2){color:red;}
.post>p:nth-of-type(2){color:red;}
```

![](/assets/images/posts/css/er4htd.png)

上面的代码都把 `.post` 中的第二段文字变成了大红色，是不是代表这两个选择器就是一样的呢？其实不然。`nth-child` 仅从字面上来解释，其实包含了两层意思。首先是一个 p 元素，而且这个 p 是父元素 div 的第二个子元素；而 `nth-of-type` 从字面上解释是“选择父元素 div 的第二个 p 元素”。

上面一段话看起来是不是很晕，有没有更好方法来区分它们呢？~~~ 有的！把上面的 HTML 结构改变一下，在段落前加一个标题 h1。

```
<style type="text/css">
    .post>p:nth-child(2){color:red;}
    .post>p:nth-of-type(2){color:blue;}
</style>

<h1>我是标题</h1>
<p>我是文章的第一个段落</p>
<p>我是文章的第二个段落</p>
```

![](/assets/images/posts/css/GR7mj.png)

可以看到：`:nth-child(2)` 选择的是第一个段落，而 `:nth-of-type(2)` 选择的是第二个段落

如果在 h1 标题后面添加一个 h2 标题

![](/assets/images/posts/css/As3g5h.png)

此时 `:nth-child(2)` 将无法选择任何元素，因为，此时 div 的第二个元素并不是段落一 p，所以无法选择任何元素。但 `:nth-of-type(2)` 仍然能正常工作，因为选择的始终是 div 中第二个段落 p。

大家只需记住一点：`选择器:nth-child(n)` 先根据后面的数字选中父元素的第 n 个子元素，再判断这个子元素是否是前面的选择器，如果是则样式生效，否则无效；而 `选择器:nth-of-type(n)` 先在父元素中找出所有符合前面选择器的子元素，再从这些子元素中选择第 n 个子元素(在父元素中有可能不是第 n 个)

如果父元素所有子元素类型都是相同的，那么这两个选择器是没区别的