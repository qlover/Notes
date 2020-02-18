
## 二进制搜索 sorted

二进制搜索也可以叫二分查找或者折半查找,以下是 Lodash 中以该算法实现的方法和一些其它有关的方法:

1. lodash.sortedIndex 使用二进制搜索来确定应将值插入数组以保持其排序顺序的最低索引。
2. lodash.sortedIndexBy 此方法类似于 lodash.sortedIndex，不同之处在于它接受为值调用的iteratee，并接受数组的每个元素以计算其排序等级。使用一个参数调用iteratee ：（值）。
3. lodash.sortedIndexOf 此方法类似于 lodash.indexOf，不同之处在于它对已排序的数组执行二进制搜索。
4. lodash.sortedLastIndex 此方法类似于 lodash.sortedIndex，不同之处在于它返回应将值插入数组以保持其排序顺序的最高索引。
5. lodash.sortedLastIndexBy 此方法类似于 lodash.sortedLastIndex，不同之处在于它接受为值调用的iteratee，并接受数组的每个元素以计算其排序等级。使用一个参数调用iteratee ：（值）。
6. lodash.sortedLastIndexOf 此方法类似于 lodash.sortedLastIndex，不同之处在于它接受为值调用的iteratee，并接受数组的每个元素以计算其排序等级。使用一个参数调用iteratee ：（值）。
7. lodash.sortedUniq 此方法类似于 lodash.sortedLastIndex，不同之处在于它接受为值调用的iteratee，并接受数组的每个元素以计算其排序等级。使用一个参数调用iteratee ：（值）。
8. lodash.sortedUniqBy 此方法类似于 lodash.sortedLastIndex，不同之处在于它接受为值调用的iteratee，并接受数组的每个元素以计算其排序等级。使用一个参数调用iteratee ：（值）。


### `#`baseSortedIndexBy 

该方法直接生成带回调的二进制搜索,间接的生成通常的 indexOf 搜索,下面先来个方法使用的实例:

```js
let objects = [{ 'x': 4 }, { 'x': 5 }]

// 得到 40 应该按顺序插入的索引值
console.log( lodash.sortedIndex([30, 50], 40) )
// => 1
// 得到 5 出现的第一次出现索引
console.log( lodash.sortedIndexOf([4, 5, 5, 5, 6], 5) )
// => 1
console.log( lodash.sortedIndexBy(objects, { 'x': 4 }, 'x') )
// => 0
```

该方法基本上是所有 sorted 方法的实现的函数,首先`#baseSortedIndex`是直接生成 sorteIndex,sorteIndexOf 这类方法实现的函数,而该方法的内部会使用下面二个条件判断是否使用 baseSortedIndexBy 方法:

1. sorted 方法的参数二是数值类型(不包括 NaN)
2. 搜索的数组在常量`HALF_MAX_ARRAY_LENGTH`范围内(2147483647 个长度)

如果都不满足则会使用 baseSortedIndexBy 搜索

### 二进制搜索(二分查找) `#`baseSortedIndex

baseSortedIndex 和 baseSortedIndexBy 两个方法的区别就在于,后者会对遍历的元素回调处理,前者会直接遍历返回,在 Lodash 的二进制搜索比较重要的有以下几点:

1. 搜索的序列应该是排序序列
2. 中间值取整
3. 取最低或最高索引只由一个`<=`或`<`决定,当值大于或等于时,就表示已经满足条件,那么当前索引就为最低索引,最高索引也就是要大于当前元素的索引

Lodash 中的二分查找并非使用递归,而是直接遍历,那么又应该另注意以下几点:

1. 当中间值小于指定值,遍历范围则为中间索引至数组长度
2. 当中间值大于指定值,遍历范围则为0至中间索引
3. 每次循环确定最新的遍历范围,最后返回遍历范围的末尾


只寻找出满足条件的最高范围:

```js
const binarySerach = (array, value, isHighest) => {
  let low = mid = computed = 0,
      high = array.length;

  while ( low < high ) {
    mid = parseInt( (low + high) / 2 )
    computed = array[mid]

    // 重新指定最新遍历范围
    if( computed <= value ){ // !!! 当中间值小于或等于指定值,范围:[mid+1, high]
      low = mid + 1
    } else {// !!! 当中间值大于定值,范围:[low, mid]
      high = mid
    }
  }
  
  return high
}

console.log( binarySerach([10,20,20,30,40], 25) )
```

以上是搜索出满足条件的最高索引位置,试想,如果需要控制是搜索最低或是最高索引时,以中间值 computed 和 指定值 value 规则应该是:

- 如果搜索最高索引,条件a:也就是最后一个大于 value(可能会出现连续相同的 value) 的索引,也可以表示为条件b:小于等于最后一个 value 值的后一个索引
- 如果搜索最低索引,条件c:也就是第一个小于 value(可能会出现连续相同的 value) 的索引的后一个索引,条件d:也可以表示为小于等于第一个 value 值的前一个索引

这样一来,程序上判断的条件就可以组合成多种情况,下面先来看条件a和c组合情况:

```js
const binarySerach = (array, value, isHighest) => {
  let low = mid = computed = 0,
      high = array.length;
      
  while ( low < high ) {
    mid = parseInt( (low + high) / 2 )
    computed = array[mid]
    if( isHighest ){ // 最高索引
      if( computed > value ){
        // 最后一个大于 value 的索引
        return mid
      } else {
        low = mid
      }
    } else { // 最低索引
      if( computed < value ){
        // 第一个小于 value 的索引的后一个索引
        return mid + 1
      } else {
        high = mid 
      }
    }
  }
  return high
}
console.log( binarySerach([10,20,20,30,40], 25, true) )
// => 3
console.log( binarySerach([10,20,20,30,40], 25) )
// => 3

console.log( binarySerach([10,20,20,30,40], 20, true) )
// => 3
console.log( binarySerach([10,20,20,30,40], 20) )
// => 1
```

很显然上面的代码臃肿并不是 lodash 的风格,lodash 组合了条件b和c,下面直接附上 lodash 的做法:
```js
const binarySerach = (array, value, isHighest) => {
  let low = mid = computed = 0,
      high = array.length;

  while ( low < high ) {
    // 使用无称号右移取整
    mid = (low + high) >>> 1
    computed = array[mid]
    if ( isHighest ? (computed <= value) /*条件b*/
       : (computed < value) /*条件c*/ ) {
      low = mid + 1;
    } else {
      high = mid;
    }
  }
  return high
}
console.log( binarySerach([10,20,20,30,40], 25, true) )
// => 3
console.log( binarySerach([10,20,20,30,40], 25) )
// => 3

console.log( binarySerach([10,20,20,30,40], 20, true) )
// => 3
console.log( binarySerach([10,20,20,30,40], 20) )
// => 1
```

那么一个二分查找已经完成,附上 baseSortedIndex 源码:

```js
function baseSortedIndex(array, value, retHighest) {
  var low = 0,
      high = array == null ? low : array.length;

  if (typeof value == 'number' && value === value && high <= HALF_MAX_ARRAY_LENGTH) {
    while (low < high) {
      var mid = (low + high) >>> 1,
          computed = array[mid];

      if (computed !== null && !isSymbol(computed) &&
          (retHighest ? (computed <= value) : (computed < value))) {
        low = mid + 1;
      } else {
        high = mid;
      }
    }
    return high;
  }
  return baseSortedIndexBy(array, value, identity, retHighest);
}
```

`#`baseSortedIndexBy 方法思想与 baseSortedIndex 类似,内部会对值用回调处理,并且会处理 undefined,null,NaN 和 symbol 的情况

## 键值查找 find 

1. lodash.findIndex 此方法类似于 lodash.find，不同之处在于它返回第一个元素谓词的索引返回true，而不是元素本身。
2. lodash.findLastIndex 此方法类似于 lodash.findIndex，不同之处在于它从右到左遍历collection的元素。
3. lodash.find 遍历collection的元素，返回第一个元素谓词，返回true。谓词由三个参数调用：（值，索引|键，集合）。
4. lodash.findLast 此方法类似于 lodash.findIndex，不同之处在于它从右到左遍历collection的元素。
5. lodash.findKey 此方法类似于 lodash.find，不同之处在于它返回第一个元素谓词的键，而不是元素本身，返回谓词。
6. lodash.findLastKey 此方法类似于 lodash.findKey，不同之处在于它以相反的顺序遍历集合的元素。

其中 findIndex 和 findLastIndex 直接使用 `#baseFindIndex` 实现,find 和 findLast 分别使用 findIndex 和 findLastIndex 并借助基函数`#createFind`实现,findKey 与 findLastKey 则是由 `#baseFindKey` 直接实现

### 数组索引搜索 `#`baseFindIndex

该方法很简单,主要由 indexOf 算法实现,并且直接支持回调和是否从右向前遍历,至于 findIndex 和 findLastIndex 接口的 predicate 参数可以是函数,值或数组,这一点可参考 getIteratee 部分具体的值转换,当然 findLastIndex 方法也要指定从右向前遍历

其源码:
```js
function baseFindIndex(array, predicate, fromIndex, fromRight) {
  var length = array.length,
      index = fromIndex + (fromRight ? 1 : -1);

  while ((fromRight ? index-- : ++index < length)) {
    if (predicate(array[index], index, array)) {
      return index;
    }
  }
  return -1;
}
```

### `#`createFind

createFind 生成的函数与 baseFindIndex 方法首先第一个不同的地方就是遍历的对象不同, createFind 可以遍历数组和对象,而 baseFindIndex 只能遍历数组,所以一个可以返回任意类型:

```js
let users = [
  { money: [100, 500], name: 'Qlover' },
  { money: [500, 50], name: 'Lee' },
  { money: [350, 100], name: 'Fred' }
]

console.log( lodash.findIndex(users[0], 'name') )
// => -1
console.log( lodash.findIndex(users, obj => obj.name.length < 5) )
// => 1
console.log( lodash.findIndex(users, 'name') )
// => 0

console.log( lodash.find(users[0], val => val.length > 1 ) )
// => [ 100, 500 ]
console.log( lodash.find(users[0], val => val.length > 3 ) )
// => Qlover
// 返回第一个满足条件的元素
console.log( lodash.find(users, obj => obj.name.length < 5) )
// =>{ money: [ 500, 50 ], name: 'Lee' }
```

createFind 方法也是一个典型的函数式的方法,接收的参数就是 findIndex 或 findLastIndex,间接来说也就是 createFind 方法实现也离不开 baseFindIndex,那么 createFind 生成的函数自然也可以使用 baseFindIndex,且回调可以是 getIteratee 指定值,比如一个函数或一个键值,直接看源码吧:

```js
function createFind(findIndexFunc) {
  // perdicate 为用户自定义回调
  return function(collection, predicate, fromIndex) {
    var iterable = Object(collection);
    if (!isArrayLike(collection)) {
      // 不是一个数组
      // 使用 getIteratee 重新将值进行转换成新回调
      // 如果用户并未传入,好么 undefined 会被转换成 identity
      var iteratee = getIteratee(predicate, 3);
      
      collection = keys(collection);
      // 那么新的搜索回调的返回值由刚刚重新转换后回调 iteratee 的值
      // 如果用户自定义为 key 那就取 key 值
      // 如果为函数...
      // 如果为数组...
      // 这些都将作为搜索对象时的回调
      predicate = function(key) { return iteratee(iterable[key], key, iterable); };
    }
    var index = findIndexFunc(collection, predicate, fromIndex);
    return index > -1 ? iterable[iteratee ? collection[index] : index] : undefined;
  };
}
```

其将源码对比,会发现 createFind 生成的方法只不过比 baseFindIndex 生成的方法多了对象这种情况,处理就是将默认或用户指定的回调做一步包装得到搜索时调用的回调


### 键的搜索 `#`baseFindKey

如果熟悉了`#createBaseFor`这个基函数,那么理解起该方法也就简单许多,createBaseFor 可以理解为 lodash 中所有遍历的抽象类,比如 baseEach 这个核心思想方法之一就是由 createBaseFor 生成,具体可参考 lodash 核心 array 部分

createBaseFor 思想就是接收一个对象,该对象可以是类数组也可以是一个对象,取得这个对象的可枚举属性组成的数组,遍历该数组分别将对象的属性值、当前属性和当前对象本身三个参数传递给回调,如果回调返回 false 则返回遍历对象,也正是因为如果遍历对象返回 false 则会停止遍历,这对于搜索来说可不能这样

比如搜索的对象中值为 true 的第一个键,遍历的第一个值就为 false、null 或者 0 那不是直接就返回了, baseFindKey 就在这个问题的基础上对 createBaseFor 调用的回调更上一屋的包装,这一点才是该方法重点

```js
// eachFunc 可能是 createBaseFor() 或 createBaseFor(true)
function baseFindKey(collection, predicate, eachFunc) {
  var result;
  eachFunc(collection, function(value, key, collection) {
    // 本身 createBaseFor 调用的该回调返回 false 这里的话就会直接返回 result
    // 还未搜索直接是 undefined
    if (predicate(value, key, collection)) {
      result = key;
      return false;
    }
  });
  return result;
}
```


