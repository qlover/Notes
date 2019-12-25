
以函数为单位,分析每个函数实现的奇妙逻辑和思想


# 类型判断

- Object.prototype.hasOwnProperty(prop) 判断对象自身是否有某个属性
- Object.prototype.propertyIsEnumerable() 指定属性是否可以被枚举

## isArguments 

判断是否是一个 arguments,也就是函数参数对象, 该对象有两个特点, 一就是 `typeof` 操作符会返回 `object` 并且有 `callee` 属性，lodash 中作了以下几个处理
1. isObjectLike 像是一个对象
2. 是否有 callee 属性
3. 并且判断 callee 不可枚举

```js
function _isArguments(value) {
	return value != null && typeof value == 'object' &&
		value.hasOwnProperty('callee') &&
			!value.propertyIsEnumerable('callee');
}
```

## isFunction 

判断是否是一个函数,其中 lodash 对 `AsyncFunction`,`Function`,`GeneratorFunction` 三种函数都作了判断

```js
function* gen1(){}
async function getData(){}
function bar () {}
// 套用 jquery 的方式获取原生对象的 toString 引用
console.log(({}).toString.call(gen1) ) // [object GeneratorFunction]
console.log(({}).toString.call(getData) ) // [object AsyncFunction]
console.log(({}).toString.call(bar) ) // [object Function]
```

## isObject

lodash 将非 null 值,普通对象和函数都当作对象，而`isObjectLike`只是除开了函数

```js
function _isObject(value) {
  var type = typeof value;
  return value != null && (type == 'object' || type == 'function');
}
console.log(typeof null) // object
console.log(typeof ({})) // object
console.log(_isObject(null) ) // false
console.log(_isObject({}) ) // true
```
## isNumber 

lodash 有点类似恒等于于数字,但 lodash 并没有直接用恒等于,而是和 isFunction 一样,用了 toString 将字符数值和普通数字区分开来

```js
function _isNumber(value) {
  return typeof value == 'number' ||
    (isObjectLike(value) && baseGetTag(value) == numberTag);
}
```

## isNaN

lodash 这个方法可以说是一个比较有趣的方法, lodash 检测一个值是否是一个*非数字*是基于 `Number.isNaN` 并非全局下的 isNaN

并且 isNaN 还比较诡异

lodash 的 isNaN 说明为：`如果 value 是一个 NaN，那么返回 true，否则返回 false`

### Global.isNaN

全局下的 isNaN 与中其他的值不同，`NaN` 不能通过相等操作符 `==` || `===` 来判断 ，因为 `NaN == NaN` 和 `NaN === NaN` 都会返回 false

NaN 是什么？ 千万不要以先入为主的思想去把 NaN 当作不是一个数字来看待,因为不是一个数字的类型在 JavaScript 中太多了,就比如一个`{}`,一个`/\d/`,一个`[]`太多太多了,如果这些都是 NaN 那么难道 NaN 表示的就是一个*不是一个数字*的意思吗？那这就是一个错的

```js
// 这几个会隐式的将不是数字的操作数转换成数字计算
console.log( 1 + 1) // 2
console.log( false + 1) // 1
console.log( 1 + []) // 1
// 下面那个会隐式的调用自己的 toString,结果就会将变成字符串连接
console.log( {} + 1) // [object Object]1
console.log( /\d/ + 1) // /\d/1
```

那 NaN 到底是一个什么呢? 借 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN#Description) 一句话，NaN 就是下面两个东西：

1. 算术运算返回一个未定义的或无法表示的值时，NaN 就产生了
2. 将某些不能强制转换为数值的非数值转换为数值的时候，也会得到 NaN

*但是，NaN并不一定用于表示某些值超出表示范围的情况*

先看什么是*无法表示*的值,首先未定义从字面意思也能理解,在 JavaScript 中就是 undefined 嘛
```js
console.log( undefined + 1) //=> NaN
```
算术运算符 `+` 将 undefined 和 1 相加时会隐式转换成数字与右操作数相加, 在隐式转换的时候 undefined 本身是没有 toString 或者是 valueOf 方法的，也就无法完成运算符 `+` 的运算,这个结果对于 JavaScript 那就是一个不能用任何可以得到的结果表示,那这个结果只能是 NaN 了

其二不能被强制转换成数字的类型被转换成数字类型那结果也是 NaN

```js
console.log( Number(undefined) ) //=> NaN
console.log( Number(false) ) //=> 0
console.log( Number({}) ) //=> NaN
console.log( Number(/\d/) ) //=> NaN
console.log( Number('a2b') ) //=> NaN
console.log( Number('') ) //=> 0
```
上述中可以看出很多种情况在做强制转换的时候都会返回 NaN,还有一种则是某些值超出了范围

```js
console.log(0 / 0 ) //=> NaN
```
而对于全局的 isNaN 方法来说,会首先尝试将这个参数转换为数值，然后才会对转换后的结果是否是 NaN 进行判断,也许看到这里也会不明白上面为什么或那里有问题,为什么诡异,我看到这里的时候也是如此,那么先带着疑惑看看 `Number.isNaN`

*!!!JavaScript 中只有 NaN 不等于或不恒等于自身*

### Number.isNaN

同样借 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/isNaN) 一句话，注意，这里的中文文档个人觉得翻译有误,中文文档翻译的是 “确定传递的值是否为 NaN和其类型是 Number” 有点像中式英语，人个觉得就里翻译成确定传递的值是否为 Number 类型的 NaN

该方法不会强制将参数转换成数字，只有在参数是真正的数字类型，且值为 NaN 的时候才会返回 true

注意, MDN 上解释到参数只有是数字类型，且为 NaN 的时候才表示为 NaN,为什么会这样呢?我也是突然看见了这么一个例子才恍然大悟

```js
function typeOfNaN(x) {
  if (Number.isNaN(x)) {
    return 'Number NaN';
  }
  if (isNaN(x)) {
    return 'NaN';
  }
}

console.log(typeOfNaN('100F')) //=>"NaN"

console.log(typeOfNaN(NaN)) //=>"Number NaN"
```

其实什么全局的 isNaN 也好，还是ES5 加强后的 `Number.isNaN` 或者是 NaN 本身,理解 NaN 是什么，从什么角度才是最重要的，全局上理解 NaN 则可以看作是一个其是否是一个可以被转换成数字类型的值,从 ES5 加强后的可以看作是一个其是否是一个可以非数字的数值类型的值

Number.isNaN 则是将需要判断数值 value 是 NaN 那么返回 true，否则返回 false，这样就可以避免掉使用全局的在理解上的混淆, lodash 我想也是因为这样的吧

# 参考链接

- https://www.cnblogs.com/hefty/p/8190969.html
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN