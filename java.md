# 多态 #

将 new 的对象交给父类引用，这样，父类对象就可以访问到子类的成员

# 集合 #

+ Collection 接口
	
	+ List 子接口
		+ ArrayList 主要实现类
		+ LinkedList 实现类
		+ Vector 实现类
	+ Set 子接口
		+ HashSet 主要实现类
		+ LinkedHashSet 实现
		+ TreeSet 实现类

+ Map 接口
	+ HashMap 主要实现类
	+ TreeMap 实现类
	+ HashTable 废弃的实现类
		+ Properties 实现类： 该类主要会用在对 properties 属性文件的操作

+ 迭代器 
	+ Iterator
	+

+ 集合工具
	+ Collections
	+ Arrays
	
## 判断自定义对象时 
Collection 中需要重写 equaals() 方法判断两个
Map 中需要重写  hasCode() 和 equals() 


Map 的每一个键值对用 Entry 这个内部类对象存放
Map 的所有键用 Set 存放
Map 的所有值用 Collection 存放


在遍历集合时，可以使用 for 循环，或者是迭代器，这时的迭代器就是迭代器的那两个接口

集合的遍历排序规则是用排序两个接口的实现类定义

# 泛型 #


# IO #

IO 有两种，一是字节，二是字符

非文本文件只能用字节流
但文本文件也可以使用字符流

字节流操作文本文件
字符流操作非文本文件

*字节流就是一个字节数组，byte[]*

+ 节点流
	+ FileInputStream 
	+ FileOutputStream
	+ FileReader
	+ FileWriter

+ 缓冲流, 比节点流更快,可以提升文件操作的效率
	+ BufferedInputStream
	+ BufferedOutputStream
		- flush()
	+ BufferedReader
		- readLine() 	一次读取一行
	+ BufferedWriter
		- flush()

+ 转换流： 将一个字节流转换成一个字符流
	+ InputStreamReader

	+ OutputStreamWriter

+ 打印流: 有自动的 flush() 刷新
	+ PrintStrem: 输出字节流

	+ PrintWriter：输出字符流

+ 对象流：实现对象的输入和输出，非常类似 JS 的 JSON 
	+ 对象序列化 与 反序列化
		任何实现了 Serializable 接口的对象转化为字节数字
		可以主将 Java 对象转换成平台无关的二进制流，持久地保存在磁盘上

	+ ObjectInputStream

	+ ObjectOutputStream

	但是这两个类的对象不能序列化 static 和 transient 成员

+ RandomAccessFile : 随机访问，程序可以在任意位置读，写
	1. 该类即可以充当一个输入流，又可以充当一个输出流
	2. 支持从文件开头读取，写入
	3. 支持从任意位置的读取，写入


+ 标准的输出流： System.out
+ 标准的输入流： System.in



# 多线程 #
java.lang.Thread


## 线程调度
时间片
抢占式：高优先级的线程抢占CPU
		

## 线程优先级
+
	- MAN_PRIORITY 10
	- MIN_PRIORITY 1
	- NORM_PRIORITY 5
+ getPriority() 返回线程优先值
+ setPriority(int np) 改变线程优先级
	- 线程创建时继承父优先级

## Thread 常用方法
- start(): 		启动线程并执行相应的 run() 方法
- run(): 		子线程要执行的代码
- Thread static currentThread(): 调取当前的线程
- getName(): 	获取当前线程名
- setName(): 	设置当前线程名
- yield(): 		调用此方法的线程强制释放当前执行权
- join(): 		当前线程调用 
- Boolean isAlive(): 判断当前线程是否还存活
- sleep(): 		显式的让当前线程睡眠



程序需要同时执行两个或多个任务
程序需要实现一些需要等待的任务时
需要一些后台运行的程序时

+ 线程的创建
	1. 继承 Thread
	2. 实现 Runnable
		多个线程共用一个数据的时候可用实现方式 

+ 线程生命同期
	JDK 中用 Thread.State 枚举类表示了线程的几种状态
	1. 新建
	2. 就绪
	3. 运行
	4. 阻塞
	5. 死亡


+ 线程的同步
	但也因为线程同步会影响效率变低的问题
	+ 线程安全
		当多个线程同时操作共享数据过程中，未执行完毕的情况下
		另外的线程参与进来，就导致了共享数据安全问题了
	线程同步就是用来解决线程安全的问题
	
	+ 同步代码块 Synchronized(同步监视器){ 需要被同步的代码}
		- 多个线程共同操作一个数据
		- 由任何一个类的对象来充当，
			哪个线程获取此监视器(锁),谁就执行代码块中的内容
			它只是起一个关门开门的作用，当一个线程进去
				它就把门关闭，当这个线程出来就再把这个门打开
				只有门是打开线程才能进去，就起到锁的作用了 
	+ 同步方法
		将操作共享数据的方法声明为 synchronized
		同步方法锁默认为： this

	+ 线程死锁
		不同的线程中分别占用对方需要的同步资源不放弃
		都在等待对方放弃自己需要的同步资源就形成了线程死锁


+ 线程的通信
	主要是下面的这三个方法，但这三个方法定义在 Object
	并且只能在 synchronized 中使用
	+ wait() 让当前线程扶挂起并放弃CPU，同步资源
		使别的线程可以访问并修改共享资源 

	+ notify() 唤醒正在排队等待同步资源的线程中优先级最高者结束等待

	+ notifyAll() 吃醋正在排队等待资源的所有线程结束等待


+ 线程分类
	+ 守护线程
	+ 用户线程
		- thread.setDaemon(true) 将用户线程变成守护线程，start() 前

· 提高应用程序响应
· 提高计算机系统CPU的利用率
· 改善程序结构

*要想启动线程必须调用  start()*

子类抛的异常不能大于父类

# 常用类 #

+ String
	- int length()
	- char charAt(int index)
	- boolean equals(Object obj)
	- int compareTo(String s)
	- int indexOf(String s, int startpoint)
	- int lastIndexOf(String s)
	- int lastIndexOf(String s, int startpoint)
	- boolean startsWith(String prefix)
	- boolean endWith(String suffix)
	- boolean regionMatches(int f, String other, int otherStart)
	- String substring(int startpoint)
	- String substring(int start, int end)
	- String replace(char oldChar, char newChar)
	- String trim()
	- String conncat(String str)
	- String[] split(String regex)


+ StringBuffer 可变字符序列,与 String 不同的是长度是可变的
+ StringBuilder 可变字符序列
	是 JDK5+ 但，线程不安全，效率要高于  StringBuffer
	这三个效率中，StringBuiler > StringBuffer > String
+ System
+ Date
+ SimpleDateFormat
+ Calendar
+ Math
+ BigInteger & BigDecimal

# 反射 #


*每一个运行时类只加载一次*
当有了 Class 的实例后，才可以进行下面的操作

+ Class 类
	*在用反射创建类时，该类必须要有无参的构造器*
	java.lang.Class 是反射的源头
	
	+ 获取 Class 的实例
		+ 创建类的对象: 调用 Class 对象的 newInstance() 方法
			- 并且类必须有一个无参的构造器
			+ 但是并不是没有无参构造器就不能创建
				- 只要在操作的时候明确调用类中的构造器，并将参数传递之后就可以实例化
				- 可通过类的 getDeclaredConstructor(Class ... partentTyope)
			


+ 运行时类的所有结构

	+ 实现的接口 	Interface
		-  Class<?>[] getInterfaces() 获取所有实现接口
 
	+ 继承的父类	SuperClass
		- Class getSuperclass()  获取所有父类
		+ Type getGenericSuperclass() 获取所有带泛型的父类
 			+ 获取泛型的类型

				@Test
				public void test2(){
					Class clazz = Person.class;
					Type type1 =  clazz.getGenericSuperclass();
					ParameterizedType param = (ParameterizedType)type1;
					Type[] args = param.getActualTypeArguments();
					System.out.println(((Class)args[0]).getTypeName());
				}

	+ 构造器		Constructor
		- Constructor<?>[] getDeclaredConstructors() 获取所有的构造器

	+ 方法 		Method
		+ 方法结构
			- 注解 
			- 修饰符	
			- 返回类型	
			- 方法名	
			- 形参列表	
			- 异常		
		- Method[] getMethods() 获取运行时类及其父类中 public 的所有方法
		- Method[] getDeclaredMethods() 获取运行时类本身声明的所有方法
		- Annotation[] getAnnotations() 	获取所有注解
		- int getModifiers() 返回属性修饰符编号，该可用 Modifier.toString() 
		- getType() 获取方法类型
		- getName() 获取方法名
		+ Class[] getParameterTypes() 返回所有形参列表
			- 但是形参的形参名获取不到 getName() 只能获取到形参的类型 
		- Class[] getExecptionTypes() 获取所有抛出的异常

	+ 属性 		Field
		- Field[] getFields()	  获取运行时类及其父类中 public 属性
		- Field[] getDeclaredFields() 获取运行时类本身声明的所有属性
		- int getModifiers()  返回属性修饰符编号，该可用 Modifier.toString() 方法得到修改符串
		- getType() 获取属性类型
		- getName() 获取属性名

	+ 注解 		Annotation
		- Annotation[] getAnnotations() 获取该类的注解
 
	+ 所在包 	Package
		-  Package getPackage() 获取该类所在的包
 
	+ 内部类
		-  Class<?>[] getClasses() 获取所有内部类

+ 反射调用类的结构
	
	+ 获取属性
		- Field getField(Sring fieldName) 获取运行时类 public 的属性名
		- Field getDeclaredField(Sring fieldName) 获取运行时类的属性名

	+ 获取方法 
		+ 调用指定构造器
			-  Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes) 调用指定构造器
		- Method getMethod(String methodName ... args) 调用方法
		- Method getDeclaredMethod(String methodName ... argsObj) 获取运行时类中声明的一个方法
		+ Object invoke(Object obj ... args) 执行方法, 并返回该方法的返回值
		


+ 动态代理 
	+ 实现 InvocationHandler  接口的类
	+ 被代理类的实例的创建 

+ 动态代理与编程



反射可以理解成操作有目的的操作未知的类的对象


# 网络编程 #

+ IP 和端口号
	+ InetAdderss 类
+ 网络通信协议
	+ TCP
		+ Socket
		+ ServerSocket
	+ UDP
		+ DatagramSocket
		+ DatagramPacket 

+ URL 编程
	+ URL 类
		- 一个 URL 类的对象就是一个资源
		- getProtocol()
		- getHost()
		- getPort()
		- getPath()
		- getFile()
		- getRef()
		- getQuery()
		+ openStream() 能从网络读取一个数据,注意只能读取，如果需要输出则需要 URLConnection
			+ URLConnection  表示到 URL 所引用的远程对象的连接
				+ openConnection() 该方法生成对应的 URLConnection 对象

		+ openConnection() 
+ Socket  套接字
网络通信其实就是 Socket 间的通信，并且数据是在两端间通过 IO 传输

客户端与服务端需要显示的告诉对方，请求或者数据发送，完毕，否则会一直占用请求的资源不会停止
	socket.shutdownOutput()  发送完成 
	socket.shutdownInput()  接收完成 






# Java 数据库储存技术  #
Java Database Connectivity

+ JDBC
	- 可以连接任何提供了 JDBC 驱动程序的数据库系统
	+ Java 与数据库之间存在的关系
		1. Java 应用
		2. JDBC 一组规范，接口
		3. 提供 JDBC 的实现，也叫 JDBC 的驱动
		4. 到数据库
	+ 面向接口编程
		- 接口定义了方法，而接口不能直接实例，需要实现的类
			这个类在 JDBC 中就是每种数据库的厂商所自己数据库能够支持 Java 所自己实现的 JDBC 接口的实现类
		- 每种数据库实现类都有自己的 API，但它们都是实现了 JDBC 接口
		- 所以只需要按照 JDBC 中的方法来编程就能达到效果，这就是面向接口
	+ Driver 接口
		- 该接口是据 JDBC 驱动实现的接口
	
	- 也不是操作那种数据库就需要那种数据库的实现类，也就是驱动

	+ 连接
		+ JDBC URL
			+ 组成 
				- JDBC:子协议:子名称
			+ 三种数据库的 URL
				- jdbc:oracle:thin:@localhost:1521:sid
				- jdbc:microsoft:sqlserver//localhost:1433;DatabaseName=sid
				- jdbc:mysql://localhost:3306/sid
		+ 方法一
			- 通过反射得到相应驱动的实现类对象
		+ 方法二
			+ DriverManager 驱动的管理类
				- 可以通过重载的 getConnection() 方法获取数据库连接
				- 可以同时管理多个驱动程序
			- 通过 DriverManager 类的静态方法 getConnection() 得到连接

	+ 数据操作， Statement 接口
		+ Statement 接口是操作数据库的接口
			- executeQuety() 得到查询结果集
			- executeUpdate() 方法执行 SQL 语句 ，可以是 insert, update, deleted 不能是 select
		+ 查询
			+ ResultSet 这个查询返回的结果集
			- getResultSetMetaData() 可以得到 ResultSetMetaData 的对象
	+ PreparedStatement 是 Statement 子接口
		- *该接口可以防止 sql 注入*
		- 还可以尽量提高性能
		- 该接口可以实现大量拼接 sql 的过程，它可以传入带占位符的 SQL 语句，并且提供了补充占位符变量的方法

		// 1. 创建 PreparedStatement
		String sql = "INSERT INTO user VALUES(?,?,?,?,?)"
		PreparedStatement ps = conn.preparseStatement(sql);
		// 2. 调用 PreparedStatement 的 setXXX() 来设置占位符的值
		// setObject() 可以设置任何类型

		// 3. 执行 SQL 语句， executeQuery() ，并且执行时不需要传入 sql 语句

	+ ResultSetMetaData
		- int getColumnCount() 返回一共有多少条列的数据
		- String getColumnLabel(int index) 获取索引位置的列名
		- Object getObject(int index) 获取索引位置的值

	+ DatabaseMetaData 
		- 该类提供了许多用于获取数据源的信息

## DAO (Data Access Object) ##

访问数据信息的类，CRUD 操作（create, read, update, delete）
不包含任何业务逻辑信息

+ JDC	
+ 第三方O/R 具


## JavaBean

JvavEE 中的类的属性是通过 getter/setter 定义
	也就是说 JavaBean 在操作属性的时候不是按照成员变量的变量名，而是去掉对应的 get/set 后面的名字
	所以一般情况下 getter/setter 的名字和成员变量名字一致
而之前的成员变量就是字段
beanutils 专门操作

BeanUtil-1.9.3 +
Collection-3.2.2 +
logging-1.2






！！！完成 mon2_day7.qlover.Beanutils 项目











swift.sql



重定向请求会变


## 修改 Tomcat 端口号
conf/server.xml
<Connector port="8080"
	protocol="HTTP/1.1"
	connectionTimeout="20000"
	redirectPort="8443"
/>

# JavaWeb #

request
response


# 重定向与转发 #


重定向：
	客户端行为
	response.sendRedirect()
	本质上等同于两次请求
	request 不同
	地址栏 URL 会改变 
请求转发：
	服务器行为
	resquest.getRequestDispatcher().forward()
	一次请求
	request 相同
	地址栏不会改变



+ RequestDispatcher 接口
	- forward() 实现请求转发

+ HttpServerletRequest 
	- sendRdirect() 实现重定向



# JSP #

+ 静态内容 
+ 指令
	<%@  %>
+ 注释
	<!-- -->
	<%-- --%>
	//
	/**/
+ 声明
	<%!  %>

+ 表达式
	<%=  %>

+ 脚本
	<%  %>



## JSP 九大内置对象 ##

+ out 
	- println()
	- clear() 清除缓冲区，在 flush() 后抛异常
	- clearBuffer 清除缓冲区，在 flush() 后不会抛异常
	- flush() 将缓冲区内容输出到客户端
	- getBufferSize()
	- getRemainning() 返回缓冲区还剩余多少可用
	- isAutoFlush()
	- close() 
+ request
	- setCharacterEncoding() 设置语法的编码方式
		- conf/config.xml 下
			<Connector  URIEndodeing="utf-8"/>
			可解决 URL 传递时的编码问题

+ response
	- sendRedirect() 重定向客户端请求
	- getWiter() 输出流，默认比 out 对象靠前

+ session
	- getCreateTime()
	- String getId()
	- setAttribute()
	- getAttribute()
	- getValueNames()
	- getMaxInactiveInterval()
	- setMaxInactiveInterval()
	+ 销毁 Session
		- session.invalidate()
		+ Session 超时，默认 30 分钟
			- <seesion-config>
					<sesstion-tiemout>
						[分钟数]
					</sesstion-tiemout>
				</seesion-config>
		- 服务器重启
	*闭关所有页面后，Session 并不会马上被销毁，而是会等到过期时间一到，自动销毁*


+ application
	- 实现了用户间数据的共享，可存放全局属性
	- 开始于服务器的启动，结束于服务器闭关
	- ServerletContext 的实例


+ Page
	- 指向当前 JSP 页面本身，类似 JS 的 document
	- java.lang.Object 的实例

+ pageContext
	- 提供了对 JSP 页面内所有的对象及名字的访问
	- 对象的本类名也叫 pageContext

+ config
	- 在一个 Servlet 初始化时， JSP 引擎向它传递信息用的

+ exception
	- 一个异常对象，该对象只存在于 isErrorPage="true" 的页面中
	- java.lang.Throwable 的对象




！！！ 完成 mon2_day9 login.jsp 项目


## JSP 动作元素 ##

+ 存取 JavaBean
	- <jsp:useBean>
	- <jsp:setProperty>
	- <jsp:getProperty>

+ JSP1.2+ 基本元素
	+ <jsp:include>
		- <%@ include file="URL" %> 指令
		- <jsp:include page="URL" flush="true|false" />
			- page 页面地址
			- flush 是不读取缓冲区内容
	+ <jsp:forward>
		- request.getRequestDispathcer().forward() 与之相同
	+ <jsp:param>
		- 常与 <jsp:forward> 一起使用，并且是其的子标签
	- <jsp:plugin>
	- <jsp:params>
	- <jsp:fallback>

+ JSP2.0+ JSP Document
	- <jsp:root>
	- <jsp:declaration>
	- <jsp:script>
	- <jsp:expression>
	- <jsp:text>
	- <jsp:output>

+ JSP2.0+ 动态生成 XML 标签
	- <jsp:attribute>
	- <jsp:body>
	- <jsp:element>

+ JSP2.0+ 用于 Tag File 中
	- <jsp:invoke>
	- <jsp:dobody>


## JavaBean 作用范围 ##

+ page
+ request
+ session
+ application



## Cookie ##
+ Cookie 类

+ response.addCookie()


## MVC ##

+ Controller 用 Servlet 充当
+ View 用 JSP 充当
+ Model 用 JavaBean 充当




# Spring MVC #

+ 轻量级
+ 依赖
+ 面向切面编程
+ 容器
+ 框架
+ 站式



# Maven 管理 Spring MVC

## POM
Project Object Model

## Dependency
依赖

## Coordinates 
坐标

groudId
artifactId
version
packaging

## Spring 注解
+ @Autowrite 			自动装配， 也就是让 Spring 得到的实例注入到到当前
+ @Service  			对应业务层		指定自动装配时得到实例的名称
+ @Repository 		对应数据访问层
+ @Controll   		对就表现层
+ @RequestMapping 	指定该类/方法 映射到 URL 上是什么地址
+ @ResponseBody 		修饰方法，响应所使用的方法
+ @ResquestBody 		修饰方法，得到前端的响应
+ @PathVariable 		地址的一个变量
+ @RequestParam 		与表单某个元素关联
+ @MoelAttribute 		绑定参数

## Binding
将请求的的字段按照名字的匹配原则填入模型

也就是控件的名字要和 Bean 的字段名相同


请求重写向
@Controller
public String fun(){
	return "rdirect:{url}";
}


## JSON

+ ContentNegotiationViewResolver
	- 一个处理相同数据格式的






# ModelAndView #

+ setViewName() 
	- 跳转到指定的页面

+ addObject()
	- 设置所需返回值




mvn archetype:generate -DgroupId=imooc-lushaobin -DartifactId=spring-mvc-learning -DarchetypeArtifactId=maven-archetype-webapp