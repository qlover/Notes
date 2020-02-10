
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

该方法基本上是所有 sorted 方法的基函数,首先`#baseSortedIndex`是直接生成 sorteIndex,sorteIndexOf 这类方法的基函数,而该方法的内部会使用下面二个条件判断是否使用 baseSortedIndexBy 方法:

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

1. lodash.findIndex 此方法类似于_.find，不同之处在于它返回第一个元素谓词的索引返回true，而不是元素本身。
2. lodash.findLastIndex 此方法类似于_.findIndex，不同之处在于它从右到左遍历collection的元素。
3. lodash.find 遍历collection的元素，返回第一个元素谓词，返回true。谓词由三个参数调用：（值，索引|键，集合）。
4. lodash.findLast 此方法类似于_.findIndex，不同之处在于它从右到左遍历collection的元素。
5. lodash.findKey 此方法类似于_.find，不同之处在于它返回第一个元素谓词的键，而不是元素本身，返回谓词。
6. lodash.findLastKey 此方法类似于_.findKey，不同之处在于它以相反的顺序遍历集合的元素。

