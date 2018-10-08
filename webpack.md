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



# webPack 

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