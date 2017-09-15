---
layout: post
title: ElementUI table + pagination 序号问题
categories: UI
description: ElementUI table + pagination 序号问题
keywords: ElementUI, table, pagination, 序号
---

目前 ElementUI(1.4.4)使用 table + pagination 时，el-table-column 设置 `type="index"` 点击下一页后，序号没有对应按照上一页的数据继续增加，而是直接显示为1,2,3，。。。

本文是在基于 @yuanshengchao 在 [Element issues](https://github.com/ElemeFE/element/issues/1086) 中提出的方案进行优化(其实就是照搬)

具体代码去看链接就可以了，有可能当你看到本文时，Element 团队已经处理好这个问题了