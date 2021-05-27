## 一、安装依赖

> npm init -y

>npm install koa

> npm install nodemon -D

> 中间件得使用路由得划分
>
> npm install koa-router

> 对用户传递过来得json数据进行解析
>
> npm install koa-bodyparser

> 连接数据库
>
> npm install mysql2

> md5加密密码
>
> npm install crypto

> 使用jwt
>
> npm install jsonwebtoken

> 图像的处理
>
> npm install koa-multer

## 二、项目目录结构的划分

![image-20210201141418543](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210201141418543.png)

### 修改package.json问题

```json
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
"start": "nodemon ./src/main.js"	
},
```

## 三、配置信息的组织

![image-20210201132126805](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210201132126805.png)

通常在根目录下新建一个.env文件

安装`dotenv`依赖,将环境变量加载到项目

> npm install dotenv

`config.js`

```js
const  dotenv = require('dotenv')

dotenv.config()

// console.log(process.env.APP_PORT)

module.exports = {
  APP_PORT
} = process.env
```

 

## 四、token

> npm install jsonwebtoken

> openssl

创建密钥：

> ```
> genrsa -out private.key 1024
> ```

创建公玥

> ```
>  rsa -in private.key -pubout -out public.key
> ```