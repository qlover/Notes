## 差集(外排) `#`baseDifference

>两个集合也可以相"减"。A在B中的相对补集，写作B−A，是属于B的、但不属于A的所有元素组成的集合。

[来自百度百科](https://baike.baidu.com/item/%E9%9B%86%E5%90%88%E8%BF%90%E7%AE%97/12737171)

`#baseDifference`分别可以接收四个参数,前两个参数分别为源集合和目标集合,后两个参数用于控制在方法内比较时的方式

该方法的主要目的寻找出`源集合`相对于`目标集合`中的不同元素,返回一个新的数组,外排方法且不会去重, 目前 Lodash 中以下三种方式进行运算:

1. `Lodash.difference` 通常情况直接使用 SameValueZero 比较方式求差集
2. `Lodash.differenceBy` 指定回调比较方式求差集
3. `Lodash.differenceWith` 指定比较器方式求差集

baseDifference 方法源码虽有点长,但很好理解,核心思想就是两个数组依次一一比较,要么就通常的值比较,要么就用回调或者比较器的结果对值进行运算,如果不相等就将元素追加到结果,只是 Lodash 在循环的时候有点不一样,使用双 while 循环,跳出内循环使用的是[continue lable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/continue)语法意思都一样,下面是 baseDifference 部分源码:

```js
function baseDifference(array, values, iteratee, comparator) {
  // 默认使用 arrayIncludes 方式判断数组是否包含另一个元素
  // 得到源数组长度和目标数组长度
  // ...

  if (iteratee) {
    // 如果存在回调,则使用 arrayMap 对每个元素使用回调
    // 核心版本中 arrayMap 名称叫 baseMap
    values = arrayMap(values, baseUnary(iteratee));
  }
  // 有关比较器的一些判断
  // ...
  
  outer:
  while (++index < length) {
    // 遍历源数组(从左至右)
    var value = array[index],
      // 得到通常和回调处理后的值
      computed = iteratee == null ? value : iteratee(value);

    value = (comparator || value !== 0) ? value : 0;
    // 通常情况,排除 NaN 情况
    // JavaScript 中只有 NaN 不恒等于自己
    if (isCommon && computed === computed) {
      var valuesIndex = valuesLength;
      // 遍历目标数组(从右至左)
      while (valuesIndex--) {
        // 一旦出现相等重新下一次比较
        if (values[valuesIndex] === computed) {
          continue outer; // 跳跃到 outer: 标识
        }
      }
      // 不相等的值
      result.push(value);
    } 
    // 有回调或比较器情况
    else if (!includes(values, computed, comparator)) {
      result.push(value);
    }
  }
  return result;
}
```

*unary 是高阶函数的一种操作*

但是 baseDifference 接收的参数只能是两个,而像 Lodash.difference 这样的方法是可以接收一个 rest 参数,所以三个差集方法都用上了 baseRest 方法

### Lodash.difference

`#baseDifference`生成的通常比较求差集方法,可以接收一个源数组和一个 rest 数组参数,下面是该方法源码:

*不是求所有数组的差集,而是求源数组和 rest 数组的差集*

```js
var difference = baseRest(function(array, values) {
  return isArrayLikeObject(array)
    ? baseDifference(array, baseFlatten(values, 1, isArrayLikeObject, true))
    : [];
});
```

baseRest 回调中的 baseDifference 只有两个参数,一个是源数组,另一个是 baseFlatten 扁平后的数组,为什么要扁平,因此处的 values 是接收的一个 rest 参数,一个数组,可以有多个目标数组, baseFlatten 将其扁平化为一个数组,只是要注意,设置了 isStrict

*不得不佩服 Lodash 想的,之前我还以为是将 rest 分开遍历,没想到是直接扁平化一次*

Lodash.differenceBy 和 Lodash.differenceWith 一致可以接收一个源数组和一个 rest 数组参数,rest 参数的最后一个元素作为回调或比较器,比较器方式则更优先与其它方式



## 补集(内排) `#`basePullAll

>两个集合也可以相"减"。A在B中的相对补集，写作B−A，是属于B的、但不属于A的所有元素组成的集合。
>在特定情况下，所讨论的所有集合是一个给定的全集U的子集。这样，U−A称作A的绝对补集，或简称补集（余集），写作A′或CUA

[来自百度百科](https://baike.baidu.com/item/%E9%9B%86%E5%90%88%E8%BF%90%E7%AE%97/12737171)

该方法的主要目的去掉`源集合`中包含的`目标集合`元素,返回源集合,内排方法且不会去重,目前 Lodash 中以下几种方式进行运算:

1. `Lodash.pull` 通常使用 SameValueZero 从数组中原地删除所有给定值的元素,接收需要删除的元素 rest 参数
2. `Lodash.pullAll` 与 Lodash.pull 的唯一区别其接收的是需要删除的元素的数组
3. `Lodash.pullBy` 在 Lodash.pullAll 基础上增加了指定回调
4. `Lodash.pullWhit` 在 Lodash.pullAll 基础上增加了指定比较器

基本与差集定义一致,但是在其限制了一个全集U,也就是限定了集合之间的运算应该是包含关系,该方法就是求的`目标数组`与`源数数组`之间的目标数组的补集(说成补集到不如直接是删除元素)

```js
let array = [1,2,3,4,5]
lodash.pullAll(array, [1,2])
console.log(array);
//=> [ 3, 4, 5 ]
```

虽说和差集的定义一致,但是在补集结果只能是限定的全集,也就是源数组,重复的元素也会返回,该方法有以下几个要素:

1. 会使用 indexOf 算法查找元素存在位置
2. 内排方法,但是其内部是还会多一个用于比较的复制数组
3. 结果使用`Array.prototype.splice`API原地截取

*如果只是做简单的过滤掉目标数组的补集数组,建议使用 Lodash.filter 过滤或作相反差集*

这里直接附上部分源码:

```js
function basePullAll(array, values, iteratee, comparator) {

  // ...
  // 得到用于比较的复制数组 seen
  // ...
  
  while (++index < length) {
    var fromIndex = 0,
        value = values[index],
        computed = iteratee ? iteratee(value) : value;
    // 使用 indexOf 算法判断元素是否存在
    // !!! 因为 indexOf 算法会再根据数组给定的元素再对数组进行遍历
    // 其复杂度相当于 O(n^3)
    while ((fromIndex = indexOf(seen, computed, fromIndex, comparator)) > -1) {
      if (seen !== array) {
        splice.call(seen, fromIndex, 1);
      }
      splice.call(array, fromIndex, 1);
      // Array.prototype.splice
    }
  }
  return array;
}
```

双循环加上 indexOf 的操作无限接近了`O^3`,并不只是单纯的判断是否存在某个元素,而是要知道截取的索引,但得到索引又需要又更多的开销,与通常的判断是否存在和存在并得到索引,最好的办法就是使用普通循环得到索引位置,显然 Lodash 的`#baseIndexOf`方法就是这样做的

这里附上一个[链接](https://robin-front.github.io/2017/07/03/arr-of-indexOf-vs-includes-and-for-loop.html),其阐述了使用普通循环,indexOf 和 includes 三种算法

```js
let array = [1,2,3,4,5]
lodash.pullAll(array, [1,2])
console.log(array);
//=> [ 3, 4, 5 ]
console.log( lodash.filter([1,2,3,4,5],
  v => !(v == 1 || v == 2) )
);
//=> [ 3, 4, 5 ]
```

## 并集 `#`baseUniq

>若A和B是集合，则A和B并集是有所有A的元素和所有B的元素，而没有其他元素的集合。A和B的并集通常写作 "A∪B"，读作“A并B”，用符号语言表示，即：A∪B={x|x∈A,或x∈B}

[来自百度百科](https://baike.baidu.com/item/%E5%B9%B6%E9%9B%86)

该方法的主要目的将`源数组`和`目标数组`连接起来,外排方法,目前 Lodash 中以下几种方式进行运算:

1. `Lodash.union` 去重并使用 SameValueZero 进行比较
2. `Lodash.unionBy` 去重并指定回调进行比较
3. `Lodash.unionWith` 指定比较器进行比较,比较器取函数 false 值,且不会去重

当指定比较器该方法使用`#arrayIncludesWith`方法判断是否存在,该方法应注意以下两点:

1. 方法接收三个参数,**参数一是一个扁平化后一维数组**
2. 因结果取唯一值数组需要去重,当超过了处理个数限制会直接使用 Set 去重
3. 只有当超过了限制和比较器比较时会使用算法去重(union 和 unionBy 会直接循环去重)

因为接收的参数一是一个一给数组,那么其核心思想那就是对数组做去重操作,至于生成的 Lodash.union 方法当它接收一个参数时就是去重,当接收 rest 参数就是说并集运算

`#baseUniq`源码比较长,附上部分源码:

```js
function baseUniq(array, iteratee, comparator) {

  // ...
  // 小细节, seen 和 result 都为同一个数组
  result = []
  seen = result

  // 有比较器则使用 arrayIncludesWith 做去重
  // 如果操作的元素超过限制的 200 个, 直接使用 Set 去重并返回

  // 中间变量 seen 监听回调或比较器的处理结果集合

  outer:
  while (++index < length) {
    // 回调处理后的值
    // iteratee 具体可见 #getIteratee 方法
    computed = iteratee ? iteratee(value) : value;
    // 比较器处理后的值
    value = (comparator || value !== 0) ? value : 0;

    // 通常处理
    if (isCommon && computed === computed) {
      // 从 seen 集合中判断去重
      while (seenIndex--) {
        if (seen[seenIndex] === computed) {
          continue outer;
        }
      }
      result.push(value);
    }
    // 带有回调或比较器的处理
    else if (!includes(seen, computed, comparator)) {
      // seen 和 result 为同一个数组,可以直接比较
      if (seen !== result) {
        seen.push(computed);
      }
      result.push(value);
    }
  }
  return result;
}
```

源码中 includes 部分注意特别注意的两点,一当比较器比较时才会使用 includes 算法去重;二当比较器比较时, seen 和 result 做了恒等运算,(这一部分暂时并不知道有什么用,而且并没有做去重操作有点违背 union 操作)

下面是一个当比较的元素不是原始值是对象时的 unionWith:
```js
let ob = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }]
let ot = [{ 'x': 1, 'y': 1 }, { 'x': 1, 'y': 2 }]
console.log( lodash.unionWith(ob, ot, lodash.isEqual) )
//=> [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }, { 'x': 1, 'y': 1 }]
```

好像对象上又去重?

==TODO 这一部分==

## 对称差集`#baseXor`

>对称差集：集合A与集合B的对称差集定义为集合A与集合B中所有不属于A∩B的元素的集合，记为A△B,也就是说A△B={x|x∈A∪B,x∉A∩B}，即A△B=(A∪B)—(A∩B).也就是A△B=（A—B）∪（B—A）

[来自百度百科](https://baike.baidu.com/item/%E5%AF%B9%E7%A7%B0%E5%B7%AE%E9%9B%86/1739582?fr=aladdin)

该方法的主要目的寻找出`源集合`与`目标集合`中的彼此所有不同的元素,返回一个新的数组,外排方法且`会去重`,目前 Lodash 中以下三种方式进行运算:

1. `Lodash.xor` 通常情况直接使用 SameValueZero 比较方式求对称差集
2. `Lodash.xorBy` 指定回调比较方式求对称差集
3. `Lodash.xorWith` 指定比较器方式求对称差集

```js
console.log( lodash.difference([1,2, 1], [2,3,4,5], [2,4]) )
//=> [ 1, 1 ]
console.log( lodash.xor([1,2, 1], [2,3,4,5], [2,4]) )
//=> [ 1, 3, 5 ]
console.log( lodash.xor([1,2, 1], [2,3,[4,5], 6], [2,4]) )
//=> [ 1, 3, [ 4, 5 ], 6, 4 ]
console.log( lodash.xor([1,2, 1], [2,3,[4,5], 6], [2,[4,5], 4 ]) )
//=> [ 1, 3, [ 4, 5 ], 6, [ 4, 5 ], 4 ]
// 每一次比较是将数组看作一个元素
```

该方法接收的参数与`baseDifference`基本一致,运算很简单,源与数组与目标数组全部依次做差集运算,每一次结果会被记录,累计后的结果扁平化且去重,那么下面直接附上其源码:

```js
function baseXor(arrays, iteratee, comparator) {
  var length = arrays.length;
  if (length < 2) {
    return length ? baseUniq(arrays[0]) : [];
  }
  var index = -1,
      result = Array(length);

  while (++index < length) {
    var array = arrays[index],
        othIndex = -1;
    // 将一个数组看作一个元素
    while (++othIndex < length) {
      if (othIndex != index) {
        // 每一次的结果累计
        result[index] = baseDifference(result[index] || array, arrays[othIndex], iteratee, comparator);
      }
    }
  }
  // 并不是尝试扁平化
  return baseUniq(baseFlatten(result, 1), iteratee, comparator);
}
```