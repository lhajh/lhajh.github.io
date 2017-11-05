---
layout: post
title: ElementUI 问题集合
categories: UI
description: ElementUI 问题集合
keywords: ElementUI
---

ElementUI 问题集合

## table + pagination 序号问题
目前 ElementUI(1.4.4)使用 table + pagination 时，el-table-column 设置 `type="index"` 点击下一页后，序号没有对应按照上一页的数据继续增加，而是直接显示为1,2,3，。。。

本文是在基于 @yuanshengchao 在 [Element issues](https://github.com/ElemeFE/element/issues/1086) 中提出的方案进行优化(其实就是照搬)

具体代码去看链接就可以了，有可能当你看到本文时，Element 团队已经处理好这个问题了

## table formatter 问题

使用 formatter 时，按照官方给出的例子，第三个参数永远是 undefined，如果需要对所有列都 format(逻辑是一样的)，则需要每一列都写一个函数，将需要 format 的参数传入，这样无疑是没有意义的

[具体问题](https://github.com/ElemeFE/element/issues/6606)

[解决](https://jsfiddle.net/xf9j7x9r/1/)

## Pagination 分页

### 问题

废话不多说，先看图

![](/assets/images/posts/elementUi/aw3r4g.png)

注：在 current-change 事件会调 fetchDesign 方法

如图，原意是想在发请求前将 total 置为 0，即初始值，请求成功后根据返回的值来改变 total，这样就不用写 else 和 catch 了

结果就出问题了，不论点击第几页，永远会跳回第一页。比如点击第四页，current-change 事件会触发两次，第一次的回调参数是 4，而第二次就是 1，所以会永远跳回第一页

结果就去 `https://github.com/ElemeFE/element/issues/
`。发现一个[和我类似的](https://github.com/ElemeFE/element/issues/6809)

其中 @huguangju 的话点醒了我，原文：

> 删除jumper中的数字后，当前页跳到第1页并不是点击next直接导致的，而是jumper失焦后其值此时为空字符串，应用值到internalCurrentPage前会校验设置的页码，部分源码如下，会被设置为1：

```
getValidCurrentPage(value) {
    value = parseInt(value, 10);
    // ...
    if (resetValue === undefined && isNaN(value)) {
        resetValue = 1; // here
    } else if (resetValue === 0) {
        resetValue = 1;
    }
    return resetValue === undefined ? value : resetValue;
}
```

因为每次触发 current-change 事件都会调用 fetchDesign 方法，而在这个方法中都将 total 置为 0，这样由于要校验设置的页码，比如点击第四页，当前为 4，而 total 为 0，总数是 0，哪来的第四页，所以就被置为 1 了。

### 解决

那就不能图省事，在发请求前就将 total 置为 0，而是根据请求返回来改变 total 的值。如图：

![](/assets/images/posts/elementUi/ef4T6d.png)