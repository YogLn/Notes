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

在webpack.config.js中增加

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

安装模块

`cnpm install url-loader html-loader --save-dev`

因为引入的是文件所以还需下载

`cnpm install fileloader --save-dev`

`cnpm install`

执行`npm link webpack --save-dev`再`webpack`

### 6.webpack实现代码热更换(搭建本地服务器)

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

安装模块：

`cnpm install webpack-dev-server -g`

安装模块所需要的依赖包

`cnpm install`

执行`npm link webpack --save-dev`再`webpack-dev-server`

### 7.ES6语法转成ES5语法	

 **babel-loader 加载 ES2015 代码,然后使用 Babel 转译为 ES5**

> webpack 3.x | babel-loader 8.x | babel 7.x

```bash
npm install babel-loader@8.0.0-beta.0 @babel/core @babel/preset-env webpack
```

> webpack 3.x babel-loader 7.x | babel 6.x

```bash
npm install babel-loader babel-core babel-preset-env webpack
```

用法

[文档：使用 loader](https://webpack.js.org/loaders/)

在 webpack 配置对象中，需要添加 babel-loader 到 module 的 loaders 列表中，像下面这样：

```javascript
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
    }
  ]
}
```

### 8.版权的pluginBannerPlugin

我们先来使用一个最简单的插件，为打包的文件添加版权声明

该插件名字叫BannerPlugin，属于webpack自带的插件。

按照下面的方式来修改webpack.config.js的文件：

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200918120735041.png" alt="image-20200918120735041" style="zoom:80%; margin-left:0px" />

重新打包程序：查看bundle.js文件的头部，看到如下信息

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200918120750169.png" alt="image-20200918120750169" style="zoom:95%;margin-left:0px" />

### 9.webpack配置分离

将配置文件分离成`base.config.js`、`prod.config.js`和`dev.config.js`

安装

```
npm install webpack -merge --save-dev
```

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200918133215933.png" alt="image-20200918133215933" style="zoom:67%;margin-left:0px" />

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200918133431896.png" alt="image-20200918133431896" style="zoom:80%;margin-left:0px" />

再在`webpack.json`的配置文件中添加`--config 想要添加的配置文件路径`

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200918133810396.png" alt="image-20200918133810396" style="zoom: 67%;margin-left:0px" />

但是这样会造成打包过后追加的dist文件位于抽离的配置文件的文件夹下，此处为build文件夹，修改

```
output:{
        //输出文件名称
        filename:"bundle.js",
        //输出的路径
        //绝对路径
        path:path.resolve(__dirname,'../dist') 
        //作用：把一个路径或路径片段的序列解析为一个绝对路径。相当于执行cd操作
    },
```

