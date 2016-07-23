`mapStateToProps` 是将状态树传递到表现层组件的，表现层组件通过 props 获取这些值。
`mapStateToProps` 第一个参数是 state，每当 state 发生变化都会出发 `mapStateToProps` 。
第二个参数是 ownProps (可选)，是传入到该组件的 props，如果使用的这个参数，那么每当传给组件的 props 发生变华都会
触发 `mapStateToProps`

mapDispatchToProps 是将 dispatch 传递到表现层组件的，表现层组件通过 props 获取函数。


## 如何设计文件结构

### 表现层组件

表现层组件是组成页面的部分，应该各自分成单独的文件。

如果一个页面内有多个组件，还不知道如何分类这些组件时，可以以组件名称创建一个目录，暂时将这些组件放入该目录中，等有其他页面需要用到这些组件时再进行重构。

如果表现层组件的渲染内容包含容器层组件怎么办？ (重新设计组件，不要出现这种情况，例如在容器层传递被包含的容器组件给表现层组件的 this.props.children，在表现层组件通过 this.props.children 渲染被包含的容器层)

### 容器层组件

容起层有自己的 render 方法，将表现层的组件连接起来

## 组件分离原则

Redux 将组件分为三类：表现层组件，容器层组件，其他组件。

表现层只负责渲染外观，通常就是一个手动创建的 react 组件，不直接通过 store 获取 state 和 dispatch。

容器层组件负责将 redux 与表现层连接起来，所有获取 state 数据，dispatch action 的操作都在这里完成。
在容器层内定义名为 `mapStateToProps` 的 object，把需要的 state 数据放在 object 内。定义名为
`mapDispatchToProps` 的 object，把需要的 function 定义在 object 内的 propery，在该 function 可以 dispatch action。
最后通过 connect 使 redux 与表现层连接。

表现层组件可以 render 容器层，容器层只能连接表现层。

其他组件是表现层组件与容器层组件的组合，当一个小型的组件涉及到 state 与 dispatch，由于太小还无法细分为表现层和容器层
时可以将他们放在一起定义。

## 常用 state 操作

因为 state 不能直接修改，所以要使用一些函数返回一个新的 state。


修改 state 值并返回新 state

```javascript
Object.assign({}, state, {
        isFetching: true,
        didInvalidate: false
      })
```

# 技巧

通常 action creator 是返回一个原生对象，然后在其他地方 dispatch 原生对象。

通过使用 redux-thunk 这个 middleware，可以在 action creator 直接 dispatch 对象，将返回值从原生对象改为 function，即可在 function 内使用 dispatch。这个函数接收 dispatch 和 getState 做为参数。

你可以在 connect 的第二个参数的对象中传入这种 function，使被包裹的组件可以通过他访问 state 的不同部分，或直接 dispatch action，例如读取默认数据。

由 connect 返回的组件中，可以直接调用 action creator，如果这个 action creator 返回的是 function，这个 function 会立即执行。

 (redux real-world 项目和 https://github.com/gaearon/redux-thunk)
