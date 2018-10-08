# Scrapy


	building 'twisted.test.raiser' extension
	error: Microsoft Visual C++ 14.0 is required. Get it with "Microsoft Visual C++ Build Tools": http://landinghub.visualstudio.com/visual-cpp-build-tools

: https://www.jb51.net/article/125081.htm


`scrapy startproject [项目名]`

`scrapy genspider [爬虫名] ['http://' 爬取的域]`

`scrapy crawl [爬虫名]` 	# 执行爬虫



# Spider #

是最基本的类，所有编写的爬虫必须继承这个类

+ name
	name 是 spider 类最基本的属性，也是必须的属性
+ allowed_domains
	包含了 spider 允许爬取的域名的列表，可选
+ start_urls
	初始 url 元组/列表，当没有特定的 URL 时， spider 将从该列表中进行爬取
+ start_requests()
	该方法必须返回一个可迭代对象，该对象包含了 spider 用于爬取的第一个 Request

+ paser()
	当请求 url 返回网页没有指定回调函数时，默认的 Request 对象回调函数，用来处理网页返回的 response, 以及生成 item 或者 Request 对象
+ log()
	使用 scrapy.log.msg() 方法记录 log 时





### 腾讯招聘

[腾讯招聘](https://hr.tencent.com/position.php)

职位名	
positionName
职位链接	
positionLink
职位类型	
positionType
招聘人数	
peopelNumber
工作地点	
workLocation
发布时间	
publishTime




>> ModuleNotFoundError: No module named 'win32api'
[](https://sourceforge.net/projects/pywin32/files/pywin32/Build%20221/)
安装相同版本的 pywin32 

>> Object of type 'bytes' is not JSON serializable
[](https://blog.csdn.net/z564359805/article/details/80599126)
[](https://blog.csdn.net/bear_sun/article/details/79397155)















https://blog.csdn.net/daybreak1209/article/details/52425308








