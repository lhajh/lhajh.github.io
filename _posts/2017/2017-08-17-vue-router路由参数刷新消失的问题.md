---
layout: post
title: vue-router路由参数刷新消失的问题
categories: Vue
description: vue-router路由参数刷新消失的问题
keywords: vue-router
---

vue-router路由参数刷新消失的问题

## 问题
页面使用vue-router在跳转时发的参数有用来给下一个页面请求数据用的。在进入页面后再次刷新，参数就消失了。
这是上个页面跳转写的。

![](http://i.imgur.com/SPOlhPS.png)

这边是从上个页面接收这个参数请求数据
```
methods: {
            init() {
                console.log(this.$route.params.articleId);
                this.params = this.$route.params;
                this.articleId = this.params.articleId;
                this.getArticleDetail(this.articleId);
            },
            getArticleDetail(id) {
                this.$http({
                    method: "GET",
                    url: "/getData/getArticleDetail",
                    params: {
                        id: id
                    },
   		  ...
        },
```
进入页面后再次刷新的话请求的数据就没有了。
## 解决
你在用 vue-router 跳转的时候可以把参数写进 query 里 `this.$router.push({name:'articleDetail, query:{articleId: articleId}});`
这样你的 url 就会像 `http://xxx.xxx.xxx/articleDetail?articleId=123`，这样无论你怎么刷新 articleId 都不会丢失
然后在你的 init 方法里 可以用 `this.articleId = this.$route.query.articleId;` 来获取id

如果要用 params 传参的话，可能需要在你的路由路径里也加上这个参数，比如你用`
this.$router.push({name:'articleDetail, params:{articleId: articleId}});`跳转，那么在路由里就要这样写
```
routes: [
    {
      path: '/articleDetail/:articleId',
      name: 'articleDetail'
    }
]
```
`path: '/articleDetail/:articleId',`里的 :articleId 是必须要有的

如果要传递对象，需要使用JSON转一下
```
this.$router.push({
    path: '/manage/composition',
    query: {
        dropDefaultActive: this.dropDefaultActive,
        option: JSON.stringify(this.option)
    }
})
```
相应的，接收页面还需要转回来
```
getUrlParam() {
    let param = this.$route.query;
    this.dropDefaultActive = param.dropDefaultActive;
    this.option = JSON.parse(param.option)
}
```