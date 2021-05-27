# Android Studio 学习笔记(四)：Adapter和RecyclerView说明

在现版本中，滚动控件有多种，而相比于ListView，GridView，RecyclerView的用途更广，因此将前两者作为Adapter适配器的引入，再对RecyclerView进行简单讲解。

## MVC & Adapter

为了方便理解，这里介绍一下Android应用设计的基础，也就是MVC架构，如图。

![mvc](https://img2018.cnblogs.com/blog/1938293/202002/1938293-20200218211348271-579284952.png)

- 控制器（Controller）- 可看作一个中间桥梁，响应来自View的用户交互，通过对View设定的事件逻辑修改Model，再回传实现View的数据刷新。
- 视图（View） - 用户看到的图形界面，由界面设计人员负责。
- 模型（Model） - 保存数据状态，其中由程序员编写程序应有的功能（实现算法等等）、数据库专家进行数据管理和数据库设计(可以实现具体的功能)。

MVC架构:Model(数据)以Controller(控制器)设定的方式呈现在View(用户界面)中。

简而言之：Adapter在其中充当Controller(控制器)的角色，在其中设定每一个元素长什么样子，怎么排列各个元素的逻辑，再把包含代码逻辑的复杂数据按设定好的样式给View。其中自带的BaseAdapter用得最多。

**常见用法是新建一个类继承自BaseAdapter，重写其中的方法并构造新的方法，结合ListView、GridView控件使用。**ListView和GridView的用法相似，只是功能不同，下面以ListView为例共同进行讲解

## ListView & GridView

ListView是一种用于垂直显示的列表控件，如果显示内容过多，则会出现垂直滚动条。样式可参考某宝上购物列表的样子。GridView是以网格的形式排列显示的控件，样式可以类比手机桌面图标的排列。

下面以ListView的Adapter适配器为例(新建类MyListAdapter.java文件源码部分)，就以下几个方面进行代码说明：

- 重写的两个方法 getCount(),getView() getItemId(),getItem()保持默认
- getView()参数说明及原理
- setTag()和getTag()原理简述

```
public class MyListAdapter extends BaseAdapter {
private Context mContext;    
private LayoutInflater mLayoutInflater;  

MyListAdapter(Context context) {       
this.mContext = context;        
mLayoutInflater = LayoutInflater.from(context);    
//加载布局管理器
}    

@Override    
public int getCount() {        
//返回值 定义列表Item个数，即列表长度
return 10;    
}    

@Override    
public Object getItem(int position) {
//根据position返回对应Item
return null;    }  

@Override    
public long getItemId(int position) {   
//根据position返回对应Item的Id
return 0;    }    

static class ViewHolder {        
//为了方便复用，Item中元素抽象出来，新建了一个静态类ViewHolder，
public ImageView imageView;        
public TextView tvTitle, tvTime, tvContent;    }    

@Override    
public View getView(int position, View convertView, ViewGroup parent) {     
//position 当前Item是界面中的第几个(从0计数)
//convertView 展示在界面上的Item
//parent convertView的父控件，这里指ListView
// 该方法将xml布局转换为view对象 
// 通过控件重用减少内存占用，比如一共new convertView x 1000 ，一次性全加载完肯定不够用
//这段代码让滑出屏幕的convertView在新滑进来的Item中重新使用，只需修改各控件的值
//未显示的Item保存在构件Recycler里

ViewHolder holder = null;        
if (convertView == null) { 
//没有供复用的convertView，新建一个
convertView = mLayoutInflater.inflate(R.layout.layout_list_item, null);            
holder=new ViewHolder();            
holder.imageView = convertView.findViewById(R.id.iv);            
holder.tvTitle=convertView.findViewById(R.id.tv_title);            
holder.tvTime=convertView.findViewById(R.id.tv_time);            
holder.tvContent=convertView.findViewById(R.id.tv_content);            
convertView.setTag(holder);      

//Tag不像ID是用标示view的。Tag从本质上来讲是就是相关联的view的额外的信息。
//贴上一个标签，这个标签就是ViewHolder实例化后对象的一个属性，标示此时带有holder的convertView
//之后对于ViewHolder实例化的对象holder的操作，
//都会因为java的引用机制而一直存活并改变convertView的内容
}        
else{ 
//有供复用的convertView，给控件重赋值，setText()自定义文本内容
holder= (ViewHolder) convertView.getTag();   
取到此时带有holder的convertView
}                
holder.tvTitle.setText("这是标题");        
holder.tvTime.setText("2020-02-18");        
holder.tvContent.setText("这是内容");        
return convertView;    }}
```

## RecyclerView

官方定义为: A flexible view for providing a limited window into a large data set.

RecyclerView能够灵活展示大数据集，视图的复用管理比前两者(ListView、GridView)更好，通过加载不同的布局管理器(LayoutManager)可显示列表(LinerLayoutManager)、网格(GridLayoutManager)、瀑布流(StaggeredGridLayoutManager)等形式，且不同ViewHolder可实现item多元化的功能。标准化了ViewHolder，编写Adapter面向的是ViewHolder而不再是View，使得复用的逻辑被封装，写起来更加简单，而且直接省去了listview中convertView.setTag(holder)和convertView.getTag()这些繁琐的步骤。

RecyclerView的**四大组成**是：

- Layout Manager：Item的布局。
- Adapter：为Item提供数据。
- Item Decoration：Item之间的划分样式Divider。
- Item Animator：添加、删除Item动画。

设计demo如下，采用LinearLayoutManager，和ListView效果相同，单数和双数采用不同的holder，实现了点击和长按事件

![img](http://m.qpic.cn/psc?/V13rVnAS4cHwTO/WUyRLVwskOVTItG8F0x76wkSDqfSTvI7*DOcvngIjud39.PssBg2eoxa1VtFnWnX8GTBVEY8ybtDVx*B1zXpzo.Md0H635uRYaTbWq6hD*g!/b&bo=WAI1BQAAAAACZyk!&rf=viewer_4)

### 使用步骤（以列表视图为例）

1. 首先需要在app/build.gradle中

   引入包，版本自定

   (androidx为 `implementation 'androidx.recyclerview:recyclerview:1.0.0'`)

   或者导入design库，版本自定

   (androidx为`implementation 'com.google.android.material:material:1.1.0-alpha09'`)

2. 分别设置Activity(LinearRecyclerViewActivity)和Item(layout_linear_item)的布局文件

3. 指定布局管理器LayoutManager ,用于确定RecyclerView中Item的展示方式以及决定何时复用已经不可见的Item，避免重复创建以及执行高成本的findViewById()方法。

   ```
   mRvMain.setLayoutManager(new LinearLayoutManager(LinearRecyclerViewActivity.this));
   ```

4. 设置装饰样式

   addItemDecoration可以允许应用给Adapter中来的View加特效和布局偏移，这对于在View间加分割线，高亮显示，可视化分组都很有用。
   可以看作是给View加特技，一个个加的过程中如果加相同的特技，就起到分隔的作用；如果这几个加一种特技，另外几个加别的特技，就可以起到分组的作用；如果某几个加特技，就可以起到高亮的作用。

   ```
   mRvMain.addItemDecoration(new MyDecoration());
   class MyDecoration extends RecyclerView.ItemDecoration{
   //可重新定义以下三种方法中的任一种
   public void onDraw(Canvas c, RecyclerView parent, State state)
   //在Canvas上绘制内容，在绘制Item之前调用。
   //（如果没有通过getItemOffsets设置偏移的话，Item的内容会将其覆盖）
   public void onDrawOver(Canvas c, RecyclerView parent, State state)
   //在Canvas上绘制内容,在Item之后调用。(画的内容会覆盖在Item的上层)
   public void getItemOffsets(Rect outRect, View view, RecyclerView parent, State state)
   //通过Rect为每个Item设置偏移，用于绘制Decoration。
   }
   ```

   RecyclerView的背景、onDraw绘制的内容、Item、onDrawOver绘制的内容，各层级关系如下：

   ![onDraw](https://img2018.cnblogs.com/blog/1938293/202002/1938293-20200218211347786-247523248.jpg)

5. 指定适配器Adapter继承自RecyclerView.Adapter类

   ```
   public class LinearAdapter extends RecyclerView.Adapter<RecyclerView.ViewHolder>
   //也可创建静态内部类ViewHolder继承自RecyclerView.ViewHolder
   ```

6. RecyclerView将ListView中getView()的功能拆分成了`onCreateViewHolder()` 和 `onBindViewHolder()`。

7. 实现3个方法

   - onCreateViewHolder(@NonNull ViewGroup parent, int viewType)
     把View直接封装在ViewHolder中，负责每个Item的布局
   - onBindViewHolder(@NonNull RecyclerView.ViewHolder holder, final int position)
     主要用于适配渲染数据到View中，该方法使用ViewHolder而不是原来的convertView。
   - getItemCount()
     类似于BaseAdapter的getCount()，即定义总共有多少个Item。

### 长按事件的代码补充

天哥的视频演示中跳过了长按事件的代码，这里作下补充

LinearAdapter.java中

```
private OnItemLongClickListener mLongListener;
//新建私有变量用于保存监听器及set方法，这里的set方法统一放在下面的Adapter里了

public LinearAdapter(Context context, OnItemClickListener listener, OnItemLongClickListener Longlistener) {  
...
this.mLongListener = Longlistener;
//相当于setOnItemLongClickListener()方法
}

 public void onBindViewHolder(@NonNull RecyclerView.ViewHolder holder, final int position) {
        ...
        holder.itemView.setOnLongClickListener(new View.OnLongClickListener() {
        //实现回调
            @Override
            public boolean onLongClick(View v) {
                mLongListener.onLongClick(position);
                Toast.makeText(mContext,"长按 pos:"+position,Toast.LENGTH_SHORT).show();
             //另一种方法是在相应的Activity中设置Toast
                return true;
             //源码这里已经给出解释，如果返回值设为true，则系统消耗掉长按事件，这样就不会和点击事件冲突
            }
        });

    }
    public interface OnItemLongClickListener {
        void onLongClick(int pos);//新建内部接口
    }
```

另一种方法，上述代码中的Toast语句删去。

还需在LinearRecyclerViewActivity.java添加

```
mRvMain.setAdapter(new LinearAdapter(LinearRecyclerViewActivity.this, new LinearAdapter.OnItemClickListener() {...},
new LinearAdapter.OnItemLongClickListener(){   
@Override    
public void onLongClick(int pos) {        
Toast.makeText(LinearRecyclerViewActivity.this,"longClick"+pos,Toast.LENGTH_SHORT).show();    
}}));
```

## 参考资料

1. Android 常用控件及使用方法

   https://wenku.baidu.com/view/70ca306225c52cc58bd6bec6.html

2. Android Activity，Adapter基础讲解|菜鸟教程

   https://www.runoob.com/android/android-acitivities.html

   https://www.runoob.com/w3cnote/android-tutorial-adapter.html

3. Android与MVC设计模式

   https://www.cnblogs.com/vi3nty/p/7593973.html

4. Android MVC 模式的介绍与实战

   https://blog.csdn.net/qq_27061049/article/details/83061248

5. BaseAdapter中getView()里的3个参数是什么意思https://zhidao.baidu.com/question/1382059394224466060.html?qbl=relate_question_1&word=public View getView(int position%2C View convertView%2C ViewGroup parent)

6. 对convertView的理解

   https://www.cnblogs.com/tekkaman/p/6268337.html

7. convertView&setTag方法的一点理解

   https://blog.csdn.net/xiao_ziqiang/article/details/50812471

8. Android Adapter以及getView()方法的理解

   https://www.cnblogs.com/vice/p/9043086.html

9. Android 控件 RecyclerView

   https://www.jianshu.com/p/4f9591291365

10. ItemDecoration学习

    https://www.jianshu.com/p/986605949373

11. RecyclerView：打造悬浮效果

    http://www.sohu.com/a/199914786_465908

12. 探究onCreateViewHolder和onBindViewHolder两者关系和调用次数

    https://blog.csdn.net/csdn_aiyang/article/details/80094302

13. 揭开RecyclerView的神秘面纱(二)：处理RecyclerView的点击事件

    https://www.jianshu.com/p/f2e0463e5aef

14. Android开发视频教程最新版 Android Studio开发

    https://www.bilibili.com/video/av38409964?t=2327&p=14



