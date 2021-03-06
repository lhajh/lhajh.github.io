---
layout: post
title: VUE axios 发送 Form Data 格式数据请求
categories: [Vue]
description: VUE axios 发送 Form Data 格式数据请求
keywords: vue
---

 `axios` 默认是 `Payload` 格式数据请求, 但有时候后端接收参数要求必须是 `Form Data` 格式的, 所以我们就得进行转换.

 `Payload` 和 `Form Data` 的主要设置是根据请求头的 `Content-Type` 的值来的.

- Payload Content-Type: 'application/json; charset=utf-8'

- Form Data Content-Type: 'application/x-www-form-urlencoded'

## 一、 设置单个的 POST 请求为 Form Data 格式

方法一: 修改 headers 和 transformRequest

```js
axios({
  method: 'post',
  url: 'http://localhost:8080/login',
  data: {
    username: this.loginForm.username,
    password: this.loginForm.password
  },
  transformRequest: [
    function (data) {
      let ret = ''
      for (let it in data) {
        // 如果 data[it] 是一个对象, 需要先使用 JSON.stringify, 再使用 encode
        ret += encodeURIComponent(it) + '=' + encodeURIComponent(data[it]) + '&'
      }
      ret = ret.substring(0, ret.lastIndexOf('&'))
      return ret
    }
  ],
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
})
```

方法二: 利用 [URLSearchParams](https://developer.mozilla.org/zh-CN/docs/Web/API/URLSearchParams), 不兼容 IE, 貌似安卓 / iOS 低版本也不兼容

```js
const params = new URLSearchParams();
params.append('username', this.loginForm.username);
params.append('password', this.loginForm.password);
axios({
  method: 'post',
  url: 'http://localhost:8080/login',
  data: params
})
```

方法三: 利用 [FormData](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData), 兼容性比 `URLSearchParams` 好一点

```js
const params = new FormData();
params.append('username', this.loginForm.username);
params.append('password', this.loginForm.password);
axios({
  method: 'post',
  url: 'http://localhost:8080/login',
  data: params
})
```

方法四: 利用 qs

```js
const params = {
  username: this.loginForm.username,
  password: this.loginForm.password
}
axios({
  method: 'post',
  url: 'http://localhost:8080/login',
  data: qs.stringify(params)
})
```

## 二、 全局设置 POST 请求为 Form Data 格式

因为像上面那样每个请求都要配置 `transformRequest` 和 `Content-Type` 非常的麻烦, 重复性代码也很丑陋, 所以通常都会进行全局设置. 具体代码如下

```js
import axios from 'axios'
import qs from 'qs'

// 实例对象
let instance = axios.create({
  timeout: 6000,
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
})

// 请求拦截器
instance.interceptors.request.use(
  config => {
    config.data = qs.stringify(config.data) // 转为 formdata 数据格式
    return config
  },
  error => Promise.error(error)
)
```

就是我们在封装 `axios` 的时候, 设置请求头 `Content-Type` 为   `application/x-www-form-urlencoded` . 然后在请求拦截器中, 通过 `qs.stringify()` 进行数据格式转换, 这样每次发送的 `POST` 请求都是 `Form Data` 格式的数据了. 其中 `qs` 模块是安装 `axios` 模块的时候就有的, 不用另行安装, 通过 `import` 引入即可使用.

上面的方法会将全部 `post` 请求都转换成 `Form Data` 格式了, 实际上并不是所有请求都是这样的. 我们可以根据一个参数判断是否需要转换

```js
import axios from 'axios'
import qs from 'qs'
import { Message, Loading } from 'element-ui'
/**
 * post
 * @param  {String} url -必选   [地址]
 * @param  {Object || Array} params -必选 [参数]
 * @param  {String} type -可选  [设定为 form 为 formdata 提交]
 * @param  { Boolean } isLoading -可选 [是否显示加载状态, 默认显示]
 * @return {Object}        [Promise]
 */
export const $post = (url, params, type, isLoading = true) => {
  if (isLoading) {
    var loadingInstance = Loading.service({
      text: '正在加载中'
    })
  }
  type === 'form' && params = qs.stringify(params)
  return new Promise((resolve, reject) => {
    axios.post(url, params, {})
      .then(res => {
        if (isLoading) {
          loadingInstance.close()
        }
        if (res.status === 200) {
          if (res.data.code === 0) {
            resolve(res.data)
          } else if (res.data.code === 401) {
            failMessage(res.data.message)
            setTimeout(() => {
              window.location.href = '/static/login.html'
            }, 2000)
          } else {
            failMessage(res.data.message)
            reject(res)
          }
        } else {
          failMessage()
          reject(res)
        }
      })
      .catch(mes => {
        if (isLoading) {
          loadingInstance.close()
        }
        if (mes.response.status === 306) {
          failMessage('您身份已过期，3秒后返回登录页面')
          setTimeout(() => {
            window.location.href = '/static/login.html'
          }, 3000)
        } else if (mes.response.status === 403) {
          failMessage('无访问权限')
          reject(mes.response.data)
        } else {
          failMessage()
          reject(mes.response.data)
        }
      })
  })
}

function failMessage (mes = '数据获取失败') {
  Message({
    showClose: true,
    message: mes,
    type: 'warning'
  })
}
```
