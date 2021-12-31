![image-20211213113601796](https://gitee.com/yogln/image-mac/raw/master/image-20211213113601796.png)

## 前言

之前写`react`项目都是通过`redux`对数据进行统一个管理，但是现在实习团队使用是`mobx`，原因是简单，有一种在`react`写`vuex`的感觉，查看了好多资料都是很古老的装饰器使用方式，但是现在都是函数式编程，而且装饰器在最新的`mobx6`也已经废弃，所以决定自己捣鼓一个`mobx6`结合`react`的使用方式

![image-20211210172944145](https://gitee.com/yogln/image-mac/raw/master/image-20211210172944145.png)	

每个事件都会通过一个`action`来更新`observable`，`observable`的状态变更会被传入到所有依赖于他们的计算和副作用里。

## 安装

npm安装

```
npm install --save mobx
```

yarn安装

```
yarn add mobx
```

## 概念

MobX区分了应用程序中的以下三个概念：

1. State(状态)
2. Actions(动作)
3. Derivations(派生)

state可以存储在任何数据结构中：普通对象、数组、类、循环数据结构或引用.

*任何* 来源是*State(状态)* 并且不需要进一步交互的东西都是 Derivation(派生)。Derivations 包括许多方式，如用户界面、computed值等

## 结合react使用

我们通过一个简单的案例，在`react`中使用`mobx6`

![image-20211213100806661](https://gitee.com/yogln/image-mac/raw/master/image-20211213100806661.png)

我们新建一个`store`的文件夹，里面`index.js`是目录的总的出入口，示例新建一个`counter.js`作为单独的数据接结构

`counter.js`

```js
import {makeAutoObservable} from 'mobx';

class Count {
	constructor() {
		makeAutoObservable(this)
	}
	counter=0;
	
	increment = () => {
		this.counter++
		console.log('object');
		console.log(this.counter);
	}
}

const counterStore = new Count();
export {counterStore}
```

这个函数可以捕获*已经存在*的对象属性并且使得它们可观察。任何 JavaScript 对象（包括类的实例）都可以作为 `target` 被传递给这个函数。 一般情况下，`makeObservable` 是在类的构造函数中调用的，并且它的第一个参数是 `this` 。 `annotations` 参数将会为每一个成员映射 [注解](https://zh.mobx.js.org/observable-state.html#可用的注解)。

在`index.js`中引入,这里作为统一的出口

```js
import * as React from 'react';
import {configure} from 'mobx';
import {counterStore} from './counter';
import numStore from './about';

configure({enforceActions: 'always'})

export const stores = {counterStore, numStore}
export const CounterContext = React.createContext(stores)

export const useStores = () => React.useContext(CounterContext)
```

`configure`[**用法**](https://zh.mobx.js.org/configuration.html)：对正在使用的 MobX 实例进行全局行为设置。用它来改变 MobX 整体的行为方式，具体的配置方式可以查看[文档](https://zh.mobx.js.org/configuration.html)。这里的

```js
configure({enforceActions: 'always'})
```

*enforceActions* 配置的目的是让你不会忘记使用 [`action`](https://zh.mobx.js.org/actions.html) 包裹事件处理函数。

可供选择的配置:

- `"observed"` (**默认值**): 可观察状态必须通过`actions`来修改。 这是默认选项，对于有一定复杂度的应用来说这是推荐的严格模式。
- `"never"`: 状态可以在任何地方被修改。
- `"always"`: 任何状态都能只能通过`actions`来修改，在实际开发中也包括新建状态。

`"observed"`带来的好处是它允许你可以在`actions`外部创建可观察的对象然后自由地修改，即使这些对象还没有被使用。 由于状态在原则上总是被一些事件处理函数所创建，并且事件处理函数总是要被另外一个函数再包一层，因此设置本字段为`always`是理论上最优的。



那么如何进行使用呢？我们可以在想要使用的页面中

```js
import React from 'react'

import {observer} from 'mobx-react-lite';
import {useStores} from './store';

export default observer(function App() {
	const {counterStore} = useStores()
	const {counter, increment} = counterStore
	return (
		<>
			<h2>counter:{counter}</h2>
			<button onClick={e => increment()}>+1</button>
		</>
	)
})
```

这里使用到了第三方库`mobx-react-lite`,这里的主要目的是让我们使用的数据被改变后页面可以实现实时刷新，不然当我们点击按钮的时候值被修改了，但是页面中却没有变化

```bash
yarn add mobx-react-lite
```

这个时候我们点击屏幕就可以实现，在界面中使用`mobx`里面的数据和通过方法修改里面的数据并能在页面实时显示。