# Lodash 数据结构

如果熟悉 Java 就知道它的数据结构树是很完善的,就比如 Map 来说, Java 提供一个 Map 接口,HashMap,TreeMap 等等都是实现了 Map 接口的一种数据结构, 像 Properties 又是 HashTable 的实现子类,整个数组结构树是完善的, 相比 JavaScript Set,List,Map 这样的数据结构支持并不完善,也只有到了 ES6 之后才一个一个逐渐加入规范

## Hash

目前好像 ECMA 还没将 HASH 加入规范,至少到今天的 ES10 好像我也没有发现有 HASH 实现的规范

>Creates a hash object

创建一个哈希对象

哈希由数组+链表组成,根据关键码值(Key value)而直接进行访问的数据结构。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做散列函数，存放记录的数组叫做散列表......

1. 构造器可接收二维数组组成的键值对的数组
2. 提供 get,set,delete,clear,has 接口
3. 键可以任意类型,包括空, 这一点是因为内部用对象构成,对象能可有什么键其就可有什么键
4. 键不能重复,同键的值可以覆盖

话不多说先直接看 Loash 的 Hash 类:
```js
/** Used to stand-in for `undefined` hash values. */
var HASH_UNDEFINED = '__lodash_hash_undefined__';

/* Built-in method references that are verified to be native. */
var nativeCreate = Object.create;

/** Used to check objects for own properties. */
var hasOwnProperty = Object.prototype.hasOwnProperty;
    
/**
 * Creates a hash object.
 *
 * @private
 * @constructor
 * @param {Array} [entries] The key-value pairs to cache.
 */
function Hash(entries) {
  var index = -1,
      length = entries == null ? 0 : entries.length;

  this.clear();
  while (++index < length) {
    var entry = entries[index];
    // 这里可以看每一个键值是由数组组成
    // 元素1是键,元素2是值
    this.set(entry[0], entry[1]);
  }
}

Hash.prototype = {
  constructor: Hash,
  clear : function hashClear() {
    this.__data__ = nativeCreate ? nativeCreate(null) : {};
    this.size = 0;
  },
  'delete' : function hashDelete(key) {
    var result = this.has(key) && delete this.__data__[key];
    this.size -= result ? 1 : 0;
    return result;
  },
  get : function hashGet(key) {
    var data = this.__data__;
    if (nativeCreate) {
      var result = data[key];
      return result === HASH_UNDEFINED ? undefined : result;
    }
    return hasOwnProperty.call(data, key) ? data[key] : undefined;
  },
  has : function hashHas(key) {
    var data = this.__data__;
    return nativeCreate ? (data[key] !== undefined) : hasOwnProperty.call(data, key);
  },
  set : function hashSet(key, value) {
    var data = this.__data__;
    this.size += this.has(key) ? 0 : 1;
    data[key] = (nativeCreate && value === undefined) ? HASH_UNDEFINED : value;
    return this;
  }
}
```

下面是 Hash 实例:

```js
// 可接收一个数组组成的键对的数组
let hash = new lodash.Hash([['money', 100], ['age', 20]])
console.log( hash.get('money') ) //=> 100
console.log( hash.get('age') ) //=> 20

// 也可以是一个空的 hash
let hash2 = new Hash
```

## ListCash

> Creates an list cache object

创建一个列表缓存对象

1. 构造器可接收二维数组组成的键值对的数组
2. 提供 get,set,delete,clear,has 接口
3. 键可以任意类型,包括空
4. 键不能重复,同键的值可以覆盖

只是注意,虽然是以数组为主存放,但存入的键值还是以数组,每一个数组组成键和值,

```js
let list = new lodash.ListCache([['money', 100], ['age', 20]])
// console.log( list.__data__ )
//=> [ [ 'money', 100 ], [ 'age', 20 ] ]
console.log( list.get('age') )
//=> 20
```

那么如何使用 get 方法从二维数组中定位到指定的键值组成的元素呢?其实不就是每个数组的索引为0的元素吗,只取键的时候需要对二维去进行验证,这个验证的方法就是`#assocIndexOf`私有方法,它会返回指定 key 在的二维数组中的元素的索引,下面附加该方法源码:

```js
function assocIndexOf(array, key) {
  var length = array.length;
  while (length--) {
    if (eq(array[length][0], key)) {
      return length;
    }
  }
  return -1;
}
```

最后的值就是这个数组的第二个元素,值得注意的是,虽然 List 数据结构允许重复,但是 Lodash 中 ListCache 是用来作缓存,显然这样的设计是不能出现重复的元素,如果出现重复 Lodash 会覆盖

```js
list.set('money', 500)
console.log( list.get('money'))
//=> 500
```


## MapCash

> Creates a map cache object to store key-value pairs

创建地图缓存对象以存储键值对

1. 构造器可接收二维数组组成的键值对的数组
2. 提供 get,set,delete,clear,has 接口
3. 键可以任意类型,包括空,但是会区分出字符串,对象和原始值三种类型存放
4. 键不能重复,同键的值可以覆盖

>Creates a map cache object to store key-value pairs

创建一个缓存键值对的 Map

```js
let map = new lodash.MapCache([
  ['money', 100], [55, 500], [ [1,2], 88], 
  [ {a:1}, 99], [ {b:1}, 999]
])
console.log( map.__data__ )
/*
{ hash:
   Hash { __data__: [Object: null prototype] { '55': 500 }, size: 1 },
  map: Map { [ 1, 2 ] => 88, { a: 1 } => 99, { b: 1 } => 999 },
  string:
   Hash { __data__: [Object: null prototype] { money: 100 }, size: 1 } }
 */
```

MapCache 不再是像 ListCache 只用一个数组存放所有的类型数据,而是分成了 hash, map, string 三种情况,`__data__`.hash 区分通常的哈希,`__data__`.map 区分键是对象情况,使用的数据结构优先为 ES6 的 Map,不支持则会选择 ListCahe, 而 `__data__`.string 和`__data__`.hash 一样,只是将字符串单独区分出来

以下是当不支持 Map 时的替换方式:
```js
/*
{ hash:
   Hash { __data__: [Object: null prototype] { '55': 500 }, size: 1 },
  map:
   ListCache { __data__: [ [Array], [Array], [Array] ], size: 3 },
  string:
   Hash { __data__: [Object: null prototype] { money: 100 }, size: 1 } }
*/
```

当 MapCache 有以上三种`__data__`的情况,使用`#getMapData`方法获取,并且因为存放的数据键值可以是原始值,可以是字符串还可以是对象,首先要先保证键是有效的键,比如如果键为 null 或者为`__proto__`关键字则不行,且要保证每个键的唯一,这个时候还需要借助`#isKeyable`判断值是否是一个有效的键值,下面列出两个方法的源码:
```js
function isKeyable(value) {
  var type = typeof value;
  return (type == 'string' || type == 'number' || type == 'symbol' || type == 'boolean')
    ? (value !== '__proto__')
    : (value === null);
}
function getMapData(map, key) {
  // MapCache 有三种 data 
  var data = map.__data__;
  return isKeyable(key)
    // hash,string data
    ? data[typeof key == 'string' ? 'string' : 'hash']
    // map data
    : data.map;
}
```

## SetCashe

> Creates an array cache object to store unique values

创建一个数组缓存对象以存储唯一值

该结构可以看是 MapCache 的子类实现,其内部存储完全是按照 MapCache,只是显露的接口是 add 不是 set,且只有 add,push 和 has 两个接口

1. 构造器可接收键组成的数组
2. 提供 add,push,has 接口
3. 键可以任意类型,包括空,但是会区分出字符串,对象和原始值三种类型存放
4. 键不能重复


SetCache 实现的是 Set 接口, Set 具有唯一不重复,也就是 SetCache 并不是存放的键值对,而是键

```js
let obj = { x: 2, y: 1 }
let set = new lodash.SetCache([ obj ])
console.log( set.__data__.size ) //=> 1
console.log( set.has(obj) )
set.push(obj) // 尝试再次添加 obj 键
console.log( set.__data__.size ) //=> 1
// 尝试添加一个不同的键
let arr = [1,2,3]
set.push(arr)
console.log( set.__data__.size ) //=> 2
console.log( set.has(arr) ) //=> true
```

但是 MapCache 是存放的键值对, SetCache 只存放键,所以这里的每一个值都为前面的那个空值时的常量`HASH_UNDEFINED`,下面附上其构造器:
```js
/**
 *
 * Creates an array cache object to store unique values.
 *
 * @private
 * @constructor
 * @param {Array} [values] The values to cache.
 */
function SetCache(values) {
  var index = -1,
      length = values == null ? 0 : values.length;

  this.__data__ = new MapCache;
  while (++index < length) {
    this.add(values[index]);
  }
}
SetCache.prototype.add = SetCache.prototype.push = function setCacheAdd(value) {
  // 每一次值都为空
  this.__data__.set(value, HASH_UNDEFINED);
  return this;
}
SetCache.prototype.has = function setCacheHas(value) {
  return this.__data__.has(value);
}
```

## Stack

>Creates a stack cache object to store key-value pairs

创建一个堆栈缓存对象以存储键值对

1. 构造器可接收二维数组组成的键值对的数组
2. 提供 get,set,delete,clear,has 接口
3. 键可以任意类型,包括空
4. 键不能重复,同键的值可以覆盖

Stack 唯一与 ListCache 不同的是, Stack 将`__data__`包装了成了 ListCache,

如果在调用 set 方法是,自身的`__data__`超过了常量`LARGE_ARRAY_SIZE`所设置的 200 个元素,则会使用 MapCache 来代替 ListCache,但是,在初始化时并不会有该限制

```js
let common = [
  ['money', 100], [55, 500], [ [1,2], 88], [ ,'qlover'],
  [ {a:1}, 99], [ {b:1}, 999], [ Symbol('age'), 300], [null, 99], [ true , 88]
]
let arr = []
for(let i = 0; i < 210; ++i){
  arr.push([i, 10+i])
}
let stack = new lodash.Stack(arr)
console.log( stack.__data__ instanceof lodash.ListCache ) 
//=> true
stack.set(999, 100)
console.log( stack.__data__ instanceof lodash.MapCache ) 
//=> true
```

Stack 还不至这样,还有更多奇特的地方,如果自身的`__data__`不是 ListCache 实例时,在调用 set 方法时会调用指定实例 set 方法,但是如果没有实例 set 那就另说了

```js
let stack = new lodash.Stack()
stack.__data__ = new lodash.Hash()
stack.set('money', 100)
console.log( stack.__data__ instanceof lodash.MapCache )
//=> false
console.log( stack.__data__ instanceof lodash.Hash )
//=> true

stack.__data__ = new lodash.SetCache()
stack.set('age', 20)
console.log( stack.__data__)
// TypeError: data.set is not a function
```

下面附上 Stack 源码:
```js
/** Used as the size to enable large array optimizations. */
var LARGE_ARRAY_SIZE = 200;

/**
 * Creates a stack cache object to store key-value pairs.
 *
 * @private
 * @constructor
 * @param {Array} [entries] The key-value pairs to cache.
 */
function Stack(entries) {
  var data = this.__data__ = new ListCache(entries);
  this.size = data.size;
}

Stack.prototype = {
  constructor: Stack,
  clear: function stackClear() {
    this.__data__ = new ListCache;
    this.size = 0;
  }
  'delete': function stackDelete(key) {
    var data = this.__data__,
        result = data['delete'](key);

    this.size = data.size;
    return result;
  }
  get: function stackGet(key) {
    return this.__data__.get(key);
  }
  has: function stackHas(key) {
    return this.__data__.has(key);
  }
  set: function stackSet(key, value) {
    var data = this.__data__;
    if (data instanceof ListCache) {
      var pairs = data.__data__;
      if (!Map || (pairs.length < LARGE_ARRAY_SIZE - 1)) {
        pairs.push([key, value]);
        this.size = ++data.size;
        return this;
      }
      data = this.__data__ = new MapCache(pairs);
    }
    data.set(key, value);
    this.size = data.size;
    return this;
  }
}
```

# Lodash 完整版


## `#getIteratee`

## Lodash.memoize 函数记忆

```js
var values = lodash.memoize(lodash.values);

console.log( values )
/*
{ [Function: memoized]
cache:
 MapCache {
   size: 0,
   __data__: { hash: [Hash], map: Map {}, string: [Hash] } } }
*/
console.log( values({ 'a': 1, 'b': 2 }) )
//=> [ 1, 2 ]
```


## `#`stringToPath

说这个方法之前先看下面这个例子:

```js
const lodash = require('../lib/lodash.4.17.15')
let obj = {
  a:[ 1, { b: { d: 20 }, c: 10 }],
  e: 200
}
console.log( lodash.get(obj, 'a[1].b.d') )
//=> 20
console.log( lodash.get(obj, 'a[0]') )
//=> 1
console.log( lodash.result(obj, 'a[1].c') )
// => 10
```

lodash.get 或 lodash.result 方法都可以得到对象的属性值,只是寻找属性的这个过程是一个字符串来表示,`#stringToPath`方法主要就是做这件事,只是它返回的并不是一个值,只是一个数组组成的各个部分的属性访问,这里就暂时叫做`访问路径数组`,最后由 get 或 result 方法得到这个数组解析出最后的值
