
# 计算机(内存,CPU,X64&X86)

# 程序(程序=算法+数据结构,栈,堆,树,图...)

# 网络(internet,http,tcp,udp...)

## HTTP

HTTP是一个client-server协议，其作用有:
1. 缓存
2. 开放同源限制
3. 认证
4. 代理和隧道
5. 会话(服务端的请求状态)

请求
```
GET / HTTP/1.1 # GET Method / Path HTTP/1.1 HTTP协议版本
Host: developer.mozilla.org # Headers
Accept-Languaage: fr # Headers
```

响应
```
HTTP/1.1 200 OK # 200 状态码 Ok 状态消息
Date: ....
Server: ...
Last-Modified: ...
```

### HTTP 缓存

# 请求/响应(req,res)

# e.设计(页面架构,针对不同显示用不同单位等)

# 浏览器(内核,同源策略原理,渲染...)

## 同源策咯

1. URL 端口一样(IE除外)
2. 域名一样
3. 协议一样

`about:blank` 和 `javascript:` 会打开该 UTL 的文档源,因为这两个 URL 没有明确的包含有关原始服务器的信息

使用 `document.domain` 必须将父域和子域中设置相同的 `document.domain`

检测 `Cross-Site Request Forgery` (CSRF) 可以阻止跨源访问

### CORS

`CORS` 跨域资源共享，使用额外的 HTTP 请求头来允许不同源服务器上指定的资源

XHR 和 Fetch 遵循同源策略



# 文档(DOM标准,语义化,元素,属性,结构)

# 文档规范(XML,HTML,HTML5)

# 布局(居中,自适应,响应式,流式,浮动,定位...)

# 元素(block,inline,inline-block,table-cell,flex...)

# 元数据(head,title,meta,a,link...)

# 样式(CSS规范,CSS2.0,盒子模型,书写规范,CSS3,兼容性,布局实现,页面实现,重用性高)

# 脚本

# JavaScript 基础(作用域/作用域链,运算符,nudefind&null,变量对象,提升,底层兼容(事件...))

# JavaScript 基础(函数一等公民,闭包,原型,this,三种对象(内置,宿主...))

# JavaScript 基础(callbacks/deffered,异步,Promise,Genterenr,await/async)

# JavaScript 基础(AMD,UMD,ES6,TypeScript(静态),Node.JS(包管理))

# JavaScript 基础(客户端请求,跨域(core,jsonp...),缓存)

# 框架/库(jQuery-raw,Vue,React,Angluar)

# 前端(html+css+js,分离/不分离,自动化/脚手架(webpack,gulp,yo...),插件,SPA,SEO,性能测试,优化...)

# UI(字体,图片,移动端,排版,布局设计(响应式设计,尺寸设计))

# UX()

# 动画(数学,逻辑,计算机科学,AI,big-data,Colud-computer)

# 前后链接(动态加载,分离加载,模板引擎)

# 后台(数据库设计,表设计,冗余/多表,查询,PHP,Node.js,Java,Python)

# ...

# https://leohxj.gitbooks.io/front-end-database/content/index.html


