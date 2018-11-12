

node install -g n # 安装 node.js 版本管理工具

// 将安装依赖添加到 dependencies
npm install -S [module_name]
npm install --save [module_name]

// 将安装的依赖添加到 devDependencies
npm install -D [module_name]
npm install --save-dev [module_name]


*devDependencies 只用于开发环境*
*dependencies 只用于生产环境*


npm congfig get registry # 查看安装的镜像

原地址为:[http://registry.npmjs.org](http://registry.npmjs.org)

// 将安装的地址换成国内地址
npm config set registry http://registry.npm.taobao.org 

npm install  # 安装当前目录下 package.json 中的所有依赖


## package.json

+ priavte 以便确保我们安装包是私有的(private)

+ main 主要入口文件

+ scripts 添加 npm 脚本执行命令行



