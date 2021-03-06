
# 计算机(内存,CPU,X64&X86)

## 进程与线程

1. 进程
  操作系统中的`进程`是常驻程序和应用程序运行的标准单位,是唯一`已运行`实体,也就是进程表示其`正在运行`。

  *面向线程设计系统中`线程容器`才是基本标准单位*

2. 线程
  `一个进程`中可以包含`若干个线程`,它们可以利用进程所拥有的资源,是操作系统能够进行运算调度的最小单位,也是进程中实际动作单位。由于线程比进程更小，基本上不拥有系统资源，故对它的调度所付出的开销就会小得多，能更高效的提高系统内多个程序间并发执行的程度。

以上两点简单的解释出进程与线程之间关系,由于进程与线程关系,`单任务`或`多任务`就会出现在程序的设计上,意思就是一个程序可以在同一时间执行一个或多个任务,对应着进程与线程关系那就是:一个进程可以有一个线程或多个线程,那么进程与线程之前则可有这三大关系

1. `单线程单进程(单任务进程)`一个进程和一个线程组成的程序叫单线程
  + 优点
    - 系统稳定
    - 扩展性极强
    - 软件丰富
  + 缺点
    - 由于需要在上一个任务完成后才能开始新的任务，所以其效率通常比多线程应用程序低

2. `多线程单进程(多任务进程)`,在单个程序中同时运行多个线程完成不同的工作,称为多线程
  + 优点
    - 使用线程可以把占据时间长的程序中的任务放到后台去处理
    - 用户界面可以更加吸引人，这样比如用户点击了一个按钮去触发某些事件的处理，可以弹出一个进度条来显示处理的进度
    - 程序的运行速度可能加快
    - 在一些等待的任务实现上如用户输入、文件读写和网络收发数据等，线程就比较有用了。在这种情况下可以释放一些珍贵的资源如内存占用等等。
    - 多线程技术在IOS软件开发中也有举足轻重的位置。
    - 线程应用的好处还有很多，就不一一说明了
  + 缺点
    - 如果有大量的线程,会影响性能,因为操作系统需要在它们之间切换。
    - 更多的线程需要更多的内存空间。
    - 线程可能会给程序带来更多“bug”，因此要小心使用。
    - 线程的中止需要考虑其对程序运行的影响。
    - 通常块模型数据是在多个线程间共享的，需要防止线程死锁情况的发生。

3. `多线程多进程(多任务多进程)`

*JavaScript 是一种典型的单任务进程*

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

## 请求起始行

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

### 请求头

来自请求的 (HTTP headers)[https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers] 遵循和 HTTP header 相同的基本结构：不区分大小写的字符串，紧跟着的冒号 (':') 和一个结构取决于 header 的值。 整个 header（包括值）由一行组成，这一行可以相当长

### 请求内容

*不是所有的请求都有一个 body*
例如获取资源的请求，GET，HEAD，DELETE 和 OPTIONS，通常它们不需要 body

有些请求将数据发送到服务器以便更新数据：常见的的情况是 POST 请求

### 响应状态行

HTTP 响应的起始行被称作 状态行 (status line)，包含以下信息：

1. 协议版本，通常为 HTTP/1.1。
2. 状态码 (status code)，表明请求是成功或失败。常见的状态码是 200，404，或 302。
3. 状态文本 (status text)。一个简短的，纯粹的信息，通过状态码的文本描述，帮助人们理解该 HTTP 消息。

一个典型的状态行看起来像这样
```HTTP/1.1 404 Not Found```

### 响应主体

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


内核 |是否开源 | 插件支持 | 应用浏览器 |支持操作系统
:-: | :-: | :-: | :-: | :-:
Trident| 否，但提供接口调用 |ActiveX | IE  |Windows
Gecko |是，多种协议授权发行 | MPL、GPL、LGPL NPAPI |Firefox| Windows,Mac,Linux/BSD|
Blink |是 | NPAPI |Chrome，Opera | Windows,Mac,Linux/BSD
Webkit | 是，遵从LGPL协议 | NPAPI | Chrome,Safar | Windows,Mac,Linux/BSD


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

## 同源策略

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

- 前文提到的由 XMLHttpRequest 或 Fetch 发起的跨域 HTTP 请求。
- Web 字体 (CSS 中通过 @font-face 使用跨域字体资源), 因此，网站就可以发布 TrueType 字体资源，并只允许已授权网站进行跨站调用。
- WebGL 贴图
- 使用 drawImage 将 Images/video 画面绘制到 canvas
- 样式表（使用 CSSOM）

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

当 HTTP 请求不再是一次简单请求时(对服务器有副作用),也就是不满足简单请求的条件时,浏览器必须先完成预检请求
完全满足以下五个条件
1. PUT | DELETE | CONNECT |OPTIONS | TRACE | PATCH
2. *人为设置* `CORS 安全的首部字段集合` 之外的首部字段
  Accept,Accept-Language,Content-Language,Content-Type (需要注意额外的限制),DPR,Downlink,Save-Data,Viewport-Width,Width
3. Content-Type *不属于* application/x-www-form-urlencoded | multipart/form-data | text/plain 其中之一
4. `XMLHttpRequestUpload` 注册任意多个监听事件
5. 使用了 `ReadableStream` 对象

预检请求与响应
```
# 请求
OPTIONS /resources/post-here/ HTTP/1.1 # 以 options 方式发送第一次请求
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.5; en-US; rv:1.9.1b3pre) Gecko/20081130 Minefield/3.1b3pre
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
Connection: keep-alive
Origin: http://foo.example
Access-Control-Request-Method: POST # 告知服务器实际请求时用 POST 方法
Access-Control-Request-Headers: X-PINGOTHER, Content-Type # 告知服务器实际请求将会携带两个自定义请求头部字段

# 响应


HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2.0.61 (Unix)
Access-Control-Allow-Origin: http://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS # 预检响应,服务器允许POST GET OPTIONS 
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type # 允许
Access-Control-Max-Age: 86400 # 该预检请求有效期 86400s
Vary: Accept-Encoding, Origin
Content-Encoding: gzip
Content-Length: 0
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Content-Type: text/plain
```

不再是简单请求时,浏览器这时会在实际请求前对服务器发送一次 OPTION 方式的请求来验证服务器是否允许该实际请求


实际请求与响应
```
# 请求
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

<?xml version="1.0"?><person><name>Arun</name></person> # 发送的非简单请求的内容
# content-type 为 xml
# 响应


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
# 允许浏览器访问响应内容, 如果不设置浏览器将不会把响应内容返回给请求的发送者
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



## 文档规范

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

## CSS 规范

- 用两个空格来代替制表符（tab） – 这是唯一能保证在所有环境下获得一致展现的方法。
- 为选择器分组时，将单独的选择器单独放在一行。
- 为了代码的易读性，在每个声明块的左花括号前添加一个空格。
- 声明块的右花括号应当单独成行。
- 每条声明语句的 : 后应该插入一个空格。
- 为了获得更准确的错误报告，每条声明都应该独占一行。
- 所有声明语句都应当以分号结尾。最后一条声明语句后面的分号是可选的，但是，如果省略这个分号，你的代码可能更易出错。
- 对于以逗号分隔的属性值，每个逗号后面都应该插入一个空格（例如，box-shadow）。
- 不要在 rgb()、rgba()、hsl()、hsla() 或 rect() 值的内部的逗号后面插入空格。这样利于从多个属性值（既加逗号也加空格）中区分多个颜色值（只加逗号，不加空格）。
- 对于属性值或颜色参数，省略小于 1 的小数前面的 0 （例如，.5 代替 0.5；-.5px 代替 -0.5px）。
- 十六进制值应该全部小写，例如，#fff。在扫描文档时，小写字符易于分辨，因为他们的形式更易于区分。
- 尽量使用简写形式的十六进制值，例如，用 #fff 代替 #ffffff。
- 为选择器中的属性添加双引号，例如，`input[type="text"]` 只有在某些情况下是可选的，但是，为了代码的一致性，建议都加上双引号。
- 避免为 0 值指定单位，例如，用 `margin: 0` 代替 `margin: 0px`


## 层叠和继承

CSS 样式来源：

- 创作人员:link(外部引用) < style(style 元素) < style:(style 元素)
- 读者:页面中会提供一些让用户自定义字体大小颜色等的快捷键
- 代理:浏览器

CSS的层叠&&继承

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

+ 三种盒子模型
	- content-box (默认值,标准), width 不包括 padding 和 border	
	- border-box width 包括 padding 和 border
	- padding-box`css3` width 只包括 padding

### 兼容性

一旦为页面设置了恰当的 DTD，大多数浏览器都会按照上面的图示来呈现内容。然而 IE 5 和 6 的呈现却是不正确的。

W3C 的规范盒子模型是为 content-box 但 IE5.X 和 6 在`怪异模式`中使用自己的非标准模型可以理解为 border-box

虽然有方法解决这个问题。但是目前最好的解决方案是回避这个问题。

*不要给元素添加具有指定宽度的内边距，而是尝试将内边距或外边距添加到元素的父元素和子元素。*

解决IE8及更早版本不兼容问题可以在HTML页面声明 `<!DOCTYPE html>` 即可

## CSSDOM(CSS 对象模型)


## CSS 解析

+ 优先级
	1. id选择器`#myid`
	2. 类选择器`.myclassname`
	3. 标签选择器`div,h1,p`
	4. 相邻选择器`h1+p`
	5. 子选择器`ul > li`
	6. 后代选择器`li a`
	7. 通配符选择器`*`
	8. 属性选择器`a[rel="external"]`
	9. 伪类选择器`a:hover, li:nth-child`
	- 最常用的选择器是类选择器。
	- li、td、dd等经常大量连续出现，并且样式相同或者相类似的标签，我们采用类选择器跟标签名选择器结合的后代选择器 .xx li/td/dd {} 的方式选择。
	- 极少的情况下会用ID选择器，当然很多前端开发人员喜欢header，footer，banner，conntent设置成ID选择器的，因为相同的样式在一个页面里不可能有第二次。

+ 解析(CSSOM)
	- 获取: CSS 解析器获取页面中 CSS, 可以是 link, style, style:
	+ 解析规则(解析CSS选择器用于生成浏览器可绘制的 CSS 数据结构)
		- 从右向左解析,如 sizzle 选择器引擎
		- CSS 解析完毕后,需要将解析的结果与 DOM Tree 的内容一起进行分析建立一棵 Render Tree,最终用来进行浏览器 `paint`
	+ 生成 CSS 数据结构
		- `margin: 10px;` 生成 `margin-top: 10px; margin-right: 10px; margin-bottom:10px; margin-left: 10px; `,也就是将属性都转换成普通属性写法
		- 计算属性单位值变成页面上的绝对单位值, `getComputedStyle() 可获取计算属性`
		- 级联(层叠样式) CSS 样式来源可以是多个,所以解析器用样式计算规则,取优先级最高的那个属性值,并层叠其它样式
		- 然后用样式表来源规则排序 style: > style > link, 得到最后的样式
	+ 构建 CSSOM 与 DOM Tree 一起 paint
		
+ 性能提升(减少浏览器 style 查找)
	1. 避免使用通用选择器
	2. 避免使用标签或 class 选择器限制 id 选择器
	3. 避免使用标签限制 class 选择器
	4. 避免使用多层标签选择器。使用 class 选择器替换，减少css查找
	5. 避免使用子选择器
	6. 使用继承
	7. 不必要的样式,避免给某些有默认样式的元素再赋同样的样式,(`img,input,button{vertical-align:baseline;}`)

### 小结
1. 在现代浏览器中大多数的选择器速度都非常快,因此使用不同的选择器来优化性能是没有必要的
2. `过度使用`的样式与`从未使用`的样式所占比例比`选择任何选择器`消耗的性能更多.所以将CSS库进行拆分,那可能是更好的选择;
3. 如果你的CSS库由多人编写,那么可以使用`UnCSS`这样的工具来自动删除未被使用的样式;
4. 明智的使用花括号内的属性才能赢的性能上的收益.在优化时,首先寻找”昂贵”的样式,会为你和用户带来最大的收益

# 可视化格式化模型(Visual formatting model)(CSS 元素,格式上下文,基线)

页面布局由盒子模型组成, DOM Tree 包含多个或 0 个盒子,每个盒子有它的宽度与高度

*布局是在盒子模型基础上完成的*
## 元素
+ 块级
	+ 块级元素
		- 通俗说法就是独占一行
		- 块级元素是源文档中会被视觉格式化为块状（例：段落）的元素
		- display 属性为 block | list-item | table 的元素就会成为一个`块级元素`
	+ 块级盒子
		- 块级元素生成一个`块级盒`
		- 块级盒子生成 `BFC`
	+ 匿名块盒
		- 一段不被任何元素包含的文本元素[匿名块盒](https://www.w3.org/TR/CSS2/visuren.html#anonymous-block-level)
	+ 块级容器
		- 只包含块级盒子
		- IFC 只包含行内级盒子
		- 不可替换行内块
		- 不替换的表格单元格
+ 行内级
	+ 行内元素
		- 行内级元素是在源文档中那些不为其内容形成新的块、其内容分布在多行中的元素
		- display 属性为 inline | inline-table | inline-block	
	+ 行内盒
		- `inline` 的不可替换元素
		- 行内级元素生成行内级盒
		- 块级盒子生成 `IFC`
	+ 原子行内级盒
		- inline 的可替换元素 | inline-block | inline-table
	+ 匿名行内盒
		- 同匿名块盒,不被任何行内级元素包含的文本元素
		- 空白内容，根据 white-space 属性
		- 如果可被折叠则不会产生任何匿名行内盒
+ display
	- block 产生一个块盒
	- inline-block 产生一个行内级块容器。行内块的内部会被当作块盒来格式化，而此元素本身会被当作原子行内级盒来格式化
	- inline 产生一个或多个的行内框
	- list-item 产生一个主块框或标记框(principal block box and marker box)

## 格式上下文

+ 块格式上下文(Block formatting contexts)
	- 指页面中的一个渲染区域，并且拥有一套渲染规则，他决定了其子元素如何定位，以及与其他元素的相互关系和作用
	- *块级格式化上下文并不是 display 属性值为 block,是块级盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域*
	- 一个 BFC 可以看作一个迷你布局,可以将任何子元素包括进去,其脱离文档流肯定就能产生 BFC
	- html 根节点就是典型的一个 BFC 也是页面中默认的第一个 BFC，页面中可以用满足以上的创建条件生成多个新的 BFC
	+ 创建条件
		1. 根元素 `html`
		2. 浮动元素（元素的 float 不是 none）
		3. 绝对定位元素（元素的 position 为 absolute 或 fixed）
		4. 行内块元素（元素的 display 为 inline-block）
		5. 表格单元格（元素的 display为 table-cell，HTML表格单元格默认为该值）
		6. 表格标题（元素的 display 为 table-caption，HTML表格标题默认为该值）
		7. 匿名表格单元格元素（元素的 display为 table、table-row、 table-row-group、table-header-group、table-footer-group（分别是HTML table、row、tbody、thead、tfoot的默认属性）或 inline-table）
		8. overflow 值不为 visible 的块元素
		9. display 值为 flow-root 的元素
		10. contain 值为 layout、content或 paint 的元素
		11. 弹性元素（display为 flex 或 inline-flex元素的直接子元素）
		12. 网格元素（display为 grid 或 inline-grid 元素的直接子元素）
		13. 多列容器（元素的 column-count 或 column-width 不为 auto，包括 column-count 为 1）
		14. column-span 为 all 的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中（标准变更，Chrome bug）

	+ 特性
		1. 解决外边距折叠
		2. 浮动清除, clear: both; 另一种方案

```
<style>
.parent { border: 1px solid #f00; }
.bfc { overflow: hidden; }
.box1 { width: 200px; height: 200px; background: pink; }
.box2 { width: 100px; height: 100px; background: lightgreen; }
.f_l { float: left; }
.margin20 { margin: 20px; }
</style>

<p>html根节点默认是一个BFC，&lt;div style="overflow: hidden;"&gt;&lt;/div&gt;生成多个新的BFC</p>
<p>div1先生成新的BFC，然后div2在根节点的BFC中排列</p>
<div class="parent bfc">
  <div class="f_l box2">div1</div>
  <div class="box1">div2</div>
</div>
<p>div2先在根节点的BFC中排列，然后div1生成新的BFC</p>
<div class="parent bfc">
  <div class="box1">div2</div>
  <div class="f_l box2">div1</div>
</div>
<p>div1先生成新的BFC，然后div2生成新的BFC</p>
<div class="parent bfc">
  <div class="f_l box2">div1</div>
  <div class="box1 bfc">div2</div>
</div>
<p>不触发BFC，margin重叠</p>
<div class="parent bfc">
  <div class="box1 margin20">div1</div>
  <div class="box1 margin20">div2</div>
</div>
<p>触发BFC，margin不重叠</p>
<div class="parent bfc">
  <div class="box2 margin20">div1</div>
  <div class="bfc">
    <div class="box2 margin20">div2</div>
  </div>
</div>
```

+ 行内格式化上下文(Inline formatting contexts)
	+ 创建条件 
		1. 块盒中有行内级元素
	+ 特性
		1. 子元素水平方向横向排列，并且垂直方向起点为元素顶部
		2. 子元素只会计算水平盒模型属性垂直并不会(padding,width,border)
		3. `vertical-align` 决定垂直方向上的对方方式
		4. 行框的宽度是由包含块和与其中的浮动来决定, 可以出现一行或多行
		5. 行框一般左右边贴紧其包含块，但 float 元素会优先排列
		6. 行框高度由 CSS 行高计算规则来确定，同个IFC下的多个行框高度可能会不同
		7. 行内级盒的总宽度少于包含它们的行框时，`text-align` 决定水平对齐方式
		8. 当一个行框超过父元素的宽度时 `word-wrap` 决定换行方式


margin-top margin-bottom 不生效:
```
<style>
.warp { border: 1px solid red; display: inline-block; }
.text { margin: 20px; background: green; }
</style>
<div class="warp">
  <span class="text">文本一</span>
  <span class="text">文本二</span>
</div>
```

浮动元素优先排列:
```
<style>
.warp { border: 1px solid red; width: 200px; }
.text { background: green; }
.f-l { float: left; }
</style>
<div class="warp">
    <span class="text">这是文本1</span>
    <span class="text">这是文本2</span>
    <span class="text f-l">这是文本3</span> <!-- 强制优先第二排排列 -->
    <span class="text">这是文本4</span>
</div>
```

IFC 高度由最高的行内元素决定:
```
<style>
.warp { border: 1px solid red; display: inline-block; }
.text { background: green; }
.h1 { line-height: 10px;}
.h2 { line-height: 20px;}
.h3 { line-height: 30px;}
.h4 { line-height: 40px;}
</style>
<div class="warp">
    <span class="text h1">这是文本1</span>
    <span class="text h2">这是文本2</span>
    <span class="text h3">这是文本3</span>
    <span class="text h4">这是文本4</span>
</div>
```

行内元素盒模型与其行内元素容器垂直对齐:
```HTML
<style>
.warp { border: 1px solid red; display: inline-block;}
.text { background: green; }
.warp img{ vertical-align: middle; }
</style>
<div class="warp">
  <span class="text">这是文本1</span>
  <img src="https://www.w3school.com.cn/i/eg_cute.gif">
  <span class="text">这是文本2</span>
  <span class="text">这是文本3</span>
  <span class="text">这是文本4</span>
</div>
```

## 基线


# 脚本

# JavaScript 基础(作用域/作用域链,运算符,nudefind&null,变量对象,提升,底层兼容(事件...))

## 作用域

作用域是指程序源代码中定义变量的区域。

作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。

JavaScript 采用词法作用域(lexical scoping)，也就是静态作用域。

+ 词法作用域(静态作用域)
  - 函数的作用域在函数定义的时候就决定了。
+ 动态作用域
  - 作用域在函数被调用的时候才决定。

```js
var value = 1;
function foo() {
  console.log(value);
}
function bar() {
  var value = 2;
  foo();
}
bar(); // 1
```
### 作用域链

当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。

这样由多个执行上下文的变量对象构成的链表就叫做作用域链。

函数的作用域在函数定义的时候就决定了。
+ 函数创建
  - 函数有一个内部属性 `[[scope]]`，当函数创建的时候，就会保存所有父变量对象到其中，你可以理解 [[scope]] 就是所有父变量对象的层级链，但是注意：[[scope]] 并不代表完整的作用域链！
  ```
  function foo() {
    function bar() {
        ...
    }
  }
  // 函数创建时，各自的[[scope]]为：
  foo.[[scope]] = [
    globalContext.VO
  ];
  bar.[[scope]] = [
    fooContext.AO,
    globalContext.VO
  ];
  ```
+ 函数激活
  - 当函数激活时，进入函数上下文，创建 VO/AO 后，就会将活动对象添加到作用链的前端。
  - 这时候执行上下文的作用域链，我们命名为 Scope：
  ```Scope = [AO].concat([[Scope]]);```


## 执行上下文

当变量提升时：
```js
var foo = function () {
  console.log('foo1');
}
foo();  // foo1
var foo = function () {
  console.log('foo2');
}
foo(); // foo2
```
当函数提升时：
```js
function foo() {
  console.log('foo1');
}
foo();  // foo2
function foo() {
  console.log('foo2');
}
foo(); // foo2
```

JavaScript 的可执行代码(executable code)的类型分三种，全局代码、函数代码、eval代码。

当执行到一个函数的时候，就会进行准备工作，这里的"准备工作"就叫做"执行上下文(execution context)"

当执行一个函数的时候，就会创建一个执行上下文，并且压入执行上下文栈，当函数执行完毕的时候，就会将函数的执行上下文从栈中弹出。

`执行上下文栈(Execution context stack，ECS)`来管理执行上下文


## 变量对象(Variable object，VO)

变量对象是与执行上下文相关的数据作用域，存储了在上下文中定义的变量和函数声明。

### 全局对象

1. 可以通过 this 引用，在客户端 JavaScript 中，全局对象就是 Window 对象。
2. 全局对象是由 Object 构造函数实例化的一个对象。
3. 预定义了一堆，一大堆函数和属性。
4. 作为全局变量的宿主。
5. 客户端 JavaScript 中，全局对象有 window 属性指向自身。

PS:见[W3School](https://www.w3school.com.cn/jsref/jsref_obj_global.asp)

全局函数和属性

- Infinity  代表正的无穷大的数值。
- java  代表 java.* 包层级的一个 JavaPackage。
- NaN 指示某个值是不是数字值。
- Packages  根 JavaPackage 对象。
- undefined 指示未定义的值。
- decodeURI() 解码某个编码的 URI。
- decodeURIComponent()  解码一个编码的 URI 组件。
- encodeURI() 把字符串编码为 URI。
- encodeURIComponent()  把字符串编码为 URI 组件。
- escape()  对字符串进行编码。
- eval()  计算 JavaScript 字符串，并把它作为脚本代码来执行。
- getClass()  返回一个 JavaObject 的 JavaClass。
- isFinite()  检查某个值是否为有穷大的数。
- isNaN() 检查某个值是否是数字。
- Number()  把对象的值转换为数字。
- parseFloat()  解析一个字符串并返回一个浮点数。
- parseInt()  解析一个字符串并返回一个整数。
- String()  把对象的值转换为字符串。
- unescape()  对由 escape() 编码的字符串进行解码。


### 活动对象

正如全局对象，当全局环境一产生，全局对象就由此诞生。

在函数上下文中，就用活动对象(activation object, AO)来表示变量对象。

活动对象是在进入函数上下文时刻被创建的，它通过函数的 arguments 属性初始化。arguments 属性值是 Arguments 对象。


执行上下文的代码会分成两个阶段进行处理：
+ 分析，当进入执行上下文(这时候还没有执行代码)，变量对象会包括
  1. 函数的所有形参 (如果是函数上下文)
    - 由名称和对应值组成的一个变量对象的属性被创建
    - 没有实参，属性值设为 undefined
  2. 函数声明
    - 由名称和对应值（函数对象(function-object)）组成一个变量对象的属性被创建
    - 如果变量对象已经存在相同名称的属性，则完全替换这个属性
  3. 变量声明
    - 由名称和对应值（undefined）组成一个变量对象的属性被创建；
    - 如果变量名称跟已经声明的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性
+ 执行，代码执行
  - 在代码执行阶段，会顺序执行代码，根据代码，修改变量对象的值


```js
function foo(a) {
  var b = 2;
  function c() {}
  var d = function() {};
  b = 3;
}
foo(1);

// 在进入上下文后，AO为
AO = {
  arguments: {
    0: 1,
    length: 1
  },
  a: 1,
  b: undefined,
  c: reference to function c(){},
  d: undefined
}
// 执行完成后，AO为
AO = {
  arguments: {
    0: 1,
    length: 1
  },
  a: 1,
  b: 3,
  c: reference to function c(){},
  d: reference to FunctionExpression "d"
}
```

## undefined & null

*undefined和null在if语句中，都会被自动转为false*

+ null 表示"没有对象"，即该处不应该有值
  1. 作为函数的参数，表示该函数的参数不是对象。
  2. 作为对象原型链的终点。
+ undefined 表示"缺少值"，就是此处应该有一个值，但是还没有定义
  1. 变量被声明了，但没有赋值时，就等于undefined。
  2. 调用函数时，应该提供的参数没有提供，该参数等于undefined。
  3. 对象没有赋值的属性，该属性的值为undefined。
  4. 函数没有返回值时，默认返回undefined。

## 变量提升 

- 所有的声明都会提升到作用域的最顶上去。
- 同一个变量只会声明一次，其他的会被忽略掉或者覆盖掉。
- 函数声明的优先级高于变量申明的优先级，并且函数声明和函数定义的部分一起被提升。



## 事件驱动--addEventListener

EventTarget.addEventListener() 方法将指定的监听器注册到 EventTarget 上，当该对象触发指定的事件时，指定的回调函数就会被执行。 事件目标可以是一个文档上的元素 Element,Document 和 Window 或者任何其他支持事件的对象 (比如 XMLHttpRequest)

- 允许给一个事件注册多个监听器。 特别是在使用AJAX库，JavaScript模块，或其他需要第三方库/插件的代码。
- 提供了一种更精细的手段控制 listener 的触发阶段。（即可以选择捕获或者冒泡）。
- 对任何 DOM 元素都是有效的，而不仅仅只对 HTML 元素有效。

```javascript
target.addEventListener(String type, [Function | Object] listener[, options]);
target.addEventListener(String type, [Function | Object] listener[, useCapture]);
target.addEventListener(String type, [Function | Object] listener[, useCapture, wantsUntrusted  ]);  // Gecko/Mozilla only
```

- type 事件类型的字符串。
- listener 一个回调函数或者是一个实现了 Event 接口的对象
  *回调时箭头函数时，原函数中可用的变量和常量在事件处理器中同样可用*
+ options  一个指定有关 listener 属性的可选参数对象。可用的选项如下：
  - capture:  Boolean，表示 listener 会在该类型的事件捕获阶段传播到该 EventTarget 时触发。
  - once:  Boolean，表示 listener 在添加之后最多只调用一次。如果是 true， listener 会在其被调用之后自动移除。
  - passive: Boolean，设置为true时，表示 listener 永远不会调用 preventDefault()。如果 listener 仍然调用了这个函数，客户端将会忽略它并抛出一个控制台警告。
  - mozSystemGroup: 只能在 XBL 或者是 Firefox' chrome 使用，这是个 Boolean，表示 listener 被添加到 system group。
- useCapture


事件监听器可以被指定为回调函数或实现`EventListener`的对象，其`handleEvent()`方法用作回调函数

*回调要么是一个函数要么就是一个实现了 EventListener 的对象里面的 handleEvent() 方法*

回调函数本身具有与 `handleEvent()` 方法相同的参数和返回值;也就是说，回调接受一个参数：一个基于Event 的对象

### 事件原型
```js
(function() {
  if (!Event.prototype.preventDefault) {
    Event.prototype.preventDefault=function() {
      this.returnValue=false;
    };
  }
  if (!Event.prototype.stopPropagation) {
    Event.prototype.stopPropagation=function() {
      this.cancelBubble=true;
    };
  }
  if (!Element.prototype.addEventListener) {
    var eventListeners=[];
    
    var addEventListener=function(type,listener /*, useCapture (will be ignored) */) {
      var self=this;
      var wrapper=function(e) {
        e.target=e.srcElement;
        e.currentTarget=self;
        if (typeof listener.handleEvent != 'undefined') {
          listener.handleEvent(e);
        } else {
          listener.call(self,e);
        }
      };
      if (type=="DOMContentLoaded") {
        var wrapper2=function(e) {
          if (document.readyState=="complete") {
            wrapper(e);
          }
        };
        document.attachEvent("onreadystatechange",wrapper2);
        eventListeners.push({object:this,type:type,listener:listener,wrapper:wrapper2});
        
        if (document.readyState=="complete") {
          var e=new Event();
          e.srcElement=window;
          wrapper2(e);
        }
      } else {
        this.attachEvent("on"+type,wrapper);
        eventListeners.push({object:this,type:type,listener:listener,wrapper:wrapper});
      }
    };
    var removeEventListener=function(type,listener /*, useCapture (will be ignored) */) {
      var counter=0;
      while (counter<eventListeners.length) {
        var eventListener=eventListeners[counter];
        if (eventListener.object==this && eventListener.type==type && eventListener.listener==listener) {
          if (type=="DOMContentLoaded") {
            this.detachEvent("onreadystatechange",eventListener.wrapper);
          } else {
            this.detachEvent("on"+type,eventListener.wrapper);
          }
          eventListeners.splice(counter, 1);
          break;
        }
        ++counter;
      }
    };
    Element.prototype.addEventListener=addEventListener;
    Element.prototype.removeEventListener=removeEventListener;
    if (HTMLDocument) {
      HTMLDocument.prototype.addEventListener=addEventListener;
      HTMLDocument.prototype.removeEventListener=removeEventListener;
    }
    if (Window) {
      Window.prototype.addEventListener=addEventListener;
      Window.prototype.removeEventListener=removeEventListener;
    }
  }
})();
```


以上来自 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener)

### jQuery 事件

不得不说 jQuery 中换了一种思维,保留了事件原型中的大部分特性

+ $.Event 事件对象
  - $.Event 与原生 event 相全部得到的一个全新事件对象
+ 事件注册
  - `on()` 入口，与传统事件绑定不同，可以绑定多个事件并独立执行
  - 用 $.Callbacks 回调存放,并没每个回调设置标识，可以直接用 `off()` 直接移除
  - 并且回调也可以是对象
+ 事件执行
  - 事件触发执行 $.Event
  - 事件队列里的回调全部执行
+ 抛弃原生捕获和冒泡
  - 模拟捕获和冒泡
- 支持自定义事件
- 手动触发



# JavaScript 基础(函数一等公民,闭包,原型,继承,this,三种对象(内置,宿主...))

## 类型转换

基本类型（基本数值、基本数据类型）是一种既非对象也无方法的数据。在 JavaScript 中，共有7种基本类型：string，number，bigint，boolean，null，undefined，symbol (ECMAScript 2016新增)。

除了 null 和 undefined之外，所有基本类型都有其对应的包装对象：String,Number,Bigint,Boolean,Symbol,而包装对象的`valueOf()`方法可以得到该对象的`基本类型`也就是`原始值`

下面是几个对象的基本类型例子:
```js
// console.log( 5.valueOf() ) // SyntaxError: Invalid or unexpected token
console.log( Number(5).valueOf() ) //=> 5
console.log( String('kobe').valueOf() ) //=> kobe
console.log( true.valueOf() ) //=> true
console.log( Boolean(true).valueOf() ) //=> true
console.log( Symbol('foo').valueOf() ) //=> Symbol(foo)
```

其中这里要解释一下,`true.valueOf()`也有原始值,这是因为 true 本身而言,是一个布尔值字面量,这与`[]`是一样的

JavaScript 调用`valueOf`方法将对象转换为原始值。你很少需要自己调用 valueOf 方法；当遇到要预期的原始值的对象时，JavaScript 会自动调用它。

[String 对象的 valueOf 方法返回一个String对象的原始值。该值等同于String.prototype.toString()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/valueOf)

其中每个对象的包装类型的构造器可以用来强制转换数据类型

```js
console.log( Number('123') ) //=> 123
console.log( String('123') ) //=> "123"
console.log( Boolean('kobe') ) //=> true
console.log( Symbol('kobe') ) //=> Symbol(kobe)
```

如果当一个没有基本类型的值调用 valueOf 时，会返回对象本身

*Math 和 Error 对象没有实现 valueOf() 方法*

### 隐式转换

JavaScript 遇到预期为一个类型时,比如预期布尔值的地方（比如if语句的条件部分），就会将非布尔值的参数自动转换为布尔值。系统内部会自动调用Boolean函数

记住一个规则:运算返回什么类型,运算的操作数就会转换成什么类型

比如算术运算符的二元运算`-`,因为`-`运算符结果是数值,也就会将操作数转换成数值,下面是一个表达式
```js
console.log( Number(true)) //=> 1
console.log( Number(false)) //=> 0
console.log( true - false ) //=> 1
```
*隐式转换时 toString 和 valueOf 方法是关键,不要随意重写,著名的 jQuery 原型污染就是攻击者重写 Object 原型上的 toString 方法导致类型转换的混乱*

### `+`运算符

`+`运算符在 JavaScript 中是个非常特别的运算符,可以当作一元运算符,也可以当作二元运算符,并且如果当作二元操作符时,操作数有一个是字符串,则会被视为字符串连接,而不会当作数值相加

下面是个特别的例子:
```js
console.log( [] + 1 ) //=> 1
console.log( typeof ([] + 1) ) //=> string
console.log( 1 + [1] ) //=> 11
console.log( typeof ([] + 1) ) //=> string
```

再用一个例子来分析转换过程:
```js
const originToTtring = Array.prototype.toString
Array.prototype.toString = function(){
  console.log('[] use tostring')
  return originToTtring.call(this, arguments)
}
const originVluaeOf = Array.prototype.valueOf
Array.prototype.valueOf = function(){
  console.log('[] use valueOf')
  return originVluaeOf.call(this, arguments)
}

console.log( [] + 1 )
// [] use valueOf
// [] use tostring
//=> '1'
```

此处的`+`并不知道是用作字符串连接还是数值相加

1. 数字`1`本身是`Number`的原始值,则不在需要转换
2. `[]`空数组,先调用`valueOf`转换成原始值,结果还是一个空数组,是一个对象
3. 当对象进行`+`运算,将对象转换成字符串,`[].toString()`结果为空字符串
4. 由于这里`+`当作二元运算符操作,有一个操作数是字符串,那么结果为字符串

JavaScript 中的表达式当`+`作为一元运算符时,结果就不会是两种情况就一种,返回数值,所以一元`+`运算符和数值包装类`Number`构造器一致
```js
console.log( +[1] )
// [] use valueOf
// [] use tostring
//=> 1
```
1. `[]`首先调用`valueOf()`转换成原型类型
2. 转换后的值依然是一个对象,则`toString()`,则为`"1"`
3. 当`+`作为一元运算符返回数值,相当于`Number("1")`结果为 1

*相反的其它可以当作一元运算符的都与此类似*

[相关运算符见](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

## 三种对象

- 原生对象：Object、Function、Array、String、Boolean、Number、Date、RegExp、Error、EvalError、RangeError、ReferenceError、SyntaxError、TypeError、URIError、Global
- 内置对象：Global（全局对象）、Math
- 宿主对象：有宿主提供的对象，在浏览器中window对象以及其下边所有的子对象(如bom、dom等等)，在node中是globla及其子对象，也包含自定义的类对象。【何为“宿主对象”？  在web中，ECMAScript中的“宿主”当然就是我们网页的运行环境，即“操作系统”和“浏览器”。所有非本地对象都是宿主对象（host object），即由 ECMAScript 实现的宿主环境提供的对象。】
- 全局对象：一般全局对象会有两个，一个是ecma提供的Global对象，一个是宿主提供。如在浏览器中是window、在nodejs中是global。【所以啊，在浏览器中全局对象是Global+window】
通常情况下ecma提供的Global对象对是不存在的，没有具体的对象，

## 函数一等公民

JavaScript 中一切皆对象，这是语言层面上的一切皆对象，而代码层面上更像是一切皆函数

回顾一下函数在 JavaScript 中存在的形式

```js
// 1. 函数声明
function bar(){}
// 2. 函数表达式
let foo = function(){}
// 3. 匿名函数，比如事件处理回调
$(function(){})
// 4. 构造函数
let Parent = function( name, age ){
  this.name = name;
  this.age = age;
}
// 5. 函数作为参数
setTimeout(function(){
  // 匿名函数体内
}, 100)
// 6. 函数作为返回参数
var check = function( reg ){
  return function( text ){
    return reg.test(text)
  }
}
var checkNumber = check(/^[0-9]+$/) // 利用 check 返回的函数作为另一个函数给 checkNumber 
console.log( checkNumber(100) ) // true
// 同时这里也用到了 curry 
// 7. es6 之前表示一个作用域
var scope = 'eee'
(function(){
  var scope = 'abc'
});
// 8. 自我执行函数，模块化
( function( global, factory ) {
  // define
}( typeof window !== "undefined" ? window : this, function( window, noGlobal ) {
  // main
} ) );
// 9. ES6 新增特性箭头函数
var foo = args => args + 1
// ...
```

通常情况下函数调用完成，其内在的所有局部变量和自己都会被垃圾收回自动销毁

## 闭包

闭包是一种保护私有变量的机制，在函数执行时形成私有的作用域，保护里面的私有变量不受外界干扰。

直观的说就是形成一个不销毁的栈环境，让函数或变量可以驻留在内存中,看下面实例

```js
var regCheck = function( reg ){
  return function( text ){
    return reg.test(text)
  }
}
var checkNumber = regCheck(/^[0-9]+$/)
const checkEmail = regCheck(/^(\d|\w)+\@(\d|\w)+\.(com|cn)$/)
console.log( checkNumber('123') )  //=> true
console.log( checkNumber('1d23') ) //=> false
console.log( checkEmail('12adsf3@sina.com') )  //=> true
```
这是一个闭包的流行使用方式，柯里化


## 从规范了解 this

ECMAScript 的类型分为语言类型和规范类型。

+ 语言类型
  - 语言类型是开发者直接使用 ECMAScript 可以操作的
  - 也就是熟悉的 Undefined, Null, Boolean, String, Number, 和 Object
+ 规范类型
  - 用来用算法描述 ECMAScript 语言结构和 ECMAScript 语言类型的
  - Reference, List, Completion, Property Descriptor, Property Identifier, Lexical Environment, 和 Environment Record

### Reference

规范8.7章中解释 Reference 类型就是用来解释诸如 delete、typeof 以及赋值等操作行为的，Reference 的构成，由三个组成部分，分别是：
- base value： 属性所在的对象或者就是 `EnvironmentRecord`,它的值只可能是 undefined,Object,Boolean,String,Number,environment record 其中的一种
- referenced name： 属性的名称
- strict reference：是否严格引用

一个 base value 可以是一个对象，如下面例子：
```js
// 一个用户定义的对象
var foo = {
  bar: function () {
    return this;
  }
};
foo.bar();
// 对应成规范中的 Reference 就是下面对象所以描述
var BarReference = {
  base: foo,
  propertyName: 'bar',
  strict: false
};
```

规范中获取 Reference 组成部分的方法
- GetBase(V)。 返回对象属性的具体值,而不再是一个 Reference
- GetReferencedName(V)。 返回引用值 V 的引用名称组件。
- IsStrictReference(V)。 返回引用值 V 的严格引用组件。
- HasPrimitiveBase(V)。 如果基值是 Boolean, String, Number，那么返回 true。
- IsPropertyReference(V)。 如果基值是个对象或 HasPrimitiveBase(V) 是 true，那么返回 true；否则返回 false。
- IsUnresolvableReference(V)。 如果基值是 undefined 那么返回 true，否则返回 false。

### MemberExpression 

从字面意思上可以看出成员表达式的意思，但在规范中指出 MemberExpression 可以是以下几种
- PrimaryExpression : Identifier原始表达式
- FunctionExpression  函数定义表达式
- MemberExpression [ Expression ]  属性访问表达式
- MemberExpression . IdentifierName  属性访问表达式
- new MemberExpression Arguments  对象创建表达式

```js
function foo(){
}
foo() // foo 是 MemberExpression,也就是 ()左边的部分
```

### 确认 this

规范 11.2.3 第一步，第六步和第七步所述，确认 this 过程如：
1. 计算 MemberExpression 表达式的结果赋值给 ref
2. 判断 ref 是不是一个 Reference
  1. 如果 ref 是 Reference，并且 IsPropertyReference(ref) 是 true, 那么 this 的值为 GetBase(ref)
  2. 如果 ref 是 Reference，并且 base value 值是 Environment Record, 那么this的值为 ImplicitThisValue(ref)
  3. 如果 ref 不是 Reference，那么 this 的值为 undefined

*非严格模式下 this 为 undefined 时，会隐式的转换成全局对象*

看下面各各例子

```js
var value = 1;
var foo = {
  value: 2,
  bar: function () {
    return this.value;
  }
}
//示例1
console.log(foo.bar());
//示例2
console.log((foo.bar)());
//示例3
console.log((foo.bar = foo.bar)());
//示例4
console.log((false || foo.bar)());
//示例5
console.log((foo.bar, foo.bar)());

function bar (){
  console.log(this)
}
bar()
```
+ 执行上下文表达式

也就是原始表达式，函数调用情况时，MemberExpression 是 foo, 即标识符调用

规范 10.3.1 最后会调用  `GetIdentifierReference()` 方法直接返回该标识的 Reference 类型的值如下：
```js
var fooReference = {
  base: EnvironmentRecord,
  name: 'foo',
  strict: false
};
```
这时虽然返回的是一个 Reference 但是 IsPropertyReference() 并不是 true,因为 EnvironmentRecord 虽是引用类型的取值的一种但不是对象，所以 

this 应该是 ImplicitThisValue(ref) ，规范 10.2.1.1.6 解释到 `Return undefined`, 所以最后的 this 应该就是 undefined,非严格模式下就是 window

+ 属性访问表达式
示例1的 MemberExpression 是一个属性访问表达式，也就是 MemberExpression . IdentifierName 结果是 foo.bar, 根据规范 11.2.1 所述,该 MemberExpression 返回的是一个 Reference, 也就是返回结果应该是如下：
```js
var ref = {
  base: foo,
  name: 'bar',
  strict: false
}
```
此时的 MemberExpression 结果就是一个 Reference,而 IsPropertyReference(ref) 也等于 true
按照确认 this 步骤 2.1 所描述，将 MemberExpression 结果赋值给 ref, 如果 ref 是一个 Reference, 并且 IsPropertyReference(ref) 是 true,此时就可以确认 `this=GetBase(ref)`, 此时 GetBase 返回的则是 foo，那么 this 就等于 foo，所以该例子的结果应该是 2

+ 括号成员表达式
foo.bar 被 () 包住，根据规范 11.1.6 The Grouping Operator 所述,实际上 () 并没有对 MemberExpression 进行计算，所以其实跟示例 1 的结果是一样的。

+ 赋值运算

示例3可以看出是不仅有括号包裹还有赋值去处，根据规范 11.13.1  Simple Assignment, 也就是等号运算,等号运算符返回右操作数，这里也就是右边的 `foo.bar`

规范里面明确说明了该情况时会调用 `GetValue()` 返回值,而 GetValue() 在 规范 8.7.1 中解释不会返回 Reference 类型

也就上面所述 ref 并不是一个 Reference, this 为 undefined，当前情况并没有在严格模式下，所以这里的 this 应该就是 window

+ 逻辑运算,逗号运算

同样调用了 GetValue() 结果和上一个示例一致

综上所述，用网上一句话解释就是 this 为调用函数的对象


## 原型

- prototype： 类指向的原型属性
- constructor： 原型指向的类（构造器）
- `__proto__`： 实例指向的原型对象

JavaScript 中一个类就是一个构造函数,当一个类(构造函数)存在后，会自动创建一个与之独立的原型对象
```js
function Money(){ }
```
当 Money 类被实例化后，就是被 new 一次后，这个被 new 的对象就拥有了类(构造函数)中的成员,如果类中没有则会去找这个类的原型中的成员

Money 类被创建就会产生一个 pototype 属性,该属性就是表示其原型 

函数本身就有 length,name,arguments,caller,prototype 这些属性，函数也是对象，所以构造函数也如此

可以理解为 Money 本身是一个类,实例化后就会得到一个对象,而 prototype 也是一个类实例化后的对象，只是 Money 是默认继承了它，它们相互存在却又相互独立

虽然类和原型是独立的，但它们两个之间也可以用 `prototpye` 和 `constructor` 两个属性相互访问
```js
console.log( Money.prototype) // Money 类访问它的原型
console.log( Money.prototype.constructor) // Money 类的原型访问它的构造器
```
*值得注意的是这两个属性一直相互调用访问对方*
*原型链和作用域链是一个道理,向上寻找的过程就是原型链*

而 `__proto__` 这个属性就有意思了，它表示实例对象的原型对象,也就是说 `Money.prototype === (new Money()).__proto__` 是成立的,只是从不同的角度都能够得到原型对象

### 原型指向

```js
function Dollar(){ }
function RMB(){ }
Dollar.prototype.syb = '$'
RMB.prototype.syb = '￥'

// 改变原型
var dollar = new Dollar()
console.log(dollar.__proto__) // Dollar { syb: '$' }
console.log(dollar.syb) // $
Dollar.prototype = RMB.prototype
console.log(dollar.__proto__) // Dollar { syb: '$' }
dollar = new Dollar()
console.log(dollar.syb) // ￥
console.log(dollar.__proto__.constructor) // [Function: Dollar]
```
```js
function Dollar(){ }
function RMB(){ }
Dollar.prototype = {
  syb : '$'
}
RMB.prototype = {
  syb : '￥'
}

var dollar = new Dollar()
console.log(dollar.__proto__) // { syb: '$' }
console.log(dollar.syb) // $
Dollar.prototype = RMB.prototype
console.log(dollar.__proto__) // { syb: '$' }
dollar = new Dollar()
console.log(dollar.syb) // ￥
console.log(dollar.__proto__.constructor) // [Function: Object]
```

上面是两种不同方式的改变原型指向方法,第一方法只是为原型添加了 syb 属性,会保留构造指向,其它的什么也没改变
第二种方法则不同了，首先将原型改变成了一个新的对象,并且会丢失构造指向，也就是同时会丢失原来原型指向和你先指向
当原型指向改变第二次实例化的对象原型指向也会发生改变，也就会出现第二次实例时打印出的人民币而不是美元符号
因为字面量对象写法也相当于做了一个 new Object()，所以第二个例子中的最后一行的原型也就是会变一个 Object 的实例对象

```js

function Dollar(){ }
function RMB(){ }
Dollar.prototype.syb = '$'
RMB.prototype.syb = '￥'

// 改变原型
var dollar = new Dollar()
var dollar2 = new Dollar()
console.log(dollar.syb) // $
console.log(dollar2.syb) // $
dollar.__proto__ = RMB.prototype
console.log(dollar.syb) // ￥
console.log(dollar2.syb) // $
```
利用上面的定义再来一次
```js
var dollar = new Dollar()
var dollar2 = new Dollar()
console.log(dollar.syb) // $
console.log(dollar2.syb) // $
Dollar.prototype = RMB.prototype
console.log(dollar.syb) // $
console.log(dollar2.syb) // $
```
类指向的原型，会影响到每一个实例
实例调用指向的原型只会影响到当前实例

### Object 与 Function

Object 原型链的顶端, Object 构造器是 Function 的实例,Object 作为对象是继承自 Function.prototype 的,此时 Function.prototype 继承自 Object.prototype
Function 与 Object 是同等级但不同层次，Function 是自己的构造函数
也有的说只有 Object 是原型链的顶端，Function 是继承 Object 的

下面来看几段例子:
```js
// 向函数原型添加 my
// 因为原型链的关系 继承自 Function.prototype
Function.prototype.my = function(){
  return 200;
}
// 如果 Object 在向上搜索的时候没有在 Function 中找到就去 Object 查找
Object.prototype.me = function(){
  return 300;
}

var f = new Function()
var o = new Object()
console.log(Object.my()); //=> 200
console.log(Object.me()); //=> 300
console.log(Function.my()); //=> 200
console.log(Function.me()); //=> 300
console.log(f.my()); //=> 200
console.log(f.me()); //=> 300
// console.log(o.my()); // TypeError: o.my is not a function
console.log(o.me()); //=> 300
// 这也说明了 Object 作为对象是继承自 Function.prototype 的 
```

在类的层次上, Function 和 Object 可以看作是相互继承
而在实例的层次上，new Function() 和 new Object() 却不是，当实例 o.my() 在向上寻找过程中, 直接去了父类 Object 的原型，当 Object 原型中也不存在时，也就是到了原型链的顶端(Object 之上), null


```js
// 实例原型顶端
console.log(Object.prototype.__proto__) //=> null
console.log(Function.prototype.__proto__ == Object.prototype) //=> true
console.log(Function.prototype.__proto__.__proto__ == Object.prototype.__proto__) //=> true
```
*null 为原型链的终点*

## 继承

在原生的 JS 实现继承并非是轻松，也显得很简单，就比如组合继承，组合继承很类似其它面向对象语言一样，可以在类本体中调用类型构造一样，或者是原型式的继承，将类的原型作为需要继承对象的一个实例，这个地方就不在一一介绍各种继承的实现，《JavaScript高级程序设计》 有更详细的解释

这里就来说说 bable 转码后的 es6 的继承实现

```js
class Person{
  constructor(){
    this.money = 100
  }
}
class Student extends Person{
  constructor(){
    super()
    this.age = 20
  }
}
let sutdent = new Student()
console.log(sutdent.age) //=> 20
console.log(sutdent.money) //=> 100
```

这是一段很普通的 es6 的继承，下面看一下 bable 转换后的 es5 继承方式

```js
"use strict";

function _possibleConstructorReturn(self, call) {
  if (!self) {
    throw new ReferenceError("this hasn't been initialised - super() hasn't been called");
  }
  return call && (typeof call === "object" || typeof call === "function") ? call : self;
}

function _inherits(subClass, superClass) {
  if (typeof superClass !== "function" && superClass !== null) {
    throw new TypeError("Super expression must either be null or a function, not " + typeof superClass);
  }
  // Object.create 创建一个全新的对象
  subClass.prototype = Object.create(superClass && superClass.prototype, {
    constructor: {
      value: subClass,
      enumerable: false,
      writable: true,
      configurable: true
    }
  });
  // 设置 subClass 原型为 superClass 
  // 浏览器不支持 setPrototypeOf 则用 __proto__ 属性代替
  if (superClass) Object.setPrototypeOf ? Object.setPrototypeOf(subClass, superClass) : subClass.__proto__ = superClass;
}

function _classCallCheck(instance, Constructor) {
  if (!(instance instanceof Constructor)) {
    throw new TypeError("Cannot call a class as a function");
  }
}

var Person = function Person() {
  _classCallCheck(this, Person);

  this.money = 100;
};

var Student = function(_Person) {
  _inherits(Student, _Person);

  // Student 闭包返回 Student
  function Student() {
    _classCallCheck(this, Student);

    var _this = _possibleConstructorReturn(this, (Student.__proto__ || Object.getPrototypeOf(Student)).call(this));

    _this.age = 20;
    return _this;
  }

  return Student;
}(Person); // 隐式的传入需要继承对象的构造

var sutdent = new Student();
console.log(sutdent.age);
console.log(sutdent.money);
```

仔细阅读会发现 bable 不仅用上了经典的构造器还用上了组合寄生的方式实现继承
- 每一个构造在调用时都用了 `_classCallCheck` 方法判断是否用了 new 操作，如果没有用 new 操作则直接函数调用，会抛出错误
- `_inherits` 方法主要实现了继承，用 `superClass` 的原生生成一个全新对象作为 `subClass` 原型对象,重写 `construcotr` 方法, 最后用 `Object.setPrototypeOf()` 方法设置一个指定的对象的原型为父类达到继承
- `_possibleConstructorReturn` 方法在子类内部完成 this 的绑定解决子类与父类 this 指向

# JavaScript 基础 异步解决方案

JavaScript 是一门单线程语言,即一次只能完成一个任务,若有多个任务要执行,则必须排队按照队列来执行(前一个任务完成,再执行下一个任务)。异步解决方案也是主要是为了解决了 JavaScript 单线程上I/O操作时带来的耗时,因JS加载而页面渲染的卡顿等问题。以下是在 JavaScript 中异步的几种处理方法:

1. 回调队列(异步编程最基本方法)
2. 事件监听
3. Promise, CommandJS提出的一种规范，其目的是为异步编程提供统一接口
4. gengerator
5. async await
6. nextTick setImmidate 
7. [async.js](https://github.com/caolan/async)

## Promise/A+ 规范

一个 Promise 就是一个异步操作的结果,当然在 JavaScript 世界里就是一个对象，与 Promise 进行交互的最主要方式就是通过 `then()` 方法, then 方法也是 Promise 接口必需要实现的方法,详情可见 [commonjs Promises/A+规范](http://wiki.commonjs.org/wiki/Promises/A)

首先 Promise 生命周期,也就是 Promise 的三个阶段
1. Pending 初始化状态
2. Fulfilled 完成状态,此状态必须有一个值,且不能改变
3. Rejected 被拒绝状态,此状态也必须有一个值,且不能改变

### promise.then(onFulfilled, onRejected)

Promise 接口实现必须提供一个 then 方法去接收当前或者最终结果,从这里其实可以看出 Promise 也可以不做异步操作,因为可以直接操作当前的最终结果

方法 then 有以下几个*不得不做*的事情:

1. onFulfilled 和 onRejected 可选,但是如果有参数时必须时函数
2. 当 onFulfilled 和 onRejected 作为函数时,只能在 promise 完成||被拒绝之后调用一次,且 promise 作为第一个参数
3. 两个函数参数不能在当前 promise 之前调用
  - 这个规范在 [3.2](https://promisesaplus.com/#notes) 有说明,意思就是可以在将当前的 promise 结果后通过其它的方式实现 `nextTick` 调度,确保事件循环之后并使用新堆栈异步执行onFulfilled 和 onRejected 
4. 规范说明两个参数必须以函数调用,也就是把函数当作一种纯函数调用,不存在 this 上下文关系的函数
5. 可以在同一次 promise 中多次被调用,但必须按照其对应的原始调用顺序执行
6. 必须返回 promise, 也就是如果一个 then 函数参数返回一个值,后面接收这个值的地方必将是一个 promise

从规范中可以看出一个 promise 必须实现 then 方法,且要满足以上几个条件,并且还可以在当前事件循环之后用异步方式调用 then 参数函数

### ES6 Promise 实现 


ES6 中一个 `new Promise( executor )` 表示一个 promise 异步操作的最终完成 (或失败), 及其结果值, `executor` 是带有 `resolve` 和 `reject` 两个参数的函数, 并且 executor 函数在 Promise 构造函数返回 promise 实例对象前被调用

resolve 被调用时,会把 promise 状态更改为 Fulfilled, es6 中就是已经完成

rejected 被调用时,会把 promise 状态更改为 Rejected 也就是失败

executor 内部操作完成后可能成功也可能失败,但如果内部出现异常, 那么该 promise 状态就为 Rejected, executor 的返回值也会被忽略


- Promise.length 其值总是为 1 (构造器参数的数目)
+ Promise.all(iterable)
  - iterable 是一个可迭代的 promise 集合,只有当集合中每一个 promise 都成功才会执行,集合中所有 promise 返回值的数组作为成功回调的返回值,最后返回一个新的 promise
+ Promise.race(iterable)
  - 当 iterable 参数里的任意一个子 promise 被成功或失败后,父 promise 马上也会用子 promise 的成功返回值或失败详情作为参数调用父 promise 绑定的相应句柄，并返回该 promise 对象
+ Promise.reject(reason)
  - 返回一个状态为失败的Promise对象，并将给定的失败信息传递给对应的处理方法
+ Promise.resolve(value)
  - 返回一个状态由给定 value 决定的 Promise 对象
  - 如果 value 是一个带有 then 方法的对象则返回的 promise 由 value.then 决定
  - 如果不是则返回的 Promise 对象状态为 fulfilled,并且将该value传递给对应的then方法。
  - *有时也可以利用该方法特性检测 value 是否是一个 promise 对象*
  ```js
  var o = {
   then(){
     console.log('o.then')
   }
  }
  // 有 then 方法的对象
  Promise.resolve(o).then(() => {
   console.log('native then')
  })
  // o.then
  ```
  ```js
  var o = {
    run(){
      console.log('o.then')
    }
  }
  // 不是有 then 方法的对象
  Promise.resolve(o).then( v => {
    v.run() // o.then
    console.log('native then') // native then
  })
  // o.then
  ```
+ Promise.prototype.catch(onRejected)
  - 添加一个拒绝(rejection) 回调到当前 promise, 返回一个新的 promise
+ Promise.prototype.then(onFulfilled, onRejected)
  - 添加一个成功和失败回调, 返回一个新的 promise, 将以回调的返回值来 resolve.
+ Promise.prototype.finally(onFinally)
  - 无论当前 promise 状态都会执行

*值得注意的是 ES6 的 promise 实现不是支持 IE 的*

## async/await 



# JavaScript 基础(AMD,UMD,ES6,TypeScript(静态),Node.JS(包管理))

AMD,CMD,CommonJS 这三个规范都是为 js 模块化加载而生的，都是在用到或者预计要用到某些模块时候加载该模块,使得大量的系统巨大的庞杂的代码得以很好的组织和管理,模块化使得我们在使用和管理代码的时候不那么混乱，而且也方便了多人的合作

## CommonJS

一个有目标的构建 JavaScript 生态系统 Web 服务器组，在浏览器和命令行应用程序和桌面 `CommonJs` 是一个更偏向于服务器端的规范, `Node.js` 采用了这个规范,根据 CommonJS 规范，一个单独的文件就是一个模块加载模块使用`require`方法，该方法读取一个文件并执行，最后返回文件内部的`exports`对象同步加载模块

- require() 模块引用,用来引入外部模块
- exports 模块定义,对象用于导出当前模块的方法或变量，唯一的导出口
- module 模块标识,对象就代表模块本身

在 CommonJS 中，有一个全局性方法 require()，用于加载模块假设有一个 Math.js 模块，就可以这样加载
```js
var math = require('Math');
console.log(math)
```

浏览器不兼容 CommonJS 的根本原因，在于缺少四个 Node.js 环境的变量 module,exports,require,global,也就是说在浏览器不认识这四个全局的变量
```js
$ node # 进入 node 环境
> module 
Module {
  id: '<repl>',
  exports: {},
  parent: undefined,
  filename: null,
  loaded: false,
  children: [],
  paths:
   [ /* node 定义的一些路径 */ ] }
>
```

## AMD
Asynchronous Module Definition 异步模块定义的意思, `define` 和 `require` 这两个定义模块、调用模块的方法，合称为 AMD 模式

[异步加载依赖模块](https://github.com/amdjs/amdjs-api/wiki/AMD)

AMD 也采用require([module], callback)语句加载模块，但是不同于CommonJS，它要求两个参数
第一个参数[module]，是一个数组，里面的成员就是要加载的模块
第二个参数callback，则是加载成功之后的回调函数

主要有两个 Javascript 库实现了 AMD 规范：`require.js` 和 `curl.js`,require([modules], callbacks([modules])),[modules] 表示所依赖的模块

```js
// 浏览器中引入 require.js 并定义好 data-main 入口
const ulr = 'localhost:8080'
require(['require.config'], function ( config ) {
  require(['axios'], function _axios( axios) {
    axios.get(url).then( function( res ){
      console.log(res)
    })
  })
})
```

AMD 就只有二个接口，一个定义，一个加载,而定义 define(id, dependencies, factory)
- id 模块名，省略则为文件名
- dependencies 依赖数组，就是该模块需要的依赖
- factory 工厂方法，为模块初始化要执行的函数或对象

## 源生规范


jQuery 中不仅兼容了 CommonJS 还兼容了 AMD 规范，并且还检查了 CMD 的规范
而这三个规范的兼容，则就是 jQuery 最后的一个框架，同时兼容

### CommonJS

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

### AMD

AMD 也有两个全局变量， require, define 但 与 CommonJS 都有 require 所以检测 define
公有属性 define.amd
提供对外接口
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

### CMD
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

### UMD 通用规范
```js
// 利用 UMD 规范实现一个 aQuery
// root   代表当前环境的全局变量是什么
// factory  模块的工厂方法，用于返回一个编写的模块
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

关于 es6 的一些特性可参考 [传送门](https://github.com/qlover/Notes/blob/master/es6.md)

## TypeScript 

一个 JavaScript 的超集,始于JavaScript，归于JavaScript, TypeScript可以编译出纯净、 简洁的JavaScript代码，并且可以运行在任何浏览器上、Node.js环境中和任何支持ECMAScript 3（或更高版本）的JavaScript引擎中

TypeScript 为 JavaScript 提供了一个非常强大的能力, *可选类型*



# JavaScript 基础(客户端请求,跨域(core,jsonp...),缓存)

## 前端 HTTP 请求方式

+ 第一代原生方式 xhr
+ ES6 新增第二代原生方式 fetch
+ 第三方
  - axios.js 对第一代原生方式的封装
  - vue-resource vue 插件
  - rxjs 另一种响应式的处理分发和流程操作类库

## 前端 HTTP 请求方式--axios

Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中, axios 的特点：

- 从浏览器中创建 XMLHttpRequests
- 从 node.js 创建 http 请求
- 支持 Promise API
- 拦截请求和响应
- 转换请求数据和响应数据
- 能够作到 abort, 并且能够自定义处理请求
- 自动转换 JSON 数据
- 客户端支持防御 XSRF

从 [axios](https://cdn.bootcss.com/axios/0.19.0-beta.1/axios.js) 源码入手

```
{ [Function: wrap]
  request: [Function: wrap],
  getUri: [Function: wrap], // 别名函数, 发送 getUri 请求
  delete: [Function: wrap], // 别名函数, 发送 delete 请求
  get: [Function: wrap], // 别名函数, 发送 get 请求
  head: [Function: wrap], // 别名函数, 发送 head 请求
  options: [Function: wrap], // 别名函数, 发送 options 请求
  post: [Function: wrap], // 别名函数, 发送 post 请求
  put: [Function: wrap], // 别名函数, 发送 put 请求
  patch: [Function: wrap],
  defaults:
   { adapter: [Function: httpAdapter],
     transformRequest: [ [Function: transformRequest] ],
     transformResponse: [ [Function: transformResponse] ],
     timeout: 0,
     xsrfCookieName: 'XSRF-TOKEN',
     xsrfHeaderName: 'X-XSRF-TOKEN',
     maxContentLength: -1,
     validateStatus: [Function: validateStatus],
     headers:
      { common: [Object],
        delete: {},
        get: {},
        head: {},
        post: [Object],
        put: [Object],
        patch: [Object] } },
  interceptors:
   { request: InterceptorManager { handlers: [] },
     response: InterceptorManager { handlers: [] } },
  Axios: [Function: Axios],
  create: [Function: create],
  Cancel: [Function: Cancel],
  CancelToken: { [Function: CancelToken] source: [Function: source] },
  isCancel: [Function: isCancel],
  all: [Function: all],
  spread: [Function: spread],
  default: [Circular] }
```
在 node 环境中打印出来的结果中可以看出，axios 暴露出来的一些属性和方法

### request

抛开 webpack 打包的代码，这里就直接从 494 行开始，`__webpack_require__(11)` 方法(846行),该方法内部声明配置了 axios 的一些默认配置

以下是 axios 主要依赖模块

```js
var utils = __webpack_require__(2); // 工具方法
var buildURL = __webpack_require__(6); // 地址构建器
var InterceptorManager = __webpack_require__(7); // 拦截器
var dispatchRequest = __webpack_require__(8); // 请求调度器
var mergeConfig = __webpack_require__(22); // 合并配置
```

### InterceptorManager 拦截器

每实例化一次 axios 时默认总会给当前实例增加一个 `defaults` 属性 和 `interceptors`对象,前者表示该实例的默认配置,后者则表示一个拦截器, InterceptorManager 内部用一个 `handlers` 栈维护,每个拦截器都会有 fulfilled 和 rejected 两个方法,

https://zhuanlan.zhihu.com/p/25383159

#### 连接拦截器

```js
if (typeof config === 'string') {
  config = arguments[1] || {};
  config.url = arguments[0];
} else {
  config = config || {};
}
```
该方法是整个 axios 请求入口,方法的开始和 jQuery.prototype.ajax 一样都是对参数作处理,也就是说可以直接传入 url 地址, 或者传入 url,再传入一个配置项对象

源码 537 行
```js
var chain = [dispatchRequest, undefined]; // Line:537
var promise = Promise.resolve(config); // Line:538
this.interceptors.request.forEach(function unshiftRequestInterceptors(interceptor) {
  chain.unshift(interceptor.fulfilled, interceptor.rejected);
});

this.interceptors.response.forEach(function pushResponseInterceptors(interceptor) {
  chain.push(interceptor.fulfilled, interceptor.rejected);
});

while (chain.length) {
  promise = promise.then(chain.shift(), chain.shift()); // Line:549 
}

return promise;
```

538行 由 ES6 的 Promise 实现可得出, Promise.resolve(config) 是为了将 config 创建成一个 promise 对象, 而 537 行创建了一个数组包裹的请求调度器,将注册在拦截器中的所有 fulfilled 和 rejected 方法依次放入请求调度器和 undefined 占位前后

特别注意第 537 行, 因为 promise 在 538 行时，状态已经为 resolved 了，也就是说已经完成了，也就会从头触发到尾巴，但是中途会发现 shift() 方法会经过 `dispatchRequest, undefined` 这两个占位符，当然此时的 then 永远会 reslove，永远不会 reject,因为 rejected 是一个 undefined，这也是为什么会出现 undefined 占位符原因

此处后面的 response 响应的拦截器也会永远的被执行,但再那之前,会经过请求调度器,所以目转调度器

补充一段实例,该实例是在浏览器环境中,当前保证能够成功响应时
```js
const url = 'axios-test-json.json'
axios.interceptors.request.use( function rs1(config){
  console.log('request 1 success')
  return config
}, function re1 (error){
  console.log('response 1 error', error)
  return Promise.reject(error)
})
axios.interceptors.response.use(  function rs2(config){
  console.log('request 2 success')
  return config
}, function re2 (error){
  console.log('response 2 error', error)
  return Promise.reject(error)
})
axios.request(url).then( r => console.log('response'))
// request 1 success
// request 2 success
// response
```

特别注意 Promise.resolve(config) 是将 config 包装成一个 promise 对象, ES6 的该方法实现有介绍, config 在默认情况下是不存在 then 方法的,也就是说 config 是一个不带 then 方法的对象,所以*返回的 Promise 对象状态为 fulfilled,并且将该value传递给对应的then方法*,这里在的 then 方法就是 549 行的 then,因为当前的状态直接为 fulfilled, 所以不仅将请求之前需要执行的拦截器的成功回调全部依次执行

这也是为什么 rs1 和 rs2 两个方法都被直接 fulfilled 执行了

如果当响应不成功时, 在响应的拦截器会将会依次执行，特别注意!!!


### 请求处理器  adapter

接下来就是源码第 779 行,利用 adapter 请求处理器,对 config 内容进行 promise 操作,取消操作的地方都用了 `throwIfCancellationRequested` 阻止当次操作

`__webpack_require__(13)` 创建默认 config 的方法，来到源码 970 行

```js
module.exports = function xhrAdapter(config) {
  return new Promise(function dispatchXhrRequest(resolve, reject) {
    request.onreadystatechange = function handleLoad() {
      settle(resolve, reject, response)
      request = null
    }
    request.onabort = function handleAbort() {}
    request.onerror = function handleError() {}
    request.ontimeout = function handleTimeout() {}
  })
}
```

该方法是 defaluts.adapter 的源头。 axios 是可以支持 node 和浏览器环境的，虽然是对底层做了封装，但是对于过老 IE 这样的浏览器是不支持 `XMLHttpRequest` 对象的，也就不支持过老的版本

1. 浏览器环境中使用浏览器设定默认的 `Content-Type` 头部字段
2. 默认可携带 HTTP 验证并 bota 转码
3. 可配置超时时间，默认不超时
4. 取消，错误和超时分别用原生 onabort，onerror和ontimeout 事件监听
5. 响应状态 `[200, 300)` 区间视为成功

*此处的响应完全是原生的响应内容*,需要返回给请求调度器对响应进行转换才是最后请求成功后的样子

### 请求调度器 dispatchRequest

直接来到源码的 `__webpack_require__(8)` 方法(717行),该方法中引入的更多的模块

```js
var utils = __webpack_require__(2);
var transformData = __webpack_require__(9); // 请求响应转换工具
var isCancel = __webpack_require__(10);  // 取消操作时付加对象,有个内部属性 __CANCEL__
var defaults = __webpack_require__(11); // 默认配置
var isAbsoluteURL = __webpack_require__(20); // URL 判断工具
var combineURLs = __webpack_require__(21); // 组全 URL
```
`isAbsoluteURL` 判断 URL 是否是一个绝对路径,遵守 [`RFC 3986`](http://www.cnpaf.net/Class/Rfcen/200610/16779.html) 编码规范方案

```js
function throwIfCancellationRequested(config) {
  if (config.cancelToken) {
    config.cancelToken.throwIfRequested();
  }
}
```

该方法简短,也没什么其它操作,但该方法在源码其它位置往往都在主方法体内的第一行,如注释其就是为了判断该实例是否取消了请求,而这个标识就是`config.cancelToken`

源码 777 行:
```js
var adapter = config.adapter || defaults.adapter;
return adapter(config).then(function onAdapterResolution(response) {
  // 成功时对此次请求进行取消检查
  throwIfCancellationRequested(config);
  // Transform response data
  response.data = transformData(
    response.data,
    response.headers,
    config.transformResponse
  );
  return response;
}, function onAdapterRejection(reason) {
  if (!isCancel(reason)) {
    // 失败时对此次请求进行取消检查
    throwIfCancellationRequested(config);
    // Transform response data
    if (reason && reason.response) {
      reason.response.data = transformData(
        reason.response.data,
        reason.response.headers,
        config.transformResponse
      );
    }
  }
  return Promise.reject(reason);
});
```
config.adapter 就是用户自定义请求的来源,在 axios 内部,默认每个实例都会有一个请求处理方法 `defaults.adapter`, 但值得注意的是该方法需要返回一个 Promise 来处理后续操作并且还得提供一个有效的响应, axios 官方上解释,在当前的请求前后会分别执行转换和拦截,转换则是转换请求或响应,拦截则是拦截器,详情可见[例子](https://github.com/axios/axios/tree/master/lib/adapters)

最后无论成功失败,将响应转换成 axios 独特格式 ☺

其实这个转换默认只是判断是否为字符串,如果是字符串则直接  `JSON.parse()` 否则啥也做,当然,这个转换规则可以是多个,默认是只有一个的,最后回到原点, 源码 549 行，将最后的 promise 返回给用户

*响应以用请求的转换可以具体参数 `config.transformResponse` 配置*

个人认为在此源码 549 行是整个 axios 的核心，因为它的奇妙设计，利用栈队列这个样的数据结构，完美的实现了请求拦截器,请求处理和响应拦截器之间的次序，很直观的对机器表达出了自己想要做的事，个人很佩服这一点。


## 跨域解决方案

当请求的目标地址和当前网站地址的 URL 端口不一样，或域名，或协议一样，满足其中任何一个的请求就会触发浏览器的同源策略限制，也就是不让你访问，完美的跨域解决是前后两端共同商讨决策

以下是可通过跨域访问的几种方案:

1. 通过 jsonp 跨域
2. document.domain + iframe跨域
3. location.hash + iframe
4. window.name + iframe跨域
5. postMessage 跨域
6. HTTP 访问控制(CORS)
7. nginx 代理跨域
8. nodejs 中间件代理跨域
9. WebSocket 协议跨域

### HTTP 访问控制

req 发送 origin 字段, res 响应`Access-Control-Allow-Origin`,如果 origin 来源在 Access-Control-Allow-Origin 中则是达成 CORS,这也是简单请求完成的最简单的访问控制,使用该方案应注意下几点:
1. 如果是简单请求,在之前的 HTTP 访问控制有说明,什么是简单请求和预检请求,如果该次跨域只个简单请求则会直接发送跨域请求,后端的`Access-Control-Allow-Origin`字段可以是`*`
2. 如果不是简单请求,那么每一次非简单请求都增加一个 option 方式的预检请求消耗,特别注意
3. 如果需要共享资源(因为简单的跨域请求是不会共享资源的,也就是 cookie,session 等会话或存储) 那一定注意,后端的 HTTP 控制头部字段必须支持`Access-Control-Allow-Origin`不能在是`*`应该具体到某一个访问域下面
4. 如果要携带 cookie, 后端同样要允许`Access-Control-Allow-Credentials`字段,虽然可以资源共享了,但那也是后端可以支持,并不代表前后两端都能接收和发送,所以前端还必须将 `Credentials` 请求字段设置为 `true`

*注意 application/json 或者是 application/xml 已经不满足简单请求,所以该 content-type 就应该需要预检*


下面来构建一次简单请求和需要预检请求的环境

首先由于 Chrome 的 `Provisional headers are shown` 临时响应头问题所以客户端运行在FF `http://local.notetest.com:81` 浏览器环境

服务器用 node http 模块 `http://localhost:8080` 起一个服务器(个从觉得简单快捷些)，关于 node 可详情可见[https://nodejs.org/dist/latest-v8.x/docs/api/http.html](https://nodejs.org/dist/latest-v8.x/docs/api/http.html)

#### 简单请求

客户端请求:
```js
// http://local.notetest.com:81/index.html
const url = 'http://localhost:8080'
// 浏览器使用原生二代 fetch 请求
fetch(new Request(url)).then( r => console.log(r))
```
node 服务器端：
```js
const http = require('http')
http.createServer( (request, response) => {
  const host = request.headers.origin // 请求地址
  // 打印源地址和请求方法
  console.log(`Host: ${host} Method: ${request.method}`) 
  response.writeHead(200, {
    'Content-Type': 'text/plain', // 简单请求普通文本
    'Access-Control-Allow-Origin' : '*', // 允许所有源地址跨域访问
    'Set-Cookie': 'money=1000' // 默认始终设置一个 cookie 
  })
  response.end('{ "name" : "qlover", "age" : 21 }')
}).listen(8080)
```

浏览器中直接访问, 服务器会在命令行中打印出 `Host: http://local.notetest.com:81 Method: GET`, 该次请求属于简单请求，请求和响应的头部信息如下
```
# 请求
请求网址:http://localhost:8080/
请求方法:GET
远程地址:127.0.0.1:8080
版本:HTTP/1.1
Referrer 政策:no-referrer-when-downgrade

Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:70.0) Gecko/20100101 Firefox/70.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Referer: http://local.notetest.com:81/
Origin: http://local.notetest.com:81
Connection: keep-alive
Cache-Control: max-age=0

# 响应
HTTP/1.1 200 OK
Content-Type: text/plain
Access-Control-Allow-Origin: *
Set-Cookie: money=1000
Date: Sun, 22 Dec 2019 09:15:08 GMT
Connection: keep-alive
Transfer-Encoding: chunked

# 服务器打印信息
Host: http://local.notetest.com:81 Method: GET
```

上述的简单请求使用 fetch 直接成功，后台设置`Access-Control-Allow-Origin`字段为`*`,简单请求直接通过，请求 content-type 指定为 json 格式返回,这样就不满足简单请求

```js
fetch(new Request(url, {
  mode: 'cors',
  headers: {
    'Content-Type': 'application/json'
  }
})).then( r => console.log(r))
```
node 服务器打印结果为 `Host: http://local.notetest.com:81 Method: OPTIONS`, 说明这个时间已经不再是简单,进行了预检请求,这个时的请求会在前台控制台打印抛出错误
```
Access to XMLHttpRequest at 'http://localhost:8080/' from origin 'http://local.notetest.com:81' has been blocked by CORS policy: Request header field content-type is not allowed by Access-Control-Allow-Headers in preflight response.
```
这个时候的服务器端并未允许 content-type 字段,并且响应数据还只是 text/plain 所以会报错,接下来 node 允许`Content-Type`字段并改变服务器的响应内容为 json

```js
http.createServer( (request, response) => {
  const host = request.headers.origin // 请求地址
  // 打印源地址和请求方法
  console.log(`Host: ${host} Method: ${request.method}`) 
  response.writeHead(200, {
    'Content-Type': 'application/json', // 返回 json
    'Access-Control-Allow-Origin' : '*', // 允许所有源地址跨域访问
    'Access-Control-Allow-Headers' : 'Content-Type', // 允许请求通过 Content-Type 字段
    'Set-Cookie': 'money=1000' // 默认始终设置一个 cookie 
  })
  response.end('{ "name" : "qlover", "age" : 21 }')
}).listen(8080)
```

此时会先以 OPTION 方式进行预检请求,并且请求响应头部如下:
```
# 请求
请求网址:http://localhost:8080/
请求方法:OPTIONS  # 请求方法为 options
远程地址:127.0.0.1:8080
版本:HTTP/1.1
Referrer 政策:no-referrer-when-downgrade

Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:70.0) Gecko/20100101 Firefox/70.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Access-Control-Request-Method: GET
Access-Control-Request-Headers: content-type
Referer: http://local.notetest.com:81/
Origin: http://local.notetest.com:81
Connection: keep-alive
Cache-Control: max-age=0


# 响应
HTTP/1.1 200 OK
Content-Type: application/json
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: Content-Type
Set-Cookie: money=1000
Date: Sun, 22 Dec 2019 09:12:16 GMT
Connection: keep-alive
Transfer-Encoding: chunked

# 服务器打印结果
Host: http://local.notetest.com:81 Method: OPTIONS
```
当预检请求通过,之后的 post 请求就会当作实际的请求发送出去,这个时候的请求和响应头信息如下
```
#请求
请求网址:http://localhost:8080/
请求方法:GET
远程地址:127.0.0.1:8080
版本:HTTP/1.1
Referrer 政策:no-referrer-when-downgrade

Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:70.0) Gecko/20100101 Firefox/70.0
Accept: application/json, text/plain, */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: application/json;charset=utf-8
Content-Length: 35
Origin: http://local.notetest.com:81
Connection: keep-alive
Referer: http://local.notetest.com:81/

# 响应
HTTP/1.1 200 OK
Content-Type: application/json
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: Content-Type
Set-Cookie: money=1000
Date: Sun, 22 Dec 2019 08:37:57 GMT
Connection: keep-alive
Transfer-Encoding: chunked

# 实际请求发送后服务器打印结果
Host: http://local.notetest.com:81 Method: OPTIONS
Host: http://local.notetest.com:81 Method: POST
```

上述完成了一次非简单请求，仔细的话会发现，每次 node 都在响应付加了 cookie ，前端的 cookie 却一致获取不到，并且每一次的请求头都没有 cookie 信息，这是因为前后都没有允许跨域通过响应内容，接下来利用`Access-Control-Allow-Credentials`字段允许前后端通过响应内容,并且要注意 origin 只能是具体的源地址

```js
fetch(new Request(url, {
  mode: 'cors',
  headers: {
    'Content-Type': 'application/json'
  },
  credentials : 'include'
})).then( r => console.log(r))
```

```js
http.createServer( (request, response) => {
  const host = request.headers.origin // 请求地址
  // 打印源地址和请求方法
  console.log(`Host: ${host} Method: ${request.method}`) 
  response.writeHead(200, {
    'Content-Type': 'application/json', // 简单请求普通文本
    'Access-Control-Allow-Credentials': true, // 允许跨域通过 cookie 
    'Access-Control-Allow-Origin' : host, // 只能是具体源地址
    'Access-Control-Allow-Headers' : 'Content-Type',
    'Set-Cookie': 'money=1000' // 默认始终设置一个 cookie 
  })
  response.end('{ "name" : "qlover", "age" : 21 }')
}).listen(8080)
```

预检查请求的请求和响应如下：
```
# 请求
请求网址:http://localhost:8080/
请求方法:OPTIONS
远程地址:127.0.0.1:8080
版本:HTTP/1.1
Referrer 政策:no-referrer-when-downgrade

Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:70.0) Gecko/20100101 Firefox/70.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Access-Control-Request-Method: GET
Access-Control-Request-Headers: content-type
Referer: http://local.notetest.com:81/
Origin: http://local.notetest.com:81
Connection: keep-alive
Cache-Control: max-age=0

# 响应
HTTP/1.1 200 OK
Content-Type: application/json
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: http://local.notetest.com:81
Access-Control-Allow-Headers: Content-Type
Set-Cookie: money=1000; path=/ # 服务器返回 cookie
Date: Sun, 22 Dec 2019 09:04:18 GMT
Connection: keep-alive
Transfer-Encoding: chunked

# 服务器打印
Host: http://local.notetest.com:81 Method: OPTIONS
```

实际请求:

```
# 请求

请求网址:http://localhost:8080/
请求方法:GET
远程地址:127.0.0.1:8080
版本:HTTP/1.1
Referrer 政策:no-referrer-when-downgrade


Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:70.0) Gecko/20100101 Firefox/70.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Referer: http://local.notetest.com:81/
Content-Type: application/json
Origin: http://local.notetest.com:81
Connection: keep-alive
Cookie: money=1000 # !!! 第二次实际请求已经将上一次的预检请求携带上了
Cache-Control: max-age=0

# 响应
HTTP/1.1 200 OK
Content-Type: application/json
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: http://local.notetest.com:81
Access-Control-Allow-Headers: Content-Type, Set-Cookie, *
Set-Cookie: money=1000; path=/ # 服务器同样返回 cookie
Date: Sun, 22 Dec 2019 09:06:16 GMT
Connection: keep-alive
Transfer-Encoding: chunked

# 服务器打印
Host: http://local.notetest.com:81 Method: OPTIONS
Host: http://local.notetest.com:81 Method: GET
```

以上就是跨域时会遇见的三种情况


# JavaScript 进阶 范式的转变 函数式

函数式编程是一种编程范式-一种构建计算机程序的结构和元素的方式-将计算视为对数学函数的评估，并且避免了状态和可变数据的更改

[来自](https://en.wikipedia.org/wiki/Functional_programming)

它既是从特定角度去看待问题的思维,又是实现思维的配套工具,现代编程语言常常是多范式,支持多样化的编程范式,如面向对象、元编程、函数式、过程式等等。

1. 引用透明,值或状态的不可变性,无副作用
2. 变换,记忆、映射、筛选、化约/折叠

## 范式的转变

> 编程范型、编程范式或程序设计法（英语：Programming paradigm），（范即模范、典范之意，范式即模式、方法），是一类典型的编程风格，是指从事软件工程的一类典型的风格（可以对照方法学）。如：函数式编程、程序编程、面向对象编程、指令式编程等等为不同的编程范型。

如熟悉的面向对象,面向过程什么的这就是范式,当然范也不止局限与程序上我们所熟知的面向对象，面向过程这类似编程思想,更多的应该是对程序执行的看法,比如面向对象思想中程序是一系列相互作用的对象，而在函数式思想中一个程序会被看作是一个无状态的函数计算的序列

[来自百度百科](https://baike.baidu.com/item/%E7%BC%96%E7%A8%8B%E8%8C%83%E5%9E%8B?fromtitle=%E7%BC%96%E7%A8%8B%E8%8C%83%E5%BC%8F&fromid=23696164)

下面以一个字符串数组为例子,取出数组中字符串长度大于3,并将每个元素首字母大写,并以逗号分隔为字符串

```js
const capitalize = str => str.replace(str[0], str[0].toUpperCase())
let names = ['jan', 'Tyrande', 'illidan', 'maiev', 'Dk']
```

命令式:

```js
let result = []
for(let i = 0, len = names.length; i < len; ++i){
  if( names[i].length > 3 ){
    result.push(capitalize(names[i]))
  }
}
result = result.reduce( (x, y) => x + ',' + y )
console.log(result)
// => Tyrande,Illidan,Maiev
```

函数式:

```js
console.log( names.filter(s => s.length > 3)
  .map(capitalize)
  .reduce( (x, y) => x + ',' + y )
)
// join(',')
// => Tyrande,Illidan,Maiev
```

在这样的问题上我个人更喜欢函数式的风格


## 引用透明 纯函数(Pure functions)

如果给定相同的参数，它将返回相同的结果，并且不会引起任何明显的副作用,这个副作用就是不会修改全局对象或通过引用传递参数的变量，也就是每一个函数，每一个功能都是独立的，不会影响到程序其它任何一处地方

首先借作者[TK](https://www.freecodecamp.org/news/functional-programming-principles-in-javascript-1b8fc6c3563f/)的一个例子:

```js
let PI = 3.14
const calculateArea = radius => radius * radius * PI
console.log(calculateArea(10)) //=> 314
```

这个方法可以接收一个半径作为参数,返回为这个半径的圆的面积,但是如果当变量 PI 的值被改变,那们就违背了,给定相同的参数相同的结果的这一定义,因为一个纯函数是不会修改全局对象或通过引用传递参数的变量,下面是另一个例子，如何将上面方法变成一个纯函数:

```js
let PI = 3.14
const calculateArea = (radius, pi) => radius * radius * pi
console.log(calculateArea(10, PI)) //=> 314
```

即使将 PI 值改变和 radius 改变,方法 aclculateArea 返回值与变量PI没有任何关系,有关的而是参数 raduis 和 pi

也正是因为纯函数这种结构在函数式中就显得十分重要,函数式的引用透明和变换都不能离开纯,如果一个函数不纯,比如这个时候将该函数“记忆”起来,函数的内部函数的外部状态都可以被改变,就不能作为记忆来使用,记忆后是不能改变任何状态的，应该是无副作用，显然纯函数显得十分重要


## 引用透明 函数式的数据结构

“异常”违背了大多数函数式语言所遵循的一些前提条件。函数式偏好没有副作用的纯函数,抛出异常的行为本身就是一种副作用，会导致程序进入异常流程。函数式语言以操作值为其根本，因此喜欢在返回值里表明错误并作出响应，这样就不需要打断程序的一般流程了


### 函数式的错误处理

出现异常时的返回值的限制是首先会遇阻碍,语言规定了方法只能返回一个值，当然可以把多个值放进一个对象里面或数组里一起返回

利用对象返回多个值:
```js
const divide = (x, y) => y == 0 ?
  {'exception' : new Error('被零除')} : {'answer': x / y}

console.assert( 2 == divide(4, 2) )
// Assertion failed
console.assert( 2 == divide(4, 2).answer )
// 什么都没有
console.assert( 2 == divide(4, 0).answer )
// Assertion failed
```

当断言失败时并不知道发生了什么，也就是有什么错误类型,当成功也不知道是否成功,还需要看是否包含了某个键，所以用键值对返回多个值有以下三个缺点:

1. 键没有安全性可能会随时改变,也就是不能捕获到类型方面的错误(将键变成指定类型可有限的处理)
2. 方法的调用者无法直接得知执行是否成功，还需要用键进行对比
3. 没有办法强制结果只包含一个键值对

### Either 数据结构

Either 设计规定它要么有左值要么有右值,但绝对不会同时存在两者。这种数据结构也叫不相交联合体(disjoint union),错误处理是 Either 的主要用途之一

*函数式习惯将异常置于左值,正常结果置于右值*

但通常的对于此类数据结构,如果数据类型不能明确,键不能明确或者是状态的未知。比如 Scala 语言天生属于函数式 Either 也是得到了实现,再比如 Java 虽然天生不属于函数式但有泛型可以选择性的实现 Either

JavaScript 天生也是一个多范式语言,也因为 JavaScript 的鸭子类型众多在该方面上也显得有不足

下面则利用 ES10 的特性稍严谨的实现一个 Either 类的例子:

```js
class Either {
  // ES10 # 私有
  #left = null
  #right = null

  // !!!此处应该为私有的构造器，访问外界的修改
  // ES10 还未实现私有的构造器
  // 这里模拟一次私有的构造器
  constructor(left, right){
    this.#left = left
    this.#right = right
  }

  getLeft(){ return this.#left }
  getRight(){ return this.#right }
  isLeft(){ return !!this.#left }
  isRight(){ return !!this.#right }
  static setLeft(left){
    return new Either(left, null)
  }
  static setRight(right){
    return new Either(null, right)
  }

}

const divide = (x, y) => y == 0 ?
  Either.setLeft(new Error('除数不能为0')) :
    Either.setRight( x / y)
    
const result = divide(4, 2)
console.assert( 2 == result.getRight() )
// 什么都没有
console.assert( 200 == result.getRight() )
// Assertion failed
```

不再需要寻找键而判断执行是否成功,直接利用 either 类的特点(正常结果为右值),并且一旦内部方法设定好后其 left right 记录值不会在改变,也做到了“键”的唯一,无论异常还是正常结果都多了一次间接层,调用 get 方法,恰恰也是这间接层为实现`缓求值`提供了可靠的基础

*借用 JavaScript 超集 TypeScript 可以弥补 JavaScript 明确类型的不足,或是借助第三方库`Ramda.js`这个函数式库*

### Either 包装异常


## 列表变换的基本操作

### 筛选 与 映射

筛选(filter)根据用户定义的条件筛选列表中的条目,并产生一个较小的新列表

映射(map)操作对原集合的每一个元素执行给定的函数,从而变换成一个新的集合,

ES5 提供的两个支持函数式的方法 Array.prototype.filter 和 Array.prototype.map 分别就对应得筛选和映射

### 折叠(fold)\化约(reduce)

函数式的语言中 foldLeft 和 reduce 方法操作在功能上大致生命,但根据具体的编程语言而有微妙的区别

两者都用一个累加器(accumulator)来收集每一次处理的集合,reduce 一般可指定一个初始的累积器,而 flod 初始累积器始终为空,并且 fold 或 recude 都应该是外排方法不应该改变原集合,折叠运算或化约运算常常用在需要从一个集合处理产生另一个大小不同的集合或单一值的情况

最简单明了的一个例子就是利用折叠运算,也就是使用 reduce 这样的方法求一个数组元素的和

*需要把集合分成一小块一小块来处理的时候就可以折叠或化约*


## 变换 函数的部分施用

使一个多参数函数得以省略部分参数，从而转化为一个参数数目较少的函数，也就是让函数先作用其中一些参数

*柯里化可以算作是部分施用的一个特殊例子*

柯里化和函数的部分施用都是从数学里借用过来的编程方法,这两种方法以不同的面目出现在各种类型的语言里,在函数式语言中尤其普遍,他们都有能力处理操纵方法的参数数目

虽然看起来柯里化和部分施用效果一样,但是这两种方法区分很重要,而且很容易被错误的理解,虽然有时候这两种方法的返回结果一致,但两种方法裁然不同

*与这两个容易搞混的还有一个叫偏函数*

### 部分施用(Partial application)

下面借作者 Neal Ford 在《函数式编程思维》里的一个例子看什么是部分施用,作者使用 Scala,这里则使用 JavaScript 变种实现:

```js
const price = product => (({
  'apple': 140,
  'orange': 223,
})[product])

const withTax = (cost, country) => (({
  'Chain': cost * 2,
  'USA': cost * 3,
})[country])

console.log( withTax( price('apple'), 'Chain' ) )
// => 280
console.log( withTax( price('orange'), 'USA' ) )
// => 669

const ChainTaxed = cost => withTax(cost, 'Chain')
const USATaxed = cost => withTax(cost, 'USA')

console.log( ChainTaxed(price('apple')) ) //=> 280
console.log( USATaxed(price('orange')) ) //=> 669
```

price 是一个使用映射获取水果单价的方法, withTax 是一个计算不同地区的水果单价和税后的方法(这里就将州变成国家),首先直接调用 withTax 传入水果单价和国家,结果是预期的结果,但每一次调用 withTax 都要带上相同的国家就显得累赘,这时可以对国家参数做部分施用,得到一个固定了国家参数值的函数,这样处理后,像 ChainTaxed 方法只需要传入水果单价就可以了,这看上去显然和柯里化没有什么差别,但如果提前做两个参数或更多的参数,这就和柯里化有小小的区别了

下面再借作者潘俊的《JavaScript函数式编程思想》中的部分应用的一个例子,其获取一个指定范围内的数字序列如下:

```js
// [start, end) 区间范围
const rangeRoutine = (step, start, end) => {
  let seq = [],
      index = start - 1
  while( ++index < end && index * step < end &&
    seq.push( index * step) ){}
  return seq
}
console.log( rangeRoutine(1, 0, 6) )
// =>[ 0, 1, 2, 3, 4, 5 ]
```

一个再简单不过的方法创建一 0 开始到 6 结束并且元素步长为 1 的数字序列

方法的默认参数是 ES6 提供的特性,可以方便的对方法进行调用,如果这个例子中,步长默认为1或开始默认为6,只需要告知方法的结束位置即可,下面则在 rangeRoutine 方法不改变形参位置基础之上给步长做一个默认参数调用 rangeRoutine

```js
const rangeRoutineByStep = step => 
  (start, end) => rangeRoutine(step, start, end)
const rangeRoutineByStep1 = rangeRoutineByStep(1)

console.log( rangeRoutineByStep1(0, 6) )
// => [ 0, 1, 2, 3, 4, 5 ]
```

*以上例子完全可以在 rangeRoutine 重构,这里只是为了更清晰一下再做什么事*

原来的 rangeRoutine 方法分成了两步调用,第一次记住了步长这个参数,第二次则是提供了开始与结束位置的参数,此时这种调用方式就叫部分施用(Partial Application),柯里化再此基础上将施用的参数只是变成了一个参数,也就是单一的参数,所以柯里化也是部分施用的一种特殊例子,但好像柯里化的名词更出名一点

### 柯里化(Currying)

使一个多参数函数变成一连串单参数的函数的变换，其描述的是变换过程，不涉及变换后对函数的调用，调用都可以决定对多少个参数实施变换，余下的部分将衍生为一个参数数目较少的新函数,首先下面是一个简单的例子:

```js
let list = [1,2,3,4,5,6]
const mod = x => y => !(y % x)

console.log( list.filter(mod(2)) )
// => [ 2, 4, 6 ]
console.log( list.filter(mod(3)) )
// => [ 3, 6 ]
```

这个例子没什么用,上面的 rangeRoutineByStep1 返回的函数可以在接收两个参数,这个行为叫部分施用,现在如果将序列开始位置和结束位置再进行一次部分施用,比如将开始位置默认变成0,下面再用 rangeRoutineByStep1 方法重新将参数再省略一个得到一个柯里化后的结果:

```js
const rangeRoutineFrom0 = end => rangeRoutineByStep1(0, end)
console.log( rangeRoutineFrom0(6) )
// => [ 0, 1, 2, 3, 4, 5 ]
```

结果依然是预期的结果,中间将开始位置和结束位置分开进行部分施用,那么就将原来 rangeRoutine 方法进行了三步调用,每一步都清晰每一步再做什么,当然上面这几个例子手动演示了如何实现一个参数部分施用的过程,在一些支持函数式的语言中都有专门做这件事的方法或语言特性


### 偏函数(Partial Function)

尽管在英文名称上与部分施用相似,但偏函数并不生成部分施用函数,它的真正用途是描述只对定义域中一部分取值或类型在意义的函数,其参数被限定了取值范围,比如数学上的 1/0 是无意义的

*偏函数在代码上来说就是对部分施用参数时增加了对参数范围限定的手段*

## 变换 记忆 缓存 

记忆指在函数级别上对需要多次使用的值进行缓存的机制,比如有一个反复调用的函数，每一次根据一组特定的参数求得结果之后,就用参数值做查找用的键,把结果缓存起来,以后当函数又遇到相同参数的时候,就不需要重新计算一遍了,直接返回缓存的结果,这样的做法是计算机科学一种典型的折衷方案

> 用更多的内存去换取长期来说更高的效率

只有纯函数才适用缓存技术,两种方法实现缓存:

1. 手动实现数组结构上的缓存
2. 自动了采用有性记忆的方法

### 缓存

直接的缓存属于手动性的对方法结果进行缓存,比如一个方法接收参数1返回2,那么就将这个方法的参数1和返回值2做缓存,下一次调用该方法时同样的参数1直接返回缓存的结果,不再进行复杂的计算,

假如说有一个方法执行了特别复杂的运算(这里为计算一个数是否是素数),如果这个参数是5:

1. 只缓存结果,只将5这个数结果缓存起来,下一次方法再遇到这个值就直接使用缓存
2. 缓存所有值,缓存从 1至5数字之间所有数的结果,也就是缓存所有可能的结果,不管传入什么值,直接取缓存

下面利用上述柯里化方法没有缓存的例子:

```js
// 借用上面 rangeRoutineByStep1 方法产生一个 [start, end) 区间的序列
// !!! 不包括传入的素数,因为判断素数就是从大于1小于本身数之间做判断
const rangeRoutineFrom2 = end => rangeRoutineByStep1(2, end)

// 遍历数列时取余数判断
// true 表示整除没有余数,y 不是 x 的因数
// false 表示有余数,y 是 x 的因数
const mod = x => y => !(x % y)

// 素数计算
// true 表示是素数
// false 表示不是素数
const prime = number =>
  !rangeRoutineFrom2(number).filter(mod(number)).length

console.assert( prime(11) )
// 什么都没有
console.assert( prime(10) )
// Assertion failed
```

其中不管是多少次参数11或10都会将上述所有的步骤都计算一次,如果当这个数是100，1000，首次虽然避免不了,但第二次,第三次...

下面使用一个映射表缓存结果:

```js
let cacheMap = new Map()
const prime = number => {
  if( !cacheMap.has(number) ){
    cacheMap.set(number, !rangeRoutineFrom2(number)
      .filter(mod(number)).length
    )
  }
  return cacheMap.get(number)
}

console.assert( prime(11) )
// 什么都没有
console.assert( prime(10) )
// Assertion failed
console.dir( cacheMap )
//=> Map { 11 => true, 10 => false }
```

以上缓存了结果,如果要缓存可能出现的任何值,那么对于这里求素数来说,首次执行的时候复杂度总会是O(^2),但一旦中间可能出现的所有值被缓存,第二次后可想该效率会提升多少



### 记忆

不再像缓存只针对结果进行缓存,而针对整个方法,一般来说就是将需要记忆的函数定义成闭包,然后对该闭包执行`memoize()`方法来获取一个新函数,以后每一次调用这个新函数的时候,其结果就会被缓存,有点像是在缓存的基础上加了一个"壳",且被该方法应该满足以下两个条件:

1. 纯,没有副作用
2. 不依赖任何外部信息

再不改变 prime 方法时,利用 lodash.memoize() 方法完成对 prime 方法的记忆:

```js
const prime = lodash.memoize(number =>
  !rangeRoutineFrom2(number).filter(mod(number)).length)

console.assert( prime(10) )
console.assert( prime(11) )
// => Assertion failed
console.log( prime.cache.__data__.hash.__data__ )
// => [Object: null prototype] { '10': false, '11': true }
```

与上述对结果的缓存一致,只是并未改变原方法的前提下直接将方法做记忆,这样也同时完成了方法的自动缓存



## 变换 缓求值(Lazy Evaluation)

缓求值指尽可能的推迟求解表达式,不会和缓存那样先将值预先算好,而是在需要使用的时候才落实下来,这样的好处有以下三点:

1. 复杂的计算只有到绝对必要使用的时候才执行
2. 可以建立一个无限大的集合,只要一直接到请求就一直送出元素
3. 使用映射、筛选等概念做高效求值


> 只能被1和它本身的整除的自然数叫素数或质数

还是以上述的素数为例子,这一次不判断元素是否是一个素数,而是通过一个素数查找到下一个素数,借用 Lodash.curry 方法直接对 rangeRoutine 柯里化，解决繁琐的手动对 rangeRoutine 柯里化,并且提供一个寻找下一个素数的迭代方法(迭代子) next() 方法,如下:

```js
// lodash.curry 直接对方法柯里化
const rangeRoutine = lodash.curry((step, start, end) => {
  let seq = [],
      index = start - 1
  while( ++index < end && index * step < end &&
    seq.push( index * step) ){}
  return seq
})
const mod = x => y => !(x % y)

// 将柯里进行了部分施用
const prime = number =>
  !rangeRoutine(1,2)(number).filter(mod(number)).length

// 默认最后一个素数为1
let last = 1
// 可迭代下一个素数的迭代子
const next = () => {
  const nextTo = number => prime(++number) ?
    last = number : nextTo(number)
  return nextTo(last)
}

console.log( next(), last ) //=> 2 2
console.log( next(), last ) //=> 3 3
console.log( next(), last ) //=> 5 5
console.log( next(), last ) //=> 7 7
// ...

```

将 last 变量和 next 方法看作是一个背后用于存储数据的集合,每当我们需要得到下一个素数时,会调用 next 计算出 last 后的第一个素数并修改它,当下次调用 next 才会计算,不会像 memoize 方法，每一次遍历的数列会提前将该结果进行缓存,调用 next 时没有加上限制也是为了符合素数在数学上的无穷性

*Lodash.curry 是对柯里化进行了增强,对柯里化也进行了部分施用*

### 缓求值列表

当产生的序列属于之前产生序列的子集时,这个时候就可以利用列表形式将该子集做缓求值,以头(head)和末尾(tail)组成的列表, head 中可以包含子集但可不完全包含,末尾可以是子集与后追加序列

下面先考虑一下一个简单的缓求值列表:

```js
function LazyList(list){
    // 每个惰性的值都应该为数组
  this.list = list
}

LazyList.init = function(){
  return new LazyList( function(){ return []} )
}

LazyList.prototype.head = function(){
  // 当需要的时候才会调整该函数
  return this.list()[0]
}

LazyList.prototype.cons = function(head){
  return new LazyList( function(){ return [head, this.list] } )
}

let ll = LazyList.init().cons(1).cons(2).cons(3)

// [ 1, [Function: [ 2, Function:[ 3, Function: []]]]]

console.log(ll.head())
// => 3
```

其思路为,当每创建一个 LazyList 对象时内部会有个属性 list 记录一个函数,该函数为当前 LazyList 对象应该返回的值,只不过该值是由一个闭包负责,只有正直需要使用该值的时候才会调用该函数,如何取得其它值,这个就需要用上折叠等手段

当然如何控制整个列表方式非常多,但核心的思想不会变,将值暂时停留在闭包中,当需要的时候再从该闭包得到值



## 变换 柯里化增强 参数记忆

> 在 JavaScript 中使用经典的柯里化的函数有一点不便： 当传递给一个函数的参数超过一项，以返回接收剩余参数的函数时，需要调用函数的次数等于传递的参数的数量，因为
柯里化的函数一次只能接收一个参数，也就是说在函数名后面会跟着超过一对括号

当方法需要柯里化使用时,手动将该方法写成柯里化形式和直接使用柯里化的方法对该方法进行柯里化明示效果要好得多,比如:

```js
const rangeRoutine = lodash.curry((step, start, end) => {
  let seq = [],
      index = start - 1
  while( ++index < end && index * step < end &&
    seq.push( index * step) ){}
  return seq
})

console.log( rangeRoutine(1)(0)(4) )
// => [ 0, 1, 2, 3 ]
console.log( rangeRoutine(2, 1)(10) )
// => [ 2, 4, 6, 8 ]
console.log( rangeRoutine(3, 0, 10) )
// => [ 0, 3, 6, 9 ]
```


### 部分施用的柯里化

当使用每一次使用柯里化后的方法时可以先预使用两个或多个参数将剩余参数传递给返回的下一个方法使用,这也就对柯里化进行了部分施用,具体处理时应注意以下两点:

1. 如果第一次使用柯里化后的方法时,传入参数和原方法参数个数一致则直接调用原方法
2. 如果不是等长个数参数,则柯里化方法内部使用数组维护接收的部分参数

```js
function curry(fn){
  // 被柯里化方法的形参长度
  let len = fn.length
  // 得到 curry 除开被柯里化函数外的所有参数当作初始部分参数
  // !!! 柯里化方法中 len 和 saveArgs 变量都会被驻留在内存
  let saveArgs = [].slice.call(arguments, 1)

  // 被记忆的方法 _curry
  return (function _curry(saveArgs){
    return function _curry_inner(){
      let curArgs = saveArgs.concat([].slice.call(arguments))
      return curArgs.length >= len ? 
        fn.apply(fn, curArgs) : _curry(curArgs)
    }
  }(saveArgs))
}

const rangeRoutine = curry( function range_inner(step, start, end){
  let seq = [],
      index = start - 1
  while( ++index < end && index * step < end &&
    seq.push( index * step) ){}
  return seq
})

console.log( rangeRoutine(1, 0)(5) )
// => [ 0, 1, 2, 3, 4 ]
console.log( rangeRoutine(2)(1)(10) )
// => [ 2, 4, 6, 8 ]
```

这里需要特别注意的一点就是当 range_inner 直接被柯里化时,内部的 saveArgs 只记忆了一份初始化给 curry 除 range_inner 外所有参数,如果当第二次调用 rangeRoutine 时因为之前驻留的 saveArgs 会是之前的, 一旦在`_curry`或`_curry_inner`中被修改记忆的值就会发生变化,所以采用以参数的形式传入

这里的核心思想主要是记忆参数为主,当传入的参数符合了原方法的参数个数就调用,否则会一直等待缓存参数个数与原方法形参个数相同

### 从右向左的柯里化

先提供三种最暴力的实现:

1. 将 curyy 在应用原方法的时候的参数数组直接 reverse 就可以了
2. 利用 Array.prototype.push 的特性,每次将当次传入的参数 push 记忆的参数
3. 利用 Array.prototype.unshfit 的特性,每次记忆的参数 unshift 进当前传入参数

下面分别提供 curryRight 部分源码:

```js
function curryRight(fn){
  let len = fn.length
  let saveArgs = [].slice.call(arguments, 1)
  return (function _curry(saveArgs){
    return function _curry_inner(){
      let curArgs = saveArgs.concat([].slice.call(arguments))
      return curArgs.length >= len ? 
        /* 将原应用参数倒排 */
        fn.apply(fn, curArgs.reverse()) : _curry(curArgs)
    }
  }(saveArgs))
}

function curryRight(fn){
  let len = fn.length
  let saveArgs = [].slice.call(arguments, 1)
  return (function _curry(saveArgs){
    return function _curry_inner(){
      let curArgs = saveArgs.concat([].slice.call(arguments))
      return curArgs.length >= len ? 
        /* 将原应用参数倒排 */
        fn.apply(fn, curArgs.reverse()) : _curry(curArgs)
    }
  }(saveArgs))
}

function curryRight(fn){
  let len = fn.length
  let saveArgs = [].slice.call(arguments, 1)
  return (function _curry(saveArgs){
    return function _curry_inner(){
      // 将当次的参数 push 记忆的参数
      let curArgs = [].slice.call(arguments)
      Array.prototype.push.apply(curArgs, saveArgs)

      return curArgs.length >= len ? 
        fn.apply(fn, curArgs.reverse()) : _curry(curArgs)
    }
  }(saveArgs))
}

function curryRight(fn){
  let len = fn.length
  let saveArgs = [].slice.call(arguments, 1)
  return (function _curry(saveArgs){
    return function _curry_inner(){
      // 将当记忆的参数 unshift 进当前参数
      let curArgs = [].slice.call(arguments)
      let temp = saveArgs
      Array.prototype.unshift.apply(temp, curArgs)
      curArgs = temp

      return curArgs.length >= len ? 
        fn.apply(fn, curArgs.reverse()) : _curry(curArgs)
    }
  }(saveArgs))
}
```

但是上面这三种最快速的方法同样是从左向右处理参数,只是在处理参数的过程中顺序跌倒,

### 改变位置的柯里化

实现一个可以使用“占位符”顺位替换预留参数的柯里化,以下是顺位替换参数的两个要点:

1. 每调用一次,记录当前占位符在原方法形参列表中的索引
2. 当接收的参数个数减去占位符累积个数大于等于原方法形参个数时,对最后的参数用记录占位符顺位替换

顺位替换不能出现占位符与参数一起出现:

```js
const _ = '_hlod_'

function curry(fn){
  let fnLen = fn.length
  let curryArguments = arguments
  let saveArgs = [].slice.call(curryArguments, 1)
  let holdList = []

  return (function _curry(saveArgs){
    return function _curry_inner(){
      let args = [].slice.call(arguments)
      let curArgs = saveArgs.concat(args)
      let curLen = curArgs.length
      let index = -1, len = args.length
      
      // 记录每一次参数中占位符出现的位置
      while( ++index < len ){
        args[ index ] === _ && holdList.push(curLen + index - 1)
      }

      // 当接收所有参数个数减去占位符个数为原方法形参个数时
      if( curLen - holdList.length >= fnLen ){
        
        // 遍历替换占位符
        index = curLen - holdList.length - 1
        while( ++index < curLen ){
          // 将超过原方法形参长度后的所有参数当作替换占位符的实参
          // shift() 从头依次替换
          curArgs[ holdList.shift() ] = curArgs[ index ]
        }
        curArgs.length = fnLen
        holdList.length = 0

        return fn.apply(fn, curArgs)
      }
      return _curry(curArgs)
    }
  }(saveArgs))
}

const rangeRoutine = curry((step, start, end) => {
  let seq = [],
      index = start - 1
  while( ++index < end && index * step < end &&
    seq.push( index * step) ){}
  return seq
})
console.log( rangeRoutine(1, 0)(5) )
// => [ 0, 1, 2, 3, 4 ]
console.log( rangeRoutine(_)(1)(_)(2)(10) )
// => [ 2, 4, 6, 8 ]
console.log( rangeRoutine(3)(_)(_)(0)(10) )
// => [ 0, 3, 6, 9 ]
console.log( rangeRoutine(_)(_)(_)(5)(1)(25) )
// => [ 5, 10, 15, 20 ]

console.log( rangeRoutine(4)(_)(1)(_)(17) )//=> []
console.log( rangeRoutine(4)(_, 1)(_)(17) )//=> []
```

如上例子,如果当参数中存在占位符时,用上面的例子就会参数不是所预期那样,下面是从 lodash 中借鉴而来柯里化和作者冴羽的例子:

```js
const lodash = require('../lib/lodash.4.17.15.js')
var fn = lodash.curry(function _fn(a, b, c, d, e) {
    console.log([a, b, c, d, e]);
});


fn(1, 2, 3, 4, 5);
fn(lodash, 2, 3, 4, 5)(1);
fn(1, lodash, 3, 4, 5)(2);
fn(1, lodash, 3)(lodash, 4)(2)(5);
fn(1, lodash, lodash, 4)(lodash, 3)(2)(5);
fn(lodash, 2)(lodash, lodash, 4)(1)(3)(5)
fn(lodash, lodash, lodash, 4)(1)(2)(3)(5)
// 以上结果均为 [ 1, 2, 3, 4, 5 ]

fn(1, lodash, lodash, 4)(lodash,lodash,lodash,lodash, 5)(2)(3)
// => [ 1, 2, 3, 4, '__lodash_placeholder__' ]
```

从该例子中这里就直接总结 lodash 中柯里化以下几点:

1. 同位的占位符前者替换后者,下一次接收的参数替换上一次接收参数的占位符开始
  - 如果都为占位符则保持不变
  - 如果都不为占位则后者追加到前者
  - 如果前者不为占位符则前者替换后者
  - 如果前者为占位符则顺位替换
2. 之后的顺补,当所接收的参数和超过原方法的形参个数那么这时会将超过部分依次对前面的占位符进行替换
3. 多余占位符不处理,当超过参数部分还有占位符那么不做处理

但最终结果所以应用的参数只能是原方法形参个数

#### 终极版柯里化

这里附上实现以下柯里化的几个要点:

1. 柯里化方法维护一个计数器作为跳出递归条件
  - 当计数器等于被柯里化方法后跳出递归
2. 每次调用将传入的参数追加到记的参数后
3. 利用改变位置的柯里化的规则每一次过滤占位符元素
  - 追加的参数会用该规则做过滤

从改变位置的柯里化例子中得到以下完善的柯里化实现:

```js
const _ = '_hlod_'
const eqHold = x => x === _
function curry(fn){
  let fnLen = fn.length
  // 记录内部有效参数个数
  let count = 0

  return (function _curry(saveArgs, holds){
    return function _curry_inner(){
      let useArgs = [],
          // 用来追加到记忆参数的副本
          // 会利用规则对其过滤
          args = [].slice.call(arguments),
          index = -1,
          argLen = args.length,
          length = saveArgs.length

      // 遍历传入参数与记忆的占位符索引数组同位比较
      while( ++index < argLen ){
        let h = holds[index]
        if( eqHold(arguments[ index ]) ){
          let t = length  + index
          // 向占位符索引数组增加当前在记忆的所有参数中的索引
          holds.push( t )

          // 1. 如果都为占位符则保持不变
          if( eqHold(saveArgs[ h ]) ){
            // 将副本参数头出队
            args.shift()
            // 因为后者也是占位符此时将之前的占位符从数组中删除
            holds.splice(length, 1)
          }
        } else {
          // 当每一次传入参数不是占位符则有效参数加1
          count++
          // 4. 前者为占位符后者不是则直接后者替换前者
          if( eqHold(saveArgs[ h ]) ){
            args.shift()
            holds.splice(index, 1)
            // 记忆的参数直接被当前参数替换
            saveArgs[ h ] = arguments[ index ]
          }
        }
      }

      // 2. 都不为占位符则直接追加副本参数到记忆参数后
      // 3. 前者不为占位符则前者替换后者，就相当于将当前的参数的头出队然后追加
      useArgs = saveArgs.concat(args)

      // 有效参数大于原方法形参个数时
      if( count >= fnLen ){
        // 重置有效参数个数和记忆的占位符索引数组
        count = holds.length = 0
        // 去掉多余的参数
        useArgs.length = fnLen
        return fn.apply(fn, useArgs)
      }
      return _curry(useArgs, holds)
    }
  }([].slice.call(arguments, 1), []))
}

const lodash = require('lodash')
const fn = curry( (a, b, c, d, e) => console.log([a, b, c, d, e]) )
const fn2 = lodash.curry( (a, b, c, d, e) => console.log([a, b, c, d, e]) )

fn(1, 2, 3, 4, 5);
fn(_, 2, 3, 4, 5)(1);
fn(1, _, 3, 4, 5)(2);
fn(1, _, 3)(_, 4)(2)(5);
fn(1, _, _, 4)(_, 3)(2)(5)
fn(_, 2)(_, _, 4)(1)(3)(5)
fn(_, _, _, 4)(1)(2)(3)(5)
// 以上均为 [ 1, 2, 3, 4, 5 ]

fn(_, _, _, 4)(1)(2)(3)(_,_,5)
fn2(lodash, lodash, lodash, 4)(1)(2)(3)(lodash,lodash,5)
// => [ 1, 2, 3, 4, lodash ]
```

该柯里化思想借鉴 [lodash.curry](https://lodash.com/docs/4.17.15#curry),例子参考作者[冴羽的博客 JavaScript专题之函数柯里化](https://github.com/mqyqingfeng/Blog/issues/42)


## 变换 运算符重载

也许会有这样的一个疑问, eq,gt,lt 等等这样很多语言都存在,为什么不直接使用 ==,>,< 这些运算符来得直接

可能两个数字可以直接使用 == 比较,也可以两个数组也可以直接使用 == 比较,但如果是两个抽象的类型或者是两个不能直接使用 == 比较的类型,但又可以用来判断是否相等

> 运算符重载，就是对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型

下面直接举个简单的例子,比如让两个对象相加:

```js
let o1 = { a: 1, b: 10, c: function(){ return 13 } }
let o2 = { a: 10, b: 20, c: function(){ return 14 } }

console.log( o1 + o2 )
// => [object Object][object Object]

Object.prototype.plus = function(other){
  let result = {},
    keys = Object.keys(this),
    len = keys.length,
    index = -1

  while( ++index < len){
    let key = keys[ index ]
    if( typeof this[ key ] === 'function' ){
      let value = this[ key ]() + other[ key ]()
      result[keys[index]] = function _v(){ return value }
    } else {
      result[keys[index]] = this[ key ] + other[ key ]
    }
  }
  return result
}

let o3 = o1.plus(o2)
console.log( o3 )
// =>{ a: 11, b: 30, c: [Function: _v] }
console.assert( 27 == o3.c() )

```

## 变换 复合(函数重用)

复合作为一种重用机制,在函数式语言中主要表现为通过参数来传递作为一等语言成分的函数,其重用机制建立在列表的概念,以及可以连同执行上下文一起传递的借到块的概念之上

当语言拥有的闭包特性就不需要命令模式了,设计模式的存在就是为了弥补语言功能上的弱点



### 工厂和抽象工厂处理if条件过多

## 模板模式 template
## 策略模式 Strategy
## 享元模式 flyweight

## 工厂模式和柯里化

函数式的工厂模式的返回一个专有函数,

```js
const divides = x => y => y % x == 0
console.log([2,3,4,5,9,7,6].filter(divides(2)))
// => [ 2, 4, 6 ]
console.log([2,3,4,5,9,7,6].filter(divides(3)))
// => [ 3, 9, 6 ]
```

# javascript 类库--- [Lodash](https://lodash.com/)

`A modern JavaScript utility library delivering modularity, performance & extras.`

是一个一致性、模块化、高性能的 JavaScript 实用工具库。

借网上的一个总结：
lodash 提供的函数主要分为以下几类:

- Array，适用于数组类型，比如填充数据、查找元素、数组分片等操作
- Collection，适用于数组和对象类型，部分适用于字符串，比如分组、查找、过滤等操作
- Function，适用于函数类型，比如节流、延迟、缓存、设置钩子等操作
- Lang，普遍适用于各种类型，常用于执行类型判断和类型转换
- Math，适用于数值类型，常用于执行数学运算
- Number，适用于生成随机数，比较数值与数值区间的关系
- Object，适用于对象类型，常用于对象的创建、扩展、类型转换、检索、集合等操作
- Seq，常用于创建链式调用，提高执行性能（惰性计算）
- String，适用于字符串类型

和之前 axios 一样下面就进入 [lodash 的源码](https://cdn.bootcss.com/lodash.js/4.17.15/lodash.js
)

### Core lodash 核心  



# 框架/库(jQuery-raw,Vue,React,Angluar)

# 前端(html+css+js,分离/不分离,自动化/脚手架(webpack,gulp,yo...),插件,SPA,SEO,性能测试,优化...)

# UI(字体,图片,移动端,排版,布局设计(响应式设计,尺寸设计))

# UX()

# 动画(数学,逻辑,计算机科学,AI,big-data,Colud-computer)

# 前后链接(动态加载,分离加载,模板引擎)

# 后台(数据库设计,表设计,冗余/多表,查询,PHP,Node.js,Java,Python)

# ...

# ionic/flutter/react native

# https://leohxj.gitbooks.io/front-end-database/content/index.html



Commit Message 格式
```
<type>(<scope>): <subject>
<空行>
<body>
<空行>
<footer>
```

# 参考链接
- [defer 和 async](https://segmentfault.com/q/1010000000640869)
- [深入浅出浏览器渲染原理](https://github.com/ljianshu/Blog/issues/51)
- [HTML5代码规范](https://www.runoob.com/html/html5-syntax.html)
- [CSS像素](https://www.imooc.com/article/75237)
- [MDN meta 标签](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta)
- [viewport 深入理解](https://www.runoob.com/w3cnote/viewport-deep-understanding.html)
- [像素（px）到底绝对单位还是相对单位](https://blog.csdn.net/lianfengzhidie/article/details/86663715)
- [像素,物理像素,逻辑像素](https://www.cnblogs.com/zaoa/p/8630393.html)
- [分辨率](https://www.cnblogs.com/yesyes/p/7638607.html)
- [CSS像素](https://www.imooc.com/article/75237)
- [移动前端开发之viewport的深入理解](https://www.cnblogs.com/2050/p/3877280.html)
- [pt和px](https://www.cnblogs.com/zdz8207/p/vue-pt-px-750.html)
- [ES2015 规范](http://www.ecma-international.org/ecma-262/6.0/index.html)
- [javascript 函数式编程](https://www.freecodecamp.org/news/functional-programming-principles-in-javascript-1b8fc6c3563f/)
- [ES5 规范](http://es5.github.io/)
- [菜鸟教程 CSS 规范](https://www.runoob.com/w3cnote/html-css-guide.html#css)
- [W3C-CSS选择器汇总](https://www.w3cschool.cn/css/css-selector.html)
- [CSDN-Web性能优化之CSS性能优化篇](https://blog.csdn.net/m0_37972557/article/details/80370822)
- [浏览器解析 CSS 样式的过程](https://segmentfault.com/a/1190000018717319)
- [W3C 层叠样式表规范](https://www.w3.org/TR/css-cascade-4/#cascading)
- https://blog.csdn.net/lch1251680944/article/details/87975532
- [W3C 文档流规范](https://www.w3.org/TR/CSS2/visuren.html#normal-flow)
- [浅谈几个前端异步解决方案](https://yq.aliyun.com/articles/617734?utm_content=m_1000007927)
- [Promises/A+ 规范](https://promisesaplus.com/)
- [MDN Promise API](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [浏览器 Promise 兼容表](http://kangax.github.io/compat-table/es6/)
- [ES2015 Promise 规范](https://www.ecma-international.org/ecma-262/6.0/#sec-promise-objects)
- [axios adapters](https://github.com/axios/axios/tree/master/lib/adapters)
- [nodejs http 模块 API](https://nodejs.org/dist/latest-v8.x/docs/api/http.html)

- [JavaScript深入之词法作用域和动态作用域](https://github.com/mqyqingfeng/Blog/issues/3)
- [undefined 和 null](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)
- [EventTarget.addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- [JavaScript 运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)
- [ES5 类型转换规范](http://es5.github.io/#x9.1)
- [Object.prototype.valueOf](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf)
- [JavaScript深入之从ECMAScript规范解读this](https://github.com/mqyqingfeng/Blog/issues/7)
- [菜鸟教程 JavaScript 闭包](https://www.runoob.com/js/js-function-closures.html)
- [JavaScript 六种继承方式](https://segmentfault.com/a/1190000016708006)
- [进程与线程](https://www.liaoxuefeng.com/wiki/897692888725344/923056118147584)
- [MDN async](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [JavaScript 调用栈,回调队列,事件循环](https://cek.io/blog/2015/12/03/event-loop/)
- [C++ 视角阐述为什么需要异步](https://www.cnblogs.com/goya/p/11962828.html)
- [MDN XMLHttpRequest API](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- [MDN Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [MDN CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
- [Neal Ford 作者阐述的函数式](https://www.ibm.com/developerworks/cn/java/j-ft14/)
- [图灵程序设计丛书 函数式思维 Neal Ford]()
- [JavaScript 函数式编程思想 潘俊]()
- [冴羽的博客 JavaScript专题之函数柯里化](https://github.com/mqyqingfeng/Blog/issues/42)
