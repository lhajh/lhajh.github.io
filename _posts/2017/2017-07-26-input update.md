---
layout: post
title: input同一文件二次上传无效解决办法
categories: JS
description: input同一文件二次上传无效解决办法
keywords: input file
---

input同一文件二次上传无效解决办法

##input type=file 上传文件的过程中，遇到同一个文件二次上传无效的问题
### input[type=file]使用的是onchange去做，onchange监听的为input的value值，只有在内容发生改变的时候去触发，而value在上传文件的时候保存的是文件的内容，你只需要在上传成功的回调里面，将当前input的value值置空即可。
`event.target.value='';`