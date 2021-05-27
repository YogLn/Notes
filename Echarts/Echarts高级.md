## 1.主题

### 内置主题

Echarts默认内置了两套主题： light dark

在初始化方法中init中可以指明

```js
var chart = echart.init(dom, 'dark')
var chart = echart.init(dom, 'light')
```

### 自定义主题

1. 在主题编辑器中编辑主题
2. 下载主题， 是一个js文件
3. 引入js文件
4. 在init方法中使用

```js
var mcharts = echarts.init(document.querySelector('div'), 'itcast')
```

## 2.调色盘(遵循就近原则)

### 主题的调色盘

在下载的主题`itcast.js`文件中

```js
echarts.registerTheme('itcast', {
        "color": [
            "#c12e34",
            "#e6b600",
            "#0098d9",
            "#2b821d",
            "#005eaa",
            "#339ca8",
            "#cda819",
            "#32a487"
        ],
    // 其中的color就是调色盘
```

### 全局的调色盘

使用全局调色盘将覆盖猪主题的调色盘

```js
option: {
 	color: [
        'red',
        'green',
        'blue'
    ]   
}
```

### 局部调色盘

```js
series: [
    {
        color: [ 
        'red',
        'green',
        'blue'
        ]
    }
]
```

### 颜色渐变 

#### 线性渐变：

```js
itemStyle: {
  color: {
    type: 'linear', //线性渐变
    x: 0,
    y: 0,
    x2: 0,
    y2: 1,
    colorStops: [
      {
        offset: 0,
        color: 'red'
      },
      {
        offset: 1,
        color: 'blue'
      }
    ]
  }
}
```

![image-20210209113917327](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210209113917327.png)

#### 径向渐变：

```js
itemStyle: {
  color: {
    type: 'radial', // 径向渐变
    x: 0.5, //圆心点和半径，0.5代表在最中央
    y: 0.5,
    r: 0.5,
    colorStops: [
      {
        offset: 0,
        color: 'red'
      },
      {
        offset: 1,
        color: 'blue'
      },
    ]
  }
},
```

![image-20210209114254246](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210209114254246.png)

## 3.样式

### 直接样式

ItemStyle、textStyle、lineStyle、areaStyle、label

### 高亮样式

在emphasis中包裹ItemStyle、textStyle、lineStyle、areaStyle、label

## 4.自适应

1. 监听窗口的大小发生变化
2. 调用echarts实例对象的resize方法

```js
mcharts.setOption(options)
// 1. 监听窗口大小变化的事件
// window.onresize = function() {
//   // 2. 调用echarts实例对象的resize方法
//   mcharts.resize()
// }
window.onresize = mcharts.resize
```

## 5.加载动画

### 显示加载动画

```js
mcharts.showLoading()
```

### 隐藏加载动画

```js
mcharts.hideLoading()
```

### 增量动画

```js
// echarts增量动画都是实现好了的
mchart.setOption(option)
```

### 控制动画的开启与否

```js
animation: true //默认事true,开启动画
```

### 动画时长

```js
animationDuration: 7000, //单位毫秒
// animationDuration还可以接收回调函数
// 参数args的数值0代表没有动画效果
```

## 6.交互API

### 全局echarts对象

```js
init 
registerTheme
registerMap
connect // 实现地图关联 echarts.connect(mchart1, mchart2)
```

### 内置echarts对象

```js
on/off
dispatchAction() // 触发某些行为模拟用户的操作
clear() //销毁实例，可以再次setoption将突变再次展示出来
dispose() //销毁实例
```



