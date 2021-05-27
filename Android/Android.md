# 1.结构目录

res:资源文件

drawable:放图片和一些自定义的xml文件

layout:布局文件

mipmap:启动图片，logo

values:颜色、文字

androidmanifest.xml:应用里所有用到的某一个activity都需要在此声明

# 2.UI组件

## 2.1布局管理器

- 线性布局（LinearLayout)
- 相对布局（RelativeLayout)

### 2.1.1线性布局

##### 常用属性

```xml
<LinearLayout
    android:id="@+id/ll_1"
    android:layout_width="200dp"
    android:layout_height="200dp"
    android:background="#000000"
    android:orientation="vertical"
    android:padding="20dp">

    <View
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#ff0033" />
</LinearLayout>
```

##### 平分宽度

```xml
<View
    android:layout_width="0dp"
    android:layout_height="match_parent"
    android:background="#000000"
    android:layout_weight="1"/>
<View
    android:layout_width="0dp"
    android:layout_height="match_parent"
    android:background="#ff0033"
    android:layout_weight="1"/>
```

> **android:layout_width="0dp"为初始拥有的宽度，再按照android:layout_weight="1"权重分剩下的评分**

### 2.1.2相对布局

#### 常用属性

```xml
android:layout_below="@+id/view_1" //在view_1的下面
android:layout_toLeftOf=""
android:layout_toRightOf=""
android:layout_alignBottom=""
android:layout_alignParentBottom=""
```

## 2.2TextView

### 2.2.1页面跳转

`MainActivity`

```java
public class MainActivity extends ActionBarActivity {
    private Button mBtnTextView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mBtnTextView = (Button) findViewById(R.id.btn_textview);//强制转换为Button类型
        mBtnTextView.setOnClickListener(new View.OnClickListener(){
            @Override
            public void onClick(View v){
                //跳转到textview演示界面
                Intent intent = new Intent(MainActivity.this,TextViewActivity.class);
                //从MainActivity点击按钮跳转到TextViewActivity
                startActivity(intent);
            }
        });
    }
```



- 文字大小、颜色

  `activity_text_view.xml`

  ```xml
  <TextView
      android:id="@+id/tv_1"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="@string/tv_1"
      android:textColor="#000000"
      android:textSize="24sp" />
  ```

  `values/string.xml`

  ```xml
  <resources>
      <string name="tv_1">我要考研</string>
  </resources>
  ```

- 显示不下使用

  ```xml
  <TextView
      android:id="@+id/tv_2"
      android:layout_width="100dp"
      android:layout_height="wrap_content"
      android:layout_marginTop="20dp"
      android:ellipsize="end"		
      android:maxLines="1"		//强制一行显示，显示不完的会显示...
      android:text="西安电子科技大学"
      android:textColor="#000000"
      android:textSize="24sp" />
  ```

- 文字+icon

  ```xml
  <TextView
          android:id="@+id/tv_3"
          android:layout_width="100dp"
          android:layout_height="wrap_content"
          android:layout_marginTop="20dp"
          android:ellipsize="end"
          android:drawableRight="@drawable/arrow_sort_down"
          android:maxLines="1"
          android:text="筛选"
          android:textColor="#000000"
          android:textSize="24sp" />
  ```

- 中划线、下划线

  ```xml
  <TextView
          android:id="@+id/tv_4"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:text="这是中划线"
          android:textColor="#000000"
          android:textSize="24sp"
          android:layout_marginTop="10dp"/>
  ```

  ```xml
      <TextView
          android:id="@+id/tv_5"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:text="这是下划线"
          android:textColor="#000000"
          android:textSize="24sp"
          android:layout_marginTop="10dp"/>
  ```

  `TextViewActivity.java`

  ```java
  private TextView mTv4;
  private TextView mTv5;
  
  mTv4 = (TextView) findViewById(R.id.tv_4);
  mTv4.getPaint().setFlags(Paint.STRIKE_THRU_TEXT_FLAG);//中划线
  mTv4.getPaint().setAntiAlias(true);//去除锯齿
  mTv5 = (TextView) findViewById(R.id.tv_5);
  mTv5.getPaint().setFlags(Paint.UNDERLINE_TEXT_FLAG);
  ```

  用代码插入下划线

  `TextViewActivity.java`

  ```java
  private TextView mtv6;
  
  mTv6 = (TextView) findViewById(R.id.tv_6);
  mTv6.setText(Html.fromHtml("<ul>西华大学</u>"));
  ```

- 跑马灯

  ```xml
  <TextView
          android:id="@+id/tv_7"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:text="Talk is cheap,show me the code ------linus"
          android:textColor="#000000"
          android:textSize="24sp"
          android:layout_marginTop="10dp"
          android:singleLine="true"
          android:ellipsize="marquee"
          android:marqueeRepeatLimit="marquee_forever"
          android:focusable="true"
          android:focusableInTouchMode="true"
          />
  ```

## 2.3Button

- 文字大小、颜色

  ```xml
  android:text="按钮1"
  android:textSize="20sp"
  android:textColor="#FFFFFF"
  android:background="#ff0033"
  ```

- 自定义颜色背景

  ```xml
  android:background="@drawable/bg_btn2"
  ```

  `bg_btn2.xml`

  ```xml
  <shape xmlns:android="http://schemas.android.com/apk/res/android"
      android:shape="rectangle">
      <solid  实体，中空为stroke
          android:color="#ff9900"/>
      <corners
          android:radius="10dp"/>
  </shape>
  ```

- 自定义按压效果

  ```xml
  <item android:state_pressed="true">		--设置按压时的状态
      <shape>
          <solid android:color="#CC7A00"/>
          <corners android:radius="10dp"/>
      </shape>
  </item>
  <item android:state_pressed="false">	--设置没有按压时的状态
      <shape>
          <solid android:color="#FF9900"/>
          <corners android:radius="10dp"/>
      </shape>
  </item>
  ```

- 点击事件 

  方案一：

  ```
  <Button
          android:id="@+id/btn_4"
          android:layout_width="match_parent"
          android:layout_height="40dp"
          android:text="按钮4"
          android:onClick="showToast"/>  --定义onClick事件
  ```

  `ButtonActivity.java`

  ```java
  public void showToast(View view) {
      Toast.makeText(this,"你点击了按钮4",Toast.LENGTH_LONG).show();
  }
  ```

  方案二：

  ```xml
  <Button
      android:id="@+id/btn_3"
      android:layout_width="match_parent"
      android:layout_height="40dp"
      android:text="按钮3"
      android:textAllCaps="false" --关闭默认显示大写/>
  ```

  `ButtonActivity.java`

  ```java
  private Button mBtn3;       
  mBtn3 = findViewById(R.id.btn_3);
      mBtn3.setOnClickListener(new View.OnClickListener() {
          @Override
          public void onClick(View v) {
              Toast.makeText(ButtonActivity.this,"你点击了按钮3",Toast.LENGTH_LONG).show();
      }
  });
  ```

## 2.4EditText

```
android:hint="用户名"		--注释信息，相当于html中的palceholder
android:maxLines="1"	  
```

## 2.5RadioButton

### 2.5.1单选框

```xml
<RadioGroup
        android:id="@+id/rg_1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical">
        <RadioButton
            android:id="@+id/rd_1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="男"
            android:checked="true" --默认选中 />
        <RadioButton
            android:id="@+id/rd_2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="女"/>
    </RadioGroup>
```

### 2.5.2自定义单选框的样式

```xml
<RadioGroup
        android:id="@+id/rg_2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:layout_marginTop="100dp">
        <RadioButton
            android:id="@+id/rd_3"
            android:layout_width="50dp"
            android:layout_height="30dp"
            android:gravity="center"
            android:text="男"
            android:checked="true"
            android:button="@null"
            android:textColor="#000000"
            android:background="@drawable/selector_orange_radiobutton"/>
        <RadioButton
            android:id="@+id/rd_4"
            android:layout_width="50dp"
            android:layout_height="30dp"
            android:text="女"
            android:gravity="center"
            android:button="@null"  --取消系统自带的样式
            android:textColor="#000000"
            android:background="@drawable/selector_orange_radiobutton"  --引用外部样式
            android:layout_marginLeft="10dp"/>
</RadioGroup>
```

`selector_orange_radiobutton.xml`

```xml
<item android:state_checked="true"> 	--设置选中时的样式
    <shape>
        <solid android:color="#CC7A00"/>
        <corners android:radius="10dp"/>
    </shape>
</item>
<item android:state_checked="false">	  --设置未选中时的样式
    <shape>
        <stroke android:width="1dp"
            android:color="#AA6600"/>
        <corners android:radius="10dp"/>
    </shape>
</item>
```

### 2.5.3为单选框设置监听事件

`RadioButtonActivity.java`

```java
 private RadioGroup mRg1;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_radio_button2);

        mRg1 = (RadioGroup) findViewById(R.id.rg_1);
        mRg1.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup group, int checkedId) {//当前的组，当前的组选中的id
                RadioButton radioButton = (RadioButton) group.findViewById(checkedId);
                Toast.makeText(RadioButtonActivity.this,radioButton.getText(),Toast.LENGTH_SHORT).show();

            }
        });
    }
```

## 2.6复选框CheckBox

#### 自定义复选框

```xml
<LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/cd_4"
        android:orientation="vertical">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="你的兴趣："
            android:textColor="#000"
            android:textSize="20sp" />

        <CheckBox
            android:id="@+id/cd_5"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="编程"
            android:textSize="20sp"
            android:button="@drawable/bg_checkbox"		--引入/>
        <CheckBox
            android:id="@+id/cd_6"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="肥宅"
            android:textSize="20sp"
            android:button="@drawable/bg_checkbox"/>
```

`bg_checkbox.xml`

```xml
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_checked="true" android:drawable="@drawable/checkbox_true"/>
    <item android:state_checked="false" android:drawable="@drawable/checkbox_false"/>
</selector>
```

## 2.7ImageView

### 2.7.1加载本地图片

```xml
android:src="@drawable/本地图片地址" 
android:scaleType=""

		--fitXY:撑满组件，宽高比可能发生改变
		--fitCenter:保持宽高比缩放，知道能够完全显示
		--centerCrop:保持宽高比缩放，直到完全覆盖控件，裁剪显示
```

### 2.7.2加载网络图片

1. 引入第三方库

   [https://github.com/bumptech/glide](https://github.com/bumptech/glide)

2. 在`ImageViewActivity.java`中

   ```java
   Glide.with(this).load("网络图片地址").into(mIv4);
   ```

3. 在`AndroidManifest.xml`中授权

   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   ```

## 2.8列表视图ListView(被代替)

## 2.9网格视图GridView

## 2.10滚动视图ScrollView

- 垂直滚动：ScrollView

  **只能有一个子布局**

- 水平滚动：HorizontalScrollView

  **只能有一个子布局**

## 2.11 RecyclerView(极其重要)

> **能够灵活实现大数据集的展示·，视图的服用管理比LIstView好，能够显示列表、网格、瀑布流等形式，并且不同的ViewHolder能够实现item多元化的功能。**

## 2.12webview

### 加载网络URL:

```java
webview.loadUrl("网址")
```

### 加载assets下的html文件

```java
webview.loadUrl("file:///android_asset/test.html;")
```

### 加载html代码

```java
webview.loadData();
webview.loadDataWithBaseURL();//乱码的可能性校
```

### 网页的前进后退

```java
webview.canGoBack();//判断能不能返回
webview.goBack();//返回
webview.canGoforward();
webview.goforward();
webview.canGoBackOrForward(int steps);//是否可以前进或者后退steps
webview.goBackOrForward(int steps);
```

### 点击返回键，默认是退出当前Activity,如果希望是webview页面内的后退

![image-20201018164203611](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201018164203611.png)



## 3.UI组件之弹出组键

### 3.1Toast

> Toast是一个消息提示组件，设置显示的位置，自定义显示内容，简单封装

#### Toast的三种实现方式

- 默认显示

  ```java
  Toast.makeText(getApplicationContext(),"Toast",Toast.LENGTH_SHORT).show();
  ```

- 剧中显示

  ```java
  Toast toastCenter = Toast.makeText(getApplicationContext(),"居中Toast",Toast.LENGTH_SHORT);
  toastCenter.setGravity(Gravity.CENTER,0,0);
  toastCenter.show();
  ```

- 自定义显示（图片+文字)

  ```java
  Toast toastCustom = new Toast(getApplicationContext());
  LayoutInflater layoutInflater = LayoutInflater.from(ToastActivity.this);
  View view = layoutInflater.inflate(R.layout.layout_toast,null);
  ImageView imageView = view.findViewById(R.id.iv_toast);
  TextView textView = view.findViewById(R.id.tv_toast);
  imageView.setImageResource(R.drawable.smile);
  textView.setText("你好！");
  toastCustom.setView(view);
  toastCustom.setDuration(Toast.LENGTH_LONG);
  toastCustom.show();
  ```


### 3.2AlertDiaLog

### 3.3ProgressBar&ProgressDialog

# 4.Activity

## 4.1.1Activity的显示调用的四种方法

![image-20201027221342726](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201027221342726.png)

## 4.1.2隐示跳转

![image-20201027222026327](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201027222026327.png)

![image-20201027222053582](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201027222053582.png)

## 4.1.3Activity和四种启动模式

**Activity的android：launchMode属性**

- **standard：标准模式，每次都新建一个实例对象**
- **singleTop：Task栈顶复用模式，如果在任务栈顶发现了相同的实例则重用，否则新建并压入栈顶**
- **singleTask：Task栈内复用模式，如果在任务栈中发现了相同的实例，将其上面的任务终止并移除，重用该实例。否则新建实例并入栈**
- **singleInstance：全局单例模式，允许不同应用，进程线程等共用一个实例，无论从何应用调用该实例都重用**

> Activity是由任务管理栈的，每启动一个一个Activity就会入栈，按返回键就会从栈顶出栈

> standrd是默认的启动模式，即标准模式，每启动一个Activity就会创建一个新的实例

## 4.2Fragment

- **Fragment有自己的生命周期**
- **Fragment依赖于Activity**
- **Fragment通过getActivity(可以获取所在的Activity ; Activity通过FragmentManager的findFragmentById()或findFragmentByTag()获取Fragment**
- **Fragment和Activity是多对多的关系**

### Fragment中getActivity()为null的问题

 

### 向Frafment传递参数

