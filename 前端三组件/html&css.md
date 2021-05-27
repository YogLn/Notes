

### 1.实体的语法

**&实体的名字;**

```html
<!-- 空格-->
&nbsp;
<!-- 大于-->
&gt;
<!-- 小于-->
&lt;
<!-- 版权符号-->
&copy;
```

[更多实体](https://www.w3school.com.cn/html/html_entities.asp)

### 2.meta标签

**meta用于设置网页中 的元数据，元素据不是给用户看的，给浏览器看的**

- charset:指定网页的字符集

- name:指定数据的名称

- content:指定数据的内容

  - ​	keywords:表示网页的关键字，指定多个关键字 用,隔开
  - ​	description:用于对网站的描述
  - ​	title:标签内容作为搜索结果的超链接上的文字显示

- refresh:用于网页的重定向

  ```html
  <meta http-equiv="refresh" content="3,url=http://www.baidu.com">
  ```

### 3.语义化标签

```html
<!-- 表示斜体，不换行-->
<em></em>
```

```html
<!--表示一个长引用，自动缩进，换行-->
<blockquote>
    这句话我没说过！
</blockquote>
```

```html
<!--行内元素标签，不换行-->
<q></q>
```

```html
<hgroup>
	<h1> </h1>
	<h1></h1>
</hgroup>
```

```html
<header>网页的头部</header>
<main>网页的主体部分</main>
<footer>网页的底部</footer>
<nav>表示网页的导航</nav>
<aside>和主题相关的内容(侧边栏)</aside>
<artticle>表示一个独立的文章</artticle>
<section>表示一个独立的区块，上边的标签都不能不用sections</section>
<div>没有语义，表示区块，可代表上面的所有</div>
<span>行内元素，没有语义，用于显示选中的文字</span>
```

### 4.列表

1. 有序列表

   ```html
   <ol>
       <li></li>
   </ol>
   ```

2. 无序列表

   ```html
   <ul>
       <li></li>
   </ul>
   ```

3. 定义列表

   ```html
   <dl>
       <dt>结构</dt>
       <dd>结构表示的网页，结构用来规定网页中哪里是标题哪里是段落</dd>
   </dl>
   ```

   **列表之间可以互相嵌套**

### 5.超链接(行内元素)

```html
<a href="https://www.baidu.com">超链接</a>
```

target:指定超链接打开的位置：

​	_self:默认，在原有窗口中打开超链接

​	_blank:在一个新页面中打开超链接

```html
<a id="bottom" href="#">回到顶部</a>
<a href="#bottom">回到底部</a>
<a href="#p3">去第三个自然段</a>
```

### 6.图片标签

```html
<!-- 替换元素，基于行内元素和块元素之间，具有两种元素的特点-->
<img src="图片的路径" art="对图片的描述" width="" height="">
```

***建议：一般在pc端不建议修改图片的大小，但是在移动端需要对图片进行缩放***

**图片的格式：**

- jpeg(jpg):支持颜色丰富，不支持透明效果，不支持动图
- gif:支持颜色较少，支持简单透明，支持动图
- png:支持颜色丰富，支持复杂透明，不支持动图
- webp:谷歌新推出用来表示网页图片的一种格式，可替换上边3种，缺点：兼容性不好
- base64:将图片使用base64进行编码，可以将图片转换位字符，一般用于网页和图片一起加载的场景

### 7.内联框架

```html
<iframe scr="指定引入网页的路径" width="" height="" frameborder="有无边框0/1"></iframe>
```

### 8.音视频

```html
<!--
audio:默认不允许用户自禁操作
属性：
	controls：是否允许用户控制播放
	autoplay:自动播放
	loop:循环播放
-->
<audio src="" controls autoplay>引入一个外部的音频文件</audio>
```

```html
<!--
使用方式同audio
-->

<vedio src="">引入一个视频资源</vedio>
```

### 9.复合选择器

- 交集选择器

  ```html
  div.red{
  
  }
  <div class="red">
  </div>
  ```

- 并集选择器(选择选择器)

  作用：同时选择多个选择器对应的元素

  语法：选择器1，选择器2，选择器3

  ```html
  h1,span{
  
  }
  ```

### 10.关系选择器

- 子元素选择器：

  ```html
  div > span{
  	color:red;
  }
  
  div.box > span{
  	color:red;
  }
  ```

- 后代元素选择器：

  ```html
  div span{
  	color:red;
  }
  div > span > p{
  	color:red;
  }
  ```

- 兄弟选择器：

  ```html
  p+span{
  前一个 + 下一个
  }
  p~span{
  兄 ~ 弟
  }
  ```

### 11.元素选择器伪类

```html
a:link {//link就是链接的意思，代表当一段文本为链接时的属性。
	color: blue;
}
a:visited {//访问过的,只能修改颜色
	color: gray;
}
a:hover {//悬停
	color: red;
}
a:active {//active就是有效的、快速的意思，代表鼠标按下时一瞬间的属性。
	color: yellow;
}
```

### 12.伪元素选择器

```css
//选择文本的第一个字母
p::first-letter{
line
}
//选择文本的第一行
p::first-line{

}
//选择文本的内容
p::first-selection{

}
//before和after必须结合content使用
div::before{
    content:'最开头添加的内容';
}
div::after{
    content:'最结尾添加的内容';
}
```

### 13.em和rem

**em：相对于元素的字体大小来计算，1em=1font-size;会根据元素的字体大小的改变而改变，1em默认是16px**

**rem:相对于根元素`html`的字体大小来改变**

### 14.文档流

块元素：默认占一行，自上向下排列，默认宽度是父元素的全部，默认高度是内容撑开

行内元素：不会独占一行，自左向右水平排列，宽度和高度被内容撑开

### 15.盒子模型

每个盒子都由以下几个部分组成：

- 内容区(content)
- 内边距(padding)
- 边框(border):border-width;border-color;border-style(边框大小会影响盒子大小 )
- 外边框(margin)

**水平方向布局：**

```css
margin-left+border-left+padding-left+width+padding-right+border-right+margin-right=其父元素的宽度
```

**以上等式必须满足，如果不成立则称为过渡约束，则等式会自动调整，如果没有auto的情况，浏览器自动调整margin-right使等式满足，可以设置auto的只有`width、margin-left、margin-right`**

```css
{
    //常用居中
    widyth:xxxpx;
    margin:0 auto;
}
```

**垂直方向布局**

子元素大于父元素：

```css
overflow:visible;默认值，子元素从父元素溢出`
overflow:hidden;溢出内容被裁剪`
overflow:scroll;生成滚动条`
overflow:auto;根据需要生成滚动条`
```

垂直外边距的重叠：

- 兄弟元素：取较大值
- 父子元素：子元素的外边距会传递给父元素 

**行内元素的布局**

```css
display:inline;//将元素设置为行内元素
display:block;//将元素设置为块元素
display:inline-block;//将元素设置为行内块元素
display:table:将元素设置为一个表格
display:none:元素在页面中不显示
```

```css
visibility:visible;//默认值，正常显示
visibility:hidden;//不显示，但还占据位置
```

### 16.BFC(块级格式化环境)

BFC是CSS中的一个隐含的属性，可以为一个元素开启BFC，开启后该元素会变成一个独立的布局区域。<font color="red">解决元素塌陷的问题</font>。 

**开启BFC方式：**

- 设置元素浮动（不推荐）

- 将元素设置为行内块元素（不推荐）

- 将元素的overflow设置为一个非visible的值`overflow:hidden;overflow:auto;`(副作用小)

- 利用`clear:both`开启

  ```css
  .box::after{
      content:'';
      display:block;
      clear:both;
  }
  ```


**开启后的特点：**

- 开启BFC的元素不会被浮动元素覆盖
- 开启BFC的元素子元素和父元素的外边距不会重叠
- 开启BFC的元素可以包含浮动元素

### 17.clear

```css
clear:left;
clear:right;
```

- 作用：清除浮动元素对当前元素的影响
- 可选值：
  - left:清除左侧元素对当前元素的影响
  - right:清除右侧元素对当前元素的影响
  - both:清除两侧中影响最大的一侧
- 原理：设置清除浮动以后，浏览器会自动为元素添加一个上外边距，使其位置不受影响

### 18.less(CSS的预处理语言)

vscode安装easy-less插件，从而简化css代码开发

```css
//csss原生自持变量的设置
--变量名:值;
html{
    --color:#bfa;
}
.box{
    background-color:var(--color);
}

```

```less
//less变量
@变量名:变量值;
@a:100px;
.box{
    width:@a;
}
//less类
.@{a}{
    
}
```

### 19.弹性盒布局

**弹性容器：**通过display设置弹性容器

```css
display:flex;
display:inline-flex;
```

**弹性元素：**弹性容器的直接子元素，一个元素可以同时是弹性容器和弹性元素 

- 主轴：弹性元素排列的方向
- 侧轴：与主轴垂直的方向

```css
//指定容器中弹性元素的排列方式(方向)
flex-direction:row;//默认
flex-direction:row-reverse;
flex-direction:colum;
flex-direction:colum-reverse;
```

**弹性元素的属性：**

- 用法：flex: 增长 缩减 基数

```css
flex:1 1 auto;
flex:initail;//flex:0 1 auto;
flex:auto;//flex:1 1 auto;
flex:none;//flex:0 0 auto;//弹性元素没有弹性
```

- 基数

```css
flex-basis:auto;//默认值，表示参考元素的自身高度和宽度，如果给出了值就参考值为准
```

- 伸展性：

```css
flex-grow:0;//默认值
flex-grow:1;//可按比例伸长
```

- 收缩性：

```css
flex-shrink:0;//默认值
flex-shrink:1;//可按比例收缩
```

- 设置是否在弹性容器中自动换行:

```css
flex-wrap:nowarp;//默认值，元素不会自动换行
flex-wrap:warp;//元素沿着辅轴方向自动换行
flex-wrap:warp-reverse;
```

- 如何分配主轴上的空白空间(主轴上的元素如何排列)

可以通过order:数值；来排列顺序。

```css
justify-content:flex-start;//元素沿着起边排列，有空白留最后
justify-content:flex-end;
justify-content:center;
justify-content:space-around;//空白分布到元素两侧
justify-content:space-between;//空白均匀分布到元素间
justify-content:space-evenly;//空白分布到元素的单侧
```

- 元素在辅轴上的对齐

```css
align-items:stretch;//默认值，将元素的长度中设置为相同的值
align-items:flex-start;//元素不会拉伸，沿着辅轴起边对其
align-items:flex-end;//沿着辅轴的终边对其
align-items:center;//垂直居中对其


//设置对齐
justify-content:center;
align-items:center;
```

### 20.视口

**完美视口：**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
```

**vw(viewport width)**:表示视口的宽度

<font color="red">100vw=一个视口宽度，1vw=1%视口宽度，</font>用于移动端适应不同的设备

### 21.响应式布局

使用响应式布局可以适应所有设备，响应式布局的关键就是媒体查询，通过不同的设备来设置样式。

**使用媒体查询：**

语法：@media  查询规则{ }

**媒体类型：**

- all：所有设备
- print：打印设备
- screen：带屏幕的设备
- speech：屏幕阅读器

可以使用,连接多个类型，他们之间是一个或的关系

```css
@media print,screen{
    body{
        color:red;
    }
}
```

可以在媒体类型前添加一个only表示仅仅、只用，only的使用主要是为了兼容老版本的浏览器。

**媒体的特性：**

- width:视口的宽度
- height:视口的高度
- min-width:视口的最小宽度(视口的宽度大于min-width生效)
- max-width:视口的最大宽度(视口的宽度小于max-width生效)

```css
@media (width:500px){
    body{
        //只有当视口的宽度为500px生效,一般不适用
    }
}

@media (min-width:500px){
    body{
        //只有当视口的宽度大于500px生效
    }
}


@media (min-width:500px) and (max-width:700){
    body{
        //只有当视口的宽度大于500px小于700px生效
    }
}


@media only(not) screen and (min-width:500px),(max-width:300px){
    body{
        //当视口的宽度大于500px小于300px生效
    }
}
```

**断点：样式切换的分界点**

常用断点：

- 小于768：超小屏幕 max-width=768px;
- 大于768：小屏幕 min-width=768px;
- 大于992：中型屏幕 min-width=992px;
- 大于1200：大屏幕 min-width=1200px;

**横竖屏判断：**

**orientation: portrait（竖屏，即设备中的页面可见区域高度>=宽度）**

**orientation: landscape（横屏，即设备中的页面可见区域高度<=宽度）**

```css
@media screen  and (orientation:portrait){
ul{
    
}
li{
    
}
}
```

移动端和PC端大多数使用断点来做。