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

let _this = this
$('#schedule_tree').treegrid({
	onClickRow: function (node) {
	// 节点高亮选择时触发
        let selectRow = $('.datagrid-row-selected');
        let pre = selectRow.prev(); // 此处获得上一节点，关键
        let next = selectRow.next(); // 此处获得下一节点，关键
        if (typeof(pre.attr("node-id")) === "undefined" || pre.attr("node-id").indexOf("L") === 0) {
	    // 由于这里的this指向不是Vue的实例对象，所以在外面将_this指向Vue的实例对象，下同
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

## 移动问题

现在做上下移的思路：当点击某一个节点时，判断是否可以上下移，如果可以上下移，发送请求进行上下移，成功后重新获取数据渲染Treegrid。但现在有两个问题：
1. 上下移成功后，没有继续判断是否可以移动，有时候明明已经到顶端或低端，还显示可以移动
2. 移动成功后，当前选中列没有跟着移动。如现在有两列，选中第二列(选中列颜色会变化)，上移后，选中列选中的是移动后的第一列。

## 解决问题

思路：
1. 上下移成功后，重新执行上面onClickRow的函数，这需要将函数提取出来，在onClickRow中需要调用，重新获取数据后也需要调用，而且要传递this
2. 移动成功后，手动修改选中列，即修改class值

## 完成移动

```
// 获取进度填报列表
fetchScheduleList() {
    this.loading = true
    this.$get(url, {
        tenantId: this.tenantId
    }).then((res) => {
        $('#schedule_tree').treegrid('loadData', res);
        // 成功后调用函数判断是否可以上下移
        this.changeUpDown(this)
        this.loading = false;
    }).catch((mes) => {
        this.$message.error(mes.message)
        this.loading = false;
    })
}
/**
 * 上下移状态
 * @param _this Vue实例对象
 */
changeUpDown(_this) {
    let selectRow = $('.datagrid-row-selected');
    if (selectRow.length) {
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
    } else {
        _this.changeUpDisabled = _this.changeDownDisabled = true;
    }
},
moveUp() {
    let selectRow = $('.datagrid-row-selected');
    let scheduleId, replaceId
    if (selectRow.length) {
        scheduleId = selectRow[0].id.substring(selectRow[0].id.lastIndexOf('-') + 1)
        let pre = selectRow.prevAll('tr[node-id]');
        if (pre.length) {
            replaceId = pre[0].id.substring(pre[0].id.lastIndexOf('-') + 1)
        } else {
            replaceId = ''
        }
    }
    this.$get(url, {
        scheduleId,
        replaceId,
        moveType: 0
    }).then((res) => {
        this.fetchScheduleList()
        this.$message({
            type: 'success',
            message: '上移成功'
        });
        setTimeout(() => {
	    // 手动修改选中列，如果不延迟，选中列的颜色会先移动，然后需要移动的列才会移动
            $('#schedule_tree').treegrid('select', replaceId)
        }, 30)
    }).catch((error) => {
        console.log(error)
    });
},
moveDown() {
    let selectRow = $('.datagrid-row-selected');
    let scheduleId, replaceId
    if (selectRow.length) {
        scheduleId = selectRow[0].id.substring(selectRow[0].id.lastIndexOf('-') + 1)
        let next = selectRow.nextAll('tr[node-id]')
        if (next.length) {
            replaceId = next[0].id.substring(next[0].id.lastIndexOf('-') + 1)
        } else {
            replaceId = ''
        }
    }
    this.$get(url, {
        scheduleId,
        replaceId,
        moveType: 1
    }).then((res) => {
        this.fetchScheduleList()
        this.$message({
            type: 'success',
            message: '下移成功'
        });
        setTimeout(() => {
            $('#schedule_tree').treegrid('select', replaceId)
        }, 30)
    }).catch((error) => {
        console.log(error)
    });
}

$('#schedule_tree').treegrid({
	onClickRow: function (node) {
	    _this.changeUpDown(_this)
	}
})
```

## treegrid分级问题
- treegrid根据_parentId来分级
- 树子节点的_parentId和其父节点的id对应
- 根节点的_parentId为null
- 一个节点的id和_parentId不能相同
