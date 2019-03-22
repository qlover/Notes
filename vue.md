
data 属性，当作为组件时，data 不在是一个属性，而是一个方法

v-once 指令
绑定数据一次，可阻止后面修改数据，而重新渲染该节点的数据

改变属性数组时不用 [] 运算符，用属性的 Vue.set(vm.items, index, newValue)
|| vm.$set()

<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">
<!-- 同上 -->
<input v-on:keyup.enter="submit">
.enter
.tab
.delete (捕获“删除”和“退格”键)
.esc
.space
.up
.down
.left
.right

<!-- 缩写语法 -->
<input @keyup.enter="submit">

// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>


混入(mixins)

定义一个混入对象，将这个对象在一个组件中以 mixins:[xxx] 使用该混入对象
当和组件中的属性数据发生冲突时以组件数据优先
混入对象和组件的钩子函数会混合成为一个数组，并且混入对象的钩子方法会被先调用

# Vuex

## mapState()
该方法可以将 state 映射到 Vue 组件中
当一个组件需要获取多个状态的时候，就可以用 mapState() 来生成计算属性。
该方法返回一个对象，如果需要与局部计算属性混合使用就需要将这两个对象或者多个对象合并成一个对象，ES6的扩展运算符可以大大地简化

## getter 
可通过属性和方法两种方式访问
mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性

Vuex 的 state/getter 和 mapState/mapGetters 很类似 Redux 的 containers, 包裹组件的一个容器好将 store 中的状态映射到 Vue 组件中

## Mutation
类似 Redux 的 reducer 和事件类型常量, 就是用来更新状态操作的方法
因为状态是响应的，所以应该初始化好所需属性，需要添加新属性时，要使用 Vue.set() 或 vm.$set()
但要特别注意，Mutation 是同步方法！！！不会产生副作用

## Action
一个作用，用于分发 Action,也就是用来触发不同 reducer 的
可以产生副作用

### 组件中分发 action
1. 使用 mapActions() 像在 Redux 中的 containers 容器中，手动写各种 actions 的作用，但记得要根节点注入 store

2. this.$store.dispatch('xxx') 直接分发
	dispatch() 可以处理 action 返回的 Promise,并且还会是会返回 Promise



















