---
layout: post
title: Easyui Treegrid 上下移动实现
categories: jQuery
description: Easyui Treegrid 上下移动实现
keywords: Easyui, Treegrid
---

Easyui Treegrid 上下移动实现

## [Easyui Treegrid 上下移动实现](http://blog.csdn.net/guichang2012/article/details/48155775)
上面是源码地址，由于自己使用Vue，使用源码有问题，故自己又加工了一下
 
首先是项目框架
![](/assets/images/posts/jquery/JFHv8CS.png)
点击下移之后
![](/assets/images/posts/jquery/2YMSjfI.png)
当然上移也是可以的，下面看看代码
```
function move(o) { // 将此方法加入上下移的按钮事件即可  
    var n = $("#lineNodes").treegrid("getSelected");
	if(n === null){
		alert("无法移动！");
		return;
	}
    var selectRow = $('#datagrid-row-r2-2-'+n.id); 
	/* 注：在Vue中，当切换页面时，row-r后面的数字会累加，
	如切换到别的页面再切换回来，会变成row-r3-2，
	如果再切换到别的页面再切换回来，会变成row-r4-2
	所以在Vue中无法使用这个选择器，而treegrid当点击某一个节点时，
	会给该节点添加`datagrid-row-checked datagrid-row-selected`
	这两个类，可以根据其中某一个类来替换上面的选择器 */
    if(o=="up") {  
        var pre = selectRow.prev();//此处获得上一节点，关键  
        // alert(typeof(pre.attr("node-id"))=="undefined");return;  
        if(typeof(pre.attr("node-id"))=="undefined" || pre.attr("node-id").indexOf("L")==0) {  
            alert("无法移动！");  
        }else {//下面写数据库中的排序逻辑  
            var preId = pre.attr("node-id");  
            $.post("lineNodes/exchangeOrder.json",{"id1":n.id,"id2":preId},function(data) {  
                if(data=="true") {  
                    var n2 = $("#lineNodes").treegrid("pop",n.id);  
                    $("#lineNodes").treegrid("insert",{before:preId,data:n2});  
                    $("#lineNodes").treegrid("select",n.id);  
                }else {  
                    alert("移动过程出现异常，请稍后再试");  
                }  
            });  
        }  
    }else if(o=="down") {  
        var next = selectRow.next();//此处获得下一节点，关键  
        //alert(next.attr("node-id"));return;  
        if(typeof(next.attr("node-id"))=="undefined" || next.attr("node-id").indexOf("L")==0) {  
            alert("无法移动！")  
        }else {  
            var nextId = next.attr("node-id");  
            $.post("lineNodes/exchangeOrder.json",{"id1":n.id,"id2":nextId},function(data) {  
                if(data=="true") {  
                    var n2 = $("#lineNodes").treegrid("pop",nextId);  
                    $("#lineNodes").treegrid("insert",{before:n.id,data:n2})  
                }else {  
                    alert("移动过程出现异常，请稍后再试");  
                }  
            });  
        }  
    }  
}  
```
操作就是选中某行记录点击上下移按钮，关键代码：
```
var n2 = $("#lineNodes").treegrid("pop",nextId);  
$("#lineNodes").treegrid("insert",{before:n.id,data:n2})
```
先把当前选中行弹出来再插入到某行前面，如果是上移就插入到上条记录前面，如果是下移就将下一行插入到当前选中行前面，获取上、下行代码：
```
var next = selectRow.prev();  
var next = selectRow.next();
```
注意取不到会是null的判断
## 自己提取出来的
基于Vue点击某一个节点，切换上移、下移按钮是否可点击状态
template:
```
<el-button type="primary" :disabled="changeUpDisabled" @click="moveUp">上移</el-button>
<el-button type="primary" :disabled="changeDownDisabled" @click="moveDown">下移</el-button>
```
script:
```
data () {
	return {
		changeUpDisabled: true,
		changeDownDisabled: true
	}
}

$('#schedule_tree').treegrid({
	onClickRow: function (node) {
	    // 节点高亮选择时触发
        let selectRow = $('.datagrid-row-selected');
        let pre = selectRow.prev(); // 此处获得上一节点，关键
        let next = selectRow.next(); // 此处获得下一节点，关键
        if (typeof(pre.attr("node-id")) === "undefined" || pre.attr("node-id").indexOf("L") === 0) {
            _this.changeUpDisabled = true
        } else {
            _this.changeUpDisabled = false
        }
        if (typeof(next.attr("node-id")) === "undefined" || next.attr("node-id").indexOf("L") === 0) {
            _this.changeDownDisabled = true
        } else {
            _this.changeDownDisabled = false
        }
	}
})
```
这样相当于将上移、下移按钮显示状态和实际与后台处理分开来做。只要上移、下移按钮可以点击，在函数moveUp和moveDown中不用再判断了，就可以直接处理
## 优化
上面代码仍然有问题，当有多个同级的根节点时，所有的根节点都不可以上下移
![](/assets/images/posts/jquery/1GkQh9f.png)
上图中管理工具-应用和管理工具-权限是同级的根节点，正常情况应该是管理工具-应用不能上移，但可以下移，而且在其下的子节点再创建子节点，也会有问题
![](/assets/images/posts/jquery/s2yPH2y.png)
企业形象（定制VI）正常应该是可以上下移的，此时只可以上移，而他同级的项目列表只可以下移
优化后代码：
```
$('#schedule_tree').treegrid({
	onClickRow: function (node) {
	    // 节点高亮选择时触发
        let selectRow = $('.datagrid-row-selected');
        // 此处获得之前所有同级有属性[node-id]的tr(有可能没有),关键
        let pre = selectRow.prevAll('tr[node-id]');
        // 此处获得之后所有同级有属性[node-id]的tr(有可能没有),关键
        let next = selectRow.nextAll('tr[node-id]');
        if (pre.length) {
            _this.changeUpDisabled = false
        } else {
            _this.changeUpDisabled = true
        }
        if (next.length) {
            _this.changeDownDisabled = false
        } else {
            _this.changeDownDisabled = true
        }
	}
})
```

## treegrid分级问题
- treegrid根据_parentId来分级
- 树子节点的_parentId和其父节点的id对应
- 根节点的_parentId为null
- 一个节点的id和_parentId不能相同
