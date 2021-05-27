# 一.环境配置

1. 安装

   ```
   npm install axios --save
   ```

2. 导入

   ```
   import axios from 'axios'
   ```

3. 基本使用，默认为get请求

   ```
   axios({
       url: "url"
   }).then(res => {
       console.log(res);
   });
   ```

# 二.发起请求

1. 直接使用`acios(config)`

   ```
   axios({
       url: "url",
       method: 'get'
   }).then(res => {
       console.log(res);
   });
   ```

2. 使用`axios.请求类型(url,[config])`

   ```
   axios.get({
       url: "url"
   }).then(res => {
       console.log(res);
   });
   ```

# 三.请求参数

- get

  使用`params`属性

  ```
  axios.get({
      url: "url",
      params:{
          page: 1
          ...
      }
  }).then(res => {
      console.log(res);
  });
  ```

- post，put，delete

  使用`data`属性

  ```
  axios.post({
      url: "url",
      data:{
          page: 1
          ...
      }
  }).then(res => {
      console.log(res);
  });
  ```

# 四.并发请求

- 直接获取结果数组：

  ```
  axios.all([axios({}), axios({})]).then(res => {});
  ```

- 分开获取结果

  ```
  axios.all([axios({}), axios({})]).then(axios.spread((res1,res2)=>{}));
  ```

# 五.全局配置

```
axios.defaults.baseURL = 'url'//设置全局请求根路径
axios.defaults.timeout = 5000//设置全局超时时间（毫秒）

axios.get({
    url: "/uri"
}).then(res => {
    console.log(res);
});
```

# 六.axios实例

> 避免使用全局axios造成公共配置冲突

```
const instance = axios.create({
    baseURL = 'url',
    timeout = 5000,
    ...
})
//接下去请求使用instance即可
```

# 七.网络请求的封装

```
src/network/request.js
```

1. 方法一，函数回调

   ```
   import axios from 'axios'
   export function request(config,success,failure){
       //1.创建axios实例
       const instance = axios.create({
           baseURL: 'url',
           timeout: 5000
       })
       //2.发送真正的网络请求
       instance(config).then(res=>{
           //函数回调
           success(res)
       }).catch(err =>{
           //函数回调
           failure(err)
       })
   }
   ```

2. 方法二，将回调函数和`config`全部包装成一个对象

   ```
   import axios from 'axios'
   export function request(config){
       //1.创建axios实例
       const instance = axios.create({
           baseURL: 'url',
           timeout: 5000
       })
       //2.发送真正的网络请求
       instance(config.baseURL).then(res=>{
           //函数回调
           config.success(res)
       }).catch(err =>{
           //函数回调
           config.failure(err)
       })
   }
   ```

3. 方法三

   ```
   import axios from 'axios'
   export function request(config){
       return new Promise((resolve,reject)=>{
           //1.创建axios实例
           const instance = axios.create({
               baseURL: 'url',
               timeout: 5000
           })
           
           //2.发送网络请求
           instance(config).then(res=>{
               resolve(res)
           }).catch(err=>{
               reject(err)
           })
       })
   }
   ```

   使用：

   ```
   request({
       url:'url'
   }).then(res=>{
       
   }).catch(err=>{
       
   })
   ```

4. **方法四**

   ```
   import axios from 'axios'
   export function request(config){
       //1.创建axios实例
       const instance = axios.create({
           baseURL: 'url',
           timeout: 5000
       })
   
       //2.发送网络请求
       return instance(config);
   }
   ```

# 八.拦截器

```
src/network/request.js
import axios from 'axios'
export function request(config){
    //1.创建axios实例
    const instance = axios.create({
        baseURL: 'url',
        timeout: 5000
    })
    
	//2.1 请求拦截
    instance.interceptors.request.use(config=>{
        //请求成功，拿到请求数据
        console.log(config);
        
        //返回config，不返回就直接拦截
        return config;
    },error=>{
        //请求失败
        console.log(error)
    })
    //2.2 响应拦截
    instance.interceptors.response.use(res=>{
        //响应成功，拿到响应结果，并取出其中的data
        res = res.data;
        //返回相应结果
        return res;
    },error=>{
        //响应失败，拿到响应错误信息
    })
    
    //3.发送网络请求
    return instance(config);
}
```

- 请求拦截使用场景
  - config中的一些信息不符合服务器要求
  - 配置加载动画，在响应成功后将动画隐藏
  - 某些网络请求，携带一些特殊的信息