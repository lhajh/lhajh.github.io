---
layout: post
title: vue 数据刷新的问题
categories: Vue
description: vue 数据刷新的问题
keywords: vue, props
---

vue 修改了数组中某一项值, 却没有触发 DOM 更新

具体功能就是上面是展示新闻图片, 下面是新闻列表, 每隔 5 秒高亮下一列新闻, 同时更新新闻的 src, 使上面图片跟着新闻列表实时刷新. 类似轮播效果

```html
<img :src="newsImg.logoUrl"
  height="40%"
  width="100%"
  @click="showDetail(newsImg)">
<div class="data-detail-box">
  <div :class="['data-detail', {active: item.isActive}]"
    v-for="item in newsData"
    :key="item.id"
    @click="showDetail(item)">
    <span class="detail-title"
      :title="item.title">{{item.title}}</span>
    <span class="detail-date">{{item.publishDate}}</span>
  </div>
</div>
```

```js
// 获取新闻列表
fetchNews () {
  this.$get(api.fetchNews(this.parkId, 1))
    .then(res => {
      if (res.data.length) {
        res.data.forEach(item => {
          item.publishDate = initTime(item.publishDate, '-')
        })
        this.newsData = res.data
        this.$nextTick(() => {
          let num = 0
          this.newsData[num].isActive = true
          this.newsImg = this.newsData[num]
          this.newsInter = setInterval(() => {
            num++
            let newsBox = document.querySelector('.data-detail-box').getBoundingClientRect()
            let newsBoxBottom = newsBox.bottom
            let itemBox = document.querySelector('.data-detail.active').getBoundingClientRect()
            let itemBottom = itemBox.bottom
            let itemHeight = itemBox.height
            // 判断当前选中列是否处于父元素可视区域最后一个
            if (itemBottom + itemHeight / 2 > newsBoxBottom) {
              num = 0
              document.querySelector('.data-detail-box').scrollTop = 0
            }
            // 判断当前选中列是否是最后一个
            if (num >= this.newsData.length) {
              num = 0
              document.querySelector('.data-detail-box').scrollTop = 0
            }
            this.newsImg = this.newsData[num]
            this.newsData.forEach(item => {
              item.isActive = false
            })
            this.newsData[num].isActive = true
          }, 5000)
        })
      } else {
        this.newsData = []
      }
    })
    .catch(mes => {
      this.newsData = []
    })
}
```

本来这个样子功能都实现的, 但领导觉得每切换一次新闻就重新下载一张图片要浪费服务器的带宽(由于 img 是动态 src 会重复下载相同图片), 需要前端将图片缓存起来, 只加载一次

那我就图片也循环生成吧, 选中哪个就显示哪个, 其余隐藏即可吧

```html
<img v-for="item in newsData"
  :key="item.logoUrl"
  :src="item.logoUrl"
  v-show="item.isActive"
  height="40%"
  width="100%"
  @click="showDetail(item)">
<div class="data-detail-box">
  <div :class="['data-detail', {active: item.isActive}]"
    v-for="item in newsData"
    :key="item.id"
    @click="showDetail(item)">
    <span class="detail-title"
      :title="item.title">{{item.title}}</span>
    <span class="detail-date">{{item.publishDate}}</span>
  </div>
</div>
```

然而列表不会轮播了, 图片也跟着不动了, 也没有报错

但切换会原来的就可以, 这是为什么啊?

对比两次的代码, 可以看出, 只是修改了 img 部分, 原来的 img 是动态 src, 每过 5 秒重新加载, 会触发 DOM 刷新(从而带动列表刷新, 个人猜测), 而现在 img 一次性都加载完了, 只是根据 `isActive` 来显隐, 并不会触发 DOM 刷新, 当然列表也不会刷新了. 那就只能靠列表自己触发了

```js
// this.newsData.forEach(item => {
//   item.isActive = false
// })
this.newsData = this.newsData.map(item => {
  item.isActive = false
  return item
})
```

这应该也可以证明 vue 绑定值是 `浅 watch`

顺便 newsImg 也不需要了
