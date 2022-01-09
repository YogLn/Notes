# 安装

```bash
npm install webpack webpack-cli –g # 全局安装
npm install webpack webpack-cli –D # 局部安装
```

修改`pakeage.json`文件，通过在`scripts`中配置命令就可以使用nodemodules下`.bin`下的webpack为不是全局的webpack进行打包

```json
"scripts": {
  "build": "webpack", //未指定打包配置，默认查找webpack.config.js
  "build": "webpack --config wk.config.js" //这是指定打包配置文件是wk.config.js
}
```

# CSS篇

安装依赖

```bash
npm install css-loader -D
npm install less-loader -D
npm install style-loader -D

npm install postcss-loader -D
npm install postcss-preset-env -D // 可以未css样式添加前缀
```

`webpack.config.js`配置

```js
const path = require('path')

module.exports = {
  entry: './src/index.js', //指定入口文件
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, './build') // 指定打包文件夹，必须是绝对路径！！！
  },
  module: {
    rules: [
      {
        // 规则使用正则表达式
        test: /\.css$/, // 匹配资源
        // loader: 'css-loader', // 写法一
        // use: ['css-loader'] // 写法二
        // 写法三 注意: 编写顺序(从下往上, 从右往做, 从后往前)
        use: [
          'style-loader', // 将解析之后的css插入到页面中
          {
            loader: 'css-loader', // 负责将.css文件进行解析
            options: {
              importLoaders: 1 // 启用/禁用或者设置在 css-loader 前应用的 loader 数量
            }
          },
          'postcss-loader'
        ]
      },
      {
        test: /\.less$/,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              importLoaders: 2
            }
          },
          'postcss-loader',
          'less-loader'
        ]
      }
    ]
  }
}
```

# 资源篇

- 在webpack5之前，加载这些资源我们需要使用一些loader，比如`raw-loader` 、`url-loader`、`file-loader`； 
- 在webpack5之后，我们可以直接使用资源模块类型`asset module type`，来替代上面的这些loader

`webpack.config.js`配置

```js
module: {
  rules: [
    {
        test: /\.(png|jpe?g|gif|svg)$/,
        // type: "asset/resource", file-loader的效果
        // type: "asset/inline", url-loader的效果
        type: 'asset',
        generator: {
          filename: 'img/[name].[hash:6][ext]' //指定资源的输出路径和文件名，注意扩展名前没有.
        },
        // 设置url-loader的limit效果
        parser: {
          dataUrlCondition: {
            maxSize: 100 * 1024
          }
        }
      },
      {
        test: /\.ttf|eot|woff2?$/i,
        type: 'asset/resource',
        generator: {
          filename: 'font/[name].[hash:6][ext]'
        }
      }
  ]
}
```

# Plugin篇

```bash
npm install clean-webpack-plugin -D //每次打包自动删除打包的文件夹
npm install html-webpack-plugin -D // 打包后生成html文件 默认情况下是根据ejs的一个模板来生成的，在html-webpack-plugin的源码中，有一个default_index.ejs模块
npm install copy-webpack-plugin -D // 将一些文件进行复制
```

`webpack.config.js`配置

```js
const path = require('path')
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const {DefinePlugin} = require('webpack')

module.exports = {
  plugins: [
		new CleanWebpackPlugin(),
		new HtmlWebpackPlugin({
			title: '学习webpack',
      template: './public/index.html' // 指定html模板
		}),
    new DefinePlugin({ 
      // DefinePlugin允许在编译时创建配置的全局常量,编译template就可以正确的编译了，会读取到BASE_URL的值
      BASE_URL: '"./"'
    })，
    new CopyWebpackPlugin({
      patterns: [ //复制的规则在patterns中设置
        {
          from: "public", // 设置从哪一个源中开始复制,
    			// to: './dist', //复制到的位置，可以省略，会默认复制到打包的目录下
          globOptions: { //设置一些额外的选项，其中可以编写需要忽略的文件
            ignore: [
              "**/index.html", //我们已经通过HtmlWebpackPlugin完成了index.html的生成,忽略复制
              "**/.DS_Store",
              "**/abc.txt"
            ]
          }
        }
      ]
    })
	]
}
```

# 模块化篇

## mode

在配置中我们可以指定一个`mode`告知 webpack 使用相应模式的内置优化

```bash
string = 'production': 'none' | 'development' | 'production'
```



| 选项          | 描述                                                         |
| :------------ | :----------------------------------------------------------- |
| `development` | 会将 `DefinePlugin` 中 `process.env.NODE_ENV` 的值设置为 `development`. 为模块和 chunk 启用有效的名。 |
| `production`  | 会将 `DefinePlugin` 中 `process.env.NODE_ENV` 的值设置为 `production`。为模块和 chunk 启用确定性的混淆名称，`FlagDependencyUsagePlugin`，`FlagIncludedChunksPlugin`，`ModuleConcatenationPlugin`，`NoEmitOnErrorsPlugin` 和 `TerserPlugin` 。 |
| `none`        | 不使用任何默认优化选项                                       |

## source-map

[详细文档](https://webpack.docschina.org/configuration/devtool/)

>  source-map是从已转换的代码，映射到原始的源文件，使浏览器可以重构原始源并在调试器中显示重建的原始源。

如何可以使用source-map呢？两个步骤： 

- 第一步：根据源文件，生成source-map文件，webpack在打包时，可以通过配置生成source-map； 
- 第二步：在转换后的代码，最后添加一个注释，它指向sourcemap；

```bash
//# sourceMappingURL=common.bundle.js.map
```

> 最初source-map生成的文件带下是原始文件的10倍，第二版减少了约50%，第三版又减少了50%，所以目前一个 133kb的文件，最终的source-map的大小大概在300kb

**source-map内容**

- version：当前使用的版本，也就是最新的第三版； 
- sources：从哪些文件转换过来的source-map和打包的代码（最初始的文件）； 
- names：转换前的变量和属性名称（因为我目前使用的是development模式，所以不需要保留转换前的名 称）； 
- mappings：source-map用来和源文件映射的信息（比如位置信息等），一串base64 VLQ（veriable length quantity可变长度值）编码； 
- file：打包后的文件（浏览器加载的文件）； 
- sourceContent：转换前的具体代码信息（和sources是对应的关系）； 
- sourceRoot：所有的sources相对的根目录；

**最佳实践**

- 开发阶段：推荐使用 source-map或者cheap-module-source-map ü 这分别是vue和react使用的值，可以获取调试信息，方便快速开发； 
- 测试阶段：推荐使用 source-map或者cheap-module-source-map ü 测试阶段我们也希望在浏览器下看到正确的错误提示； 
- 发布阶段：false、缺省值（不写）

# babel篇

Babel是一个工具链，主要用于旧浏览器或者缓解中将ECMAScript 2015+代码转换为向后兼容版本的 JavaScript；包括：语法转换、源代码转换、Polyfill实现目标缓解缺少的功能等；

## babel编译器执行过程

![image-20220109154125007](https://s2.loli.net/2022/01/09/PtG9OiqlLrnFjuN.png)

## 安装

我们可以给webpack提供一个 preset，webpack会根据我们的预设来加载对应的插件列表，并且将其传递给babel

```bash
npm install babel-loader @babel/core @babel/preset-env
```

## 使用

`webpack.config.js`的`rules`中新增一个规则

```js
module: {  rules: [     {          test: /\.m?js$/,          use: {            loader: 'babel-loader'          }      }  ]}
```

`babel.config.js`

```js
module.exports = {  presets: [    [      '@babel/preset-env'    ]  ]}
```

# polyfill	

当我们使用了一些语法特性（例如：Promise, Generator, Symbol等以及实例方法例如 Array.prototype.includes等），但是某些浏览器压根不认识这些特性，必然会报错，我们可以使用`polyfill`来填充或者说打一个补丁，那么就会包含该特性了。

## 使用

- babel7.4.0之前，可以使用 @babel/polyfill的包，但是该包现在已经不推荐使用了

- babel7.4.0之后，可以通过单独引入core-js和regenerator-runtime来完成polyfill的使用

安装

```js
npm install core-js regenerator-runtime --save
```

配置`webpack.config.js`,忽略`nodemodules`文件夹

```js
module: {  rules: [     {          test: /\.m?js$/,       		exclude: /node_modules/,          use: {            loader: 'babel-loader'          }      }  ]}
```



配置`babel.config.js`

```js
module.exports = {  presets: [    [      '@babel/preset-env',      {        // false: 不用任何的polyfill相关的代码        // usage: 代码中需要哪些polyfill, 就引用相关的api        // entry: 手动在入口文件中导入 core-js/regenerator-runtime, 根据目标浏览器引入所有对应的polyfill        useBuiltIns: 'entry', //设置以什么样的方式来使用polyfill；        corejs: 3 //设置corejs的版本，目前使用较多的是3.x的版本      }    ]  ]}
```

# React

在我们编写react代码时，react使用的语法是jsx，jsx是可以直接使用babel来转换的。  对react jsx代码进行处理需要如下的插件： 

- @babel/plugin-syntax-jsx 

- @babel/plugin-transform-react-jsx 

- @babel/plugin-transform-react-display-name 

 但是开发中，我们并不需要一个个去安装这些插件，我们依然可以使用preset来配置

```bash
npm install @babel/preset-react -D
```

`babel.config.js`

```js
module.exports = {  presets: [    ["@babel/preset-react"]  ]}
```

`webpack.config.js`也要设置对jsx的支持

```js
module: {  rules: [     {          test: /\.jsx$/,       		exclude: /node_modules/,          use: {            loader: 'babel-loader'          }      }  ]}
```

# Vue

```bash
npm install vue-template-compiler vue-loader -D
```

`webpack.config.js`

```js
const VueLoaderPlugin = require('vue-loader/lib/plugin');

module.exports = {
    module: {
     		rules: [
          {
            test: /\.vue$/,
            use: "vue-loader"
          }
        ] 
    },
  plugins: [
    new VueLoaderPlugin()
  ]
}
```

# HRM

我们希望浏览器可以事实的监听页面的变化并刷新浏览器

```bash
npm install --save-dev webpack-dev-server
```

`webpack.config.js`

```js
// 专门为webpack-dev-server
devServer: {
  hot: true
},
```

修改`package.json`的配置

```json
"scripts": {
  "build": "webpack",
  "serve": "webpack serve"
},
```