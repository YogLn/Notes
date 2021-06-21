### 1.webpack打包初体验

使用淘宝镜像全局安装webpack

`cnpm install webpack webpack-cli -g`

创建文件夹

`mkdir demo01`

进入目录

`cd 目录文件夹`

进入上一层目录

`cd ../`

对包的初始化

`npm init -y`		(-y代表一个默认的配置快速生成一个package.json文件)

在本地进行webpack的依赖

`cnpm install webpack webpack-cli --save-dev`

开发环境下对文件进行打包

`webpack ./src/index.js -o ./dist/bundle.js --mode=development`

webpack 当前目录问文件 -o(指定打包文件) 指定打包文件 打包模式(development、production)

**生产环境比开发环境多了代码压缩和代码混淆**

package-lock.json的主要作用就是锁定依赖项的安装目录和依赖包的版本信息。 

### 2.使用webpack配置文件打包

创建`webpack.config.js`的配置文件在文件中进行如下配置

```js
let path=require("path")//获取当前的文件目录
module.exports = {
    mode: "development", //设置开发模式不会对代码进行压,上线设置为"production",
    devtool:"source-map", //建立js映射
    //入口文件
    entry:"./src/index.js",
    //输出文件
    output:{
        //输出文件名称
        filename:"bundle.js",
        //输出的路径
        //绝对路径
        path:path.resolve(__dirname,'dist') 
        //作用：把一个路径或路径片段的序列解析为一个绝对路径。相当于执行cd操作
    },
    mode:'development'
}
```

### 3.配置loader完成样式打包.css&.less

#### 打包css

在js文件中无法使用css文件在使用loader后可以在js文件中导入css文件

webpack打包CSS需要安装style-loser和css-loader两个模块

> npm install style-loader css-loader -D

在`webpack.config.js`的配置文件中新增

```js
module:{
        //对某种格式的文件进行转换处理
        rules:[
            {
                test:/\.css$/,//正则表达式获取以.css结尾的文件
                // 写法一：
                // loader: "css-loader", 
                // 写法二：
                use: [
                  // 从下往上执行
                  "style-loader",
                  "css-loader"
                ]
                // 写法三：用于传递参数
                // use: [
                //   {loader: "css-loader:", opotions: ""}
                // ]
            }

        ]
    }
```

#### 打包less

打包less需要安装less 和 less-loader两个模块

> npm install less less-loader -D

```js
module: {
    rules: [
      {
        test: /\.(css|less)$/,
        // 写法一：
        // loader: "css-loader", 
        // 写法二：
        use: [
          // 从下往上执行
          "style-loader",
          "css-loader",
          "less-loader"
        ]
        // 写法三：用于传递参数
        // use: [
        //   {loader: "css-loader:", opotions: ""}
        // ]
      }
    ]
  }
```

#### 认识postcss

**PostCSS是一个通过JavaScript来转换样式的工具；** 

**这个工具可以帮助我们进行一些CSS的转换和适配，比如自动添加浏览器前缀、css样式的重置；** 

**但是实现这些功能，我们需要借助于PostCSS对应的插件**

> npm install postcss postcss-cli -D

安装插件

> npm install postcss-preset-env -D

安装打包依赖

> npm install postcss-loader -D

单独配置`postcss.config.js`文件

```js
module.exports = {
  plugins: [
    require('postcss-preset-env')
  ]
}
```

对`postcss-loader`进行使用

```js
use: [
      // 从下往上执行
      "style-loader",
      "css-loader",
      "less-loader",
      "postcss-loader"
    ]
```



### 4.配置plugin插件完成html模板的打包

**自动将js和打包编译的内容整合在一起**

> cnpm install html-webpack-plugin

```js
//安装依赖：html-webpack-plugin，
let HtmlWebpackPlugin=require('html-webpack-plugin');
```

```js
 //plugins的配置
    plugins:[
        new HtmlWebpackPlugin({
            template:'src/index.html'
        })
    ]
```

有cannot find module 'webpack/lib/node/NodeTemplatePlugin错误的时候解决办法：

执行`npm link webpack --save-dev`再`webpack`

### 5.打包图片资源文件

#### 利用url-loader进行图片的打包处理

将较小的图片编码为base64的URI

> npm install url-loader -D

> npm install file-loader -D

```js
{
    test:/\.(jpg|gif|png)$/,
    loader:"url-loader",
    //图片小limit(8k)时，base64处理，会将图片编译成base64字符串形式
    //图片大于limit时，需要file-loader模块进行加载
    options:{
        limit:8*1024,
        //url-loader的es6模块化解析与index.html的解析冲突所以关闭
        esMOdule:false,
        //取图片hash的前10位。图片原来的扩展名
        name:'[hash:10].[ext]'
        //打包后按照原来的图片命名并且放在img文件夹下
        //name:'img/[name].[hash:10].[ext]'
    }
},
{
    test:/\.html$/,
    loader:'html-loader'
}
```

#### 利用file-loader进行图片的打包处理

```
npm install file-loader -D
```

```js
{
        test: /\.(jpe?g|png|gif|svg)$/,
        use: [
          {
            loader: "file-loader",
            options: {
              outputPath: "img",
              name: "[name]_[hash:6].[ext]"
            }
          }
        ]
      }{
        test: /\.(jpe?g|png|gif|svg)$/,
        use: [
          {
            loader: "file-loader",
            options: {
              // outputPath: "img",
              name: "img/[name]_[hash:6].[ext]"
            }
          }
        ]
      }
```

#### 利用webpack5的asset进行资源的打包(不需要安装任何依赖)

```js
{
        test: /\.(jpe?g|png|gif)$/,
        type: "asset",
        generator: {
          filename: "img/[name]_[hash:6][ext]" // 这里的ext包含了.
        },
        parser: {
          dataUrlCondition: {
            maxSize: 100 * 1024
          }
        }
      }
```

字体文件打包type: "asset、resource",

### 6.webpack实现代码热更换(搭建本地服务器)

### webpack给我们提供了watch模式

 方式一：

```js
 *"scripts"*: {

  *"build"*: "webpack --watch"

 },
```

方式二：

```js
module.exports = {
  watch: true,
}
```

在webpack.config.js中增加

```js
devServer:{
    //项目构建路径
    //contentBase:path.resolve(__dirname,'dist'),
    contentBase:'./dist'
    //启动gzip的编码压缩
    compress:true,
    //端口号
    port:3000,
    //自动打开浏览器
    open:true,
    //是否实时监听
    inline:true
}
```

### webpack-dev-server

安装模块：

> npm install webpack-dev-server -D

```js
  "scripts": {
    "build": "webpack --watch",
    "serve": "webpack serve"
  },
```

> npm run serve

### 7.ES6语法转成ES5语法

#### 单个插件的转换

> npm install @babel/core @babel/cli -D

安装箭头函数转换插件

> npm install @babel/plugin-transform-arrow-functions -D 

安装let、const块级作用域的转换

> npm install @babel/plugin-transform-block-scoping -D

使用命令

> npx babel demo.js --out-dir dist --plugins=@babel/plugin-transform-arrow-functions,@babel/plugin-transform-block-scoping



#### 使用preset进行转换

通过上面的方法，需要每个每个插件的进行安装，十分麻烦，所以开发中通过预设进行转换

> npm install @babel/preset-env -D

打包

> npx babel demo.js --out-file test.js --presets=@babel/preset-env

`demo.js`

```js
const arr = ['abc', 'bbc', 'cbc', 'nbc']
arr.forEach(item => {
  console.log(item);
})	
```

`test.js`

```js
"use strict";

var arr = ['abc', 'bbc', 'cbc', 'nbc'];
arr.forEach(function (item) {
  console.log(item);
});

```



#### webpack中的使用

但是webpack是模块化打包工具，只会帮助我们进行模块打包，并不会将ES6转化成ES5，所以我们应该需要相应的loader来进行处理

> npm install babel-loader @babel/core

使用：

```js
{
    test: /\.js$/,
    use: {
      loader: "babel-loader",
      options: {
        // plugins: [
        //   "@babel/plugin-transform-arrow-functions",
        //   "@babel/plugin-transform-block-scoping"
        // ]
        presets: [
          "@babel/preset-env"
        ]
      }
    }
  }
```



#### webpack中的使用，抽离`babel.config.js`

在根目录新建`babel.config.js`

```js
module.exports = {
  presets: [
    "@babel/preset-env"
  ]
}
```

修改`webpack.config.js`

```js
{
    test: /\.js$/,
    use: {
      loader: "babel-loader",
      // options: {
      //   // plugins: [
      //   //   "@babel/plugin-transform-arrow-functions",
      //   //   "@babel/plugin-transform-block-scoping"
      //   // ]
      //   presets: [
      //     "@babel/preset-env"
      //   ]
      // }
    }
  }
```



### 8.版权的pluginBannerPlugin

我们先来使用一个最简单的插件，为打包的文件添加版权声明

该插件名字叫BannerPlugin，属于webpack自带的插件。

按照下面的方式来修改webpack.config.js的文件：

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200918120735041.png" alt="image-20200918120735041" style="zoom:80%; margin-left:0px" />

重新打包程序：查看bundle.js文件的头部，看到如下信息

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200918120750169.png" alt="image-20200918120750169" style="zoom:95%;margin-left:0px" />

### 10.webpack插件的使用

#### a.CleanWebpackPlugin插件

> npm install clean-webpack-plugin -D

每次打包时会先将之前打包的文件先删除，使用方式

先引入`CleanWebpackPlugin`类

```js
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
```

再在plugin中传入`CleanWebpackPlugin`对象

```js
plugins: [
    new CleanWebpackPlugin()
]
```



#### b.HtmlWebpackPlugin

> npm install html-webpack-plugin -D

引入`HtmlWebpackPlugin	`

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
```

再传入对象

```js
plugins: [
    new HtmlWebpackPlugin()
]
```

##### **打包模板html**

在根目录创建public文件夹，里面包含一个html文件

再在`HtmlWebpackPlugin`传入参数

```js
new HtmlWebpackPlugin({
  template: "./public/index.html",
  title: "哈哈哈"
})
```

**注意：修改title时候模板中的title标签应该为`<title><%= htmlWebpackPlugin.options.title %></title>`**

[更多的HtmlWebpackPlugin使用方式](https://juejin.cn/post/6844903853708541959)



#### c.DefinePlugin

> npm install define-plugin -D

可以定义一些Ejs的模板类似于<%=BASE_URL%>

```js
new DefinePlugin({
  BASE_URL: "'./'"
})
```



#### d.CopyWebpackPlugin

> npm install copy-webpack-plugin -D

这个插件可以用于将一些文件，例如：图标，通过复制的形式进入打包的文件夹

```js
new CopyWebpackPlugin({
  patterns: [ // 一个文件夹一个对象
    {
      from: "public",
      globOptions: {
        ignore:  [ // 需要忽略的文件
          "**/index.html"
        ]
      }
    }
  ]
})
```



### 11.webpack打包vue

安装vue3

> npm install vue@next

我们需要合适的Loader来处理文件

> npm install vue-loader -D

在webpack的模板规则中进行配置：

```js
{
    test: /\.vue$/,
    loader: "vue-loader"
}
```

为我们必须添加@vue/compiler-sfc来对template进行解析：

> npm i vue-template-compiler --save-dev

> npm install @vue/compiler-sfc -D

另外我们需要配置对应的Vue插件：

```js
const { VueLoaderPlugin } = require('vue-loader')
```

```js
plugins: [
    // 请确保引入这个插件！
    new VueLoaderPlugin()
  ]
```

### 12.webpack配置

```js
  devServer: {
    contentBase: "./public"
  },
```

它会将`public`下的资源进行打包，将原来拷贝public的资源的代码进行注释

```js
// new CopyPlugin({
//   patterns: [
//     {
//       from: "public",
//       to: "./",
//       globOptions: {
//         ignore: ["**/index.html"],
//       }
//     }
//   ]
// }),
```

对单个模块实现热替换

```js
devServer: {
    hot: true // 开启模块热替换
  },
```

```js
import './js/element';

if(module.hot) {
  module.hot.accept("./js/element.js", () => {
    console.log("element模块发生了更新");
  })
}
```

其他的配置：

```js
host: "0.0.0.0",
port: 8001,
open: true,
compress: true, // 开启gzip模式进行压缩， 不压缩html文件
```

#### 利用proxy解决跨域问题

```js
 proxy: {
  "/api": {
    target: "http://121.4.100.103:5000",
    pathRewrite: {
      "api": ""  // 取消原来请求路径中的api
    },
    secure: false, //利用https发送请求会要求携带安全整数，否者会报错，设置false即可
    changeOrigin: true // 设置自动改变源
  }
}
```

请求方式：

```js
axios.get("/api/moment?offset=0&size=10").then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
})
```

#### reslove的配置

```js
  reslove: {
    extensions: [".js",".json", ".vue"],
    alias: {
      "@": path.resolve(__dirname, "./src"),
      "js": "@/js"
    }
  },
```

### 13.webpack分离

这里我们创建三个文件： 

- webpack.comm.conf.js 
- webpack.dev.conf.js 
- webpack.prod.conf.js

修改package.json里面的`script`

```json
"scripts": {
    "build": "webpack --config ./config/webpack.prod.config.js",
    "serve": "webpack serve --config ./config/webpack.dev.config.js"
  },
```

对文件的代码进行抽离和合并

使用`webpack-merge`的插件进行合并

> npm i webpack-merge -D

```js
const { merge } = require('webpack-merge');
const commonConfig = require('./webpack.comm.config');

module.exports = merge(commonConfig,{
    
})
```



