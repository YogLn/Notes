# [Android Studio 学习笔记(二)：布局简介和xmlns说明

## Android 布局简介

可以把界面设计看作是，让各个控件都摆放得有条不紊，各得其所。布局就相当于放置控件的容器，通过控制布局参数，可以按照一定规律调整内部控件的位置。

一个丰富的Android界面由**UI控件和结构化布局对象**组成。

常见布局可分为以下几类：

- 线性布局(LinearLayout)
- 相对布局(RelativeLayout)
- 表格布局(TableLayout)
- 网格布局(GridLayout)
- 帧布局(FrameLayout)

此外，还有基本不会使用的绝对布局(AbsoluteLayout)和采用可视化界面拖拽控件的约束布局(ConstraintLayout)

布局至少需要设置的属性为**android:layout_width**和**android:layout_height**

分别控制布局的宽度和高度，一般填入的参数为match_parent[[1\]](https://www.cnblogs.com/ifever/p/12309148.html#fn1)和wrap_content[[2\]](https://www.cnblogs.com/ifever/p/12309148.html#fn2)，如果有需要也可填入具体尺寸

下面介绍线性布局和相对布局的几个特有属性。

## 线性布局

- `LinearLayout`用于使所有子视图在单个方向（垂直或水平）保持对齐，方向由**android:orientation**决定，默认值为vertical(垂直)，可改为horizontal(水平)
- 若需要让子元素使用大小不同的屏幕空间 ，可设置权重划分属性 **android:layout_weight**，规则是控件先按占用设置的宽度或高度，剩余的宽度或高度按设置权重相应的比例来分配。

## 相对布局

- `RelativeLayout` 用于以相对位置显示子视图。可以指定为相对于同级元素定位（例如在另一个视图的左侧或下方）或相对于父级区域定位（例如在左侧、底部或中心对齐）

- margin与padding的区别:

  margin代表的是偏移,针对的是容器中的组件

  padding代表的则是填充,针对的是组件中的元素

- gravity类似于Word里面的设置文字对齐方式，可以设置容器内组件的对齐方式，

**线性布局比较适合所有控件都是整齐排列的页面，加载速度较慢。**

**相对布局可以按照自己的想法来放置控件的位置，但写起来比较麻烦，需要自己考虑好所有控件的的布局。**

## 代码说明

简单分析下布局文件xml中的开头:

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout 
xmlns:android="http://schemas.android.com/apk/res/android"    
xmlns:app="http://schemas.android.com/apk/res-auto"    xmlns:tools="http://schemas.android.com/tools"
...
```

version="1.0":声明用的xml版本是1.0

encoding="UTF-8":声明用xml传输数据使用的字符编码为UTF-8。(UTF-8可传中文)

下面是三个同样的句式，xmlns:namespace-prefix="namespaceURI"

xmlns:保留字`xml namespace`的缩写，意思是xml命名空间

namespace-prefix:命名空间前缀

namespaceURI:命名空间唯一标识符 URI[[3\]](https://www.cnblogs.com/ifever/p/12309148.html#fn3)

xmlns:android="http://schemas.android.com/apk/res/android"

声明这个命名空间引用自Android系统

xmlns:app="http://schemas.android.com/apk/res-auto"

导入自定义控件的一些属性或者support支持包

xmlns:tools="http://schemas.android.com/tools"

用于调试预览，打包为apk的时候所有tools添加的属性都会被摒弃，方便进行页面调整

![img](https://img2018.cnblogs.com/blog/1938293/202002/1938293-20200214191441684-1839225343.png)

> 图片来自——Android布局文件中的xmlns:tools作用以及用法https://blog.csdn.net/qq_24531461/article/details/52804473

本来打算做个demo演示一下的，考虑到控件还没写，放到后面的部分吧....

## 参考资料

1. Google 安卓开发者指南 布局部分

   https://developer.android.google.cn/guide/topics/ui/layout

2. Android开发的常用布局

   <https://zhuanlan.zhihu.com/p/87343947 >

3. Android Studio中match_parent和wrap_content的区别https://blog.csdn.net/qq_38900441/article/details/83420480

4. Android线性布局和相对布局的详解和区别https://blog.csdn.net/weixin_40783315/article/details/83589369

5. RelativeLayout(相对布局)|菜鸟教程

   https://www.runoob.com/w3cnote/android-tutorial-relativelayout.html

6. <?xml version="1.0" encoding="utf-8"?>的作用

   https://www.cnblogs.com/neil-/p/5416337.html

7. Why this line xmlns:android="http://schemas.android.com/apk/res/android" must be the first in the layout xml file?

   http://www.it1352.com/79986.html

8. Android布局文件中的xmlns:tools作用以及用法https://blog.csdn.net/qq_24531461/article/details/52804473

9. Android布局理解之xmlns

   https://www.jianshu.com/p/accb46a12a53

