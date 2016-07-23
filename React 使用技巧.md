

## radio 选中

```javascript
<tbody>
                    <tr>
                        <td><input type="radio" name="site_name"
                                   value={result.SITE_NAME}
                                   checked={this.state.site === result.SITE_NAME}
                                   onChange={this.onSiteChanged} />{result.SITE_NAME}</td>
                        <td><input type="radio" name="address"
                                   value={result.ADDRESS}  
                                   checked={this.state.address === result.ADDRESS}
                                   onChange={this.onAddressChanged} />{result.ADDRESS}</td>
                    </tr>
               </tbody>
```

## React 组件默认值

例如 radio 需要根据 state 控制默认值，可以这样做

```javascript
<input id='repayment-way-one' value="1" type="radio" defaultChecked= { this.state.value==="1" }  onChange={this.changeRepaymenyWay} name='repayment-way' />
<input id='repayment-way-one' value="1" type="radio" defaultChecked= { this.state.value==="2" }  onChange={this.changeRepaymenyWay} name='repayment-way' />
```


# React 优点

composability

## 组件的所属关系

组件的拥有者是设置其他组件 props 值的组件，注意只有组件之间是拥有关系，html 元素和组件之间是上下级关系。

# 组件的设计

设计优先级：布局 > 样式

常用的提取出来，特例不考虑到组件，单独用低一级的组件拼装。

单为一个页面设计的组件不要跨页面使用

##

- 通过属性控制
- 通过 children 控制
- 完全一致时才写成组件


# 布局与样式



# 1.定义 state 和还是 props？

state 是可变的，props 是不可变的，在设计组件时，组件自身要操纵一些变化才使用 state (例如响应用户输入，网络请求结果) ，否则都应该通过 props 渲染组件。

改变 props 的工作交给组件调用者。

尽可能使多数组件保持无状态，通过这样分离逻辑与减少冗余 (重复代码)，使应用程序更容易理解。最常见的模式是创建几个无状态的组件专门用于渲染数据，在他们的上方用一个有状态的组件将 state 传递给无状态组件的 `props`。有状态组件管理所有逻辑，无状态组件只关心如何根据数据渲染界面。

### 什么应该放到 state？

state 应该包含组件的事件处理函数中可能会改变而触发 UI 刷新的数据。在构建有状态组件时，将 state 表现为他所能代表的最小状态，并只将他们保存在 `this.state`。在 `render()` 中简单与其他基于这个 state 的信息共同计算。你将会发现这样的做法是最符合大多数正确程序的。添加额外的冗余操作或把计算后的值放在 state 中意味着你需要自己明确的保持同步而不是依赖 React 为你计算。

### 什么不应该放到 state？

- 计算后的数据：不要担心把 state 参与计算－他可以确保你的 UI 与你的计算结果与 `render()` 保持一致。例如，如果你有一列数组在 state 中，你想按顺序渲染成字符串，只需要在 render 中通过 `this.state.listItems.length + ' list items'` 计算结果，而不是计算后再放入 state 中。
- React 组件：在 `render()` 中根据 props 和 state 的值渲染内容，而不是直接将他们方法 state。
- 从 props 复制的数据：尽可能直接使用 props 的数据。将 props 值存到 state 中使用的一种有效的方式是需要知道以前的值，因此 props 可能会根据父组件重新渲染导致值发生变化。

#  构建 HTML 的方式

## 数组

jsx 会自动将数组内的 html 标签转换

```javascript
let htmlArray = [];
htmlArray.push( <div></div>);

return <table>{ htmlArray }</table>
```

2. JavaScript 方式

在 render 函数的 return 的花括号中返回 html，例如：

```javascript

return <div >{
    [<div key={1}></div>,<div key={2}></div>,<div key={3}></div>].map((item)=>{
        return item
        })
    }</div>;

```

注意数组中的每个组件都需要关键字 key

### key 的作用

假设你有一个组件，这个组件下面有两个按顺序排列的子组件，这两个子组件没有添加 key。那么当父组件重新渲染，需要改变子组件的顺序时，就会出现问题，可以是丢失内容 (Child Reconciliation) ，或者顺序错误，或者子组件被隐藏。

也可以通过 ReactFragment 创建组件，这样就不用传递 key，[地址](https://facebook.github.io/react/docs/create-fragment.html)。


# 写组件时明确设置默认值

有默认值的组件可以一目了然的知道要传递哪些值。

# 在 render 利用解构对象获取 props 的值  

可以清晰知道 props 内哪些值需要在 render 中使用。

如果 render 内有函数，使用参数的方式传递数据，不要直接在函数内使用 this.props.A。

let {A,B} = this.props;

# 严格区分上下级组件的关系

父组件不要直接传递 HTML 元素给子组件，如果子组件有需要根据 props 变化的部分，建议提取成一个组件，直接传递需要的值给新组件。



# 命名规则

class:  a-b-c
css id 或 class 命名结尾添加类型，如 label，input

组件属性名 props: a_b_c

全局变量：aaAA

组件名: AbcD

函数 aBcDe

不直接使用 object，使用解构对象的方式提取数据使用

const {a,b,c} = words;

# React 与 form 组件

https://facebook.github.io/react/docs/two-way-binding-helpers.html

https://facebook.github.io/react/docs/forms.html

ReactLink

valueLink
