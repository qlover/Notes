
## `#getIteratee` ==^c3^==

核心思想方法之一, Lodash 中只要有关于回调的地方,都由该方法处理

其中涉及`#iteratee`和`#baseIteratee`两个主要的方法,`#iteratee`也是`lodash.iteratee`

1. 参数一,要转换为迭代器的值
2. 参数二,迭代器的参数个数(该参数为抽象参数)

### 迭代器的参数个数

首先先看参数二, getIteratee 源码中`result`为最后得到的内部处理回调方法,都为其传递了参数二`arguments[1]`,但是不管 baseIteratee 还是 iteratee 都并没有接收参数二

下面列出几个有关该参数的方法:

```js
// lodash.reduce
function reduce(collection, iteratee, accumulator) {
  // ...
  getIteratee(iteratee, 4)
}

// lodash.map
function map(collection, iteratee) {
  // ...
  getIteratee(iteratee, 3)
}

// lodash.pullAllBy
function pullAllBy(array, values, iteratee) {
  // ...
  getIteratee(iteratee, 2)
}
```

上述中只例出了如何调用 getIteratee 分别介绍一下:
- `reduce(collection, iteratee, accumulator)` 将 collection 减小为一个值，该值是通过 iteratee 运行 collection 中的每个元素的累加结果，其中为每个连续的调用提供前一个的返回值。如果未指定累加器，则将集合的第一个元素用作初始值。使用四个参数调用iteratee 分别是:累加器，值，索引|键，集合

- `map(collection, iteratee)` 通过运行 iteratee 中的集合中的每个元素来创建值数组。使用三个参数调用iteratee 分别是:值，索引|键，集合自身

- `pullAllBy(array, values, iteratee)` 此方法类似于_.pullAll，不同之处在于它接受为数组和值的每个元素调用的iteratee，以生成比较它们的条件。使用一个参数调用iteratee ：（值）


下面使用上述几个方法来几个实例:

```js
console.log( 
  lodash.reduce([1,2,3,4,5], (result, value, key, array) => {
    return result + value
  }, 0)
)//=> 15

console.log( 
  lodash.map([5, 4], (item, key, array) => item * item)
)
//=> [ 25, 16 ]

let array = [{ 'x': 1 }, { 'x': 2 }, { 'x': 3 }, { 'x': 1 }]
lodash.pullAllBy(array, [{ 'x': 1 }, { 'x': 3 }], 'x')
console.log(array)
// => [ { x: 2 } ]

array = [{ 'x': 1, 'y': 2 }, { 'x': 3, 'y': 4 }, { 'x': 5, 'y': 6 }]
lodash.pullAllWith(array, [{ 'x': 3, 'y': 4 }], lodash.isEqual)
console.log(array)
// => [ { x: 1, y: 2 }, { x: 5, y: 6 } ]
```

仔细发现上面的的例子,特别是回调函数部分,比如 map 方法的回调调用时,其可以有三个形参,我们使用的时候大多部分都只使用第一个参数,再比如说 pullAllWith 方法,使用的 lodash.isEqual 方法可以接收两个形参用来作比较,还有就是回调如 pullAllBy 使用时只给了一个字符串`x`,其回调不管是什么类型,其结果总是意料之中,主要还是因为 getIteratee 方法做的转换

最主要的还是调用 getIteratee 时,参数二好像与实际回调可接收形参个数一致,所以初步可以确定,参数二就是为了规定实际回调可以接收多少个参数

### `#`baseIteratee

重点在于,无论什么样的值传递给 getIteratee 都会返回一个函数,这件事就是由 baseIteratee 完成,以下是传递给该方法对应参数类型返回的函数作用:

1. 函数直接返回
2. 值(path),返回具有记忆的函数,该函数返回的函数接收的实参对象的 path 值
3. 数组,元素一当作 path,元素二当作指定比较的值
  - 如果元素一是 key,返回函数用于比较指定对象该 key 值与元素二相等性
  - 如果元素一是 path,返回函数用于判断指定对象是否存在该路径值
4. 通常对象

```js
const odd = v => v % 2 == 0
const max = (a,b) => Math.max(a,b)
console.log( lodash.getIteratee(odd, 1)) // [Function: odd]
console.log( lodash.getIteratee('a', 3)) // [Function]
console.log( lodash.getIteratee(10, 2)) // [Function]
console.log( lodash.getIteratee(/d/, 2)) // [Function]
console.log( lodash.getIteratee({a:1}, 2)) // [Function]
console.log( lodash.getIteratee(max, 2)) // [Function: max]
```
getIteratee 首先将值传递给 iteratee,而 iteratee 方法会根据得到值判断是否是函数,如果是函数则直接会传递该函数给 baseIteratte ,如果不是函数,则会利用`#baseClone`克隆一份值传递给 #baseIteratte,所以无论什么参数都会交给`#baseIteratee`处理,它才是回调处理者


#### 值的转换 `#`basePropertyDeep 获取属性访问路径值

该方法处理传递给 baseIteratee 的参数是一个 path 值的情况,将该 path 记忆给返回函数实参对象的访问路径,做一个属性访问操作,与 Lodash.result 方法接收的参数一致

如果值是 key 会直接使用 `#baseProperty` 取对象 key 值,则返回函数该该 key 值,指定对象键值:

```js
let users = [
  { money: [100, 500], name: 'Qlover' },
  { money: [500, 50], name: 'Lee' },
  { money: [350, 100], name: 'Fred' }
]
// `#baseProperty`
const baseProperty = key => obj => obj ? obj[key] : undefined

// 传递给 iteratee 的值是 money|name 是一个 key 返回一个回调
const getMoney = baseProperty('money')
const getName = baseProperty('name')

// 返回的回调,给定对象就返回对象的 key
// 这个 key 是被记忆起来的为 money
console.log( getMoney(users[1]) )
//=> [ 500, 50 ]

console.log( getName(users[2]) )
//=> Fred
```

*`#castPath` 方法也是一个核心思想方法之一,管理属性访问路径*

*`#basePropertyOf`与`#baseProperty`相反其,记忆的是对象*

如果值是 path 会使用路径访问,下面是以 Lodash.result 方法为例:

```js
const basePropertyDeep = path => obj => lodash.result(obj, path)

console.log( basePropertyDeep('[1].money')(users) )
//=> [ 500, 50 ]
console.log( basePropertyDeep('[2].name')(users) )
//=> Fred
```

`#property`方法将 key 和 path 情况做了统一,以上就是当 getIteratee 接收的参数是一个值时的处理流程

#### 数组的转换 `#`baseMatchesProperty

该方法处理传递给 baseIteratee 的参数是一个数组的情况, baseIteratee 会将该数组的元素一和二分别传递给 baseMatchesProperty 当作 `path` 和`比对值`,其方法内部和上述的值的转换类似,无论 path 是一个单独的 key 还是是路径都会返回一个记忆了该 path 值的函数,但不是做属性访问操作,而是做比对值,与接收的参数二,也就是数组的第二个元素做比较

key 恒等比较[Strict Equality Comparison](http://ecma-international.org/ecma-262/6.0/#sec-strict-equality-comparison), path 做 baseIsEqual 的无序(Unordered)和部分(Partial)比较,下面看一段例子:

```js
let users = [
  { money: [100, 500], name: 'Qlover' },
  { money: [500, 50], name: 'Lee' },
  { money: [350, 100], name: 'Fred' }
]

// 数组元素一是一个 key 
let iteratee = lodash.getIteratee([ 'name', 'Lee' ], 2)
console.log(iteratee) //=> [Function]

// 比对值
console.log( iteratee(users) ) //=> false
console.log( iteratee(users[1]) ) //=> true

// 比对值
// users[0] { money: [100, 500], name: 'Qlover' }
let path = "[0].money"
iteratee = lodash.getIteratee([ path, [100, 500]], 2)
console.log( iteratee(users) ) //=> true

// 判断操作
// 还有一种就是当元素二没有时,会做
path = "[1].age"
iteratee = lodash.getIteratee([ path ], 2)
console.log( iteratee(users) ) //=> false
// 这里显然 users[1] 没有 age 属性
// 且数组只有一个元素时时候始终为 false
```

#### 通常对象的转换 `#`baseMatches

这也是其余任何类型时的处理方式, baseMatches 方法只接收一个参数, Lodash.matches 由该方法生成,做对象的比较和核心版本中的一致,都可以做深度的比较

但是完整版本中 baseMatches 实现方式不同,这其中有一个地方可以用来借鉴,因为对象比较复杂,深度有的会比较深,如果是不对象使用深度比较是一极度消耗资源的, Lodash 做了一个中间处理`#getMatchData`方法,该方法会将接收的对象转换成一个由二维数组组成的结构,其中每一个元素的索引0-2分别是:键、值、比较标识,比较标识就是用来区分当前元素是否需要进行深度比较的标识

以下是部分源码与实例:

```js
function getMatchData(object) {
  // ...
  while (length--) {
    var key = result[length],
        value = object[key];
    // 键 值 比较标识排除了 NaN
    result[length] = [key, value, isStrictComparable(value)];
  }
  return result;
}

function baseMatches(source) {
  var matchData = getMatchData(source);
  if (matchData.length == 1 && matchData[0][2]) {
    return matchesStrictComparable(matchData[0][0], matchData[0][1]);
  }
  return function(object) {
    // baseIsMatch 包含了 baseIsEqual 深度
    return object === source || baseIsMatch(object, source, matchData);
  };
}

function baseIsMatch(object, source, matchData, customizer) {

  // 对数据进行验证
  while (index--) {
    // ...
  }
  while (++index < length) {
    // ...
    
    // 当标识为 true 也就是原始值时 不做深度比较
    if (noCustomizer && data[2]) {
      // 直接比较
      if (objValue === undefined && !(key in object)) {
        return false;
      }
    } else {
      // 深度比较
      var stack = new Stack;
      // ...
      if (!(result === undefined
            ? baseIsEqual(srcValue, objValue, COMPARE_PARTIAL_FLAG | COMPARE_UNORDERED_FLAG, customizer, stack)
            : result
          )) {
        return false;
      }
    }
  }
  return true;
}


let users = [
  { money: [100, 500], name: 'Qlover' },
  { money: [500, 50], name: 'Lee' },
  { money: [350, 100], name: 'Fred' }
]

let iteratee = lodash.getIteratee( { money: [500, 50], name: 'Lee' } , 2)
console.log(iteratee(users[0]))//=> false
console.log(iteratee(users[1]))//=> true
```

下面附上核心版本中的方法源码:

```js
function baseMatches(source) {
  var props = nativeKeys(source);
  return function(object) {
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




## `#`castPatah ==^c4^==

核心思想方法之一, Lodash 中只要有关于属性访问路径的地方,都由该方法处理路径得到路径数组



















## Lodash.result

相比核心版本中的 result 方法完善了许多,并支持了字符串路径选择,方法核心思想有以下两点:

1. 将路径字符串拆分成数组,每一个访问都会变成一个元素
2. 遍历路径数组依次取出该路径值,这期间会将上一次的值记录

直接附上一个例子:
```js
let obj = {
  a:[ 1, { b: { d: 20 }, c: [
    2, {
      func: function(){
        return 'qlover'
      }
    }
  ] }],
  e: 200
}

console.log( lodash.result(obj, 'a[1].c[1].func') )
// => qlover

function getByPath (object, pathArray, defaultValue) {
  let index = -1,
      length = pathArray.length,
      value,

  while ( ++index < length) {
    if( !(value = object[pathArray[index]]) ) {
      return defaultValue
    }
    object = typeof value === 'function' ? value() : value
  }
  return object
}

console.log( getByPath(obj, ['a','1', 'c', '1', 'func']) )
// => qlover
console.log( getByPath(obj, ['a','1', 'c', '1', 'funcd'], 'not') )
// => not
console.log( getByPath(obj, ['a','1', 'cc', '1', 'funcd'], 'not') )
// => not
```

Lodash.result 源码:

```js
function result(object, path, defaultValue) {
  path = castPath(path, object);
  var index = -1,
      length = path.length;

  // Ensure the loop is entered when path is empty.
  if (!length) {
    length = 1;
    object = undefined;
  }
  while (++index < length) {
    var value = object == null ? undefined : object[toKey(path[index])];
    if (value === undefined) {
      index = length;
      value = defaultValue;
    }
    object = isFunction(value) ? value.call(object) : value;
  }
  return object;
}
```

参数 path 可以是字符串或直接是路径表示的数组,如果是字符串会被`#castPath`方法转换成路径数组

当解析的值是函数时,会使用其父对象的 this 绑定调用该函数,并返回其结果,这一点与`Lodash.get`的唯一不同点

当未找到值时,注意 Lodash 的作法,并不是直接返回,而是将 index 赋值成 length 来达到结束循环

*castPath 方法是由 memoize 高阶函数记忆生成,此处暂不考虑*

## 


## 参考链接

- [十个可以用ES6替代的Lodash特性](http://blog.sollrei.me/post/frontend/2018-12-06)
- [MDN 扩展语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)


## TODO

- getIteratee
- SetCache
- arrayIncludes
- arrayIncludesWith
- cacheHas
- baseTimes
