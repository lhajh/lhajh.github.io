---
layout: post
title: jQuery插件
categories: jQuery
description: 工作中使用jQuery插件的问题总结
keywords: jQuery, 插件
---

工作中使用jQuery插件的问题总结

## [jQuery UI库中dialog对话框功能使用全解析](http://www.jb51.net/article/82904.htm)
## jQuery ui Dialog 的弹出窗体高度会产生变化 
jQuery ui Dialog 的弹出窗体有时会出现当再次打开或在父窗体拖动时，高度会发生变化，则需要进行设置。
open , dragstop 事件处理
```
  $('#showResult').dialog(
	{ 
		height: 320, 
		width: 300, 
		position: ['right'],
        open: function (event, ui) {
            // Height setter has no effect after init either
            $(this).dialog("option", "height", 320);
            // Width setter works after initialization too
            $(this).dialog("option", "width", 300);
        },
        dragStop: function (event, ui) {
            $(this).dialog("option", "height", 320);
            // Width setter works after initialization too
            $(this).dialog("option", "width", 300);
        }
    });
```
## jQuery.validate失去焦点时就验证
```
$("#form").validate({  
    onfocusout: function(element) { $(element).valid(); },  
    rules:{  
          
    }   
});  
```
## [jQuery Validate 表单验证](http://www.runoob.com/jquery/jquery-plugin-validate.html)
## [jQuery Validate 相关参数及常用的自定义验证规则](http://blog.csdn.net/xh16319/article/details/9987847)
## [jQuery分页插件pagination.js](http://www.jq22.com/yanshi5697)
## JQuery跳出each循环的方法
```
在jquery跳出循环不可以直接使用continue和break了，因为在jquery中没有这两条命令。
后来上网查了下，得到了结果：
return false;——跳出所有循环；相当于 javascript 中的 break 效果。
return true;——跳出当前循环，进入下一个循环；相当于 javascript 中的 continue 效果
```
## [Jquery结合HTML5实现文件上传](http://www.jb51.net/article/68396.htm)
### 源码中有点错误，在 `浏览器版本太低，不支持改上传！`的上一行等号右面应该是undefined
## treegrid分级问题
- treegrid根据_parentId来分级
- 树子节点的_parentId和其父节点的id对应
- 根节点的_parentId为null
- 一个节点的id和_parentId不能相同