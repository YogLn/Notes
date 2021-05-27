## 1.redux安装

> yarn add redux





## 2.react-redux

主要有个connect 函数，减少冗余的代码，内部做了很多的事情，使用方式在`index.js`中

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import store from './store'; 

import { Provider } from 'react-redux'; // 导入provider

ReactDOM.render(
  <Provider store={store}>  //引入主件进行包裹,传入store
    <App />
  </Provider>,
  document.getElementById('root')
);
```

在具体的要使用connect的页面中

```jsx
import React from 'react'
import { addAction, subAction } from '../store/actionCreators'
// 1.引入connect
import { connect } from 'react-redux'

function About(props) {
	return (
		<div>
			<h2>About 当前计数：{props.counter}</h2>
			<button onClick={(e) => props.increment(1)}>+1</button>
			<button onClick={(e) => props.increment(5)}>+5</button>
			<button onClick={(e) => props.decrement(1)}>-1</button>
			<button onClick={(e) => props.decrement(5)}>-5</button>
		</div>
	)
}

// 2. mapStateToProps 和 mapDispatchToProps
const mapStateToProps = state => ({
  counter: state.counter
})
const mapDispatchToProps = dispatch => ({
  increment(num) {
    dispatch(addAction(num))
  },
  decrement(num) {
    dispatch(subAction(num))
  }
}) 

// 对组件进行传入
export default connect(mapStateToProps, mapDispatchToProps)(About)
```

## 3.redux-thunk

redux-thunk主要时在redux的dispatch的时候可以传以一个函数（原本只能传入一个对象），从而能在redux中发送网络请求

> yarn add redux-thunk

在`store/index.js`中

```jsx
// 1.添加applyMiddleware
import { createStore, applyMiddleware } from 'redux'

import reducer from './reducer'
// 2.引入redux-thunk
import thunkMiddleware from 'redux-thunk';

// 3.取出应用了中间件的返回值
const storeEnhancer = applyMiddleware(thunkMiddleware)
// 4.传入createStore
const store = createStore(reducer, storeEnhancer)

export default store
```

在jsx页面，在mapdispatchToProps中添加

```js
const mapDispachToProp = dispatch => {
  return {
    getHomeDate() {
      dispatch(getHomeMultidateAction) //这里不要加()会自动调用
    }
  }
}
```

actionCreators函数中定义action

```js
// redux-thunk中定义的action函数
export const getHomeMultidateAction = dispatch => {
  // console.log(dispatch); //这里的dispatch时传过来的
  axios({
    url: 'http://123.207.32.32:8000/home/multidata'
  }).then(res => {
    console.log(res);
  }) 
}
```

### 页面使用在redux中请求到的数据

```js
export const changeBannersAction = (banner) => ({
  type: actionTypes.CHANGE_BANNERS,
  banner
})

export const changeRecommendsAction = (recommend) => ({
  type: actionTypes.CHANGE_RECOMMEND,
  recommend
})

export const getHomeMutidateAction = dispatch => {
  console.log(dispatch);
  axios({
    url: 'http://123.207.32.32:8000/home/multidata'
  }).then(res => {
    // console.log(res.data.data);
    const bannerList = res.data.data.banner.list
    const recommendList = res.data.data.recommend.list
    dispatch(changeBannersAction(bannerList))
    dispatch(changeRecommendsAction(recommendList))
  }) 
}
```

在reducer中定义好数据

```jsx
const initState = {
  counter: 0,
  banner: [],
  recommend: []
}
```

```js
 case actionTypes.CHANGE_BANNERS: 
      return {...state, banner: action.banner}
    case actionTypes.CHANGE_RECOMMEND: 
      return {...state, recommend: action.recommend}
```

页面的使用

```js
const mapStateToProps = (state) => ({
	counter: state.counter,
	banner: state.banner,
	recommend: state.recommend
})
```

```js
<ul>
    {
        props.banner.map((item, index) => {
            return <li key={item.acm}>{item.title}</li>
        })
    }
</ul>	
```



## 4.redux-devtools

在store的`index.js`中

```js
// 1. 引入compose
import { createStore, applyMiddleware, compose } from 'redux'
import reducer from './reducer'
import thunkMiddleware from 'redux-thunk';

// 2. composeEnhancers函数
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const storeEnhancer = applyMiddleware(thunkMiddleware)

// 3. 用composeEnhancers（合并组件）包裹 storeEnhancer传入
const store = createStore(reducer, composeEnhancers(storeEnhancer))

export default store
```

## 5.reducer的拆解	

`home/reducer.js`

```js
import { reducer as counterReducer} from './counter';
import { reducer as homeReducer} from './home';

function reducer (state = {}, action) {
  return {
    counterInfo: counterReducer(state.counterInfo, action),
    homeInfo: homeReducer(state.homeInfo, action)
  }
}
export default reducer
```

后者使用`redux` 库函数

```js
import { combineReducers } from 'redux';

const reducer = combineReducers({
  counterInfo: counterReducer,
  homeInfo: homeReducer
})
```

