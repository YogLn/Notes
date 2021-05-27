## 一.什么是nodejs

nodejs 是一个基于v8引擎的运行时环境

## 1.使用node管理工具

node版本管理工具：n(不支持windows)，nvm-windows

- nvm list :  查看电脑上有哪些node 版本
- nvm install 14.13.1 安装指定版本
- nvm install lts 安装lts
- nvm list available
- nvm  use 版本号 ：切换到 对对应的版本进行使用
- nvm uninstall 版本号 : 卸载对应的版本

## 2.Node中的REPL

- REPL是Read-Eval-Print Loop的简称，翻译为："读取-求值-输出 循环"
- 是一个加简单的、交互式编程环境在node环境下执行代码

## 二.JS模块化和Node模块化

```js
// 清空控制台
console.clear()

// 打印函数的调用栈
console.trance()
```

### 1.commonJs

> **node实现commonjs就是对象的引用赋值(浅层拷贝)**

```js
// 导出
exports.name = name

// 引入
const { name } = require('./路径')
```

> 导出的本质上是：**Module.export**导出，一旦是 **Module.expor**导出，expor导出将不起作用
>
> Node内部帮我们做了一件事：**Module.export = export**
>
> module.export 和export 以及require指向的都是同一块地址空间，但是当用module.export = {}进行对象的导出时，内存中会重新开辟一块内存进行导出，exports将失去效果。

#### require的核心模块

**1.没有后缀名的文件的查找顺序：**

1. 直接找文件X
2. 查找X.js
3. 查找X.json
4. 查找X.node

**2.没有找到对应的文件**

查找目录下的index文件

1. 直接找文件X/index.js
2. 查找X/index.js
3. 查找X/index.json
4. 查找Xindex.node

node中的commonjs和加载过程同步

```js
require('./index.js')

console.log('main的代码被执行了') //先执行index.js 的文件再执行mian.js 中的代码
```

### 2.ESmodule

在html文件中引入js文件并没有把他当成一个模块，必须加上type=module才会被当成一个模块

```html
<body>
    <script src="./index.js" type="module"></script>
</body>
```

#### **三种导出方式：**

1. 方式一：在方法或者变量前加`export`

   ```js
   export const name = '小明'
   
   export const function sayHello(msg) {
       console.log(msg)
   }
   ```

2. 方式二：export(不是对象)

   ```js
   export {
   	name,
       sayHello
   }
   ```

3. export导出的时候给变量起别名

   ```js
   export {
   	name as fName,
       sayHello as fSayHello
   }
   ```

#### 常见的三种导入方式

1. import { } from './路径.js'   

   ```js
   import { name } from './路径.js'   //.js 不能省略
   ```

2. 导出变量后起别名

   ```js
   import { name as wNamme } from './路径.js'
   
   // 导出的时候起过别名fName
   import { fName as wNamme } from './路径.js'
   ```

3. 通过* as foo

   ```js
   import * as foo from './路径.js'
   
   console.log(foo.name)
   ```

#### export和import的结合使用(了解)

```js
export { name, age} form './xxx.js'
```

#### 默认导出(在一个模块化中只能有一个默认导出方式)

```js
export default function () {
    console.log('默认导出')
}

// 引入
import 任意名字 form './xxx.js'
```

EsModule加载过程是异步的

> 运行ES模块的文件后缀.mjs

#### CommonJS和ES module之间的交互：

- 通常情况下CommonJS不能加载EsModule
- 多数情况下，ESModule可以加载CommonJS

## 三、Node中的常用内置模块

### 1.path模块(拼接路径)

```js
const path = require('path')

const basePath = '/User'
const fileName = '/abc.txt'

// 拼接路径
const filePath = path.reslove(basePath, fileName)
```

#### path模块的方法：

1.  获取路径信息

   ```js
   const filepath = '/User/yogln/abc.txt'
   
   // 获取路径
   path.dirname(filepath)
   // 获取文件名
   path.basename(filepath)
   // 获取文件的后缀名
   path.extname(filepath)
   ```

2.  join路径拼接

   ```js
   const path = path.join(basePath, fileName)
   ```

3. resolve路径拼接

   ```js
   const filePath = path.reslove(basePath, fileName) 
   ```

   > 区别： 
   >
   > reslove    会判断拼接的字符串路径中是否有以 `/` 或 `./` 或者`../`开头的路径，
   >
   > ​				有的话会根据当前位置的路径的相对路径进拼接
   >
   > 
   >
   > join     只会进行字符串的拼接 以`/`开头的只会作为开头

### 2.fs模块(文件系统)

#### 1.fs获取文件信息的三种操作方式

```txt
// 读取文件abc.txt
hello world
```

```js
const fs = require('fs')
const filepath = './abc.txt'

// 1. 方式一：同步操作，后续需要执行的代码会被阻塞
const info = fs.ststSync(filepath)
console.log(info)            

// 2. 方式二：异步方式
fs.stat(filepath, (err, info) => {
    if(err) {
        console.log(err)
        return
    }
    console.log(info)
})

// 3. 方式三： promise
fs.promises.stat(filepath).then(info => {
    console.log(info)
}).catch(err => {
    console.log(err)
})
```

#### 2.文件描述符

```js
const fs = require('fs')
fs.open('./abc.txt', (err, fd) => {
    if(err) {
        return
    }
   // 打印文件描述符 
  	console.log(fd)  
   // 通过文件描述符获取文件信息
    fs.fstat(fd, (err, info) => {
        console.log(info)
    })
})
```

#### 3.文件的读写

##### 写文件

```js
const fs = require('fs')

const content = '你好啊，李银河'

fs.writeFile('./abc.txt', content, err => {
  console.log(err)
})
```

> **flag选项**
>
> w 打开文件写入，默认值； 
>
>  w+打开文件进行读写，如果不存在则创建文件； 
>
>  r打开文件读取，读取时的默认值； 
>
>  r+ 打开文件进行读写，如果不存在那么抛出异常； 
>
> a打开要写入的文件，将流放在文件末尾。如果不存在则创建文件；
>
> a+打开文件以进行读写，将流放在文件末尾。如果不存在则创建文件

```js
// 在文件末尾追加
fs.writeFile('./abc.txt', content, {flag: "a"}, err => {
  console.log(err)
})
```

>encoding 选型，默认utf-8
>
>如果不填写encoding，返回的结果是Buffer；

#### 4.文件读取

```js
fs.readFile('./abc.txt', {encoding: 'utf-8'}, (err, data) => {
  console.log(data);
})
```

#### 5.文件夹的操作

```js
const fs = require('fs')

// 1.创建文件夹
const dirname = './yogln'
if (!fs.existsSync(dirname)) {
  fs.mkdir(dirname, err => {
    console.log(err)
  })
}
// 2. 读取文件夹中所有文件
fs.readdir(dirname, (err, files) => {
  console.log(files); //输出： [ 'test.txt' ]
})

// 3.文件夹的重命名
fs.rename('./yogln', 'newName', err => {
  console.log(err);
})
```



**扩展：递归读取文件的内容**`{ withFileTypes: true },获取文件夹的类型（对象）` 

```js
function getFiles (dirname) {
  fs.readdir(dirname, { withFileTypes: true }, (err, files) => {
    for (let file of files) {
      if(file.isDirectory()) {
        const filePath = path.reslove(dirname, file.name)
        getFiles(filePath)
      }else{
        console.log(file.name);
      }
    }
  })
}
```

### 3.events模块

```js
const EventEmitter = require('events')

// 1.创建发射器
const emitter = new EventEmitter()

// 2.监听某一事件
// on alisa addListerner
emitter.on('click', args => {
  console.log('监听到了click事件', args);
})

// 3. 发出一个事件
emitter.emit('click','yogln')


// 打印结果
// 监听到了click事件 yogln
```

取消监听

```js
const listener2 = args => {
  console.log('监听到了click事件', args);
}

emitter.off('click',listener2 )
```

其他很少用的方法

```js
emitter.eventsNames() //获取注册的哪些方法

// 只监听一次
emitter.once('click', args => {
  console.log('监听到了click事件', args);
})

// 将这次监听放在最前面
emitter.prependListener('click', args => {
  console.log('监听到了click事件', args);
})

// 移除所有的监听器
emitter.removeAllListener()
// 单独移除某个监听器
emitter.removeAllListener('click')
```

## 四、npm包管理

### **Script:**

npm start和npm run start的区别是什么？

- 它们是等价的； 
- 对于常用的 start、 test、stop、restart可以省略掉run直接通过 npm start等方式运行；

### semver版本规范

**semver版本规范是X.Y.Z：** 

-  X主版本号（major）：当你做了不兼容的 API 修改（可能不兼容之前的版本）； 
-  Y次版本号（minor）：当你做了向下兼容的功能性新增（新功能增加，但是兼容之前的版本）；
-  Z修订号（patch）：当你做了向下兼容的问题修正（没有新功能，修复了之前版本的bug）；

> 我们这里解释一下 ^和~的区别： 
>
>  ^x.y.z：表示x是保持不变的，y和z永远安装最新的版本；
>
>  ~x.y.z：表示x和y保持不变的，z永远安装最新的版本；

## 五、创建自己的脚手架工具

### 1.通过自定义命令xyl执行入口文件index.js

创建好相应的项目，里面有·`index.js`入口文件

```js
#!/usr/bin/env node
console.log('xyl cli')
```

执行`npm init -y` 初始化一个`package.json`文件,在里面配置

```json
"bin": {
    "xyl": "index.js"
  },
// xyl为命令
// index.js 事要执行的文件
```

**这步不能省，执行npm link 命令，将我们的命令进行一个链接**

再在终端种输入`xyl`,执行独对应的index文件

### 2.通过自定义的命令xyl --version查看版本号

在项目中先下载`commander`

> npm install commander

```js
#!/usr/bin/env node
const program = require('commander')
// 查看版本号，通过require请求道package.json对象拿到version
program.version(require('./package.json').version)


// 通过process进程的参数解析我们输入的命令
program.parse(process.args)
```

执行xyl --version命令即显示版本号，xyl -- help命令默认

### 3.其他选项

```js
// 增加自己的options
program.option('-x --xyl','a xyl cli')
program.option('-d --dest <dest>','a destination floder,例如：-d/src/components')
// 监听
program.on('--help', ()=> {
  console.log("Other: ")
  console.log("   other options~")
})
```

###  4.使用create 命令创建项目

新建`create.js`

```js
const program = require('commander')

const  creatCommand =() => {

  program
    .command('create <project> [others...]') //命令
    .description('clone repository into a floder') //描述
    .action((project, others) => { //动作
      console.log(project, others)
    })

}
module.exports = creatCommand
```

在`index.js `中调用

```js
const program = require('commander')
// 创建其他指令
creatCommand()
```

### 5.开发自己的脚手架工具(项目文件夹06 learnCli)

```js
// 将回调函数转换成promise
const { promisify } = require('util')

const download = promisify(require('download-git-repo'))
const open = require('open')

const { vurRepo } = require('../config/repo-config')
const { commandSpawn } = require('../utils/terminal')
const createProjetActions = async (project) => {
  // 等待时间太长，打印点信息
  console.log('xyl helps you creat project')
  // 1.clone 项目
  await download(vurRepo, project, { clone: true })
  // 2.执行npm install 
  const command = process.platform === 'win32' ? 'npm.cmd' : 'npm'
  await commandSpawn(command, ['install'], { cwd: `./${project}` })
  // 3.运行 npm run serve
  // 防止阻塞第4步操作打开浏览器8080占用阻塞
  commandSpawn(command, ['run', 'serve'], { cwd: `./${project}` })
  // 4.打开浏览器
    // 4.1 npm install open
  open('http://localhost:8080/')

}

module.exports = {
  createProjetActions
}
```

**后面因为有点难理解没学了·······**

### 6.发布自己的脚手架

1. 在npm官网上注册

2. 在终端中输入命令 

   > npm login

3. 修改`package.json`,keywords ,license, repository等

   ```json
   "repository": {
       "type": "git",
       "url": "仓库地址"
   }
   ```

4. 执行npm publish发布

   > npm publish

## 六、Buffer

### 1.buffer的编码解码

```js
const msg = 'Hello World'

// 创建buffer
const buffer = new Buffer.from(msg)
console.log(buffer)

// 一个中文占据3个字节
const msg2 = '你好'
// const buffer2 = new Buffer.from(msg2)
// console.log(buffer.length);  // 6 
const buffer2 = new Buffer.from(msg2, 'utf16le')
console.log(buffer2.length); // 2
// utf16l一个汉字占用两个编码

// 解码
console.log(buffer2.toString('utf16le'))
```

### 2.buffer的alloc创建方式

```js
// 通过
const buffer = Buffer.alloc(8)
console.log(buffer)

// 修改buffer
buffer[0] = 88
// js中0x开头表示16进制
buffer[1] = 0x88
console.log(buffer)
```

### 3.buffer的文件操作

#### a.读取文本

```js
const fs = require('fs')

// 不加入{encoding: 'utf-8'},
fs.readFile('./foo.txt', (err, data) => {
  console.log(data)
  console.log(data.toString())
})
```

#### b.读取图片

```js
const fs = require('fs')

fs.readFile('./captain.jpg', (err, data) => {
  console.log(data)
  fs.writeFile('./foo.jpg', data, err => { // 写入图片
    console.log(err)
  })
})
```

#### c.使用sharp对图片进行操作

安装sharp库,对图片进行操作

> npm install sharp

**方式一**

```js
const sharp = require('sharp')

sharp('./captain.jpg')
  .resize(200,200)
  .toFile('./baz.jpg')
```

将会生成新的200 x 200 baz.jpg图片

**方式二**

```js
sharp('./captain.jpg')
  .resize(200,200)
  .toBuffer()
  .then( data => {
    fs.writeFile('./bax.jpg', data, err => console.log(err))
})
```

## 七、事件循环和异步IO

> 什么是事件循环？
>
> ​	事件循环是JavaScript和浏览器或者Node之间的桥梁

- 进程：计算机中已经运行的程序
- 线程：操作系统能够运行调度的最小单位

### 1.浏览器的事件循环

#### 事件循环的队列

- 宏任务队列： ajax、setTimeOut、setInterval、DOM监听、UIRendering等
- 微任务队列：Promise的then回调、MutaionObserver API、quenceMicrotask()等

#### 执行顺序

1. 优先执行script代码
2. 执行微任务队列
3. 执行完所有的微任务队列后才执行宏任务队列

### 2.阻塞IO和非阻塞IO

- 操作系统为我们提供了阻塞式调用和非阻塞式调用： 
  -  阻塞式调用： 调用结果返回之前，当前线程处于阻塞态（阻塞态CPU是不会分配时间片的），调用线程只有 在得到调用结果之后才会继续执行。 
  -  非阻塞式调用： 调用执行之后，当前线程不会停止执行，只需要过一段时间来检查一下有没有结果返回即可。

### 3.阻塞和非阻塞，同步和异步的区别？

**阻塞和非阻塞是对于被调用者来说的** 

-  在我们这里就是系统调用，操作系统为我们提供了阻塞调用和非阻塞调用；

 **同步和异步是对于调用者来说的；**

-  在我们这里就是自己的程序； 
-  如果我们在发起调用之后，不会进行其他任何的操作，只是等待结果，这个过程就称之为同步调用； 
- 如果我们再发起调用之后，并不会等待结果，继续完成其他的工作，等到有回调时再去执行，这个过程就是 异步调用； 

Libuv采用的就是非阻塞异步IO的调用方式

### 4.Node中的事件循环

**事件循环会不断的从任务队列中取出对应的事件（回调函数）来执行**

但是一次完整的事件循环Tick分成很多个阶段：

-  定时器（Timers）：本阶段执行已经被 setTimeout() 和 setInterval() 的调度回调函数。 
-  待定回调（Pending Callback）：对某些系统操作（如TCP错误类型）执行回调，比如TCP连接时接收到 ECONNREFUSED。
- idle, prepare：仅系统内部使用。 
- 轮询（Poll）：检索新的 I/O 事件；执行与 I/O 相关的回调； 
-  检测：setImmediate() 回调函数在这里执行。 p 关闭的回调函数：一些关闭的回调函数，如：socket.on('close', ...)。

#### node中的微任务和宏任务

我们会发现从一次事件循环的Tick来说，Node的事件循环更复杂，它也分为微任务和宏任务： 

-  宏任务（macrotask）：setTimeout、setInterval、IO事件、setImmediate、close事件； 
- 微任务（microtask）：Promise的then回调、process.nextTick、queueMicrotask；

#### 执行顺序

1. next ticks 队列: process.nextTick；
2. 其他微任务队列: Promise的then回调、queueMicrotask
3. timers队列: setTimeout、setInterval
4. io队列： Io事件
5.  check queue队列 :setImmediate
6. close队列： close事件

## 八、Stream

### 1.通过stream流读取文件(createReadStream)

```js
const fs = require('fs')
// 通过流的方式,精准读取
const reader = fs.createReadStream('./foo.txt', {
  start: 3,
  end: 6,
  highWaterMark: 2 //表示每次读多少
})
// 监听读取到的data
reader.on('data', data => {
  console.log(data)
})
```

补充:

```js
reader.on('open', () => {
  console.log('文件被打开')
})
reader.on('close', () => {
  console.log('文件被关闭')
})
```

暂停和恢复

```js
reader.pause()

reader.resume()
```

### 2.通过Stream流写入文件(createWriteStream)

```js
const fs = require('fs')

const writer = fs.createWriteStream('./bar.txt', {
  flags: 'a',// win 系统'r+' 才能够实现
  start: 4
})

writer.write('你好啊', err => {
  if (err) {
    console.log(err)
  }
  console.log('写入成功')
})
```

文件的关闭

```js
// writer.close()
writer.end('hello srteam')
// end写文件的同时关闭,用的较多
writer.on('close', () => {
  console.log('文件关闭')
})
```

### 3.文件的读取和写入(pipe)

```js
const fs = require('fs')

const reader = fs.createReadStream('./foo.txt')
const writer = fs.createWriteStream('./foz.txt')

reader.pipe(writer)
writer.close()
```

## 九、http

### 1.http开发web服务器

```js
const http = require('http')

// 创建一个web服务器
const server = http.createServer((req,res) => {
  res.end('hello server')
})

// 启动服务器  (端口号0-65535, 主机, )
server.listen(8000, '0.0.0.0', () =>{
  console.log('服务器启动成功')
})
```

为了每次修改代码后，服务器能够自动重启可以安装工具`nodemon`

> npm install -g nodemon

通过`nodemon`启动服务器

> nodemon index.js

### 2.创建服务器的方式

```js
// 方式一
const server = http.createServer((req,res) => {
  res.end('hello server')
})

// 方式二
const server = new http.Server((req,res) => {
  res.end('hello server')
})
```

> 两种创建服务器的本质其实都是一样的

### 3.监听主机和端口号

- Server通过listen方法来开启服务器并且在某一个主机和端口上监听网络请求： 
  - 也就是当我们通过 ip:port的方式发送到我们监听的Web服务器上时
  - 我们就可以对其进行相关的处理
- listen函数有三个参数： 
  -  端口port: 可以不传, 系统会默认分配端, 后续项目中我们会写入到环境变量中；
  -  主机host: 通常可以传入localhost、ip地址127.0.0.1、或者ip地址0.0.0.0，默认是0.0.0.0； 
    - localhost：本质上是一个域名，通常情况下会被解析成127.0.0.1； 
    - 127.0.0.1：回环地址（Loop Back Address），表达的意思其实是我们主机自己发出去的包，直接被自己接收；
      - 正常的数据库包经常 应用层 - 传输层 - 网络层 - 数据链路层 - 物理层 ； 
      - 而回环地址，是在网络层直接就被获取到了，是不会经常数据链路层和物理层的； 
      - 比如我们监听 127.0.0.1时，在同一个网段下的主机中，通过ip地址是不能访问的； 
    - 0.0.0.0：
      -  监听IPV4上所有的地址，再根据端口找到不同的应用程序； 
      - 比如我们监听 0.0.0.0时，在同一个网段下的主机中，通过ip地址是可以访问的
-  回调函数：服务器启动成功时的回调函数

###  4.request对象

#### a.request对象url

```js
const http = require('http')
const url = require('url')
const qs = require('querystring')

// 创建一个web服务器
const server = http.createServer((req, res) => {
  // 1.request 对象等装了客户端向服务器端发送的所有信息

  if (req.url === '/login') {
    res.end('欢迎登录')
  } else if (req.url === '/home') {
    res.end('欢迎回来')
  } else {
    res.end('404错误请求')
  }
    
 // 通过qs解析url
  const {pathname, query } = url.parse(req.url)
  if(pathname === '/login') {
    console.log(query)
    console.log(qs.parse(query))
    const { username, password } = qs.parse(query)
    console.log(username, password)
  }
})

// 启动服务器  (端口号0-65535, 主机, )
server.listen(8000, '0.0.0.0', () => {
  console.log('服务器启动成功')
})
```

发送请求

> http://localhost:8000/login?username=aa&&password=123

打印结果：

```
username=aa&&password=123
[Object: null prototype] { username: 'aa', password: '123' }
aa 123
```

#### b.requset对象method，获取POST方法的body内容

```js
const http = require('http')
const url = require('url')
const qs = require('querystring')

// 创建一个web服务器
const server = http.createServer((req, res) => {
  const { pathname } = url.parse(req.url)
  if (pathname === '/login') {
    if (req.method === 'POST') {
      req.setEncoding('utf-8')
      req.on('data', data => {
        const { username, password } = JSON.parse(data) //将拿到的字符串转成对象
        console.log(username, password)
      })
    }
  }
  res.end('hello server')
})
```

### 5.response对象

#### a.response 响应对象

```js
const http = require('http')

// 创建一个web服务器
const server = http.createServer((req,res) => {
  // 响应结果 
  res.write('响应结果')
  res.end()
})
```

#### b.状态码

##### 1.请求状态码

```js
const server = http.createServer((req,res) => {
  // 设置状态码
  // 方式一： 直接给属性赋值
  res.statusCode = 400
  // 方式二： 和head一起设置
  res.writeHead(503)
})
```

##### 2.响应状态码

```js
const server = http.createServer((req,res) => {
  // 设置响应的header
  res.setHeader("Content-type", "text/plain;charset=utf-8")
  res.writeHead(200, {
    "Content-type": "text/plain;charset=utf-8"
  })
  res.write('响应结果')
})
```

#### c.http发送网络请求

**get请求**

```js
const http = require('http')

http.get('http://localhost:8000/login', res => {
  res.on('data', data => [
    console.log(data.toString())
  ])

  res.on('end', () => {
    console.log('获取到了所有结果')
  })
})
```

其他请只需要修改method方法

```js
const req = http.request({
  method: 'POST',
  hostname: 'localhost',
  port: 8000
}, res => {
  res.on('data', data => [
    console.log(data.toString())
  ])

  res.on('end', () => {
    console.log('获取到了所有结果')
  })
})
```

### 6.文件上传

原生的文件上传过于麻烦，真实的项目中采用框架，这里不再····

## 十、expresss框架

### 1.安装

脚手架安装：

> npm install -g express-generator

通过express创建项目

> express 项目名

安装依赖

> npm install 

运行项目

> node bin/www

### 2.初体验

```js
const express = require('express')

// express其实是一个函数： createApplication
// 返回app
const app = express()

// 监听默认路径
app.get('/', (req, res, next) => {
  res.end('hello express')
})
app.post('/login', (req, res, next) => {
  res.end('欢迎登录')
})
// 开启监听
app.listen(8000, () => {
  console.log('express启动成功')
})  
```

发送请求和相应的请求路径返回对应的结果

### 3.中间件

#### a.最普通的中间件

>注册多个普通的中间件的时候默认情况下只有第一个相应，只有使用next()才会调用下一个中间件

```js
const express = require('express')

const app = express()

// 编写普通的中间件
// use方式注册一个中间件
app.use((req, res, next) => {
  console.log('注册一个普通中间件01')
  res.end('结束请求')
  next()
})
app.use((req, res, next) => {
  console.log('注册一个普通中间件02')
  // res.end('结束请求') //如果这里写入了会报错，原因十因为上面的中间件已经结束了响应 
  next()
})

app.listen(8000, (req, res, next) => {
  console.log('普通的中间件')
})
```

#### b.路径匹配中间件

> 路径匹配中间件,只匹配路径不匹配方法，多个中间件也需要next执行

```js
const express = require('express')

const app = express()
// app.use(path, cb)
app.use('/home', (req, res, next) => {
  console.log('Home 01')
  res.end('home middleware')
  next()
})
app.use('/home', (req, res, next) => {
  console.log('Home 02')
}) 


app.listen(8000, (req, res, next) => {
  console.log('路径匹配中间件服务器启动成功~')
})
```

#### c.路径和方法匹配中间件

> 既匹配路径也匹配方法

```js
const express = require('express')

const app = express()

// 路径和方法匹配中间件
app.get('/home', (req, res, next) => {
  console.log('home path')
  res.end()
})
app.post('/login', (req, res, next) => {
  console.log('login path')
  res.end()
})

app.listen(8000, () => {
  console.log('路径和方法中间件服务器启动成功~')
})
```

#### d.连续注册中间件

> 同样也需要next()才能执行下一个中间件

```js
const express = require('express')

const app = express()

app.use((req, res, next) => {
  console.log('普通中间件')
  next()
})
app.get('/home', (req, res, next) => {
  console.log('连续注册中间件01')
  next()
}, (req, res, next) => {
  console.log('连续注册中间件02')
  next()
}, (req, res, next) => {
  console.log('连续注册中间件03')
  res.end('home page')
})

app.listen(8000, () => {
  console.log('连续注册中间件服务器启动成功~')
})
```

### 4.中间件的应用body参数解析（大项目插件： body-parse)

自己编写的body解析

```js
const express = require('express')

const app = express()

app.use((req, res, next) => {
  if (req.headers["content-type"] === 'application/json') {
    req.on('data', data => {
      const info = JSON.parse(data.toString())
      req.body = info
    })
    req.on('end', () => {
      next()
    })
  } else {
    // 不解析
    next()
  }
})

app.post('/login', (req, res, next) => {
  console.log(req.body)
  res.end('登录成功~')
})

app.post('/products', (req, res, next) => {
  console.log(req.body)
  res.end('上传商品成功~')
})

app.listen(8000, () => {
  console.log('服务器启动成功~')
})
```

当通过body  json传值的时候可以进行对应的解析，下面的方法更简单

```js
const express = require('express')

const app = express()

app.use(express.json()) // 直接进行json格式的解析

app.post('/login', (req, res, next) => {
  console.log(req.body)
  res.end('登录成功~')
})

app.post('/products', (req, res, next) => {
  console.log(req.body)
  res.end('上传商品成功~')
})

app.listen(8000, () => {
  console.log('服务器启动成功~')
})
```

如果是其他传值的方式怎么办呢？ 例如：表单传值：urlencoded

```js
app.use(express.urlencoded({ extended: true}))
// extended: trued 对urlencoded进行解析的时候使用的是第三方库qs 
// extended: false 对urlencoded进行解析的时候使用的是node内置模块querystring 
```

  

### 5.中间件的应用formdata的解析

express自带的解析工具没有办法帮助我们解析，需要下载第三方工具`multer`

> npm install multer

```js
const express = require('express')
const multer = require('multer')

const app = express()
const upload = multer()

app.use(upload.any())
app.post('/login', (req, res, next) => {
  console.log(req.body)
  console.log('登录成功')
})
app.listen(8000, () => {
  console.log('服务器启动成功~')
})
```

### 6.form-data文件上传

```js
const path = require('path')
const express = require('express')
const multer = require('multer')

const app = express()
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, './uploads/',)
  },
  filename: (req, file, cb) => {
    cb(null, Date.now() + path.extname(file.originalname))
      // 获取时间戳和原来文件的后缀名
  }
})

const upload = multer({
  // dest: './uploads/'
  storage
})

app.post('/login', app.use(upload.any()), (req, res, next) => {
  console.log(req.body)
  console.log('登录成功') 
})

app.post('/upload', upload.single('file'), (req, res, next) => {
  console.log(req.files)
  res.end('文件上传成功')
})

app.listen(8000, () => {
  console.log('服务器启动成功~')
})
```

**app.use(upload.any())不能放在全局使用**

### 7.中间件打印日志

先安装第三方日志库：`morgan`

> npm install morgan

```js
const express = require('express')
const morgan = require('morgan')
const fs = require('fs')

const app = express()
const writerStream = fs.createWriteStream('./logs/access.log', {
  flags: 'a+'
})

// combined : 打印日志的一种格式
// { stream: writerStream} : 保存日志的地方
app.use(morgan('combined', { stream: writerStream }))

app.get('/home', (req, res, next) => {
  res.end('hello world')
})

app.listen(8000, () => {
  console.log('服务器启动成功')
})
```

### 8.request参数解析

#### params

```js
const express = require('express')

const app = express()

app.get('/products/:id', (req, res, next) => {
  console.log(req.params)
  // console.log(req.query)
  res.end('商品详细数据')
})

app.listen(8000, () => {
  console.log('服务器启动成功')
})
```

###  9.响应数据

```js
const express = require('express')
const app = express()

app.get('/login', (req, res, next) => {
  // 返回数据方法一：
  // res.type('applicaton/json') // 设置返回的格式是json
  // res.end(JSON.stringify({name: 'xyl', age: 18}))
  // 返回数据方法二：(开发中使用最多)
  res.json({name: 'xyl', age: 18})
  // 返回状态,响应状态码
  res.statusCode(204)
})
app.listen(8000, () => {
  console.log('服务器启动成功')
})
```

### 10.路由的使用

`/routers/user.js`

```js
const express = require('express')

const router = express.Router()

router.get('/users', (req, res, next) => {
  res.json(['xyl', 'kobe', 'why'])
})
router.post('/users', (req, res, next) => {
  res.json('create user success')
}) 
router.get('/users/:id', (req, res, next) => {
  res.json(`${req.params.id}的用户信息`)
})

module.exports = router 

```

```js
const express = require('express')
const userRouter = require('./routers/user')

const app = express()
app.use("/", userRouter)


app.listen(8000, () => {
  console.log('服务器启动成功')
})
```

### 11.静态项目的部署

```js
app.use(express.static('./项目路径'))
```

## 十一、koa框架

### 1.koa初体验

安装koa

> npm init -y

> npm install koa

```js
const Koa = require('koa')

const app = new Koa()

app.use((ctx, next) => {
  console.log('中间件被执行')
  // 返回结果
  ctx.response.body = "我是返回的结果"
})

app.listen(8000, () => {
  console.log('koa服务器启动成功')
})
```

**注意：**

- koa中间件**只能通过use注册**，没有提供app.use('/login', ())这种方式
- 没有app.get,app.post等方法
- 也没有提供连续注册中间件的方式

### 2.koa中路由的使用

安装第三方库

> npm install koa-router

`routers/user.js`

```js
const Router = require('koa-router')

const router = new Router({prefix: '/users'})

router.get('/', (ctx, next) => {
  ctx.response.body = 'get request'
})
router.put('/', (ctx, next) => {
  ctx.response.body = 'put request'
})

module.exports = router
```

```js
const Koa = require('koa')
const userRouter = require('./routers/user')
const app = new Koa()

app.use((ctx, next) => {
 next()
})
app.use(userRouter.routes())
// 当没有实现的方法会提示该方法未实现
app.use(userRouter.allowedMethods())
app.listen(8000, () => {
  console.log('服务器启动成功')
})

```

### 3.koa中的params和query参数解析

koa中的params和query区别：

- 后端方法：`app.get("/deleteUser/:id", function(req, res) {...});`

- 请求URL：`http://localhost:8080/deleteUser/2?user=张三&passwd=zhangsan`

- 页面结果：

  ```js
  req.params: {
  	"user": "张三",
  	"passwd": "zhangsan"
  }
  req.query: {
  	"id": "2"
  }
  ```

`req.params`为查询参数对。而`req.query`是对应于后端代码中`:名称`的部分。

```js
const Koa = require('koa')
const Router = require('koa-router')
const userRouter = new Router({prefix: '/users'})

userRouter.get('/:id', (ctx, next) => {
  console.log(ctx.request.params)
  console.log(ctx.request.query)
})
const app = new Koa()

app.use(userRouter.routes())
app.listen(8000, () => {
  console.log('服务器启动成功')
})

/*
*结果：
{ id: 'abc' }
[Object: null prototype] { name: 'xyl', age: '18' }
*/
```

输入请求`localhost:8000/users/abc?name=xyl&age=18`

### 4.koa中的json和urlencoded解析

安装第三方库

> npm install koa-bodyparser

```js
const Koa = require('koa')
const bodyParser = require('koa-bodyparser')
const app = new Koa()

app.use(bodyParser()) // 使用第三方库进行解析
app.use((ctx, next) => {
  console.log(ctx.request.body)
})

app.listen(8000, () => {
  console.log('服务器启动成功')
})
```

### 5.koa中的form-data解析

安装第三方库

> npm install koa-multer

```js
const Koa = require('koa')
const bodyParser = require('koa-bodyparser')
const multer = require('koa-multer')
// const Router = require('koa-router')
const app = new Koa()

// const loginRouter = new Router({prefix: '/login'})
const upload = multer()

app.use(bodyParser())
app.use(upload.any())
app.use((ctx, next) => {
  console.log(ctx.req.body)
  ctx.response.body = 'hello'
})
// loginRouter.post('/',upload.any(), (ctx, next) => {
//   console.log(ctx.req.body)
//   console.log(ctx.request.body)
// })
// app.use(loginRouter.routes())

app.listen(8000, () => {
  console.log('服务器启动成功')
})
```

**两种方式，注释掉的是理由的方式**

### 6.文件上传

```js
const Koa = require('koa')
const Router = require('koa-router')

const app = new Koa()
const uploadRouter = new Router({prefix: '/upload'})

// 服务器跑起来自动生成文件夹
const upload = multer({
  dest: './uploads/'
})

uploadRouter.post('/avatar', (ctx, next) => {
  console.log(ctx.req.file)
  ctx.response.body = '上传头像成功'
})

app.use(uploadRouter.routes())

app.listen(8000, () => {
  console.log('服务器启动成功')
})
```

### 7.响应数据

```js
app.use((ctx, next) => {
  // ctx.response.body = {
  // }
  // 设置状态码
  ctx.status = 200
  // 设置内容
  ctx.response.body = []
})
```

### 8.静态服务器

安装第三方库

> npm install  koa-static

```js
const Koa = require('koa')
const koaStatic = require('koa-static')

const app = new Koa()
app.use(koaStatic('/home'))

app.listen(8000, () => {
  console.log('服务器启动成功')
})
```

## 十二、koa和express的区别

- express是完整和强大的，其中帮助我们内置了非常多好用的功能； 
- koa是简洁和自由的，它只包含最核心的功能，并不会对我们使用其他中间件进行任何的限制。 甚至是在app中连最基本的get、post都没有给我们提供； 我们需要通过自己或者路由来判断请求方式或者其他功能；

### next() 函数

- express 返回的不是promise
- koa返回的是promise

### 洋葱模型

![image-20210128221822774](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210128221822774.png)

## 十三、MySQL

### 1.数字类型

MySQL支持的数据类型有：数字类型，日期和时间类型，字符串（字符和字节）类型，空间类型和 JSON数 据类型。

- MySQL的数字类型有很多： 

  - 整数数字类型：INTEGER，INT，SMALLINT，TINYINT，MEDIUMINT，BIGINT；

  - 浮点数字类型：FLOAT，DOUBLE（FLOAT是4个字节，DOUBLE是8个字节）； 

  - 精确数字类型：DECIMAL，NUMERIC（DECIMAL是NUMERIC的实现形式）；

    ![image-20210128221331876](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210128221331876.png)

### 2. 日期类型

- YEAR以YYYY格式显示值 
  - 范围 1901到2155，和 0000。
- DATE类型用于具有日期部分但没有时间部分的值： 
  -  DATE以格式YYYY-MM-DD显示值 ； 
  -  支持的范围是 '1000-01-01' 到 '9999-12-31'； 
- DATETIME类型用于包含日期和时间部分的值： 
  - DATETIME以格式'YYYY-MM-DD hh:mm:ss'显示值； 
  -  支持的范围是1000-01-01 00:00:00到9999-12-31 23:59:59; 
- TIMESTAMP数据类型被用于同时包含日期和时间部分的值： 
  -  TIMESTAMP以格式'YYYY-MM-DD hh:mm:ss'显示值； 
  - 但是它的范围是UTC的时间范围：'1970-01-01 00:00:01'到'2038-01-19 03:14:07'; 
- 另外：DATETIME或TIMESTAMP 值可以包括在高达微秒（6位）精度的后小数秒一部分 
  -  比如DATETIME表示的范围可以是'1000-01-01 00:00:00.000000'到'9999-12-31 23:59:59.999999';

### 3.字符串类型

- CHAR类型在创建表时为固定长度，长度可以是0到255之间的任何值；
  - 在被查询时，会删除后面的空格； 
- VARCHAR类型的值是可变长度的字符串，长度可以指定为0到65535之间的值；
  -  在被查询时，不会删除后面的空格；
-  BINARY和VARBINARY 类型用于存储二进制字符串，存储的是字节字符串；
  -  https://dev.mysql.com/doc/refman/8.0/en/binary-varbinary.html 
- BLOB用于存储大的二进制类型；
- TEXT用于存储大的字符串类型；

### 4.聚合函数的使用

```sql
# 求手机价格总和
select SUM(price) totalPrice from `products`
# 求华为手机的价格总和
select SUM(price) totalPrice from `products` where brand ='华为'
# 求华为手机的平均价格华为
select AVG(price) totalPrice from `products` where brand ='华为'
# 最高和最低
select MAX(price) from = `products`
select MIN(price) from = `products`
# 求手机的个数
select COUNT(*) from `products` where brand = '华为'
select COUNT(DISTINCT price) from `products`
```

### 5.GROUP BY的使用

```sql
select brand,AVG(price),COUNT(*),AVG(score) from `profucts` GROUP BY brand
```

### 6.HAVING的使用

```sql
select brand,AVG(price) avgPrice,COUNT(*),AVG(score) from `profucts` GROUP BY brand HAVING avgPrice > 2000
```

- 需求：求评分大于7.5的手机的平均价格
  - 升级： 求评分大于7.5的手机，按照品牌分类求出平均价格

```sql
select brand, AVG(price) from 'products' where score > 7.5 GROUP BY brand
```

### 7.外键

#### a.创建外键

```sql
# 先添加字段
later table `profucts` ADD `brand_id` INT

# 修改brand_id为外键
alter table `products` add foreign key(brand_id) references brand (id);
```

#### b.外键存在时更新或者删除数据

我们可以给更新或者删除时设置几个值：

- RESTRICT（默认属性）：当更新或删除某个记录时，会检查该记录是否有关联的外键记录，有的话会报错的， 不允许更新或删除； 
- NO ACTION：和RESTRICT是一致的，是在SQL标准中定义的； 
- CASCADE：当更新或删除某个记录时，会检查该记录是否有关联的外键记录，有的话： 
  - 更新：那么会更新对应的记录； 
  - 删除：那么关联的记录会被一起删除掉； 
- SET NULL：当更新或删除某个记录时，会检查该记录是否有关联的外键记录，有的话，将对应的值设置为 NULL

```sql
# 第一步：查看表结构：
SHOW CREATE TABLE `products`;

# 第二步：删除之前的外键
ALTER TABLE `products` DROP FOREIGN KEY products_ibfk_1;

# 第三步：添加新的外键，并且设置新的action
ALTER TABLE `products` ADD FOREIGN KEY (brand_id)
					   REFERENCES brand(id)
					   ON UPDATE CASCADE ON DELETE CASCADE;
```

### 8.多表查询

#### a.获取到的笛卡尔乘积

```sql
SELECT * FROM `products`, `brand`;

SELECT * FROM `products`, `brand` WHERE `products`.brand_id = `brand`.id;
```

#### b.多表之间的连接 SQL JOIN

![image-20210130172102276](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210130172102276.png)

#### c.左连接

> 我们希望获取到的是左边所有的数据（以左表为主）：
>
> 完整写法是LEFT [OUTER] JOIN，但是OUTER可以省略的

![image-20210130172159192](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210130172159192.png)

```sql
SELECT * FROM `products` LEFT JOIN `brand` ON `products`.brand_id = `brand`.id;

SELECT * FROM `products` LEFT JOIN `brand` ON `products`.brand_id = `brand`.id
WHERE brand.id IS NULL;
```

#### d.右连接

> 我们希望获取到的是右边所有的数据（以由表为主）：
>
> 完整写法是RIGHT [OUTER] JOIN，但是OUTER可以省略的

![image-20210130172418720](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210130172418720.png)

```sql
SELECT * FROM `products` RIGHT JOIN `brand` ON `products`.brand_id = `brand`.id;

SELECT * FROM `products` RIGHT JOIN `brand` ON `products`.brand_id = `brand`.id
WHERE products.id IS NULL;
```

#### e.内连接

> 内连接是表示左边的表和右边的表都有对应的数据关联
>
> 内连接有其他的写法：CROSS JOIN或者 JOIN都可以；

![image-20210130172811193](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210130172811193.png)

```sql
SELECT * FROM `products` INNER JOIN `brand` ON `products`.brand_id = `brand`.id;
```

我们会发现它和之前的下面写法是一样的效果

```sql
SELECT * FROM `products`, `brand` WHERE `products`.brand_id = `brand`.id;
```

**但是他们代表的含义并不相同： **

- **SQL语句一：内连接，代表的是在两张表连接时就会约束数据之间的关系，来决定之后查询的结果；** 
- **SQL语句二：where条件，代表的是先计算出笛卡尔乘积，在笛卡尔乘积的数据基础之上进行where条件的帅 选；**

#### f.全连接

> SQL规范中全连接是使用FULL JOIN，但是MySQL中并没有对它的支持，我们需要使用 UNION 来实现：
>
> ![image-20210130172730398](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210130172730398.png)

```sql
(SELECT * FROM `products` LEFT JOIN `brand` ON `products`.brand_id = `brand`.id WHERE `brand`.id IS NULL)
UNION
(SELECT * FROM `products` RIGHT JOIN `brand` ON `products`.brand_id = `brand`.id WHERE `products`.id IS NULL);
```

### 9.连接查询PPT16

### 10.对象和数组

#### a.将查询到的数组转换成对象（一对多）

```sql
select products.id as id,products.title title
	   JSON_OBJECT('id',title)
from products left join bramd on products.brand_id = brand.id
```

#### b.将查询到的多条数组组织成对象放在数组中(多对多)

```sql
SELECT stu.id id, stu.name stuName, stu.age stuAge, cs.name csName, cs.price csPrice
JSON_ARRAYAGG(JSON_OBJECT(('id', cs.id. 'name', cs.name))
FROM `students` AS stu
RIGHT JOIN `students_select_courses` ssc On stu.id = ssc.student_id
RIGHT join `courses` cs on ssc.course_id = cs.id
GROUP BY stu.id
```

![image-20210131163118600](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210131163118600.png)

## 十四、node操作mysql

### 1.基本使用

安装mysql2

> npm install mysql2

```js
const mysql = require('mysql2')

// 1. 创建一个数据库连接
const connection = mysql.createConnection({
  host: 'localhost',
  port: 3306,
  database: 'school',
  user: 'root',
  password: 'root'
})

// 2. 执行sql语句
const statement = `select * from students`

connection.query(statement, (err, res, fields) => {
  console.log(res)
  connection.destroy()
})
```

### 2.预处理语句（推荐）

有点：

- 更加安全，防止sql注入
- 更好的性能

```js
const mysql = require('mysql2')

// 1. 创建一个数据库连接
const connection = mysql.createConnection({
  host: 'localhost',
  port: 3306,
  database: 'school',
  user: 'root',
  password: 'root'
})
// 2. 执行预处理语句
const statement = `select * from students where age > ?`

connection.execute(statement, [18], (err, res) => {
  console.log(res)
  connection.destroy()
})
```

### 3.连接池

当多个请求发过来请求数据，只有等上一个连接完成了释放后，性能很低，mysql2为我们提供了连接池

```js
const mysql = require('mysql2')

// 1. 创建连接池
const connections = mysql.createPool({
  host: 'localhost',
  port: 3306,
  database: 'school',
  user: 'root',
  password: 'root',
  connectionLimit: 10
})
// 2. 使用连接池
const statement = `select * from students where age > ?`
connections.execute(statement, [18], (err, res) => {
  console.log(res)
})
```

**promise方式**

```js
connections.promise.execute(statement, [18]).then(res => {
  console.log(res)
}).catch( err => {
  console.log(err)
})
```

### 4.sequelize的基本使用

安装`sequelize`和`mysql2`

> npm install mysql2 sequelize

```js
const { Sequelize } = require('sequelize')

const sequelize = new Sequelize('school', 'root', 'root', {
  host: 'localhost',
  dialect: 'mysql'
})

sequelize.authenticate().then(() => {
  console.log('连接数据库成功')
}).catch( err => {
  console.log('连接数据库失败')
  console.log(err)
})
```

### 5.sequelize单表操作的增改查

```js
const { Sequelize, DataTypes, Model, Op } = require('sequelize')

const sequelize = new Sequelize('school', 'root', 'root', {
  host: 'localhost',
  dialect: 'mysql'
})

// 测试连接
sequelize.authenticate().then(() => {
  console.log('连接数据库成功')
}).catch(err => {
  console.log('连接数据库失败')
  console.log(err)
})

// 建立关系映射
class Student extends Model { }
Student.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  name: {
    type: DataTypes.STRING,
    allowNull: false
  },
  age: {
    type: DataTypes.INTEGER,
  }
}, {
  tableName: 'students',
  createdAt: false,
  updatedAt: false,
  sequelize
})

async function queryName () {
  // 1. 查询数据库中所有的内容
  const res = await Student.findAll({
    where: {
      age: {
        [Op.gte]: 18 // 大于18岁
      }
    }
  })
  console.log(res)

  // 2. 插入数据
  const result = await Student.create({
    name: 'yogln',
    age: 22
  })
  console.log(result)

  // 3. 更新数据
  const result = await Student.update({
    age: 100,
  }, {
    where: {
      id: 7
    }
  })
  console.log(result)
}
queryName()
```

### 6.sequelize表一对多的操作

```js
const { Sequelize, DataTypes, Model, Op } = require('sequelize')

const sequelize = new Sequelize('school', 'root', 'root', {
  host: 'localhost',
  dialect: 'mysql'
})

// 测试连接
sequelize.authenticate().then(() => {
  console.log('连接数据库成功')
}).catch(err => {
  console.log('连接数据库失败')
  console.log(err)
})

// 建立关系映射
class Courses extends Model { }
Courses.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true,
  },
  name: {
    type: DataTypes.STRING,
    allowNull:false,
  },
  price: DataTypes.INTEGER
}, {
  tableName: 'courses',
  createdAt: false,
  updatedAt: false,
  sequelize
})

class Student extends Model { }

Student.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  name: {
    type: DataTypes.STRING,
    allowNull: false
  },
  age: {
    type: DataTypes.INTEGER,
  },
  courseId: {
    field: 'id',
    type: DataTypes.INTEGER,
    // 外键约束
    references: {
      model: Courses,
      key: 'id'
    }
  }
}, {
  tableName: 'students',
  createdAt: false,
  updatedAt: false,
  sequelize
})


// 将两张表联系在一起
Student.belongsTo(Courses, {
  foreignKey: 'courseId'
})

async function queryName () {
  const res = await Student.findAll({
    include: {
      model: Courses 
    }
  })
  console.log(res)
}
queryName()
```

### 7.sequlize表多对多的操作

```js
const { Sequelize, DataTypes, Model, Op, QueryTypes } = require('sequelize')

const sequelize = new Sequelize('school', 'root', 'root', {
  host: 'localhost',
  dialect: 'mysql'
})


// Courses
class Courses extends Model { }
Courses.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true,
  },
  name: {
    type: DataTypes.STRING,
    allowNull: false,
  },
  price: DataTypes.INTEGER
}, {
  tableName: 'courses',
  createdAt: false,
  updatedAt: false,
  sequelize
})
// Student
class Student extends Model { }
Student.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  name: {
    type: DataTypes.STRING,
    allowNull: false
  },
  age: {
    type: DataTypes.INTEGER,
  },
  courseId: {
    field: 'id',
    type: DataTypes.INTEGER,
    // 外键约束
    references: {
      model: Courses,
      key: 'id'
    }
  }
}, {
  tableName: 'students',
  createdAt: false,
  updatedAt: false,
  sequelize
})

// StudentCourses
class StudentCourses extends Model { }
StudentCourses.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  StudentId: {
    type: DataTypes.INTEGER,
    references: {
      model: Student,
      key: 'id'
    },
    field: 'student_id'
  },
  courseId: {
    type: DataTypes.INTEGER,
    references: {
      model: Courses,
      key: 'id'
    },
    field: 'course_id'
  }
},{
  tableName: 'students_select_courses',
  createdAt: false,
  updatedAt: false,
  sequelize
})

// 多对多关系的联系
Student.belongsToMany(Courses, {
  through: StudentCourses,
  foreignKey: 'student_id',
  otherKey: 'course_id'
})

Courses.belongsToMany(Student, {
  through: StudentCourses,
  foreignKey: 'course_id',
  otherKey: 'student_id'
})

// 查询
async function query() {
  const res = await Student.findAll({
    include: {
      model: Courses
    }
  })
  console.log(res)
}

query()
```

## 十五、允许跨域

![image-20210210122654813](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210210122654813.png)