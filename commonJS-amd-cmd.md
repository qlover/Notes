

# AMD,CMD,CommonJS
这三个规范都是为 js 模块化加载而生的，都是在用到或者预计要用到某些模块时候加载该模块
使得大量的系统巨大的庞杂的代码得以很好的组织和管理
模块化使得我们在使用和管理代码的时候不那么混乱，而且也方便了多人的合作



## CommonJS

一个有目标的构建JavaScript生态系统Web服务器组，在浏览器和命令行应用程序和桌面
CommonJs 是一个更偏向于服务器端的规范
Node.js采用了这个规范
根据 CommonJS 规范，一个单独的文件就是一个模块
加载模块使用require方法，该方法读 取一个文件并执行，最后返回文件内部的exports对象
同步加载模块

require() 用来引入外部模块
exports 对象用于导出当前模块的方法或变量，唯一的导出口
module 对象就代表模块本身。



在 CommonJS 中，有一个全局性方法 require()，用于加载模块
假设有一个 Math.js 模块，就可以这样加载

var math = require('Math');
console.log(math)

模块引用 require
模块定义 exports
模块标识 module

浏览器不兼容 CommonJS 的根本原因，在于缺少四个Node.js环境的变量
module
exports
require
global

也就是说在浏览器不认识这四个全局的变量

这个是在 Node 环境下运行的
```NodeJS
console.log(Module)
//=>
Module {
  id: '<repl>',
  exports: {},
  parent: undefined,
  filename: null,
  loaded: false,
  children: [],
  paths:
   [ 'C:\\Users\\Qlover\\repl\\node_modules',
     'C:\\Users\\Qlover\\node_modules',
     'C:\\Users\\node_modules',
     'C:\\node_modules',
     'C:\\Users\\Qlover\\.node_modules',
     'C:\\Users\\Qlover\\.node_libraries',
     'E:\\Environment\\NodeJS\\lib\\node' ] }
```
其它的也是一样，




## AMD
 Asynchronous Module Definition，中文名是异步模块定义的意思
 define 和 require 这两个定义模块、调用模块的方法，合称为 AMD 模式

(异步加载依赖模块)[https://github.com/amdjs/amdjs-api/wiki/AMD]
	jQuery 1.7+
	Dojo   1.6+



AMD 也采用require([module], callback)语句加载模块，但是不同于CommonJS，它要求两个参数
第一个参数[module]，是一个数组，里面的成员就是要加载的模块
第二个参数callback，则是加载成功之后的回调函数

主要有两个Javascript库实现了AMD规范：require.js 和 curl.js
require([modules], callbacks([modules]))
[modules] 表示所依赖的模块，

### 加载模块

比如，加载一个 jQuery 模块

*见 AMD-main.js*

### 定义模块

如果一个模块不依赖其他模块，那么可以直接定义在 define() 函数之中

*定义见 math.js*
*加载它 见 AMD-main.js*


其实，也不难看出， AMD 就只有二个接口，一个定义，一个加载
而定义 define(id, dependencies, factory)
	id 模块名，省略则为文件名
	dependencies 依赖数组，就是该模块需要的依赖
	factory 工厂方法，为模块初始化要执行的函数或对象

## CMD

则是另一个奇特, 主要是因 seaJS 引起
(http://web.jobbole.com/83761/#article-comment)[http://web.jobbole.com/83761/#article-comment]
(https://blog.csdn.net/xcymorningsun/article/details/52709608)[https://blog.csdn.net/xcymorningsun/article/details/52709608]


# 源生规范


jQuery 中不仅兼容了 CommonJS 还兼容了 AMD 规范，并且还检查了 CMD 的规范
而这三个规范的兼容，则就是 jQuery 最后的一个框架，同时兼容

## CommonJS

有三个全局变量 module, exports, require (global 在 NodeJS 中讨论)
但 AMD 中也有 require ，所以不用 require 检测
提供对外接口：
	1. exports

```js
// 文件名: foo.js
// 依赖
var $ = require('jquery');
// 方法
function foo(){};
 
// 暴露公共方法（一个）
module.exports = foo;
```

## AMD

AMD 也有两个全局变量， require, define 但 与 CommonJS 都有 require 所以检测 define
公有属性 define.amd
提供对外接口
	1. exports.XXX =
	2. return XXX

```js
// 文件名: foo.js
// 依赖
define(['jquery'], function ($) {
    //    方法
    function foo(){};
 
    //    暴露公共方法
    return foo;
});
```



## CMD
与 AMD 类似
公有属性 define.amd
对外接口
	1. exports.XXX
	2. return XXX

```js
// 文件名: foo.js
// 依赖
define(['jquery'], function ($) {
    //    方法
    function foo(){};
 
    //    暴露公共方法
    return foo;
});
```

## UMD 通用规范
```js
// 利用 UMD 规范自己实现一个
// root 	代表当前环境的全局变量是什么
// factory	模块的工厂方法，用于返回一个编写的模块
(function(root, factory) {
	// 1. AMD --> define.amd
	if (typeof define === 'function' && define.amd) {
		// 则用 define() 定义
		define(['aquery'], factory);
	}
	// 2. CommonJS  --> exports || module.exports
	else if (typeof exports === 'object') {
		// CommonJS 以 exports 提供对外接口
		module.exports = factory(require('aquery'));
	}
	// 3. 浏览器全局变量(root 即 window)
	else {
		// window.aQuery = factory() return => aQuery
		root.aQuery = factory(root.aQuery);
	}

}(this, function($) {
	// 定义该模块核心类
	
	// 暴露公共方法
	// 这也是为了给浏览器全局变量设置

}));
```
