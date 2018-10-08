

# ECMAScript6 #


1996年11月，JavaScript 的创造者 Netscape 公司，决定将 JavaScript 提交给国际标准化组织ECMA，希望这种语言能够成为国际标准。

次年，ECMA 发布262号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言称为 ECMAScript，这个版本就是1.0版。


ECMAScript 6.0（以下简称 ES6）是 JavaScript 语言的下一代标准，已经在2015年6月正式发布了。它的目标，是使得 JavaScript 语言可以用来编写复杂的大型应用程序，成为企业级开发语言。

ECMAScript 和 JavaScript 的关系是，前者是后者的规格，后者是前者的一种实现。另外的 ECMAScript 方言还有 Jscript 和 ActionScript

ES6 既是一个历史名词，也是一个泛指，含义是5.1版以后的 JavaScript 的下一代标准，涵盖了 ES2015、ES2016、ES2017 等等，而 ES2015 则是正式名称，特指该年发布的正式版本的语言标准




# let 和 const

+ 不存在变量提升
+ 有块级作用域
+ 暂时性死区
+ 不能重复声明

ES6 声明变量的六种方法

# 解构赋值

这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值

+ 数组，对象，字符串，数值，布尔值，函数参数 可解构
+ 解构可有默认值
+ 解构用数组形式解构
+ 不具备 Iterator 接口的数据不能解构

如果解构的右边值为恒等 undefined， 则不会有值， null 也是

如果解构不成功，变量的值就等于 undefined

只要某种数据结构具有 Iterator 接口，都可以采用数组形式的解构赋值

*只要等号右边的值不是对象或数组，就先将其转为对象* 
*由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错*

## 解析不使用圆括号 

+ 变量声明语句中，不能带有圆括号
+ 函数参数中，模式不能带有圆括号
+ 赋值语句中，不能将整个模式，或嵌套模式中的一层，放在圆括号之中


*赋值语句的非模式部分，可以使用圆括号*

## 解构的用途
1. 交换值
2. 函数返回多个值
3. 函数参数的定义
4. 提取 JSON 数据
5. 遍历 map 结构
6. 输入模块的指定方法


# 字符串


JavaScript内部，字符以UTF-16的格式储存，每个字符固定为2个字节。对于那些需要4个字节储存的字符（Unicode码点大于0xFFFF的字符），JavaScript会认为它们是两个字符。


+ 可以直接用 \u0000~\uFFFF 之间的字符
+ 大括号表示法与四字节的 UTF-16 编码是等价的
+ ES6为字符串添加了遍历器接口

## es6 提供的字符串操作方法
+ codePointAt()
+ String.fromCodePoint()
+ at()
+ normalize()
+ includes()
	表示是否找到了参数字符串。
+ startsWith()
	表示参数字符串是否在源字符串的头部。
+ endsWith()
	表示参数字符串是否在源字符串的尾部。
+ repeat()
	方法返回一个新字符串，表示将原字符串重复n次
	类似与 python 字符串的 * 操作
+ padStart()
	在头部或尾部补全
+ padEnd()
	在头部或尾部补全
+ String.raw()
	返回一个 / 都转义的字符串

# 模板字符串

+ 可用 ${} 替换变量
+ 多行
+ 能嵌套
+ 标签模板
+ 模板字符串的 raw 属性


# 正则

JavaScript 语言的正则表达式，只支持先行断言（lookahead）和先行否定断言（negative lookahead），不支持后行断言（lookbehind）和后行否定断言（negative lookbehind）


+ u 修饰
	- 为“Unicode模式”， 用来正确处理大于\uFFFF的Unicode字符
	- 对于码点大于0xFFFF的Unicode字符，点字符不能识别，必须加上u修饰符
	- 匹配 unicode 字符
	- 所有量词都会正确识别码点大于0xFFFF的Unicode字符
+ y 修饰 
	- 后一次匹配都从上一次匹配成功的下一个位置开始, 与 g 类似
	- y 修饰符号隐含了头部匹配的标志 ^
+ s 修饰, dotAll 模式
即点（dot）代表一切字符。所以，正则表达式还引入了一个dotAll属性，返回一个布尔值，表示该正则表达式是否处在dotAll模式

正则表达式中，点（.）是一个特殊字符，代表任意的单个字符，但是行终止符（line terminator character）除外。

以下四个字符属于”行终止符“
	U+000A 换行符（\n）
	U+000D 回车符（\r）
	U+2028 行分隔符（line separator）
	U+2029 段分隔符（paragraph separator）
		
+ sticky 
	表示是否设置了 y 修饰
+ flags 
	返回正则表达式的修饰符
+ RegExp.escape()
	字符串必须转义，才能作为正则模式。

# 数组

+ Array.from()
	- 只要有 length 和 Iterator 接口的数据结构，都能转为数组
	- 而扩展运算符是调用 Symbol.iterator 接口转换
+ Array.of()
	- 方法用于将一组值，转换为数组
+ copyWithin(target, start = 0, end = this.length )
	- 当前数组内部，将指定位置的成员复制到其他位置
	- 使用这个方法，会修改当前数组
+ find( callback )
	- 用于找出第一个符合条件的数组成员
	- 当 callback 为返回 true 时返回该成员
	- callback( value, index, arr)
+ findIndex( callback )
	- 该方法与 find() 类似
	- 只是当都不满足时会返回 -1 
	- 另外 find() 和 findIndex() 可以发现 NaN
+ fill(target, start, end)
	- 给定值，填充一个数组
	- 数组中已有的元素，会被全部抹去
	- 原改变原数组
+ entries()
	- 对 key-> value 遍历
+ keys()
	- 对 key 遍历
+ values()
	- 对 value 遍历
	- 三个遍历方法都返回一个 Iterator 对象
+ includes(target, start)
	- 某个数组是否包含给定的值 
	- 可以发现 NaN
	- start 开始位置
	- 方法属于 ES7

Map 结构的 has 方法，是用来查找键名的
	比如 Map.prototype.has(key)、WeakMap.prototype.has(key)、Reflect.has(target, propertyKey)。
Set 结构的 has方法，是用来查找值的
	比如Set.prototype.has(value)、WeakSet.prototype.has(value)。

Array.from()
... 运算符
copyWithin()
fill()
for...of()
以及三个遍历的三个方法都会发现数组空位，但都处理不一致
而 ES5 则是会忽略空位


# 函数

+ name 属性会返回当前函数的名字
+ length 
	会返回没有指定默认值的参数个数
	但 rest 参数不计算
+ 作用域 
+ reset 参数与 扩展运算符
	reset 参数后不能再有其它参数
	而扩展运算符则是将一个数组转为用逗号分隔的参数序列
+ 箭头函数
	- `( 形参列表 ) => { 函数体 }`
	- 可直接在 => 前写一个形参位置
	- 函数可直接返回和在函数体中返回
```javascript
	let f2 = (a, b) =>  a + b;
```
	- 多个参数用 () 包括形参
	- 没有参数则要用 () 占位
	- 直接返回对象时要加括号
	- 函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
	- 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
	- 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替。
	- 不可以使用yield命令，因此箭头函数不能用作Generator函数。
+ 绑定 this
	ES7 的提案
	双冒号（::），双冒号左边是一个对象，右边是一个函数
	该运算符会自动将左边的对象，作为上下文环境（
	如果双冒号左边为空，右边是一个对象的方法，则等于将该方法绑定在该对象上面即this对象），绑定到右边的函数上面。
+ 函数尾调用

尾调用（Tail Call）是函数式编程的一个重要概念，本身非常简单，一句话就能说清楚，就是指某个函数的最后一步是调用另一个函数

+ 蹦床函数

# 对象
	
+ 成员简写
+ 成员名表达式
+ Object.is(v1, v2)
	- 比较 v1 v2 是否相等
	- 一是+0不等于-0，二是NaN等于自身
	- 用 === 比较
+ Object.assign(target, ...source)
	- 将源 source 所有可枚举属性，复制到目标对象 target
	- 参数不是能 undefined null
	- 字符串会以数组形式，拷贝入目标对象
	- 同名属性会替换
	- 浅拷贝
	- 可以用来处理数组
	- 会忽略 enumerable 为 false 的属性
+ Object.getOwnPropertyDescriptor(obj, prop)
	- 得到 obj 对应 prop 属性的描述对象
	- ES6规定，所有 Class 的原型的方法都是不可枚举的

+ 遍历
	+ for...in
		- 只遍历对象自身的和继承的可枚举的属性
	+ Object.keys()
		- 返回对象自身的所有可枚举的属性的键名
	+ JSON.stringify()
		- 只串行化对象自身的可枚举的属性
	+ Object.getOwnPropertyName()
		- 返回一个数组，包含对象自身的所有属性 和 不可枚举属性
		- 不含Symbol属性
	+ Object.getOwnPropertySymbols(obj)
		- 返回一个数组，包含对象自身的所有 Symbol 属性
	+ Reflect.ownKeys(obj)
		- 返回所有类型的键名
+ Object.entries()
	- 方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组	



# Symbol

一种新的原始数据类型, 表示独一无二的值

它是JavaScript语言的第七种数据类型

前六种是：Undefined、Null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）

Symbol 函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的Symbol函数的返回值是不相等的

Symbol 不能与其他类型的值进行运算,可以显式转为字符串,也可以转为布尔值

Symbol 可作为对象属性名，不能用点运算符

任何值都不可能有相同的值了

*注意 Symbol 的值不是一个对象*


+ Symbol.for(kes)
	- Symbol.for为 Symbol 值登记的 key，是全局环境的
	- Symbol.for()不会每次调用就返回一个新的 Symbol 类型的值，而是会先检查给定的key是否已经存在，如果不存在才会新建一个值
+ Symbol.keyFor()
	- 获取 Symbol.for() 定义的 键名 

+ 十一个内置的 Symbol 值
	- `Symbol.hasInstance` 	使用 `instanceof` 时调用
	- `Symbol.isConcatSpreadable` 表示该对象使用 `Array.prototype.concat()` 时，是否可以展开。
	- `Symbol.species` 指向当前对象的构造函数，创造实例时，默认会调用这个方法
	- `Symbol.match` `str.match(myObject)` 时，如果该属性存在，会调用它，返回该方法的返回值
	- `Symbol.replace` `String.prototype.replace` 方法调用时，会返回该方法的返回值
	- `Symbol.search` `String.prototype.search` 方法调用时，会返回该方法的返回值
	- `Symbol.split` `String.prototype.split` 方法调用时，会返回该方法的返回值
	- `Symbol.iterator` 指向该对象的默认遍历器方法
	- `Symbol.toPrimitive` 该对象被转为原始类型的值时，会调用这个方法，返回该对象对应的原始类型值
	- `Symbol.toStringTag` 	`Object.prototype.toString` 方法时，如果这个属性存在，它的返回值会出现在 `toString` 方法返回的字符串之中，表示对象的类型
	- `Symbol.unscopables` 该对象指定了使用with关键字时，哪些属性会被with环境排除

# Set 和 Map

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值

Set 本身是一个构造函数，用来生成 Set 数据结构，不会发生内部转换

Set的遍历顺序与插入顺序是一致的

Map 则似于对象，也是键值对的集合，但是 key 的范围不限于字符串,各种类型都可以作为 key

+ `Set()` 可以接受一个数组（或类似数组的对象）作为参数，用来初始化
+ `add(value)`：添加某个值
+ `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
+ `has(value)`：该值是否存在
+ `clear()`：清除所有成员
+ `keys()`： 遍历 key
+ `values()`：遍历 value
+ `entries()`：遍历 key-value
+ `forEach()`：使用回调函数遍历每个成员


## WeakSet 和 WeakMap

该 Set 中成员只能是对象，并且都是弱对象，也就是无法引用 WeakSet 成员

该 Ma 只接受对象作为键名

+ `WeakSet.prototype.add(key, value)`：向 WeakSet 实例添加一个新成员。
+ `WeakSet.prototype.delete(key)`：清除 WeakSet 实例的指定成员。
+ `WeakSet.prototype.has(key)`：返回一个布尔值，表示某个值是否在 WeakSet 实例之中。
+ `WeakMap.prototype.get(key)`：
+ `WeakMap.prototype.set(key, value)`：
+ `WeakMap.prototype.has(key)`：
+ `WeakMap.prototype.delete(key)`：

# 对象代理 `Proxy`

+ `Proxy(target, handler)`
	+ target 被代理的对象
	+ handler 代理时的对象
		1. get(target, propKey, receiver) 拦截属性的读
		2. set(target, propKey, value, receiver) 拦截属性的写
		3. has(target, propKey)	拦截 in 操作
		4. deleteProperty(target, propKey) 拦截 delete 操作
		5. ownKeys(target) 拦截 OwnProperty 或 Object.keys() 操作
		6. getOwnPropertyDescriptor(target, propKey) 拦截 getOwnPropertyDescriptor()
		7. defineProperty(target, propKey, propDesc) 拦截 defineProperty 和 defineProperties 操作
		8. preventExtensions(target) 拦截 preventExtensions 操作
		9. getPrototypeOf(target) 拦截 getPrototypeOf 操作
		10. isExtensible(target) 拦截 isExtensible 操作
		11. setPrototypeOf(target, proto) 拦截 setPrototypeOf 操作
		12. apply(target, object, args) 拦截 ... call apply 操作
		13. construct(target, args) 拦截 new 操作

+ `Proxy.revocable()` 返回一个可取消的 Proxy 实例


# Reflect 

可以拿到语言内部的方法

对一些 in 或 delete 的操作有 函数行为

Reflect对象的方法与Proxy对象的方法一一对应

1.  Reflect.apply(target,thisArg,args)
2.  Reflect.construct(target,args)
3.  Reflect.get(target,name,receiver)
4.  Reflect.set(target,name,value,receiver)
		Reflect.set 会触发 Proxy.defineProperty
5.  Reflect.defineProperty(target,name,desc)
6.  Reflect.deleteProperty(target,name)
7.  Reflect.has(target,name)
8.  Reflect.ownKeys(target)
9.  Reflect.isExtensible(target)
10. Reflect.preventExtensions(target)
11. Reflect.getOwnPropertyDescriptor(target, name)
12. Reflect.getPrototypeOf(target)
13. Reflect.setPrototypeOf(target, prototype)



# Promise

Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大

三种状态： pending, resolved, rejected

无法取消Promise，一旦新建它就会立即执行，无法中途取消

如果不设置回调函数，Promise内部抛出的错误

当处于Pending状态时，无法得知目前进展到哪一个阶段

Promise新建后就会立即执行

+ Promise(res, rej)
+ Promise.prototype.then(doneCallback, faileCallback)
	方法返回的是一个新的Promise实例
	并且与 jQuery 异步队列 then 一样，参数会传递
+ Promise.prototype.catch()
	.then(null, rejection)的别名,	用于指定发生错误时的回调函数
+ Promise.all()
	将多个Promise实例，包装成一个新的Promise实例
+ Promise.race()
	方法同样是将多个Promise实例，包装成一个新的Promise实例
+ Promise.resolve()
	将现有对象转为Promise对象
+ Promise.reject(reason)
	方法也会返回一个新的 Promise 实例，该实例的状态为rejected
+ Promise.try()
	目前是一个提案， Bluebird、Q和when 这些库已经实现

## async
将同步函数变成异步


#  Iterator 和 for...of

Iterator的作用有三个：
	一是为各种数据结构，提供一个统一的、简便的访问接口；
	二是使得数据结构的成员能够按某种次序排列；
	三是 ES6 创造了一种新的遍历命令 for...of 循环，Iterator接口主要供for...of消费


Iterator接口的目的，就是为所有数据结构，提供了一种统一的访问机制，即for...of循环

ES6规定，默认的Iterator接口部署在数据结构的Symbol.iterator属性，或者说，一个数据结构只要具有Symbol.iterator属性，就可以认为是“可遍历的”（iterable）

三类数据结构原生具备Iterator接口：数组、某些类似数组的对象、Set和Map结构

ES6 借鉴 C++、Java、C# 和 Python 语言，引入了for...of循环，作为遍历所有数据结构的统一的方法。


# Generator 函数

Generator 函数是 ES6 提供的一种异步编程解决方案，语法行为与传统函数完全不同

形式上，Generator 函数是一个普通函数，但是有两个特征。一是，function关键字与函数名之间有一个星号；二是，函数体内部使用yield语句，定义不同的内部状态（yield在英语里的意思就是“产出”）

不同的是，调用Generator函数后，该函数并不执行

返回的也不是函数运行结果，而是一个指向内部状态的指针对象

下一步，必须调用遍历器对象的next方法，使得指针移向下一个状态。
换言之，Generator函数是分段执行的，yield语句是暂停执行的标记，而next方法可以恢复执行

Generator函数可以不用yield语句，这时就变成了一个单纯的暂缓执行函数
Generator 函数从暂停状态到恢复运行，它的上下文状态（context）是不变的

+ Generator.prototype.next() 
	参数就会被当作上一个yield语句的返回值
+ Generator.prototype.throw()
	可以在函数体外抛出错误，然后在Generator函数体内捕获
+ Generator.prototype.return()
	可以返回给定的值，并且终结遍历Generator函数


## yeild 语句 

yield语句如果用在一个表达式之中，必须放在圆括号里面
作函数参数或放在赋值表达式的右边，可以不加括号

1. 遇到yield语句，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值
2. 下一次调用next方法时，再继续往下执行，直到遇到下一个yield语句。
3. 如果没有再遇到新的yield语句，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值
4. 如果该函数没有return语句，则返回的对象的value属性值为undefined

## yeild* 语句 

用来在一个 Generator 函数里面执行另一个 Generator 函数

Generator 函数将 JavaScript 异步编程带入了一个全新的阶段

Thunk 函数是自动执行 Generator 函数的一种方法

Generator 四种异步操作之后的第五种异步替代方法

## async 函数

`async函数就是将 Generator 函数的星号（*）替换成async，将yield替换成await，仅此而已`

它就是 Generator 函数的语法糖

async 函数返回一个 Promise 对象

# Class

Class不存在变量提升

虽然是另一种语法，但实致上还是 function

类的数据类型就是函数，类本身就指向构造函数

在 class 类中声明的方法，就是原型上的方法

类的内部所有定义的方法，都是不可枚举的

类也可以使用表达式的形式定义

+ constructor()
	如果没有显式定义，一个空的constructor方法会被默认添加
	默认返回实例对象（即this）

## extends & super

extends 继承
super 表示父类的构造器

在子类的构造函数中，只有调用super之后，才可以使用this关键字

子类的__proto__属性，表示构造函数的继承，总是指向父类

子类prototype属性的__proto__属性，表示方法的继承，总是指向父类的prototype属性

只要是一个有prototype属性的函数，就能被继承

Object.getPrototypeOf方法可以用来从子类上获取父类

子类的构造函数必须执行一次super函数

super指向父类的原型对象

类中可以用 getter/setter 

static关键字，就表示该方法不会被实例继承

`#` 开头的成员表示私有和成员，ES7 的提案

##　new.target属性

如果构造函数不是通过new命令调用的，new.target会返回undefined

Mixin模式指的是，将多个类的接口“混入”（mix in）另一个类

# 类的修饰

修饰器（Decorator）是一个函数，用来修改类的行为。这是ES7的一个提案，目前Babel转码器已经支持


# Module

## export

用于输出变量

## import

用于输入变量, 后面必须加上 {}

## export default

可用这个语句输出一个默认的 default 变量函数，并且可用 import 以任何名字导入
后面不能跟变量声明语句
且 import 后面不能加 {}

浏览器中加载模块用 `<script type="module"></script>`, 这是个异步加载


1. 代码是在模块作用域之中运行，而不是在全局作用域运行。模块内部的顶层变量，外部不可见。
2. 模块脚本自动采用严格模式，不管有没有声明use strict。
3. 模块之中，可以使用import命令加载其他模块（.js后缀不可省略，需要提供绝对 URL 或相对 URL），也可以使用export命令输出对外接口。
4. 模块之中，顶层的this关键字返回undefined，而不是指向window。也就是说，在模块顶层使用this关键字，是无意义的。
5. 同一个模块如果加载多次，将只执行一次。

ES6 模块也允许内嵌在网页中，语法行为与加载外部脚本完全一致
`<script type="mudole"> </script>` 之间

CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
CommonJS 模块是运行时加载，ES6 模块是编译时输出接口

ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块

由于 ES6 输入的模块变量，只是一个“符号连接”，所以这个变量是只读的，对它进行重新赋值会报错

export 输出的实例不同的 import 都是同一个实例

ES6 模块之中，顶层的this指向undefined
CommonJS 模块的顶层this指向当前模块，这是两者的一个重大差异

## 循环加载

两个脚本执行相互依赖加载

ES6模块是动态引用，如果使用import从一个模块加载变量（即import foo from 'foo'），那些变量不会被缓存，而是成为一个指向被加载模块的引用，需要开发者自己保证，真正取值的时候能够取到值

CommonJS模块遇到循环加载时，返回的是当前已经执行的部分的值，而不是代码全部执行后的值，两者可能会有差异

## 严格模式


# 编码风格

1. 用 let 与 const 后者优先
2. 字符串不用双引号
3. 解构赋值, 数组赋值 和 对象参数
5. 对象解构返回多个值
5. 对象最后成员, 单行不加逗号, 多行加逗号
6. Object.assign 方法添加属性
7. 简洁表达法表示成员
8. 表达式创建动态成员名
9. ... 拷贝数组
10. Array.from 转换数组
11. 简单用箭头函数, 否则传统函数
12. rest 取代 arguments
13. 使用函数默认参数语法
14. 实体对象用 Object, key-value 结构用 Map
15. Class 取代 prototype
16. import 取代 require
17. export 取代 module.exports
18. 模块只有一个输出值，使用 export default
19. export default 与 export 不同时使用
20. import 后不要 *
21. exrpot 首字母, 小写函数, 大写对象
22. ESLint 检测语法风格统一

# SIMD 

单指令，多数据

它是 JavaScript 操作 CPU 对应指令的接口，你可以看做这是一种不同的运算执行模式