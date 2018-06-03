---
layout: post
title: 大多数项目中会用到的 13 条 webpack 小技巧
categories: [JS]
description: 大多数项目中会用到的 webpack 小技巧
keywords: webpack
---

本文是作者对自己所学的 webpack 技巧的总结，在没有指定特殊情况下适用于 webpack 3.0 版本。

## 进度汇报

使用 `webpack --progress --colors` 这样可以让编译的输出内容带有进度和颜色。

## 压缩

在生产环境中构建项目时，使用
```js
webpack -p
```
这行代码在 webpack 2 中还会自动设置
```js
process.env.NODE_ENV === 'production'
```

## 复数文件打包

通过设置 output 属性为 `[name].js` 来导出复数包。下面的例子将会生成 `a.js` 和 `b.js`。
```js
module.exports = {
  entry: {
    a: './a',
    b: './b'
  },
  output: {filename: '[name].js' }
}
```
担心会重复打包？使用 [CommonsChunkPlugin](https://webpack.github.io/docs/list-of-plugins.html#commonschunkplugin) 来把通用部分移入一个新的输出文件中。
```js
plugins: [new webpack.optimize.CommonsChunkPlugin('init.js') ]
```

```html
<script src='init.js'></script>
<script src='a.js'></script>
```

## 分离 app 文件与第三方库文件

使用 CommonsChunkPlugin 将第三方代码移动到 vendor.js 中。
```js
var webpack = require('webpack')

module.exports = {
  entry: {
    app: './app.js',
    vendor: ['jquery', 'underscore', ...]
  },

  output: {
    filename: '[name].js'
  },

  plugins: [
    new webpack.optimize.CommonsChunkPlugin('vendor')
  ]
}
```

让我们来看看，CommonsChunkPlugin 是怎么起作用的:

- 我们指定了一个叫 vendor 的入口，并且它加载了 jquery 等第三方库。
- CommonsChunkPlugin 识别到这些第三方库在 app.js 中出现重复，便将 app.js 中的第三方库都移除。
- 在 vendor.js 中，CommonsChunkPlugin 还加入了 webpack 的运行时间。

> 参考链接：[Code splitting](https://webpack.github.io/docs/code-splitting.html#split-app-and-vendor-code)

## 资源映射 （webpack 1）

最好的资源映射选项是 `cheap-module-eval-source-map`。当使用 chrome/firefox 的开发者工具时，它会显示原始资源文件。另一方面，它比 `source-map` 和 `eval-source-map` 更快。
```js
// 只在 webpack 1 中有效
const DEBUG = process.env.NODE_ENV !== 'production'

module.exports = {
  debug: DEBUG ? true : false,
  devtool: DEBUG ? 'cheap-module-eval-source-map' : 'hidden-source-map'
}
```
你的文件在 chrome 开发者工具中显示为 `webpack:///foo.js?a93h`。如果我们希望文件名显示得更清晰呢，比如说 `webpack:///path/to/foo.js`？
```js
output: {
	devtoolModuleFilenameTemplate: 'webpack:///[absolute-resource-path]'
}
```
> 参考链接: [devtool documentation](https://webpack.github.io/docs/configuration.html#devtool)

## 资源映射（webpack 2-3）

在 webpack 2-3 版本中，最好的资源映射选项是 `cheap-module-source-map`，因为 cheap-module-eval-source-map 策略已经不能在 chrome/firefox 中显示正确的路径。

```js
const DEBUG = process.env.NODE_ENV !== 'production'

module.exports = {
  devtool: DEBUG ? 'cheap-module-source-map' : 'hidden-source-map'
}
```
如果你正在使用 [extract-text-webpack-plugin](https://www.npmjs.com/package/extract-text-webpack-plugin)，可以用 `'source-map'` 替代，否则 css 的资源映射会不起作用。

```js
// 只有当你使用 extract-text-webpack-plugin 时
module.exports = {
  devtool: DEBUG ? 'source-map' : 'hidden-source-map'
}
```

同样的， 想要 `webpack:///path/to/foo.js` 这样清晰的路径，我们可以写成下面这样：

```js
output: {
  devtoolModuleFilenameTemplate: 'webpack:///[absolute-resource-path]'
}
```

> 参考链接: [devtool documentation](https://webpack.js.org/configuration/devtool/#devtool)

## 输出 css 文件

这是一个复杂的过程，你可以在 [这里找到答案](recipes/css.md)。（译者注：这篇指南目前还没有翻译。）

## 开发模式

你想要某些配置只存在于开发环境中吗？
```js
const DEBUG = process.env.NODE_ENV !== 'production'

// Webpack 1
module.exports = {
  debug: DEBUG ? true : false,
  devtool: DEBUG ? 'cheap-module-eval-source-map' : 'hidden-source-map'
}

// Webpack 2
module.exports = {
  devtool: DEBUG ? 'cheap-module-source-map' : 'hidden-source-map'
}
```
在 webpack 1 中，打包你的项目资源时，通过 `env NODE_ENV=production webpack -p` 来调用 webpack 命令。
而在 webpack 2 中，只要 webpack -p 就可以了，因为 webpack 自动帮你设置了 `NODE_ENV`。

## 分析包的大小

你想知道资源包中有哪些 “重量级” 依赖吗？使用 `webpack-bundle-size-analyzer` 吧。
```js
$ yarn global add webpack-bundle-size-analyzer

$ ./node_modules/.bin/webpack --json | webpack-bundle-size-analyzer
jquery: 260.93 KB (37.1%)
moment: 137.34 KB (19.5%)
parsleyjs: 87.88 KB (12.5%)
bootstrap-sass: 68.07 KB (9.68%)
...
```
如果你正在生成资源映射，你也可以使用 source-map-explorer，它能够独立于 webpack 工作。
```js
$ yarn global add source-map-explorer

$ source-map-explorer bundle.min.js bundle.min.js.map
```
> 参考链接:
[webpack-bundle-size-analyzer](https://github.com/robertknight/webpack-bundle-size-analyzer)
[source-map-explorer](https://www.npmjs.com/package/source-map-explorer)

## 更小的 react 项目

react 会默认生成一些开发工具，而在生产环境中你并不需要它们。使用 EnvironmentPlugin 来让他们人道毁灭吧。这大概会节约 30kb 左右的空间。
```js
plugins: [
  new webpack.EnvironmentPlugin({
    NODE_ENV: 'development'
  })
]
```
在 webpack 1 中，使用 `env NODE_ENV=production webpack -p` 命令启动 webpack 来打包资源。而在 webpack 2 中，只要 `webpack -p` 就可以了，理由略。

> 参考链接: [EnvironmentPlugin documentation](https://webpack.js.org/plugins/environment-plugin/)

## 更小的 Lodash

[Lodash](https://lodash.com/) 非常有用，但是我们通常用到的只是其功能中的沧海一粟。 [lodash-webpack-plugin](https://github.com/lodash/lodash-webpack-plugin) 可以使用 [noop](https://lodash.com/docs#noop), [identity](https://lodash.com/docs#identity) 或其他更简化的选项来替换 [feature sets](https://github.com/lodash/lodash-webpack-plugin#feature-sets)，来帮助你减少 lodash 占用的空间。

```js
const LodashModuleReplacementPlugin = require('lodash-webpack-plugin');

const config = {
  plugins: [
    new LodashModuleReplacementPlugin({
      path: true,
      flattening: true
    })
  ]
};
```

这种方法可以帮助你省下至少 10kb。如果你的项目中 lodash 的比重很高，那你节省的资源会更多。

## 引入文件夹中所有文件

你是不是曾经尝试过下面的代码却发现不起作用？
```js
require('./behaviors/*')  /* 看似很正确 */
```
事实上，你应该使用 require.context。
```js
// http://stackoverflow.com/a/30652110/873870
function requireAll (r) { r.keys().forEach(r) }

requireAll(require.context('./behaviors/', true, /\.js$/))
```
> 参考链接: [require.context](http://webpack.github.io/docs/context.html#require-context)

## 清除 extract-text-webpack-plugin 日志

如果你在使用 [extract-text-webpack-plugin](https://www.npmjs.com/package/extract-text-webpack-plugin) 时看过下面的调试日志：
```
Child extract-text-webpack-plugin:
        + 2 hidden modules
Child extract-text-webpack-plugin:
        + 2 hidden modules
Child extract-text-webpack-plugin:
        + 2 hidden modules
```
你可以使用 `stats: {children: false}` 来关闭它：
```js
/* webpack.config.js */
stats: {
  children: false,
},
```
> 参考链接: [extract-text-webpack-plugin#35](https://github.com/webpack-contrib/extract-text-webpack-plugin/issues/35)

## 参考资料

- [原文地址](https://github.com/rstacruz/webpack-tricks)

## 总结

以上就是 [rstacruz](https://github.com/rstacruz) 总结的 13 条关于 webpack 的建议，这几乎是所有项目都用得到的 Webpack 配置技巧吧~
