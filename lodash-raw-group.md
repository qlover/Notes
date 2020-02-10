
## 数组切片

有关于切片的方法分别是:

1. Lodash.drop 从头可指定删除个数元素
2. Lodash.dropRight 从末尾可指定删除个数元素
3. Lodash.dropWile 从头指定回调结果的删除元素
4. Lodash.dropRightWile 从末尾指定回调结果的删除元素
5. Lodash.take 从头可指定获取个数元素
6. Lodash.takeRight 从末尾可指定获取个数元素
7. Lodash.takeWile 从头指定回调结果的获取元素
8. Lodash.takeRightWile 从末尾指定回调结果的获取元素


注意, while 切片并不是过滤,而是删除或获取连续满足条件的元素直到不满足回调结果为止,下面以从头删除元素是偶数的元素为例:
```js
let nums = [ 2,5,4,6,8 ];
console.log( lodash.dropWhile(nums, o => o % 2 == 0 ) )
//=> [ 5, 4, 6, 8 ]
nums = [ 2,4,6,8 ];
console.log( lodash.dropWhile(nums, o => o % 2 == 0 ) )
//=> []
```

数组切片也是就数组的截取,这几个方法都是由`#baseSlice`生成,只是各自都有各自实现的逻辑

### `#baseWhile`

该方法可以我的做到从头或从末尾是截取元素,下面用一个例子来分析,首先分为两个步骤:

- 计算出满足回调条件的最后一个元素的索引,也就是截取结束位置
- 根据是删除还是获取计算出截取范围*!注意是与名称相反的范围*
  1. 从左至右
    + drop start=index+1 end=length
    + take start=0 end=index
  2. 从右至左 
    + drop start=0 end=index+1 
    + take start=index+1 end=length 

index 为结束索引,当删除时,截取出来的元素要包括 index 位置元素,所以这个元素是最后一个不被满足条件元素,其它地方一样,见下面例子:

```js
let numbers = [2, 5, 4, 6, 8]
const odd = o => o % 2 == 0

// let users = [ 2,5,4,6,8 ];
lodash.dropWhile(numbers, odd )
lodash.takeWhile(numbers, odd )
lodash.dropRightWhile(numbers, odd )
lodash.takeRightWhile(numbers, odd )
/*
[ 5, 4, 6, 8 ]
[ 2 ]
[ 2, 5 ]
[ 4, 6, 8 ]
 */

const baseWhile = (array, predicate, isDrop, fromRight) => {

  // 确定遍历方向
  // 从头则 index 从 -1 开始
  // 从末尾 index 从 array.length 开始
  let length = array.length,
      index = fromRight ? length - 1 : 0;

  // 确定截取结束位置
  for(; index < length ; fromRight ? index-- : ++index ){
    if( !predicate(array[index], index, array) ){
      break;
    }
  }

  // 计算截取范围
  // 如果是从右截取 index 应该是开始位置
  return isDrop
    // drop
    ? array.slice( fromRight ? 0 : index , fromRight ? index + 1 : length )
    // take
    : array.slice( fromRight ? index + 1 : 0 , fromRight ? length : index );
}

// 从左至右 drop 
console.log( baseWhile(numbers, odd, true ) )
//=> [ 5, 4, 6, 8 ]
// 从左至右 take
console.log( baseWhile(numbers, odd ) )
//=> [ 2 ]

// 从右至左 drop 
console.log( baseWhile(numbers, odd, true, true ) )
//=> [ 2, 5 ]
// 从右至左 take
console.log( baseWhile(numbers, odd, false, true) )
//=> [ 4, 6, 8 ]
```
下面是`#baseWhile`源码:

```js
function baseWhile(array, predicate, isDrop, fromRight) {
  var length = array.length,
      index = fromRight ? length : -1;

  while ((fromRight ? index-- : ++index < length) &&
    predicate(array[index], index, array)) {}
  return isDrop
    ? baseSlice(array, (fromRight ? 0 : index), (fromRight ? index + 1 : length))
    : baseSlice(array, (fromRight ? index + 1 : 0), (fromRight ? length : index));
}
```

*不要被删除和获取名字误导了,删除就是截取剩余部分,获取就是截取删除部分,都是做截取操作,过滤可以使用 Lodash.filter*

## 对象组合

有关于切片的方法分别是:

1. Lodsah.chunk 创建一个元素数组，将其分为大小长度的组。如果无法均匀分割数组，则最后一块将是剩余的元素。
2. Lodash.zip 创建一个分组元素数组，其中第一个元素包含给定数组的第一个元素，第二个元素包含给定数组的第二个元素，依此类推。
3. Lodash.zipObject 此方法类似于_.fromPairs，不同之处在于它接受两个数组，一个是属性标识符，另一个是对应值。
4. Lodash.zipObjectDeep 此方法类似于_.zipObject，但它支持属性路径。
5. Lodash.toPairs 为对象创建自己的可枚举字符串键值对的数组，这些对象可以由_.fromPairs消耗。如果object是映射或集合，则返回其条目。
6. Lodash.toPairsIn 与 Lodash.toPairs 类似但包括原型属性
7. Lodash.fromPairs Lodash.toPairs的逆运算,此方法返回由键值对组成的对象
8. Lodash.unzip Lodash.zip 逆运算
9. Lodash.unzipWith 此方法类似于_.unzip，不同之处在于它接受iteratee来指定应如何组合重组的值。使用每个组的元素（... group）调用iteratee

### 分块|打散 Lodash.chunk

[Lodash.chunk(array, size=1)](https://lodash.com/docs/4.17.15#chunk) 以 size 为单位分割数组,默认情况下会外排将每一个元素单独分割并返回一个新数组

*Lodash.flatten 可以做 chunk 的逆运算*

以下是 chunk 源码:
```js
function chunk(array, size, guard) {
  //? 暂时还不知道 guard 能有什么用
  if ((guard ? isIterateeCall(array, size, guard) : size === undefined)) {
    size = 1;
  } else {
    // 利用 Math.max 验证 size 有效性
    // 也就是其要大于等于 0 
    size = nativeMax(toInteger(size), 0);
  }
  var length = array == null ? 0 : array.length;
  if (!length || size < 1) {
    return [];
  }
  var index = 0,
      resIndex = 0,
      // 最大可能拆分
      result = Array(nativeCeil(length / size));

  while (index < length) {
    result[resIndex++] = baseSlice(array, index, (index += size));
  }
  return result;
}
```

源码中因注意以下几点:

1. `Math.max` 和 toInteger 两个方法验证数字有有效值,不需要在进行是否大于小0，如果小于0 给默认值在这里就不需要,这样的做法省去了对数字的判断
2. `Math.ceil`对长度与拆分块长度取最大可能,计算出新数组应该有多少个元素
3. baseSlice 私有方法对数组截取,依次填充



### 对象属性数组 `#baseToPairs` toPairs toPairsIn fromPairs

该方法生成 Lodash.toPairs 和 Lodash.toPairsIn 两个方法,生成函数接收一个对象作为参数,因为是`对象属性`数组,那么就包括可枚举和不可枚举属性

Lodash.toPairs 使用 Object.keys 遍历属性;Lodash.toPairsIn 使用`in`运算符遍历属性

方法很简单,接收属性数组,`#arrayMap`遍历出新的数组,下面附上源码:

```js
function createToPairs(keysFunc) {
  return function(object) {
    var tag = getTag(object);
    if (tag == mapTag) {
      return mapToArray(object);
    }
    if (tag == setTag) {
      return setToPairs(object);
    }
    return baseToPairs(object, keysFunc(object));
  };
}

function baseToPairs(object, props) {
  return arrayMap(props, function(key) {
    return [key, object[key]];
  });
}

var toPairs = createToPairs(keys);
var toPairsIn = createToPairs(keysIn);
```

接下来就是 toPairs 的逆运算, Lodash.fromPairs, 因为是做逆运算,那么也就表示他们两个方法的返回值应该互为他们的接收值,当然这不是参绝对的,fromPairs 接收的是 toPairs 组成的数组键值对,所以只需要遍历该数组返回一个新对象即可,下面看其源码和例子:

```js
function fromPairs(pairs) {
  var index = -1,
      length = pairs == null ? 0 : pairs.length,
      result = {};

  while (++index < length) {
    var pair = pairs[index];
    result[pair[0]] = pair[1];
  }
  return result;
}


let obj =  { money: [100, 500], name: 'Qlover' }

let pairs = lodash.toPairs(obj)
console.log( pairs )

// 添加一个其它方法
pairs.push(['toString', {}])
// => [ [ 'money', [ 100, 500 ] ], [ 'name', 'Qlover' ] ]
console.log( lodash.fromPairs(pairs) )
// => { money: [ 100, 500 ], name: 'Qlover', toString: {} }
```

建议不要使用该方法操作比较敏感的对象


### 压缩与解压组合 zip unzip

如果做过 php,就会有这样一个方法爱不释手, [array_combine](https://www.php.net/manual/en/function.array-combine.php) , 因为这个方法可以将一个数组的当成键,另一个数组当用值,组合与一个新的数组,这样的数组组合思想很好

Lodash 也有这样的思想:

```js
console.log( lodash.zip([1,2], [3,4], [true, false]) )
//=> [ [ 1, 3, true ], [ 2, 4, false ] ]
console.log( lodash.unzip([ [ 1, 3, true ], [ 2, 4, false ] ]) )
//=> [ [ 1, 2 ], [ 3, 4 ], [ true, false ] ]

let fields = [ 'money', 'name' ]
let values = [ [200, 300], 'Lee' ]

let pairs = lodash.zip(fields, values)
console.log( pairs )
// => [ [ 'money', [ 200, 300 ] ], [ 'name', 'Lee' ] ]

let obj = lodash.fromPairs(pairs)
console.log( obj )
// => { money: [ 200, 300 ], name: 'Lee' }
console.log( lodash.zipObject(fields, values) )
//=> { money: [ 200, 300 ], name: 'Lee' }
```

zip 操作是 rest 参数,那么就离不开 baseRest 方法, 而 unzip 操作只接收一个数组,返回的结果互为接收的参数,然后仔细观察它们的返回值,数组结构是一致的,这个特点很重要,那么一个方法就可以了,而处理这两种情况的方法是`#unzip`

对就是 lodash.unzip 方法...

但是 unzip 有一个设计非常巧妙的地方,如果再仔细观察上面例子中的头两行的 zip 和 unzip 操作,会发现,每一次的操作参数都是一个二维数组,并且是将每列元素遍历出来组合成的新数组,不管是 zip 还是 unzip,这种数据结构像`矩阵`

如果发现了这一点下面就是 unzip 方法的两个要点:

1. 矩阵每行的等长长度 length, 表示的行矩阵最长长度,也就控制了结果的总长度
2. 每列的值获取,使用 map + baseProperty 方式

#### 矩阵列集合

就是获取出每一列的集合,这里可以参考一下 getIteratee 部分的 baseIteratee 值的转换这一部分,先看下面两个例子:

对象集合的列数组:

```js
let users = [
  { money: [100, 500], name: 'Qlover' },
  { money: [500, 50], name: 'Lee' },
  { money: [350, 100], name: 'Fred' }
]
// 获取对象的属性值
const baseProperty = key => obj => obj ? obj[key] : undefined

// 传递给 iteratee 的值是 money|name 是一个 key 返回一个回调
const getName = baseProperty('name')

// 返回的回调,给定对象就返回对象的 key
// 这个 key 是被记忆起来的为 money

console.log( getName(users[2]) )
// => Fred 

// 这是获取指定的一个对象的 name 键值
// 那么如果是列,一个对象集合只需要对它进行遍历

console.log( lodash.map( users, getName ) )
//=> [ 'Qlover', 'Lee', 'Fred' ]
```

数组集合的列数组:

```js
const baseProperty = key => obj => obj ? obj[key] : undefined
let users = [ [100, 500], [500, 50], [350, 100] ]

// 指定元素数组的第1个元素
const getName = baseProperty(0)

console.log( getName(users[2]) ) // => 350 
console.log( lodash.map( users, getName ) )
//=> [ 'Qlover', 'Lee', 'Fred' ]

// 如果是每一列全部的集合,只需要更改 baseProperty 方法指定的索引值

// 每行矩阵最多只有 2 个元素
let index = -1,
    length = 2,
    result = [];

while ( ++index < length ) {
  result.push(lodash.map( users, baseProperty(index) ))
}
console.log( result )
//=> [ [ 100, 500, 350 ], [ 500, 50, 100 ] ]
```

这可能要仔细的花点时间揣摩上面的两个例子,关键点就再一使用 baseProperty 这个高阶函数和 map 配合

#### 结果值的长度

这一点只需要观察 zip 和 unzip 两个方法返回的结果值, zip 矩阵每一行的长度决定 unzip 结果元素总长度; unzip 元素总长度决定 zip 矩阵每一行的长度

下面就直接附上两个方法的源码:

```js
// lodash.zip
var zip = baseRest(unzip)

// lodash.unzip
function unzip(array) {
  if (!(array && array.length)) {
    return [];
  }
  var length = 0;
  // 过滤掉非类数组的同时得到了 length 的最大值
  array = arrayFilter(array, function(group) {
    if (isArrayLikeObject(group)) {
      length = nativeMax(group.length, length);
      return true;
    }
  });
  return baseTimes(length, function(index) {
    return arrayMap(array, baseProperty(index));
  });
}
```
baseRest 起一个作用就是将 rest 参数组合成了 unzip 可以接收的一个矩阵值,也就是个二维数组

### 使用回调压缩与解压组合 zipWith unzipWith 

不得不说 Lodash 设计的很好,将函数为单位独立出来,试想 zip 已经是将数组进行压缩组合,而 zipWith 将 zip 的结果用回调遍历一次,不就达到了 zipWith 的方法的初衷了吗？

这样一来,不要多写其它多余方法,只需要在对外多暴露一个接口而已,当然这都是看了源码才悟出的道理,那么下面就附上其源码:

```js
function unzipWith(array, iteratee) {
  if (!(array && array.length)) {
    return [];
  }
  // 个人觉得将这一行删掉看起来视觉上更好
  // 没有一个额外申请的变量
  // var result = unzip(array);
  if (iteratee == null) {
    // return result;
    return unzip(array);
  }
  // return arrayMap(result, function(group) {
  return arrayMap(unzip(array), function(group) {
    return apply(iteratee, undefined, group);
  });
}
var zipWith = baseRest(function(arrays) {
  var length = arrays.length,
      iteratee = length > 1 ? arrays[length - 1] : undefined;

  iteratee = typeof iteratee == 'function' ? (arrays.pop(), iteratee) : undefined;
  return unzipWith(arrays, iteratee);
});
```


### 键值组合 `#baseZipObject` zipObject zipObjectDeep

该方法用于生成 Lodash.zipObject 和 Lodash.zipObjectDeep,主要作用将源数组值当作键,目标数组值当作值组成一个新对象

方法组合流程也很简单遍历源数组,将值取出来循环赋值上目标数组的值,只是做赋值操作的方法不同 zipObject 直接使用`#assignValue`,而 zipObjectWith 使用的是`#baseSet`

baseSet 和 baseGet 两个方法分别是设置访问路径属性值和设置访问路径属性值,具体可参考 baseSet 部分,下面附两个方法的源码:

```js
function baseZipObject(props, values, assignFunc) {
  var index = -1,
      length = props.length,
      valsLength = values.length,
      result = {};

  while (++index < length) {
    var value = index < valsLength ? values[index] : undefined;
    assignFunc(result, props[index], value);
  }
  return result;
}
function zipObjectDeep(props, values) {
  return baseZipObject(props || [], values || [], baseSet);
}
function zipObject(props, values) {
  return baseZipObject(props || [], values || [], assignValue);
}
```
