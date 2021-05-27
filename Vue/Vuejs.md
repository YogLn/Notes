### 1.初体验

```vue
<script>
const app = new Vue(){
	el:"#app",//用于挂载要管理的元素
	data:{//定义数据
		message:"helloVuejs"
	}
}
</script>
```

### 2.列表展示(循环)

```vue
<ul>
    <li v-for="item in movies">{{item}}</li>
</ul>
```

### 3.计数器(监听器)

实现一个简单的计数器

**方案一：**

```vue
<div>
    <button v-on:click="counter++">+</button>
    <button v-on:click="counter--">-</button>
</div>
<script>
const app = new Vue(){
	el:"#app",//用于挂载要管理的元素
	data:{//定义数据
		couter:0
	}        
}
</script>
```

**方案二：**

```vue
<div>
    <button v-on:click="add">+</button>
    <button @click="sub">-</button>//语法糖
</div>
<script>
    const app = new Vue(){
	el:"#app",//用于挂载要管理的元素
	data:{//定义数据
		couter:0
	},
    methods:{
    	add:function(){
            console.log("add()被执行");
            this.couter++
   		 }，
    	sub:function(){
            console.log("sub()被执行");
            this.couter--
    	}
}

</script>
```

### 4.插值操作

- Mustache语法{{}}

**指令：**

- v-once：响应式布局不会随着改变导致界面的改变

- v-html：将服务器返回带有标签的链接进行解析

  ```vue
  url:'<a href="http://www.baidu.com">百度一下</a>'
  <h2 v-html=url></h2>
  ```

- v-text：很少使用，会覆盖原有文本

- v-on：监听

- v-for：循环，使用时最好加上key

- v-pre：将不会解析Mustache语法

- v-cloak：cloak(斗篷)，在vue解析之前有v-cloak属性，在解析之后没有v-cloak属性，基本不会使用

- v-bind：n除了内容需要动态来决定外，某些属性我们也希望动态来绑定。

  - p比如动态绑定a元素的href属性

  - p比如动态绑定img元素的src属性

  ![image-20200911165441558](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200911165441558.png)

  语法糖写法：`<a>img :src=""></a>`

  

### 5.v-bind绑定class

- 绑定方式：对象语法

  - 用法一：直接通过{}绑定一个类

  `<h2 :class="{'active': isActive}">Hello World</h2>`

  

  - 用法二：也可以通过判断，传入多个值

  `<h2 :class="{'active': isActive, 'line': isLine}">Hello World</h2>`

  

  - 用法三：和普通的类同时存在，并不冲突

  注：如果isActive和isLine都为true，那么会有title/active/line三个类

  `<h2 class="title" :class="{'active': isActive, 'line': isLine}">Hello World</h2>`

  

  - 用法四：如果过于复杂，可以放在一个methods或者computed中

  注：classes是一个计算属性

  `<h2 class="title" :class="classes">Hello World</h2>`

- 绑定方式：数组语法

  用法一：直接通过{}绑定一个类

  `<h2 :class="['active']">Hello World</h2>`

  

  用法二：也可以传入多个值

  `<h2 :class=“[‘active’, 'line']">Hello World</h2>`

  

  用法三：和普通的类同时存在，并不冲突

  注：会有title/active/line三个类

  `<h2 class="title" :class=“[‘active’, 'line']">Hello World</h2>`

  

  用法四：如果过于复杂，可以放在一个methods或者computed中

  注：classes是一个计算属性

  `<h2 class="title" :class="classes">Hello World</h2>`

### 6.**v-bind**绑定style

- 绑定方式一：对象语法

  :style="{color: currentColor, fontSize: fontSize + 'px'}"

  style后面跟的是一个对象类型

  - 对象的key是CSS属性名称


  - 对象的value是具体赋的值，值可以来自于data中的属性

- 绑定方式二：数组语法

  `<div v-bind:style="[baseStyles, overridingStyles]"></div>`

  style后面跟的是一个数组类型多个值以分割即可

### 7.计算属性

- 计算属性是写在实例的computed选项中的

  <img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200911211329189.png" alt="image-20200911211329189" style="zoom:80%; margin-left:0px" />



<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200911211808507.png" alt="image-20200911211808507" style="zoom:80%; margin-left:0px" />

- 每个计算属性都包含一个getter和一个setter
  - 在上面的例子中，我们只是使用getter来读取。
  - 在某些情况下，你也可以提供一个setter方法（不常用）
  - 在需要写setter的时候，代码如下：

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200912095310114.png" alt="image-20200912095310114" style="zoom:80%; margin-left:0px" />

### 8.时间监听

当通过methods中定义方法，以供`@click`调用时，需要**注意参数问题**：

- 情况一：如果该方法不需要额外参数，那么方法后的()可以不添加。
  - 但是注意：如果方法本身中有一个参数，那么会默认将原生事件event参数传递进去
- 情况二：如果需要同时传入某个参数，同时需要event时，可以通过`$event`传入事件。

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200912112924827.png" alt="image-20200912112924827" style="zoom:80%; margin-left:0px" />

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200912112941483.png" alt="image-20200912112941483" style="zoom:80%; margin-left:0px" />



在某些情况下，我们拿到event的目的可能是进行一些事件处理。

- Vue提供了**修饰符**来帮助我们方便的处理一些事件：
  - `.stop` - 调用 event.stopPropagation()。
  - `.prevent` - 调用 event.preventDefault()。
  - `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。
  - `.native` - 监听组件根元素的原生事件。
  - `.once` - 只触发一次回调。

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200912113914497.png" alt="image-20200912113914497" style="zoom:67%; margin-left:0px"  />

### 9.v-if、v-else和v-if-else

这三个指令与JavaScript的条件语句if、else、else if类似。

Vue的条件指令可以根据表达式的值在DOM中渲染或销毁元素或组件

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200912131116509.png" alt="image-20200912131116509" style="zoom:80%; margin-left:0px" />

### 10.数组响应式的方法

**push()**:在数组最后插入元素

**pop()**:删除数组最后的元素

**shift()**：删除数组第一个元素

**unshift()**：在数组前面添加元素

**splice()**：可以删除元素、插入元素、替换元素

​				格式：起始位置，删除、替换元素个数，替换的元素

**sort()**：排序

**reverse()**：反转

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200912152129789.png" alt="image-20200912152129789" style="zoom:80%;margin-left:0px" />

### 11.filter()、map()和reduce()

```js
const nums = [10,20,40,100,150,30]
```

- **filter()**

  - filter中的回调函数必须有个要求，必须返回一个boolean值
  - 返回值为true时，自动将这次回调的n加入到新的数组中
  - 返回值为false时，自动过滤

  ```js
  let newNums = nums.filter(function(n){
      return n<100
  })
  ```

- **map()**

  ```js
  let new2Nums = nums.map(function(n){
      return n*2
  })
  ```

- **reduce()**:对数组中的值进行汇总

  ```js
  let total = nums.map(function(preValue,n){
      return preValue+n	
  },0)
  ```

### 12.v-model

**Vue中使用v-model指令来实现表单元素和数据的双向绑定**

v-model其实是一个语法糖，它的背后本质上是包含两个操作：

- v-bind绑定一个value属性

- v-on指令给当前元素绑定input事件

```js
<input type="text" v-model="message">
等同于
<input type="text" v-bind:value="message" v-on:input="message = $event.target.value">
```

- 复选框同意协议执行下一步

```js
<div id="app">
  <label for="agree">
    <input type="checkbox" id="agree" v-model="isAgree">同意协议
  </label>
  <button :disabled="!isAgree">下一步</button>
</div>

  <script>
    const app = new Vue({
      el: '#app',
      data: {
        isAgree: false,       
      }
    })
  </script>
```

- 复选框动态取值

```js
  <label v-for="item in originHobby">
    <input type="checkbox" :value="item" v-model="hobby">{{item}}
  </label>
  <h2>您的选择是：{{hobby}}</h2>


<script src="../js/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        hobby: [],
        originHobby:["篮球","足球","网球","乒乓球","游泳"]        
      }
    })
  </script>
```

- 修饰符

  - **lazy修饰符：**

  默认情况下，v-model默认是在input事件中同步输入框的数据的。

  也就是说，一旦有数据发生改变对应的data中的数据就会自动发生改变。

  lazy修饰符可以让数据在失去焦点或者回车时才会更新：

  - **number修饰符：**

  默认情况下，在输入框中无论我们输入的是字母还是数字，都会被当做字符串类型进行处理。

  但是如果我们希望处理的是数字类型，那么最好直接将内容当做数字处理。

  number修饰符可以让在输入框中输入的内容自动转成数字类型：

  - **trim修饰符：**

  如果输入的内容首尾有很多空格，通常我们希望将其去除

  trim修饰符可以过滤内容左右两边的空格

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200913113708028.png" alt="image-20200913113708028" style="zoom:80%; margin-left:0px" />

### 13.组件化

- 组件的使用分成三个步骤：
  1. 创建组件构造器
  2. 注册组件
  3. 使用组件。

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200913145515762.png" alt="image-20200913145515762" style="zoom:80%; margin-left:0px" />

- 父组件和子组件

  <img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200913161215609.png" alt="image-20200913161215609" style="zoom:80%;margin-left:0px" />

- 注册组件的语法糖

  在上面注册组件的方式，可能会有些繁琐。

  Vue为了简化这个过程，提供了注册的语法糖。

  主要是省去了调用Vue.extend()的步骤，而是可以直接使用一个对象来代替

  - **语法糖注册全局组件和局部组件**：

  <img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200913161445355.png" alt="image-20200913161445355" style="margin-left:0px" />

  <img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200913161455714.png" alt="image-20200913161455714" style="margin-left:0px"/>

- 模板分离写法

  Vue提供了两种方案来定义HTML模块内容：

  - 使用<script>标签

  - 使用<template>标签

    

  <img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200913171510805.png" alt="image-20200913171510805" style="zoom:80%;" />

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200913171528975.png" alt="image-20200913171528975" style="zoom:80%;" />

### 14.组件通信

![image-20200914131418725](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200914131418725.png)

#### 1.父组件向子组件传递

- 在组件中，使用选项props来声明需要从父级接收到的数据。props的值有两种方式：
  - 方式一：字符串数组，数组中的字符串就是传递时的名称。
  - 方式二：对象，对象可以设置传递时的类型，也可以设置默认值等。

```js
<div id="app">
    <cpn :cmovies="movies" :cmessage="message"></cpn>
  </div>
  <template id='cpn'>
    <div>
      <ul v-for="item in cmovies">
        <li>{{item}}</li>
      </ul>
      <h1>{{cmessage}}</h1>
    </div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const cpn = {
      template: '#cpn',
      props: ['cmessage', 'cmovies'],
      data() {
        return {}
      },
      methods: {}
    }

    const app = new Vue({
      el: "#app",
      data: {
        message: "父组件向子组件传递数据",
        movies: ["盗梦空间", "肖申克的救赎", "阿甘正传"]
      },
      components: {
        cpn
      }

    })
  </script>
```

- 在前面，我们的props选项是使用一个数组。我们说过，除了数组之外，我们也可以使用对象，当需要对**props**进行类型等验证时，就需要对象写法了。验证都支持哪些数据类型呢？String、Number、Boolean、Array、Object、Date、Function、Symbol
- 当我们有自定义构造函数时，验证也支持自定义的类型

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200914131841266.png" alt="image-20200914131841266" style="zoom:90%;margin-left:0px" />

#### 2.子组件向父组件传递

props用于父组件向子组件传递数据，还有一种比较常见的是子组件传递数据或事件到父组件中。

我们应该如何处理呢？这个时候，我们需要使用**自定义事件**来完成。

什么时候需要自定义事件呢？

当子组件需要向父组件传递数据时，就要用到自定义事件了。

我们之前学习的**`v-on`不仅仅可以用于监听DOM事件，也可以用于组件间的自定义事件**。

自定义事件的流程：

- 在子组件中，通过$emit()来触发事件。
- 在父组件中，通过v-on来监听子组件事件。

```js
 <div id="app">
    <cpn @itemclick="cpnclick"></cpn>
  </div>

  <template id="cpn">
    <div>
      <button v-for="item in categories" @click="btnClick(item)">{{item.name}}</button>
    </div>
  </template>
  <script src="../js/vue.js"></script>
  <script>
    //子组件
    const cpn = {
      template: '#cpn',
      data() {
        return {
          categories: [{
              id: 'aaa',
              name: '热门推荐'
            },
            {
              id: 'bbb',
              name: '手机数码'
            },
            {
              id: 'ccc',
              name: '家用家电'
            },
            {
              id: 'ddd',
              name: '电脑办公'
            },
          ]
        }
      },
      methods: {
        btnClick(item) {
          this.$emit('itemclick', item)
        }

      }

    }

    //父组件
    const app = new Vue({
      el: '#app',
      data: {},
      components: {
        cpn
      },
      methods: {
        cpnclick(item) {
          console.log('cpnclick', item)
        }
      }
    })
  </script>
```



### 15.组件访问

有时候我们需要父组件直接访问子组件，子组件直接访问父组件，或者是子组件访问跟组件。

- 父组件访问子组件：使用$children或$refs
- 子组件访问父组件：使用$parent
- 子组件访问根组件：使用$root

```js
components: {
        cpn: {
          template: '#cpn',
          data () {
            return {
              name: '我是子组件的name'
            }
          },
          methods: {
            showMessage() {
              console.log("showMessage")
            }
          }
        }
      },
      methods: {
        btnClick () {
          console.log(this.$children)
          console.log(this.$children[0].showMessage)
          this.$children[0].showMessage()
          this.$children[0].name
          for(let i of this.$children) {
            console.log(i.name)
            i.showMessage()
          }
        }
```

```js
cpn: {
          template: '#cpn',
          data () {
            return {
              name: '我是子组件的name'
            }
          },
          methods: {
            showMessage() {
              console.log("showMessage")
            }
          }
        }
      },
      methods: {
        btnClick () {
          console.log(this.$refs.third.name)
          this.$refs.third
        }
      }
```

```js
 components: {
            ccpn: {
              template: "#ccpn",
              methods: {
                btnClick () {
              // 访问父组件
              console.log(this.$parent)
              console.log(this.$parent.name)
            }
              }
```

```js
      components: {
        cpn: {
          template: "#cpn",
          data () {
            return {
              name: "我是cpn组件的name"
            }
          },
          components: {
            ccpn: {
              template: "#ccpn",
              methods: {
                btnClick () {
              // 访问根组件
              console.log(this.$root)
              console.log(this.$root.name)
            }
              }
```

### 16.插槽

#### 插槽的基本使用

```js
<body>
  <!-- 父组件 -->
  <div id="app">

    <cpn></cpn>
    <cpn>
      <span style="color:red;">这是插槽内容222</span>
    </cpn>
    <cpn>
      <i style="color:red;">这是插槽内容333</i>
    </cpn>
    <cpn></cpn>

  </div>

  <!-- 插槽的基本使用使用<slot></slot> -->
  <!-- 子组件 -->
  <template id="cpn">

    <div>
      <div>
        {{message}}
      </div>
      <!-- 插槽默认值 -->
      <slot><button>button</button></slot>
    </div>
  </template>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>

  <script>
    const cpn = {
      template: "#cpn",
      data() {
        return {
          message: "我是子组件"
        }
      },
    }
    const app = new Vue({
      el: "#app",
      data() {
        return {
          message: "我是父组件消息"
        }
      },
      components: {
        cpn
      },
    })
  </script>
</body>
```

#### 具名插槽的使用

- 调用不指定名字的插槽（隐含名字`default`）会替换所有没有名字的插槽
- 当插槽都使用`name`属性设置了名字，需要使用`<template>`的`v-slot:`指定插槽名字

```js
<body>
  <!-- 父组件 -->
  <div id="app">

    <cpn>
      <span>没具名</span>
      <span slot="left">这是左边具名插槽</span>
      <!-- 新语法 -->
      <template v-slot:center>这是中间具名插槽</template>
      <!-- 新语法缩写 -->
      <template #right>这是右边具名插槽</template>
    </cpn>
  </div>
  <!-- 插槽的基本使用使用<slot></slot> -->
  <!-- 子组件 -->
  <template id="cpn">
    <div>
      <slot name="left">左边</slot>
      <slot name="center">中间</slot>
      <slot name="right">右边</slot>
      <slot>没有具名的插槽</slot>
    </div>
  </template>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
  <script>
    const cpn = {
      template: "#cpn",
      data() {
        return {
          message: "我是子组件"
        }
      },
    }
    const app = new Vue({
      el: "#app",
      data() {
        return {
          message: "我是父组件消息"
        }
      },
      components: {
        cpn
      },
    })
  </script>
</body>
```

#### 作用域插槽的使用

> 编译作用域：父组件模板所有的东西都在父级作用域编译；子组件模板的所有东西都会在子级作用域内编译

> 父组件替换插槽的标签，但是内容由子组件来提供

```js
<body>
  <div id="app">
    <cpn></cpn>
    <cpn>
      <template slot-scope="slot">
        <span>{{slot.data.join(' - ')}}</span>
      </template>
    </cpn>
  </div>
  <template id="cpn">
    <slot :data="movies">
      <ul>
        <li v-for="item in movies">{{item}}</li>
      </ul>
    </slot>
  </template>
  <script src="../js/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
      },
      components: {
        cpn: {
          template: '#cpn',
          data() {
            return {
              movies: ['盗梦空间', '阿甘正传', '霸王别姬', '加勒比海盗']
            }
          }
        }
      }
    })
  </script>
</body>
```

### 17.模块化

#### **CommonJS**（了解）

- **nCommonJS的导出：**

![image-20200915220337467](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200915220337467.png)

- #### nCommonJS的导入

- ![image-20200915220352194](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200915220352194.png)

#### ES6的export指令

- **export指令用于导出变量，比如下面的代码：**

![image-20200915220430914](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200915220430914.png)

![image-20200915220440370](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200915220440370.png)

- **ES6的Import指令**

  - **首先，我们需要在HTML代码中引入两个js文件，并且类型需要设置为module**

    ![image-20200915220552668](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200915220552668.png)	

  - **import指令用于导入模块中的内容，比如main.js的代码**

    ![image-20200915220609997](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200915220609997.png)

  - **如果我们希望某个模块中所有的信息都导入，一个个导入显然有些麻烦：**

    **通过*可以导入模块中所有的export变量**

    **但是通常情况下我们需要给*起一个别名，方便后续的使用**

    ![image-20200915220724480](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200915220724480.png)

### 18.webpack配置Vue

**打包项目报错信息**

<img src="C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200917175434893.png" alt="image-20200917175434893" style="zoom:90%; margin-left:0px" />

### 19.**Vue** CLI的使用

#### 1).**安装Vue脚手架**  

 `npm install -g @vue/cli`

**Vue CLI2初始化项目**

`vue init webpack my-project`

**Vue CLI3初始化项目**

`vue create my-project`

#### 2).**runtime-compiler和runtime-only**

##### Vue程序运行过程:

```
template -> ast（抽象语法树） -> render(functions) -> virtual dom -> UI
```

#### 3).区别

- runtime-compiler：

  - 流程：

    `template`->`ast（抽象语法树）`->`render(functions)`->`virtual dom`->`UI`

- runtime-only：

  - 流程：

    `render(functions)`->`virtual dom`->`UI`

  - 特点：

    1. 性能更高
    2. 代码量更少

#### 4).render

```
main.js
import Vue from 'vue'
import App from './App.vue'

new Vue({
  render: h => h(App)
}).$mount('#app')
```

`h`本质上是内部的一个`createElement()`函数

函数用法：

1. 普通用法

   ```
   createElement('标签'，{标签的属性},['标签内容'])
   ```

   标签内容还可以继续加入`createElement()`函数

2. 传入组件对象

   替换之前传入组件通过template渲染的用法👇

   ```
   import Vue from 'vue'
   import App from './App.vue'
   
   new Vue({
     /*
     template: '<App/>'
     components: {
       App
     }
     */
     render: h => h(App)
   }).$mount('#app')
   ```

   而App中的template将由vue-template-compiler解析编译成render，可以直接由render()使用

#### 5).指定路径别名

1. 在根目录创建`vue.config.js`并指定路径和路径别名

   其中`@`默认为`src`目录

   ```
   module.exports = {
     configureWebpack: {
       resolve: {
         alias: {
           'assets': '@/assets',
           'components': '@/components',
           'views': '@/views'
         }
       }
     }
   }
   ```

2. 使用

   - js中：

     直接使用别名即可

     `App.vue`

     ```
     import MainTabBar from "components/MainTabBar";
     ```

   - HTML中

     在别名前加入`~`

     `src/components/MainTabBar.vue`

     ```
     <img slot="item-icon" src="~assets/img/tabbar/home.svg" />
     ```



### 20.vue-router

#### 1).HTML5的history

> 进行URL的修改但不请求数据

- `history.pushState({},'','url')`：将URL加入栈内
- `history.back()`：弹出栈顶
- `history.replaceState({},'','url')`：代替栈顶
- `history.go(负数)`：弹出规定数目的URL

#### 2).vue-router使用

**步骤一: 安装vue-router**

`npm install vue-router --save`

**步骤二: 在模块化工程中使用它(因为是一个插件, 所以可以通过Vue.use()来安装路由功能)**

- 第一步：导入路由对象，并且调用 Vue.use(VueRouter)
- 第二步：创建路由实例，并且传入路由映射配置
- 第三步：在Vue实例中挂载创建的路由实例

**使用vue-router的步骤:**

- 第一步: 创建路由组件
- 第二步: 配置路由映射: 组件和路径映射关系
- 第三步: 使用路由: 通过`<router-link>`和`<router-view>`

#### 3).基本配置

1. 创建组件`Home.vue`和`About.vue`

2. 创建路由组件和配置映射信息

   `router/index.js`

   ```
   //1.1 导入vue
   import Vue from 'vue'
   //1.2 导入路由对象
   import VueRouter from 'vue-router'
   //1.3 导入home组件
   import Home from '../components/Home.vue'
   
   //2.安装路由插件
   Vue.use(VueRouter)
   
   //3.配置路由和组件的映射关系
   const routes = [
     {
       path: '/',
       //重定向到/home
       redirect: '/home'
     },
     {
       path: '/home',
       name: 'Home',
       component: Home
     },
     {
       path: '/about',
       name: 'About',
       //1.4 导入组件并且组件为懒加载，当访问到的时候才加载
       component: () => import('../components/About.vue')
     }
   ]
   
   //4.创建vue-router对象并导入映射
   const router = new VueRouter({
     //使用HTML5的history模式，默认使用hash模式
     mode: 'history',
     base: process.env.BASE_URL,
     routes
   })
   
   //5.导出router对象，在main.js中导入
   export default router
   ```

3. 导入路由组件并挂在到Vue实例上

   `main.js`

   ```
   import Vue from 'vue'
   import App from './App.vue'
   import router from './router'
   //是否显示加载信息
   Vue.config.productionTip = false
   
   new Vue({
     //导入路由组件
     router,
     //渲染App组件到index.html中的.app内
     render: h => h(App)
   }).$mount('#app')
   ```

4. 使用路由，通过`<router-link>`和`<router-view>`

   `App.vue`

   ```
   <template>
     <div id="app">
       <div id="nav">
         <!--路由链接标签-->
         <router-link to="/">Home</router-link>|
         <router-link to="/about">About</router-link>
       </div>
       <!--跳转后组件内容的渲染占位-->
       <router-view />
     </div>
   </template>
   
   <style>
   </style>
   ```

#### 4).router-link

属性：

- `to=""`：设置跳转的url
- `tag=""`：设置渲染的标签，默认为a标签
- `replace`：不能返回上一级
- `active-class=""`：重命名`router-link-active`属性（该属性是只有活跃的路由才会具有）
  - 也可以在`router/index.js`的路由实例属性中设置`linkActiveClass=""`重命名所有路由的属性

#### 5).代码路由跳转

除了使用`<router-link>`进行跳转，也可以在普通标签上设置监听事件在方法中进行跳转

```
<template>
  <div id="app">
    <div id="nav">
      <button @click="userClick">User</button>
    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  methods: {
    profileClick() {
      //获取index.js中的router对象，类似history中的pushState
      this.$router.push('/user')
      //类似history中的replaceState
      //this.$router.replace('/user')
    }
  }
};
</script>
```

#### 6).路由嵌套

1. 创建子路由

   `components/HomeNews.vue`

   ```
   <template>
     <div class="container">
       <ul>
         <li>新闻1</li>
         <li>新闻2</li>
         <li>新闻3</li>
         <li>新闻4</li>
       </ul>
     </div>
   </template>
   
   <script>
   export default {};
   </script>
   
   <style scoped>
   </style>
   ```

   `components/HomeMusic.vue`

   ```
   <template>
     <div class="container">
       <ul>
         <li>音乐1</li>
         <li>音乐2</li>
         <li>音乐3</li>
         <li>音乐4</li>
       </ul>
     </div>
   </template>
   
   <script>
   export default {};
   </script>
   
   <style scoped>
   </style>
   ```

2. 添加嵌套路由

   `router/index.js`

   ```
   import Vue from 'vue'
   import VueRouter from 'vue-router'
   
   const Home = () => import('../components/Home.vue')
   const About = () => import('../components/About.vue')
   const HomeNews = () => import('../components/HomeNews.vue')
   const HomeMusic = () => import('../components/HomeMusic.vue')
   
   Vue.use(VueRouter)
   
   const routes = [
     {
       path: '/',
       redirect: '/home'
     },
     {
       path: '/home',
       name: 'Home',
       component: Home,
       //设置嵌套路由，此时路径不需要'/'
       children: [
         {
           path: 'news',
           component: HomeNews
         },
         {
           path: 'music',
           component: HomeMusic
         }
       ]
     },
     {
       path: '/about',
       name: 'About',
       component: About
     }
   ]
   
   const router = new VueRouter({
     mode: 'history',
     base: process.env.BASE_URL,
     routes
   })
   
   export default router
   ```

3. 在`Home.vue`中设置路由

   `components/Home.vue`

   ```
   <template>
     <div class="container">
       <h2>我是首页</h2>
       <router-link to="/home/news">新闻</router-link>|
       <router-link to="/home/music">音乐</router-link>
       <router-view />
     </div>
   </template>
   
   <script>
   export default {};
   </script>
   
   <style scoped>
   </style>
   ```

#### 7).`$route`和`$router`

- `$router`：是路由js中导出的router对象，可以用来进行路由跳转并传递参数
- `$route`：获取当前活跃状态的路由，可以用来接收参数

#### 8).keep-alive

> 缓存组件状态，不会在跳转的时候一直创建和销毁，避免重复渲染

**使用**

```
<keep-alive>
  <router-view/>
</keep-alive>
```

**activated()和deactivated()在使用keep-alive以后**可以在组件中回调这两个方法

在组件被激活和不激活时调用

**属性**

逗号后不能加空格

- `include="...,..."`：包含组件，使用组件的name属性来指定
- `exclude="...,..."`：排除组件，使用组件的name属性来指定

### 21.vuex 

官方解释：Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。

它采用 集中式存储管理 应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

Vuex 也集成到 Vue 的官方调试工具 [devtools](https://github.com/vuejs/vue-devtools)[ extension](https://github.com/vuejs/vue-devtools)，提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。

![image-20200925212527970](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200925212527970.png)

- State：不用多说，就是我们的状态。（你姑且可以当做就是data中的属性）
- View：视图层，可以针对State的变化，显示不同的信息。（这个好理解吧？）
- Actions：这里的Actions主要是用户的各种操作：点击、输入等等，会导致状态的改变。

**一起在来看一副官方给出的图片**

![image-20200925212700677](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200925212700677.png)

首先，我们需要在某个地方存放我们的Vuex代码：

这里，我们先创建一个文件夹store，并且在其中创建一个index.js文件

在index.js文件中写入如下代码

![image-20200925213055816](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200925213055816.png)

我们来对使用步骤，做一个简单的小节：

1. 提取出一个公共的store对象，用于保存在多个组件中共享的状态
2. 将store对象放置在new Vue对象中，这样可以保证在所有的组件中都可以使用到
3. 在其他组件中使用store对象中保存的状态即可

**通过this.$store.state.属性的方式来访问状态**

**通过this.$store.commit('mutation中方法')来修改状态**

注意事项：

- 我们通过提交mutation的方式，而非直接改变store.state.count。
- 这是因为Vuex可以更明确的追踪状态的变化，所以不要直接改变store.state.count的值。

#### **Vuex**核心概念

- `state`：放置状态信息
  - 单一状态树：一个应用只有一个store
- `Getters`：类似于计算属性，会进行缓存
- `Mutation`：实现状态更新
- `Action`：代替Mutation进行异步操作
- `Module`：将store中的state进行分模块管理

##### **1.State单一状态树**

英文名称是Single Source of Truth，也可以翻译成单一数据源

##### 2.Getters基本使用

![image-20200925213613699](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200925213613699.png)

![image-20200925213639411](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20200925213639411.png)

​	3**.我们强调, 不要再Mutation中进行异步操作.Action类似于Mutation, 但是是用来代替Mutation进行异步操作的.**

​	4.当应用变得非常复杂时,store对象就有可能变得相当臃肿.为了解决这个问题, Vuex允许我们将store分割成模块(Module), 而每个模块拥有自己的state、mutations、actions、getters等



### 22.项目目录结构

```
src
├─assets---静态文件
│  ├─css---css文件
│  └─img---图片文件
├─common---公共js文件
├─components---组件
│  ├─common---在任何项目中都可以使用的公共组件
│  └─content---在当前项目中使用的组件
├─network---网络请求相关封装
├─router---路由
├─store---vuex
```