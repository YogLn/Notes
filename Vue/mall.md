对象判断是否为空：

```js
Object.key(obj).length === 0
```

#### 方法一：

定义一个`bus.js`文件，在里面new 一个vue得实例对象

```javascript
import Vue from 'vue'

const bus = new Vue();

export default  bus;
// 公共实例文件
// 主要用于兄弟组件之间的传值问题
1234567
```

在其它`.vue`文件中引用

```javascript
import bus from './bus.js'
```

使用

```javascript
// 发射
bus.$emit('事件', 参数)

// 接受
bus.$on('事件', 参数 => {
	
})
1234567
```

#### 方法二(推荐）：

在`main.js`中挂载`$bus`

```javascript
Vue.prototype.$bus = new Vue()
1
```

使用

```javascript
this.$bus.$emit('事件', 参数)

this.$bus.$on('事件', 参数 => {
})
```

### 解决移动端300ms延迟fastClick插件

```js
import FastClick from 'fastclick '

//解决移动端300ms延迟
FastClick. attach( document . body)

```



### 移动端图片使用懒加载vue-lazyLoad

```js
// 1.引入
import VueLazyLoad from ' vue- lazyload '
// 2.使用
Vue . use(VueLazyLoad)
// 3.img标签中src的地方使用v-lazy
<img v-lazy="" />
```

在js中引入图片使用`require()`