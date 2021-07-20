# Vue3 学习笔记基础

# 与vue2相比基础的地方有太多相似之处不再记述

# 一、组件化开发

## 1.父传子：

```
App.vue
 <son msg="hello world"></son>
复制代码
son.vue
export default {
  // props: ['msg']
  props: {
    msg: {
      type: String,
      default: "default"
    }
  }
}
复制代码
```

## 2.子传父

```
App.vue
<template>
  <h2>计数：{{counter}}</h2>
  <son @add="add"></son>
</template>
<script>
  methods: {
    add() {
      this.counter++
    }
  },
}
</script>
复制代码
Son.vue
<template>
  <button @click="increment">+1</button>
</template>
<script>
export default {
  emits: ['add', 'sub'],
  methods: {
    increment() {
      this.$emit('add')
    }
  }
}
</script>
复制代码
```

## 3.provide和inject

用于根组件和子孙组件之间传递数据

![image-20210608181527801.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e1ae8cca54c844379ec98fb677c5e058~tplv-k3u1fbpfcp-watermark.image)

```
App.vue
<template>
    <father></father>
    <button @click="addName">+name</button>
</template>
<script>
export default {
  provide() {
    return {
      name: 'yogln',
      msg: this.msg,
      names: computed(() => this.names) //利用computed做响应式
    }
  },
  data() {
    return {
      msg: 'hello world',
      names: ['abc', 'cba', 'nba']
    }
  },
  methods: {
    addName() {
      this.names.push('why')
    }
  },
}
</script>
复制代码
Son.vue
<script>
  export default {
    inject: ["name", "msg", "names"]
  }
</script>
复制代码
```

## 4.事件总线

有于`vue`取消了`$on`,`$off`等事件，所以只能依赖第三方库`mitt`进行非父子组件之间的通信，安装依赖

```
npm i mitt
复制代码
```

封装`mitt.js`

```js
import mitt from 'mitt'
export default mitt()
复制代码
```

使用方式：

发送方：

```vue
<script>
import emitter from './utils/eventBus'
  export default {
    methods: {
      btnClick() {
        console.log('btnclick')
        emitter.emit("yogln", "yogln事件")
      }
    },
  }
</script>
复制代码
```

接受方：

```vue
<script>
import emitter from './utils/eventBus'
  export default {
    created() {
      emitter.on('yogln', msg => {
        console.log(msg)
      })

      emitter.on('*', (type, msg) => {
        console.log(type, msg)
      })
    },
  }
</script>
复制代码
```

## 5.具名插槽的使用

```
NavBar.vue
 <div class="nav-bar">
    <div class="left">
      <slot name="left"></slot>
    </div>
    <div class="center">
      <slot name="center"></slot>
    </div>
    <div class="right">
      <slot name="right"></slot>
    </div>
  </div>
复制代码
App.vue
 <div>
    <nav-bar>
      <template v-slot:center>
        <button>按钮</button>
      </template>
      <template #right>
        <span>右边</span>
      </template>
    </nav-bar>
  </div>
复制代码
```

**注意：v-slot:可以简写成#**

### 动态插槽

我们可以使用`v-slot:[dynamicSlotName]`方式动态绑定一个名称

```vue
<template v-slot=[name]>
  ···
</template>
复制代码
```

## 6.作用域插槽的使用

### 渲染作用域

在Vue中有渲染作用域的概念：

- 父级模板里的所有内容都是在父级作用域中编译的；
- 子模板里的所有内容都是在子作用域中编译的；

案例：

![image-20210608204005205.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9a19654f23f6412eacfa98eb21ee3c2e~tplv-k3u1fbpfcp-watermark.image)

```
App.vue
<show :names="names">
  <template #="slotProps">
    <button>{{slotProps.item}}</button>
  </template>
</show>
复制代码
Show.vue
<template v-for="(item, index) in names" :key="item">
  <slot :item="item" :index="index"></slot>
</template>
复制代码
```

### 作用域插槽与具名插槽的混用

```
App.vue
<template>
  <show :names="names">
    <template #yogln="slotProps">
      <button>{{slotProps.item}}</button>
    </template>
  </show>
</template>
复制代码
Show.vue
<template>
  <template v-for="(item, index) in names" :key="item">
    <slot name="yogln" :item="item" :index="index"></slot>
  </template>
</template>
复制代码
```

### 独占默认插槽的缩写

```
App.vue
<template>
  <show :names="names" #="slotProps">
      <button>{{slotProps.item}}</button>
  </show>
</template>
复制代码
```

注意

如果我们有默认插槽和具名插槽，那么按照完整的template来编写，只要出现多个插槽，请始终为所有的插槽使用完整的基于 `<template> `的语法：

## 7.动态组件

动态组件是使用 component 组件，通过一个特殊的attribute is 来实现 ， 注意`currentTab`时一个组件

```vue
<component :is="currentTab"></component>
复制代码
```

动态组件传递参数和传递参数一样

```vue
<component :is="currentTab" :age="18"></component>
复制代码
```

**注意，这里的age加了 `:` 表示18的一个Number类型，不加 `:` 则是字符串类型**

## 8.keep-alive

```vue
<keep-alive>
    <component :is="currentTab"></component>	
</keep-alive>
复制代码
```

keep-alive有一些属性：

- include - string | RegExp | Array。只有名称匹配的组件会被缓存；
- exclude - string | RegExp | Array。任何名称匹配的组件都不 会被缓存；
- max - number | string。最多可以缓存多少组件实例，一旦达 到这个数字，那么缓存组件中最近没有被访问的实例会被销毁；

include 和 exclude prop 允许组件有条件地缓存：

- 二者都可以用逗号分隔字符串、正则表达式或一个数组来表示；
- 匹配首先检查组件自身的 name 选项

![image-20210610143630010.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/da810c2ef06b4b80aefd2796ea11edf0~tplv-k3u1fbpfcp-watermark.image)

## 9.异步组件的使用

通过Import()函数引用资源会对打包文件进行分包

```js
import("./utils/math.js").then(res => res.sum(20, 30))

import("./utils/math.js").then({sum})
复制代码
```

### 不使用路由打包进行分包

```vue
<script>
import { defineAsyncComponent } from 'vue' 
const AsyncAbout = defineAsyncComponent(() => import('./About.vue')) 
  export default {
    components: {
      AsyncAbout
    }
  }
</script>
复制代码
```

另一种写法：

```vue
<script>
const AsyncAbout = defineAsyncComponent({
  loader: () => import('./About.vue')
  // loadingCompoent: Loading // 加载组件的时候显示的loading组件
  // errorCompoent: 出错时显示的组件
  // delay: 2000 //loadingComponent显示之前的延迟事件
})
</script>
复制代码
```

### suspense和异步组件的使用

```vue
<suspense>
  <template #default>
    <async-about></async-about>
  </template>
  <template #fallback>
    <loading></loading>
  </template>
</suspense>
复制代码
```

**suspense组件当前还处于实验阶段**

## 10.`$refs`、`$parent`、`$root`、`$el`

```vue
<home ref="homeRef"></home>
复制代码
```

通过`this`进行调用，可以调用组件的`data`和`methods`

`$parent`引用父组件

`$root`引用根组件

`$el`引用组件里面的el,`this.$refs.homeRef.$el`可以获得组件内的所有标签

**注意：vue3中已经移除了this.$children**

## 11.组件的生命周期

![image-20210610160630994.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/348a342d0c284c30b917f62034eaebff~tplv-k3u1fbpfcp-watermark.image)

**组件使用了`leep-alive`时候还会有两个生命周期：`activited`和`deactivited`**

## 12.组件的v-model

### 基本使用

```
App.vue
<my-input v-model="message"></my-input>

// 实际上所做的事情 <my-input :="message" @update:modelValue="message = $event"></my-input> 和input组件的v-model类似
复制代码
MyInput.vue
<template>
    <h2>modelValue:  {{modelValue}}</h2>
    <input :value="modelValue" @input="inputChange">
</template>

<script>
  export default {
    props: {
      modelValue: {
        type: String
      }
    },
    emits: ["update:modelValue"],
    methods: {
      inputChange(event) {
        this.$emit("update:modelValue", event.target.value)
      }
    },
  }
</script>
复制代码
```

**注意： modelValue是可以修改的，通过`v-model:title="title"`**

### 通过计算属性绑定input

```vue
<input :value="value" @input="inputChange">
复制代码
computed: {
  value: {
    set(value) {
      this.$emit("update:modelValue", value)
    },
    get() {
      return this.modelValue
    }
  }
}
复制代码
```

![image-20210610164245512.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/73f140e3be2e415299817eca5b7b3cad~tplv-k3u1fbpfcp-watermark.image)

### 通过一个input组件传递不同的参数

```
App.vue
<template>
    <my-input v-model="message"  v-model:title="title"></my-input>
    <h2>modelValue:  {{message}}</h2>
    <h2>title:  {{title}}</h2>
</template>
<script>
import MyInput from './MyInput2.vue'
  export default {
    components: {
      MyInput
    },
    data() {
      return {
        message: 'message',
        title: 'title'
      }
    },
  }
</script>
复制代码
Myinput.vue
<template>  
    <input v-model="value">
    <input v-model="titleValue">
</template>
<script>
  export default {
    props: {
      modelValue: String,
      title: String
    },
    emits: ["update:modelValue", "update:title"],
    computed: {
      value: {
        set(value) {
          this.$emit("update:modelValue", value)
        },
        get() {
          return this.modelValue
        }
      },
      titleValue: {
        set(titleValue) {
          this.$emit("update:title", titleValue)
        },
        get() {
          return this.title
        }
      }
    }
  }
</script>
复制代码
```

![image-20210610170720182.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/382fcff35a234587a064890fc787ef1b~tplv-k3u1fbpfcp-watermark.image)

# 二、动画

## 1.动画的基本使用

```vue
<template>
    <div>
        <button @click="isShow = !isShow">显示/隐藏</button>
        <transition name="yogln">
          <h2 v-if="isShow">Hello world</h2>
        </transition>
    </div>
</template>

<style scoped>
.yogln-enter-from,
.yogln-leave-to {
  opacity: 0;·
}

.yogln-enter-to,
.yogln-leave-from {
  opacity: 1;
}

.yogln-enter-active,
.yogln-leave-active {
  transition: opacity 1s ease;
}
</style>
复制代码
```

![image-20210618111358562.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2947eb5b327d4f45bb4d63e54ebc304b~tplv-k3u1fbpfcp-watermark.image)

当插入或删除包含在 transition 组件中的元素时，Vue 将会做以下处理：

1. 自动嗅探目标元素是否应用了CSS过渡或者动画，如果有，那么在恰当的时机添加/删除 CSS类名；
2. 如果 transition 组件提供了JavaScript钩子函数，这些钩子函数将在恰当的时机被调用；
3. 如果没有找到JavaScript钩子并且也没有检测到CSS过渡/动画，DOM插入、删除操作将会立即执行；

我们会发现上面提到了很多个class，事实上Vue就是帮助我们在这些class之间来回切换完成的动画：

- v-enter-from：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。
- v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动 画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
- v-enter-to：定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter-from 被移除)，在过渡/ 动画完成之后移除。
- v-leave-from：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。
- v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在 过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
- v-leave-to：离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave-from 被删除)，在过渡/ 动画完成之后移除。

### class添加的时机和命名规则

![image-20210618114404678.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/12e1283a8b5b45819a95909a4dbea0f7~tplv-k3u1fbpfcp-watermark.image)

class的name命名规则如下：

- 如果我们使用的是一个没有name的transition，那么所有的class是以 v- 作为默认前缀；
- 如果我们添加了一个name属性，比如 ，那么所有的class会以 why- 开头；

## 2.css过渡animation的基本使用

```js
<template>
  <div>
    <div class=""><button @click="isShow = !isShow">显示/隐藏</button></div>
    <transition name="yogln">
      <h2 class="title" v-if="isShow">Hello world</h2>
    </transition>
  </div>
</template>

<style scoped>
.title {
  width: 200px;
  margin: 0 auto;
}

.yogln-enter-active {
  animation: bounce 1s;
}

.yogln-leave-active {
  animation: bounce 1s reverse;
}

@keyframes bounce {
  0% {
    transform: scale(0);
  }

  50% {
    transform: scale(1.2);
  }

  100% {
    transform: scale(1);
  }
}
</style>
复制代码
```

**注意：`h2`标签是一个块级元素，独占一行，如果不修改宽度会出现意料之外的效果**

## 3.过渡和动画一起使用

### type属性

过渡`animation`和动画`transform`是可以一起使用的，但是如果两者的时间使用不一致也会出现一些问题。例如：

```css
.yogln-enter-active,
.yogln-leave-active {
  transition: opacity 2s ease;
}

.yogln-enter-active {
  animation: bounce 1s;
}

.yogln-leave-active {
  animation: bounce 1s reverse;
}
复制代码
```

这个时候就会出现`animation `结束了而`transition`还没有结束的问题，可以通过指定`transition`的`type`属性指定按照哪个属性的执行时间为准

```vue
<transition name="yogln" type="animation">
  <h2 class="title" v-if="isShow">Hello world</h2>
</transition>
复制代码
```

### duration属性

使用`duration`可以指定动画的时间

```vue
<transition name="yogln" type="animation" :duration="1000">
  <h2 class="title" v-if="isShow">Hello world</h2>
</transition>
复制代码
```

`duration`也可以写成对象的形式

```vue
<transition name="yogln" 
            type="animation" 
            :duration="{enter: 800,leave: 1000}">
  <h2 class="title" v-if="isShow">Hello world</h2>
</transition>
复制代码
```

### mode属性

切换两个标签或者两个组件的动画效果时，默认效果是非常难看的，我们这个时候就可以使用到`mode`属性,mode有两个值

- in-out:指定要展示的组件先in再将不展示的组件out
- out-in:指定不展示的组件先out再将要展示的组件in

```vue
<transition name="yogln" type="animation" mode="out-in">
  <h2 class="title" v-if="isShow">Hello world</h2>
  <h2 class="title" v-else>你好啊,李银河</h2>
</transition>
复制代码
```

### appear属性

当我们想在第一次加载页面的时候就有动画效果，可以添加`appear`属性

```vue
<transition name="yogln" type="animation" mode="out-in" appear>
  <h2 class="title" v-if="isShow">Hello world</h2>
  <h2 class="title" v-else>你好啊,李银河</h2>
</transition>
复制代码
```

## 4.组件的过渡

组件的动画效果使用也是类似的

```vue
<transition name="yogln" type="animation" mode="out-in" appear>
  <component :is="isShow ? 'home' : 'about'"></component>
</transition>
复制代码
```

## 5.第三方库的使用animate.css

安装`animate.css`

```
npm i animate.css
复制代码
```

在`main.js`中导入`animate.css`：

```js
import "animate.css"
复制代码
```

[动画网站](https://link.juejin.cn/?target=https%3A%2F%2Fanimate.style%2F%23documentation)

### 使用方式一：

直接使用animate库中定义的 keyframes 动画

```vue
<template>
    <button @click="isShow = !isShow">切换</button>
    <transition name="yogln">
      <h2 class="title" v-if="isShow">Hello World</h2>
    </transition>
</template>
<style scoped>
.yogln-enter-active {
  animation: flip 1s;
}
.yogln-leave-active {
  animation: flip 1s reverse;
}
</style>
复制代码
```

### 使用方式二：

直接使用animate库提供给我们的类

```vue
<transition enter-active-class="animate__animated animate__bounceInUp"
            leave-active-class="animate__animated animate__zoomOutDown">
  <h2 class="title" v-if="isShow">Hello World</h2>
</transition>
复制代码
```

## 6.第三方库的使用gsap

### gsap的动画效果

安装

```
npm i gsap
复制代码
```

在使用动画之前，我们先来看一下transition组件给我们提供的JavaScript钩子，这些钩子可以帮助我们监听动画执行到 什么阶段了。

![image-20210618145528865.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8f99064e10204bd985b19b5902a0536e~tplv-k3u1fbpfcp-watermark.image) 当我们使用JavaScript来执行过渡动画时，需要进行 done 回调，否则它们将会被同步调用，过渡会立即完成。

添加 :css="false"，也会让 Vue 会跳过 CSS 的检测，除了性能略高之外，这可以避免过渡过程中 CSS 规则的影响。

```vue
<transition name="yogln" @enter="enter" @leave="leave">
  <h2 class="title" v-if="isShow">Hello World</h2>
</transition>

<script>
import gsap from 'gsap'
export default {
  data() {
    return {
      isShow: true,
    }
  },
  methods: {
    enter(el, done) {
      gsap.from(el, {
        scale: 0,
        x: 200,
        onComplete: done
      })
    },
    leave(el, done) {
      gsap.to(el, {
        scale: 0,
        x: 200,
        onComplete: done
      })
    },
  },
}
</script>
复制代码
```

### gsap实现数字变换

```vue
<template>
    <input type="number" step="100" v-model="counter">
    <h2>当前计数： {{showNumber.toFixed(0)}}</h2>
</template>
<script>
import gsap from 'gsap'
export default {
  data() {
    return {
      counter: 0,
      showNumber: 0
    }
  },
  watch: {
    counter(newValue) {
      gsap.to(this, {
        duration: 1,
        showNumber: newValue,
      })
    },
  },
}
</script>
复制代码
```

## 7.列表过渡的移动动画

使用`transition-group`组件可以实现列表的变换

### 使用列表过渡动画实现数字的变换

![image-20210618164706878.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/acea61d9b4a74d3f9633d5b4713ccfcc~tplv-k3u1fbpfcp-watermark.image)

```vue
<template>
  <div>
    <button @click="addNum">添加数字</button>
    <button @click="delNum">删除数字</button>
    <button @click="shuffle">数字洗牌</button>

    <transition-group name="yogln" tag="p">
      <span v-for="item in numbers" :key="item" class="item">{{item}}</span>
    </transition-group>
  </div>
</template>
<script>
import _ from 'loadsh'
export default {
  data() {
    return {
      numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9],
      currNum: 10,
    }
  },
  methods: {
    addNum() {
      this.numbers.splice(this.randomIndex(), 0, this.currNum++)
    },
    delNum() {
      this.numbers.splice(this.randomIndex(), 1)
    },
    randomIndex() {
      return Math.floor(Math.random() * this.numbers.length)
    },
    shuffle() {
      this.numbers = _.shuffle(this.numbers)
    }
  },
}
</script>

<style scoped>
.item {
  margin: 0 10px;
  display: inline-block;
}

.yogln-enter-from,
.yogln-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.yogln-enter-active,
.yogln-leave-active {
  transition: all 1s ease;
}

.yogln-leave-active {
  position: absolute;
}

.yogln-move {
  transition: transform 1s ease;
}
</style>
复制代码
```

`.yogln-move `主要是在插入或者移除元素的时候，右边的一堆数字有过渡效果

### 列表动画实现交替动画

![image-20210618164633418.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5e7b91cfcc164c58bd835f1eabc7fabd~tplv-k3u1fbpfcp-watermark.image)

```vue
<template>
  <div>
    <input type="text" v-model="keyword" @input="input">
    <transition-group tag="ul" name="yogln" :css="false" 
                      @before-enter="beforeEnter" 
                      @enter="enter" 
                      @leave="leave">
      <li v-for="name, index in showNames" :key="name" :data-index="index">{{name}}</li>
    </transition-group>
  </div>
</template>

<script>
import gsap from 'gsap'
export default {
  data() {
    return {
      names: ['abc', 'cba', 'nba', 'why', 'lilei', 'hmm', 'kobe', 'james'],
      keyword: ''
    }
  },
  computed: {
    showNames() {
      return this.names.filter(item => item.indexOf(this.keyword) !== -1)
    }
  },
  methods: {
    beforeEnter(el) {
      el.style.opacity = 0
      el.style.height = 0
    },
    enter(el, done) {
      gsap.to(el, {
        height: '1.5em',
        opacity: 1,
        delay: el.dataset.index * 0.3,
        onComplete: done,
      })
    },
    leave(el, done) {
      gsap.to(el, {
        height: 0,
        opacity: 0,
        delay: el.dataset.index * 0.3,
        onComplete: done,
      })
    },
  },
}
</script>
复制代码
```

**注意此处一个小技巧，通过`:data-index="index"`,在方法中通过`dataset.index`获得传递来的索引值**

# 三、Composition API

## 1.mixin

### mixin的基本使用

```vue
<script>
	import demomixin from './mixin/demomixin'
    export default {
        mixin: [demomixin]
    }
</script>
复制代码
```

### mixin的合并规则

如果Mixin对象中的选项和组件对象中的选项发生了冲突，那么Vue会如何操作呢？

这里分成不同的情况来进行处理；

1. 情况一：如果是data函数的返回值对象
   - 返回值对象默认情况下会进行合并；
   - 如果data返回值对象的属性发生了冲突，那么会保留组件自身的数据；
2. 情况二：如何生命周期钩子函数
   - 生命周期的钩子函数会被合并到数组中，都会被调用；
3. 情况三：值为对象的选项，例如 methods、components 和 directives，将被合并为同一个对象。
   - 比如都有methods选项，并且都定义了方法，那么它们都会生效；
   - 但是如果对象的key相同，那么会取组件对象的键值对

### 全局混入Mixin

如果组件中的某些选项，是所有的组件都需要拥有的，那么这个时候我们可以使用全局的mixin

- 全局的Mixin可以使用 应用app的方法 mixin 来完成注册；
- 一旦注册，那么全局混入的选项将会影响每一个组件；

```js
const app = createApp(App)
app.mixin({
  created() {
    console.log("created");
  },
})
app.mount('#app')
复制代码
```

## 2.extends(了解)

extends类似于mixin却没有mixin灵活所以用的很少

```vue
<script>
import BasePage from './BasePage.vue'
export default {
	extends: BasePage
}
</script>
复制代码
```

## 3.setup()函数的参数

我们先来研究一个setup函数的参数，它主要有两个参数：

- 第一个参数：props
- 第二个参数：context

props非常好理解，它其实就是父组件传递过来的属性会被放到props对象中，我们在setup中如果需要使用，那么就可以直接通过props参数获取：

- 对于定义props的类型，我们还是和之前的规则是一样的，在props选项中定义；
- 并且在template中依然是可以正常去使用props中的属性，比如message；
- 如果我们在setup函数中想要使用props，那么不可以通过 this 去获取（后面我会讲到为什么）；
- 因为props有直接作为参数传递到setup函数中，所以我们可以直接通过参数来使用即可；

另外一个参数是context，我们也称之为是一个SetupContext，它里面包含三个属性：

- attrs：所有的非props的attribute；
- slots：父组件传递过来的插槽（这个在以渲染函数返回时会有作用，后面会讲到）；
- emit：当我们组件内部需要发出事件时会用到emit（因为我们不能访问this，所以不可以通过 this.$emit发出事件）

## 4.setup()函数的返回值

setup既然是一个函数，那么它也可以有返回值，它的返回值用来做什么呢？

setup的返回值可以在模板template中被使用； 也就是说我们可以通过setup的返回值来替代data选项；

甚至是我们可以返回一个执行函数来代替在methods中定义的方法

```vue
<script>
export default {
  setup(props, ctx) {
    const name = 'yogln'
    const counter = 100
    const increment = () => {
      counter++
    }
    return {
      name,
      increment
    }
  },
}
</script>
复制代码
```

**setup不可以使用this**

**setup不可以使用this**

**setup不可以使用this**

## 5.setup()函数实现相应式刷新

### reactive API

```vue
<template>
  <div>
    <h2>{{state.name}}</h2>
    <h2>{{state.counter}}</h2>
    <button @click="increment">+1</button>
  </div>
</template>
<script>
import { reactive } from 'vue'
export default {
  setup(props, ctx) {
    const state = reactive({
      name: 'yogln',
      counter: 100,
    })
    const increment = () => {
      state.counter++
    }
    return {
      state,
      increment,
    }
  },
}
</script>
复制代码
```

那么这是什么原因呢？为什么就可以变成响应式的呢？

这是因为当我们使用reactive函数处理我们的数据之后，数据再次被使用时就会进行依赖收集； 当数据发生改变时，所有收集到的依赖都是进行对应的响应式操作（比如更新界面）； 事实上，我们编写的data选项，也是在内部交给了reactive函数将其编程响应式对象的

### ref API

reactive API对传入的类型是有限制的，它要求我们必须传入的是一个对象或者数组类型：

- 如果我们传入一个基本数据类型（String、Number、Boolean）会报一个警告

这个时候Vue3给我们提供了另外一个API：ref API

- ref 会返回一个可变的响应式对象，该对象作为一个 响应式的引用 维护着它内部的值，这就是ref名称的来源；
- 它内部的值是在ref的 value 属性中被维护的

```vue
<template>
  <div>
    <h2>{{counter}}</h2>
    <button @click="increment">+1</button>
  </div>
</template>

<script>
import { ref } from 'vue'
export default {
  setup(props, ctx) {
    let counter = ref(100)
    const increment = () => {
      counter.value++
    }
    return {
      counter,
      increment
    }
  },
}
</script>
复制代码
```

这里有两个注意事项：

- 在模板中引入ref的值时，Vue会自动帮助我们进行解包操作，所以我们并不需要在模板中通过 ref.value 的方式 来使用；
- 但是在 setup 函数内部，它依然是一个 ref引用， 所以对其进行操作时，我们依然需要使用 ref.value的方式

模板中的解包是浅层的解包，如果我们的代码是下面的方式： 如果我们将ref放到一个reactive的属性当中，那么在模板中使用时，它会自动解包

### ref的浅层解包

```vue
<template>
  <div>
    Home Page
    <h2>{{message}}</h2>
    <!-- 当我们在template模板中使用ref对象, 它会自动进行解包 -->
    <h2>当前计数: {{counter}}</h2>
    <!-- ref的解包只能是一个浅层解包(info是一个普通的JavaScript对象) -->
    <h2>当前计数: {{info.counter.value}}</h2>
    <!-- 当如果最外层包裹的是一个reactive可响应式对象, 那么内容的ref可以解包 -->
    <h2>当前计数: {{reactiveInfo.counter}}</h2>
    <button @click="increment">+1</button>
  </div>
</template>

<script>
  import { ref, reactive } from 'vue';
  export default {
    setup() {
      let counter = ref(100);

      const info = {
        counter
      }
      
      const reactiveInfo = reactive({
        counter
      })

      // 局部函数
      const increment = () => {
        counter.value++;
        console.log(counter.value);
      }

      return {
        counter,
        info,
        reactiveInfo,
        increment
      }
    }
  }
</script>
复制代码
```

## 6.readonly

readonly会返回原生对象的只读代理（也就是它依然是一个Proxy，这是一个proxy的set方法被劫持，并且不 能对其进行修改）

在开发中常见的readonly方法会传入三个类型的参数：

- 类型一：普通对象；
- 类型二：reactive返回的对象；
- 类型三：ref的对象

readonly返回的对象都是不允许修改的；

但是经过readonly处理的原来的对象是允许被修改的； 比如 const info = readonly(obj)，info对象是不允许被修改的； 当obj被修改时，readonly返回的info对象也会被修改； 但是我们不能去修改readonly返回的对象info； 其实本质上就是readonly返回的对象的setter方法被劫持了而已

```vue
<script>
import {readonly} from 'vue'
  export default {
    setup(props, ctx) {
      const info = {
        name: "yogln"
      }
      const readInfo = readonly(info)
      readInfo.name = "why"
    }
  }
</script>
复制代码
```

![image-20210620232952295]()

## 7.reactive判断的API

### isProxy

- 检查对象是否是由 reactive 或 readonly创建的 proxy。

### isReactive

- 检查对象是否是由 reactive创建的响应式代理：
- 如果该代理是 readonly 建的，但包裹了由 reactive 创建的另一个代理，它也会返回 true；

### isReadonly

- 检查对象是否是由 readonly 创建的只读代理。

### toRaw

- 返回 reactive 或 readonly 代理的原始对象（不建议保留对原始对象的持久引用。请谨慎使用）。

### shallowReactive

- 创建一个响应式代理，它跟踪其自身 property 的响应性，但不执行嵌套对象的深层响应式转换 (深层还是原生对象)。

### shallowReadonly

- 创建一个 proxy，使其自身的 property 为只读，但不执行嵌套对象的深度只读转换（深层还是可读、可写的）

## 8.refAPI

### toRefs

如果我们使用ES6的解构语法，对reactive返回的对象进行解构获取值，那么之后无论是修改结构后的变量，还是修改reactive 返回的state对象，数据都不再是响应式的.

Vue为我们提供了一个toRefs的函数，可以将reactive返回的对象中的属性都转成ref； 那么我们再次进行结构出来的 name 和 age 本身都是 ref的

这种做法相当于已经在state.name和ref.value之间建立了链接，任何一个修改都会引起另外一个变化

```vue
<template>
    <button @click="changeAge">按钮</button>
    <h2>{{name}}--{{age}}</h2>
</template>
<script>
import { reactive, toRefs } from 'vue'
export default {
  setup(props) {
    const state = reactive({
      name: 'yogln',
      age: 18
    })
    let { name, age} = toRefs(state)
    const changeAge = () => {
      age.value++
    }
    return {
      name, 
      age,
      changeAge
    }
  },
}
</script>
复制代码
```

### toRef

```js
const { name } = state
let age = toRef(state, 'age')
const changeAge = () => {
  age.value++
}
复制代码
```

### unref

如果我们想要获取一个ref引用中的value，那么也可以通过unref方法：

如果参数是一个 ref，则返回内部值，否则返回参数本身；

这是 `val = isRef(val) ? val.value : val `的语法糖函数；

### isRef

判断值是否是一个ref对象。

### shallowRef

创建一个浅层的ref对象；

### triggerRef

手动触发和 shallowRef 相关联的副作用

### customRef

创建一个自定义的ref，并对其依赖项跟踪和更新触发进行显示控制：

- 需要一个工厂函数，该函数接受 track 和 trigger 函数作为参数；
- 并且应该返回一个带有 get 和 set 的对象；

自定义Ref的使用，实现文本框的防抖效果

```
app.vue
<template>
  <div>
    <input type="text" v-model="message">
    <h2>{{message}}</h2>
  </div>
</template>
<script> 
import useDebounceRef from './hook/useDebounceRef'
export default {
  setup(props) {
    const message = useDebounceRef('Hello World')
    return {
      message
    }
  },
}
</script>
复制代码
useDebounceRef.js
import {customRef} from 'vue'  

export default function(value) {
  let timer = null
  return customRef((track, trigger) => {
    return {
      get() {
        track()
        return value
      },
      set(newValue) {
        clearTimeout(timer)
        timer = setTimeout(() => {
          value = newValue
          trigger()
        }, 1000);
      }
    }
  })
}
复制代码
```

## 9.computed

- 方式一：接收一个getter函数，并为 getter 函数返回的值，返回一个不变的 ref 对象；
- 方式二：接收一个具有 get 和 set 的对象，返回一个可变的（可读写）ref 对象

```vue
<template>
    <h2>{{fullName}}</h2>
    <button @click="changeName">按钮</button>
</template>
<script>
import { computed, ref } from 'vue'
export default {
  setup(props) {
    const firstName = ref('Kobe')
    const lastName = ref('Bright')

    const fullName = computed({
      get: () => {
        return firstName.value + ' ' + lastName.value
      },
      set: (newValue) => {
        const names = newValue.split(' ')
        firstName.value = names[0]
        lastName.value = names[1]
      }
    })

    const changeName = () => {
      fullName.value = 'coder yogln'
    }
    return {
      fullName,
      changeName,
    }
  },
}
</script>
复制代码
```

## 10.watchEffect

### watchEffect基本使用

watchEffect传入的函数会被立即执行一次，并且在执行的过程中会收集依赖； 收集的依赖发生变化时，watchEffect传入的函数会再次执行；

```vue
<template>
  <div>
    <h2>{{name}}--{{age}}</h2>
    <button @click="changeName">按钮</button>
  </div>
</template>
<script>
import { ref, watchEffect } from 'vue'
  export default {
    setup(props) {
      const name = ref('yogln')
      const age = ref(18)

      watchEffect(() =>{
        console.log("name", name.value, "age", age.value)
      })
      const changeName = () => {
        name.value = 'why'
      }
      return {
        name,
        age,
        changeName
      }
    }
  }
</script>
复制代码
```

![image-20210621144848469.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/82e1fd10d1a54239865418f0f8ae5af8~tplv-k3u1fbpfcp-watermark.image)

### wathEffect的停止侦听

```js
const stop = watchEffect(() =>{
    console.log("name", name.value, "age", age.value)
})

stop() // 执行后停止侦听
复制代码
```

### watchEffect取消副作用

```js
watchEffect((onInvalidate) => {
  onInvalidate(() => {
    // 清除副作用的代码，比如取消上一次的网络请求
    console.log('onInvalidate')
  })
  console.log('name', name.value, 'age', age.value)
})
复制代码
```

### watchEffect的执行时机

我们想在setup里面拿到element元素的引用，并且打印一下拿到的值

```vue
<template>
	<h2 ref="title">Hello World</h2>
</template>
<script>
import { ref, watchEffect } from 'vue'
  export default {
    setup(props) {
      const title = ref(null)
      watchEffect(() => {
        console.log(title.value)
      }, {
        // flush: 'pre' //默认
        flush: 'post' // 挂载完再执行
      })
      return {
        title
      }      
    }
  }
</script>
复制代码
```

在setup中拿到元素或者组件，我们只需要定义一个ref对象，绑定到元素或者组件的ref属性上即可。

当`flush: pre`为默认值的时候，我们会发现打印结果打印了两次：

- 这是因为setup函数在执行时就会立即执行传入的副作用函数，这个时候DOM并没有挂载，所以打印为null；
- 而当DOM挂载时，会给title的ref对象赋值新的值，副作用函数会再次执行，打印出来对应的元素；
- flush 选项还接受 sync，这将强制效果始终同步触发。然而，这是低效的，应该很少需要。

## 11.watch

### watch的基本使用

- 传入一个getter函数

```vue
<template>
    <h2>{{info.name}}</h2>
    <button @click="changeName">修改</button>
</template>

<script>
import { reactive, watch, ref } from 'vue'
export default {
  setup(props) {
    const info = reactive({
      name: 'yogln',
    })
    // 1. 传入一个getter函数
    watch(() => info.name, (newValue, oldValue) => {
      console.log('newValue:', newValue, 'oldValue:', oldValue)
      // newValue: kobe oldValue: yogln
     })
    return {
      info,
      changeName,
    }
  },
}
</script>
复制代码
```

- 传入一个传入一个可响应式对象：reactive，返回的本身都是reactive对象

```js
watch(info, (newValue, oldValue) => {
    console.log('newValue:', newValue, 'oldValue:', oldValue)
    // newValue: Proxy {name: "kobe"} oldValue: Proxy {name: "kobe"}
})
复制代码
```

- 传入一个可响应式对象：ref对象是value值本身

```js
const name = ref('yogln')
const changeName = () => {
  name.value = 'kobe'
}
watch(name, (newValue, oldValue) => {
  console.log('newValue:', newValue, 'oldValue:', oldValue)
  // newValue: kobe oldValue: yogln
})
复制代码
```

### watch侦听多个数据源

侦听多个数据源可以将传入的侦听对象用数组包裹

```js
watch([name, () => ({ ...info })], (newValue, oldValue) => {
  console.log('newValue:', newValue, 'oldValue:', oldValue)
//newValue: (2) ["kobe", {…}]0: "kobe"1: name: "yogln"__proto__: Objectlength: 2__proto__: Array(0) oldValue: (2) ["yogln", {…}]
})
复制代码
```

### watch深度侦听

**当侦听的对象是Reactive对象的时候，并且传入的对象并没有通过...解构默认开启深度侦听{deep: true}**

**但是解构后是需要手动设置开启深度侦听的**

```js
const info = reactive({
  name: 'yogln',
  friends: {
    name: 'why'
  }
})
const name = ref('yogln')
watch([name, () => ({ ...info })], (newValue, oldValue) => {
  console.log('newValue:', newValue, 'oldValue:', old
              vbbValue)
  // newValue: kobe oldValue: yogln
}, {
  deep: true,
  immediate: true // 开启首次执行
})
复制代码
```

## 12.生命周期的钩子

在setup函数中如何使用生命周期函数可以使用直接导入的`onX` 函数注册生命周期钩子；

```vue
<script>
import { onMounted, onUpdated } from 'vue'
  export default {
    setup(props) {
      onMounted(() => {
        console.log("onMounted")
      })
      onUpdated(() => {
        console.log("onUpdated")
      })
    }
  }
</script>
复制代码
```

![image-20210628143514843.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3b2e5ad9f25d476487eeb5b5192d2f89~tplv-k3u1fbpfcp-watermark.image)

在setup函数中，`beforecreate`和`created`两个生命周期没有对应的钩子，直接在`setup`函数中使用，`setup`函数执行在生命周期之前

## 13.Provide和Inject

针对子孙组件之间在setup传递值得使用方式，vue为我们提供了`provide`和`inject`两个函数

```
app.vue
<template>
    <home></home>
</template>

<script>
import { ref, provide, readonly } from 'vue'
import Home from './Home.vue'
export default {
  components: { Home },
  setup(props) {
    const name = ref('yogln')
    const age = ref(18)

    provide('name', readonly(name))
    provide('age', readonly(age))
  },
}
</script>
复制代码
```

使用readonly对数据进行包裹，保证单一数据流，防止子组件对父组件得数据进行操作。

```
home.vue
<template>
    <h2>name</h2>
    <h2>age</h2>
</template>
<script>
import {inject} from 'vue'
  export default {
    setup(props) {
      const name = inject("name", "默认名字")
      const age = inject("age", 0)
      return {
        name, 
        age
      }
    }
  }
</script>
复制代码
```

## 14.compositionAPI练习

### 计数器案例和title案例中的数据进行抽离

```vue
<template>
  <div>
    <h2>{{counter}}</h2>
    <button @click="increment">+1</button>
  </div>
</template>

<script>
import { ref } from 'vue'
import useCounter from './hook/useCounter'
import useTitle from './hook/useTitle'
export default {
  setup(props) {
    const { counter, increment } = useCounter()
    const titleRef = useTitle('yogln')

    setTimeout(() => {
      titleRef.value = "why"
    }, 3000);
    return {
      counter,
      increment,
    }
  },
}
</script>
复制代码
/hook/useCounter
import { ref } from 'vue'
export default function () {
  const counter = ref(0)
  const increment = () => {
    counter.value++
  }
  return {
    counter,
    increment
  }
}
复制代码
/hook/useTitle
import { watch, ref } from 'vue'
export default function (title = "默认标题") {
  const titleRef = ref(title)
  watch(titleRef, newValue => {
    document.title = newValue
  }, {
    immediate: true
  })
  return titleRef
}
复制代码
```

![image-20210628151746673.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9db5105464a6462b817985d9118bf30d~tplv-k3u1fbpfcp-watermark.image) 标题3s后修由`yogln`改为`why`

### 监听滚动和鼠标滑动的事件

```
App.vue
<template>
  <div>
    <p class="content"></p>
    <div class="scroll">
      <div>scrollX: {{scrollX}}</div>
      <div>scrollY: {{scrollY}}</div>
    </div>
    <div class="mouse">
      <div>mouseX: {{mouseX}}</div>
      <div>mouseY: {{mouseY}}</div>
    </div>
  </div>
</template>

<script>
import useScroll from './hook/useScroll'
import useMouse from './hook/useMouse'
export default {
  setup(props) {
    const { scrollX, scrollY} = useScroll()
    const { mouseX, mouseY} = useMouse()

    return {
      scrollX,
      scrollY,
      mouseX,
      mouseY
    }
  },
}
</script>

<style scoped>
.content {
  width: 3000px;
  height: 5000px;
}
.scroll {
  position: fixed;
  right: 50px;
  bottom: 30px;
}
.mouse {
  position: fixed;
  right: 50px;
  bottom: 90px;
}
</style>
复制代码
useScroll.js
import { ref } from 'vue'
export default function () {
  const scrollX = ref(0)
  const scrollY = ref(0)

  window.addEventListener('scroll', () => {
    scrollX.value = window.scrollX
    scrollY.value = window.scrollY
  })
  return {
    scrollX,
    scrollY
  }
}
复制代码
useMouse.js
import { ref } from 'vue'
export default function () {
  const mouseX = ref(0)
  const mouseY = ref(0)
  window.addEventListener('mousemove', (event) => {
    mouseX.value = event.pageX
    mouseY.value = event.pageY
  })  
  return {
    mouseX,
    mouseY
  }
}
复制代码
```

![image-20210628160046244.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1c67232c5b96404c85d960a5c4f101f0~tplv-k3u1fbpfcp-watermark.image)

## 15.setup的顶层编写方式(实验性)

```
App.vue
<template>
  <div>
    <h2>counter:{{counter}}</h2>
    <button @click="increment">+</button>
    <home message="嘿嘿嘿"></home>
  </div>
</template>
<script setup>
import { ref } from 'vue'
import Home from './Home.vue'
const counter = ref(0)
const increment = () => {
  counter.value++
}
</script>
复制代码
Home.vue
<template>
  <div>
    <h2>{{message}}</h2>
  </div>
</template>

<script setup>
import { defineProps } from 'vue'
const props = defineProps({
  message: {
    type: String,
    default: '哈哈哈'
  }
})
</script>
复制代码
```

# 四、其他的component

## 1.render函数的基本使用

render函数需要返回vNode节点，h函数可以帮我们返回vNode节点。

```vue
<script>
import { h } from 'vue'
  export default {
    render() {
      return h('h2', {class: 'title'}, 'Hello Render')
    }
  }
</script>
复制代码
```

![image-20210628180810810.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0b9816df09764f0c848b5e50fd0b7aa6~tplv-k3u1fbpfcp-watermark.image)

## 2.render函数实现计数器案例

```vue
<script>
import { h, ref } from 'vue'
  export default {
    setup(props) {
      const counter = ref(0)
      const increment = () => {
        counter.value++
      }
      return {
        counter,
        increment
      }
    },
    render() {
      return h('div', null, [
        h('h2', null, `当前计数：${this.counter}`),
        h('button', {
          onClick: () => {
            this.increment()
          }
        }, "+1")
      ])
    }
  }
</script>
复制代码
```

## 3.render函数组件的使用

```
App.vue
<script>
import { h } from 'vue'
import home from './Home.vue'
export default {
  render() {
    return h('div', null, [
      h('h2', null, "App component"),
      h(home, null, {
        default: props => h('span', `app传入：${props.name}`)
      })
    ])
  }
}
</script>
复制代码
Home.vue
<script>
import { h } from 'vue'
  export default {
    render() {
      return h('div', {class: 'home'}, [
        h('h2', null, "Hello World"),
        this.$slots.default ? this.$slots.default({name: 'yogln'}) : h('h2', null, '我是默认值')
      ])
    }
  }
</script>
复制代码
```

![image-20210628184902686.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f243f88d95cc47dc98523435c598beae~tplv-k3u1fbpfcp-watermark.image)

## 4.jsx的使用

```vue
<script>
import { ref } from 'vue'
export default {
  setup(props) {
    const counter = ref(0)
    const increment = () => {
      counter.value++
    }
    const decrement = () => {
      counter.value--
    }
    return {
      counter,
      increment,
      decrement
    }
  },
  render() {
    return (
      <div>
        <h2>当前计数：{this.counter}</h2>
        <button onClick={this.increment}>+1</button>
        <button onClick={this.decrement}>-1</button>
      </div>
    )
  },
}
</script>
复制代码
```

![image-20210628184836727.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7832c5abff374205a36aa8fed210ca0d~tplv-k3u1fbpfcp-watermark.image)

## 5.自定义指令

### 自定义指令实现方式

类似于v-show、v-for、v-model， Vue允许我们来定义自定义指令，通常在某些情况下，你需要对DOM元素进行底层操作，这个时候就会用到自定义指令

自定义指令分为两种：

- 自定义局部指令：组件中通过 `directives `选项，只能在当前组件中使用；
- 自定义全局指令：app的 `directive `方法，可以在任意组件中被使用；

比如我们来做一个非常简单的案例：当某个元素挂载完成后可以自定获取焦点

![image-20210628191909030.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a24014042c234b068caac5fb8a90e5db~tplv-k3u1fbpfcp-watermark.image)

1. 实现方式一：如果我们使用默认的实现方式；

   ```vue
   <template>
       <input type="text" ref="inputRef">
   </template>
   <script>
   import { ref, onMounted } from 'vue'
     export default {
       setup(props) {
         const inputRef = ref(null)
         onMounted(() => {
           inputRef.value.focus()
         })
         return {
           inputRef
         }
       }
     }
   </script>
   复制代码
   ```

2. 实现方式二：自定义一个 v-focus 的局部指令；

   ```vue
   <template>
       <input type="text" v-focus>
   </template>
   <script>
   export default {
     directives: {
       focus: {
         mounted(el) {
           el.focus()
         },
       }
     }
   }
   </script>
   复制代码
   ```

3. 实现方式三：自定义一个 v-focus 的全局指令；

```js
const app = createApp(App)
app.directive('focus', {
  mounted(el) {
    el.focus()
  }
})
app.mount('#app')
复制代码
```

### 自定义指令的生命周期

**一个指令定义的对象，Vue提供了如下的几个钩子函数：**

- created：在绑定元素的 attribute 或事件监听器被应用之前调用；
- beforeMount：当指令第一次绑定到元素并且在挂载父组件之前调用；
- mounted：在绑定元素的父组件被挂载后调用；
- beforeUpdate：在更新包含组件的 VNode 之前调用；
- updated：在包含组件的 VNode 及其子组件的 VNode 更新后调用；
- beforeUnmount：在卸载绑定元素的父组件之前调用；
- unmounted：当指令与元素解除绑定且父组件已卸载时，只调用一次；

### 自定义指令的参数和修饰符

```vue
<template>
  <div>
    <button v-yogln:info.aaa.bbb="{name: 'yogln'}">按钮</button>
  </div>
</template>
<script>
export default {
  directives: {
    yogln: {
      created(el, bindings, vNode,preNode) {
        console.log("created")
        console.log(el, bindings, vNode,preNode)
        console.log(bindings)
      },
      mounted() {
        console.log("mounted")
      },
    }
  }
}
</script>
复制代码
```

![image-20210628195227371.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c5a61f6799364fca8704baf3c6dd2ffc~tplv-k3u1fbpfcp-watermark.image)

### 自定义指令转换时间戳

转换时间戳的库`dayjs`

```
npm i dayjs
复制代码
App.vue
<template>
    <h2 v-format-time="'YY/MM/DD HH:mm:ss'">{{time}}</h2>
    <h2 v-format-time>{{time}}</h2>
</template>
<script>
import { ref } from 'vue'
  export default {
    setup(props) {
      const time = ref(1624882233)
      return {
        time
      }
    }
  }
</script>
复制代码
directive/format-time.js
import dayjs from 'dayjs';
export default function(app) {
  app.directive('format-time', {
    created(el, bindings) {
      // 拿到时间文本
      let textContent = el.textContent
      // 拿到传入的日期格式
      let formatString = bindings.value
      if(!formatString) {
        formatString = 'YYYY-MM-DD HH:mm:ss'
      }
      if(textContent.length == 10) {
        textContent = textContent * 1000
      }
      el.textContent = dayjs(textContent).format(formatString)
    },
  })
}
复制代码
directive/index.js
import formatTime from './format-time';
export default function registerTime(app) {
  formatTime(app)
}
复制代码
```

对有无传入格式的时间戳进行转化

![image-20210628203131326.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f10017a5634647d49d95f29080267faf~tplv-k3u1fbpfcp-watermark.image)

## 6.认识Teleport

我们希望组件不是挂载在这个组件树上的，可能是移动到Vue app之外的其他位置：

比如移动到body元素上，或者我们有其他的div#app之外的元素上； 这个时候我们就可以通过teleport来完成

Teleport是一个Vue提供的内置组件，类似于react的Portals；

- teleport翻译过来是心灵传输、远距离运输的意思；
- 它有两个属性：
  - to：指定将其中的内容移动到的目标元素，可以使用选择器；
  - disabled：是否禁用 teleport 的功能

`index.html`中增加一个标签

```html
<div id="yogln"></div>
复制代码
<teleport to='#yogln'>
  <h2>Teleport</h2>
  <button>按钮</button>
</teleport>
复制代码
```

![image-20210629092722953.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/67b42d2e1b8d4b589b669140667a8be8~tplv-k3u1fbpfcp-watermark.image)

这个时候发现`h2`和`button`组件的挂载由原来的`app`变成了`yogln`上。

## 7.插件的使用

通常我们向Vue全局添加一些功能时，会采用插件的模式，它有两种编写方式：

- 对象类型：一个对象，但是必须包含一个 `install `的函数，该函数会在安装插件时执行；
- 函数类型：一个function，这个函数会在安装插件时自动执行；

插件可以完成的功能没有限制，比如下面的几种都是可以的：

- 添加全局方法或者 property，通过把它们添加到 config.globalProperties 上实现；
- 添加全局资源：指令/过滤器/过渡等；
- 通过全局 mixin 来添加一些组件选项；
- 一个库，提供自己的 API，同时提供上面提到的一个或多个功能；

### 对象插件的使用方式

```js
export default {
  install(app) {
    console.log(app);
    app.config.globalProperties.$name = "yogln"
  }
}
复制代码
```

在`main.js`中

```js
import pluginObject from './components/plugin/plugin-object';
app.use(pluginObject)
复制代码
```

在全局中使用定义的`$name`

```js
import getCurrentInstance from 'vue';  
created() {
    console.log(this.$name)
  },

  setup(props) {
    const instance = getCurrentInstance()
    console.log(instance.appContext.config.globalProperties.$name)    
  }
复制代码
```

### 方法插件的使用方式

```js
export default function(app) {
  console.log(app);
}
复制代码
```

![image-20210629094148260.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7dd888c82fa04f2d9f726a1a378a3ecb~tplv-k3u1fbpfcp-watermark.image)

使用方式和对象方式一样