---
layout: post
title: 多个相同的select，当其中一个select的option被选中时，其余的select对应的option为不可选状态
categories: JS
description: 多个相同的select，当其中一个select的option被选中时，其余的select对应的option为不可选状态
keywords: select, option
---

多个相同的select，当其中一个select的option被选中时，其余的select对应的option为不可选状态

```
var arr = [
	{name:"materialName",isSelected:false},
    {name:"unit",isSelected:false},
    {name:"realNum",isSelected:false},
    {name:"uploadDate",isSelected:false}]
$.each($('select'), function(){
  $(this).data("last", $(this).val()).change(function () { // 将默认值作为改变之前的值保存起来
  var $this = $(this)
  var oldvalue = $(this).data("last");//这次改变之前的值
  $(this).data("last", $(this).val()); //每次改变都附加上去，以便下次变化时获取
  var newvalue = $(this).val(); //当前选中值
  $.each(arr, function(i, e){
    if(oldvalue === e.name){
      e.isSelected = false
      $('select').not($this).children('option[value='+e.name+']').removeAttr('disabled')
    }
    if(newvalue === e.name){
      e.isSelected = true
      $('select').not($this).children('option[value='+e.name+']').attr('disabled', 'disabled')
    }
  })
})
})
```