---
layout: post
title: 修改浏览器默认滚动条
categories: [CSS]
description: 修改浏览器默认滚动条
keywords: CSS, 滚动条
---

作为一个有理想的程序员, 页面好看与否直接决定了你是不是一个合格的前端, 而作为页面一部分 -- 滚动条, 直接影响了页面的美观

那么如何修改滚动条的样式呢? 要么使用 `js`(代表者 - `jQuery`), 要么使用 `css`

关于 `jQuery` 滚动条插件网上一大堆, 就不介绍了

下面重点介绍通过 `css` 来修改原生滚动条样式

## 修改默认滚动条样式

注:

> **以下特性是非标准的，请尽量不要在生产环境中使用它！--MDN**

```css
body,
html {
  height: 100%;
  /* 三角箭头的颜色 */
  scrollbar-arrow-color: #3dffff;
  /* 立体滚动条的颜色  */
  scrollbar-face-color: #3dffff;
  /* 立体滚动条亮边的颜色 */
  scrollbar-3dlight-color: #3dffff;
  /* 滚动条空白部分的颜色   */
  scrollbar-highlight-color: #3dffff;
  /* 立体滚动条阴影的颜色 */
  scrollbar-shadow-color: #3dffff;
  /* 立体滚动条强阴影的颜色 */
  scrollbar-darkshadow-color: #3dffff;
  /* 立体滚动条背景颜色 */
  scrollbar-track-color: #a3ffff;
  /* 滚动条的基本色 */
  scrollbar-base-color: #3dffff;
  -ms-overflow-style: -ms-autohiding-scrollbar;
}

/* 整个滚动条 */
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
  background: transparent;
}
/* 滚动条轨道 */
::-webkit-scrollbar-track {
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  -webkit-border-radius: 4px;
  border-radius: 4px;
  background: transparent;
}
/* 滚动条上的滚动滑块 */
::-webkit-scrollbar-thumb {
  -webkit-border-radius: 4px;
  border-radius: 4px;
  background: rgba(144, 146, 152, 0.3);
}
::-webkit-scrollbar-thumb:hover {
  background: rgba(144, 146, 152, 0.5);
}
/* 滚动条上的按钮 (上下箭头) */
::-webkit-scrollbar-button {
  display: none;
}
/* 当同时有垂直滚动条和水平滚动条时交汇的部分 */
::-webkit-scrollbar-corner {
  background: transparent;
}
```

### 参考资料

- [::-webkit-scrollbar](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::-webkit-scrollbar)

- [自定义浏览器滚动条的样式，打造属于你的滚动条风格](https://www.lyblog.net/detail/314.html)

- [css scrollbar 样式设置](https://segmentfault.com/a/1190000012800450)

## 隐藏滚动条

[CSS 实现隐藏滚动条同时又可以滚动](https://www.cnblogs.com/alice626/p/6206760.html)
