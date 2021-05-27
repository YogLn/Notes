# Android Studio 学习笔记(五)：WebView 简单说明

Android中一个用于网页显示的控件，实际上，也可以看做一个功能最小化的浏览器，看起来类似于在微信中打开网页链接的页面。WebView主要用于在app应用中方便地访问远程网页或本地html资源。同时，WebView也在Android中充当Java代码和JS代码之间交互的桥梁。

## WebView基本用法

设置WebView至少有两种方法

1. 布局文件中添加WebView控件 & Activity中设置加载页面
2. 调用setContentView()方法，直接通过代码创建

一般采用第一种方法，在 Activity中设置加载页面步骤如下

### 设置加载网址并使用loadUrl()方法加载

对变量`private WebView wView;`

1. 电脑本地文件：放在app/src/main/assets下的html

   `wView.loadUrl("file:///android_asset/test.html");`

   手机本地文件

   `wView.loadUrl("content://com.android.htmlfileprovider/sdcard/test.html");`

2. 远程资源:

   需先在清单文件 AndroidManifest文件中为app申请网络使用权限：

   `<uses-permission android:name="android.permission.INTERNET"/>`

   - 填入网页地址进行加载

     `wView.loadUrl("https://www.cnblogs.com/ifever/")`

   - 加载 HTML 页面的一小段内容
     `WView.loadData(String data, String mimeType, String encoding)`

- 默认情况下，WebView会调用系统默认浏览器加载传入的网址或者资源。如果需要在当前app页面内加载，则需要设置WebViewClient 中的shouldOverrideUrlLoading()方法

### 在当前app页面内加载

```
wView.setWebViewClient(new WebViewClient() {   

@Override    
public boolean shouldOverrideUrlLoading(WebView view, String url) {    
return false;    
}    

@Override    
public boolean shouldOverrideUrlLoading(WebView view, WebResourceRequest request) {        
return false;   
}
```

以上两个方法都会在WebView加载新的url时触发。

Android 5.0以下系统会回调第一个方法，反之回调第二个方法。因此，为了兼容不同的系统版本，可以同时重写这两个方法。

可以看到，这两个方法都有一个boolean返回值。假如修改为true，会带来重定向问题。

#### 返回键始终无法退出应用：重定向问题

设置true的代码如下

```
public boolean shouldOverrideUrlLoading(WebView view, String url) {
  view.loadUrl(url);//手动加载
  return true;
}
public boolean shouldOverrideUrlLoading(WebView view, WebResourceRequest request) {
  view.loadUrl(request.getUrl().toString());//手动加载
  return true;
}
```

虽然不会影响加载，不过会带来用户点击回退键始终无法退出应用的问题，下面简单介绍下原理：

假设访问网页过程为A - > B - > C共三个站点，在C时点回退，C - > B没问题，接着再点 B -> A，这个时候问题出现了。尽管B来到了A，但是因为重定向又跳转到了B，如此循环往复...

返回值设为false的情况（且删去view.loadUrl()方法）就可解决这种问题。其实无论返回true还是false，只要为WebView设置了WebViewClient，都不影响正常的加载功能，只是系统就不会再将url交给第三方的浏览器去处理了。

这两种返回值的真正区别是这样的：

shouldOverrideUrlLoading返回false，代表将url交给当前WebView加载，也就是正常的加载状态；shouldOverrideUrlLoading返回true，代表开发者已经对url进行了处理，WebView就不会再对这个url进行加载了。可以用于屏蔽某些网址，借此实现黑名单机制。

解决了用户点击回退键始终无法退出应用，但还存在一个显著问题：用户希望回退到上一级页面，却直接退出了app

## 回退问题解决方法

goBack() //回退到上一级页面

canGoBack()//能否回退到上一级页面

```
@Override
        public boolean onKeyDown(int keyCode, KeyEvent event) {
            if (keyCode == KeyEvent.KEYCODE_BACK && wView.canGoBack()) {
            //如果按下返回键，同时又可以回退到上一级页面，就返回上一级
                wView.goBack();
                return true;
            }
            return super.onKeyDown(keyCode,event);
        }
```

默认情况下，返回键会执行finish()方法，结束当前Activity。

有些app为了防止用户误触，在按下返回键且无法回退上一级页面时，会提示需要再次按下返回键，才结束当前Activity。这意味着需要在一定时间内连续按动两次返回键才可退出，设计更加人性化。将上述代码改为

```
private long exitTime = 0;
@Override
    public void onBackPressed() {
        if (wView.canGoBack()) {
            wView.goBack();
        } else {
            if ((System.currentTimeMillis() - exitTime) > 2000) {
                Toast.makeText(getApplicationContext(), "再按一次退出程序",
                        Toast.LENGTH_SHORT).show();
                exitTime = System.currentTimeMillis();
            } else {
                finish();
            }

        }
    }
```

此外修改布局文件，通过添加按钮并编写，也可以添加其他新功能，常用的有

finish(); //关闭当前Activity，一键退出

wView.reload(); //刷新当前页面

wView.setScrollY(0); //滚动到顶部

- 添加进度条

public void onProgressChanged(WebView view, int newProgress){
super.onProgressChanged(view,newProgress);
}

newProgress*100，即可获得当前加载的百分比，具体方法网上可查，这里暂不深入

- 标题改为当前网页标题

public void onReceivedTitle(WebView view, String title) {
super.onReceivedTitle(view,title);
setTitle(title);
}

在开通Java权限后，可以设置网页弹窗，至少有两种方式，alert、confirm、prompt三种类型的弹窗

- wView.loadUrl("javascript:alert('hello')");
- wView.evaluateJavascript("javascript:alert('hello')",null);

放置位置如下：

```
wView.setWebViewClient(new WebViewClient() {   

// @Override    
// public boolean shouldOverrideUrlLoading(WebView view, String url) {    
//     return false;    
// }    

@Override    
public boolean shouldOverrideUrlLoading(WebView view, WebResourceRequest request) {        return false;    
}    

@Override        
public void onPageFinished(WebView view, String url) {           
//网页加载完成时
super.onPageFinished(view, url);            
//wView.loadUrl("javascript:alert('hello')");          
// wView.evaluateJavascript("javascript:alert('hello')",null);        
}});
```

开通Java权限及其他设置见下文

## WebSettings：WebView状态管理

WebSettings 用来管理WebView 状态

为了方便，这里写为`WebSettings settings = wView.getSettings();`

默认情况下，WebView是不支持Javascript的，需要调用 setJavaScriptEnabled(true)，即

```
settings.setJavaScriptEnabled(true);
```

比如打开百度，添加该行代码前，网页少了很多广告，进入搜索结果会提示需要切换为完整版（因为无法不支持JS所以无法），但不涉及复杂功能的情况下，不影响使用

添加后，反而容易报错，也存在安全隐患，在本人的oppo r11上调试就会出现net_ERROR错误，暂时找不到原因

## 在网页自定义缩放

即双指缩放页面的手势控制缩放，由以下代码实现

```
settings.setUseWideViewPort(true);//设定支持viewport属性,有些页面是很早期的只适配了pc的浏览器的页面，页面很大，需要宽大的视野来展示，这时候设置为true就可以在足够的空间拖动，并且页面可以以任意比例缩放。
settings.setLoadWithOverviewMode(true);   //自适应屏幕,用尽可能大的视野展示页面，一般和上一属性搭配使用。当然大视野显示的字都很小。
settings.setBuiltInZoomControls(true); // 使用缩放控件
settings.setDisplayZoomControls(false); // 隐藏缩放控件 
settings.setSupportZoom(true);//设定支持缩放,只设置此属性并不能实现缩放，在某些版本的手机上也只是能实现点击缩放。
```

在很多网页依旧不能缩放，经查阅，应该是页面已经被设置好手机样式了，但对于传统的PC版网页，缩放功能仍有效。

WebView在实际使用中需要用到的两个类，这里记录一下二者的区别

## WebViewClient与WebChromeClient的区别

WebViewClient主要帮助WebView处理各种通知、请求事件的
WebChromeClient主要帮助WebView处理Javascript的对话框、网站图标、网站title、加载进度等

实际使用的话，如果你的WebView只是用来处理一些html的页面内容，只用WebViewClient就行了，如果需要更丰富的处理效果，比如JS、进度条等，就要用到WebChromeClient，根据实际使用的方法进行回调

## 参考资料

1. 这是一份全面 & 详细的Webview使用攻略
   https://www.jianshu.com/p/3c94ae673e2a/
2. Android WebView 的使用(超详细用法)
   https://blog.csdn.net/weixin_40438421/article/details/85700109
3. Android WebView全面讲解
   https://www.2cto.com/kf/201712/706339.html
4. 安卓使用WebView加载百度首页
   https://www.jianshu.com/p/b073102fb504
5. Android webView 支持缩放及自适应屏幕
   https://blog.csdn.net/qq_40885379/article/details/83058732
6. android WebView（二）缩放
   https://blog.csdn.net/w2865673691/article/details/44941495?utm_source=distribute.pc_relevant.none-task
7. 【天哥】Android开发视频教程最新版 Android Studio开发
   https://www.bilibili.com/video/av38409964
8. WebViewClient与WebChromeClient
   https://www.cnblogs.com/yaowen/p/5562971.html
9. WebView(网页视图)基本用法|菜鸟教程
   https://www.runoob.com/w3cnote/android-tutorial-webview.html