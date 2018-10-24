---
layout: post
title: if else 语句有可能同时执行吗
categories: JS
description: if else 语句有可能同时执行吗
keywords: js, if else
---

上学时就知道 if else 是两个对立关系, 两者只能执行其中的一个语句, 但最近却发现貌似并不是这样动, 两者竟然同时执行

## 框架

Vue + ElementUI

## 场景

具体场景就是最近动一个项目, 类似网盘的文件夹, 需要加入权限控制

![](/assets/images/posts/js/9381916C49.jpg)

如上图所示, 下面是一个 `table-checkbox` 的文件夹和文件的列表, 上面是关于文件夹和文件的操作.

每个文件夹和文件都有其自己的权限

```js
// 文件夹权限
documentPrivilege: {
  hasPreviewPrivilege: true,
  hasDownloadPrivilege: true,
  hasAddPrivilege: true,
  hasUpdatePrivilege: true,
  hasDeletePrivilege: true
}
// 基础文件夹权限
baseDocumentPrivilege: {
  hasPreviewPrivilege: true,
  hasDownloadPrivilege: true,
  hasAddPrivilege: true,
  hasUpdatePrivilege: true,
  hasDeletePrivilege: true
},
```

当点击某一个文件夹或文件, 就会根据权限判断上面的操作是否可点击

![](/assets/images/posts/js/F7579BB.jpg)

## 关键代码

```js
// 选择项发生变化
handleSelectionChange (val) {
  this.documentSelection = val
  this.permissionsDisabled()
}
// 根据权限判断按钮是否可点击
permissionsDisabled () {
  // 当前选中文件夹或文件
  if (this.documentSelection.length) {
    this.documentSelection.forEach(item => {
      if (item.documentPrivilege) {
        for (const key in this.baseDocumentPrivilege) {
          if (this.baseDocumentPrivilege.hasOwnProperty(key)) {
            // 只要有一个是 false 就不可点击
            this.documentPrivilege[key] =
              this.documentPrivilege[key] && item.documentPrivilege[key]
          }
        }
      }
    })
    // 当前没有选中任何文件夹或文件
  } else {
    this.documentPrivilege = this.baseDocumentPrivilege
  }
}
```

## 问题

页面初始化没有问题, 只有新建文件夹可以点击, 但如果选中某一个没有新建文件夹权限的文件或文件夹, 再取消, 新建文件夹不可点击了, 打印发现 `documentPrivilege` 和 `baseDocumentPrivilege` 一样了

## 分析

`documentPrivilege` 和 `baseDocumentPrivilege` 一样是因为都是引用的相同地址值, 但选中一个时只会进 if 判断, 并不会进 else, 感觉就是 if else 同时执行了

其实并不是, 由于该方法 `permissionsDisabled` 会执行多次(每次点击 checkbox 都会出发), 只要有一次没有选中任何文件夹或文件, 就会将 `documentPrivilege` 和 `baseDocumentPrivilege` 指向为同一个地址值, 这样改变 `documentPrivilege`, `baseDocumentPrivilege` 也会变

## 解决

不要使用地址引用值赋值

可以使用 `Object.assign` 或 ES6 的 `...`

```js
else {
  this.documentPrivilege = Object.assign(
    {},
    this.baseDocumentPrivilege
  )
}
```

## 总结

发生此 bug 的三个关键条件:

1. 方法里有 if else 逻辑判断
2. 该方法被调用多次
3. 对象赋值使用地址引用值

解决就只能是不要使用地址引用值

看来 if else 并不能同时执行, 只不过是多次调用方法从而使 if else 看起来同时执行了
