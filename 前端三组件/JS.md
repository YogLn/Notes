### 1.正则表达式

**语法：var 变量 = new RegExp("正则表达式","匹配模式");**

`var reg = new RegExp("a","i");`

**第二个参数可选值：**

**i:忽略大小写；**

**g:全局匹配模式；**

使用test()方法检测一个字符串是否满足正则表达式的规则，是返回true,否则返回false。

**使用字面量来创建正则表达式**

**语法：var 变量 = /正则表达式/匹配模式**

`reg = /a/i;`

有a或者b：

```js
reg = /a|b/;        /*  /[ab]/ = a|b */
console.log(reg.test("abc"));
```

[a-z]:任意小写
[A-Z]: 任意大写

```js
reg = /^[ab]/;//除了ab
```

### 2.正则表达式与字符串的实用方法

- split()

  可以将一个字符串拆分为数组，可以传递一个正则表达式作为参数进行拆分

  默认全局匹配

  不能指定全局，只能返回第一个

  ```js
  var str = "1a2b3c4d5e6f7";
  var result = str.split(/[a-z]/);
  //输出1234567
  ```

- search()

  可以搜索字符串中是否含有指定内容，搜索到返回第一次出现的索引，否则返回-1；可以接受正则表达式作为参数然后去检索字符串。

  ```js
  var str = "hello abc hello abc";
  result = str.search(/a[ab]c/);
  //输出6
  ```

- match()

  根据正则表达式提起符合内容，默认只会提取第一个并停止检索，可以设置全局匹配模式就会找到所有内容。

  match()会将匹配到的内容封装到一个数组中返回。

  ```js
  str = "1a2a3a4a5e6f7A8B9C";
  result = str.match(/[a-z]/ig);
  console.log(result);
  //输出：a,b,c,d,e,f,A,B,C
  console.log(result[2]);
  //输出：c
  ```

- replace()

  可以将字符串的指定内容替换为新的内容，被替换的内容可以接受正则表达式。

  默认只会替换第一个，可以设置全局匹配

  ```js
  str = "1a2a3a4a5e6f7A8B9C";
  result = str.replace(/[a-z]/ig,"");
  console.log(result);
  //输出：123456789
  ```

  

### 3.正则表达式语法

- 量词

  通过两次指定一个内容的次数，只对他前边的内容起作用

  {n}表示出现n次

  {m,n}表示出现m-n次

  {m,}m次以上

  +表示至少一个,相当于{1，}

  *表示0个或者多个，相当于{0,}

  ？表示0个或者1个 ，相当于{0,1}

```js
var reg = /a{3}/;
//表示aaa;
var reg = /ab?c/;
```

- 检测以a开头的字符串

  ^ 表示以xx开头

  $ 表示以xx结尾

  ```js
  var reg = /^a/;
  var reg = /a$/;
  ```

  如果同时^和$,只能是一个字母

  判断手机号：

  ```js
  var phoneReg = /^1[3-9][0-9]{9}$/;
  ```

- 检查字符串是否含有.

  .表示任意字符，无法直接检测，使用\转义字符：

  `\.表示.`

  `\\表示\`

    

- \w:表示任意数字、字母、_   `[A-z0-9_]`

  \W:表示除了数字、字母、_   `[^A-z0-9_]`

  \d:任意数字 `[0-9]`

  \D:除了数字 `[^0-9]`

  \s:空格

  \S:除了空格

  \b:单词边界

  \B:除了单词边界 

  ```js
  //去除字符串空格,前后空格：
  var str = "   hello   ";
  str = str.replace(/^\s*|\s*$/g);
  ```

  

### 4.DOM查询

**节点的属性：**

|          | nodeName   | nodeType | nodeValue                         |
| -------- | ---------- | -------- | --------------------------------- |
| 文档节点 | #doucument | 9        | null                              |
| 元素节点 | 标签名     | 1        | null                              |
| 属性节点 | 属性名     | 2        | 属性值                            |
| 文本节点 | #text      | 3        | <font color="red">文本内容</font> |

```js
window.onload = function(){
//当页面执行完后再执行
}
```

**获取元素节点：**

- 通过**document**对象调用
  - getElementById():通过**id**属性获取**一个**元素的节点
  - getElementsByTagName():通过**标签名**获取**一组**元素的节点对象
  - getElementsByName():通过**name**属性获取**一组**元素节点对象

- 获取元素的子节点：
  - getElementsByTagName():方法，返回当前节点的指定标签名后代节点
  - childNodes:属性，表示当前节点的所有子节点，会获取所有节点，空白、换行会当作文本节点
  - firstChild:表示当前节点的第一个子节点，包括空白文本节点
  - lastChild:表示当前节点的最后一个子节点

- 获取父节点和兄弟节点

  - parentNode:表示当前节点的父节点
  - previousSibling:表示当前节点的前一个兄弟节点
  - previousElementSibling:获取前一个兄弟元素
  - nextSibling:表示当前的的后一个兄弟节点

- 补充：

  - document.body:获取body标签

  - document.documentElement:获取html标签

  - document.all:代表页面中所有元素的个数

  - document.getElementsByClassName():IE8及以下不支持

  - document.querySelector():需要一个选择器的字符串作为参数，可以更具一个CSS选择器来查询一个元素的节点对象,IE8可支持

    ```js
    //获取box1下的div元素
    var div = document.querySelector(".box div");//.是必须要有的
    ```

  - document.querySelectorAll():返回一个NodeList



### 5.DOM的增删改

- 增加节点：

```js
var li = document.creatElement("li");
//用于创建元素节点对象，用标签名作为参数
                                
var Text = document.creatTextNode("广州");
//可以用来创建文本节点对象

//把新的子节点添加到子节点
//父节点.appendChild(子节点);
li.appendChild(Text);

//获取节点
var city = document.getElementById("city");
//方法一：

//将广州添加到节点下
city.appendChild(li);
//将节点插入到指定对象前面insertBefore
//父节点.insertBefore(新节点，旧节点)
city.insertBefore(li,bj);

//方法二：
city.innerHTML += "<li>广州</li>";

//方法三：
//一般两种方式结合使用
var li = document.creatElement("li");
li.innerHTML = "广州";
city.appendChild(li);
```

- 替换节点

```js
//使用新节点替换已有节点
//父节点.replaceChid(新节点,旧节点);
city.replaceChile(li,bj);

```

- 删除节点

```js
//父节点.removeChild(子节点);
city.removeChild(bj);
//子节点.parentNode.removeChild(子节点);更为常用
```

### 6.通过JS修改元素的样式内联样式

**语法：元素.style.样式名 = 样式值**

```js
box.style.width = "300px";
```

**注意：如果css样式中含有 "-"，则不合法，需要将样式名i需改为驼峰命名法。**

```js
box.style.backgroundColor="red";
```

**如果样式中含有<font color="red">!important</font>则JS修改样式失效，所以尽量不要为CSS添加<font color="red">!important</font>**

获取元素的样式：

```js
alert(box.style.with);//获取为内联样式，不为内敛样式则无法读取
```

获取元素当前显示样式：

语法：元素.currentStyle.样式名      **只有IE浏览器支持**

```js
alert(box.currentStyle.with);//当前生效的样式值，哪个生效就获取哪个	
```

getComputedStyle()需要两个参数，返回一个对象	     **其他浏览器及IE9以上都支持**

- 第一个参数：要获取样式的元素
- 第二个参数：可以传递的伪元素，一般都为null

```js
alert(getComputedStyle(box,null).width);
```

#### 通用getstyle()

```js
function getStyle(obj,name){
    if(window.getComputedStyle){
        //正常浏览器方式
        return getComputedStyle(obj,null)[name];
    }else{
        //IE8方式
        return obj.currentStyle[name];
    }
}
```

### 7.事件

- 事件对象

  当事件的响应函数被触发时，浏览器每次都会将事件的一个事件对象作为实参传递进响应函数。

  ```js
  window.onload = function(event){
      event = event || window.event;
  }
  ```

  

- 事件冒泡(Bubble)

  指的是事件的向上传导，当后代元素被触发时，祖先元素也会被触发

  在大部分的开发中冒泡都是有用的，如果不希望发生冒泡，可以通过事件对象取消。

  ```js
  //设置为true取消冒泡
  event.cancelBubble = true;
  ```

- 事件的委派

  将我们的事件统一绑定给共同的祖先元素。利用了事件冒泡，减少事件绑定的次数，提高效率。

- 事件的绑定

  addEventListener()可为相同元素绑定多个事件

  - 通过这个方法为事件绑定响应函数
  - 事件的字符串不要用on
  - 回调函数，当事件触发时，该函数会被调用
  - 是否在捕获阶段触发事件，需要传一个布尔值，一般都为false

  ```js
  btn.addEventListener("click",function(){},false);
  ```

### 8.类的操作

- 通过修改类来间接改变CSS样式，浏览器只需要重新渲染页面一次，性能较好

  ```js
  .b1{
      //b1的样式
  }
  .b2{
      //b2的样式
  }
  
  
  box.className = "b2";
  box.className += " b2";//原有的b1样式和b2样式都有
  ```

  

### 9.Json

特殊格式的字符串，可以被任意语言识别并转换位任意语言对象。

- json中的属性名必须加<font color="red">**""**</font>,其他一样。
- json中的值可以是字符串、数值、布尔值、null、对象、数组。

**将json字符串转换为js对象：**

```js
var json = '{"name":"孙悟空","age":18}';
var obj = JSON.parse(json);
console.log(obj.name);
```

**将js对象转换为json字符串：**

```js
var str = JSON.stringify(obj);
```

如果需要兼容IE7及以下的json操作，则可以通过引入一个外部的js文件来处理。

### 10.AJAX

AJAX:异步的JavaScript和XML,无刷新页面与服务器交互技术。

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200902151736564.png" alt="image-20200902151736564" style="zoom: 67%;margin-left:45px" />









