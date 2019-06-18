---
layout: post
title: 如何优雅地链式取值
categories: [JS]
description: 如何优雅地链式取值
keywords: js
---

开发中, 链式取值是非常正常的操作, 如:

```js
res.data.goods.list[0].price
```

但是对于这种操作报出类似于 `Uncaught TypeError: Cannot read property 'goods' of undefined` 这种错误也是再正常不过了, 如果说是 res 数据是自己定义, 那么可控性会大一些, 但是如果这些数据来自于不同端(如前后端), 那么这种数据对于我们来说我们都是不可控的, 因此为了保证程序能够正常运行下去, 我们需要对此校验:

```js
if (res.data.goods.list[0] && res.data.goods.list[0].price) {
  // your code
}
```

如果再精细一点, 对于所有都进行校验的话, 就会像这样:

```js
if (
  res &&
  res.data &&
  res.data.goods &&
  res.data.goods.list &&
  res.data.goods.list[0] &&
  res.data.goods.list[0].price
) {
  // your code
}
```

不敢想象, 如果数据的层级再深一点会怎样, 这种实现实在是非常不优雅, 那么如果优雅地来实现链式取值呢?

## 一、 optional chaining

这是一个出于 stage 2 的 ecma 新语法, 目前已经有了 babel 的插件 [babel-plugin-transform-optional-chaining](https://www.npmjs.com/package/babel-plugin-transform-optional-chaining), 这种语法在 swift 中有, 可以看下官方给的实例

```js
a ? .b // undefined if `a` is null/undefined, `a.b` otherwise.
a == null ? undefined : a.b

a ? . [x] // undefined if `a` is null/undefined, `a[x]` otherwise.
a == null ? undefined : a[x]

a ? .b() // undefined if `a` is null/undefined
a == null ? undefined : a.b() // throws a TypeError if `a.b` is not a function
// otherwise, evaluates to `a.b()`

a ? .() // undefined if `a` is null/undefined
a == null ? undefined : a() // throws a TypeError if `a` is neither null/undefined, nor a function
// invokes the function `a` otherwise
```

## 二、 通过函数解析字符串

我们可以通过函数解析字符串来解决这个问题, 这种实现就是 lodash 的 `_.get` 方法

```js
var object = {
  a: [
    {
      b: {
        c: 3
      }
    }
  ]
}
var result = _.get(object, 'a[0].b.c', 1)
console.log(result)
// output: 3
```

这里还有一些其他的库, 如 `Typy` 和 `Ramda` , 可以做到这一点. 但是在轻量级前端项目中, 特别是如果你只需要这些库中的一两个方法时, 最好选择另一个轻量级库, 或者编写自己的库.

实现起来也非常简单, 只是简单的字符串解析而已:

```js
function get(obj, props, def) {
  if (obj == null || typeof props !== 'string') return def
  const temp = props.split('.')
  const fieldArr = [].concat(temp)
  temp.forEach((e, i) => {
    if (/^(\w+)\[(\w+)\]$/.test(e)) {
      const matchs = e.match(/^(\w+)\[(\w+)\]$/)
      const field1 = matchs[1]
      const field2 = matchs[2]
      const index = fieldArr.indexOf(e)
      fieldArr.splice(index, 1, field1, field2)
    }
  })
  return fieldArr.reduce((pre, cur) => {
    const target = pre[cur] || def
    if (target instanceof Array) {
      return [].concat(target)
    }
    if (target instanceof Object) {
      return Object.assign({}, target)
    }
    return target
  }, obj)
}
```

```js
var c = {
  a: {
    b: [1, 2, 3]
  }
}
get(c, 'a.b') // [1,2,3]
get(c, 'a.b[1]') // 2
get(c, 'a.d', 12) // 12
```

## 三、 使用解构赋值

这个思路是来自 github 上 [You-Dont-Need-Lodash-Underscore](https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore) 这个仓库, 看到这个的时候真的佩服

```js
const c = {
  a: {
    b: [1, 2, 3, 4]
  }
}

const { a: result } = c
// result : {b: [1,2,3,4]}
const {
  a: { c: result = 12 }
} = c
// result: 12
```

当然, 这个时候为了保证不报 uncaught TypeError, 我们仍然需要定义默认值, 就像这样, 貌似如果不加 lint 可读性堪忧

```js
const {
  a: { c: { d: result2 } = {} }
} = c
```

## 四、 使用 Proxy

这个是组内同事提到的, 一个简单实现如下:

```js
function pointer(obj, path = []) {
  return new Proxy(() => {}, {
    get(target, property) {
      return pointer(obj, path.concat(property))
    },
    apply(target, self, args) {
      let val = obj
      let parent
      for (let i = 0; i < path.length; i++) {
        if (val === null || val === undefined) break
        parent = val
        val = val[path[i]]
      }
      if (val === null || val === undefined) {
        val = args[0]
      }
      return val
    }
  })
}
```

我们可以这样使用:

```js
let c = {
  a: {
    b: [1, , 2, 3]
  }
}

pointer(c).a() // {b: [1,2,3]}

pointer(c).a.b() // [1,2,3]

pointer(d).a.b.d('default value') // default value
```

## 五、 Oliver Steele 的嵌套对象访问模式

这是我个人的最爱, 因为它使代码看起来干净简单. 我从 stackoverflow 中选择了这种风格, 一旦你理解它是如何工作的, 它就非常吸引人了.

```js
const name = ((user || {}).personalInfo || {}).name
```

使用这种表示法, 永远不会遇到无法读取未定义的属性 `name` . 做法是检查用户是否存在, 如果不存在, 就创建一个空对象, 这样, 下一个级别的键将始终从存在的对象访问.

不幸的是, 你不能使用此技巧访问**嵌套数组**.

## 六、 使用数组 Reduce 访问嵌套对象

Array reduce 方法非常强大, 可用于安全地访问嵌套对象.

```js
const getNestedObject = (nestedObj, pathArr) => {
  return pathArr.reduce(
    (obj, key) => (obj && obj[key] !== 'undefined' ? obj[key] : null),
    nestedObj
  )
}

// 将对象结构作为数组元素传入
const name = getNestedObject(user, ['personalInfo', 'name'])

// 要访问嵌套数组，只需将数组索引作为数组元素传入。.
const city = getNestedObject(user, ['personalInfo', 'addresses', 0, 'city'])
// 这将从 addresses 中的第一层返回 city
```

这差不多就是心中所谓的优雅了.

综上, 在实际工作中, 使用方法四会是最优雅, 可读性也非常强, 但考虑到浏览器的话, 可能方法二会更加常用, 当然, 如果你所要取的值层级不是太深, 你组内的同事要严格的 lint, 方法三也不失为一种好的选择.

## 参考资料

- [如何优雅地链式取值 - 掘金](https://juejin.im/post/5ba08483e51d450e99430a7f)
- [如何在 JavaScript 中访问暂未存在的嵌套对象 - 掘金](https://juejin.im/post/5d0824c0f265da1bd04ee1bb)
