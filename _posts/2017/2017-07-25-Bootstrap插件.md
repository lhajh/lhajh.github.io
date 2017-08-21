---
layout: post
title: 工作中使用Bootstrap插件问题总结
categories: Bootstrap
description: Bootstrap插件
keywords: Bootstrap, 插件
---

工作中使用Bootstrap插件问题总结

## [Bootstrap日期和时间表单组件datetimepicker](http://www.bootcss.com/p/bootstrap-datetimepicker/index.htm)
## bootstrap-paginator的样式为什么是竖着的，我想改成横着的?
### 答：bootstrap-paginator.js里的
```
listContainer = $("<ul></ul>"),改成listContainer = $("<ul class='pagination'></ul>")
```