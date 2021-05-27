## 1.目录结构的划分

![image-20210405220940941](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210405220940941.png)

## 2.初始化CSS

> yarn add normalize.css

  ```js
@import "~nomalize.css"
  ```

## 3.配置别名

> yarn add @craco/craco

![image-20210406102955780](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210406102955780.png)

### 创建`craco.config.js`

```js
const path = require('path')

const reslove = dir => path.resolve(__dirname, dir)

mofule.exports ={
    webpack: {
	alias: {
    "@": resolve("src"),
    "components":resolve("src/components")
	}
  }
}
```

## 4.路由安装

> yarn add react-router-dom
>
> yarn add react-router-config



![image-20210406104520416](C:\Users\linwa\AppData\Roaming\Typora\typora-user-images\image-20210406104520416.png)

## 5.样式

> yarn add styled-components

## 6.子路由的映射（上面已经安装）

> yarn add react-router-config

### 获取子路由routes

```js
import { renderRoutes } from 'react-router-config'
```



```js
export default memo(function Discover (props) {
  const { route } = props
  return (
    <DiscoverWrapper>
      { renderRoutes(route.routes)}
    </DiscoverWrapper>
  )
})
```

## 7. redux

> yran add redux react-redux redux-thunk

react-redux： reatc组件和redux进行结合起来

resux-thunk： 进行异步请求

### store/index.js

```js
import { createStore, applyMiddleware, compose} from 'redux'
import thunk from 'redux-thunk'
import reducer from './reducer'

const composeEnhancers = (typeof window !== 'undefined' && window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__) || compose;
const store = createStore(reducer, composeEnhancers(
  applyMiddleware(thunk)
))

export default store
```

### reducer.js

```js
import { combineReducers } from 'redux-immutable'
import { reducer as recommendReducer } from '../pages/discover/c-pages/recommend/store'
import  { reducer as playerReducer } from '../pages/player/store'

const cReducer = combineReducers({
  recommend: recommendReducer,
  player: playerReducer
})

export default cReducer
```

### App.js

```js
import { Provider } from 'react-redux'

export default memo(function App () {
  return (
    <Provider store={store}> //************
      <HashRouter>
        <Header />
        <Suspense fallback={<div>page loading</div>}>
          {renderRoutes(routes)}
        </Suspense>
        <Footer />
        <AppPlayerBar />
      </HashRouter>
    </Provider>

  )
})
```

