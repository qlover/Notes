React 不是完整的 MVC, MVVM 
跟 Web Components 不冲突
组件化开发

{} 在 jsx 中的表达式执行


# 前端自动化

## 安装 yeoman 和 webpack

`$ npm install [安装的包名]` 
	# 如果上述命令中没有安装的包名,则会找当前目录的 `package.json`, 否则报错
`$ npm install -g yo`
`$ npm install -g bower`
`$ npm install -g grunt-cli`  	# 安装 grunt

`$ yo react-webpack [当前目录名]`  # 生成项目

`$ npm install -g generator-angular` 

`$ grunt [gruntfile.js]`



## 前端集成解决方案
+ Yeoman : 生成项目文件,代码结构
+ Bower :  web 包管理器
+ Grunt | Gulp : build tool

+ CodeKit	Mac 整合
+ FIS 		百度
+ Spirit 	腾讯

## 安装 yeoman 和 webpack

`$ npm install [安装的包名]` 
	# 如果上述命令中没有安装的包名,则会找当前目录的 `package.json`, 否则报错
`$ npm install -g yo`
`$ npm install -g bower`
`$ npm install -g grunt-cli`  								# 安装 grunt
`$ npm install -g generator-react-webpack`
`$ npm ls -g --depth=1 2>/dev/null | grep generator-`
`$ yo react-webpack [当前目录名]` 							 	# 生成项目

`$ npm install -g generator-angular` 

`$ grunt [gruntfile.js]`




# yeoman

generator 是 yeoman 的模具，也就是生成器

`$ yo angular [项目名]`
	- 是否使用 gulp
	? Would you like to use Gulp (experimental) instead of Grunt? No
	- 是否使用 Sass, 如果 yes 要保证 Ruby 环境
	? Would you like to use Sass (with Compass)? Yes
	- 是否包括 bootstrap 
	? Would you like to include Bootstrap? Yes
	- 是否使用 Sass 版本的 Bootstrap
	? Would you like to use the Sass version of Bootstrap? Yes

`$ npm install` # 直接安装 package.json 中指定的包

## 目录结构

+ app/
+ test/
- .bowerrc 		
- .editconfig  		# 指定当前项目代码的风格
- .gitattributes 	# git 的配置荐
- .gitigonre  		# 存放不上传 git 的文件
- .jshintrc  		# jshin 配置文件
- .travis.yml  		# 为开源打造的集成环境
- bower.json 		# bower 配置文件
- Gruntfile.js  	# grunt 配置文件
- package.json  	# 配置文件



#### package.json
```
{
	"name": "project-name",
	"version" : "该应用的版本号",

	// 生产环境的依赖
	"dependencies" : {
		"grunt" : "^0.4.5", // ^ 表宽松限制 ~ 严格版本
		"grunt-contrib-concat": "^0.4.0"
	},
	// 开发的依赖
	"devDependencies":{

	},
	
	// NodeJS 的版本
	"engines":{
		"node": ">=0.10.10"
	},
	// 当前目录执行的命令
	"scripts":{
		"test" : "grunt test" // $ test ==> $ grunt test
		"install" : "" 		// 此时对应 $ npm install 命令
	}
}
```



# bower 

`$ bower install [ 包名 | github 短写 | github 完整地址 | url ]`
`$ bower init` # 初始化当前的 bower 项目, 如果是 windows ,则要在 cmd 中执行
`$ bower install angular --save-dev` 	
	# 将 bower 安装的添加到 bower.json 中的 devDependencies 配置项中

两个配置文件 `.bowerrc` 和 `bower.json`

#### .bowerrc
```
{
	"director" : "bower_components",
	"proxy" : "http://proxy.tencent.com:8080" // 配置 http 代理
	"https-proxy" : "https://proxy.tencent.com:8080" // 配置 https 代理
	"timeout" : '60000 // 超时时间
}
```
#### bower.json 
```
{
  "name": "jquery-bootstrap-in-action",
  "homepage": "https://github.com/qlover/e-web",
  "authors": [
    "qlover <myused@sina.com>"
  ],
  "description": "test bower by using jquery & bootstrap",
  "main": "",
  "keywords": [
    "jquery",
    "bootstrap"
  ],
  "license": "MIT",
  "private": true,
  "ignore": [
    "**/.*",
    "node_modules",
    "bower_components",
    "test",
    "tests"
  ],
  "dependencies": {
    "bootstrap": "^4.1.1",
    "jquery": "jquery/jquery#^3.3.1"
  }
}

```

### license 
- ISC
- 



# grunt


## 使用 webapp 搭建项目

`$ npm install -g generator-webapp`  # 安装 webapp 指令
`$ yo webapp [项目名]`  # 使用 webapp 构建项目
	- Modernizr 用来检测用户浏览器是否支持某些 H5 和 CSS3 新特性的库


*如果在 yeoman 的 generator-webapp 已经不在用grunt而是用gulp，那怎么yo webapp是生成gruntfile.js文件了。解决方法有两种*

### 第一种方法：用安装对应的npm版本包

`$ npm isntall -g generator-webapp@v0.5.0`
	# 安装指定的 webapp ,而不是最新的，避免生成 gulpfile.js

### 用 github 的 branch 的 tag 和 npm link 来解决

[详见](https://www.cnblogs.com/liangcheng11/p/6825587.html)


> You don't seem to have a generator with the name “mocha” installed.
`$ npm install -g generator-mocha`

确定了 webapp 版本后

`$ grunt [task 名字[:生成的目录名]]`

`$ npm init` # 初始化当前项目，得到 package.json 		
`$ npm install grunt --save-dev`  # 安装 package 时更新到 devDependencies 中	

几个插件

`$ npm install load-grunt-tasks --save-dev`
`$ npm install time-grunt --save-dev`
`$ npm install grunt-contrib-copy --save-dev`
`$ npm install grunt-contrib-clean --save-dev`

`$ grunt copy` 	# 执行配置后的 grunt-contrib-copy 复制命令
`$ grunt clean`  	# 执行配置后的 grunt-contrib-clean 清楚命令

## 目录结构

+ app/
+ bower_components/
+ node_modules/
+ test/
- .editorconfig
- .gitattributes
- .gitignore
- .jshintrc
- .yo-rc.json
- .bower.json
- Gruntfile.js
- package.json


### gruntfile.js  `grunt 配置文件`

```
// 额外参数, 后可接一个 node 中的 fs 中的一个方法
// 也可以是一个函数
filter: function(filepath) {
	return (!grunt.file.isDir(filepath));
},
// 匹配以点开头的文件
dot: true,
// 只匹配路径最后的一个名字
matchBase: true,
// 处理 src 到 dest 的动态映射
expand: true,
cwd: '<%= config.app %>/',
// 只复制源路径的 .html 文件
src: '*.html',
dest: '<%= config.dist %>/',
// 是否更改后缀名
ext: '.min.html',
// 从那开始修改后缀名
// frist 从第一个点
// last 从最后一个点
extDot: 'first',
// 是否将中间的文件夹去掉
flatten: true,
// 在 ext, extDot 执行后调用
// 重命名
rename: function(dest, src) {
	return dest + 'js/' + src;
}
```

## Grunt 的三个结构
+ Task 执行时的任务， 其实就是 grunt.initConfig({}) 的每个键名，以及 grunt.registerTask() 注册的
	比如 `grunt copy` `grunt server`
	见 gruntfile.js 中的可运行的任务

	+ server 

	`$ grunt server` 	# 运行该项目，修改文件，会自动更新

	修改 /app/index.html 
		
		```>> File "app\index.html" changed.```

	+ wiredep 	重新启动
	+ concurent:server 并行执行
	+ autoprefixer 需要添加 css 厂商前缀的属性自动添加 
	+ connect:livereload 
	+ watch 监听本地文件修改
	+ build 

+ Target 执行任务时作用的目标， 其实就是 grunt.initConfig({}) 的每个键名值的键名
	比如 `grunt copy:dist` 

+ Option 执行任务时的参数， 命令行中最后跟上的参数
	比如 `grunt server --allow-remote` 表示修改访问地址
	


## 组合 Tasks 


+ server

+ test

+ build
	`$ grunt build`

	+ dist/ 
		该目录则是最后项目的目录
	+ robots.txt 爬虫文件

	+ concat 任务
		合并文件
```javscript
concat : {
	options : {
		separator : ';' // 指定合并文件时，加分号分隔
	},
	dist : {
		src : [], // 合并的源文件列表
		dest : [], // 合并后的文件列表
	}
}
```
	+ htmlmin 任务
```
options: {
  // readonly = "readonly"
  collapseBooleanAttributes: true,
  // 是否清除 script, style, pre, textarea 里边的空格
  collapseWhitespace: true,
  // 所有空格保留成一个空格
  conservativeCollapse: true,
  // 属性值是否省去引号
  removeAttributeQuotes: true,
  // 去掉 script style 中 HTML 注释内容
  removeCommentsFromCDATA: true,
  // 是否删除掉值为空的属性
  removeEmptyAttributes: true,
  // 是否去掉对浏览器渲染没有效果的标签
  removeOptionalTags: true,
  // 相对于 input 中 type = text 的情况
  // 因为不写默认也有，如果为 true ，则不能用 type='text' 属性选择器
  removeRedundantAttributes: true,
  // 就否用 HTML5 的声明方式
  useShortDoctype: true
},
```

## Plugins 插件


`npm install -g generator-gruntplugin` # 安装 yoeman 的插件 generator
`yo gruntplugin [项目名]` # 利用 yo 的 gruntplugin 构建一个项目


# gulp


`npm install -g gulp` # 安装 gulp 
`npm install -g generator-gulp-webapp@0.2.0` # 安装 gulp 的 webapp 构建工具
`yo gulp-webapp` # 使用 gulp-webapp 生成项目

`gulp serve` # 启动本地的 web server




## 组合 Task

+ serve

+ build

+ default



Content Delivery Network(CDN 内容分发网络)
	长缓存： Cache-Control: max-ag = 2592000

index.html Http Header -> Cache-Control: no-cache
<script src="js/a.js?ts=235252342"></script>

iterm 控制台
cakeBrew
PhantomJS
mochaJS
chaiJS
kaleidoscope对比工具 MAC