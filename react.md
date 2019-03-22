# 组件对象 React.Compoment

每一个该对象的实例都是一个页面组件对象，组件之间可以组合，可以单个存在，实现组件与页面分离

+ 属性 `props`
	- 每个组件的固有属性,不可变，可传递
	- 是每一个 React 对象固有的属性对象，类似于 HTML 中的标签属性对象集合
	- 每个组件之间的属性只能从父传递到子，而不能从子传递到父
	+ 属性传递的一种方式--ES6 扩展运算符(...)
		- 扩展运算符可以将从父类传递的参数以对象形式传递到子类
		- 也就是打包所有父类传递的参数，然后全部一层一层向下传递
	+ `refs`
		- React 支持一种非常特殊的属性 Ref ，你可以用来绑定到 render() 输出的任何组件上

+ 状态 `state`
	- 每个组件固有的属性，可变，不可传递
	+ componentDidMount()
		- 一旦组件从 DOM 中添加， React 会调用该方法

	+ componentWillUnmount()
		- 一旦组件从 DOM 中移除， React 会调用该方法

	+ setState()
		- 设置状态属性对象

+ render()
	- 组件对外的唯一接口

+ 事件
	- React 事件绑定属性的命名采用驼峰式写法，而不是小写
	- 如果采用 JSX 的语法你需要传入一个函数作为事件处理函数，而不是一个字符串(DOM 元素的写法)
	- 不能用 `return false` 阻止默认行为, 必须明确的使用 `event.preventDefault`
	+ 最重要的一点就是 `this` 的绑定，
		- Function.prototype.bind
		+ ES6 箭头函数
	+ 向事件处理程序传递参数
		- 事件: this.handler.bind(this，要传的参数)
		- 函数: handler(传过来的参数，event)

+ 条件渲染
	- React 组件可以用 `&&` 和 三目运算来选择渲染组件

+ 列表 & Keys
	+ key 也是每一个组件的一个属性，可用来作为每一个组件的唯一标识

+ 生命周期

	+ componentWillMount
		- 在渲染前调用,在客户端也在服务端

	+ componentDidMount
		- 在第一次渲染后调用，只在客户端
		- 之后组件已经生成了对应的DOM结构，可以通过 `this.getDOMNode()` 来进行访问 
		- 如果你想和其他 JavaScript 框架一起使用，可以在这个方法中调用 `setTimeout`, `setInterval`或者发送 AJAX 请求等操作(防止异步操作阻塞UI)

	+ componentWillReceiveProps
		- 在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用。

	+ shouldComponentUpdate
		- 返回一个布尔值
		- 在组件接收到新的props或者state时被调用
		- 在初始化时或者使用forceUpdate时不被调用
		- 可以在你确认不需要更新组件时使用

	+ componentWillUpdate
		- 在组件接收到新的 props 或者 state 但还没有 render 时被调用
		- 在初始化时不会被调用

	+ componentDidUpdate
		- 在组件完成更新后立即调用
		- 在初始化时不会被调用

	+ componentWillUnmount
		- 在组件从 DOM 中移除之前立刻被调用

+ 组件 API
	+ 设置状态: setState(object nextState[, function callback])
		- nextState，将要设置的新状态，该状态会和当前的 state 合并
		- callback，可选参数，回调函数。该函数会在 setState 设置成功，且组件重新渲染后调用。

	+ 替换状态: replaceState(object nextState[, function callback])
		- nextState，将要设置的新状态，该状态会替换当前的 state。
		- callback，可选参数，回调函数。该函数会在 replaceState 设置成功，且组件重新渲染后调用。
		- replaceState()方法与setState()类似，但是方法只会保留nextState中状态，原state不在nextState中的状态都会被删除。

	+ 设置属性: setProps(object nextProps[, function callback])
		- nextProps，将要设置的新属性，该状态会和当前的props合并
		- callback，可选参数，回调函数。该函数会在setProps设置成功，且组件重新渲染后调用。

	+ 替换属性: replaceProps(object nextProps[, function callback])
		- nextProps，将要设置的新属性，该属性会替换当前的props。
		- callback，可选参数，回调函数。该函数会在replaceProps设置成功，且组件重新渲染后调用。

	+ 强制更新: forceUpdate([function callback])
		- callback，可选参数，回调函数。该函数会在组件render()方法调用后调用。

	+ 获取DOM节点: DOMElement findDOMNode()
		- 返回值：DOM元素DOMElement

	+ 判断组件挂载状态: bool isMounted()
		- 返回值：true或false，表示组件是否已挂载到DOM中
		- 可以使用该方法保证了setState()和forceUpdate()在异步场景下的调用不会出错。

+ 组件通信
	+ 利用 props 的传递


# react-router

v4 的版本则将路由进行了拆分，将其放到了各自的模块中，不再有单独的 router 模块，充分体现了组件化的思想

<BrowserRouter> 的使用与之前作为 history 属性传入的方式也不同了。

v4 中改从 'react-router-dom' 引入的原因是因为还有个 native 版本，这个意味着是 web 版本
`import { BrowserRouter, Route } from 'react-router-dom'`

## 包含式路由与exact

如果想要只匹配一个路由，除了 exact 属性之外，还可以使用 Swtich 组件

在 v4 的版本中废弃了 <IndexRoute>，而该用 <Route exact> 的方式进行代替。如果没有匹配的路由，也可通过 <Redirect> 来进行重定向到默认页面或合理的路径。

	
# Redux 

单向数据流

安装  redux 和 react-redux

`npm install react-redux redux`


## 目录结构

+ actions
+ components
+ container
+ reducer
- index.html
- server.js
- webpack


## 组件对象 container





+ store 
	- 存放全局的所有状态的一个对象
+ action
	- 描述做什么事的类型

+ reducer
	- 修改状态，返回一个全新的 state ,达到做什么事

+ store.getState()
	- 得到 redux 的全新数据
	- 在根元素中将这个数据当做 props 渲染到组件中

+ store.dispatch()
	- 更新 redux 中的数据
	- 也就是发出做什么事的类型，action

+ store.subscribe(render)
	- 监听 redux 中的状态，一旦状态被改变，就调用 render 回调

+ Provider
	- 所有被该组件包括的组件都可以直接通过 react-redux 通信
	- 而不用一层一层的传递信息
	- 目的就是让所有组件都能够访问到 Redux 中的数据
+ connect(mapStateToProps, mapDispatchToProps)(MyComponent)
	- 该方法起一个很重要的作用就是，将 redux 的状态信息传递给 react 组件，也就是说将 store 和组件之间的一个联系
	+ mapStateToProps()
		- 把 Redux 中的数据映射到 React 中的 props 中去
	+ mapDispatchToProps()
		- 就是把各种 dispatch 也变成了 props 让你可以直接使用
	+ MyComponent 
		- 最后应用的组件

开发环境调用后台路径配置：
`proxy.config.js`文件下可以自由定义接口调用到的后台地址，业务模块不要出现应用路径(BackGround)；
 
各个模块目录下：
- api.js      定义与后台交互的接口方法
- action.js   定义页面操作触发的动作(eg. 点击查询按钮)
- reducer.js  定义触发动作后的影响(eg. 查询完成后将查询结果set回state，视图自动刷新)
