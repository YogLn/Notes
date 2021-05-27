`/src/network/request.js`

```js
import axiox from 'axios'
import { BASE_URL, TIMEOUT } from './config'
const instance = axiox.create({
  baseURL: BASE_URL,//baseURL会在发送请求的时候拼接在url参数的前面
  timeout: TIMEOUT
})

instance.interceptors.request.use(function (config) {
  config.headers.Authorization = window.sessionStorage.getItem('token')
  return config;
}, function (error) {
  // 对请求错误做些什么
  return Promise.reject(error);
});

// 添加响应拦截器
//此处可以根据服务器的返回状态码做响应的处理
//404 404 500
instance.interceptors.response.use(function (response) {
  // 对响应数据做点什么

  return response;
}, function (error) {
  // 对响应错误做点什么
  return Promise.reject(error);
});

export function get (url, params) {
  return instance.get(url, {
    params
  })
}

export function post (url, data) {
  return instance.post(url, data)
}

export function del (url) {
  return instance.delete(url)
}

export function put (url, data) {
  return instance.put(url, data)
}
```



`/src/network/config.js`

```js
const devBaseURL = "http://";
const proBaseURL = "http://";
export const BASE_URL = process.env.NODE_ENV === 'development' ? devBaseURL: proBaseURL;

export const TIMEOUT = 5000;

```



在`main.js`中引用

```js
import * as request from './network/request'

Vue.prototype.$http = request
```



然后发送请求就可以使用

```js
this.$http.get()
this.$http.post()
this.$http.put()
this.$http.del()
```



