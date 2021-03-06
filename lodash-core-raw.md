
以函数为单位,分析每个函数实现的奇妙逻辑和思想


# 类型判断--Lang

- Object.prototype.hasOwnProperty(prop) 判断对象自身是否有某个属性
- Object.prototype.propertyIsEnumerable() 指定属性是否可以被枚举
- Array.prototype.slice() 截取数组，外排
- Number.isNaN()
- Function.prototype.length
- Function.length

## Lodash.isArguments 

判断是否是一个 arguments 对象, arguments 也就是函数参数对象, 是数组的一种`鸭子类型`, 该对象有两个特点, 一就是 `typeof` 操作符会返回 `object` 并且有 `callee` 属性，lodash 中作了以下几个处理
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

## Lodash.isFunction 

判断是否是一个函数,其中 lodash 对 `AsyncFunction`,`Function`,`GeneratorFunction` 三种函数都作了判断

```js
function* gen1(){}
async function getData(){}
function bar () {}

console.log(({}).toString.call(gen1) ) // [object GeneratorFunction]
console.log(({}).toString.call(getData) ) // [object AsyncFunction]
console.log(({}).toString.call(bar) ) // [object Function]
```

## Lodash.isObject

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
## Lodash.isNumber 

lodash 有点类似恒等于于数字,但 lodash 并没有直接用恒等于,而是和 isFunction 一样,用了 toString 将字符数值和普通数字区分开来

```js
function _isNumber(value) {
  return typeof value == 'number' ||
    (isObjectLike(value) && baseGetTag(value) == numberTag);
}
```

## Lodash.isNaN

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


## Lodash.isEmpty

lodash 将下面几种视为空:
1. 一个没有可以枚举属性的对象
2. 一个 length 为 0 的 `arguments`||`空数组`||`缓冲区`|| 字符串 ||  jQuery 对象集合
3. size 为 0 的 map || set
4. 函数始终为空

```js
console.log( Lodash.isEmpty(null) ) // => true
console.log( Lodash.isEmpty(true) ) // => true
console.log( Lodash.isEmpty('1') ) // => false
console.log( Lodash.isEmpty(1) ) // => true
console.log( Lodash.isEmpty([1, 2, 3]) ) // => false
console.log( Lodash.isEmpty({ 'a': 1 }) ) // => false
```

需要注意的是，三个 length
1. Function.length 作为 Function 构造器的 length 属性，始终为 1，且不可枚举
2. Function.prototype.length 函数的形参个数
3. argument.length 函数的实参个数

```js
console.log( Function.length ) //=> 1
console.log( function(){}.length ) //=> 0
console.log( function(a,b){}.length ) //=> 2
console.log( (function(a,b){ return arguments.length}(1)) ) //=> 1
console.log( ((a,b,c)=>{}).length ) //=> 3
```
Lodash.isEmpty 会将一个函数当作空，不管该函数有多少个形参或实参，这里说起函数下面看一个 `isNative` 的方法

### Lodash.isNative 判断一个参数是否为原生函数

原生函数就像是数组的 join,split,push 等这类函数,也就是由 JavaScript 实现的函数,但是该方法不包括在 lodash 核心中

```js
console.log( Lodash.isNative( Lodash.isEmpty ) ) //=> false
console.log( Lodash.isNative( [1,2,3].push ) ) //=> true
```

## Lodash.eq

先看看 ECMAScript 规定的几个内部比较规范，可能会更好理解 eq 方法，以 x,y 两个未知数做比较有以下几可能:

+ 恒等于与 SameVlue 对比多处理了有符号数和 NaN 情况 [Strict Equality Comparison](http://ecma-international.org/ecma-262/6.0/#sec-strict-equality-comparison)
	- x y 两个数类型不同则为 false
  - x 为 undefined 或 null 直接返回 true
  - 当 x 为 Number
  1. x 或 y 其中一个是 NaN 返回 false
  2. 如果相等返回 true
  3. `-0`与`+0`, `+0`与`-0` true
  4. 其他情况一律返回 false
  - 当 x 为 String
  1. x 与 y 索引上的每一位都相等，则返回 true
  2. 其它一律 false
  - 当 x 为 Boolean
  1. x 与 y 除非都为 true 或 false, 返回 true
  2. 其它一律 false
  - 当 x 为 Symbol, 只有自己和自己相等
  - 当 x 为 Object, 只有自己和自己相等
  - 其它情况一律 false


+ [SameValue](http://ecma-international.org/ecma-262/6.0/#sec-samevalue)
	1. 如果类型不同返回 false
	2. x 为 undefined 或 null 返回 true
	3. 当 x 为 Number
		- x 与 y 都为 NaN 为相等
		- `-0`与`+0`, `+0`与`-0` 返回 false
		- 数值一样返回 true
		- 其它情况一律 false
	4. 当 x 为 String, x 与 y 索引上的每一位都相等，则返回 true
	5. 当 x 为 Boolean,x 与 y 除非都为 true 或 false, 返回 true
	6. 当 x 为 Symbol, 只有自己和自己相等
	7. 当 x 为 Object, 只有自己和自己相等

+ [SameValueZero](http://ecma-international.org/ecma-262/6.0/#sec-samevaluezero)
	1. 如果类型不同返回 false
	2. x 为 undefined 或 null 返回 true
		- x 与 y 都为 NaN 为相等
		- `-0`与`+0`, `+0`与`-0` true
		- 数值相等返回 true
		- 其它一律 false
	3. 当 x 为 Boolean,x 与 y 除非都为 true 或 false, 返回 true
	4. 当 x 为 Symbol, 只有自己和自己相等
	5. 当 x 为 Object, 只有自己和自己相等

先多说一句规范中这么多的比较规范, lodash 使用了 `SameValueZero`, 仔细观察三个规范的相同处于不同处，会发现除了在数字比较时 NaN 和 `-0` 与 `+0` 比较有出处，其它地方几乎一样，也就是说，在恒等的基础上将 NaN 和 `-0` 与 `+0` 进行了多一次的判断，上面有提到一个 NaN 的特性不知道还有印象没有，说 JavaScript 中只有 NaN 不恒等自己

```js
function _eq(value, other) {
  return value === other || (value !== value && other !== other);
}
console.log( _eq(NaN, NaN) ) //=> true
console.log( _eq(-0, +0) ) //=> true
console.log( _eq(1, '1') ) //=> false
console.log( _eq(null, undefined) ) //=> false
console.log( _eq(null, {}) ) //=> false
```
而由于全局 isNaN 的判断诡异,如果非要利用 isNaN 则可以使用 Number.isNaN 来专门判断是否是一个非数字类型,下面例子与上面例子是恒等的
```js
function _eq(value, other) {
  return value === other || (Number.isNaN(value) && Number.isNaN(other))
}
```

## clone 

值克隆(如果要深度复制可见完整版本),该方法除了错误对象、函数、DOM 节点和 WeakMaps 基本全都可以完成复制

当然如果说只是要一个复制值那很简单,也就是两个变量交换而已,如果是一个引用类型需要复制，比如说要复制一个数组，我们通常想到的办法则是将数组遍历一次依次返回出一个新的数组，或者是直接借用原生 Array.ptotoype.map, Array.ptotoype.slice 或其它外排方法，直接返回出一个新的数组,话虽是这样说，但是 JavaScript 世界中却没有想的那样简单，下面来看看 `Lodash.clone()` 

```js
let a = [1,2,3]
let b = Lodash.clone(a)
console.log(a)//=> [ 1, 2, 3 ]
console.log(b)//=> [ 1, 2, 3 ]
console.log(a === b)//=> false
let c = { 'ns' : a, 'name': 'qlover'}
let d = Lodash.clone(c)
console.log(c)//=> { ns: [ 1, 2, 3 ], name: 'qlover' }
console.log(d) //=> { ns: [ 1, 2, 3 ], name: 'qlover' }
console.log(c === d) //=> false
console.log(c.ns === d.ns) //=> true
```

可以明显的看出来 clone 做到了值的复制但并没有深度克隆,说起深度克隆我就想起了`jQuery.extend()`,这个方法是真的设计巧妙, jQuery.extend 这个方法没一个形参，却可以接受若干个实参，其中可以像普通值复制一样参数一 target 参数二 source ,又或者是参数一传入布尔值作为深浅复制标识,内部值复制遍历赋值深度克隆则递归,用 arguments 这个特殊对象,在内部将实参整理成固定形参方式，这样不管传入什么参数，其内部的参数组合递归复制是完成深浅复制的特点,而 lodash 的做法与 jQuery.extend 相似，两者都有自己特色。下面是 Lodash.clone() 的源方法

```js
function clone(value) {
  if (!isObject(value)) {
    return value;
  }
  return isArray(value) ? copyArray(value) : copyObject(value, nativeKeys(value));
}
```
在附上一个私有的 `#`baseSlice 私有方法,该方法和 Array.prototype.slice 一样截取数组的方法

```js
function _baseSlice(array, start, end) {
  var index = -1,
      length = array.length;

	// 保存开始值到数组的最小索引
  if (start < 0) {
    start = -start > length ? 0 : (length + start);
  }
 
	// 保存结束始终不超过数组长度和0以下
  end = end > length ? length : end;
  if (end < 0) {
    end += length;
  }
  length = start > end ? 0 : ((end - start) >>> 0);
  start >>>= 0; 
  // 遍历赋值
  var result = Array(length);
  while (++index < length) {
    result[index] = array[index + start];
  }
  return result;
}
```


# 数组操作 Array 

## `#`arrayPush

从字面意思上看可以看出是一个数组 push 操作,于数组原型上的 push 操作不同的是,数组原型上的 push 只能一次 push 一个元素,如果该元素又是一个数组,则结果也会将这个数组当作一个元素作为 push 的值

而 arrayPush 利用`Function.prototype.apply`方法特性,可以类似`Array.prototype.concat`方法一样,将 push 的多个元素一依次追加到目标末尾

## `#`baseEach 私有方法 ==^c1^==

是创建`baseEach`或`baseEachRight`这两个私有方法的源方法,接收两个参数第一个是迭代回调，第二个是布尔值，可指定从右到左迭代

```js
function _createBaseFor(fromRight) {
  return function(object, iteratee, keysFunc) {
    // ...
    return object;
  };
}

function _baseForOwn(object, iteratee) {
	// 此处的 false 为 _createBaseFor 传递参数表示是否从右至左遍历
  return object && _createBaseFor(false)(object, iteratee, Object.keys);
}

function _createBaseEach(eachFunc, fromRight) {
  return function(collection, iteratee) {
    // ...
    return collection;
  };
}
var baseEach = _createBaseEach(_baseForOwn);
var baseEachRight = _createBaseEach(_baseForOwn, true);
```

听说函数式的思想就是将参数,返回,各种部分当作函数来用,也`应证函数是一等人民`

### 基函数 `#`createBaseFor

Lodash.forIn 和 Lodash.forOwn 的基函数,什么是基函数,可以理解为 Lodash.forOwn 函数是由该函数生成,或者是 Lodash.forIn 函数的母亲是该函数

这里的两个生成函数并不是核心里面的,但核心中有一个叫 `baseFor` 的重要函数, `baseForOwn` 函数也是由 baseFor 该生成专门用来可遍历`类数组`的函数

```js
function _createBaseFor(fromRight) {
  return function(object, iteratee, keysFunc) {
    var index = -1,
        iterable = Object(object),
        props = keysFunc(object),
        length = props.length;

    while (length--) {
      var key = props[fromRight ? length : ++index];
      if (iteratee(iterable[key], key, iterable) === false) {
        break;
      }
    }
    return object;
  };
}
```
该方法很简单,就是做一个类数组遍历操作,而这里的对象遍历操作避免了 for...in 操作,可能有的人注意到了。
`_createBaseFor`

方法可以接收一个参数,表示其是否从右至左遍历,虽然不知道为什么这样,是为了方便也好还是怎样,收集资料后个人觉得可能是因为 for...in 遍历出来的顺序不一定按顺序。

### 基函数 `#`createBaseEach
```js
function _createBaseEach(eachFunc, fromRight) {
  return function _createBaseEach_return(collection, iteratee) {
    if (collection == null) {
      return collection;
    }
    // 类数组处理
    if ( !Lodash.isArrayLike(collection)) {
      return eachFunc(collection, iteratee);
    }
    // 通常集合处理
    var length = collection.length,
        index = fromRight ? length : -1,
        iterable = Object(collection);

    while ((fromRight ? index-- : ++index < length)) {
      if (iteratee(iterable[index], index, iterable) === false) {
        break;
      }
    }
    return collection;
  };
}
```

jQuery 的链式操作应该是10多年前的一个新潮思想, jQuery 的链式操作做到了`do less, write more`其原理就是在每个方法后面返回了 this, 这里看得第一眼就让我想起了链式操作,当集合`collection`是数组走`eachFunc`也好还是对象走`iteratee`最终结果不仅遍历了元素且返回了原集合，该方法内部对`类数组`和通常可遍历的对象都作了处理，如果是类数组则用上面的 `_createBaseFor` 生成的函数作遍历，其余的转换成对象，对对象的属性值作遍历操作最后返回遍历集合, 这整个函数调来调去就是为了生成`baseEach`方法。

1. `createBaseFor` 是生成遍历函数的基函数,也可以理解为抽象类,作用就是可以让一个函数具有遍历功能,`baseFor`就是 createBaseFor 返回的函数,只用来赋值接收
2. 其次 `baseForOwn` 是专门用来为 baseFor 赋值(其实就是没有 formRight 参数),规定了遍历集合，遍历集合回调和遍历集合的属性集合(这里用的是 Object.keys)
3. 最后可以理解为最后的实现类,`createBaseEach`方法专门用来生成遍历指定集合，可遍历类数组或通常可遍历对象
4. 而 `baseEach` 或者是 `baseEachRight` 就是 createBaseEach 暴露出来的最终接口, 

*createBaseFor,createBaseEach 都属于基函数,且这里面每一个函数都 pure*

## Lodash.flatten

将数组扁平化一次，这里的扁平化一次代表的是将深度扁平一次

```js
console.log(Lodash.flatten([1,2,[3,[4]],5,[6]])) //=>[ 1, 2, 3, [ 4 ], 5, 6 ]
```

### `#`baseFlatten 私有方法

该方法在 flatten 方法之上,用递归的方式实现了多层次的扁平化, flatten 可以说只是这个方法的一个接口,该方法可以接收五个参数
参数一需要扁平化的数组，参数二需要扁平化有层次，参数三是一个回调，这个回调在 lodash 源码中默认表示判断是否是一个数组或类数组,参数四是一个用于判断是否需要跳过参数三回调的检查,而参数五可以是一个初始化参数,像 Array.prototype.reduce 方法一样


```js
function _baseFlatten(array, checkCallback, result) {
  var index = -1

  checkCallback = checkCallback || Array.isArray
  result = result || [] // 默认初始值为空数组

  while ( ++index < array.length ) {
    var value = array[index]
    if( checkCallback(value) ){ // 为数组
    	// 特别注意此处应该 array 就应该为 value 这个数组
    	_baseFlatten(value, checkCallback, result)
    } else {
      result.push(value)
    }
  }
  return result
}

console.log( _baseFlatten([1,2,[3,4],5]) ) //=> [ 1, 2, 3, 4 , 5 ]
console.log( _baseFlatten([1,2,[3,4],5]) ) //=> [ 1, 2, 3, 4 , 5 ]
console.log( _baseFlatten([1,2,[3,4],5], Array.isArray, [100]) ) //=> [ 100, 1, 2, 3, 4, 5 ]
```

这是一个简单版的 baseFlatten, 可接收四个参数可以实现一个简单的扁平化,也可以，在当前方法中添加一个深度标识或再添加上一个 Lodash.`#`baseFlatten 方法的一个 isStrict 标识,再对上述方法改造

```js
function _baseFlatten(array, depth, checkCallback, result) {
  var index = -1,
  length = array.length

  checkCallback || ( checkCallback = Array.isArray )
  result || ( result = [])

  while ( ++index < length ) {
    var value = array[index]
    if( depth && checkCallback(value)){ // 如果有深度
      if( depth > 1  ){ // 深度大于 1 并且为数组
        _baseFlatten(value, depth - 1, checkCallback, result)
      } else {
      // !!! 当深度超过 0 次，需要将当前是数组的元素添加到 result 中
      // 两种方法 concat, 还有一种就是 apply
        result = result.concat(value)
      }
    } else {
      result.push(value)
    }
  }
  return result
}

console.log( _baseFlatten([1,2,[3,4],5], 0) ) //=> [ 1, 2, [ 3, 4 ], 5 ]
console.log( _baseFlatten([1,2,[3,4],5], 1, Array.isArray) ) //=> [ 1, 2, 3, 4 , 5 ]
console.log( _baseFlatten([1,2,[3,4],5], 1, null, [200]) ) //=> [ 200, 1, 2, 3, 4, 5 ]
```

再递归后的第一次最需要注意的是，也是整个扁平化的关键点,因为当一个元素是数组时,该元素上有可能也有数组，也可能没有数组,而如果是扁平第一次,就说明该元素是一个数组才扁平，而关键就是将这整个数组在当次扁平时加入到结果中,而实现整个数组加入到另一个数组中有 concat 和 apply 两个方法，下面来看 lodash 中是怎么实现该方法的,源码如下:

```js
function _isFlattenable(value) {
  return Lodash.isArray(value) || Lodash.isArguments(value);
}
function _baseFlatten(array, depth, predicate, isStrict, result) {
  var index = -1,
      length = array.length;
  // 参数整理
  predicate || (predicate = _isFlattenable);
  result || (result = []);

  while (++index < length) {
    var value = array[index];
    // 当深度大于 0 且回调返回 true 时
    // 这里的回调是默认等于 _isFlattenable 也就是判断是否为数组或类数组
    if (depth > 0 && predicate(value)) {
    	// 如果超过2个深度，则递归调用自己,并且每一次之后深度都会减少一次，达到扁平
      if (depth > 1) {
        // Recursively flatten arrays (susceptible to call stack limits).
        _baseFlatten(value, depth - 1, predicate, isStrict, result);
      } else {
      	// arrayPush(result, value)
        [].push.apply(result, value); // apply 到数组中
      }
    } else if (!isStrict) { // 如果为真，跳过也跳过回调检查,直接返回
      result[result.length] = value; // 其实这里也是一个 push 操作
    }
  }
  return result;
}

console.log( _baseFlatten([1,2,[3,[4,5],6],7,8,[9]], 1, _isFlattenable, false, [10, [20] ] ) )
//=> [ 10, [ 20 ], 1, 2, [ 3, [ 4, 5 ], 6 ], 7, 8, [ 9 ] ]
console.log( _baseFlatten([1,2,[3,[4,5],6],7,8,[9]], 2, _isFlattenable, false ) )
//=> [ 1, 2, 3, 4, 5 , 6, 7, 8, 9 ]
console.log( _baseFlatten([1,2,[3,[4,5],6],7,8,[9]], 2, _isFlattenable, true ) )
//=> [ 4, 5 ]
console.log( _baseFlatten([1,2,[3,[4,5],6],7,8,[9]], 0, _isFlattenable ) )
//=> [ 1, 2, [ 3, [ 4, 5 ], 6 ], 7, 8, [ 9 ] ]
```

从源码中两个或运算看整理参数也是与通常不同,或运算也算是一个表达式,与通常的 `result = result || []` 这样写法不同，但实际结果相同，这是因为或运算有个特点,也就是如果表达式为真则直接返回左操作数，如果为假则会返回右操作数,这里右操作数是一个赋值操作，而赋值运算也会返回一个结果,如果在 apply 和最后中括号赋值直接在内部操作，可能该方法会变纯

还有个较巧妙的地方就是 isStrict 参数,先来看看可以做什么,如果将 isStrict 设置为 true,那么结果就只会将指定深度的元素合并,如下试例:

```js
console.log(
  _baseFlatten(
    [1,2,[3,[4,5],6],[7,8,[9]]], 
    2, _isFlattenable, true
  )
)
//=> [ 4, 5, 9 ]
```

每遍历出来的元素要么是一个原始值,要么又是一个类数组,如果遍历出来的那个元素是个类数组则会递归调用,但每一次的最后结果都是一样,会将每个原始值连接起来,这个时候 isStrict 参数就出现了作用,默认情况下 isStrict 为 false 是允许将遍历出来的原始值接追加到结果末尾,但是如果 isStrict 设置为 true,那么每一次调用的结果都不会将原始值追加到结果末尾,只会`arrayPush`将是类数组追加到末尾

所以如果需要扁平的数组已经是被扁平后的数组,这个时候将 isStrict 设置为 true 结果肯定是一个空数组

```js
console.log( _baseFlatten([1,2,3,4,5,6,7,8,9], 1, _isFlattenable, true) )
//=>
``` 


### Lodash.flattenDeep

由 `_baseFlaten`可知道影响扁平深度的是一个 depth 计数变量，而 flattenDeep 则是`_baseFlatten`暴露出来深度扁平的方法,而其中的 depth 则是一个 `INFINITY`,但需要请注意的是 INFINITY 是不能直接被字面量表示的,在 loadsh 中用 `1/0` 表达示得到了 INFINITY,flattenDeep 会一直扁平化数组到元素中没有数组为止

```js
console.log( Lodash.flattenDeep([1,2,3,4,[5,6,7,8,[9,10,11,12,13,[14,15,16,17]]]]))
//=> [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17 ]
```

## Lodash.forEach


对于 ES5 提供的一些方法，比如 Array.prototype.filter,Array.prototype.map,Array.prototype.reduce lodash 都已经有实现,如果浏览器不支持 ES5, lodash 则是比较好的选择

```js
console.log( Lodash.filter ) //=> [Function: filter]
console.log( Lodash.forEach ) //=> [Function: forEach]
console.log( Lodash.every ) //=> [Function: every]
console.log( Lodash.map ) //=> [Function: map]
console.log( Lodash.reduce ) //=> [Function: reduce]
```

Lodash.forEach 方法是由 `#baseEach()` 私有方法生成的

回忆一下 `#baseEach()`，由 `#createBaseEach(baseForOwn)` 和参数 `baseForOwn` 两个函数生成，而 baseForOwn 又是由 `createBaseFor(false)`生成,此处最后则生成了 forEach,一个最终的实现接口


```js
let foo = [1,2,3,4,5,6]
let bar = Lodash.forEach(foo, function(i,v){})
console.log(foo)//=> [ 1, 2, 3, 4, 5, 6 ]
console.log(bar)//=> [ 1, 2, 3, 4, 5, 6 ]
```

可以接收两个参数，参数一需要被迭代的集合,参数二则是一个迭代回调, `#baseEach()`本身就是由 createBaseEach 生成, createBaseEach 就是可迭代类数组或对象,而迭代回调方法则可自定义,为什么这么说呢?

### `#`baseIteratee

lodash 解释该私有方法是主要是实现 `Lodash.iteratee`, forEach 也是由它生成的,以下是它的伪源码:

```js
// 可遍历的本体函数
// 可以想成是一个默认的遍历回调,就是返回遍历值
function _identity(value) {
  return value;
}

// 默认回调2
function _baseMatches(source) {
  var props = nativeKeys(source);
  return function(object) {
    //...
    return true;
  };
}

// 默认回调3
function _baseProperty(key) {
  return function(object) {
    //...
  };
}

// 生成一个默认回调方法
function _baseIteratee(func) {
	// 自定义直接返回
  if (typeof func == 'function') {
    return func;
  }
  // 如果没有则直接返回本体回调
  if (func == null) {
    return identity;
  }
  // 如果是对象或是其它则对应返回回调2和3
  return (typeof func == 'object' ? _baseMatches : _baseProperty)(func);
}

function forEach(collection, iteratee) {
  return baseEach(collection, _baseIteratee(iteratee));
}
```

也就说, baseIteratee 在这里是为了为 baseEach 参数二回调进行一个处理,这也是为什么在 Lodash.iteratee 参数二可以是一个函数,一个对象,一个数组等

#### `#`baseMatches 与 `#`baseProperty

该回调作用很明确,第一步遍历可枚举的属性与目标对象的每个属性和值是否相等,也是`Lodash.metches`的基函数

而取得对象的所有属性名用的是可枚举对象属性名的 Object.keys,得到目标对象的一个可枚举属性数组,在通常的 Object.keys 之上,在 ES5 里，Object.keys 参数不是对象（而是一个原始值）,那么它会抛出 TypeError。 在 ES2015 中,非对象的参数将被强制转换为一个对象。

```js
const _nativeKeys = function overArg(func, transform) {
  return function _overArg_inner_(arg) {
    return func(transform(arg));
  };
}(Object.keys, Object)
console.log( _nativeKeys({'name': 'qlove', 'age': 10}) )
```

第二步:

1. 目标对象不是一个对象,直接直接属性名取属性值进行比较
2. 目标对象是一个取属性数组中的每一个属性名与目标对象的值进行比较

不是对象时比较相等不再是像通常直接`foo[prop] == bar[prop]`这样,lodash 是一个函数工具库,作为回调2`#baseIsEqual()`,baseIsEqual 之后会详细解释,下面是 baseMatches 回调2源码:

```js
function _baseMatches(source) {
  var props = nativeKeys(source);
  return function _baseMatches_inner_(object) {
    var length = props.length;
    if (object == null) {
      return !length;
    }
    object = Object(object);
    while (length--) {
      var key = props[length];
      if (!(key in object &&
            baseIsEqual(source[key], object[key], COMPARE_PARTIAL_FLAG | COMPARE_UNORDERED_FLAG)
          )) {
        return false;
      }
    }
    return true;
  };
}
```
与之相反,如果不是对象，则直接用属性名取值,作为回调3的判断

```js
function _baseProperty(key) {
  return function(object) {
    return object == null ? undefined : object[key];
  };
}
```

最后附上整个 baseIteratee 源码:
```js
function baseIteratee(func) {
  if (typeof func == 'function') {
    return func;
  }
  if (func == null) {
    return identity;
  }
  return (typeof func == 'object' ? baseMatches : baseProperty)(func);
}
```

如果参数为回调,会直接返回该回调函数生成一个由传入回调的回调迭代函数,如果是一个对象,这的对象不局限只是 object 任何 typeof 返回的对象都可以,如果为对象则会返回默认的 baseMatches 方法作为返回的回调迭代函数,该函数作用就是判断这两个对象值是否相等,而 baseProperty 是接收一个不是对象的类 Key,最终会返回调用该回调迭代的指定对象的该 key 值

## `#`baseIsEqual ---位掩码标识

位运算的几个规则:

1. 与运算`&`, 两个数二进制位都为 1 则结果为 1, 否则为 0
2. 或运算`|`, 两个数二进制位有一个为 1 则结果为1，否则为 0
3. 非运算`~`, 一个数的 1 与 0 位互换
4. 异或`^`, 两个数二进制位,相互为 1 或相互为 0 则为 0, 否则为 1
5. 左移`<<`, 将左操作数的二进制位依次向左移动右操作数位数(溢出的位数用0补充),取最后结果
5. 右移`>>`, 将左操作数的二进制位依次向右移动右操作数位数(演出的位数用0补充),取最后结果
6. 无符号右移`>>>`, 将左操作数的二进制位依次向右移动,如果出现溢出，则不算作符号位,直接算作正常位,取最后结果

记住以上的计算规则,位掩码会用上

这里借网上一个例子,这个例子不是什么1000 瓶毒药多个老鼠可以尝出那瓶毒药,不研究算法,是一个关于权限的例子

假设有这样一个系统，有四个权限,查看,更新,删除,新增四个权限,有许多用户，每个用户分配了一个权限ID,这个ID就包含了四个权限可用值如何确定这个ID包含了那些权限?

当只允许有一个权限时：
```js
const CAN_SELECT = 1 // 可以修改权限 十进行 1 二进行 1 << 0 == 0001
const CAN_INSERT = 2 // 可以新增权限 十进行 2 二进行 1 << 1 == 0010
const CAN_DELETE = 4 // 可以新增权限 十进行 4 二进行 1 << 2 == 0100
const CAN_UPDATE = 8 // 可以新增权限 十进行 8 二进行 1 << 3	== 1000

// 权限变量
// 可以是指定的一个权限
// 也可以是几个权限组合
// 首先确定只有一个权限
let auth = 1

// 抓住与运算特点,我们要看一个数是否满足另一个数
// 也就是两个数的二进制位有相等的位，且在并不改变原位数的值
// 只有与运算,两个数二进制位都为 1 则结果为 1, 否则为 0

// 0001 & 0001 => 0001
// 两个操作数相等
console.log( (auth & CAN_SELECT) != 0 ) //=> true
// 0001 & 0100 => 0000
console.log( (auth & CAN_UPDATE) != 0 ) //=> false

// 改变权限
auth = 4
console.log( (auth & CAN_SELECT) == CAN_SELECT ) //=> false
console.log( (auth & CAN_DELETE) == CAN_DELETE ) //=> true
```

当可以有多个权限时:

```js
auth = 3
// 0011 & 0001 => 0001
console.log( (auth & CAN_SELECT) != 0 ) //=> true
// 0011 & 0010 => 0010
console.log( (auth & CAN_INSERT) != 0 ) //=> true
```

四种权限有 1 6种组合方式,这16种组合方式就都是通过位运算得来的,其中参与位运算的每个因子你都可以叫做掩码`MASK`,所以还可以这样看是否有删除或更新权限

```js
auth = 7

// 删除和更新权限分别是 4 和 8
// 是否有其中一个权限,就可以看这个两个权限是否包含

// 首先, 可以直接从十进制看出只要一个数大于4就表示了有两个权限的其中一个
// 但是不一定两个比较顺序是否一致
// 或者是将两个权限相加再用与运算不就可以了

// 0100 | 1000 => 1100
// 1110 & 1100 => 1100
console.log( (auth & (CAN_DELETE | CAN_UPDATE)) != 0 ) //=> true

const CAN_DELETE_UPDATE = CAN_DELETE | CAN_UPDATE
console.log( !! (auth & CAN_DELETE_UPDATE) ) //=> true
```

如上 CAN_DELETE_UPDATE 在程序上就叫掩码,就是位掩码
如果当需要权限必须满足时呢?其实,仔细的话就会发现上面的 与运算后的结果要么为0要么不为0,为0则表示被包含在其中,而如果不等于0且等掩码不就表示必须要有这两个权限了

```js
let auth = 6
const CAN_DELETE_UPDATE = CAN_DELETE | CAN_UPDATE
console.log( (auth & CAN_DELETE_UPDATE) == CAN_DELETE_UPDATE ) //=> false
auth = 12
console.log( (auth & CAN_DELETE_UPDATE) == CAN_DELETE_UPDATE ) //=> true
```

当然十六进制的话表示的更多,其它的八进制都可以,但它们的思想是一样的,最后借网上一个总结增加其它的位操作方法如下:

1. 增加属性 `|`

	如果需要向flag变量中增加某个FLAG，使用 `|` 运算符 `flag |= XXX_FLAG`

	原因: 如果flag变量没有XXX_FLAG，则`|`完后flag对应的位的值为1，如果已经有XXX_FLAG，则`|`完后值不会变，对应位还是1。

2. 包含属性 `&`

	如果需要判断flag变量中是否包含XXX_FLAG，使用"&"运算符，flag & XXX_FLAG != 0 或者 flag & XXX_FLAG = XXX_FLAG。

	原因: 如果flag变量里包含XXX_FLAG，则`&`完后flag对应的位的值为1，因为XXX_FLAG的定义保证了只有一位非0，其他位都为0，所以如果是包含的话进行`&`运算后值不为0，该位上的值为此XXX_FLAG的所在位上的值，不包含的话值为0。

3. 去除属性 `&~`

	如果需要去除flag变量的XXX_FLAG, 使用 `flag &= ~XXX_FLAG`
	原因: 先对XXX_FLAG进行取反则XXX_FLAG原来非0的那一位变为0，然后使用`&`运算后如果flag变量非0的那一位变为0，则意味着flag变量不包含XXX_FLAG

### `#`baseIsEqual

上述的回调2方法中,就用位掩码操作,虽然在核心中不多,但是在完整的 lodash 中丰富的们拉掩码操作

当值不相等,并且 value 和 other 都 likeObject, 则需要深度比较, 而在第三个参数会传入 `COMPARE_PARTIAL_FLAG | COMPARE_UNORDERED_FLAG` 拉掩码, baseIsEqual lodash 解释的是 1 是无序比较, 2 是部分比较, 如果进行或运算就是 3, `#baseIsEqualDeep`,该方法很长,从接收到参数看起,该方法是可以接收6个参数的,对此处而言,将形参转换成如下:

1. 形参 object 作为比较源对象 value
2. 形参 other 作为比较目标对象 other
3. 形参 bitmask 作为比较时的位掩码 bitmask
4. 形参 sustomizer 无实参
5. 形参 equalFunc 作为进一步比较回调 baseIsEqual, 也就是当前函数本身,由此就可以看出, baseIsEqualDeep 一定会进行递归比较
6. 形参 stack 无实参

当 baseIsEqualDeep 所有形参从 baseIsEqual 传入,第一步处理参数,区分操作数是数组还是对象,如果是纯对象还需要用 toString 获取出具体是什么对象,直接比较出结果为止

确实很长, forEach 由私有方法 baseEach 生成, baseEach 是由私有方法`#createBaseEach(baseForOwn)` 和参数 `baseForOwn` 两个函数生成,生成 forEach 时,的迭代回调可以是自定义,如果没有则加用默认迭代回调,而默认回调基本上有三个,一个是 identity,
baseMatches, baseProperty 三个回调构成

如果当自定义迭代回调不存在，则会直接是 identity 返回本身值,如果如果是对象或者是类数组则会分别用 baseMatches 和 
baseProperty, baseMatches 可以深度比较，当然这里最重要的不是什么底层实现，也不是 equalArrays，equalByTag，equalObjects 其中的谁谁生成了谁,而是位掩码这种思想,关于 baseIsEqual 或是 baseIsEqualDeep 之后会详细分析源码


## Lodash.tap 

tap 属于一个链式调用方法,这里也是一个可以操作数组的方法, lodash 解释就调用一个拦截器并返回原值,作用就是为链式操作做准备,就像是一个请求拦截器,在请求完发送之前处理的方法,这个方法也是如果,但该方法可以这样说,是目前为止核心中最简单的一个方法,源码就是为一个值参数一个回调，然后返回作为参数的原值,代码层面上简单,但思想独特

```js
function _tap(value, interceptor){
  interceptor(value)
  return value
}

let bar = [1,2,3]
let foo = _tap(bar, function(array){
  array.pop()
})
console.log( bar === foo)
console.log(foo.pop()) //=> 2
```

## Lodash.thru

与 Lodash.tap 类似,其返回的并不是原值,而是被拦截器的返回值,如果一个原值是数组,被 thru, 拦截器返回一个对象,则 thru 最后就会返回对象

```js
let bar = [1,2,3]
function _thru(value, interceptor){
  return interceptor(value)
}
let foo = _thru(bar, function(array){
  return { 'bar' : array};
})
console.log( foo ) //=>{ bar: [ 1, 2, 3 ] }
```

*类似这样的方法还有很多,其实主要就是对函数或数据类型做一个包装,支持函数式的操作*

## Lodash.max && Lodash.min

max 寻找数组中最大的值,min 寻找数组中最小的值,想象有一串数字,不借用 max 方法找到最大的值,通常方法,遍历一次这串数字,取其中一个数字与所有数字进行比较,如果出现比该数字还大的数字则赋值成较大的数字,依次遍历到最后一个数字,从语言层面上来说就是循环或是递归,当然通常办法也只有这样。

下面就先来了解几个方法

### `#`baseGt

该方法接收两个参数,判断两个一个数是否大于另一个数

```js
function _baseGt (value, other) {
  return value > other
}
console.log(_baseGt(2, 1)) //=>true
```

### `#`baseLt

该方法接收也两个参数,判断两个一个数是否小于另一个数

```js
function _baseLt (value, other) {
  return value < other
}
console.log(_baseGt(2, 1)) //=> false
```

可能会有不解,为什么一个大于或是小于都要单独写个方法来,前面的比较两个数相等也要专门写个 eq 方法来比较直接用`==,>,<`这样的运算符它不香吗?

它还真有点不香,这个就要从表达式(expression)和语句(statement)说起,具体的话可参考 [ecma表达式规范](https://www.ecma-international.org/ecma-262/6.0/#sec-ecmascript-language-expressions) 和 [ecma语句规范](https://www.ecma-international.org/ecma-262/6.0/#sec-ecmascript-language-statements-and-declarations)

```js
function(){}//报错
(function(){})//不报错
function f(x){ return x + 1 }()//报错
function f(x){ return x + 1 }(1)//不报错，为什么返回 1
```

1. function 作为函数声明语句，而函数声明语句 function 关键字后面应该是函数名，这里后面跟圆括号，当然会报错
2. 给它加上一对圆括号，解析器会把`()`里的当做表达式去解析，在这里就会当做匿名函数表达式解析，所以不会报错
3. 在一条语句后面加上`()`会被当做分组操作符，分组操作符里必须要有表达式，所以这里报错
4. 在一条函数声明语句后面加上`(1)`，仅仅是相当于在声明语句之后又跟了一条毫无关系的表达式

函数式编程要求，只使用表达式，不使用语句。也就是说，每一步都是单纯的运算，而且都有返回值。
原因是函数式编程的开发动机，一开始就是为了处理运算（computation），不考虑系统的读写（I/O）。"语句"属于对系统的读写操作，所以就被排斥在外。
当然，实际应用中，不做I/O是不可能的。因此，编程过程中，函数式编程只要求把I/O限制到最小，不要有不必要的读写行为，保持计算过程的单纯性。[来自百度百科](https://baike.baidu.com/item/函数式编程/4035031?fr=aladdin)

*举个非常简单的例子,有许多数字需要进行大于比较,每两个数字之间进行比较就会产生一个操作, `>`或`<`操作,代码就是为了简便人们方便计算,简单的来说就是让人们偷懒,将多个操作换成一个操作不香吗?错误也会少很多呀,当然这只是个玩笑话。*

*函数的计算往往比指令的执行重要且函数的计算可随时调用*

### `#`baseExtremum

极值函数基函数,主要实现 Lodash.max 和 Lodash.min,如果说有了 baseGt 和 baseLt 让我来实现 baseExtremum 我会像下面这样去实现它

```js
function _baseLt(value, other) {
  return value < other
}
function _baseGt(value, other) {
  return value > other
}

function _max(array) {
  let i = 0,
      len = array.length,
      result = array[0]
  while (++i < len) {
    result = _baseGt(array[i], result) ? array[i] : result
  }
  return result
}
console.log(_max([2,3,4,8,5,2,6]))//=> 8
```

类似的 min 方法也可以这样得到,可`_baseGt`和`_baseLt`确实独立存在且是个纯函数,`_max`也独立存在,但是好像该方法除了算数组最大的值好像就没有其它用途了,与其这样还不如直接将方法内部的`_baseGt`直接写成`>`操作符,就当成一个普通的工具函数企不是一样的,也确实可以这样最大值,最小值,中间值...就分别写个方法嘛也是一样的,*能偷懒就偷懒有时候太老实也不是一件很好的事*

何不将一系列操作直接嵌套成一个函数调用呢,下面来看看 lodash 中的这个一操作的函数源码

```js
function baseExtremum(array, iteratee, comparator) {
  var index = -1,
      length = array.length;

  while (++index < length) {
    var value = array[index],
    //先走一遍迭代回调,可以将操作前对操作数作其它操作
        current = iteratee(value); 

    if (current != null && (computed === undefined
          ? (current === current && !false)
          : comparator(current, computed) // 一系列的操作
        )) {
      var computed = current,
          result = value;
    }
  }
  return result;
}
```

lodash 解释 主要生成 `_.max` 和` _.min` 这样的方法,用`comparator`确定极值,参数一是一个目标数组,参数二是一个迭代回调,之前的 each 等许多地方都用上了迭代回调,参数三就是这里的一系列操作

```js
function _baseExtremum(array, iteratee, comparator) {
  var index = -1,
      length = array.length;

  while (++index < length) {
    var value = array[index],
    //先走一遍迭代回调,可以将操作前对操作数作其它操作
        current = iteratee(value); 

    if (current != null && (computed === undefined
          ? (current === current && !false)
          : comparator(current, computed) // 一系列的操作
        )) {
      var computed = current,
          result = value;
    }
  }
  return result;
}
function _baseLt(value, other) {
  return value < other
}
function _baseGt(value, other) {
  return value > other
}
function _identity (value) {
  return value
}

function _max(array) {
  return (array && array.length)
    ? _baseExtremum(array, identity, _baseGt)
    : undefined;
}
function _min(array) {
  return (array && array.length)
    ? _baseExtremum(array, identity, _baseLt)
    : undefined;
}
console.log( _max([1,2,4,5,8,7,3]))//=> 8
console.log( _min([1,2,4,5,8,7,3]))//=> 1
```
借用`_baseExtremum`生成`_max`和`_min`,每个方法独立,从代码层面上看逻辑清晰,高可用,稳定每个方法都无副作用



## Lodash.prototype 原型数组方法

```js
console.log( Lodash.join([1,2,3,4], ',') ) //=> 1,2,3,4
console.log( [1,2,3,4].join(',') )//=> 1,2,3,4

console.log( '1,2,3,4'.split(',') )//=> [ '1', '2', '3', '4' ]
console.log( Lodash.split('1,2,3,4', ',') )//=> [ '1', '2', '3', '4' ]
```

第一个 lodash 成员都会有自己的属性,方法,这样的设计与大多数都一样,只是在 lodash 中,个人认为,大多地方不在是以数据为单位,就像 jQuery 以每个 dom 元素作为单位, lodash 以一个函数作为一个单位,主要围绕着函数

内部用私有方法 baseEach 为 pop, join, replace, reverse, split, push, shift, sort, splice, unshift 这些字符串数组方法扩展,其方法原型基本都是来自 String.prototype 或 Array.prototype



# Object

## `#`baseGetTag

jQuery 有一个`class2type`这样的一个内部属性,该属性利用`Object.prototype.toString()`方法,得到的对象字符串,用该字符串解析准确的对象类型, Lodash 也利用了该方法,只不过个人认为,Lodash 中不能以用 jQuery 的眼光看待 Object.prototype.toString

jQuery 中 class2type 是一个对象,该对象将 `Boolean Number String Function Array Date RegExp Object Error Symbol`等常见类型用 each 分别组合成 toString 结果填充到对象中

```js
// jquery 中的 class2type
let class2type = {};
"Boolean Number String Function Array Date RegExp Object Error Symbol"
  .split(" ")
  .map(function( name, i ) {
    class2type[ "[object " + name + "]" ] = name.toLowerCase();
  })
console.log(class2type)
// 最终 class2type 是一个关系映射表
/*
{ '[object Boolean]': 'boolean',
  '[object Number]': 'number',
  '[object String]': 'string',
  '[object Function]': 'function',
  '[object Array]': 'array',
  '[object Date]': 'date',
  '[object RegExp]': 'regexp',
  '[object Object]': 'object',
  '[object Error]': 'error',
  '[object Symbol]': 'symbol' }
*/
const type = function( obj ) {
  if ( obj == null ) {
      return obj + "";
  }
  return typeof obj === "object" || typeof obj === "function" ?
    class2type[ class2type.toString.call( obj ) ] || "object" :
    typeof obj;
}
console.log(type(/d/)) //=> regexp
console.log(type({})) //=> object
console.log(type(null)) //=> null
console.log(type([1,2,3])) //=> array

// 判断是否是一个数组
console.log( type([1,2,3]) === 'array') //=> true
```
而 Lodash 中以函数为单位,将每一个部分独立出来,作为和 jQuery 类似

```js
// lodash 中的 class2type
let argsTag = '[object Arguments]',
  arrayTag = '[object Array]',
  asyncTag = '[object AsyncFunction]',
  boolTag = '[object Boolean]',
  dateTag = '[object Date]',
  errorTag = '[object Error]',
  funcTag = '[object Function]',
  genTag = '[object GeneratorFunction]',
  numberTag = '[object Number]',
  objectTag = '[object Object]',
  proxyTag = '[object Proxy]',
  regexpTag = '[object RegExp]',
  stringTag = '[object String]';
let _nativeObjectToString = Object.prototype.toString;
function _objectToString(value) {
  return _nativeObjectToString.call(value);
}
function _baseGetTag(value) {
  return _objectToString(value);
}

function type (value) {
  return _baseGetTag(value)
}

function isObject(value) {
  var type = typeof value;
  return value != null && (type == 'object' || type == 'function');
}

function isRegExp(value) {
  return _baseGetTag(value) == regexpTag;
}

function isFunction(value) {
  // ...
  var tag = _baseGetTag(value);
  return tag == funcTag || tag == genTag || tag == asyncTag || tag == proxyTag;
}

console.log(type(/d/)) //=> [object RegExp]
console.log(type({})) //=> [object Object]
console.log(type(null)) //=> [object Null]
console.log(type([1,2,3])) //=> [object Array]

console.log( Array.isArray([2,3,4]))//=> true
console.log( isFunction(function* (){}))//=> true
console.log( isFunction(function (){}))//=> true
```

代码很长,但思维缜密,每个部分,每一个方法,属性可以说都是独立于整个类库中,如果说 isArray ES5 已经提供,那么 isFunction 就是只属于它自己的,baseGetTag 也是 Lodash.getTag 的基函数


## `#`baseValues

`Lodash.values`是由该方法生成的,Lodash.values 方法返回一个指定对象`可枚举`属性值的数组

```js
console.log( Lodash.values([1,2,3,4,5,6]) ) //=> [ 1, 2, 3, 4, 5, 6 ]
console.log( Lodash.values('hi') ) //=> [ 1, 2, 3, 4, 5, 6 ]
console.log( Lodash.values(100) ) //=> []
```

baseValues 的具体步骤,操作一得到指定对象的可枚举的属性,Object.keys 可直接获取,操作二 baseEach 遍历出可枚举属性的属性值,只是这个过程中加入了一个`baseMap`操作, baseMap 是实现 map 的基函数,但内部还是用 createBaseEach 完成,回调分别是键,值和当前遍历的对象

*baseMap 基本是由 baseEach 遍历的结果*

## `#`createAssigner 基函数

将 sources 中的对象的属性依次赋值给 target 对象, 同属性时会被覆盖,这里直接借 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 其特点

1. 可直接浅复制一个对象，symbol 类型也可以复制
2. 可合并对象
3. 继承属性和不可枚举属性是不能拷贝的
4. 原始类型会被包装为对象
5. 异常会打断后续拷贝

这里的主角是 lodash 不是 ES6 下面就来看看 lodash 中的 assign 

createAssigner 接收一个回调`assigner`,该回调主要用来为返回的函数形参 object 添加 rest 参数 sourcces 中对象的属性,也是生成`baseRest`这个私有方法的基函数,lodash 中可接收若干实参的函数基本离不开 baseRest 方法, assign 操作也会离不开 rest 参数所以 createAssigner 最后的返回是被 baseRest 包裹,实际上就是返回了一个 baseRest 方法,可对若干的源对象进行遍历操作

```js
// ...
// 为 assigner 回调形参为 3 个以上时提供自定义函数 customizer
customizer = (assigner.length > 3 && typeof customizer == 'function')
  ? (length--, customizer)
  : undefined;
// 包装成对象
object = Object(object);
while (++index < length) {
  // ...
  // 注意回调可接收四个参数
  assigner(object, source, index, customizer);
}
// ...
```

源码的 createAssigner 方法中最值得注意的地方有两处

1. assigner 回调可以接收四个参数,依次为 target 对象,当前复制的源对象 source, 遍历的索引 index, customizer
2. assigner 第四上参数 customizer, 该参数可以是一个函数, `Lodash.assignWith`中有解释,意思就是如果是以 assignWith 这样的方式进行属性复制则可以自定义复制值的最终结果,如果为函数时可以接收五个参数`objValue, srcValue, key, object, source`,可以看作是 createAAssigner 提供的一个钩子函数

### `#`copyObject 私有方法

上述的 createAssigner 回调 assigner, 在 Lodash.assign() 或 Lodash.assignIn() 方法指定的是 copyObject() 为 assigner, copyObject 的参数二也是直接用 Object.keys 或 for...in 遍历出来的所有属性名数组

```js
/**
 * Copies properties of `source` to `object`.
 *
 * @private
 * @param {Object} source The object to copy properties from.
 * @param {Array} props The property identifiers to copy.
 * @param {Object} [object={}] The object to copy properties to.
 * @param {Function} [customizer] The function to customize copied values.
 * @returns {Object} Returns `object`.
 */
function copyObject(source, props, object, customizer) {}
```
*该方法只能浅复制*

该方法就注意以下几点:
1. 空对象会被转换成 true
2. 如果存在 customizer 则会将五个参数传入回调得到最终的结果作为复制值
3. 当复制的对象 object 不为 `undefined`,`null`,`-0`,`0或+0`,`NaN`,`''`(空字符串)时会将属性当作一个关键字

`#baseAssignValue()`与`#assignValue()`方法的区别就是, baseAssignValue 是直接用中括号操作符进行赋值,而 assignValue 会进行一次`SameValueZero`的判断,如果不相等则会利用 baseAssignValue 赋值成对象的一个属性，下面则是`_createAssigner`实现过程

```js
function _eq(value, other) {
  return value === other || (value !== value && other !== other);
}
function _baseAssignValue(object, key, value) {
  object[key] = value;
}
function _assignValue(object, key, value) {
  var objValue = object[key];
  if (!(Object.prototype.hasOwnProperty.call(object, key) && _eq(objValue, value)) ||
      (value === undefined && !(key in object))) {
    _baseAssignValue(object, key, value);
  }
}

function _copyObject(source, props, object, customizer) {
  var isNew = !object;
  object || (object = {}); 
// `undefined`,`null`,`-0`,`0或+0`,`NaN`,`''` 其余都会为 true
  var index = -1,
      length = props.length;

  while (++index < length) {
    var key = props[index];
    // 自定义的结果值
    var newValue = customizer
      // 分别为五个参数
      // objValue, srcValue, key, object, source
      ? customizer(object[key], source[key], key, object, source)
      : undefined;

    if (newValue === undefined) {
      newValue = source[key];
    }
    if (isNew) {
      _baseAssignValue(object, key, newValue);
    } else {
      _assignValue(object, key, newValue);
    }
  }
  return object;
}

let a = {'name': 'qlover', 'age': 20}
let b =_copyObject(a, Object.keys(a), { 'c': 100 },
  function(objValue, srcValue, key, object, source){
    return 'preix_' + srcValue
})
console.log(a)//=> { name: 'qlover', age: 20 }
console.log(b)//=> { c: 100, name: 'preix_qlover', age: 'preix_20' }
```

assign,assignIn 和 assignWith 三者在 lodash 中的区别如下
1. assign 可枚举属性
2. assignIn 可枚举和不可枚举属性
3. assignWith 可枚举属性,并提供自定义 customizer

作一个补充:

```js
function baseRest(func, start) {
  return setToString(overRest(func, start, identity), func + '');
}
```
核心版本中`#setToString()`方法其实就是`#identity()`方法,这是因为在完整版本中 setToString 可重写对象的 toString 方法


## Lodash.assign

该方法松散的基于`Object.assign()`,作用很简单就是对象的复制,将所有可枚举属性的值从一个或多个源对象复制到目标对象,返回目标对象

assign 与 assignIn 不同,只会复制可枚举属性,也就是说像原型上不可枚举的属性是不会被复制的

但是凡是没有绝对,就比如当利用`JSON.parse()`方法解析一个包含有不可枚举的自身属性时,就是个例外,这里就不得不提一个叫`原型污染`的东西,原型污染就是当一个不可以枚举的属性被重新复制到了目标对象上引起的程序错乱,是前端的一种可利用攻击方式

```js
let o1 = { a: 1}
let o2 = {}

console.log(o1.a)//=> 1
console.log(o2.a)//=> undefined

Object.assign(o2, { b: 2, '__proto__': { a : 10}})

console.log(o1.a)//=> 1
console.log(o2.a)//=> undefined
```

利用`Object.assign()`方法对对象 o2 作一个扩展功能,如果`Object.assign()`方法将源对象的`__proto__`属性成功的重写,那么就会影响到所有继承 o2 原型的对象,但在这里还是很安全的并没有发生所期望的事情,下面则就不同了

```js
let o1 = { a: 1}
let o2 = {}

console.log(o1.a)//=>1
console.log(o2.a)//=> undefined

Object.assign(o2, JSON.parse('{ "b": 2, "__proto__": { "a" : 10}}'))

console.log(o1.a)//=> 1
console.log(o2.a)//=> 10
```

同样的是利用Object.assign方法,不同的是源对象是一个被 json 解析后的对象, json 解析后的对象有个特点,就是该对象是一个"纯"的对象,非常纯,纯到解析出来的属性全部都是可以枚举的属性

```js
let o = { b: 1}
console.log( Object.keys(JSON.parse('{ "b": 2, "__proto__": { "a" : 10}}')) )
//=> [ 'b', '__proto__' ]
console.log( Object.keys(o))
//=> [ 'b' ]
```
也就是说如果解析的属性名其原型或本身都会存在时,会丢失属性的修饰,从这里也就不难看出,不管是用 Object.assign 或是用基于 Object.assign 的 Lodash.assign 都会存在这样的一个问题

lodash 的解决方案就是利用`Object.prototype.hasOwnProperty()`和`in`运算符,`hasOwnProperty`和`in`都检测一个对象是否含有特定属性,但只有`in`不会忽略原型链上的属性,回到上述的`_baseAssignValue`和`_assignValue`,lodash核心版本中 Lodash.assign 是私有方法并没有暴露出来,是因为核心版本中的处理并非到达`_assignValue`方法内部,而完整版中`_assignValue`方法排除了这个可能,这是就是因为 assign 在核心版本中除了 null undefined 这些参数其它都会直接 baseAssignValue 直接赋值


# Function

## `#`baseRest ==^c2^==

当一个函数可以接收若干个参数,ES5 这之前都提供了一个叫 arguments 的类数组对象,不管是什么实参,一旦传入都会被 arguments 接收到,但 arguments 很特殊不能像通常数组一样使用,ES6 这之后加了一个新特性叫 rest 参数,可以做到像 python `*args` 这样的形参形式,可以接收若干实参的数组,而`Lodash.rest`可以理解为 arguments 到 rest 参数的过渡

如果想将数组元素迭代为函数参数,一般使用`Function.prototype.apply`的方式进行调用,也就是以数组方式传入参数,比如实现一个 sum 求和操作:

```js
function sum(nums){
  return nums.reduce( function( acc, v){
    return acc + v
  }, 0)
}

// 通常操作
;(function foo() {
  return sum(Array.from(arguments)) 
}).apply(null, [2,3,4])
// 9

// ES6 实现
;(function foo(...args) {
  return sum(args)
})(2,3,4)
// 9
```

为了实现像 ES6 这样的 rest 参数 lodash 是这样做的。

```js
function _overRest(func, start, transform) {
  // 取函数形参的第1个参数开始，将第一个参数后面的所有参数当作 rest 参数
  start = Math.max(start === undefined ? (func.length - 1) : start, 0);
  return function _overRest_return() { // 最终返回的函数，也是最终接收参数的地方
    // 重置 rest 参数
    var args = arguments,
        index = -1,
        length = Math.max(args.length - start, 0),
        array = Array(length); // 转换 arguments 的长度数组
    // arguments 转换成真数组
    while (++index < length) {
      array[index] = args[start + index]; // 转置 arguments 的元素到新数组中
    }
    index = -1;
    // 处理 args
    // 默认 start 为 1 也就是数组第二个元素
    // 默认就将 start 后所有的元素当作一个新的数组
    // start 后面的数组当作是回调的真 rest 参数
    // 而 start 前面的可以看作是回调 rest 参数的固定形参
    var otherArgs = Array(start + 1);
    while (++index < start) {
      otherArgs[index] = args[index];
    }
    otherArgs[start] = transform(array);
    console.log('s>', start, otherArgs)
    // args 最后作为参数返回给回调
    // 最后的 args 也是一个真数组
    return func.apply(this, otherArgs);
  };
}

function _identity(value){return value;}

function _baseRest(func, start){
  return _identity(_overRest(func, start, _identity));
}
function _rest(func, start){
  return _baseRest(func, start);
}
```

关键在于 start 参数, start 参数标识了传入实参与 rest 参数间的分隔,也就是将形参部分分成了已知和未知,未知就当作了 rest 参数,函数中应用了这一点,最后将参数传回回调,rest 就完成了

```js
let revice = _rest(function(name, car){
  console.log(name, car)
})
revice('qlover', 'BMW', 'Audi', 'Alpha') //=> qlover [ 'BMW', 'Audi', 'Alpha' ]

revice = function(name, ...car){
  console.log(name, car)
}
revice('qlover', 'BMW', 'Audi', 'Alpha') //=> qlover [ 'BMW', 'Audi', 'Alpha' ]
```

如何做到回调最后一个形参始终为 rest 参数从两点入手:

1. 参数 func 的形参个数
2. 返回函数`_overRest_return`的实参个数

当 start 为`func.length - 1`时,即表示为 func 最后一个形参,`_overRest_return`接收的实参(args)个数是多于 start 的,那么 args 的 start 后面的部分(`args.length - start`)则为 rest 参数

当 start 为指定第几个形参为最后一个形参时,也同样如此,指定 start 时,一定要分好 func 的形参避免传递不了参数,附上一个实例:

```js
lodash.rest(function(name, bmw, cars){
  console.log('cars is<', cars)
}, 2)('qlover', 'BMW', 'Audo', 'Alpha')
// cars is< [ 'Audo', 'Alpha' ]

lodash.rest(function(name, bmw, cars){
  console.log('cars is<', cars)
})('qlover', 'BMW', 'Audo', 'Alpha')
// rest is< [ 'Audo', 'Alpha' ]
```

完整版的 Lodash.spread 可以做 rest 逆运算 ES6 已经实现了该操作,并且还为该操作另添加了一个新语法,`...`运算符,该运算符不仅可以做剩余操作也可以做扩展操作,[详情可见](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

## `#`baseMap

返回一个遍历后的新数组,内部使用 baseEach 对参数 collection 进行遍历,同样的会将 value,key,object 三个参数作为形参传递给迭代回调,并且内部维护的`index`作为索引为新数组依次赋值为迭代回调的返回值

```js
function _baseMap(collection, iteratee) {
  var index = -1,
      result = isArrayLike(collection) ? Array(collection.length) : [];

  baseEach(collection, function(value, key, collection) {
    result[++index] = iteratee(value, key, collection);
  });
  return result;
}
```

## Lodash.sortBy

`Array.prototype.sort([compareFunction(firstEl, secondEl)])` 方法用原地算法对数组的元素进行排序，并返回数组。默认排序顺序是在将元素转换为字符串，然后比较它们的UTF-16代码单元值序列时构建的,`如果compareFunction`省略,元素按照转换为的字符串的各个字符的Unicode位点进行排序

[原地算法](https://en.wikipedia.org/wiki/In-place_algorithm)不依赖额外的资源或者依赖少数的额外资源，仅依靠输出来覆盖输入的一种算法操作,节省内存

以形参a和b为例:

1. compareFunction 返回值小于 0, a 排序到 b 前
2. compareFunction 返回值大于 0, a 排序到 b 后
3. compareFunction 返回值等于 0, a,b 位置不变

且 a和b的值都应该是同输入同结果,有点像纯函数,输入什么就会返回什么

下面以 `Array.prototyp.sort` 方法例子:

排列数组:
```js
console.log([9,3,-5,1,-2].sort())//=> [ -2, -5, 1, 3, 9 ]
console.log([9,3,-5,1,-2].sort( (x,y) => x - y ))//=> [ -5, -2, 1, 3, 9 ]
console.log([9,3,-5,1,-2].sort( (x,y) => x > y ? -1 : 1 ))//=> [ 9, 3, 1, -2, -5 ]
```

排列对象,排列对象的修改虽然不能直接用数组规则但可以用键的值比较:
```js
var users = [
  { 'car': 'bmw',  'money': 100 },
  { 'car': 'audi',  'money': 300 },
  { 'car': 'Alpha',  'money': 200 },
]

// 以 money 值升序排列
// console.log( users.sort( (x,y) => x.money - y.money ) )
/*//=>
[ { car: 'bmw', money: 100 },
  { car: 'Alpha', money: 200 },
  { car: 'audi', money: 300 } ]
*/
// 以 money 值升序降序
console.log( users.sort( (x,y) => x.money > y.money ? -1 : 1 ) )
/*//=>
[ { car: 'audi', money: 300 },
  { car: 'Alpha', money: 200 },
  { car: 'bmw', money: 100 } ]
*/
```

[详见](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

*该方法属于内排*

而 loadash 核心版本中也以`Array.prototype.srot()`为中心进行排序`#compareAscending()`是 loadash 默认的排序 compareFunction,默认以升序排列

```js
function _sortBy(collection, iteratee) {
  var index = 0;
  iteratee = _baseIteratee(iteratee);

  return _baseMap(_baseMap(collection, function(value, key, collection) {
    return { 'value': value, 'index': index++, 'criteria': iteratee(value, key, collection) };
  }).sort(function(object, other) {
    return _compareAscending(object.criteria, other.criteria) || (object.index - other.index);
  }), _baseProperty('value'));
}
```

sortBy 方法在对 collection 的类型排序时,会将 collection 遍历成一个对象组成的数组,每个对象赋与 index 唯一索引值,利用该索引值排序


# Lodash

## `#`baseCreate

### Object.create(ptoto[, propertiesObject])

不能以名称而去定义该方法的作用,它并不只是为了创建一个对象,其可以理解为"创建一个继承了指定对象的对象",并且创建后的对象是不存在原型对象,即`prototype`,相比 Object.assign 从 create 名称上来说到不如是用指定对象生成了一个新的对象,所以 Object.create 一个指定对象的新的实例对象,只不过这个指定的对象通常是原型对象

*ES5之前的版本不支持将参数 ptoto 设置为 null*

```js
function A(){}
A.prototype.name = 'AA'

let b = Object.create(A.prototype)
console.log(b.name) //=> AA
b.__proto__.name = 'BB'
console.log(A.prototype.name) //=> AA

let c = Object.assign({}, A.prototype)
console.log(c.name) //=> AA
c.__proto__.name = 'BB'
console.log(A.prototype.name) //=> BB
```

Lodash.`#baseCreate()` 私有方法也是实现了 Object.create 方法的作用,其源码如下:

```js
var baseCreate = (function() {
  // 准备全新的构造器
  function object() {}
  return function(proto) {
    if (!isObject(proto)) {
      return {};
    }
    // 如果支持 Object.create 则直接使用
    if (objectCreate) {
      return objectCreate(proto);
    }
    // 准备对象实例化原型为指定原型
    object.prototype = proto;
    var result = new object;
    // 丢掉返回对象的类原型对象
    object.prototype = undefined;
    return result;
  };
}());
```
## LodashWrapper

以下是摘录 lodash 与 LodashWrapper 部分源码:

```js
function _Lodash(){}
_Lodash.prototype.name = 'AA'
function _LodashWrapper(){}
console.log( (new _LodashWrapper()).name ) //=> undefined

// 指定 _LodashWrapper 原型只是以 _Lodash.prototype 对象创建出来的新对象
// 也就是说这个新对象只是 _Lodash.prototype 的一个实例对象,并不就是 _Lodash.prototype 这个原型对象本身
// 而这个对象的 __proto__ 指向的是 _Lodash.prototype 
// 也就是 _LodashWrapper.prototype 实例对象继承的 _Lodash 原型对象
_LodashWrapper.prototype = Object.create(_Lodash.prototype)
_LodashWrapper.prototype.constructor = _LodashWrapper

console.log( (new _LodashWrapper()).name ) //=> AA
_Lodash.prototype.age = 20
console.log( (new _LodashWrapper()).age ) //=> 20
console.log( _LodashWrapper.prototype.__proto__ == _Lodash.prototype ) //=> true
```

`_Lodash`与`_LodashWrapper`原型之间的关系,类比 jQuery 中 `init.prototype === jQuery.fn === jQuery.prototype` 三者关系,该构造器 lodash 的核心,链式调用也主要由该对象维护,下面是 lodash 构造器和 LodashWarpper 构造器源码:

```js
function lodash(value) {
  return value instanceof LodashWrapper
    ? value
    : new LodashWrapper(value);
}
function LodashWrapper(value, chainAll) {
  this.__wrapped__ = value;
  this.__actions__ = [];
  this.__chain__ = !!chainAll;
}
LodashWrapper.prototype = baseCreate(Lodash.prototype);
LodashWrapper.prototype.constructor = LodashWrapper;
```
LodashWrapper 可接收两个参数,`value`为实例的`__wrapped__`提供属性值, lodash 构造器接收的参数也会由 LodashWrapper 包裹,就是 lodash 工厂生产的对象,而 LodashWrapper 的原型是 lodash 原型 Lodash.prototype 的一个`实例对象`,并非是直接指向 lodash 原型,也就是说作为一个构造器或作为一个类 LodashWrapper 原型是 lodash 原型实例,那么 LodashWrapper 实例的`__proto__.__proto__`则是 lodash 原型

```js
let wrapper = new LodashWrapper('bmw')
console.log( wrapper.__proto__.__proto__ === Lodash.prototype ) //=> true
```

LodashWrapper 类在 lodash 核心版本中有三个属性它们分别是,`__chain__`、`__wrapped__`和`__actions__`,而完整版本中操作五个属性`__chain__`、`__wrapped__`、`__actions__`、`__index__`和`__values__`

### `__chain__` mixin 混入时指定

该属性描述了当前实例调用方法是否可以进行链式(seq)操作,类比 jQuery 的链式操作,jQuery 中是以方法返回的 this 为连接条件,loash 则是利用当前实例当前是否有`__chain__`属性作为连接条件

*在核心版本中可以在`mixin`方法中见到为提供 seq*

规定了某些方法是否可以或者不可以`chain`其实在内部 mixin 方法中混入时候已经决定,因为 mixin 这个方法本身而言,参数 options 是决定是否将方法 chain,其默认是开启的, 但 LodashWrapper 实例默认是未启用方法链调用的, mixin 方法是首先选择了前者,后才选择了后者,可以见其源码

```js
// ...
// 参数校验时是否添加为 seq 方法
var chain = !(isObject(options) && 'chain' in options) || !!options.chain,
    isFunc = isFunction(object);
// ...
var chainAll = this.__chain__; // 当前 LodasWrapper 实例显示指定的 __chain__ 属性
// 优先使用参数校验后的判断
// 再使用 chainAll 显示指定
console.log(methodName +' chain<'+ chain)
if (chain || chainAll) {
  // 确定启用 seq 调用
  var result = object(this.__wrapped__),
      /* 向 __actions__ 队列中添加操作记录 */
      actions = result.__actions__ = copyArray(this.__actions__);

  actions.push({ 'func': func, 'args': arguments, 'thisArg': object });
  result.__chain__ = chainAll;
  // 并且会返回一个新的 LodashWrapper 实例!!!
  return result;
}
// 非 seq 调用则直接普通函数调用
// 这也标识 seq 断开
return func.apply(object, arrayPush([this.value()], arguments));
// ...
```

所以在使用 mixin 方法的时候需要注意,是否要对混入到 seq 中,且如果混入 seq 方法最后返回的是一个全新的 LodasWrapper 实例

下面是 mixin 中添加一行 console 的打印结果试例:

```js
let wrapper = new LodashWrapper([2,3,4,1])
console.dir( wrapper.map(v => v + 10) )
// map chain<true
/*
LodashWrapper {
  __wrapped__: [ 2, 3, 4, 1 ],
  __actions__: [ { func: [Function: map], args: [Object], thisArg: [Function] } ],
  __chain__: false
}
 */

console.dir( wrapper )
/*
LodashWrapper {
  __wrapped__: [ 2, 3, 4, 1 ],
  __actions__: [],
  __chain__: false
}
 */

console.dir( wrapper.map(v => v + 10).max() )
// map chain<true
// max chain<false
//=> 14
```

当调用链式调用到 max 时 chain 是被 mixin 定义为 false,所以 max 就是当前 seq 的末尾

#### `__chain__` LodashWrapper 实例指定

那么如果当 chain 在 LodashWrapper 层面上是开启,如果当前 seq 已经到末尾,则不会向上面所述,直接普通函数调用返回结果

```js
let wrapper = new LodashWrapper([2,3,4,1], true)
console.dir( wrapper.map(v => v + 10).max() )
/*LodashWrapper {
  __wrapped__: [ 2, 3, 4, 1 ],
  __actions__: [
    { func: [Function: map], args: [Object], thisArg: [Function] },
    {
      func: [Function: max],
      args: [Arguments] {},
      thisArg: [Function]
    }
  ],
  __chain__: true
}*/
```

结果已经不再是 14 而是一个返回的 LodashWrapper 包裹对象,且`__actions__`记录的最后一次操作是 max,这之后可以一直进行 seq 操作,直到调用了 lodash 原型上的 value 或 valueOf 方法终结 seq, 而原型上的 value 是由`#baseWrapperValue()`生成

### `__wrapped__` -- `#baseWrapperValue`

该属性表现的形式为当作实例的操作的值,初始是直接接收构造传入的值,现在设`__chain__`为实例指定,那么
```js
let wrapper = new LodashWrapper([2,3,4,1], true)
console.dir( wrapper.map(v => v + 10).max().__wrapped__ )
//=> [ 2, 3, 4, 1 ]
```

`__wrapped__`竟然不是计算后的值,原因就是每一次 seq 操作后都会返回一个新的包裹对象,并且每一次传入构造的值都是初始值,直到我发现了`#baseWrapperValue`

seq 操作只是一个系列操作,如果末尾方法不是 chain 方法,则可以直接返回操作结果,但如果不是 chain 方法或者 LodashWrapper 是 chain 实例,不管是怎样的 seq 操作,每一步操作结果并非是记录的,但每一步操作是被`__actions__`队列维护

baseWrapperValue 从接收到初始化的值,就是最初传入的构造器的 value 开始,分别从`__actions__`队列中一条一条赋值结果,最后的 action 操作结果就是 baseWrapperValue 的返回最后的结果

以下是源码:

```js
function baseWrapperValue(value, actions) {
  var result2 = value;
  return reduce(actions, function(result, action) {
    return action.func.apply(action.thisArg, arrayPush([result], action.args));
  }, result2);
}
```

## chain

如果 seq 最后一个方法不是 chain 方法,则会直接返回最终的值,但如果实例化时的 LodashWrapper 对象显示指定是否 chain 则必须要等到 value 方法才会终结 seq,而上述是直接`new LodashWrapper()`,但是 lodash 中并没有暴露该包裹的构造器,对外的接口只有`lodash()`,lodash 构造器又不能显示指定是否 chain 对象

这个时候`Lodash.chain()`方法就起到了作用, chain 方法就是为 LodashWrapper 包裹对象指定显示 seq 的接口

```js
console.log(lodash([2,3,4,1])
  .map(v => v + 10)
  .sort()
  .max()
)
//=> 14
console.log(Lodash.chain([2,3,4,1])
  .map(v => v + 10)
  .sort()
  .max()
)
/*LodashWrapper {
  __wrapped__: [ 2, 3, 4, 1 ],
  __actions__: [
    { func: [Function: map], args: [Object], thisArg: [Function] },
    { func: [Function: tap], args: [Object], thisArg: [Function] },
    {
      func: [Function: max],
      args: [Arguments] {},
      thisArg: [Function]
    }
  ],
  __chain__: true
}*/
```

使用这个方法的时候需要注意的就是`原型`和`静态`都有这样的方法,核心版本中 lodash 原型上是没有 chain 方法的,只会将 chain 当作一个操作,以下摘录至 lodash 完整版本

```js
// function wrapperChain() {
//   return chain(this);
// }

// Add chain sequence methods to the `lodash` wrapper.
// lodash.prototype.chain = wrapperChain;

Lodash.prototype.chain = function(){
  return Lodash.chain(this)
}
console.log(lodash([2,3,4,1])
  .chain()
  .map(v => v + 10)
  .sort()
  .max()
  .value()
)
// => 14
```

完整版中 lodash 赋于了其原型上的 chain

## Lodash.mixin

方法可接收三个参数,参数一是目标对象或函数和参数三是可加选项对象,其中的`chain`标识是否可以链式(seq)调用,参数一和三是可选,参数二是必选,像极了 jQuery.extend 方法

1. 只会混入方法不会混入属性
2. 目标对象只要是一个对象或是一个函数就会为其混入一个静态方法
3. 其原型会混入一个新的复制方法

lodash 解释:

>Adds all own enumerable string keyed function properties of a source object to the destination object. If `object` is a function, then methods are added to its prototype as well.

将源对象的所有可枚举字符串键控函数属性添加到目标对象。如果“object”是一个函数，那么方法也会添加到它的原型中。以下是几个利用 mixin 的例子:

### 默认混入到 lodash 中
```js
function _Lodash(){ }
_Lodash._assignIn = '_assignIn'
_Lodash._before = function _before(){}

// 默认混合到 lodash 中
Lodash.mixin(_Lodash);
console.log( _Lodash.prototype._assignIn ) //=> undefined
console.log( _Lodash.prototype._before ) //=> undefined
console.log(Lodash._before) //=> [Function: _before]
console.log(Lodash._assignIn) //=> undefined
console.log(Lodash.prototype._before) //=> [Function]
```

属性并没有添加到 lodash 中,但是方法`_before`混入到了 lodash 原型和静态中,原型上失去原函数名

### 混合到指定函数中
```js
// 混合到函数中
Lodash.mixin(_Lodash, _Lodash);
console.log( _Lodash.prototype._assignIn ) //=> undefined
console.log( _Lodash.prototype._before ) //=> [Function]
console.log(_Lodash._before) //=> [Function: _before]
```

保留了原函数且混入到了`_Lodash`构造器与原型上

### 混入到指定普通对象中
```js
// 混合到普通对象中
var obj = {}
Lodash.mixin(obj, _Lodash);
console.log( _Lodash.prototype._assignIn ) //=> undefined
console.log( _Lodash.prototype._before ) //=> undefined
console.log(Lodash._before) //=> undefined
console.log(obj._before) //=> [Function: _before]
console.log(obj.prototype) //=> undefined
```
普通对象原型不存在,无法混入原型只能混入到静态中

### 混入普通对象时携带其它对象
```js
// 尝试混入普通函数时携带其它对象
Lodash.mixin(_Lodash, _Lodash, { '_chain': true});
console.log( _Lodash.prototype._assignIn ) //=> undefined
console.log( _Lodash.prototype._before ) //=> [Function]
console.log(Lodash._before) //=> undefined
console.log(_Lodash)
/* [Function: _Lodash] {
  _assignIn: '_assignIn',
  _before: [Function: _before]
} */
console.log(_Lodash.prototype)
// => _Lodash { _before: [Function] }
```

*源码中的 mixin() 前两个参数都指定是,是因为在 lodash 中 mixin 是独立调用的,如果此时是严格模式,则 this 是 undefined*

有了这三点进入 mixin 就轻松很多了,以下是 mixin 源码部分:

```js
function mixin(object, source, options) {
  var props = keys(source),
      /* 过滤掉非函数的属性 */
      methodNames = baseFunctions(source, props);
  // 参数移位重构形参与实参
  if (options == null &&
      !(isObject(source) && (methodNames.length || !props.length))) {
    options = source;
    source = object;
    object = this;
    methodNames = baseFunctions(source, keys(source));
  }

  // ... 是否需要链式调用该方法
  
  // 依次为指定对象赋值方法
  baseEach(methodNames, function(methodName) {
    var func = source[methodName];
    object[methodName] = func;
    if (isFunc) {
      object.prototype[methodName] = function() {
        // ... 链式添加
        return func.apply(object, arrayPush([this.value()], arguments));
      };
    }
  });

  return object;
}
```

*Lodash.mixin 只会混入方法不会混入属性*

# 参考链接

- [Lodash eq](https://www.cnblogs.com/hefty/p/8190969.html)
- [MDN NaN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN)
- [位运算位掩码](https://blog.csdn.net/li0978/article/details/100714987)
- [js 表达式与语句](https://www.cnblogs.com/xianshenglu/p/8386918.html)
- [ES2015 表达式与语句规范](https://www.ecma-international.org/ecma-262/6.0/#sec-ecmascript-language-expressions)
- [ES 6 规范](http://www.ecma-international.org/ecma-262/6.0/index.html)
- [ES 7 规范](http://www.ecma-international.org/ecma-262/7.0/index.html)
- [ES 8 规范](http://www.ecma-international.org/ecma-262/8.0/index.html)
- [ES 9 规范](http://www.ecma-international.org/ecma-262/9.0/index.html)
- [ES 10 规范](https://www.ecma-international.org/ecma-262/10.0/index.html)
- https://tc39.es/ecma262
- [深入理解 JavaScript Prototype 污染攻击](https://www.leavesongs.com/PENETRATION/javascript-prototype-pollution-attack.html)