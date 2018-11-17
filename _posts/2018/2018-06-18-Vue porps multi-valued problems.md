---
layout: post
title: vue props 传多值的问题
categories: Vue
description: vue props 传多值的问题
keywords: vue, props
---

一般使用 `vue` 父传子值时, 都是 `:a="a" :b="b"`, 但你见过这种 `:a="a, b, c, d..."` 吗?

废话不多说, 直接看代码:

父组件 `data`:

```js
data () {
  return {
    str: 'str',
    num: 123,
    boolean: true,
    und: undefined,
    nul: null,
    obj: {
      a: 1,
      b: 2
    },
    arr: [1, 2, 3]
  }
}
```

给子组件传值:

```html
<cockpit-ceair
  :str="str, num, boolean, und, nul, obj, arr"
></cockpit-ceair>
```

子组件 `props` 接受值:

```js
props: ['str', 'num', 'boolean', 'und', 'nul', 'obj', 'arr']
```

子组件显示值:

```html
<!-- github 渲染不出来 {}, 下面只能使用 [] 代替 {} 了, 意思到了就行 -->
<div>
  str---[[str]]
  num---[[num]]
  boolean---[[boolean]]
  und---[[und]]
  nul---[[nul]]
  obj---[[obj]]
  arr---[[arr]]
</div>
```

子组件渲染后:

![](/assets/images/posts/vue/803185001.png)

`undefined` 和 `null` 浏览器渲染为空值就不具体多说了, 但值也传过来了, 如果转换成字符串也可以显示

是不是很震惊. 居然只要一个变量就可以传递多个变量

可能有的小伙伴认为是解构赋值, 但下面这种 `props` 写法呢?

```js
props: {
  str: {
    type: String,
    default: ''
  },
  num: Number,
  boolean: {
    type: Boolean,
    default: false
  },
  und: {
    type: undefined,
    default: undefined
  },
  nul: null,
  obj: {
    type: Object,
    default () {
      return {}
    }
  },
  arr: {
    type: Array,
    default () {
      return []
    }
  }
}
```

这个具体原因目前还没有搞清楚, 而且 `vue` 官方关于 `props` 传值也没有这种写法, 可能得需要阅读 `vue` 源码来看看 `props` 到底是如何解析传过来的值后才可能知道原理了吧
