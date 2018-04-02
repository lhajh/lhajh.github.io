---
layout: post
title: js 实现随机颜色的 3 种方法与颜色格式的转化
categories: [JS]
description: js 实现随机颜色的 3 种方法与颜色格式的转化
keywords: js, rgb, hex, color
---

相信大家都知道在前端的颜色表示方式有多种，一种是以 3 个或 6 个十六进制的数子表示，一种是 RGB 的数字形式，还有一种是直接以颜色的英文来表示。这三种都是不支持透明色的。所以还有 RGBA 的表式方式，在 RGB 的的基础上加入了 Alpha 透明，使网页可以展现更加复杂绚丽的效果。

在平时的码农日常中，经常会用到求随机颜色的地方，下面是我总结的几种简单的实现随机颜色的方式：

## 随机颜色

### 十六进制格式（#000000-#FFFFFF）

第一种是比较简单的方法，这种方法是先随机生成 ffffff 以内 16 进制数，然后判断位数，少于 6 位的用 while 循环在前面加 0，凑够 6 位。

```
function randomHexColor() { //随机生成十六进制颜色
 var hex = Math.floor(Math.random() * 16777216).toString(16); //生成ffffff以内16进制数
 while (hex.length < 6) { //while循环判断hex位数，少于6位前面加0凑够6位
  hex = '0' + hex;
 }
 return '#' + hex; //返回‘#'开头16进制颜色
}
```
还有一种比较方便但是比较难懂的方法，需要用到位运算。

```
function randomHexColor() { //随机生成十六进制颜色
 return '#' + ('00000' + (Math.random() * 0x1000000 << 0).toString(16)).substr(-6);
}
```
按执行顺序可以分为以下六步：

1. 先执行 Math.random() * 0x1000000，其中 0x1000000=0xffffff+1，因为 Math.random() 取不到 1，所以 +1，这样就会生成一个 1-16777216 (不包含)以内的浮点数。
2. 然后执行 <<0，这是取整运算，去掉后面的小数点。这时为一个 16777216 (不包含)以内的十进制数。
3. 之后执行 .toString(16) ，把十进制数转化为六位以下 16 进制数。
4. 再后执行 '00000'+，这时因为之前生成的 16 进制数最少可能仅一位，在前面加上 5 个 0。
5. 最后执行 .substr(-6) ，是去从 -6 开始的后面所有字符串，也就是最后 6 位数。
6. 前面加上 # 并 retuen。

### RGB 格式

```
function randomRgbColor() { //随机生成RGB颜色
 var r = Math.floor(Math.random() * 256); //随机生成256以内r值
 var g = Math.floor(Math.random() * 256); //随机生成256以内g值
 var b = Math.floor(Math.random() * 256); //随机生成256以内b值
 return `rgb(${r},${g},${b})`; //返回rgb(r,g,b)格式颜色
}
```

### RGBA 格式

```
function randomRgbaColor() { //随机生成RGBA颜色
 var r = Math.floor(Math.random() * 256); //随机生成256以内r值
 var g = Math.floor(Math.random() * 256); //随机生成256以内g值
 var b = Math.floor(Math.random() * 256); //随机生成256以内b值
 var alpha = Math.random(); //随机生成1以内a值
 return `rgb(${r},${g},${b},${alpha})`; //返回rgba(r,g,b,a)格式颜色
}
```

## 颜色格式转化

### 十六进制转为 RGB

```
function hex2Rgb(hex) { //十六进制转为RGB
 var rgb = []; // 定义rgb数组
 if (/^\#[0-9A-F]{3}$/i.test(hex)) { //判断传入是否为#三位十六进制数
  let sixHex = '#';
  hex.replace(/[0-9A-F]/ig, function(kw) {
   sixHex += kw + kw; //把三位16进制数转化为六位
  });
  hex = sixHex; //保存回hex
 }
 if (/^#[0-9A-F]{6}$/i.test(hex)) { //判断传入是否为#六位十六进制数
  hex.replace(/[0-9A-F]{2}/ig, function(kw) {
   rgb.push(eval('0x' + kw)); //十六进制转化为十进制并存如数组
  });
  return `rgb(${rgb.join(',')})`; //输出RGB格式颜色
 } else {
  console.log(`Input ${hex} is wrong!`);
  return 'rgb(0,0,0)';
 }
}
```

### RGB 转为十六进制

```
function rgb2Hex(rgb) {
 if (/^rgb\((\d{1,3}\,){2}\d{1,3}\)$/i.test(rgb)) { //test RGB
  var hex = '#'; //定义十六进制颜色变量
  rgb.replace(/\d{1,3}/g, function(kw) { //提取rgb数字
   kw = parseInt(kw).toString(16); //转为十六进制
   kw = kw.length < 2 ? 0 + kw : kw; //判断位数，保证两位
   hex += kw; //拼接
  });
  return hex; //返回十六进制
 } else {
  console.log(`Input ${rgb} is wrong!`);
  return '#000'; //输入格式错误,返回#000
 }
}
```

# 参考资料

- [JS实现随机颜色的3种方法与颜色格式的转化](http://www.jb51.net/article/102109.htm)