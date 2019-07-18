---
layout: post
title: axios post 请求中下载文件流
categories: [JS]
description: axios post 请求中下载文件流
keywords: js
---

// NodeJS

```js
var express = require('express')
var app = express()
app.post('/download', function(req, res, next) {
  res.setHeader('Content-Type', 'application/vnd.ms-excel')
  res.write('123456')
  res.end()
})
```

// WEB 端代码：

```js
axios({
  url: 'http://localhost:3000/users/download',
  method: 'post',
  responseType: 'blob'
})
  .then(function(res) {
    var blob = new Blob([res.data], {
      type: res.headers['content-type']
    })
    var downloadElement = document.createElement('a')
    var href = window.URL.createObjectURL(blob) //创建下载的链接
    downloadElement.href = href
    downloadElement.download = 'xxx.xlsx' //下载后文件名
    document.body.appendChild(downloadElement)
    downloadElement.click() //点击下载
    document.body.removeChild(downloadElement) //下载完成移除元素
    window.URL.revokeObjectURL(href) //释放掉blob对象
  })
  .catch(function() {})
```

## 参考资料

- [axios post请求中下载~文件流 - 知乎](https://zhuanlan.zhihu.com/p/34961387)
