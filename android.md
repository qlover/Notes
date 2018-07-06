
自由门

Android 4.4+ 添加了 ART 模式

dx.bat 将所有 .class 变成  .dex

appt.exe Android 打包工具

+ adb.exe  Android 的调试桥
	- adb install xxx.apk 安装
	- adb uninstall 应用程序的包名 卸载
	- adb shell 进入手机终端
	- adb push x.x 将一个文件拉出
	- adb pull [path] x.x 将一个文件添加到手机中
ARM 公司

IntelHaxm 模拟器加速器

2.3  	10
3.0 	11
4.0 	14
4.1.2 	16

320x480
480x800
1280x720

+ src
+ gen
+ bin apk 文件
+ assets 资产文件
+ libs 		
+ drawable
+ layout
+ values

向 SD 写入要加权限
	WRITE_EXTERNAL_STORAGE




# Android UI #

##  UI Layout  ##
+ LinearLayout: 绝对布局
+ RelativeLayout: 相对布局
+ TableLayout: 表格布局
+ FrameLayout: 帧布局

## UI Componenets 组件 ##
TextView: 文本视图
EditView: 输入框视图
Button：按钮视图
ImageView: 图片视图
Toast：临时提示视图
ListViewL：列表视图
SeekBar: 可操作的进度条
OptionMenu： 菜单 


# Android 数据存储 #
手机内部小数据存储
手机内部文件存储
手机外部 SD 卡文件存储
手机内部数据库存储
网络存储



# SharedPrefrence 存储 #
/data/data/packageNamee/shared_prefs/xx.xml

相关 API
android.contentt.SharedPreference
android.content.SharedPreferences.Editor

只用来存储一些基本类型和 string 类型的小数据

SharedPreferences sp = context.getSharedPreferences(name, mode)
Edit edit = sp.edit()
edit.putString(name, value)
edit.commit()

String value = sp.getString(name, defaultValue)



一般联网的应用都会将图片缓存到 SD 卡中
在操作 SD 卡时需要声明使用的两个权限
WRITE_EXTERNAL_STORAGE
READ_EXTERNAL_STORAGE



网络存储主要是将数据上传到过程服务器上保存
网络存储时也要打开一个权限
INTERNET

三个 API
HttpUrlConnection (java)
HttpClient		(android)
AsyncHttpClient 



Android 或者 IPhone 都有 SQLlist 数据库

* tomcat 服务器




# 四大组件 #

+ Activity
Activity  是第一个启动的 activity 
当 activity 一启动就会执行 onCreate() 
	- 执行路径
		1. onCreate()
		2. onStart()
		3. onResume()
		4. onPause()
		5. onStop()
		6. onRestart()
		7. onDestory()
+ BroadCastReceiver

+ Service

+ ContenProvider 


# 五大布局 #
1. 线性
2. 相对
	+ 默认相对布局
3. 帧
4. 表格
5. 绝对 

## Activity ##


+ :id
	- @+id 表示在 R 文件中增加一个 id 
+ :layout_width

	- match_parent 充满父盒子
+ :layout_height
+ :text
+ :layout_below 在那个控件下面，接 ID 名
+ :layout_gravity 居中方位

### 四种点击事件 ###
1. 用内部类
2. 匿名内部类
3. 在 MainAcitvity 中实现 View.OnClickListeneter
4. 在 布局文件添加  onClick 属性，值就是方法名，该方法在 MA 中
	+ public void methodName(View v)


### URL & URI ###
+ URL 统一资源定位符
+ URI 统一资源标识符

## Toast 吐司
用于提示
+ static makeText()

## Intent 意图



## AttributeSet 类
该类封装了布局文件中的所有属性



## Environment 

+ Environment.getExternalStorageDirectory().getPaht()
	- 获取 SD 卡的目录
+ Environment.MEDIA_MOUNTED.equals(Environemnt.getExternalStorageState())
	- 判断 SD 卡是否可以用



# 数据存储 #

## SharePreferences ##
1. 获取该类的一个实例
	- getSharedPreferences()
2. 获取编辑器
	- Editor edit()
3. 提交，！一定要记得提交
	- sp.commit()


# XML #

Android 传输数据用两种方式，XML 和 JSON

## XML 序列化 ##
+ XmlSerializer
	- 可以通过这个类来序列化一个 XML

## XML 解析 ##


# 数据库 #

Android 中使用 SQList

+ SQLiteOpenHelper 抽象类

+ onCreate()

+ onUpgrade()


## DB 对象 ##

该对象用于操作数据库

+ long insert() 向数据库插入记录,并返回新行的 id
	- 但该方法用 Map 向数据库插入记录
+ int delete() 从数据库中删除记录，并返回影响的行数

+ update() 更新记录

+ query() 查询

# ListView #

1. 定义 listview 布局
2. 定义 listview 的数据适配器，就是一个继承自 BaseAdapter 的类
3. 实现 BaseAdapter 的 getCount() 和 getView()


### out of memory
`内存溢出`
Android 也用垃圾回收机制

如果在 listview 中出现该异常，则表示在当前一瞬间会调用非常多次的 getView() 方法
在还没来得及释放资源时，又被调用，所以会出现内存溢出的错误
解决该问题，可以使用 getView() 第二个参数 convertView 
该参数表示的时，在 Listview 中被拖过的数据的缓存，也就是数据在屏幕上看到见了就是一个该对象


当 listview 第一次被加载到屏幕中时，会创建能见的几个 view 对象
而当屏幕下滑动后，下一屏的对象将会是被拖过的对象，但是数据还是自己的数据
只是一次只会初始化第一次被加载到屏幕中能看见的那么多个 view 对象
而后面的都是一个一个的复用初始化的 view 对象
也就是被拖过的对象会变成  convertView 对象，之后复用将会是该对象，所以直接强转就得到了


### Android MVC
m: mode
v: view
c: adapter


## inflate()，打气筒 
该方法可以将一个 xml 资源转换成一个 view 对象


## 获取打气筒服务

+ View.inflate()
+ LayoutInflater.fron()
+ getSystemService()



# 权重 #
将一个控件占的宽度分空间

# HttpUrlconnection #
发送/接收 数据


## HttpClient ##


android.permission.INTERNET

## ScrollView ##
可滚动的视图 

`<ScrollView></ScrollView> 只能有一个子元素，切记只能有一个子元素`

# ANR 异常#

Application Not Response 应用无响应 异常

原由是因为在主线程（UI线程）长时的操作或者是阻塞了

*Android 4+ 强制不能在主线程连接网络*

并且只能在主线程更新 UI

## handler 原理 ##



handler 用于发/处理消息，在子线程中用 sendMessage() 发送消息
在主线程中用 handleMessage() 处理发送的消息
并且在发送消息时 handler 会有一个队列，这个队列是由 Looper 这个类监视
它会不停的监视是否有新发送过来的消息，如果有则会将发送过来的消息交给 handleMessage() 处理

只要长时操作就开启一个子线程就行了




# 图片查看 #

图片的流需要将流转换成 bitmap 位图这个


## cache & file ##

cache -> clear cache 
file  -> clear data

## runOnUiThread ##

该 API 是谷歌封装了的 Handler 
不管在什么位置上调用 action ，都运行在 UI 线程里
### runOnUiThread(Runnable runable) 


# 开源项目 #

+ smartImageView
+ AsyncHttpClient


## smartimageview ##

是一个 GitHub 托管的项目










# 多线程加速下载  #






# Fragment #

一个 Fragment 是一个 Activity 一部分

## 动态添加一个 Fragment 

1. 得到 Fragment 管理者
FragmentManager fm = getFragmentManager()

2. 开启一个事务
FragmentTransaction beginTransaction = fm.beginTransaction()

3. 提交事务
beginTransaction.commit()

## Fragment 通信

因为一个 Fragment 是一个 Activity 一部分，Fragment 相互通信则需要借助于两个 Fragment 的桥梁，桥梁则是两个 Fragment 共同有的，就是两个 Fragmeng 共同存在于的 Activity 上

getActivity()  # 利用上下文得到桥梁
因为 Fragment 没有继承 Context
getFragmentManager() # 得到了另一个 Fragment 的引用了












setDoOutput() 设置一个标记允许输出


android:singleLine
android:ellipseize
android:maxLine
android:layout_alignBottom
android:fastScrollEnabled="true" 


音乐世界 cytus 



^(?!.*(nativeGetEnabledTags|Skipped)).*$
