---
layout: post
title: treegrid 如果传入标签会显示空白
categories: jQuery
description: treegrid 如果传入标签会显示空白
keywords: treegrid, 特殊字符, 标签
---

treegrid如果传入标签会显示空白

## 问题

如图，后台传入的参数为一个标签时

![](/assets/images/posts/jquery/esHSCJv.png)

tree会将其当成标签嵌入页面，由于当成标签，在页面中是看不见的
  
![](/assets/images/posts/jquery/aKB3PZC.png)

## 解决

判断 value，如果含有标签(<>)或者特殊字符(主要针对标签，其余特殊字符一般不会引起这个问题，但为了严谨都处理一下)，将其作为 input 的 value 显示，并将 input 的边框等全部去掉

```
let columns = [
    {
        field: 'name',
        title: '名称',
        formatter: formatName,
        width: '25%',
        align: 'left'
    }
]
function formatName (value,row,index) {
    let reg = new RegExp("[`~!@#$^&*()=|{}':;',\\[\\].<>/?~！@#￥……&*（）——|{}【】‘；：”“’。，、？%+_]");
    if (reg.test(value)) { // 如果包含特殊字符
        return  '<input value="'+value+'" readonly style="border:none;background:none"/>';
    } else {
        return  value;
    }
}
```

## 还有问题？

![](/assets/images/posts/jquery/tIZbMjR.png)

虽然已经将特殊字符都添加到 value 中，但这个特殊字符太特殊了，既有<>又有""，浏览器最后解析为 input 的 `value="<el-col :span="`，而且后面有 >，浏览器认为 input 结束了，`readonly style="border:none;background:none"/>`都不在 input 中

## 继续解决

使用 textarea 标签

```
function formatName (value,row,index) {
    let reg = new RegExp("[`~!@#$^&*()=|{}':;',\\[\\].<>/?~！@#￥……&*（）——|{}【】‘；：”“’。，、？%+_]");
    if (reg.test(value)) {// 如果包含特殊字符
        return  `<textarea readonly style="width:100%; border:none; background:none; outline:none;">${value}</textarea>`;
    } else {
        return  value;
    }
}
```

## 再来问题？

当文本太长时，textarea 会换行

## 终极解决

使用 xmp 标签(已在 HTML4.0 被移除，但各大浏览器仍支持)

```
function formatName (value,row,index) {
    let reg = new RegExp("[`~!@#$^&*()=|{}':;',\\[\\].<>/?~！@#￥……&*（）——|{}【】‘；：”“’。，、？%+_]");
    if (reg.test(value)) {// 如果包含特殊字符
        return `<xmp style="margin:0; width:400px; overflow: hidden; text-overflow:ellipsis; white-space: nowrap;">${value}</xmp>`
    } else {
        return  value;
    }
}
```

xmp 默认 white-space: pre; margin: 1em 0px; 需要修改一下。width 根据实际情况而定