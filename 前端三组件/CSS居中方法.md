# 一、行内元素的居中

## 1.水平居中

### 1.text-align

```css
    .parent {
      background-color: red;
      text-align: center;
    }
```

```html
  <div class="parent">
    <span class="child">content</span>
  </div>
```

![image-20201119192130498](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201119192130498.png)

### 2.fit-content

```css
    .parent {
      background-color: red;
      width: fit-content;
      margin: auto;
    }
```

```html
  <div class="parent">
    <span class="child">content</span>
  </div>
```

![image-20201119192235935](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201119192235935.png)

## 2.垂直居中

### 1.line-height(单行文本)

```css
    .parent {
      height: 200px;
      line-height: 200px;
      background-color: red;
    }
```

```html
  <div class="parent">
    <span class="child">content</span>
  </div>
```



![image-20201119192448103](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201119192448103.png)

# 二、块级元素的居中

## 水平居中:margin:0 auto;

```css
    .parent {
      background-color: red;
    }
    .child {
      margin: 0 auto;
      width: 100px;
      background-color: royalblue;
    }
```

```html
  <div class="parent">
    <div class="child">content</div>
  </div>
```

![image-20201119192932193](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201119192932193.png)

# 三、水平垂直居中

#### **1.提前知道子元素的宽高**

```css
    .parent {
      position: relative;
      height: 200px;
      background-color: red;
    }
    .child {
      height: 100px;
      width: 100px;
      position: absolute;
      background-color: royalblue;
      left: 50%;
      top: 50%;
      margin-top: -50px;
      margin-left: -50px;
    }
```

```html
  <div class="parent">
    <div class="child">content</div>
  </div>
```

**也可以使用css中的`calc`函数**

```css
    .parent {
      position: relative;
      height: 200px;
      background-color: red;
    }
    .child {
      height: 100px;
      width: 100px;
      position: absolute;
      background-color: royalblue;
      left: calc(50% - 50px);
      top: calc(50% - 50px);
    }
```

![image-20201119193658494](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201119193658494.png)

#### 2.不知道子元素的宽高

```css
    .parent {
      position: relative;
      height: 200px;
      background-color: red;
    }
    .child {
      position: absolute;
      background-color: royalblue;
      left: 50%;
      top: 50%;
      transform: translate(-50%,-50%);
    }
```

#### 3.定位+margin

```css
    .parent {
      position: relative;
      height: 200px;
      background-color: red;
    }
    .child {
      width: 100px;
      height: 100px;
      background-color: royalblue;
      position: absolute;
      left: 0;
      top: 0;
      right: 0;
      bottom: 0;
      margin: auto;
    }
```

![image-20201119194519044](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201119194519044.png)

#### 4.padding

```css
    .parent {
      padding: 20px;
      background-color: red;
    }
    .child {
      height: 200px;
      background-color: royalblue;
    }
```

![image-20201119194720141](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201119194720141.png)

#### 5.通过flex实现

```css
    .parent {
      height: 200px;
      display: flex;
      align-items: center; /*控制垂直居中*/
      justify-content: center; /*控制水平居中*/
      background-color: red;
    }
    .child {
      width: 100px;
      height: 100px;
      background-color: royalblue;
    }
```

![image-20201119195034301](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201119195034301.png)

#### 6.伪元素

```css
 .parent {
      height: 200px;
      text-align: center;
      vertical-align: middle;
      background-color: red;
    }
    .child {
      width: 100px;
      height: 100px;
      display: inline-block;
      vertical-align: middle;
      background-color: royalblue;
    }
    .parent::before {   /*默认行内元素，添加display: inline-block;成为行内块元素*/
      content: "";
      width: 20px;
      height: 200px;
      display: inline-block;
      vertical-align: middle;
    }
```

![image-20201119195347410](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201119195347410.png)

#### 7.calc函数

```css
    .parent {
      height: 600px;
      width: 600px;
      background-color: red;
    }
    .child {
      width: 100px;
      height: 100px;
      padding: calc((100% - 100px) / 2);/*使子元素的margin都是250px*/
      background-clip: content-box;/*只对子元素生效，防止f覆盖父元素*/
      background-color: royalblue;
    }
```

![image-20201119200030099](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20201119200030099.png)