---
layout: post
title: 前端简洁并实用的工具类
categories: [JS]
description: 前端简洁并实用的工具类
keywords: js, tool
---

本文主要从日期,数组,对象,axios,promise 和字符判断这几个方面讲工作中常用的一些函数进行了封装,确实可以在项目中直接引用,提高开发效率.

## 1.日期

日期在后台管理系统还是用的很多的,一般是作为数据存贮和管理的一个维度,所以就会涉及到很多对日期的处理

### 1.1 element-UI 的日期格式化

![](/assets/images/posts/js/3804807279-5a9526c3b7f16_articlex.png)

DatePicker 日期选择器默认获取到的日期默认是 Date 对象,但是我们后台需要用到的是 yyyy-MM-dd,所以需要我们进行转化

方法一:转化为 dd-MM-yyyy HH:mm:ss
```
export const dateReurn1 = date1 => date1.toLocaleString("en-US", { hour12: false }).replace(/\b\d\b/g, '0$&').replace(new RegExp('/','gm'),'-')
```
方法二:

从 element-UI 的 2.x 版本提供了 value-format 属性,可以直接设置选择器返回的值

![](/assets/images/posts/js/41132791-5a95295fb50a2_articlex.png)

### 1.2 获取当前的时间 yyyy-MM-dd HH:mm:ss

没有满 10 就补 0
```
export default const obtainDate = () => {
  let date = new Date();
  let year = date.getFullYear();
  let month = date.getMonth() + 1;
  let day=date.getDate();
  let hours=date.getHours();
  let minu=date.getMinutes();
  let second=date.getSeconds();
  //判断是否满10
  let arr=[month,day,hours,minu,second];
  arr.forEach( item => {
    item < 10 ? '0' + item : item;
  })
  return `${year}-${arr[0]}-${arr[1]} ${arr[2]}:${arr[3]}:${arr[4]}`
}
```

## 2.数组

### 2.1 检测是否是数组

```
export default const judgeArr = arr => Array.isArray(arr)
```

### 2.2数组去重 set 方法

1. 常见利用循环和 indexOf (ES5的数组方法,可以返回值在数组中第一次出现的位置)这里就不再详写,这里介绍一种利用 ES6 的 set 实现去重.
2. set 是新增数据结构, 似于数组，但它的一大特性就是所有元素都是唯一的.
3. set常见操作, 大家可以参照下面这个:[新增数据结构 Set 的用法](https://www.cnblogs.com/kongxianghai/p/7250248.html)
4. set 去重代码

```
let arr = [1,2,2,3,5,4,5]
export const changeReArr = arr => Array.from(new Set(arr)) // 利用 set 将 [1,2,2,3,5,4,5] 转化成 set 数据, 利用 array from 将 set 转化成数组类型
```
或者
```
export const changeReArr = arr => [...new Set(arr)] // 利用 ... 扩展运算符将 set 中的值遍历出来重新定义一个数组, ... 是利用 for...of 遍历的
```
Array.from 可以把带有 lenght 属性类似数组的对象转换为数组，也可以把字符串等可以遍历的对象转换为数组，它接收 2 个参数，转换对象与回调函数, ... 和 Array.from 都是 ES6 的方法

### 2.3 纯数组排序

常见有冒泡和选择, 这里我写一下利用 sort 排序
```
export const orderArr = arr => arr.sort((a, b) => a - b) // 将 arr 升序排列,如果是倒序 -(a - b)
```

### 2.4 数组对象排序

```
export const orderArr = arr => arr.sort((a,b) => {
  let value1 = a[property];
  let value2 = b[property];
  return value1 - value2;
  //sort 方法接收一个函数作为参数，这里嵌套一层函数用
  //来接收对象属性名，其他部分代码与正常使用 sort 方法相同
  //注：这里我验证了一下，会报 property is not defined
  // arr = [{q:1},{w:23},{c:4}]
})
```

### 2.5 数组的"短路运算" every 和 some

数组短路运算这个名字是我自己加的,因为一般有这样一种需求,一个数组里面某个或者全部满足条件,就返回 true

情况一:全部满足
```
export const allTrueArr = arrs => arrs.every(arr => arr > 20) // 如果数组的每一项都满足则返回 true,如果有一项不满足返回 false,终止遍历
```
情况二:有一个满足
```
export default const OneTrueArr= arrs => arrs.some(arr => arr>20) // 如果数组有一项满足则返回 true,终止遍历,每一项都不满足则返回 false
```

以上两种情景就和 `||` 和 `&&` 的短路运算很相似,所以我就起了一个名字叫短路运算,当然两种情况都可以通过遍历去判断每一项然后用 break 和 return false 结束循环和函数.

## 3.对象

### 3.1 对象遍历
```
export const traverseObj = obj => {
  for(let variable in obj){
    // For…in 遍历对象包括所有继承的属性,所以如果
    // 只是想使用对象本身的属性需要做一个判断
    if (obj.hasOwnProperty(variable)) {
      console.log(variable, obj[variable])
    }
  }
}
```
### 3.2 对象的数据属性

1. 对象属性分类:数据属性和访问器属性;
2. 数据属性:包含数据值的位置,可读写,包含四个特性：

    - configurable：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或能否把属性修改为访问器属性，默认为 true
    - enumerable:表示能否通过 for-in 循环返回属性
    - writable：表示能否修改属性的值
    - value：包含该属性的数据值。默认为 undefined

3. 修改数据属性的默认特性,利用 Object.defineProperty()

```
export const modifyObjAttr = () => {
  let person = {name:'张三',age:30};
  Object.defineProperty(person,'name',{
    writable:false,
    value:'李四',
    configurable:false,//设置false就不能对该属性修改
    enumerable:false
  })
} 
```

### 3.3 对象的访问器属性

1. 访问器属性的四个特性:

    - configurable：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或能否把属性修改为访问器属性，默认为false
    - enumerable:表示能否通过 for-in 循环返回属性,默认为 false
    - Get：在读取属性时调用的函数,默认值为 undefined
    - Set：在写入属性时调用的函数,默认值为 undefined 

2. 定义:

    访问器属性只能通过要通过 Object.defineProperty() 这个方法来定义

```
export const defineObjAccess = () => {
  let personAccess = {
    _name:'张三',//_表示是内部属性,只能通过对象的方法修改
    editor:1
  }
  Object.defineProperty(personAccess,'name',{
    get:function(){
      return this._name;
    },
    set:function(newName){
      if(newName !== this._name){
        this._name = newName;
        this.editor++;
      }
    }
    //如果只定义了get方法则改对象只能读
  })
}
``` 
vue中最核心的响应式原理的核心就是通过 defineProperty 来劫持数据的 getters 和 setter 属性来改变数据的




## 参考资料

- [前端简洁并实用的工具类](https://segmentfault.com/a/1190000013438501#articleHeader10)