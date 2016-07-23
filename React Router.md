嵌套的 route，在根 route 设置 link 链接，点击链接后会改变根 route 的 children。
所以要在根 route 显示 this.props.children 才可以看到二级页面。

router 的 browserHistory 和 hashHistory 分别时改变 url 的方式 (美观) 和在 url 后面添加 # 的方式 (兼容性高) 
