---
layout: post
title: 解决 vuex requires a Promise polyfill in this browser 问题
categories: [Vue]
description: 解决 vuex requires a Promise polyfill in this browser 问题
keywords: vue, Promise, babel-polyfill
---

造成这种现象的原因归根究底就是浏览器对 ES6 中的 promise 无法支持，因此需要通过引入 babel-polyfill 来使我们的浏览器正常使用 es6 的功能

首先通过npm来安装:

`npm i babel-polyfill -D`

接下来就是根据场景来引入

目前本喵遇到的出现这种错误的场景有两种：

1. 在使用 vue-cli 搭建的 unit 测试时(npm run unit)，因为测试时启动的浏览器不是我们常用的 chrome，而是 PhantomJs。为了能让其像 chrome 一样正常运转，需要在 kara.confi.js 中设置其在启动我们程序的入口文件前，先启动 polyfill.js，配置部分如下：

    `files: ['../../node_modules/babel-polyfill/dist/polyfill.js','./index.js'],`

2. 在 ie 下运行时，也会出现同样的报错，解决方式类似，不过这次是在 webpack.base.conf.js 中配置:：

    ps：这里在网上看到过三种配置方案:

    第一种:
    ```
    entry: {
      app: ["babel-polyfill","./src/main.js"]
    }
    ```            

    第二种:
    ```
    entry: { 
      app: "./src/main.js",
      "babel-polyfill":"babel-polyfill"
    }
    ```

    第三种:在 main.js 中全局 import babel-polyfill

    不知是否本喵是个例，以上方法均扑街.

    最后使用直接引入 node_modules 中的 js 文件路径，最终成功，代码如下:
    ```
    entry: {
      app: ['./node_modules/babel-polyfill/dist/polyfill.js','./src/main.js']
    },
    ```