
# 计算机(内存,CPU,X64&X86)

# 程序(程序=算法+数据结构,栈,堆,树,图...)

# 网络(internet,http,tcp,udp,ssl,加密)


## TCP/IP模型

网络结构有两种主流的分层方式: OSI 七层模型和 TCP/IP 四层模型

TCP/IP是指传输控制协议/网间协议，是目前世界上应用最广的协议

## UDP


### TCP 三次握手(建立链接)

以 A(Client) 与 B(Server) 通信为例子
· 第一次, A 向 B 发送消息, 收到消息。A 确认自己的发信能力和 B 的接收能力
· 第二次, B 向 A 发送消息, A 收到消息。A 确认自己的发送能力和接收能力，A 也确认B 的发送能力和接收能力
· 第三次, 最后 A 再向 B 发送消息, B 接收到消息。B 可确认 A 的接收能力和 B 的发送能力

Http和Https协议请求时都会通过Tcp三次握手建立Tcp连接。

通过三次握手，A和B都能确认自己和对方的收发信能力，相当于建立了互相的信任，就可以开始通信了

### TCP 四次挥手(断开链接)

- 第一次挥手：主机1（可以是客户端或服务器），设置seq和ack向主机2发送一个FIN报文段，此时主机1进入FIN_WAIT_1状态，表示没有数据要发送给主机2了
- 第二次挥手：主机2收到主机1的FIN报文段，向主机1回应一个ACK报文段，表示同意关闭请求，主机1进入FIN_WAIT_2状态。
- 第三次挥手：主机2向主机1发送FIN报文段，请求关闭连接，主机2进入LAST_ACK状态。
- 第四次挥手：主机1收到主机2的FIN报文段，想主机2回应ACK报文段，然后主机1进入TIME_WAIT状态；主机2收到主机1的ACK报文段后，关闭连接。此时主机1等待主机2一段时间后，没有收到回复，证明主机2已经正常关闭，主机1页关闭连接。

主机1向主机2断开链接,主机1向主机2说我没有要发送的数据了要求断开链接,主机2回应说同意你断开链接,主机1就说那我断开链接,主机2说好的准备断开,最后主机1等待链接被关闭

握手协议是指主要用来让客户端及服务器确认彼此的身份的一类网络协议

除此之外，为了保护SSL记录封包中传送的数据，握手协议还能协助双方选择连接时所使用的加密算法、MAC算法及相关密钥

在传送应用程序的数据前，必须使用握手协议

### OSI 与 TCP/IP 区别
1. OSI 采用七层模型, TCP/IP 四层模型
2. TCP/IP 网络接口层没有真正定义，只是概念性的描述。OSI 把它分成 2 层，每一层功能详尽。
3. 在协议开发之前，就有了OSI模型，所以OSI模型具有共通性，而TCP/IP是基于协议建立的模型，不适用于非TCP/IP的网络
4. 实际应用中，OSI模型是理论上的模型，没有成熟的产品；而TCP/IP已经成为国际标准

### TCP 与 UDP 

 \ | UDP | TCP
:-: | :-: | :-: 
是否可靠 |	不可靠传输，不使用流量控制和拥塞控制	| 可靠传输，使用流量控制和拥塞控制|
连接对象个数 |	支持一对一，一对多，多对一和多对多交互通信	| 只能是一对一通信|
传输方式 |	面向报文	| 面向字节流 |
首部开销 |	首部开销小，仅 8 字节	首部最小 20 字节，最大 60 | 字节 |
适用场景 |	适用于实时应用（IP 电话、视频会议、直播等）	| 适用于要求可靠传输的应用，例如文件传输 |


## HTTP

https://http2.akamai.com/demo

HTTP 是基于 TCP/IP 协议的应用层协议，它不涉及数据包传输，主要规定了客户端可服务区之间的通信格式

HTTP是半双工协议，在同一时刻数据只能单向流动

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

### 请求方法

`GET`方法请求一个指定资源的表示形式. 使用GET的请求应该只被用于获取数据.
`HEAD`方法请求一个与GET请求的响应相同的响应，但没有响应体.
`POST`方法用于将实体提交到指定的资源，通常导致在服务器上的状态变化或副作用. 
`PUT`方法用请求有效载荷替换目标资源的所有当前表示。
`DELETE`方法删除指定的资源。
`CONNECT`方法建立一个到由目标资源标识的服务器的隧道。
`OPTIONS`方法用于描述目标资源的通信选项。
`TRACE`方法沿着到目标资源的路径执行一个消息环回测试。
`PATCH`方法用于对资源应用部分修改。



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


## Https

Https 协议是以安全为目标的 Http 通道，简单来说就是 Http 的安全版

主要是在Http下加入SSL层（现在主流的是SLL/TLS），SSL是Https协议的安全基础,Https 默认端口号为 `443`

SSL 主要分为两个层次,一是握手协议,二则是记录协议

其中记录协议是建立在 TCP 可靠传输协议之上，主要为高层协议提示数据封装、压缩、加密等的基本功能支持

而握手协议则是建立在记录协议之上,主要则是通讯双方的认证,协商加密算法以及交换加密密钥

SSL 工作过程如下:
```
发送方的工作过程为：
  从上层接受要发送的数据（包括各种消息和数据）
  对信息进行分段，分成若干纪录
  使用指定的压缩算法进行数据压缩（可选）
  使用指定的MAC算法生成MAC
  使用指定的加密算法进行数据加密
  加SSL记录协议的头，发送数据。
接收方的工作过程为：
  接收数据，从SSL记录协议的头中获取相关信息
  使用指定的解密算法解密数据
  使用指定的MAC算法校验MAC
  使用压缩算法对数据解压缩（在需要进行）
  将记录进行数据重组
  将数据发送给高层。
```

*MAC算法 (Message Authentication Codes) 带秘密密钥的Hash函数：消息的散列值由只有通信双方知道的秘密密钥K来控制*


### SLL 证书

SSL证书是一个二进制文件，里面包含经过认证的网站公钥和一些元数据，需要从经销商购买。

证书有很多类型，按认证级别分类：
1. 域名认证（DV=Domain Validation）：最低级别的认证，可以确认申请人拥有这个域名
2. 公司认证（OV=Organization Validation）：确认域名所有人是哪家公司，证书里面包含公司的信息
3. 扩展认证（EV=Extended Validation）：最高级别认证，浏览器地址栏会显示公司名称。


### Http 和 Https 的区别如下

1. https协议需要到CA申请证书，大多数情况下需要一定费用
2. Http是超文本传输协议，信息采用明文传输，Https则是具有安全性SSL加密传输协议
3. Http和Https端口号不一样，Http是80端口，Https是443端口
4. Http连接是无状态的，而Https采用Http+SSL构建可进行加密传输、身份认证的网络协议，更安全。
5. Http协议建立连接的过程比Https协议快。因为Https除了Tcp三次握手，还要经过SSL握手。连接建立之后数据传输速度，二者无明显区别。

## 加密算法

+ 对称加密
  - 加解使用同一个密钥
  - 效率高，速度快，适合大数据量加解密
+ 非对称加密
  - 加解密使用不同密钥,公钥与密钥
  - 速度慢，但安全性高
+ Hash 算法
  - 只有加密无解密过程

### RSA

Https协议就是使用RSA加密算法，可以说RSA加密算法是宇宙中最重要的加密算法

加密过程[http://www.ruanyifeng.com/blog/2013/07/rsa_algorithm_part_two.html](http://www.ruanyifeng.com/blog/2013/07/rsa_algorithm_part_two.html)


## WebSocket

WebSocket是一种全新的协议，随着HTML5草案的不断完善，越来越多的现代浏览器开始全面支持WebSocket技术了，它将TCP的Socket（套接字）应用在了webpage上，从而使通信双方建立起一个保持在活动状态连接通道

但还是建立在 HTTP 协议基础上的,所以连接发起方依然是客户端

而一旦确立 WebSocket 通信连接，不论服务器还是客户端，任意一方都可直接向对方发送报文

WebSocket 是类似 `Socket` 的TCP长连接的通讯模式，一旦 WebSocket 连接建立后，后续数据都以帧序列的形式传输




# 请求/响应(req,res)

HTTP消息由采用ASCII编码的多行文本构成

在HTTP/1.1及早期版本中，这些消息通过连接公开地发送,在HTTP/2中，为了优化和性能方面的改进，曾经可人工阅读的消息被分到多个HTTP帧中

起始行和  HTTP 消息中的HTTP 头统称为请求头，而其有效负载被称为消息正文

## req
### 起始行

HTTP请求是由客户端发出的消息，用来使服务器执行动作。起始行 (start-line) 包含三个元素：
1. HTTP 请求方法 GET | POST | OPTION | DELETE ...
2. 请求目标 (request target)，通常是一个 URL，或者是协议、端口和域名的绝对路径，通常以请求的环境为特征
  请求的格式因不同的 HTTP 方法而异。它可以是：
  · 一个绝对路径，末尾跟上一个 ' ? ' 和查询字符串。这是最常见的形式，称为 原始形式 (origin form)，被 GET，POST，HEAD 和 OPTIONS 方法所使用。
    ```
    POST / HTTP 1.1
    GET /background.png HTTP/1.0
    HEAD /test.html?query=alibaba HTTP/1.1
    OPTIONS /anypage.html HTTP/1.0
    ```
  · 一个完整的URL，被称为 绝对形式 (absolute form)，主要在 GET 连接到代理时使用。
    ```GET http://developer.mozilla.org/en-US/docs/Web/HTTP/Messages HTTP/1.1```
  ·由域名和可选端口（以':'为前缀）组成的 URL 的 authority component，称为 authority form。 仅在使用 CONNECT 建立 HTTP 隧道时才使用。
    ```CONNECT developer.mozilla.org:80 HTTP/1.1```
  ·星号形式 (asterisk form)，一个简单的星号('*')，配合 OPTIONS 方法使用，代表整个服务器。
    ```OPTIONS * HTTP/1.1```
3. HTTP 版本 (HTTP version)，定义了剩余报文的结构，作为对期望的响应版本的指示符。

### Header

来自请求的 (HTTP headers)[https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers] 遵循和 HTTP header 相同的基本结构：不区分大小写的字符串，紧跟着的冒号 (':') 和一个结构取决于 header 的值。 整个 header（包括值）由一行组成，这一行可以相当长

### Body

*不是所有的请求都有一个 body*
例如获取资源的请求，GET，HEAD，DELETE 和 OPTIONS，通常它们不需要 body

有些请求将数据发送到服务器以便更新数据：常见的的情况是 POST 请求

## res

### 状态行

HTTP 响应的起始行被称作 状态行 (status line)，包含以下信息：

1. 协议版本，通常为 HTTP/1.1。
2. 状态码 (status code)，表明请求是成功或失败。常见的状态码是 200，404，或 302。
3. 状态文本 (status text)。一个简短的，纯粹的信息，通过状态码的文本描述，帮助人们理解该 HTTP 消息。

一个典型的状态行看起来像这样
```HTTP/1.1 404 Not Found```

### Header

### Body

不是所有的响应都有 body：具有状态码 (如 201 或 204) 的响应，通常不会有 body

Body 大致可分为三类：

1. Single-resource bodies，由已知长度的单个文件组成。该类型 body 由两个 header 定义：`Content-Type` 和 `Content-Length`。
2. Single-resource bodies，由未知长度的单个文件组成，通过将 `Transfer-Encoding` 设置为 `chunked` 来使用 chunks 编码。
4. Multiple-resource bodies，由多部分 body 组成，每部分包含不同的信息段。但这是比较少见的。


# e.设计(页面架构,针对不同显示用不同单位等)

# 浏览器(内核,同源策略原理,渲染...)

## 浏览器存储

特点 | cookie | localStorage | sessionStorage | indexDb
:-: | :-: | :-: | :-: | :-:
生命周期| 可过期 | 除非清理，否则一直存在| 页面关闭就清理 | 除非清理，否则一直存在 |
存储大小| 4K | 5M | 5M | ∞ |
与服务端通信| 请求携带在 header 头部 | no | no | no |

## 浏览器缓存机制


## 浏览器内核

浏览器最重要或者说核心的部分是“Rendering Engine”，可大概译为“渲染引擎”，不过我们一般习惯将之称为“浏览器内核”。

通常所谓的浏览器内核也就是浏览器所采用的渲染引擎

浏览器内核主要包括三个分支技术：`排版渲染引擎`、 `JavaScript引擎`，`以及其他`。

1. Trident IE 内核
  其中 IE8 的 JavaScript 引擎是 JScript 引擎, IE9 开始使用 Chakra
2. Gecko  FF 内核
  JavaScript 引擎使用 Spider Monkey 第一款 JavaScript 引擎
3. Webkit Safari 内核 Chrome 内核原型
  Android 默认浏览器使用 Webkit 内核
4. Blink Chrome 最新的内核(Safari 目前也使用的内核)
  而谷歌方面，则使用了自己研发的 V8 引擎


内核  是否开源  插件支持  应用浏览器 支持操作系统
Trident 否，但提供接口调用 ActiveX IE  Windows
Gecko 是，多种协议授权发行，包括MPL、GPL、LGPL NPAPI Firefox Windows,Mac,Linux/BSD
Blink 是 NPAPI Chrome，Opera  Windows,Mac,Linux/BSD
Webkit  是，遵从LGPL协议  NPAPI Chrome,Safar  Windows,Mac,Linux/BSD


可这样理解，浏览器内核虽然包括三个分支，但其主要就是完成页面渲染的排版引擎

因为网页浏览器的排版引擎（Layout Engine或Rendering Engine）也被称为浏览器内核、页面渲染引擎或模板引擎，它负责取得网页的内容（HTML、XML、图像等等）、整理消息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。所有网页浏览器、电子邮件客户端以及其它需要根据表示性的标记语言（Presentational markup）来显示内容的应用程序都需要排版引擎

所以说排版引擎就是用来渲染 HTML CSS 等页面的

而另一个很重要的就是 JavaScript 引擎


### 排版引擎

1. WebCore
  该引擎是在 KHTML 引擎基础上而来的
2. KHTML

#### 浏览器解析 HTML 

目前浏览器的排版引擎有三种模式：

1. 怪异模式（Quirks mode）
2. 接近标准模式（Almost standards mode）
3. 标准模式（Standards mode）

对HTML文件来说，浏览器使用文件开头的 DOCTYPE 来决定用怪异模式处理或标准模式处理


## 渲染原理

网页的生成过程，大致可以分成五步：

1. HTML代码转化成DOM
    - 当服务器返回一个HTML文件给浏览器的时候, 浏览器接受到的是一些字节数据
    - 根据请求头部信息的编码方式, 对字节流进行编码, 得到 HTML 字符串
    - 当我们浏览器获得HTML文件后，会自上而下的加载，并在加载过程中进行解析和渲染
    - 加载说的就是获取资源文件的过程，如果在加载过程中遇到外部 CSS 文件和图片，浏览器会另外发送一个请求，去获取 CSS 文件和相应的图片，`这个请求是异步的，并不会影响 HTML 文件的加载`
    - DOM 树的构建过程是一个深度遍历过程：当前节点的所有子节点都构建好后才会去构建当前节点的下一个兄弟节点
    - [HTML 解析器解析](https://blog.csdn.net/greenqingqingws/article/details/19163061)
2. CSS代码转化成CSSOM（CSS Object Model）
    - DOM 和 CSSOM 都是以 Bytes → characters → tokens → nodes → object model. 这样的方式生成最终的数据
    - `display:none` 的节点不会被加入 Render Tree，而 `visibility: hidden` 则会，所以，如果某个节点最开始是不显示的，设为 `display:none` 是更优的
3. 结合 DOM 和 CSSOM, 生成一棵渲染树（Render Tree 包含每个节点的视觉信息）
    - 根据 DOM 和 CSSOM 来构建 Render Tree(渲染树)
    - 注意渲染树，并不等于 DOM 树，因为一些像 `head` 或 `display:none` 的东西，就没有必要放在渲染树中了
    - 得到 Render Tree ,然后计算出每个节点在 layout 上的位置

4. 生成布局(layout), 也叫 flow
    - 即将所有渲染树的所有节点进行平面合成
    - 前三步都很快, 麻烦点的就是最后这两步
5. 将布局绘制(paint)在屏幕上
    - 按照算出来的规则,通过显卡,把 layout 画到屏幕上
    - 当最后一个节点被绘制, 事件 `DomContentloaded` 就会发生
    - 如果此时的, link css 或者是 img 或是 script 引用的外部资源未从服务器返回
    - 生成布局(flow)和绘制(paint)这两步，合称为"渲染"（render）

*以上的五步只是浏览器在第一时间渲染的情况*

JavaScript被认为是解析阻塞资源,所以可以加上 `async` 属性给 script 标签达到异步加载避免阻塞

在网页生成的时候, 至少会渲染一次

### 重新生成布局&重新绘制(reflow & repaint)

一旦网页在生成之后发生了以下三种情况就会重新渲染一次
1. 修改 DOM
2. 修改 CSSOM
3. 用户事件

重新渲染，就需要重新生成布局和重新绘制。前者叫做"重排"（reflow），后者叫做"重绘"（repaint）。

repaint 不一定需要 reflow，比如改变某个网页元素的颜色，就只会触发 repaint，不会触发 reflow ，因为布局没有改变
reflow 必然导致 repaint，比如改变一个网页元素的位置，就会同时触发 reflow 和 repaint，因为布局改变了


`display:none` 会触发 reflow, 而 `visibility:hidden` 只会触发 repaint, 因为没有发现位置变化

+ reflow
  - 元件的几何尺寸变了，我们需要重新验证并计算Render Tree。是Render Tree的一部分或全部发生了变化
  - *reflow 几乎是无法避免的*
  + 9 种主要触发 reflow 的动作
    1. 调整窗口大小（Resizing the window）
    2. 改变字体（Changing the font）
    3. 增加或者移除样式表（Adding or removing a stylesheet）
    4. 内容变化，比如用户在input框中输入文字（Content changes, such as a user typing text inan input box）
    5. 激活 CSS 伪类，比如 :hover (IE 中为兄弟结点伪类的激活)（Activation of CSS pseudo classes such as :hover (in IE the activation of the pseudo class of a sibling)）
    6. 操作 class 属性（Manipulating the class attribute）
    7. 脚本操作 DOM（A script manipulating the DOM）
    8. 计算 offsetWidth 和 offsetHeight 属性（Calculating offsetWidth and offsetHeight）  根据此可以实现一个jquery插件，让元素回流并重绘。ex. el.style.left=20px; a = el.offsetHeight;el.style.left=22px;
    9. 设置 style 属性的值 （Setting a property of the style attribute）
  + CSS 避免 reflow
    1. 通过改变元素的类名改变样式,并尽可能在子节点中改变
    2. 避免设置多项内联样式
    3. 使用 fixed || absolute 应用动画元素
    4. 权衡平滑和速度
    5. 避免使用 table 布局
    6. 避免使用 CSS 的 JavaScript 表达式 (仅 IE 浏览器)
    7. 精简 CSS,避免复杂 CSS 或 CSS 选择器, 并避免使用运算式
+ repaint 
    - 改变某个元素的背景色、文字颜色、边框颜色等等不影响它周围或内部布局的属性时，屏幕的一部分要重画，但是元素的几何尺寸没有变

### 加载首屏

·首屏时间和DomContentLoad事件没有必然的先后关系
·所有CSS尽早加载是减少首屏时间的最关键
·js的下载和执行会阻塞Dom树的构建（严谨地说是中断了Dom树的更新），所以script标签放在首屏范围内的HTML代码段里会截断首屏的内容。
·script标签放在body底部，做与不做async或者defer处理，都不会影响首屏时间，但影响DomContentLoad和load的时间，进而影响依赖他们的代码的执行的开始时间。

[一个关于加载首屏](https://segmentfault.com/a/1190000004292479)

### 性能提升

1. 尽量不要把读操作和写操作放在一个语句里面, 不要两个读(写)操作之间，加入一个写(读)操作
2. 样式表越简单,重排和重绘就越快
3. 重排和重绘的 DOM 元素层级越高,成本就越高
4. table 元素的重排和重绘成本,要高于 div 元素
5. 如果某个样式是通过重排得到的,那么最好缓存结果。避免下一次用到的时候,浏览器又要重排
6. 不要一条条地改变样式,而要通过改变class,或者csstext属性,一次性地改变样式
7. 尽量使用离线DOM,而不是真实的网面DOM,来改变元素样式
  - 操作 Document Fragment对象,完成后再把这个对象加入DOM
  - 使用 cloneNode() 方法,在克隆的节点上进行操作,然后再用克隆的节点替换原始节点
8. 先将元素设为 `display: none`（需要1次重排和重绘）,然后对这个节点进行100次操作,最后再恢复显示（需要1次重排和重绘）。这样一来,你就用两次重新渲染,取代了可能高达100次的重新渲染
9. position 属性为 `absolute` 或 `fixed` 的元素,reflow 的开销会比较小,因为不用考虑它对其他元素的影响
10. 只在必要的时候,才 `diaplay` 显示因为不可见的元素不影响重排和重绘, `visibility : hidden` 的元素只对 repaint 有影响,不影响 reflow
11. 使用虚拟 DOM 的脚本库,比如 React,Vue
12. 使用 window.requestAnimationFrame()、window.requestIdleCallback() 这两个方法调节重新渲染,*IE8-不支持*


### 刷新率

网页动画的每一帧（frame）都是一次重新渲染

每秒低于24帧的动画，人眼就能感受到停顿。一般的网页动画，需要达到每秒30帧到60帧的频率，才能比较流畅。如果能达到每秒70帧甚至80帧，就会极其流畅

大多数显示器的刷新频率是60Hz，为了与系统一致，以及节省电力，浏览器会自动按照这个频率，刷新动画（如果可以做到的话

所以，如果网页动画能够做到每秒60帧，就会跟显示器同步刷新，达到最佳的视觉效果。这意味着，一秒之内进行60次重新渲染，每次重新渲染的时间不能超过16.66毫秒

一秒之间能够完成多少次重新渲染，这个指标就被称为"刷新率"，英文为FPS（frame per second）

### Script 之 async/defer

1. 默认 script 加载情况
  - 浏览器会立即加载并执行指定的脚本，也就是说不等待后续载入的文档元素，读到就加载并执行
  + 加载
    - 同步加载
    - 会阻塞渲染引擎
  + 执行
    - 同步加载完成立即执行
    - 会阻塞渲染引擎
    - 执行完成继续渲染引擎
2. async 加载
  - 异步执行引入的 JavaScript
  + 加载
    - 异步无序加载
    - 不会阻塞渲染引擎
  + 执行
    - 一旦加载完成就进行执行
    - 会阻塞渲染引擎
    - 会阻塞 `load` 事件
    - 在 `DOMContentLoaded` 事件前后执行
  
2. defer 加载
  - 延迟执行引入的 JavaScript
  + 加载
    - 与渲染引擎并行顺序加载
    - 不会阻塞渲染引擎
    - 阻塞 `DOMContentLoaded` 事件
  + 执行
    - 不会阻塞渲染引擎
    - 当渲染引擎与 defer 加载都完成加载顺序执行



## ECMAScript 

ECMAScript是一种由 Ecma 国际通过 ECMA-262 标准化的脚本程序设计语言

它往往被称为JavaScript或JScript

所以它可以理解为是 JavaScript 的一个标准

但 JScript 和 JavaScript 实际上是 ECMA-262 标准的实现和扩展

1995年Netscape公司发布的Netscape Navigator 2.0中，发布了与Sun联合开发的JavaScript 1.0并且大获成功， 并且随后的3.0版本中发布了JavaScript1.1，恰巧这时微软进军浏览器市场，IE 3.0搭载了一个JavaScript的克隆版-JScript， 再加上Cenvi的ScriptEase（也是一种客户端脚本语言），导致了三种不同版本的客户端脚本语言同时存在。为了建立语言的标准化，1997年JavaScript 1.1作为草案提交给欧洲计算机制造商协会（ECMA），第三十九技术委员会（TC39）被委派来“标准化一个通用的，跨平台的，中立于厂商的脚本语言的语法和语意标准”。最后在Netscape、Sun、微软、Borland等公司的参与下制订了ECMA-262，该标准定义了叫做ECMAScript的全新脚本语言。

ECMAScript实际上是一种脚本在语法和语义上的标准。实际上 JavaScript 是由 `ECMAScript`，`DOM` 和 `BOM` 三者组成的

### JScript

JScript 是由微软公司开发的活动脚本语言，是微软对 ECMAScript 规范的实现

JScript8.0 是 基于 ECMAScript 的一个新版本, 功能更加强大 


### JavaScript 

JavaScript一种直译式脚本语言，是一种动态类型、弱类型、基于原型的语言，内置支持类型

解析 JavaScript 的解释器就被称为 JavaScript 引擎

因 JavaScript 与其它的 JScript, 或者 ActionScript 更兼容 ECMA 标准,所以 JavaScript 也叫做 `ECMAScript`

JavaScript 组成部分分三种
1. ECMAScript 描述语法与基本对象
2. DOM 描述处理网页内容的方法和接口
3. BOM 描述与浏览器进行交互的方法和接口

它最初由 Netscape 的 Brendan Eich 设计。*JavaScript是甲骨文公司的注册商标*

现在 JavaScript 已经被 Netscape 公司提交给 ECMA 制定为标准，称之为 `ECMAScript`，标准编号ECMA-262

符合ECMA-262 3rd Edition标准的实现有:


1. Microsoft公司的JScript.
2. Mozilla的JavaScript-C（C语言实现），现名SpiderMonkey
3. Mozilla的Rhino（Java实现）
4. Digital Mars公司的DMDScript
5. Google公司的V8
6. WebKit

#### 子集与超集

大多数语言都会定义它们的子集，用以更安全地执行不信任的第三方代码。

Douglas Crockford曾写过一本很簿的书《JavaScript: The Good Parts》，专门介绍JavaScript中值得发扬光大的精华部分。

这个语言子集的目标是规避语言中的怪癖、缺陷部分，最终编程更轻松、程序更健壮。

子集的设计目的是能在一个容器或"沙箱"中更安全地运行不可信的第三方JavaScript代码。所有能破坏这个沙箱并影响全局执行环境的语言特性和API在这个安全子集中都是禁止的。
为了让JavaScript代码静态地通过安全检查，必须移除一些JavaScript特性：

· 禁止使用this关键字，因为函数(在非严格模式中)可能通过this访问全局对象。
· 禁止使用with语句，因为with语句增加了静态代码检查的难度。
· 静态分析可以有效地防止带有点(.)运算符的属性存取表达式去读写特殊属性。但我们无法对方括号([])内的字符串表达式做静态分析。基于这个原因，安全子集禁止使用方括号，除非括号内是一个数字或字符串直接量。
· eval()和Function()构造函数在任何子集里都是禁止使用的，因为它们可以执行任意代码，而且JavaScript无法对这些代码做静态分析。
· 禁止使用全局变量，因此代码中不能有对Window对象的引用和对Document的引用。
· 禁止使用某些属性和方法，以免在水箱中的代码拥有过多的权限。如：arguments对象的两个属性caller和callee、函数的call()和apply()方法、以及constructor和prototype两个属性。


而相对于当前 JavaScript 的超集来说，主要就是 `TypeScript` 和 `CoffeeScript`
*TypeScrip 始于JavaScript，归于JavaScript*

浏览器的 JavaScript 引擎就是为了解释 JavaScript 而存在

可以这样理解 `JavaScript` `JSscript` `ActionScript` 分别可以是 ECMAScript 的子集

既然浏览器的 JavaScript 引擎是运行 JavaScript 的地方

那么 Babel 就是将浏览器未实现的 ECMAScript 规范语法转化为浏览器可运行的低版本代码

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

## WebSock


# 文档(DOM标准,语义化,元素,属性,结构,SEO)

在HTML中，文档类型声明是必要的。所有的文档的头部，你都将会看到`<!DOCTYPE html>` 的身影。这个声明的目的是防止浏览器在渲染文档时，切换到我们称为“怪异模式(兼容模式)”的渲染模式。`<!DOCTYPE html>` 确保浏览器按照最佳的相关规范进行渲染，而不是使用一个不符合规范的渲染模式


## DOM(Document Object Model)

DOM（Document Object Model——文档对象模型）是用来呈现以及与任意 HTML 或 XML文档交互的API。DOM 是载入到浏览器中的文档模型，以节点树的形式来表现文档，每个节点代表文档的构成部分（例如:页面元素、字符串或注释等等）。是 W3C 组织推荐的处理可扩展标志语言的标准编程接口

+ W3C DOM
  - 核心 DOM - 针对任何结构化文档的标准模型
  + XML DOM
    - 用于 XML 的标准对象模型
    - 用于 XML 的标准编程接口
    - 中立于平台和语言
    - W3C 标准
  + HTML DOM
    - HTML 的标准对象模型
    - HTML 的标准编程接口
    - W3C 标准

+ DOM 的四个基本接口
  1. Document
  2. Node
  3. NodeList
  4. NamedNodeMap
 
 DOM 事件驱动就是做了什么操作就执行什么事件
 
## 语义
 
 
 在程序中, 语义 指的是一段代码的含义
 
### 为什么要语义化?
1. 为了在没有CSS的情况下，页面也能呈现出很好地内容结构、代码结构:为了裸奔时好看；
2. 用户体验：例如title、alt用于解释名词或解释图片信息的标签尽量填写有含义的词语、label标签的活用；
3. 有利于SEO：和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息：爬虫依赖于标签来确定上下文和各个关键字的权重；
4. 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以有意义的方式来渲染网页；
5. 便于团队开发和维护，语义化更具可读性，遵循W3C标准的团队都遵循这个标准，可以减少差异化。

### HTML 文档需注意
1. 尽可能少的使用无语义的标签div和span；
2. 在语义不明显时，既可以使用 div 或者 p 时，尽量用 p, 因为 p 在默认情况下有上下间距，对兼容特殊终端有利；
3. 不要使用纯样式标签，如：b、font、u等，改用css设置。
4. 需要强调的文本，可以包含在strong或em标签中，strong默认样式是加粗（不要用b），em是斜体（不要用i）；
5. 使用表格时，标题要用caption，表头用thead，主体部分用tbody包围，尾部用tfoot包围。表头和一般单元格要区分开，表头用th，单元格用td；而且为了浏览器渲染引擎尽量少用 table 标签
6. 表单域要用 fieldset 标签包起来，并用 legend 标签说明表单的用途；
7. 每个 input 标签对应的说明文本都需要使用 label 标签，并且通过为 input 设置 id 属性，在 lable 标签中设置 `for=someld` 来让说明文本和相对应的 input 关联起来。

比如
```HTML
<h1>This is a top level heading</h1>
```
一般情况下，它将会被赋予一个很大的字号尺寸从而使它看上去更像是一个标题 (虽然你可以把它格式化为任何你想要的样式), 但是更重要的是它的语义会被在很多地方以不同的方式被使用到， 例如搜索引擎会把它包含的内容作为一个重要的关键词，从而影响这个页面在搜索结果中的排序 (参见SEO),而且屏幕阅读器会使用它来帮助视障用户更好的使用这个页面.
 
虽然可以使用 CSS 将普通 span 标签也渲染成 H1 标签样式,但是它的值没有对应到最“最高级别标题”这一语义，所以在此之上，它不会获得更多额外的描述（只是一个普通“span”元素而不是“最高级别标题”这一语义）。所以在恰当的需求下使用恰当的HTML元素是一个不错的主意
 
### 语义化的元素

- article: 表示文档、页面、应用或网站中的独立结构，其意在成为可独立分配的或可复用的结构，如在发布中，它可能是论坛帖子、杂志或新闻文章、博客、用户提交的评论、交互式组件，或者其他独立的内容项目。
- aside:元素表示一个和其余页面内容几乎无关的部分，被认为是独立于该内容的一部分并且可以被单独的拆分出来而不会使整体受影响。其通常表现为侧边栏或者嵌入内容。他们通常包含在工具条，例如来自词汇表的定义。也可能有其他类型的信息，例如相关的广告、笔者的传记、web 应用程序、个人资料信息，或在博客上的相关链接。
- details:详细信息展现元素
- figcaption: 用于描述其父节点  &lt;figure&gt; 元素里的其他数据。这意味着  &lt;figcaption&gt; 在 &lt;figure&gt; 块里是第一个或最后一个。同时 HTML Figcaption 元素是可选的；如果没有该元素，这个父节点的图片只是会没有说明/标题。
- figure:HTML  &lt;figure&gt; 元素代表一段独立的内容, 经常与说明(caption)  &lt;figcaption&gt; 配合使用, 并且作为一个独立的引用单元。当它属于主体(main flow)时，它的位置独立于主体。这个标签经常是在主文中引用的图片，插图，表格，代码段等等，当这部分转移到附录中或者其他页面时不会影响到主体。
- footer:表示最近一个章节内容或者根节点（sectioning root ）元素的页脚。一个页脚通常包含该章节作者、版权数据或者与文档相关的链接等信息。
- header:用于展示介绍性内容，通常包含一组介绍性的或是辅助导航的实用元素。它可能包含一些标题元素，但也可能包含其他元素，比如 Logo、搜索框、作者名称，等等。
- main:呈现了文档 &lt;body&gt;或应用的主体部分。主体部分由与文档直接相关，或者扩展于文档的中心主题、应用的主要功能部分的内容组成。这部分内容在文档中应当是独一无二的，不包含任何在一系列文档中重复的内容，比如侧边栏，导航栏链接，版权信息，网站logo，搜索框（除非搜索框作为文档的主要功能）。
- mark:代表突出显示的文字,例如可以为了标记特定上下文中的文本而使用这个标签. 举个例子，它可以用来显示搜索引擎搜索后关键词。
- nav:描绘一个含有多个超链接的区域，这个区域包含转到其他页面，或者页面内部其他部分的链接列表.
- section:表示文档中的一个区域（或节），比如，内容中的一个专题组，一般来说会有包含一个标题（heading）。一般通过是否包含一个标题 ( &lt;h1&gt;- &lt;h6&gt; element) 作为子节点 来 辨识每一个 &lt;section&gt;
- summary:用作 一个 &lt;details&gt;元素的一个内容的摘要，标题或图例。
- time:用来表示24小时制时间或者公历日期，若表示日期则也可包含时间和时区。


### SEO

SEO (搜索引擎优化) 是一种让网站在搜索引擎结果中更加清晰, 也帮助我们将搜索结果更靠前

搜索引擎抓取网页，跟随页面之间的链接，并索引找到的内容。 搜索时，搜索引擎会显示索引内容。 爬行者遵守规则。 如果您在为网站进行搜索引擎优化时密切关注这些规则，则会为网站提供最好的机会，以便在首批结果中显示，增加流量和可能的收入（用于电子商务和广告）。

搜索引擎为SEO提供了一些指导，但大型搜索引擎将结果排名保持为商业秘密。 SEO结合了官方搜索引擎指南，经验知识和科学论文或专利的理论知识



# 文档规范(XML,HTML,HTML5)

+ 使用正确的文档类型
	- 文档类型声明位于HTML文档的第一行 `<!doctype html>`
- 元素名使用小写
+ 所有元素应该闭合
	- 如果是单标签,可闭合也可以不闭合,但文档要求统一
	- XML 和 XHTML 中闭合是必须的
- 属性名使用小写
- HTML5 属性值可以不用引号，但是如果属性值有空格需要引号，所以属性使用引号
+ img 使用 alt 属性在图片不能显示时替代图片显示
	- 并且给 img 预留一个指定的高宽可以在加载时减少图片闪烁
- 等号前后不使用空格
- 避免一行代码过长，超过 80 字符换行
- 使用 2 个空格缩进，不留有无故空行，长段落开关缩进两格
- 不省略 html,head,body 标签
+ 元数据
	- 不能省略 `title` 标签
+ 样式表
	- link 元素使用简洁语法
	- 短规则一行书写
	- 将左花括号与选择器放在同一行
	- 左花括号与选择器间添加一个空格
	- 使用两个空格来缩进
	- 冒号与属性值之间添加一个空格
	- 逗号和符号之后使用一个空格
	- 每个属性与值结尾都要使用分号
	- 只有属性值包含空格时才使用引号
	- 右花括号放在新的一行
	- 每行最多 80 个字符
- 使用简洁语法载入文件，link,script 标签
- 使用小写文件名
+ 服务器默认文件
	- 通常默认文件名为 index.html, index.htm, default.html, 和 default.htm
	- 如果只配置一个文件时使用 `index.html`
	- HTML 完整后缀 `.html`


## Doctype

声明在文档的第一行,位于 `<html>` 标签之前,指示浏览器用那个 HTML 版本进行解析

+ 标准模式
	- 文档的 `<!doctype XXX>` 存在并且形式正确
	- 盒模型实际宽度为 width + padding-left + padding-right + border-left-width + border-right-width
+ 混杂模式
	- 文档的 `<!doctype XXX>` 不存在或者形式不正确
	- 盒模型实际宽度为 css width, 如果没有没有 overflow 的情况下,若盒子内容、内边距、或是边框的值较大，会把盒子撑开，实际宽度和高度则大于设定值

### 影响 css 的情况

主要是IE浏览器，其他Chrome，FF以及IE高版本浏览器无论在什么模式下都能正常显示

1. 盒模型是混杂模式和标准模式的主要区别
<=IE6将盒子的padding和border算到盒子尺寸中，这被称为IE盒模型。
W3C标准的盒模型中，box大小就是content大小。
这一区别将导致页面绘制时所有块级元素都出现很大差别，所以两种不同的文档模式下的页面也区别很大。
2. 影响图片元素的垂直对齐方式，就是在行框对基线的选择，IE的怪异模式会以Bottom-line为基线，标准模式下以base-line为基线。
3. 影响table元素继承字体的某些属性，在IE5的怪异模式下不会继元素的一部分属性，尤其是font-size属性。
4. IE5怪异模式中内联元素可以定义尺寸
5. IE标准模式下，overflow取值为visible即溢出可见，在怪异模式下该溢出会被当作扩展box来对待，元素的大小由其内容决定，溢出不会被裁剪，而是父元素会自动调整自己的宽高以完全适应包含内容。

### 影响 JavaScript 情况

跨浏览器确定一个窗口大小不是一件简单事，注意以下介绍的属性获取后的值都是整数而且没单位，即使是小数浏览器计算时也会四舍五入。
IE9+，FF，Safari，Opera，Chrome均为此提供四个属性：innerWidth，innerHeight，outerWidth，outerHeight。

1. FF 标准模式表现下正常，混杂模式下 `document.body` 计算值正确，但document.documentElement计算有误
2. 但在IE6中这些属性必须在标准模式下才有效
3. 标准模式下 Chrome, 要优先选择 `document.documentElement` 计算视口,混杂模式下的Chrome，无论通过document.documentElement还是document.body中的clientWidth和clientHeight属性都可取得视口大小



# 元数据(head,title,meta,a,link...)

## HTML 全局属性

1. id: id元素给元素分配一个唯一标识符，一个页面只能出现一次同一个id名称
2. class: class属性用来给元素归类，通过对文档中某一个或多个元素同时设置CSS 样式
3. accessKey: 快捷键操作，`<input  type = "text"   name = "user"   accesskey = "n">`
4. contenteditable:  让文本处于可编辑状态，设置true则可以编辑`<p  contenteditable = "true">我是一段可修改的文字</p>`
5. dir: 设置文本对齐方式，从左到右（ltr），从右到左（rtl）
6. hidden: 隐藏元素
7. lang: 设置局部语言
8. title: 对元素的内容进行额外的提示
9. tabindex: 表单中按下tab键，实现获取焦点的顺序，如果是-1，则不会被选中
10. style:  设置元素行内CSS样式

## meta

元数据就是描述其它数据的数据，比如说该网页的对应搜索引擎关键词或者是文件的最后修改时间作者等信息，在 HTML 文档中用来做这件事的就是 `meta` 元素

meta 元素通常在 head 元素中, 以 k/v 键值对的形式出现，如果没有提供 `name` 属性，那么 k/v 对中的名称会采用 `http-equiv` 属性的值

+ charset: 定义文档字符编码 `H5`
+ content: 定义与 http-equiv 或 name 属性相关 value
+ name: 作为一个元数据的 key
	- author: 就是这个文档的作者名称，可以用自由的格式去定义；
	- generator: 包含生成页面的软件的标识符。
	- keywords: 包含与逗号分隔的页面内容相关的单词。
	- referrer: 控制所有从该文档发出的 HTTP 请求中HTTP Referer 首部的内容
	- application-name: 定义正运行在该网页上的网络应用名称
	- author: 描述作者
	- description: 其中包含页面内容的简短和精确的描述。 一些浏览器，如Firefox和Opera，将其用作书签页面的默认描述
	- keywords: 文档的搜索引擎对应关键词
	+ viewport: 它提供有关视口初始大小的提示，仅供移动设备使用
		- width 定义viewport（视口）的宽度,以pixels（像素）为单位
		- height 定义viewport（视口）的高度。以pixels（像素）为单位
		- initial-scale	定义设备宽度（纵向模式下的设备宽度或横向模式下的设备高度）与视口大小之间的缩放比率。`0.0-10.0`
		- maximum-scale	定义缩放的最大值,它必须大于或等于 `minimum-scale` 的值, 不然会导致不确定的行为发生。`0.0-10.0`
		- minimum-scale	定义缩放的最小值,它必须小于或等于 `maximum-scale` 的值, 不然会导致不确定的行为发生。
		- user-scalable	定义网页是否可以被用户缩放。`yes|no`,默认为 yes
+ scheme: 定义用于翻译 content 属性值的格式，`H5` 不支持

```HTML
<!-- 针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓 -->
<meta name="HandheldFriendly" content="true">
<!-- 微软的老式浏览器 -->
<meta name="MobileOptimized" content="320">
<!-- uc强制竖屏 -->
<meta name="screen-orientation" content="portrait">
<!-- QQ强制竖屏 -->
<meta name="x5-orientation" content="portrait">
<!-- UC强制全屏 -->
<meta name="full-screen" content="yes">
<!-- QQ强制全屏 -->
<meta name="x5-fullscreen" content="true">
<!-- UC应用模式 -->
<meta name="browsermode" content="application">
<!-- QQ应用模式 -->
<meta name="x5-page-mode" content="app">
<!-- windows phone 点击无高光 -->
```



## 分辨率

分辨率，又称解析度、解像度，可以从显示分辨率与图像分辨率两个方向来分类

- 显示分辨率（屏幕分辨率）是屏幕图像的精密度，是指显示器所能显示的像素有多少
- 图像分辨率则是单位英寸中所包含的像素点数，其定义更趋近于分辨率本身的定义

分辨率决定了位图图像细节的精细程度

通常情况下，图像的分辨率越高，所包含的像素就越多，图像就越清晰，印刷的质量也就越好。同时，它也会增加文件占用的存储空间

描述分辨率的单位有
- dpi（点每英寸）
- lpi（线每英寸）
- ppi（像素每英寸）
- PPD（PPPixels Per Degree 角分辨率，像素每度）是头戴影院、VR眼镜、VR一体机类产品的参数，可以衡量用户使用该类型产品时的对显示画面的清晰感受

但只有 `lpi`是描述光学分辨率的尺度的。  

虽然dpi和ppi也属于分辨率范畴内的单位，但是他们的含义与lpi不同。

*而且lpi与dpi无法换算，只能凭经验估算。ppi 和 lpi 可以换算, `1 lpi = 1 ppi一半`*

从技术角度说，“像素”只存在于电脑显示领域，而“点”只出现于打印或印刷领域

在电脑显示领域通常所说的显示器分辨率其实是指`桌面设定`的分辨率，而不是显示器的`物理分辨率`



## 像素

### DP(物理像素,设备像素)

设备像素（物理像素），显示屏是由一个个物理像素点组成的，通过控制每个像素点的颜色，使屏幕显示出不同的图像，屏幕从工厂出来那天起，它上面的物理像素点就固定不变了，所以物理像素也是我们学说的分辨率

### DIP (设备独立像素，逻辑像素)

可以认为是计算机坐标系统中得一个点，这个点代表一个可以由程序使用的虚拟像素(比如: css像素)，这个点是没有固定下小的，越小越清晰，然后由相关系统转换为物理像素

pt 在 css 单位中属于真正的绝对单位, `1pt = 1/72(inch)`, inch及英寸，而1英寸等于2.54厘米。

*屏幕普遍采用RGB色域(红、绿、蓝三个子像素构成),而印刷行业普遍使用CMYK色域(青、品红、黄和黑)*

### DPR(设备像素比)

未缩放状态下，物理像素和CSS像素的初始比例关系，计算方法如下图

设备像素比率计算公式 `设备像素比 = 设备像素 / 设备独立像素`

(在Retina屏的iphone上，DPR为2，1个css像素相当于2个物理像素

`window.devicePixelRatio 返回当前设备像素比`

在 CSS 中表示 1 个 css 像素占用多少设备像素，如 2 代表 1 个 css 像素用 2x2 个设备像素来绘制

把 DPR 为 1 的 元数据 `initial-scale=1.0` 这样设置，把 DPR 为 2 的 `initial-scale=0.5` 这样设置


### CSS 像素(虚拟像素)
pixels 缩写, 是图像显示的基本单元，既不是一个确定的物理量，也不是一个点或者小方块，而是一个`抽象概念`

虚拟像素，可以理解为“直觉”像素，CSS和JS使用的`抽象单位`，浏览器内的一切长度都是以 CSS 像素为单位的，CSS像素的单位是px

CSS 规范描述，长度单位可以分为绝对与相对单位,就是一个相对单位,相对

CSS 规范中，长度单位可以分为两类，绝对(absolute)单位以及相对(relative)单位。

不同的设备，图像基本采样单元是不同的，显示器上的物理像素等于显示器的点距，而打印机的物理像素等于打印机的墨点。而衡量点距大小和打印机墨点大小的单位分别称为ppi和dpi：

- dpi（dots per inch）：像素密度，表示水平或垂直方向每英寸长度的像素数目，放到显示器上说的是每英寸多少物理像素及显示器设备的点距
- ppi（pixels per inch）：像素密度，表示沿对角线每英寸长度的像素数目

*px 有两种说法，msdn 上说是绝对单位,网上说是相对单位*

正因为每个设备的基本采样单元是不同的，像素是不同的，

CSS 则会针对不同设备中显示出差异，是因为浏览器会用使用 CSS 规范的`参考像素`来进行计算，目的是为了保证阅读体验一致

#### 参考像素

参考像素在 [w3c](https://www.w3.org/TR/CSS2/syndata.html#length-units) 上的描述是一个约等于 0.0213 度的视角

1个参考像素即为从一臂之遥看解析度为 `96DPI` 的设备输出（1英寸96点）时，1点（1/96英寸）的视角。它并不是1/96英寸长度，而是从一臂之遥的距离处看解析度为96DPI的设备输出一单位（即1/96英寸）时视线与水平线的夹角。通常认为常人臂长为28英寸，所以它的视角是:

	(1/96)in / (28in * 2 * PI / 360deg) = 0.0213度。

*由于 css 像素是一个视角单位，所以在真正实现时，为了方便基本都是根据设备像素换算的。浏览器根据硬件设备能够直接获取css像素*


### PPI

每英寸像素取值，更确切的说法应该是像素密度，也就是衡量单位物理面积内拥有像素值的情况

	`PPI = dp/di = V (wp*wp + hp*hp) / di`
	
- dp: 屏幕对角线分辨率
- wp: 屏幕横向分辨率
- hp: 屏幕纵向分辨率
- di: 屏幕对角线长度(单位 in)

## viewport 

viewport 就是用来显示网页的那一块区域

在桌面浏览器中css的1个像素往往都是对应着电脑屏幕的1个物理像素，这可能会造成我们的一个错觉，那就是css中的像素就是设备的物理像素

但实际情况却并非如此，css中的像素只是一个抽象的单位，在不同的设备或不同的环境中，css中的1px所代表的设备物理像素是不同的

在iphone3上，一个css像素确实是等于一个屏幕物理像素

后来技术的发展，移动设备的屏幕像素密度越来越高，从 iphon4 开始，`Retina` 屏,分辨率提高了一倍，变成了 `640x960`, 但是屏幕的尺寸并没有变化,这就是同样大小的屏幕上像素就多了一倍,而在这个时候,一个CSS 像素就等于两个物理像素

改变 1CSS 像素的原因

1. 技术进步,`Retina` 屏幕出现,导致同样大小屏幕却有更加高的像素密度, 1个CSS 像素就会是两个物理像素
2. 用户的缩放,用户的缩放操作会将 1css 像素变大一倍,而缩小也是

浏览器上的三种 viewport

1. layout viewport
	- 浏览器中可以显示出网页全部内容的区域, 可用 `document.documentElement.clientWidth` 来获取
	- 移动设备默认为 layout viewport
	*Android 2, Oprea mini 和 UC 8中无法正确获取*
2. visual viewport 
	- 表示浏览器的可视区大小
	- `window.innerWidth` 获取
3. ideal viewport
	- 移动设备理想 viewport
	- 这个就是将显示在不同设备展示的一致方式
	- 是最适合移动设备的viewport，ideal viewport的宽度等于移动设备的屏幕宽度
	- 只要在css中把某一元素的宽度设为ideal viewport的宽度(单位用px)，那么这个元素的宽度就是设备屏幕的宽度了，也就是宽度为100%的效果
	*针对不同的设备,[这上面](http://viewportsizes.com/)给出了相差参数可以查看*



## meta 对 viewport 控制

移动端上的 viewport 默认是 layout viewport, 解决方案

```HTML
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
```
将 viewport 的宽度等于设备宽度,同时不允许用户缩放

width=device-width 直接就可以将 viewport 设置为 ideal viewport
initial-scale 相对于 ideal viewport 缩放的, 当 ideal viewport 缩放为 100% 也就是为 1 时,此时也可以将 viewport 设置为 ideal viewport 
*两个都写上是为了兼容性更好*

meta viewport 标签首先是由苹果公司在其safari浏览器中引入的，目的就是解决移动设备的viewport问题。后来安卓以及各大浏览器厂商也都纷纷效仿，引入对meta viewport的支持，事实也证明这个东西还是非常有用的

viewport content 内容属性值

- width: layout viewport 的宽度,正整数||'width-device'
- initial-scale: 初始缩放值,浮点数
- minimum-scale: 最小缩放,浮点数
- maximum-scale: 最大缩放,浮点数
- height layout: viewport 高度
- user-scalable:是否允许缩放, no || yes(默认)
- target-densitydpi(已废弃):安卓私有属性,决定 1css像素占用多少物理像素, 数值 || high-dpi 、 medium-dpi、 low-dpi、 device-dpi(1:1)
*属性可多个,之间有逗号隔开*

initial-scale 默认值肯定不为1,给出两个公式方便计算

```
visual viewport宽度 = ideal viewport宽度  / 当前缩放值
当前缩放值 = ideal viewport宽度  / visual viewport宽度
```
*在iphone和ipad上，无论你给viewport设的宽的是多少，如果没有指定默认的缩放值，则iphone和ipad会自动计算这个缩放值，以达到当前页面不会出现横向滚动条(或者说viewport的宽度就是屏幕的宽度)的目的*

也可以利用 css 的 `@viewport` 控制 viewport 

### 小结

- 如果页面不设置 viewport, 那么移动设备上浏览器默认的宽度值为800px，980px，1024px等这些(单位为 css像素)
- 每个移动设备浏览器中都有一个理想的宽度，这个理想的宽度是指css中的宽度，跟设备的物理宽度没有关系，在css中，这个宽度就相当于100%的所代表的那个宽度。我们可以用 meta 标签把 viewport 的宽度设为那个理想的宽度，如果不知道这个设备的理想宽度是多少，那么用 `device-width`这个特殊值就行了，同时`initial-scale=1`也有把 viewport 的宽度设为理想宽度的作用
- 屏幕尺寸：指的是屏幕对角线的长度
- 分辨率：是指宽度上和高度上最多能显示的物理像素点个数
- 点距：像素与像素之间的距离，点距和屏幕尺寸决定了分辨率大小
- PPI:屏幕像素密度，即每英寸(1英寸=2.54厘米)聚集的像素点个数，这里的一英寸还是对角线长度
- DPI:每英寸像素点，印刷行业术语。对于电脑屏幕而言和PPI是一个意思
- 设备像素(又称为物理像素): 指设备能控制显示的最小物理单位，意指显示器上一个个的点。从屏幕在工厂生产出的那天起，它上面设备像素点就固定不变了，和屏幕尺寸大小有关，单位 pt。
- 设备独立像素(也叫密度无关像素或逻辑像素)：可以认为是计算机坐标系统中得一个点，这个点代表一个可以由程序使用的虚拟像素(比如: css像素)，这个点是没有固定下小的，越小越清晰，然后由相关系统转换为物理像素。
- css像素(也叫虚拟像素)：指的是 CSS 样式代码中使用的逻辑像素，在 CSS 规范中，长度单位可以分为两类，绝对(absolute)单位以及相对(relative)单位。px 是一个相对单位，相对的是设备像素(device pixel)
- DPR(设备像素比)：设备像素比 = 设备像素 / 设备独立像素

# 样式(CSS规范,CSS2.0,盒子模型,书写规范,CSS3,兼容性,布局实现,页面实现,重用性高)


## 层叠和继承

CSS 样式来源：

- 创作人员:link, style, style:(style 元素)
- 读者:页面中会提供一些让用户自定义字体大小颜色等的快捷键
- 代理:浏览器

+ 层叠
	1. 重要性(Importance)
		- `!important` 语法可以优先于其他规则,该语法会改变样式层叠的正常工作
		- 重载 `!important` 的唯一方法是在`当前后面规则` 或者是有`更高专用性`的规则中添加 `!important`
		+ 语法冲突时适用顺序
			1. 代理样式声明
			2. 读者普通声明
			3. 创作普通声明(link, style, style:)
			4. 创作重要声明
			5. 用户样式重要声明
	2. 专用性(Specificity)
		- 专用性基本上是衡量选择器的具体程度的一种方法,也就是`选择器优先级`
		+ 专用性衡量
			1. style: -> 1000
			2. ID->100
			3. class || attr(属性选择器) || psdudo class(伪类)->10
			4. element->1
			5. `*` `+` `>` `!` `''` `:not`->0
		+ 计算规则
			- !important>style:>id>class>element>通配符>inherit>default
			- 选择器中出现一个加 1 分
			- 多个选择器具有相同的重要性和专用性,则按照书写顺序
	3. 书写顺序(Source order)
		- 如果出现重要性与专用性一致,则就看最后一个因素,书写在后面的更优先
	
	- 这些层叠只在作用于元素的属性之上,该条规则并没有参与

+ 继承
	- inherit:显示指定属性继承至父元素
	- initial:显示指定属性与 defalut 一致,如果 default 认样式表中没有设置值,并且该属性是自然继承的,那么该属性值就被设置为 inherit *IE 不支持*
	- unset: 重置为其自然值,即如果属性是自然继承的，那么它就表现得像 inherit，否则就是表现得像 initial 
	- revert: 设置成自定义样式所定义的属性(如果被设置),否则属性值被设置成 defalut,*IE 不支持*
	+ all 属性可以控制每一个元素的继承性,属性值为以上 4 个
		-  `unicode-bidi` 与 `direction` 除外

## 盒模型

+ 盒属性
	- margin: 外边塌陷(margin collapsing),也叫外边距折叠,当两个盒子相接触,边距取最大值
	- border
	- padding
	- width && height: 内容框宽度与高度,以及子元素的框大小(全部大小)
	- *width + padding-right + paddig-left + border-right + border-left 之和是盒模型实际宽度*

+ 盒要点
	- `background-color` && `background-image` 默认到 border 外边缘,该行为可以用 `background-clip` 改变
	+ overflow 如果实际内容比 content box 大,将会溢出出现滚动条
		- auto
		- hidden
		- visible 默认
	- height 是不遵守百分比的高度,高度始终取决于内容高度,除非父元素是绝对高度
	- border 也不遵守百分比
	- box-sizing 可以用来调整盒模型
	- outline 不包括在内

+ 三种盒类型
	- block box
	- inline box
	- inline-block box

+ 两种盒子模型
	- content-box (默认值,标准), width 不包括 padding 和 border	
	- border-box width 包括 padding 和 border

### 兼容性

一旦为页面设置了恰当的 DTD，大多数浏览器都会按照上面的图示来呈现内容。然而 IE 5 和 6 的呈现却是不正确的。

W3C 的规范盒子模型是为 content-box 但 IE5.X 和 6 在`怪异模式`中使用自己的非标准模型可以理解为 border-box

虽然有方法解决这个问题。但是目前最好的解决方案是回避这个问题。

*不要给元素添加具有指定宽度的内边距，而是尝试将内边距或外边距添加到元素的父元素和子元素。*

解决IE8及更早版本不兼容问题可以在HTML页面声明 `<!DOCTYPE html>` 即可




# 元素(block,inline,inline-block,table-cell,flex...)


# 布局(居中,自适应,响应式,流式,浮动,定位...)



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

https://segmentfault.com/q/1010000000640869
https://github.com/ljianshu/Blog/issues/51
https://www.runoob.com/html/html5-syntax.html
https://www.imooc.com/article/75237
https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta
https://www.runoob.com/w3cnote/viewport-deep-understanding.html
https://blog.csdn.net/lianfengzhidie/article/details/86663715
https://www.cnblogs.com/zaoa/p/8630393.html
about px:
https://www.cnblogs.com/yesyes/p/7638607.html
https://www.imooc.com/article/75237
https://www.cnblogs.com/zaoa/p/8630393.html
https://blog.csdn.net/lianfengzhidie/article/details/86663715
https://www.cnblogs.com/2050/p/3877280.html
https://www.cnblogs.com/zdz8207/p/vue-pt-px-750.html


