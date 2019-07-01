
# 计算机(内存,CPU,X64&X86)

# 程序(程序=算法+数据结构,栈,堆,树,图...)

# 网络(internet,http,tcp,udp,ssl,加密)


## TCP/IP

网络结构有两种主流的分层方式: OSI 七层模型和 TCP/IP 四层模型

TCP/IP是指传输控制协议/网间协议，是目前世界上应用最广的协议

### TCP 三次握手(建立链接)

以 A(Client) 与 B(Server) 通信为例子
· 第一次, A 向 B 发送消息, 收到消息。A 确认自己的发信能力和 B 的接收能力
· 第二次, B 向 A 发送消息, A 收到消息。A 确认自己的发送能力和接收能力，A 也确认B 的发送能力和接收能力
· 第三次, 最后 A 再向 B 发送消息, B 接收到消息。B 可确认 A 的接收能力和 B 的发送能力

Http和Https协议请求时都会通过Tcp三次握手建立Tcp连接。

通过三次握手，A和B都能确认自己和对方的收发信能力，相当于建立了互相的信任，就可以开始通信了

### TCP 四次挥手(断开链接)

· 第一次挥手：主机1（可以是客户端或服务器），设置seq和ack向主机2发送一个FIN报文段，此时主机1进入FIN_WAIT_1状态，表示没有数据要发送给主机2了
· 第二次挥手：主机2收到主机1的FIN报文段，向主机1回应一个ACK报文段，表示同意关闭请求，主机1进入FIN_WAIT_2状态。
· 第三次挥手：主机2向主机1发送FIN报文段，请求关闭连接，主机2进入LAST_ACK状态。
· 第四次挥手：主机1收到主机2的FIN报文段，想主机2回应ACK报文段，然后主机1进入TIME_WAIT状态；主机2收到主机1的ACK报文段后，关闭连接。此时主机1等待主机2一段时间后，没有收到回复，证明主机2已经正常关闭，主机1页关闭连接。

主机1向主机2断开链接,主机1向主机2说我没有要发送的数据了要求断开链接,主机2回应说同意你断开链接,主机1就说那我断开链接,主机2说好的准备断开,最后主机1等待链接被关闭

握手协议是指主要用来让客户端及服务器确认彼此的身份的一类网络协议

除此之外，为了保护SSL记录封包中传送的数据，握手协议还能协助双方选择连接时所使用的加密算法、MAC算法及相关密钥

在传送应用程序的数据前，必须使用握手协议

### OSI 与 TCP/IP 区别
1. OSI 采用七层模型, TCP/IP 四层模型
2. TCP/IP 网络接口层没有真正定义，只是概念性的描述。OSI 把它分成 2 层，每一层功能详尽。
3. 在协议开发之前，就有了OSI模型，所以OSI模型具有共通性，而TCP/IP是基于协议建立的模型，不适用于非TCP/IP的网络
4. 实际应用中，OSI模型是理论上的模型，没有成熟的产品；而TCP/IP已经成为国际标准



## HTTP

HTTP 是基于 TCP/IP 协议的应用层协议，它不涉及数据包传输，主要规定了客户端可服务区之间的通信格式

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
+ repaint 
  - 改变某个元素的背景色、文字颜色、边框颜色等等不影响它周围或内部布局的属性时，屏幕的一部分要重画，但是元素的几何尺寸没有变


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
