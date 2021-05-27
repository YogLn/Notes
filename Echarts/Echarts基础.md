## 1.基本使用

步骤1：引入echarts.js文件

步骤2：准备液体个呈现图标的盒子

步骤3：初始化echarts实例对象

步骤4：准备配置项

步骤5：将配置项设置给echarts实例对象

```html
<!-- 步骤1：引入echarts.js文件 -->
    <script src="./lib/echarts.min.js"></script>

    <!-- 步骤2：准备液体个呈现图标的盒子 -->
    <div style="width: 600px; height: 400px"></div>
    <script>
        // 步骤3：初始化echarts实例对象 
        var mcharts = echarts.init(document.querySelector('div'))

        // 步骤4：准备配置项
  var options = {
    title: {
      text: '语文',
      link: '#',
      textStyle: {
        color: '#333'
      }
    },
    xAxis: {
      type: 'category', // 类目轴
      data: ['小明', '小红', '小王']
    },
    yAxis: {
      type: 'value' // 数值轴
    },
    series: [ // 系列列表
      {
        name: '语文',
        type: 'bar', // bar: 柱状图 line: 折线图 pie: 饼图
        data: [70, 92, 87]
      }
    ]
  }
  // 步骤5：将配置项设置给echarts实例对象
  mcharts.setOption(options)
```

## 2.柱状图 bar

### 标记：最大值， 最小值， 平均值markPoint, markLine

```js
series: [
// 系列列表
   markPoint: {
		data: [{ type: 'max', name: '最大值' }, {type: 'min', name: '最小值'}],
        },
        markLine: {
          data: [{
            type: 'average',
            name: '平均值'
       }]
      },
],
```

### 数值显示label

```js
label: {
  show: true,
  position: 'inside',// 标签的位置
  rotate: 60
}
```

### 柱的宽度barWidth

```js
barWidth: '30%'
```

### 横向的柱状图

```js
xAxis: {
  type: 'value' // 数值轴
},
yAxis: {
  type: 'category', // 类目轴
  data: ['小明', '小红', '小王']
},
```

### 提示tooltip

```js
tooltip: {
  trigger: 'axis',
  // trigger: 'item'
  triggerOn: 'click',  // 点击才会显示
  formatter: '{b}的成绩是{c}' // 提示框的内容
},
```

![image-20210207232732845](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210207232732845.png)

```js
tooltip: {
  trigger: 'axis',
  // trigger: 'item'
  triggerOn: 'click',  // 点击才会显示
  formatter: function(arg) {// 提示框的内容
    return arg[0].name + '的分数是' + arg[0].data
  }
},
```

### 工具栏toolbox

```js
toolbox: {
  feature: {
    saveAsImage: {}, // 导出图片
    dataView: {}, //数据视图,可修改
    restore: {} //重置
    dataZoom: {} //区域缩放
    magicType: { // 图标类型的切换
      type: ['bar', 'line']
    }
  }
},
```

![image-20210207233803465](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210207233803465.png)

### 图例legend:

```js
legend: {
  data: ['语文', '数学']
},
```

和series 配套使用

## 3.折线图 line：用来描述数据随时间的变化趋势

### 标记：最大值， 最小值， 平均值markPoint, markLine和柱状图一样

### markArea: 连续的部分用阴影部分表示

```js
markArea: {
            data: [
                [
                    {
                        xAxis: '一月',
                    },
                    {
                        xAxis: '二月',
                    },
                ],
                [
                    {
                        xAxis: '七月',
                    },
                    {
                        xAxis: '八月',
                    },
                ],
            ],
    },
```

![](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210208160353858.png)

### smooth: true 设置曲线平滑

### lineStyle: 线条样式

```js
 lineStyle: {
            color: 'green',
            type: 'dashed' // dashed: 虚线 dotted: 点状形成的线 solid: 默认实线
          }
```

### areaStyle: 填充风格

```js
  areaStyle: {
    color: 'pink'
  }
```

![image-20210208160947848](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210208160947848.png)

### boundaryGap: 紧挨边缘

```js
xAxis: {
    type: 'category',
    data: xDataArr,
	boundaryGap: false
},
```

### scale: true缩放、脱离0比例： 防止数值差别过小而显示不明显

```js
yAxis: {
    type: 'value',
	scale: true
},
```

### 堆叠图 stack: 'all'

在series中设置

```js
{
  name: '统一销量',
  data: yDataArr2,
  type: 'line',
  stack: 'all'，
  areaStyle: {}
}
```

配合areaStyle使用

![image-20210208162213230](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210208162213230.png)

## 4.散点图scatter 推断多个变量之间的相关性

### 图标类型

```js
 series下设置type:scatter
 xAxis和yAxis都要设置为value
```

### 调整：将坐标都设置为脱离0比例

```js
xAxis: {
  type: 'value',
  scale: true //脱离0值比例
  },
```

### 代码：

```js
xAxis: {
  type: 'value',
  scale: true //脱离0值比例
  },
  yAxis: {
  type: 'value',
  scale: true
  },
  series: [
      {
       type: 'scatter',
        data: [
         [161.2, 51.6], [167.5, 59.0], [159.5, 49.2], [157.0, 63.0], [155.8, 53.6],
                [170.0, 59.0], [159.1, 47.6], [166.0, 69.8], [176.2, 66.8], [160.2, 75.2],······]
```

**data 为二维数组**

![image-20210208171041834](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210208171041834.png)

### symbolSize： 气泡大小

```js
symbolSize: function (arg) {
    // console.log(arg)
    var height = arg[0] / 100
    var weight = arg[1]
    // BMI = 体重 /（身高*身高） > 28 肥胖
    var bmi = weight / (height * height)
    if (bmi > 28) {
        return 20
    }
    return 5
},
```

![image-20210208172207679](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210208172207679.png)

### itemStyle: 设置气泡的状态

```js
itemStyle: {
  color: 'green'
}


itemStyle: {
    color: function (arg) {
        var height = arg.data[0] / 100
        var weight = arg.data[1]
        // BMI = 体重 /（身高*身高） > 28 肥胖
        var bmi = weight / (height * height)
        if (bmi > 28) {
            return 'red'
        }
        return 'green'
    },
},
```

###  type: 'effectScatter'：涟漪动画

```js
type: 'effectScatter',
rippleEffect: {
  scale: 5
},
showEffectOn: 'emphasis', // render : 自动拥有涟漪效果 emphasis：鼠标滑动才有涟漪效果
```

## 5.直角坐标系的常用配置

### 配置1： 网格grid

option中配置：grid

```js
grid: {
    show: true,
    borderWidth: 10 //设置边框的宽度
	borderColor: 'red',
    width: 300,
    height: 200,
    top: 10,
    left: 10
},
```

###  配置2：坐标轴axis

```js
xAxis: {
    position: 'top'
},
yAxis: {
    position: 'right'
},
```

### 配置3：区域缩放dataZoomd

dataZoom用于与区域缩放是一个数组，可以配置多个区域缩放器

- 类型：type
  - slider: 滑块，下方提供一个可以左右滑动的滑块
  - inside: 内置，可以通过鼠标滚动进行缩放
- 指明产生的作用轴
  - xAxisIndex: 设置缩放控制组件是哪个x轴， 一般写0即可
  - yAxisIndex: 设置缩放控制组件是哪个y轴， 一般写0即可

```js
dataZoom: [
    {
        type: slider,
        xAxisIndex: 0
    }
]
```

- 指明初始的缩放情况start,end

## 6.饼图pie

```html
<body>
    <script src="./lib/echarts.min.js"></script>
    <div style="width: 600px; height: 400px"></div>
    <script>
        var mcharts = echarts.init(document.querySelector('div'))
        var pieData = [
            {
                name: '淘宝',
                value: '11231',
            },
            {
                name: '京东',
                value: '22673',
            },
            {
                name: '唯品会',
                value: '6123',
            },
            {
                name: '一号店',
                value: '8989',
            },
            {
                name: '聚美优品',
                value: '6700',
            },
        ]
        var option = {
            series: [
                {
                    type: 'pie',
                    data: pieData,
                    label: {
                        // 显示文字效果
                        show: true,
                        formatter: function (arg) {
                            return (
                                arg.data.name +
                                '平台' +
                                arg.data.value +
                                '元\n' +
                                arg.percent +
                                '%'
                            )
                        },
                    },
                },
            ],
        }

        mcharts.setOption(option)
    </script>
</body>
```

### 显示数值**label.formatter**

**label.formatter**

### 圆环radius

```js
 radius: 20// 设置圆环的半径
 radius: '50%'// 设置圆环的半径, 参照高度和宽度中较小的那一部分的百分比来进行设置

 radius: ['50%', '75%'] // 第一个代表内圆环的半径，第二个代表外圆环的半径
```

![image-20210208213520218](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210208213520218.png)

### 南丁格尔图roseType

```js
roseType: 'radius'
```

![image-20210208213711037](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210208213711037.png)

```js
// selectedMode: 'multiple' //偏离原点一点距离
selectedMode: 'single'
selectedOffset: 30
```

![image-20210208214252817](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210208214252817.png)

## 7.地图map

![image-20210209093557852](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210209093557852.png)

![image-20210209094043402](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210209094043402.png)

![image-20210209094807372](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210209094807372.png)

![image-20210209101600337](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210209101600337.png)

## 8.雷达图radar: 多个纬度的数据进行对比

```html
<script src="./lib/echarts.min.js"></script>

    <!-- 步骤2：准备液体个呈现图标的盒子 -->
    <div style="width: 600px; height: 400px"></div>
    <script>
        var mcharts = echarts.init(document.querySelector('div'))
        var dataMax = [
            {
                name: '易用性',
                max: 100,
            },
            {
                name: '功能',
                max: 100,
            },
            {
                name: '拍照',
                max: 100,
            },
            {
                name: '跑分',
                max: 100,
            },
            {
                name: '续航',
                max: 100,
            },
        ]
        var option = {
            radar: {
                indicator: dataMax, // 指示器
            },
            series: [
                {
                    type: 'radar',
                    data: [
                        {
                            name: '华为手机1',
                            value: [80, 90, 82, 90, 92],
                        },
                        {
                            name: '中兴手机1',
                            value: [77, 76, 80, 83, 80],
                        },
                    ],
                },
            ],
        }
        mcharts.setOption(option)
    </script>
```

![image-20210209103957531](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210209103957531.png)

### 显示数值label

### 区域面积areaStyle

### 绘制类型shape

```js
radar: {
    indicator: dataMax, // 指示器
  	shape: 'circle' //配置雷达图最外层的图形, 默认值polygon多边形
},
```

![image-20210209104551627](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210209104551627.png)

## 9.仪表盘gauge ：进度的监控

### 数值范围max，min

### 多个指针，增加数组中的对象

### 多个指针的颜色差异itemStyle

```js
series: [
  {
    type: 'gauge',
    data: [
      // 每一个对象就代表一个指针
      {
        value: 97,
        itemStyle: {
          color: 'pink'
        }
      },
      {
        value: 67
      },
    ],
    min: 50,
    max: 100
  }
]
```

![image-20210209105640164](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210209105640164.png)

## 10.总结

![image-20210209105959131](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210209105959131.png)

![image-20210209110203001](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210209110203001.png)