# Android Studio 学习笔记(三)：简单控件及实例

本文针对常用控件(Textview、Button、EditText、RadioButton、CheckBox、ImageView)进行简单说明

## 控件、组件、插件概念区分

说到控件，就不得不区分一些概念。

控件(Control):编程中用到的部件

组件(Component):软件的组成部分

插件(plugin): 应用程序中已经预留接口的组件

下面举些例子

Android的系统控件:

- Textview
- ImageView
- Button
- ...

Android的四大组件

- Activity
- Service
- Content Provider
- Broadcast Receiver

网页上常用插件莫过于flash了，用于视频加载播放。

## 控件说明

和布局类似控件至少需要设置的属性为**android:layout_width**和**android:layout_height**，分别控制布局的宽度和高度，一般填入的参数为match_parent和wrap_content，也可填入具体尺寸 。

出于实际需要，**android:id**也必不可少，id属性只能接受资源类型的值，也就是必须以@开头的值，在R中用一个int类型的值来表示。比如@+id/xyz，在R.java中表示为`int xyz = value`，其中value是一个十六进制数。

值得说明的是**@id和@+id的区别**

- @+id，就是在R.java文件里新增一个id名称，如果之前已经存在相同的id名称，那么会覆盖之前的名称。
- @id，则是直接引用R.java文件的存在的id资源，如果不存在，会编译报错。

## Textview 显示文本框

布局文件中常用的几个单位

- **dp(dip)**: device independent pixels(设备独立像素)不同设备有不同的显示效果
- **px: pixels(像素)**: 不同设备显示效果相同
- **sp: scaled pixels(放大像素)**: 主要用于字体显示
- **pt:(point**):是一个标准的长度单位，1pt＝1/72英寸，不常用

可实现的常用功能：

- 可以设置四个方向的图片: **drawableTop**(上),**drawableButtom**(下),**drawableLeft**(左),**drawableRight**(右)

- 当文字中出现了URL，E-Mail，电话号码的时候，我们可以通过设置**android:autoLink**属性；当我们点击对应部分的文字，即可跳转至某默认APP，比如一串号码，点击后跳转至拨号界面！

- 文字效果，如跑马灯，阴影等等

- **android:ellipsize="end"** 内容超过屏幕宽度，尾部显示省略号

- 实现跑马灯效果(文字循环滚动)

  ```
  android:singleLine="true" //单行显示
  android:ellipsize="marquee" //文字循环滚动
  android:marqueeRepeatLimit="marquee_forever" //设定为永久循环
  android:focusable="true" //获取键盘焦点
  android:focusableInTouchMode="true" //获取触屏焦点
  ```

  简单介绍下这里的**焦点**，获取焦点的控件就是当前可以执行操作的控件。举个例子，当我们点击文本框时，会出现光标闪烁，在光标处可以输入文字，这里的文本框就获取了焦点，单行显示如果用**android:maxLines="1"**会报错。

  这样的设计存在一个缺陷——当前界面的其他控件都无法获取焦点了，也就无法在本界面上进行其他操作，仅仅起到演示效果，而且若同时设置多个跑马灯只有第一个会有响应。*(还遇到一个坑，就是在我的红米真机上显示不出跑马灯效果，有待填坑)*所以需要设计无焦点的跑马灯

*动图的下方图标是水印*

![img](http://m.qpic.cn/psc?/V13rVnAS4cHwTO/WUyRLVwskOVTItG8F0x76xvpzXlZz0Xowimb.0S41Z5Nnc4ULMlEe7mfAkNB.sthZVUAUP89UpnzZSQZZiXD3Vcu9G7886RnXacUw0b..oY!/b&bo=zgGsAwAAAAACB0M!&rf=viewer_4&t=5)

## Button 按钮

Button一般需要实现的效果是按下后发生某种响应，跳转。 Button是TextView的子类，Textview也可像Button一样设置点击事件

点击效果的原理是:按下前为浅色，按下后变为深色，当然颜色选得恰当的话效果会更好。一种做法是在**android:background**中引用Drawable资源文件，设置**< item android:state_pressed="xx">**中xx分别为true和false的情况下，按钮状态的改变

按钮水波效果实现可见https://www.runoob.com/w3cnote/android-tutorial-button-imagebutton.html

动图中，如果未设置按钮状态改变，看不出按钮点击效果，于是我加入了点击时带圆点提示

![img](http://m.qpic.cn/psc?/V13rVnAS4cHwTO/WUyRLVwskOVTItG8F0x76xhqdmZlO10i4d7hQ5CLH2sBcAPTCl7EESC6ZynZYlPxBMsfVcF*fgr2J9.d2HsOEwsTy5XezfMwJsBDOsnMdwU!/b&bo=WAI1BQAAAAACB0k!&rf=viewer_4&t=5)

## EditText 可输入文本框

常用的实用属性如下

- **android:hint** 输入时输入框的提示文本
- **android:inputType** 指定输入类型，如果指定为phone类型，手机键盘会自动弹出数字键盘，同时输入也只接受数字类型。
- **android:maxLines="1"** 指定输入框的最大行数为1，防止格式变形

demo只演示了数字输入，可以看到号码不接受数字外的其他类型输入

![img](http://m.qpic.cn/psc?/V13rVnAS4cHwTO/WUyRLVwskOVTItG8F0x767hbBkY*wEoR6uWQ2pzwW0NFVkS7q*ysamu6wtq9sZuCGKF3gxf3Rf8jPdZOsy2J.ScpEZDXZHkwERc4w5PvXZs!/b&bo=WAI1BQAAAAACRwk!&rf=viewer_4&t=5)

## RadioButton 单选按钮

使用方法如下

比如出一道单选题，选项的集合就是布局文件中的<RadioGroup>，里面嵌套几个<RadioButton>，每个各代表一个选项，选项内容由**android:text**决定。

为了方便起见，通常在用户未选时设置默认选中的选项，在该选项中设置属性**android:checked="true"**

选项默认的样式是在文本前加点表示选中，也可以采用文本框对纯色填充的样式。则需要在**android:background**中引用Drawable资源文件，设置**< item android:state_checked="xx">**中xx分别为true和false的情况下，按钮状态的改变。

![img](http://m.qpic.cn/psc?/V13rVnAS4cHwTO/WUyRLVwskOVTItG8F0x76xO7d.Br58Gal4DMgu61EvV1n.l0chpkE8IpNUyEqd86GKM.swrCMJfv1Lp.zo6adSePw88nHhe2vCKvUE2a6XA!/b&bo=WAI1BQAAAAACRwk!&rf=viewer_4&t=5)

## CheckBox 复选框

相当于多选题，区别RadioButton的地方在于，不需设置一个选项集合，而是

题目可由TextView显示，每个选项由一个<CheckBox>表示，选项内容由**android:text**决定。

选项的按钮样式可通过引用Drawable资源文件，在Drawable资源文件中进行修改设置。

![img](http://m.qpic.cn/psc?/V13rVnAS4cHwTO/WUyRLVwskOVTItG8F0x767M*rnQ3iJT.vQtbdmUKSJmDlgbbVY649PHh0Zm2bt8AIo0NgpBsBIO7EnsYOp1ZjDcEfkNfpZh6aSLNfm*B0VE!/b&bo=WAI1BQAAAAACRwk!&rf=viewer_4&t=5)

## ImageView 图像视图

ImageView有两个可以设置图片的属性，分别是：src和background

- background通常指的都是**背景**,而src指的是**内容**!!
- 当使用src填入图片时,是按照图片大小**直接填充**,并**不会进行拉伸**

而使用background填入图片,则是会根据ImageView给定的宽度来进行**拉伸**

图片缩放类型可由两种控制方式设置:

- 布局文件中设置 **android:scaleType** 属性
- Java代码中设置**ImageView.setScaleType()**方法

ImageView源码中暂时没有实现加载网络图片的方法，当然在网上有很多种方法，这里介绍其中一种：使用第三方的库Glide加载网络图片。

按照天哥的视频进行配置 build.gradle，一开始报错了，说一下修改遇到的坑:

- 需要修改的build.gradle是在app目录下的，不是另一个
- 当前版本使用的是androidx，因此会找不到android.v7包，改成当前版本下的appcompat即可，其余同理
- 当前版本的compile都建议修改为implement

![img](https://img2018.cnblogs.com/blog/1938293/202002/1938293-20200216171008214-2020468956.jpg)

## 参考资料

1. 组件、控件和插件的区别

   https://blog.csdn.net/haiross/article/details/22662635

2. TextView、ImageView详解|菜鸟教程

   https://www.runoob.com/w3cnote/android-tutorial-textview.html，https://www.runoob.com/w3cnote/android-tutorial-imageview.html

3. Android 基础之简单控件

   https://www.jianshu.com/p/a32f639bc756

4. @id和@+id的区别

   https://blog.csdn.net/bzlj2912009596/article/details/80491153

5. 【天哥】Android开发视频教程最新版 Android Studio开发

   https://www.bilibili.com/video/av38409964

6. Android 获取焦点

   https://zhidao.baidu.com/question/455464125905865485.html

7. Android之跑马灯详解

   https://www.jianshu.com/p/67e74148a122