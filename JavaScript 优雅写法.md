## 中间参数为空，传值给第三个参数

```javascript
createStore(reducer, initialState, enhancer) {
  if (typeof initialState === 'function' && typeof enhancer === 'undefined') {
    enhancer = initialState
    initialState = undefined
  }
```

## Redux Middleware

http://redux.js.org/docs/advanced/Middleware.html

```javascript
function logger(store) {
  return function wrapDispatchToAddLogging(next) {
    return function dispatchAndLog(action) {
      console.log('dispatching', action)
      let result = next(action)
      console.log('next state', store.getState())
      return result
    }
  }
}
```

```javascript
function applyMiddleware(store, middlewares) {
  middlewares = middlewares.slice()
  middlewares.reverse()

  let dispatch = store.dispatch
  middlewares.forEach(middleware =>
    dispatch = middleware(store)(dispatch)
  )

  return Object.assign({}, store, { dispatch })
}
```

### 分析

例如有三个 logger 方法，使用 appleMiddleware 后：

```javascript
let newStore = applyMiddleware(store,[logger,logger2,logger3]);
```

dispatch 会怎么样执行呢？可以认为是这样的写法 ：

```javascript
function logger(store) {
      return middleware(action){
    //   return function
    // logger1
          console.log('dispatching', action)
    // logger2
          console.log('dispatching', action)
    // logger3
          console.log('dispatching', action)
         store.dispatch(action)
          // logger3
          console.log('next state', store.getState())
          // logger2
          console.log('next state', store.getState())
          // logger2
          console.log('next state', store.getState())
          return result
    }
}
```

实现的效果与 co 挺像的，但 co 用到了 `generator` 所以有一些差别。
