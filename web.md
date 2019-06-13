
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

HTTP 缓存的最大用处
1. 缓解服务器端压力
2. 提升性能(获取资源的耗时更短了)

#### Cache-Control (req/res) 

1. no-store 禁止 req/res 进行缓存，每一次的 req 都会下载完整的响应内容
2. no-cache 强制确认缓存， server 对 req 进行确认， cache 是否过期, 没过期就使用 cache
3. public/private(缺省) public 会被任何中间人缓存,private 浏览器私有缓存

#### If-None-Match || If-Modified-Since (req)

理论上缓存是可以永久保存的,但只有有限的存储空间

该请求头信息可以检测服务器资源是否更新

缓存失效计算公式
```
expirationTime = responseTime + freshnessLifetime - currentAge
```

#### 加速资源

对于一些低频率更新的资源,比如(css,js) 的 URL 后面加上一个版本号，该资源就会被视作完全新的资源，不好的地方就是每一处都会更新

而更好的就是当低频更新的资源（js/css）变动了，只用在高频变动的资源文件（html）里做入口的改动

### Cookie

1. 会话状态管理
2. 修改化设置
3. 浏览器行为追踪

现代浏览器中 Cookie 已经渐渐被淘汰,取而代之是 `Web Storage` 或 `IndexDB`

#### SetCookie res

服务器响应头,指定设置 Cookie

#### Cookie req
客户端接收到响应,后服务端有 Set Cookie 头信息,则会用 Cookie 头信息将保存的 Cookie 信息发送出去

res
```
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: name=qlover
Sst-Cookie: age=20
```

req
```
GET /login.php HTTP/1.0
HOST: local.php.com
Cookie: name=qlover; age=20
```

Secure 标记,如果在 req 中 Cookie 中携带了 Secure 标记,则 req 只会从 HTTPS 发送

但 Cookie 发送数据太不安全,所以在 Chrome52+ 和 FF52+ `http:` 将无法使用 Secure

HttpOnly 标记,则会将该 Cookie 作用于客户端与服务器之间,也就是说普通的 `document.cookie` API 则无法访问带有 HttpOnly 标记的 Cookie

可以避免 XSS 攻击

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

## HTTP访问控制(CORS)

`CORS` 跨域资源共享，使用额外的 HTTP 请求头来允许不同源服务器上指定的资源

XHR 和 Fetch 遵循同源策略

### 什么情况需要 CORS

· 前文提到的由 XMLHttpRequest 或 Fetch 发起的跨域 HTTP 请求。
· Web 字体 (CSS 中通过 @font-face 使用跨域字体资源), 因此，网站就可以发布 TrueType 字体资源，并只允许已授权网站进行跨站调用。
· WebGL 贴图
· 使用 drawImage 将 Images/video 画面绘制到 canvas
· 样式表（使用 CSSOM）

跨域资源共享在 HTTP 头部声明一组字段,使其能够通过浏览器有权限访问哪些资源

如果 HTTP 请求会对服务器有副作用, 浏览器则必须先用 `OPTIONS` 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨域请求,是否需要谁也会在该请求方式中得到响应

req 发送 `origin` 字段, res 响应 `Access-Control-Allow-Origin`,如果 `origin` 来源在 `Access-Control-Allow-Origin` 中则是达成 CORS,这也是`简单请求`完成的最简单的访问控制

### 简单请求

完全满足五个条件就是简单请求
1. HTTP 请求方法是 GET | HEAD | POST
2. *不得人为设置* `CORS 安全的首部字段集合` 之外的首部字段
  Accept,Accept-Language,Content-Language,Content-Type (需要注意额外的限制),DPR,Downlink,Save-Data,Viewport-Width,Width
3. Content-Type text/palin | multipart/form-data | application/x-www-form-urlencoded
4. `XMLHttpRequestUpload` 没有任何监听事件
5. 请求不包括 `ReadableStream` 对象

### 预检请求

当 HTTP 请求对服务器有副作用,浏览器必须先完成预检请求
完全满足以下五个条件
1. PUT | DELETE | CONNECT |OPTIONS | TRACE | PATCH
2. *人为设置* `CORS 安全的首部字段集合` 之外的首部字段
  Accept,Accept-Language,Content-Language,Content-Type (需要注意额外的限制),DPR,Downlink,Save-Data,Viewport-Width,Width
3. Content-Type *不属于* application/x-www-form-urlencoded | multipart/form-data | text/plain 其中之一
4. `XMLHttpRequestUpload` 注册任意多个监听事件
5. 使用了 `ReadableStream` 对象

预检请求与响应
```
 1.OPTIONS /resources/post-here/ HTTP/1.1
 2.Host: bar.other
 3.User-Agent: Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.5; en-US; rv:1.9.1b3pre) Gecko/20081130 Minefield/3.1b3pre
 4.Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
 5.Accept-Language: en-us,en;q=0.5
 6.Accept-Encoding: gzip,deflate
 7.Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
 8.Connection: keep-alive
 9.Origin: http://foo.example
10.Access-Control-Request-Method: POST # 告知服务器实际请求时用 POST 方法
11.Access-Control-Request-Headers: X-PINGOTHER, Content-Type # 告知服务器实际请求将会携带两个自定义请求头部字段
12.
13.
14.HTTP/1.1 200 OK
15.Date: Mon, 01 Dec 2008 01:15:39 GMT
16.Server: Apache/2.0.61 (Unix)
17.Access-Control-Allow-Origin: http://foo.example
18.Access-Control-Allow-Methods: POST, GET, OPTIONS # 预检响应,服务器允许POST GET OPTIONS 
19.Access-Control-Allow-Headers: X-PINGOTHER, Content-Type # 允许
20.Access-Control-Max-Age: 86400 # 该预检请求有效期 86400s
21.Vary: Accept-Encoding, Origin
22.Content-Encoding: gzip
23.Content-Length: 0
24.Keep-Alive: timeout=2, max=100
25.Connection: Keep-Alive
26.Content-Type: text/plain
```
实际请求与响应
```
POST /resources/post-here/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.5; en-US; rv:1.9.1b3pre) Gecko/20081130 Minefield/3.1b3pre
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
Connection: keep-alive
X-PINGOTHER: pingpong
Content-Type: text/xml; charset=UTF-8
Referer: http://foo.example/examples/preflightInvocation.html
Content-Length: 55
Origin: http://foo.example
Pragma: no-cache
Cache-Control: no-cache

<?xml version="1.0"?><person><name>Arun</name></person>


HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:40 GMT
Server: Apache/2.0.61 (Unix)
Access-Control-Allow-Origin: http://foo.example
Vary: Accept-Encoding, Origin
Content-Encoding: gzip
Content-Length: 235
Keep-Alive: timeout=2, max=99
Connection: Keep-Alive
Content-Type: text/plain

[Some GZIP'd payload]

```
*预检重定向被 CORS 废弃*

### CORS req 字段
```
# 源站，不管是否跨域该字段都会被发送
Origin: <origin>
# 告知实际请求时使用的 HTTP 请求方法
Access-Control-Request-Method: <method>
# 告知实际请求时携带的头
Access-Control-Request-Headers: <field-name>[, <field-name>]*
```
### CORS res 字段
```
Access-Control-Allow-Origin: <origin> | *
# 允许将访问的头放入白名单
Access-Control-Expose-Headers: X-My-Custom-Header, X-Another-Custom-Header
# 指定预检请求缓存多长时间
Access-Control-Max-Age: <delta-seconds>
Access-Control-Allow-Credentials: true
# 允许的 HTTP 请求方法
Access-Control-Allow-Methods: <method>[, <method>]*
# 允许的 HTTP 请求头
Access-Control-Allow-Headers: <field-name>[, <field-name>]*
```




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



Commit Message 格式
```
<type>(<scope>): <subject>
<空行>
<body>
<空行>
<footer>
```
