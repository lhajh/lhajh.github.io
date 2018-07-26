---
layout: post
title: ElementUI 问题集合
categories: UI
description: ElementUI 问题集合
keywords: ElementUI
---

ElementUI 问题集合

## table + pagination 序号问题
目前 ElementUI(1.4.4) 使用 table + pagination 时，el-table-column 设置 `type="index"` 点击下一页后，序号没有对应按照上一页的数据继续增加，而是直接显示为 1,2,3，。。。

本文是在基于 @yuanshengchao 在 [Element issues](https://github.com/ElemeFE/element/issues/1086) 中提出的方案进行优化 (其实就是照搬)

具体代码去看链接就可以了，有可能当你看到本文时，Element 团队已经处理好这个问题了

## table formatter 问题

使用 formatter 时，按照官方给出的例子，第三个参数永远是 undefined，如果需要对所有列都 format(逻辑是一样的)，则需要每一列都写一个函数，将需要 format 的参数传入，这样无疑是没有意义的

[具体问题](https://github.com/ElemeFE/element/issues/6606)

[解决](https://jsfiddle.net/xf9j7x9r/1/)

例子：
```html
<el-table-column
  align="center"
  prop="createDate"
  :formatter="(row, column) => formatTime(row, column, 'createDate')"
  label="创建日期"
  min-width="150">
</el-table-column>

一般 column 不常用，只需要传入需要格式化的值即可，即下面这种写法

<el-table-column
  align="center"
  prop="publishDate"
  :formatter="row => formatTime(row.publishDate)"
  label="发布时间"
  min-width="100">
</el-table-column>
```
```js
methods: {
  formatTime (value, type) {
    if (value == null || value === '') {
      return ''
    }
    type = type || 'YYYY-MM-DD'
    return this.$moment(value).format(type)
  }
}
```

## Pagination 分页

### 问题

废话不多说，先看图

![](/assets/images/posts/elementUi/aw3r4g.png)

注：在 current-change 事件会调 fetchDesign 方法

如图，原意是想在发请求前将 total 置为 0，即初始值，请求成功后根据返回的值来改变 total，这样就不用写 else 和 catch 了

结果就出问题了，不论点击第几页，永远会跳回第一页。比如点击第四页，current-change 事件会触发两次，第一次的回调参数是 4，而第二次就是 1，所以会永远跳回第一页

结果就去 `https://github.com/ElemeFE/element/issues/
`。发现一个 [和我类似的](https://github.com/ElemeFE/element/issues/6809)

其中 @huguangju 的话点醒了我，原文：

> 删除 jumper 中的数字后，当前页跳到第 1 页并不是点击 next 直接导致的，而是 jumper 失焦后其值此时为空字符串，应用值到 internalCurrentPage 前会校验设置的页码，部分源码如下，会被设置为 1：

```js
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


## 利用 v-if 动态渲染表格时，在 el-table-column 中添加 key 属性防止表格复用

先上代码
```html
<div class="monitor-container">
  <el-card class="box-card">
    <div slot="header" class="clearfix">
      <el-tabs
        @tab-click="handleClick"
        v-model="monitorActiveName">
        <el-tab-pane label="海康监控" name="0"></el-tab-pane>
        <el-tab-pane label="萤石监控" name="1"></el-tab-pane>
      </el-tabs>
      <el-button style="float: right;" type="primary"
        icon="plus" @click="addMonitor">
        {{addTxt}}
      </el-button>
    </div>
  </el-card>
  <el-table
    :data="monitorData"
    border
    :row-class-name="toggleColor"
    style="width:100%">
    <el-table-column
      v-if="monitorActiveName === '0'"
      align="center"
      prop="ipAddress"
      label="视频监控地址"
      min-width="150">
    </el-table-column>
    <el-table-column
      v-if="monitorActiveName === '0'"
      align="center"
      prop="webPort"
      label="Web 端口号"
      min-width="150">
    </el-table-column>
    <el-table-column
      v-if="monitorActiveName === '0'"
      align="center"
      prop="userName"
      label="用户名"
      min-width="150">
    </el-table-column>
    <el-table-column
      v-if="monitorActiveName === '0'"
      align="center"
      prop="deviceName"
      label="设备名称"
      min-width="150">
    </el-table-column>
    <el-table-column
      v-if="monitorActiveName === '1'"
      align="center"
      prop="appKey"
      label="appKey"
      min-width="150">
    </el-table-column>
    <el-table-column
      v-if="monitorActiveName === '1'"
      align="center"
      prop="appSecret"
      label="appSecret"
      min-width="150">
    </el-table-column>
    <el-table-column
      align="center"
      label="操作"
      width="150">
      <template slot-scope="scope">
        <el-button type="text" class="el-icon-edit"
          @click="editRow(scope.$index, scope.row)">
        </el-button>
        <el-button type="text" class="el-icon-delete"
          @click="deleteRow(scope.$index, scope.row)">
        </el-button>
      </template>
    </el-table-column>
  </el-table>
</div>
```
解释一下，有两个监控列表：海康和萤石，通过一个 tab 切换来显示，使用 v-if 来动态控制表格的显隐

具体问题看下图：

![](/assets/images/posts/elementUi/QQ20180112-111720@2x.png)

页面加载完成后，可以正常显示，顺序也和代码是一致的

切换到萤石后：

![](/assets/images/posts/elementUi/QQ20180112-111805@2x.png)

现在已经可以看出来顺序和代码不一致了

再切到海康：

![](/assets/images/posts/elementUi/QQ20180112-111819@2x.png)

设备名称和用户名对调了

以后再切换也都是这个顺序了

萤石那个还可以解决，我代码顺序反一下就可以保证 appKey 在前面，appSecret 在后面，但海康没办法，就算一开始把用户名和设备名称对调，切换一次后又对调了

可以看一下下面这个动图，发现是代码复用了：

![](/assets/images/posts/elementUi/QQ20180112-111429-HD.gif)

这时想到 v-for 通过添加 key 来唯一标识遍历后生成的对象，这里是不是也可以这么做

修改后的代码：
```html
<el-table-column
  v-if="monitorActiveName === '0'"
  key="ipAddress"
  align="center"
  prop="ipAddress"
  label="视频监控地址"
  min-width="150">
</el-table-column>
```
这里就贴出一部分，其余的都是添加了一个 key 属性，只要每个 el-table-column 的 key 值不一样就行，这里是和 prop 保持一致

## 自定义滚动条

介绍: [Vue 的自定义滚动，我用 el-scrollbar](https://qiqihaobenben.github.io/Front-End-Basics/project/el-scrollbar)

使用: [vue element 隐藏组件滚动条 scrollbar 使用](https://blog.csdn.net/zhongguohaoshaonian/article/details/79734787)

### 问题一

以下为我自己设置的全局样式

```css
.el-scrollbar {
  height: 100%;
  /* margin-right 配合下面的 padding-right 使用, 防止滚动条遮挡内容 */
  margin-right: -10px;
}
/* 查看官方网站发现是 auto, 而且也能隐藏掉横向的滚动条, 那就用 auto 吧 */
.el-scrollbar .el-scrollbar__wrap {
  overflow-x: auto;
}
.el-scrollbar__view {
  padding-right: 10px;
}
```

但由于这个滚动条是 Element Ui 内部使用的, 目前发现就 `el-select-dropdown` 中在使用, 随意修改全局样式肯定会影响人家原有样式的. 比如这样:

下拉框最后一个显示不全, `hover` 后超出界线框

![](/assets/images/posts/elementUi/2CCEC5.png)

目前解决办法就三种:

1. 不修改全局样式, 局部修改样式, 哪里使用滚动条哪里修改, 但毕竟需要滚动条的地方比 select-dropdown 要多, 不划算
2. 封装成组件, 在组件内部修改, 但这已经是一个组件了, 有点画蛇添足
3. 还是修改全局样式, 只不过在 select-dropdown 中重写样式, 使用原来的样式

```css
/* 修改 scrollbar 在 select 下显示不全问题 */
.el-select-dropdown {
  .el-scrollbar {
    height: auto;
    margin-right: 0;
    .el-scrollbar__wrap {
      overflow: scroll;
      .el-scrollbar__view {
        padding-right: 0;
      }
    }
  }
}
```

## 问题二

如果布局使用的是百分比布局, 那使用滚动条也会有问题.

比如两个子元素高度都是 `50%`, 如果使用 `el-scrillbar` 包裹子元素的话, 子元素的父元素会变成 `el-scrollbar__view`, 而这个元素是没有设置高度的, 会导致子元素的 `50%` 失效, 从而被内容撑开, 解决办法就是给 `el-scrollbar__view` 设置高度为 `100%`
