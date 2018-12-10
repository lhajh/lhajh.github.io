---
layout: post
title: 那些年被我们忽略的 vue 语法
categories: [Vue]
description: 那些年被我们忽略的 vue 语法
keywords: vue
---

使用 vue 这么长时间了，vue 所有的语法你都用过了吗？如果感觉自己还没有完全了解 vue，这篇文章是你不二选择，当然，你如果想要了解基础部分，出门左转不谢

说正事之前，先上一个小插曲，我们都知道 a 链接的 href 如果是 `#+id`，那么点击链接会直接将包含该 id 的元素滚动到页面顶部，但我们页面一般都有 header，会遮挡一部分该元素，ElementUI 巧妙的运用伪类解决了此问题

```css
.content h2:before,
.content h3:before {
  content: '';
  display: block;
  margin-top: -91px;
  height: 91px;
  visibility: hidden;
}
```

想一探究竟的请移步官网 F12 查看

好，正片开始！！！

## [多重值](https://cn.vuejs.org/v2/guide/class-and-style.html#%E5%A4%9A%E9%87%8D%E5%80%BC)

从 2.3.0 起你可以为  `style`  绑定中的属性提供一个包含多个值的数组，常用于提供多个带前缀的值，例如：

```html
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

这样写只会渲染数组中最后一个被浏览器支持的值。在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染  `display: flex`。

## [在 `template` 元素上使用  `v-if`  条件渲染分组](https://cn.vuejs.org/v2/guide/conditional.html#%E5%9C%A8-lt-template-gt-%E5%85%83%E7%B4%A0%E4%B8%8A%E4%BD%BF%E7%94%A8-v-if-%E6%9D%A1%E4%BB%B6%E6%B8%B2%E6%9F%93%E5%88%86%E7%BB%84)

具体例子请点击上面标题链接查看

因为  `v-if`  是一个指令，所以必须将它添加到一个元素上。但是如果想切换多个元素呢？此时可以把一个  `<template>`  元素当做不可见的包裹元素，并在上面使用  `v-if`。最终的渲染结果将不包含  `<template>`  元素。

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

## [`v-for` on a `template`](https://cn.vuejs.org/v2/guide/list.html#v-for-on-a-lt-template-gt)

类似于 `v-if`，你也可以利用带有 `v-for` 的 `<template>` 渲染多个元素。比如：

```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```

## [用  `key`  管理可复用的元素](https://cn.vuejs.org/v2/guide/conditional.html#%E7%94%A8-key-%E7%AE%A1%E7%90%86%E5%8F%AF%E5%A4%8D%E7%94%A8%E7%9A%84%E5%85%83%E7%B4%A0)

Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做除了使 Vue 变得非常快之外，还有其它一些好处。例如，如果你允许用户在不同的登录方式之间切换：

```html
<template v-if="loginType === 'username'">
  <label>Username</label> <input placeholder="Enter your username" />
</template>
<template v-else>
  <label>Email</label> <input placeholder="Enter your email address" />
</template>
```

那么在上面的代码中切换  `loginType`  将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，`<input>`  不会被替换掉——仅仅是替换了它的  `placeholder`。

这样也不总是符合实际需求，所以 Vue 为你提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的  `key`  属性即可：

```html
<template v-if="loginType === 'username'">
  <label>Username</label> <input placeholder="Enter your username" key="username-input" />
</template>
<template v-else>
  <label>Email</label> <input placeholder="Enter your email address" key="email-input" />
</template>
```

现在，每次切换时，输入框都将被重新渲染。

本人曾经遇到过一个类似的问题，不过是表格的，感兴趣的可以[看看](https://lhajh.github.io/ui/2017/09/15/elementui-problem-collection.html)

官网对 [`key`](https://cn.vuejs.org/v2/guide/list.html#key) 的解释

## [`v-show`](https://cn.vuejs.org/v2/guide/conditional.html#v-show)

另一个用于根据条件展示元素的选项是  `v-show`  指令。用法大致一样：

```html
<h1 v-show="ok">Hello!</h1>
```

不同的是带有  `v-show`  的元素始终会被渲染并保留在 DOM 中。`v-show`  只是简单地切换元素的 CSS 属性  `display`。

注意，`v-show`  不支持  `<template>`  元素，也不支持  `v-else`。

## [`v-if` vs `v-show`](https://cn.vuejs.org/v2/guide/conditional.html#v-if-vs-v-show)

`v-if`  是“真正”的条件渲染，因为它会确保在切换过程中条件块内的**事件监听器和子组件适当地被销毁和重建**。

`v-if`  也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show`  就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if`  有更高的切换开销，而  `v-show`  有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用  `v-show`  较好；如果在运行时条件很少改变，则使用  `v-if`  较好。

## [用  `v-for`  把一个数组对应为一组元素](https://cn.vuejs.org/v2/guide/list.html#%E7%94%A8-v-for-%E6%8A%8A%E4%B8%80%E4%B8%AA%E6%95%B0%E7%BB%84%E5%AF%B9%E5%BA%94%E4%B8%BA%E4%B8%80%E7%BB%84%E5%85%83%E7%B4%A0)

我们用  `v-for`  指令根据一组数组的选项列表进行渲染。`v-for`  指令需要使用  `item in items`  形式的特殊语法，`items`  是源数据数组并且  `item`  是数组元素迭代的别名。

你也可以用  `of`  替代  `in`  作为分隔符，因为它是最接近 JavaScript 迭代器的语法：

```html
<div v-for="item of items"></div>
```

## [一个对象的  `v-for`](https://cn.vuejs.org/v2/guide/list.html#%E4%B8%80%E4%B8%AA%E5%AF%B9%E8%B1%A1%E7%9A%84-v-for)

你也可以用  `v-for`  通过一个对象的属性来迭代。

```html
<div v-for="(value, key, index) in object">{{ index }}. {{ key }}: {{ value }}</div>
```

在遍历对象时，是按  `Object.keys()`  的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下是一致的。

### [数组更改检测注意事项](https://cn.vuejs.org/v2/guide/list.html#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)

由于 JavaScript 的限制，Vue 不能检测以下变动的数组：

1. 当你利用索引直接设置一个项时，例如：`vm.items[indexOfItem] = newValue`
2. 当你修改数组的长度时，例如：`vm.items.length = newLength`

举个例子：

```js
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```

为了解决第一类问题，以下两种方式都可以实现和  `vm.items[indexOfItem] = newValue`  相同的效果，同时也将触发状态更新：

```js
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)

// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
```

你也可以使用  [`vm.$set`](https://vuejs.org/v2/api/#vm-set)  实例方法，该方法是全局方法  `Vue.set`  的一个别名：

```js
vm.$set(vm.items, indexOfItem, newValue)
```

为了解决第二类问题，你可以使用  `splice`：

```js
vm.items.splice(newLength)
```

## [对象更改检测注意事项](https://cn.vuejs.org/v2/guide/list.html#%E5%AF%B9%E8%B1%A1%E6%9B%B4%E6%94%B9%E6%A3%80%E6%B5%8B%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)

还是由于 JavaScript 的限制，**Vue 不能检测对象属性的添加或删除**：

```js
var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` 现在是响应式的

vm.b = 2
// `vm.b` 不是响应式的
```

对于已经创建的实例，Vue 不能动态添加根级别的响应式属性。但是，可以使用  `Vue.set(object, key, value)`  方法向嵌套对象添加响应式属性。例如，对于：

```js
var vm = new Vue({
  data: {
    userProfile: {
      name: 'Anika'
    }
  }
})
```

你可以添加一个新的  `age`  属性到嵌套的  `userProfile`  对象：

```js
Vue.set(vm.userProfile, 'age', 27)
```

你还可以使用  `vm.$set`  实例方法，它只是全局  `Vue.set`  的别名：

```js
vm.$set(vm.userProfile, 'age', 27)
```

有时你可能需要为已有对象赋予多个新属性，比如使用  `Object.assign()`  或  `_.extend()`。在这种情况下，你应该用两个对象的属性创建一个新的对象。所以，如果你想添加新的响应式属性，不要像这样：

```js
Object.assign(vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

你应该这样做：

```js
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

## [显示过滤/排序结果](https://cn.vuejs.org/v2/guide/list.html#%E6%98%BE%E7%A4%BA%E8%BF%87%E6%BB%A4-%E6%8E%92%E5%BA%8F%E7%BB%93%E6%9E%9C)

有时，我们想要显示一个数组的过滤或排序副本，而不实际改变或重置原始数据。在这种情况下，可以创建返回过滤或排序数组的计算属性。

例如：

```html
<li v-for="n in evenNumbers">{{ n }}</li>
```

```js
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

在计算属性不适用的情况下 (例如，在嵌套  `v-for`  循环中) 你可以使用一个 method 方法：

```html
 <li v-for="n in even(numbers)">{{ n }}</li>
```
```js
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
 ```
