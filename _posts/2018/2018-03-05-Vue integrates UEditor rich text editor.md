---
layout: post
title: vue 集成 UEditor 富文本编辑器
categories: [Vue]
description: vue 集成 UEditor 富文本编辑器
keywords: vue, UEditor
---

vue是前端开发者所追捧的框架，简单易上手，但是基于vue的富文本编辑器大多数太过于精简。于是我将百度富文本编辑器放到vue项目中使用。

[这个是基础设置](http://blog.csdn.net/psd_html/article/details/73312859)

按照上述步骤就可以在页面中显示 UEditor，但此时上传仍不能使用

点击上传按钮会提示 `后端配置项没有正常加载，上传插件不能正常使用！`

需要在 `ueditor.config.js` 中修改 `serverUrl`

例如：
```
// 服务器统一请求接口路径
, serverUrl: window.location.protocol + "//" + window.location.host + "/api/ue/config"
```
这个 url 路径是和后台商量好的，保证访问这个接口可以返回 config.json

注: **本人使用的是 jsp 版本 utf-8 版的, 这个 config.json 在下载文件解压后的 jsp 文件夹里面. 在这个 json 文件中是前后端通信相关的配置, 包括上传路径, 文件格式限制等**

下面是 json 配置的部分代码

```js
"imageActionName": "uploadimage", /* 执行上传图片的action名称 */
"imageFieldName": "upfile", /* 提交的图片表单名称 */
"imageMaxSize": 2048000, /* 上传大小限制，单位B */
"imageAllowFiles": [".png", ".jpg", ".jpeg", ".gif", ".bmp"], /* 上传图片格式显示 */
"imageCompressEnable": true, /* 是否压缩图片,默认是true */
"imageCompressBorder": 1600, /* 图片压缩最长边限制 */
"imageInsertAlign": "none", /* 插入的图片浮动方式 */
"imageUrlPrefix": "", /* 图片访问路径前缀 */
"imagePathFormat": "/ueditor/jsp/upload/image/{yyyy}{mm}{dd}/{time}{rand:6}"
```

[这是后台上传配置](http://www.olbids.com/f/topic/view?topic=5)

下面是一下常见问题：

- [百度富文本编辑器 UEditor 1.4.3 插入视频后路径被清空问题](http://blog.csdn.net/eunyeon/article/details/52964152)
- [使用百度编辑器ueditor表格无法显示边框以及边框颜色等系列问题解决方案](http://blog.csdn.net/kingqiji01/article/details/65495647#reply)
- [如何让某一元素内的内容不被reset.css重置？](https://segmentfault.com/q/1010000013204367/a-1020000013210136)

关于被 reset.css 重置的解决方法(Vue版)：

.vue 文件
```
<el-dialog
  size="large"
  top="5%"
  :close-on-click-modal="false"
  :close-on-press-escape="false"
  :before-close="handleClose"
  :visible.sync="newsDialogVisible">
  <div class="dialog-title" ref="title">{{title}}</div>
  <p class="dialog-date" ref="date">发布时间：{{createDate}}</p>
  <!-- 利用 iframe 可以使 reset.css 不起作用；动态 src 是为了每次弹框都重新加载 html 页面，避免缓存 -->
  <iframe :src="src" width="100%" :height="iframeHeight" frameborder="0"></iframe>
</el-dialog>

// 显示新闻详情
showDetail (data) {
  this.title = data.title
  this.createDate = data.createDate
  // 利用 session 存储内容
  sessionStorage.setItem('ueContent', data.context)
  this.src = '/static/news.html'
  this.newsDialogVisible = true
  this.$nextTick(() => {
    this.iframeHeight = document.getElementsByClassName('el-dialog__body')[0].clientHeight -
    30 * 2 - this.$refs.title.clientHeight - this.$refs.date.clientHeight - 20
    let videoDom = document.querySelector('.video-js')
    if (videoDom) {
      window.videojs(videoDom)
    }
  })
},
// 关闭新闻详情
handleClose (done) {
  this.title = ''
  this.createDate = ''
  this.src = ''
  sessionStorage.removeItem('ueContent')
  done()
}
```

news.html 文件
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <div id="newsPreview"></div>
</body>
<script>
  document.getElementById('newsPreview').innerHTML = sessionStorage.getItem('ueContent')
</script>
</html>
```
