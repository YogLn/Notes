# 一.简介

> 快速搭建Vue开发环境以及对应的webpack配置

# 二.环境搭建

1. 安装脚手架

   ```
   npm install -g @vue/cli
   ```

2. 创建脚手架项目

   ```
   vue create 项目名
   ```

   自定义配置：

   - default：默认配置

     Manually：手动配置

   - pwa：先进webapp

     css：预处理器（less等需要）

     unit：单元测试

     e2e：端到端测试

   - **单独配置文件**和放在package.json配置文件的选择

   - 保存自己的配置

   - npm和yarm的选择

3. 进入项目文件夹以后，启动项目

   ```
   npm run serve
   ```

4. 如果需要修改webpack配置文件，创建一个`vue.config.js`文件进行修改

------

当从`git`上下载的项目搭建的时候，不会有`node_modules`文件夹，需要通过以下命令下载模块

```
npm install
```

# 三.runtime-compiler和runtime-only

## Vue程序运行过程

```
template`->`ast（抽象语法树）`->`render(functions)`->`virtual dom`->`UI
```

## 区别

- runtime-compiler：

  - 流程：

    `template`->`ast（抽象语法树）`->`render(functions)`->`virtual dom`->`UI`

- runtime-only：

  - 流程：

    `render(functions)`->`virtual dom`->`UI`

  - 特点：

    1. 性能更高
    2. 代码量更少

## render

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

# 四.指定路径别名

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

# 五.vue-router

> 组件和URL的映射关系

## 1.HTML5的history

> 进行URL的修改但不请求数据

- `history.pushState({},'','url')`：将URL加入栈内
- `history.back()`：弹出栈顶
- `history.replaceState({},'','url')`：代替栈顶
- `history.go(负数)`：弹出规定数目的URL

## 2.的基本配置

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

## 3.router-link

属性：

- `to=""`：设置跳转的url
- `tag=""`：设置渲染的标签，默认为a标签
- `replace`：不能返回上一级
- `active-class=""`：重命名`router-link-active`属性（该属性是只有活跃的路由才会具有）
  - 也可以在`router/index.js`的路由实例属性中设置`linkActiveClass=""`重命名所有路由的属性

## 4.代码路由跳转

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

## [5.动态路由](https://github.com/MrWater233/CodeNotes/blob/master/FrontEnd/Vue/router)

1. `router/index.js`

   设置映射关系

   ```
   const User = () => import('../components/User.vue')
   
   const routes = [
     ...
     {
       path: '/user/:userId',
       component: User
     }
   ]
   ```

2. `App.vue`

   ```
   <template>
     <div id="app">
       <div id="nav">
         <!--拼接url-->
         <router-link :to="'/user/'+userId">Home</router-link>|
       </div>
       <router-view />
     </div>
   </template>
   
   <script>
   export default {
     data() {
       return {
         userId: 'lisi'
       };
     }
   };
   </script>
   ```

3. 获取拼接uri的内容

   `components/User.vue`

   ```
   <template>
     <div class="container">
       <h2>我是用户界面</h2>
       <h2>{{userId}}</h2>
     </div>
   </template>
   
   <script>
   export default {
     computed: {
       userId() {
         //获取处于活跃的路由,，并获取路由配置url中的userId
         return this.$route.params.userId
       }
     }
   };
   </script>
   
   <style scoped>
   </style>
   ```

## 6.路由懒加载

> - 如果不进行路由懒加载，所有的业务组件将被全部打包到app.xxx.js中
> - 懒加载之后，不同的路由组件打包以后将分成不同的js文件，当路由被访问才加载对应组件，提高了用户第一次访问的效率

```
router/index.js
//设置路由懒加载
const About = () => import('../components/About.vue')

const routes = [
  ...
  {
    path: '/about',
    name: 'About',
    component: About
  }
]
```

[![img](https://raw.githubusercontent.com/MrWater233/PictureHost/master/20200327224311.png)](https://raw.githubusercontent.com/MrWater233/PictureHost/master/20200327224311.png)

## 7.路由嵌套

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

## 8.传递参数

1. params

2. query

   1. `components/Profile.vue`

      ```
      <template>
        <div class="container">
          <h2>Profile组件</h2>
          <!-- 取出URL中的query信息 -->
          <h2>{{$route.query}}</h2>
          <h2>{{$route.query.name}}</h2>
          <h2>{{$route.query.age}}</h2>
        </div>
      </template>
      ```

   2. `router/index.js`

      ```
      const Profile = () => import('../components/Profile.vue')
      
      const routes = [
        {
          path: '/profile',
          component: Profile
        }
      ]
      ```

   3. `App.vue`

      1. router-link进行跳转

         ```
         <template>
           <div id="app">
             <div id="nav">
               <!-- 设置跳转地址和query信息 -->
               <router-link :to="{path: '/profile',query:{name:'wjm',age:'20'}}">Profile</router-link>
             </div>
             <router-view />
           </div>
         </template>
         ```

      2. 代码进行跳转

         ```
         <template>
           <div id="app">
             <div id="nav">
               <button @click="profileClick">Profile</button>
             </div>
             <router-view />
           </div>
         </template>
         
         <script>
         export default {
           methods: {
             profileClick() {
               this.$router.push({
                 path: "/profile",
                 query: {
                   name: "wjm",
                   age: 18
                 }
               });
             }
           }
         };
         </script>
         ```

      跳转的URL：

      [![img](https://raw.githubusercontent.com/MrWater233/PictureHost/master/20200327234609.png)](https://raw.githubusercontent.com/MrWater233/PictureHost/master/20200327234609.png)

## 9.`$route`和`$router`

- `$router`：是路由js中导出的router对象，可以用来进行路由跳转并传递参数
- `$route`：获取当前活跃状态的路由，可以用来接收参数

## 10.导航守卫

> 在路由跳转的时候进行的监听

案例：

实现路由跳转时改变title

```
router/index.js
...
const routes = [
  {
    path: '/',
    redirect: '/home',
  },
  {
    path: '/home',
    name: 'Home',
    component: Home,
    children: [
      {
        path: 'news',
        component: HomeNews
      },
      {
        path: 'music',
        component: HomeMusic
      }
    ],
    meta: {
      title: '首页'
    }
  },
  {
    path: '/about',
    name: 'About',
    component: About,
    meta: {
      title: '关于'
    }
  },
  {
    path: '/profile',
    name: 'Profile',
    component: Profile,
    meta: {
      title: 'Profile'
    }
  }
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

//前置钩子，除了前置钩子还有后置钩子router.afterEach((to,from)=>())
//to：即将进入的目标的路由对象
//from：当前导航将要离开的路由对象
//next：调用该方法后才能进入下一个钩子
router.beforeEach((to, from, next) => {
  //获取目的路由最外层嵌套的meta中的title属性，并设置为title
  document.title = to.matched[0].meta.title
  next();
})
...
```

## 11.keep-alive

> 缓存组件状态，不会在跳转的时候一直创建和销毁，避免重复渲染

### 使用

```
<keep-alive>
  <router-view />
</keep-alive>
```

### activated()和deactivated()

**在使用keep-alive以后**可以在组件中回调这两个方法

在组件被激活和不激活时调用

### 属性

逗号后不能加空格

- `include="...,..."`：包含组件，使用组件的name属性来指定
- `exclude="...,..."`：排除组件，使用组件的name属性来指定

# 六.Vuex

> 组件间共享状态的管理工具
>
> 路由跳转不会重置状态，页面刷新会将状态重置

[![images](https://camo.githubusercontent.com/f4571e3be6d6661522cc57debd05970628cee5ba7a251e1c9bf7a9f852a00361/68747470733a2f2f767565782e7675656a732e6f72672f767565782e706e67)](https://camo.githubusercontent.com/f4571e3be6d6661522cc57debd05970628cee5ba7a251e1c9bf7a9f852a00361/68747470733a2f2f767565782e7675656a732e6f72672f767565782e706e67)

## 1.基本使用

1. 配置Vuex.store

   `src/store/index.js`

   ```
   import Vue from 'vue'
   import Vuex from 'vuex'
   
   //1.安装插件
   Vue.use(Vuex)
   
   //2.创建并导出Vuex中的store
   export default new Vuex.Store({
     //保存需要保存的状态
     state: {
       counter: 1000
     },
     //能够对state进行修改并通过开发工具进行跟踪
     mutations: {
       //默认参数state
       increment(state) {
         state.counter++;
       },
       decrement(state) {
         state.counter--;
       }
     },
     actions: {
     },
     modules: {
     },
     getters: {
   
     }
   })
   ```

2. 导入Vuex.store并进行挂载

   `src/main.js`

   ```
   import Vue from 'vue'
   import App from './App.vue'
   import router from './router'
   import store from './store'
   
   Vue.config.productionTip = false
   
   new Vue({
     router,
     //导入Vuex
     store,
     render: h => h(App)
   }).$mount('#app')
   ```

3. 创建组件并使用公共状态counter

   `src/components/HelloVuex.vue`

   ```
   <template>
     <div class="container">
       <h2>{{$store.state.counter}}</h2>
     </div>
   </template>
   
   <script>
   export default {};
   </script>
   ```

   `src/App.vue`

   ```
   <template>
     <div id="app">
       <!--显示vuex的state中的信息-->
       <h2>{{$store.state.counter}}</h2>
       <button @click="addition">+</button>
       <button @click="subtraction">-</button>
       <hr />
       <hello-vuex></hello-vuex>
     </div>
   </template>
   
   <script>
   import HelloVuex from "./components/HelloVuex";
   export default {
     components: {
       HelloVuex
     },
     methods: {
       addition() {
         //使用commit调用mutations内的方法
         this.$store.commit("increment");
       },
       subtraction() {
         this.$store.commit("decrement");
       }
     }
   };
   </script>
   ```

## 2.核心概念

- ```
  state
  ```

  ：放置状态信息

  - 单一状态树：一个应用只有一个store

- `Getters`：类似于计算属性，会进行缓存

- `Mutation`：实现状态更新

- `Action`：代替Mutation进行异步操作

- `Module`：将store中的state进行分模块管理

## 3.getters

> 类似于计算属性，会进行缓存

### 3.1 基本使用

1. 定义

   `src/store/index.js`

   ```
   export default new Vuex.Store({
     state: {
       counter: 1000
     },
     getters: {
       powerCounter(state) {
         return state.counter * state.counter
       }
     }
   })
   ```

2. 使用

   ```
   {{$store.getters.powerCounter}}
   ```

### 3.2 getter传参

#### 获取其他getter的值

```
src/store/index.js
export default new Vuex.Store({
  state: {
    counter: 1000
  },
  getters: {
    powerCounter(state) {
      return state.counter * state.counter
    },
    //通过第二个参数获取其他getter的返回值
    getGeteer(state, getters) {
      return getters.powerCounter;
    }
  }
})
```

#### 获取传入参数

1. `src/store/index.js`

   ```
   export default new Vuex.Store({
     state:{
       word: 'world'
     }
     getters: {
       sayWords(state) {
         return function(words){
           return words+state.word;
         }
       }
     }
   })
   ```

2. 使用

   `{{$store.getters.sayWords('hello')}}`

   ## 4.Mutations

   > 实现状态更新
   >
   > Mutation中的方法必须是同步方法不然devtools将不能跟踪

   ### 4.1 基本使用

   见1.基本使用

   ### 4.2 传参调用

   1. `src/store/index.js`

      ```
      export default new Vuex.Store({
        state: {
          counter: 1000
        },
        mutations: {
          increment(state, count) {
            state.counter += count;
          }
        }
      })
      ```

   2. `src/App.vue`

      ```
      <template>
        <div id="app">
          <h2>{{$store.state.counter}}</h2>
          <button @click="addCount(5)">+5</button>
          <hr />
          <hello-vuex></hello-vuex>
        </div>
      </template>
      
      <script>
      import HelloVuex from "./components/HelloVuex";
      export default {
        components: {
          HelloVuex
        },
        methods: {
          addCount(count) {
            //调用过程中传入参数
            this.$store.commit("increment", count);
          }
        }
      };
      </script>
      ```

   ### 4.3 commit的风格

   ```
   methods:{
       addCount(count){
           //1.普通的提交风格
           this.$store.commit('increment',count)
           //2.特殊的提交风格，此时Mutation接收的第二个参数边为type和count组成的对象
           this.$store.commit({
   			type: 'increment',
               count
           })
       }
   }
   ```

   ### 4.4 响应式的规则

   - 改变的状态需要先再store中提前定义好

   - 在状态中的某个对象添加属性需要在Mutation中使用

     ```
     Vue.set(state.xxx(对象),'属性','值')
     ```

   - 在状态中删除某个对象中的属性需要在Mutation中使用

     ```
     Vue.delete(state.xxx(对象),'属性')
     ```

   ### 4.5 类型常量

   > 使用常量代替Mutation名，避免出错
   >
   > 官方推荐

   1. `src/store/mutations.types.js`

      ```
      export const INCREMENT = 'increment'
      ```

   2. `src/store/index.js`

      ```
      import Vue from 'vue'
      import Vuex from 'vuex'
      import { INCREMENT } from './mutations.types'
      
      Vue.use(Vuex)
      
      export default new Vuex.Store({
        state: {
          counter: 1000
        },
        mutations: {
          //使用常量代替方法名
          [INCREMENT](state, count) {
            state.counter += count;
          }
        }
      })
      ```

   3. `src/App.vue`

      ```
      <template>
        <div id="app">
          <h2>{{$store.state.counter}}</h2>
          <button @click="addCount(5)">+5</button>
          <hr />
          <hello-vuex></hello-vuex>
        </div>
      </template>
      
      <script>
      import HelloVuex from "./components/HelloVuex";
      import { INCREMENT } from "./store/mutations.types";
      export default {
        components: {
          HelloVuex
        },
        methods: {
          addCount(count) {
            //使用常量调用方法
            this.$store.commit(INCREMENT, count);
          }
        }
      };
      </script>
      ```

   ## 5.Actions

   > 代替Mutation进行异步操作

   ### 5.1 基本使用

   1. `src/store/index.js`

      ```
      export default new Vuex.Store({
        state: {
          counter: 1000
        },
        mutations: {
          mUpdateCount(state,payload) {
            state.counter = 100;
          }
        },
        actions: {
          aUpdateCount(context,payload) {
            setTimeout(() => {
              //错误代码，不能直接改变state
              //context.counter = 100;
              context.commit('mUpdateCount',payload)
            }, 1000)
          }
        }
      })
      ```

   2. `src/App.vue`

      ```
      <template>
        <div id="app">
          <h2>{{$store.state.counter}}</h2>
          <button @click="updateCount">set</button>
          <hr />
          <hello-vuex></hello-vuex>
        </div>
      </template>
      
      <script>
      import HelloVuex from "./components/HelloVuex";
      export default {
        components: {
          HelloVuex
        },
        methods: {
          updateCount() {
            //调用Action中的异步方法
            this.$store.dispatch("aUpdateCount",100);
          }
        }
      };
      </script>
      ```

   ### 5.2 异步成功回调

   1. `src/store/index.js`

      ```
      export default new Vuex.Store({
        state: {
          counter: 1000
        },
        mutations: {
          mUpdateCount(state, payload) {
            state.counter = 100;
          }
        },
        actions: {
          aUpdateCount(context, payload) {
            return new Promise((resolve, reject) => {
              setTimeout(() => {
                context.commit('mUpdateCount', payload)
                //异步成功，进行回调
                resolve('异步的成功信息')
              }, 1000)
            })
          }
        }
      })
      ```

   2. `src/App.vue`

      ```
      <template>
        <div id="app">
          <h2>{{$store.state.counter}}</h2>
          <button @click="updateCount">set</button>
          <hr />
          <hello-vuex></hello-vuex>
        </div>
      </template>
      
      <script>
      import HelloVuex from "./components/HelloVuex";
      export default {
        components: {
          HelloVuex
        },
        methods: {
          updateCount() {
            //执行异步并进行异步成功以后的回调处理
            this.$store.dispatch("aUpdateCount", 100).then(res => {
              console.log(res);
            });
          }
        }
      };
      </script>
      ```

   ## 6.Modules

   > 将store中的state进行分模块管理

   1. `src/store/index.js`

      ```
      import Vue from 'vue'
      import Vuex from 'vuex'
      
      Vue.use(Vuex)
      //模块A
      const moduleA = {
        state: {
          name: 'zhangsan'
        },
        mutations: {
          //传参修改
          updateName(state, payload) {
            state.name = payload
          }
        },
        getters: {
          //普通使用
          hello1(state) {
            return 'hello ' + state.name;
          },
          //调用其他getter
          hello2(state, getters) {
            return getters.hello1 + '!!!';
          },
          //调用根store中的状态
          hello3(state, getters, rootState) {
            return getters.hello2 + rootState.counter
          }
        },
        actions: {
          aUpdateName(context) {
            setTimeout(() => {
              //只能commit自己模块的mutation
              context.commit('updateName', 'wangwu')
            }, 1000)
          }
        }
      };
      //模块B
      const moduleB = {
        state: {},
        mutations: {},
        actions: {},
        getters: {},
      }
      
      export default new Vuex.Store({
        state: {
          counter: 1000
        },
        modules: {
          a: moduleA,
          b: moduleB
        },
      })
      ```

   2. `src/App.vue`

      ```
      <template>
        <div id="app">
          <!-- 使用module中的state -->
          <h2>{{$store.state.a.name}}</h2>
          <!-- 调用module中的mutation -->
          <button @click="updateName">更新</button>
          <!-- 使用module中的getter -->
          <h2>{{$store.getters.hello1}}</h2>
          <h2>{{$store.getters.hello2}}</h2>
          <h2>{{$store.getters.hello3}}</h2>
          <!-- 使用module中的action进行异步操作 -->
          <button @click="asyncUpdateName">异步</button>
        </div>
      </template>
      
      <script>
      import HelloVuex from "./components/HelloVuex";
      export default {
        methods: {
          updateName() {
            this.$store.commit("updateName", "lisi");
          },
          asyncUpdateName() {
            this.$store.dispatch("aUpdateName");
          }
        }
      };
      </script>
      ```

   ## 7.目录结构

   - `src/store/index.js`

     ```
     import Vue from 'vue'
     import Vuex from 'vuex'
     import mutations from './mutations'
     import moduleA from './modules/moduleA'
     
     Vue.use(Vuex)
     
     const state = {
       counter: 1000
     }
     
     export default new Vuex.Store({
       state,
       mutations,
       modules: {
         moduleA
       },
     })
     ```

   - `src/store/mutations.js`

     ```
     export default {
       updateCount(state) {
         state.counter = 100;
       }
     }
     ```

   - `src/store/modules/moduleA.js`

     ```
     export default {
       state: {},
       mutations: {},
       getters: {},
       actions: {}
     }
     ```

   - 其他属性像mutations一样模块化即可

   # 七.项目目录结构

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
   └─views---页面视图组件
   ```