---
layout: post
title: Vue字面量语法 vs 动态语法
categories: Vue
description: 字面量语法 vs 动态语法
keywords: Vue, 字面量语法, 动态语法 
---

Vue中字面量语法和动态语法的写法

## 字面量语法 vs 动态语法
初学者常犯的一个错误是使用字面量语法传递数值：
```
<!-- 传递了一个字符串"1" -->
<comp some-prop="1"></comp>
```
因为它是一个字面 prop ，它的值以字符串 "1" 而不是以实际的数字传下去，此时会报错：`Expected Number, got String.` 如果想传递一个实际的 JavaScript 数字，需要使用 v-bind ，从而让它的值被当作 JavaScript 表达式计算：
```
<!-- 传递实际的数字 -->
<comp v-bind:some-prop="1"></comp>
<!-- 简写形式 -->
<comp :some-prop="1"></comp>
```
传递布尔值类似