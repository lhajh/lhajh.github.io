---
layout: post
title: 深入学习JavaScript apply call 方法
categories: [JS]
description: 深入学习JavaScript apply call 方法
keywords: js, call, apply
---

JavaScript 中通过 call 或者 apply 用来代替另一个对象调用一个方法，将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。简单的说就是改变函数执行的上下文，这是最基本的用法。

本文主要解决以下几个问题：

1. apply 和 call 的区别在哪里
2. 什么情况下用 apply,什么情况下用 call
3. apply 的其他巧妙用法（一般在什么情况下可以使用 apply）

## 定义

apply: 能劫持另外一个对象的方法，继承另外一个对象的属性.

Function.apply(obj,args)方法能接收两个参数

- obj：这个对象将代替 Function 类里 this 对象
- args：这个是数组，它将作为参数传给 Function（args-->arguments）

call: 和 apply 的意思一样,只不过是参数列表不一样.

Function.call(obj,[param1[,param2[,…[,paramN]]]])

- obj：这个对象将代替 Function 类里 this 对象
- params：这个是一个参数列表

## apply 示例

```
<script type="text/javascript">  
  /*定义一个人类*/  
  function Person (name,age) {  
    this.name=name;  
    this.age=age;  
  }  
  /*定义一个学生类*/  
  function Student (name,age,grade) {  
    Person.apply(this,arguments);  
    this.grade=grade;  
  }  
  //创建一个学生类  
  var student=new Student("zhangsan",21,"一年级");  
  //测试  
  console.log("name:"+student.name+"\n"+"age:"+student.age+"\n"+"grade:"+student.grade);  
  //大家可以看到测试结果name:zhangsan age:21  grade:一年级  
  //学生类里面我没有给name和age属性赋值啊,为什么又存在这两个属性的值呢,这个就是apply的神奇之处.  
</script> 
```

分析: Person.apply(this,arguments);

this:在创建对象在这个时候代表的是 student

arguments:是一个数组,也就是[“zhangsan”,”21”,”一年级”];

也就是通俗一点讲就是:用 student 去执行 Person 这个类里面的内容,在 Person 这个类里面存在 this.name 等之类的语句,这样就将属性创建到了 student 对象里面

## call示例

在 Studen 函数里面可以将 apply 中修改成如下:

Person.call(this,name,age);

## 什么情况下用 apply,什么情况下用 call

在给对象参数的情况下,如果参数的形式是数组的时候,比如 apply 示例里面传递了参数 arguments,这个参数是数组类型,并且在调用 Person 的时候参数的列表是对应一致的(也就是 Person 和 Student 的参数列表前两位是一致的) 就可以采用 apply , 如果我的 Person 的参数列表是这样的(age,name),而 Student 的参数列表是(name,age,grade),这样就可以用 call 来实现了,也就是直接指定参数列表对应值的位置(Person.call(this,age,name,grade));

## apply 的一些其他巧妙用法

细心的人可能已经察觉到,在我调用 apply 方法的时候,第一个参数是对象(this), 第二个参数是一个数组集合, 在调用 Person 的时候,他需要的不是一个数组,但是为什么他给我一个数组我仍然可以将数组解析为一个一个的参数,这个就是 apply 的一个巧妙的用处,可以将一个数组默认的转换为一个参数列表([param1,param2,param3] 转换为 param1,param2,param3) 这个如果让我们用程序来实现将数组的每一个项转换为参数列表,可能都得费一会功夫,借助 apply 的这点特性,所以就有了以下高效率的方法:

### Math.max 可以实现得到数组中最大的一项

因为 Math.max 参数里面不支持 Math.max([param1,param2]) 也就是数组

但是它支持 Math.max(param1,param2,param3…),所以可以根据刚才 apply 的那个特点来解决 

`var max=Math.max.apply(null,array)`,这样轻易的可以得到一个数组中最大的一项(apply 会将一个数组装换为一个参数接一个参数的传递给方法)

这块在调用的时候第一个参数给了一个 null,这个是因为没有对象去调用这个方法,我只需要用这个方法帮我运算,得到返回的结果就行,所以直接传递了一个 null 过去

### Math.min  可以实现得到数组中最小的一项

同样和 max 是一个思想 `var min=Math.min.apply(null,array);`

### Array.prototype.push 可以实现两个数组合并

同样 push 方法没有提供 push 一个数组,但是它提供了 push(param1,param,…paramN) 所以同样也可以通过 apply 来转换一下这个数组,即:

```
var arr1=new Array("1","2","3");  
  
var arr2=new Array("4","5","6");  
  
Array.prototype.push.apply(arr1,arr2);  
```
也可以这样理解,arr1 调用了 push 方法,参数是通过 apply 将数组装换为参数列表的集合.

### 通常在什么情况下,可以使用 apply 类似 Math.min 等之类的特殊用法:

一般在目标函数只需要 n 个参数列表,而不接收一个数组的形式（[param1[,param2[,…[,paramN]]]]）,可以通过 apply 的方式巧妙地解决这个问题!

## 参考资料

- [js中apply使用方法小议](http://www.cnblogs.com/xiaohongwu/archive/2011/06/15/2081237.html)
- [Js apply 方法 详解](https://blog.csdn.net/myhahaxiao/article/details/6952321)