
# webpack4 构建 react16

首先需要以下依赖
1. node 10.3.0
2. npm 6.1.0
3. webpack 4.16.1
4. react 16.4.1
5. babel-core 6.26.3
6. babel-loader 7.1.5
7. babel-preset-env 1.7.0
8. babel-preset-react 6.24.1


`npm init -y`
`npm i webpack webpack-cli -D`
`npm install react react-dom --save`

因为react中使用JSX语法, 所以我们需要 babel来编译它
`npm i babel-core babel-loader@7.1.5 babel-preset-env babel-preset-react -D`

*注意，因为 react16 所需要的是 babel-loader 7.1.5 版本*
`npm i babel-loader@7.1.5 -D`

　　其中babel-core是核心文件, babel6推荐使用 babel-preset-env 来对ES2015及更高版本进行转换, babel-preset-react能够转换JSX语法

.babelrc
```
{
    "presets": [
        "env",
        "react"
    ]
}
```

## less

`npm install -S css-lodaer less style-lodaer less-loader`

webpack.config.js
```
{
	test: /\.less$/,
	use: [
		'style-loader', 'css-loader', 'less-loader'
	]
}
```

HtmlWebpackPlugin({
	title: 'xxx', // 可以在模板文件中用 <%= htmlWebpackPlugin.options.title %>
	inject: false // 可以取消默认添加
	minify: {  // 对文件进行压缩

}
})

output: {
	publicPath: 'http://cdn.com/' // 表示上线上会将地址前面替换成该一个绝对路径
}


# webpack


`npm init -y` # 初始化 npm, 也就是得到一个默认的 package.json

`npm install --save-dev webpack`  # 在本地安装 webpack

`npm install webpack-cli --save-dev` # 安装 webpack-cli 命令行

`npx webpack` # 执行，可以运行在初始安装的 webpack 包(package)的 webpack 二进制文件

// 如果有了 webpack.config.js 则重新部署执行
`npx webpack --config webpack.config.js`


`npm run build` # 就能代替 npx 的执行
	如果在 package.json 文件中添加了 build 脚本命令行则
	通过向 `npm run build` 命令和你的参数之间添加两个中横线，可以将自定义参数传递给 webpack
	例如：`npm run build -- --colors`

`npm install --save-dev style-loader css-loader` # 加载 css

`npm install --save-dev file-loader` # 加载图片

// 可处理 CSV TSV XML 这三类文件
`npm install --save-dev csv-loader xml-loader` # 加载数据

`npm install --save-dev html-webpack-plugin` # 管理插件

`npm install --save-dev clean-webpack-plugin` # 清除插件

`devtool: 'inline-source-map'` # webpack.config.js 添加启动 source map

## 自动编译

1. webpack's Watch Mode(观察者模式)
	
	package.json: scripts->`"watch": "webpack --watch"`

	运行 npm run watch

	没有自己的服务器，自动编译，不能自动刷新浏览器

2. webpack-dev-server
		
	安装: `npm install --save-dev webpack-dev-server` 

	package.json `"start": "webpack-dev-server --open"`

	webpack.config.js
```
devServer: {
    // 在 localhost:8080 下建立服务
    // 将 dist 目录下的文件，作为可访问文件
    contentBase: './dist'
},
```

	运行 npm start
	
	默认实时刷新的服务器 localhost:8080


3. webpack-dev-middleware 与 自定义的 express 服务启动
	
	webpack.config.js:
```
output: {
    // 添加 puhlicPath,配置 dev-middleware 能够正常启用
    publicPath: '/'
},
```
	安装 `npm install --save-dev express webpack-dev-middleware`

	package.json `"server": "node server.js"`
	
	服务器 http://localhost:3000

	运行 npm run server

4. 模块热更新

## path.join AND path.reslove
```
> path.join('app','dist')
'app\\dist'
> path.resolve('app','dist')
'C:\\Users\\Qlover\\app\\dist'
```
path.join 只对当前的工作目录进行拼接，可以说是相对的路径
path.resolve 返回绝对的路径 




## 生产环境构建

// 通过“通用”配置，我们不必在环境特定(environment-specific)的配置中重复代码
`npm install --save-dev webpack-merge` 


再分别创建三个 common dev prod 三个不同的 webpack 配置文件

// 更改 dev-server 的命令行,指定使用 webpack.dev.js 配置文件
"start": "webpack-dev-server --open --config webpack.dev.js"

// 更改 构建命令行,指定使用 webpack.prod.js 配置文件
"build": "webpack --config webpack.prod.js"


## tree shaking

tree shaking 是一个术语，通常用于描述移除 JavaScript 上下文中的未引用代码(dead-code)。它依赖于 ES2015 模块系统中的静态结构特性，例如 import 和 export。这个术语和概念实际上是兴起于 ES2015 模块打包工具 rollup。



## 指定环境 


一些 library 可能针对具体用户的环境进行代码优化，从而删除或添加一些重要代码。
可以使用 webpack 内置的 DefinePlugin 为所有的依赖定义这个变量,在 prod 配置文件中添加

`process.env.NODE_ENV === 'production'`



## 动态引入


1. import()

import() 会返回一个 promise，因此它可以和 async 函数一起使用。但是，需要使用像 Babel 这样的预处理器和Syntax Dynamic Import Babel Plugin。

注意当调用 ES6 模块的 import() 方法（引入模块）时，必须指向模块的 .default 值，因为它才是 promise 被处理后返回的实际的 module 对象。

2. require.ensure



// webpack4 安装 babel 加载器
`npm install -D babel-loader @babel/core @babel/preset-env webpack`



## 创建 library


externals 配置












# 安装构造 React 的脚手架工具 webpack

`$ npm install -g generator-react-webpack@1.2.11`
`$ npm ls -g --depth=1 2>/dev/null | grep generator-`
`$ yo react-webpack [项目名]` # 构建项目
	是否使用路由功能，单页面不用路由功能
	? Would you like to include react-router? (Y/n) N
	？Would you like to use one of these architectures? N
	使用 CSS 语言
	? Which styles language you want to use? Sass
	后缀名
	? Which file suffix do you want to use for components? .js

`$ grunt serve` # 启动 web-server

如果出现以下错误
```
ERROR in ./src/images/yeoman.png
Module build failed: Error: Cannot find module 'file-loader'
```
`$ npm install file-loader --save-dev` # 安装相应 file-loader

[github 地址](https://github.com/webpack-contrib/file-loader)

React Developer tool 这是为了调试 React 的一个控制台工具

+ webpack.config.js 开发环境配置
+ webpack.dist.config.js 生产环境配置

全局变量 __REACT__DEVTOOLS_GLOBAL_HOOK__ 



# grunt

`$ grunt build` # 生成最终的项目

如出现这样的错误，则在 GalleryByReactApp.js 最后留一个空行
```
ERROR in ./src/components/GalleryByReactApp.js
E:\Web\React\gallery-by-react\src\components\GalleryByReactApp.js
  26:1  error  Newline required at end of file but not found  eol-last
```

+ webpack.config.js

+ webpack.dist.config.js

自动添加 css 头的模块
`$ npm install autoprefixer-loader --save-dev` # 将这个依赖添加到 dev 配置中
`$ npm install json-loader --save-dev` # 安装 webpack 的 json 加载器
	在对应的 webpack 配置文件中添加 json 对应的解析

如出现错
```
Cannot find module 'node-sass'
 @ ./src/styles/main.scss 4:14-353 11:1-15:3 12:19-358
```
则用安装 node-sass 和 sass-loader
`npm install sass-loader node-sass --save-dev`


[参考地址](https://github.com/materliu/gallery-by-react)

# React Components 

1. Mounted
	RC 被 render 解析生成对应的 DOM 节点，并插入浏览器的 DOM 结构的一个过程
2. Update
	一个 mounted 的 RC 被重新 render 的过程
	每一个状态 React 都封装了对应的 hook 函数
3. Unmounted


# SASS

@at-root 指令 将该选择器编译后置于最外层