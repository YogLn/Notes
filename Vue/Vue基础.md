# 一.NPM环境搭建

1. 安装Node.js

2. [改变npm全局安装位置](https://segmentfault.com/a/1190000014919030)

3. 使用nrm更改npm使用的镜像

   `npm install nrm -g`---全局安装nrm

   `nrm ls`---查看支持的镜像源

   `nrm use taobao`---使用淘宝镜像源

4. 项目初始化

   `npm init -y`

5. 在当前目录下的生产环境安装Vue

   `npm install vue`

# 二.MVVM模型

> 实现了View和Model之间的双向数据交互

例子：

```
<body>
    <div id="app">
        在这里输入的内容会显示在下面：
        <input type="text" v-model="msg"/>
        <h3>{{ msg }}</h3>
    </div>
    <script>
        const vm = new Vue({
            el: '#app',
            data: {
                msg: ''
            }
        })
    </script>
</body>
</html>
```

# 三.Vue实例

```
new Vue({
            el: '#app',
            data: {},
    		methods: {}
        });
```

- el：要操作的页面元素，必填参数
- data：数据，也可以理解为属性
- methods：方法

## data

当Vue实例被创建时，会获取data中的数据，并在视图中渲染并监视data的变化，当data变化时重新渲染视图

## methods

可以相应视图中的事件，并修改data的数据

> 属性：保存数据
>
> 方法：修改数据

## computed

> 特点：
>
> - 计算属性相对于methods会有**缓存**，多次调用时，方法只会执行一次，只有使用的变量改变的时候才会重新执行方法
> - 计算属性一定要有返回值
>
> 本质：一个有get和set方法的属性
>
> 调用方法：在computed定义函数，接着在html中可以用`{{属性名}}`的方式调用，不需要像方法一样加括号

```
<body>
  <div id="app">
    <button @click="a++">Add to A</button>
    <p>Age+A = {{addToA}}</p>
  </div>
  <script>
    new Vue({
      el: "#app",
      data: {
        a: 0,
        age: 18
      },
      computed: {
        addToA() {
          return this.age + this.a;
        }
      }
    })
  </script>
</body>
```

## watch

> 监听属性的变化

下面这个例子就可以监听name属性的上一个value和改变的value值

```
<body>
  <div id="app">
    <input v-model="name" />
    <span>{{name}}</span>
  </div>
  <script>
    new Vue({
      el: "#app",
      data: {
        name: ''
      },
      watch: {
        name(val, oldVal) {
          console.log(val + ' ' + oldVal);
        }
      }
    })
  </script>
</body>
```

# 四.插值语法

- `Mustache`：`{{}}`将data和标签content相互绑定

  - 过滤器

    ```
    <!--v-for="book in books"-->
    <td>{{book.price|showPrice}}</td>
    <script>
      //定义books对象里面有price属性
      filters: {
        //过滤器，过滤为指定类型
        showPrice(price) {
          return '￥' + price.toFixed(2);
        }
      }
    </script>
    ```

- `v-once`：只将值相应一次

- `v-html=""`：将值解析成html

- `v-text=""`：类似于Mustache，不灵活，不常用

- `v-pre`：将语句不解析，直接显示

- `v-cloak` ：在Vue解析指令之前会有这个属性，解析完成会将这个属性删除

  接着可以在style中自定义这个指令的css样式，如在Vue解析完成才显示内容标签内容：

  ```
  [v-clock]{
      display = none;
  }
  ```

# 五.模板语法

> 模板语法用于视图的编写

- 属性绑定：`v-bind`，简写为`:`
- 事件绑定：`v-on`，简写为`@`
- 双向绑定：`v-model`
- 条件绑定：`v-if`
- 列表渲染：`v-for`

## 1.属性绑定和双向绑定

### 属性绑定

> 属性绑定`(v-bind)`：将非表单标签的一个属性和data中的一个变量绑定

- 基本用法：

  ```
  <!--imgUrl为Vue的data内的变量-->
  <img v-bind:src="imgUrl">
  <img :src="imgUrl">
  ```

- 对象语法（也可以将`{active: isActive,line: isLine}`放到计算属性中去进行）：

  ```
  <!--v-bind:class="{类名: boolean}"-->
  <!--在Vue的data中定义isActice和isLine控制true和false来指定对应的class是否生效-->
  <!--普通class会和Vue的class合并-->
  <h2 v-bind:class="{active: isActive,line: isLine}"></h2>
  
  <!--在属性绑定中可以指定方法-->
  <h2 :class="getClasses()">
  ```

- 样式语法：

  ```
  <!--value有单引号是字符串，没有单引号是Vue的变量名-->
  <h2 :bind="{key(css属性名):value(css属性值)}">
  ```

### 双向绑定

> 双向绑定`(v-model)`：将表单标签的value值和data中的变量双向绑定

- 原理：

  背后包含两个指令的操作：

  1. `v-bind`绑定一个value属性
  2. `v-on`指令给当前元素绑定一个`input`事件

- 结合`radio`

  ```
  <input type="radio" id="male" name="sex" value="男" v-model="sex"/>
  <input type="radio" id="female" name="sex" value="女" v-model="sex"/>
  <!--Vue中的sex保存value属性-->
  ```

  当`v-model`绑定同一个变量时，`name`属性可以省略

- 结合`checkbox`

  - 单选框

    ```
    <label for="protocol">
        <input type="checkbox" id="protocol" v-model="protocol">同意
    </label> 
    <!--Vue中的protocol变量是布尔类型-->
    ```

  - 多选框

    ```
    <input type="checkbox" value="篮球" v-model="hobbies">篮球
    <input type="checkbox" value="足球" v-model="hobbies">足球
    <input type="checkbox" value="乒乓球" v-model="hobbies">乒乓球
    <input type="checkbox" value="羽毛球" v-model="hobbies">羽毛球
    <!--Vue中的hobbies为数组类型，会将value的值加入数组-->
    ```

  - 值绑定

    ```
    <body>
      <div id="app">
        <label v-for="item in originHobbies" :for="item">
          <input type="checkbox" :value="item" :id="item" v-model="hobbies" />{{item}}
        </label></br>
        {{hobbies}}
      </div>
    
      <script>
        new Vue({
          el: "#app",
          data: {
            originHobbies: ["篮球", "羽毛球", "足球"],
            hobbies: []
          }
        })
      </script>
    </body>
    ```

- 结合`select`

  - 选择一个

    ```
    <select name="fruit" v-model="fruit">
        <option value="苹果">苹果</option>
        <option value="香蕉">香蕉</option>
        <option value="榴莲">榴莲</option>
        <option value="葡萄">葡萄</option>
    </select>
    <!--Vue中用fruit保存value-->
    ```

  - 选择多个

    ```
    <select name="fruit" v-model="fruits" multiple>
        <option value="苹果">苹果</option>
        <option value="香蕉">香蕉</option>
        <option value="榴莲">榴莲</option>
        <option value="葡萄">葡萄</option>
    </select>
    <!--Vue中用fruits数组保存value-->
    ```

- 修饰符

  - `.lazy`：让数据失去焦点或者回车时才更新

  - `.number`：默认`v-model`给变量赋值为String类型，这个修饰符可以赋值为number类型

    ```
    <input type="number" v-model.number="age"/>
    ```

  - `.trim`：去除首尾空格

## 2.条件渲染

> 当满足条件时渲染页面

- `v-if`和`v-show`的区别：
  - 当满足条件时，两个元素都显示
  - 当不满足条件时，两个元素都不显示，区别：
    - `v-show`创建元素，但是`display=none`
    - `v-if`不会创建元素
- `v-if`指令链：
  - `v-if=""`、`v-else-if=""`和`v-else`：三个标签配合使用，只会执行一个

```
<body>
    <div id="app">
        <button @click="flag=!flag">切换</button>
        <div v-if="flag">你好</div>
    </div>
    <script>
        const vm = new Vue({
            el: '#app',
            data: {
                flag: true
            }
        })
    </script>
</body>

</html>
```

## 3.列表渲染

> 遍历数组、对象等

- 遍历li：

  ```
  <body>
    <div id="app">
      <ul>
        <li v-for="(person,index) in people">
          <!-- 下标.姓名.年龄 -->
          {{index}}.{{person.name}}.{{person.age}}
        </li>
      </ul>
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
          people: [
            { name: '小明', age: 17 },
            { name: '小红', age: 18 },
            { name: '小亮', age: 19 },
            { name: '小飞', age: 15 }
          ]
        }
      })
    </script>
  </body>
  ```

- 容器标签

  容器标签不会被渲染到页面中

  ```
  <body>
    <div id="app">
      <template v-for="(person,index) in people">
        <p>{{index}}.{{person.name}}.{{person.age}}</p>
      </template>
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
          people: [
            { name: '小明', age: 17 },
            { name: '小红', age: 18 },
            { name: '小亮', age: 19 },
            { name: '小飞', age: 15 }
          ]
        }
      })
    </script>
  </body>
  ```

- 遍历对象

  ```
  <body>
    <div id="app">
      <template v-for="people in people">
        <div v-for="(val,key) in people">
          <p>{{key}}--{{val}}</p>
        </div>
      </template>
      <script>
        new Vue({
          el: "#app",
          data: {
            people: [
              { name: '小明', age: 17 },
              { name: '小红', age: 18 },
              { name: '小亮', age: 19 },
              { name: '小飞', age: 15 }
            ]
          }
        })
      </script>
  </body>
  ```

- 在CLI中遍历需要指定key值，为了保证性能一般用对象本身

  ```
  <body>
    <div id="app">
      <ul>
        <li v-for="(person,index) in people" : key="index">
          <!-- 下标.姓名.年龄 -->
          {{index}}.{{person.name}}.{{person.age}}
        </li>
      </ul>
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
          people: [
            { name: '小明', age: 17 },
            { name: '小红', age: 18 },
            { name: '小亮', age: 19 },
            { name: '小飞', age: 15 }
          ]
        }
      })
    </script>
  </body>
  ```

## 4.事件绑定

### 事件

- `@click`：监听鼠标点击
- `@keyup`：监听键盘按键
- `@input`：监听`<input>`的输入

### 传参

1. 事件函数有参数，但是调用时没有传入参数，Vue会自动在第一个参数位置传入MouseEvent对象

2. 当传入多个参数需要MouseEvent对象时，在调用传参位置传入`$event`即可传入MouseEvent对象

   其他参数不加`''`就当变量传入

### 修饰符

- `.stop`：阻止冒泡
- `.prevent`：阻止默认事件
- `.enter`：只在回车时监听
- `.once`：只触发一次回调

# 六.数组响应式方法

> 响应式方法就是当data的值改变时，界面的值也会跟着改变

响应式方法：

- `push()`

- `shift()`

- `unshift()`

- ```
  splice()
  ```

  ：

  - 删除元素：
    - 参数1：开始位置
    - 参数2：删除个数
  - 替换元素：
    - 参数1：开始位置
    - 参数2：替换个数
    - 参数3：可变参数，表示替换的元素
  - 插入元素
    - 参数1：插入开始位置
    - 参数2：0
    - 参数3：可变参数，表示插入的元素

- `sort()`

- `reverse()`

不响应方法：

- 使用数组索引值修改
  - 解决方法：
    - 使用`splice()`
    - 使用`Vue.set(对象,索引,替换元素)`

# 七.组件

> 实现代码的复用和逻辑化

## 1.注册组件基本步骤

1. 创建组件构造器
2. 注册组件
3. 使用组件

```
<body>
  <div id="app">
    <!-- 3.使用组件 -->
    <my-cpn></my-cpn>
  </div>
  <script>
    //1.创建组件构造器对象
    const cpnC = Vue.extend({
      template: `
      <div>
        <h2>你好</h2>
      </div>`
    });
    //2.注册全局组件，参数：标签名、组件构造器
    Vue.component("my-cpn", cpnC);
    new Vue({
      el: "#app"
    })
  </script>
</body>
```

## 2.全局组件和局部组件

> 全局组件：可以在多个Vue实例下使用

上面的例子就是全局组件

> 局部组件：只能在定义的Vue实例下使用

```
<body>
  <div id="app">
    <!-- 3.使用组件 -->
    <cpn></cpn>
  </div>
  <script>
    //1.创建组件构造器对象
    const cpnC = Vue.extend({
      template: `
      <div>
        <h2>你好</h2>
      </div>`
    });
    //2.注册局部组件
    new Vue({
      el: "#app",
      components: {
        //标签名: 构造器
        cpn: cpnC
      }
    })
  </script>
</body>
```

## 3.父组件和子组件

```
<body>
  <div id="app">
    <!-- 3.使用组件 -->
    <cpn2></cpn2>
  </div>
  <script>
    //1.1 创建子组件构造器对象
    const cpnC1 = Vue.extend({
      template: `
      <div>
        <h2>第一个组件</h2>
      </div>`
    });
    //1.2 创建父组件构造器对象
    const cpnC2 = Vue.extend({
      template: `
      <div>
        <h2>第二个组件</h2>
        <cpn1/>
      </div>`,
      components: {
        cpn1: cpnC1
      }
    });
    //2.注册局部组件
    new Vue({
      el: "#app",
      components: {
        cpn2: cpnC2
      }
    })
  </script>
</body>
```

### 3.1 父子组件的通信

- 父组件通过`props`向子组件传递数据

  props的值两种方式：

  - 字符串数组

    ```
    <body>
      <div id="app">
        <!-- 给子组件对应属性传父组件属性 -->
        <cpn :cmovies="movies"></cpn>
      </div>
    
      <!-- 子组件模板 -->
      <template id="cpn">
        <div>{{cmovies}}</div>
      </template>
    
      <script>
        new Vue({
          el: "#app",
          data: {
            movies: ['你的名字', '言叶之庭']
          },
          components: {
            cpn: {
              template: "#cpn",
              props: ['cmovies']
            }
          }
        });
      </script>
    </body>
    ```

  - **对象**，可以设置传递类型也可以设置默认值等

    默认值在父组件没有给子组件传参的时候显示

    ```
    <body>
      <div id="app">
        <!-- 给子组件对应属性传父组件属性 -->
        <cpn :cmovies="movies" :cmsg="msg"></cpn>
      </div>
    
      <!-- 子组件模板 -->
      <template id="cpn">
        <div>
          {{cmsg}}<br />
          {{cmovies}}
        </div>
      </template>
    
      <script>
        new Vue({
          el: "#app",
          data: {
            msg: "电影：",
            movies: ['你的名字', '言叶之庭']
          },
          components: {
            cpn: {
              template: "#cpn",
              props: {
                //只指定数组类型：cmovies: Array
                //指定类型和默认值（需要为函数）
                cmovies: {
                  type: Array,
                  default() {
                    return [];
                  }
                },
                //指定类型和默认值
                cmsg: {
                  type: String,
                  default: '电影'
                }
              }
            }
          }
        });
      </script>
    </body>
    ```

  `v-bind`绑定的属性不支持驼峰，如果是驼峰，应该这么写`:c-info`

- 子组件通过**自定义事件($emit)**向父组件传递数据

  ```
  <body>
    <div id="app">
      <!-- 监听子组件item-click事件，并在父组件cpnClick方法中处理 -->
      <cpn @item-click="cpnClick"></cpn>
    </div>
  
    <!-- 子组件模板 -->
    <template id="cpn">
      <div>
        <!--定义点击触发的方法-->
        <button v-for="item in categories" @click="btnClick(item)">
          {{item.name}}
        </button>
      </div>
    </template>
  
    <script>
      //子组件
      const cpn = {
        template: "#cpn",
        data() {
          return {
            categories: [
              { id: 1, name: '热门推荐' },
              { id: 2, name: '手机数码' },
              { id: 3, name: '电脑办公' }
            ]
          }
        },
        methods: {
          //通过方法中的事件发射将信息发送给父组件
          btnClick(item) {
            //发射自定义事件：事件名，传递参数
            this.$emit('item-click', item);
          }
        }
      }
      new Vue({
        el: "#app",
        data: {
          msg: "电影：",
          movies: ['你的名字', '言叶之庭']
        },
        components: {
          cpn
        },
        methods: {
          //处理子组件发射的事件
          cpnClick(item) {
            console.log(item);
          }
        }
      });
    </script>
  </body>
  ```

### 3.2 父子组件的访问

- 父访问子

  - `$children`

    数组类型，通过下标值去获取子组件

    ```
    <body>
      <div id="app">
        <cpn></cpn>
        <cpn></cpn>
        <button @click="btnClick">按钮</button>
      </div>
    
      <!-- 子组件模板 -->
      <template id="cpn">
        <div>我是子组件</div>
      </template>
    
      <script>
        //子组件
        const cpn = {
          template: "#cpn",
          methods: {
            showMessage() {
              console.log('showMessage')
            }
          }
        }
        new Vue({
          el: "#app",
          methods: {
            btnClick() {
              //获取第一个子组件并调用他的方法
              this.$children[0].showMessage();
            }
          },
          components: {
            cpn
          },
        });
      </script>
    </body>
    ```

  - `$refs`

    对象类型，通过`ref`指定的值去获取子组件

    ```
    <body>
      <div id="app">
        <!-- 通过ref指定子组件的名字 -->
        <cpn ref="cpn1"></cpn>
        <cpn ref="cpn2"></cpn>
        <button @click="btnClick">按钮</button>
      </div>
    
      <!-- 子组件模板 -->
      <template id="cpn">
        <div>我是子组件</div>
      </template>
    
      <script>
        //子组件
        const cpn = {
          template: "#cpn",
          methods: {
            showMessage() {
              console.log('showMessage')
            }
          }
        }
        new Vue({
          el: "#app",
          methods: {
            btnClick() {
              //通过子组件ref名称调用子组件
              this.$refs.cpn1.showMessage();
            }
          },
          components: {
            cpn
          },
        });
      </script>
    </body>
    ```

- 子访问父(`$parent`)

  **不建议使用**

  ```
  <body>
    <div id="app">
      <cpn></cpn>
    </div>
  
    <!-- 子组件模板 -->
    <template id="cpn">
      <div>
        我是子组件<br />
        <!-- 点击获取父组件属性 -->
        <button @click="btnClick">按钮</button>
      </div>
    </template>
  
    <script>
      //子组件
      const cpn = {
        template: "#cpn",
        methods: {
          btnClick() {
            //访问父组件
            console.log(this.$parent.msg);
          }
        }
      }
      new Vue({
        el: "#app",
        data: {
          msg: "父组件"
        },
        components: {
          cpn
        },
      });
    </script>
  </body>
  ```

- 子访问根组件(`$root`)

## 4.组件注册语法糖

> 省略了`Vue.extend`步骤

- 全局组件

  ```
  <body>
    <div id="app">
      <!-- 2.使用组件 -->
      <cpn1></cpn1>
    </div>
    <script>
      //1 创建并注册全局组件
      Vue.component('cpn1', {
        template: `
        <div>
          <h2>第一个组件</h2>
        </div>`
      });
      new Vue({
        el: "#app",
      })
    </script>
  </body>
  ```

- 局部组件

  ```
  <body>
    <div id="app">
      <!-- 2.使用组件 -->
      <cpn1></cpn1>
    </div>
    <script>
      //1 创建并注册局部组件
      new Vue({
        el: "#app",
        components: {
          cpn1: {
            template: `
        <div>
          <h2>第一个组件</h2>
        </div>`
          }
        }
      })
    </script>
  </body>
  ```

### 抽取模板

- 方法一

  ```
  <body>
    <div id="app">
      <!-- 3.使用组件 -->
      <cpn1></cpn1>
    </div>
  
    <!-- 1.抽取template -->
    <script type="text/x-template" id="cpn">
      <div>
        <h2>第一个组件</h2>
      </div>
    </script>
  
    <script>
      //2.创建并注册局部组件
      new Vue({
        el: "#app",
        components: {
          cpn1: {
            template: "#cpn"
          }
        }
      })
    </script>
  </body>
  ```

- **方法二**

  ```
  <body>
    <div id="app">
      <!-- 3.使用组件 -->
      <cpn1></cpn1>
    </div>
  
    <!-- 1.抽取template -->
    <template id="cpn">
      <div>
        <h2>第一个组件</h2>
      </div>
    </template>
  
    <script>
      //2.创建并注册局部组件
      new Vue({
        el: "#app",
        components: {
          cpn1: {
            template: "#cpn"
          }
        }
      })
    </script>
  </body>
  ```

## 5.组件的data

> 组件不能访问Vue实例数据，需要定义自己的data属性

组件的data必须是函数，不然实例间相同的组件会相互影响，如果想要实例属性相互影响需要定义全局变量并在data中返回

```
<body>
  <div id="app">
    <!-- 3.使用组件 -->
    <cpn1></cpn1>
  </div>

  <!-- 1.抽取template -->
  <template id="cpn">
    <div>
      <h2>{{msg}}</h2>
    </div>
  </template>

  <script>
    //2.创建并注册全局组件
    Vue.component("cpn1", {
      template: "#cpn",
      data() {
        return {
          msg: "第一个组件"
        }
      }
    });
    new Vue({
      el: "#app"
    });
  </script>
</body>
```

## 6.slot插槽

> 在组件上预留空间，使组件具有扩展性

### 6.1 基本使用

```
<body>
  <div id="app">
    <!-- 在插槽中创建按钮 -->
    <cpn><button>按钮</button></cpn>
    <!-- 创建默认插槽 -->
    <cpn></cpn>
  </div>

  <template id="cpn">
    <div>
      <h2>我是组件</h2>
      <!-- 创建插槽，并指定插槽的默认值 -->
      <slot><button>默认值</button></slot>
    </div>
  </template>

  <script>
    new Vue({
      el: "#app",
      components: {
        cpn: {
          template: '#cpn'
        }
      }
    });
  </script>
</body>
```

### 6.2 具名插槽

- 调用不指定名字的插槽（隐含名字`default`）会替换所有没有名字的插槽
- 当插槽都使用`name`属性设置了名字，需要使用`<template>`的`v-slot:`指定插槽名字

```
<body>
  <div id="app">
    <!-- 通过template的v-solt:替换插槽1 -->
    <cpn>
      <template v-slot:one>
        <button>你好</button>
      </template>
    </cpn>
  </div>

  <template id="cpn">
    <div>
      <h2>我是组件</h2>
      <!-- 插槽1，设置默认值 -->
      <slot name="one"><span>插槽1</span></slot>
      <!-- 插槽2，设置默认值 -->
      <slot name="two"><span>插槽2</span></slot>
      <!-- 插槽3，设置默认值 -->
      <slot name="three"><span>插槽3</span></slot>
    </div>
  </template>

  <script>
    new Vue({
      el: "#app",
      components: {
        cpn: {
          template: '#cpn'
        }
      }
    });
  </script>
</body>
```

### 6.3 作用域插槽

> 编译作用域：父组件模板所有的东西都在父级作用域编译；子组件模板的所有东西都会在子级作用域内编译

> 父组件替换插槽的标签，但是内容由子组件来提供

```
<body>
  <div id="app">
    <!-- 调用子组件的对象复写插槽 -->
    <cpn>
      <!-- 通过template的v-slot引用名字为test的插槽对象(mySlot为任意名字) -->
      <template v-slot:test="mySlot">
        <span>{{mySlot.data.join(' - ')}}</span>
      </template>
    </cpn>
  </div>

  <template id="cpn">
    <div>
      <!-- 将子组件的内容给data标签(标签名可以任意) -->
      <slot :data="pLanguages" name="test">
        <ul>
          <li v-for="item in pLanguages">{{item}}</li>
        </ul>
      </slot>
    </div>
  </template>

  <script>
    new Vue({
      el: "#app",
      components: {
        cpn: {
          template: '#cpn',
          data() {
            return {
              pLanguages: ['Javascript', 'Java', 'C++']
            }
          }
        }
      }
    });
  </script>
</body>
```

# 八.数据请求

## FETCH

- Get

  ```
  <body>
    <div id="app">
      <ul>
        <li v-for="todo in todos">
          {{todo.title}}.{{todo.completed}}
        </li>
      </ul>
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
          todos: []
        },
        mounted() {
          //fetch Get请求
          const todos = fetch("https://jsonplaceholder.typicode.com/todos").then(res => {
            return res.json();
          }).then(todos => {
            this.todos = todos;
          });
        }
      });
    </script>
  </body>
  ```

- POST

  ```
  <body>
    <div id="app">
      <form @submit.prevent="onSubmit">
        <input type="text" v-model="todo.title">
        <input type="checkbox" v-model="todo.completed">
        <input type="submit" value="提交">
      </form>
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
          todo: {
            title: "",
            completed: false
          }
        },
        methods: {
          onSubmit() {
            //注意结尾的分号，否则无法获取
            fetch("http://jsonplaceholder.typicode.com/todos", {
              method: "POST",
              body: JSON.stringify(this.todo),
              headers: {
                "Content-Type": "application/json"
              }
            }).then(res => {
              return res.json();
            }).then(todo => {
              console.log(todo);
            });
          }
        }
      });
    </script>
  </body>
  ```

## Axios

- 安装

  ```
  npm install axios
  ```

- Get

  ```
  <body>
    <div id="app">
      <ul>
        <li v-for="todo in todos">
          {{todo.title}}.{{todo.completed}}
        </li>
      </ul>
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
          todos: []
        },
        mounted() {
          axios.get("https://jsonplaceholder.typicode.com/todos").then(res => {
            this.todos = res.data;
          });
        }
      });
    </script>
  </body>
  ```

- Post

  ```
  <body>
    <div id="app">
      <form @submit.prevent="onSubmit">
        <input type="text" v-model="todo.title">
        <input type="checkbox" v-model="todo.completed">
        <input type="submit" value="提交">
      </form>
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
          todo: {
            title: "",
            completed: false
          }
        },
        methods: {
          onSubmit() {
            axios.post("http://jsonplaceholder.typicode.com/todos",
              this.todo).then(res => {
                console.log(res.data);
              });
          }
        }
      });
    </script>
  </body>
  ```