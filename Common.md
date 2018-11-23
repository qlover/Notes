# Linux 


# Mac

# Android 




# gcc

+ gcc test.c //会默认生成a.out可执行程序 
	- -E //会生成test.i文件
	- -S //会生成test.s文件
	- -c //会生成test.o文件
	- -o test test.c //会生成名字是test可执行文件而不是默认的a.out
	- -pipe -o test test.c 使用管道代替编译中的临时文件 
	- test.c -include /root/file.h 包含某个代码。相当于在文件中加入#include
	- -C 在预处理的时候不删除注释信息，一般和-E使用。
	- -M 生成文件关联信息。包含目标文件所依赖的所有源代码
	- -MM 和-M一样，只不过忽略由#include所造成的依赖关系
	- -MD 和-M相同，只不过将输出导入到”.d”文件里面
	- -MMD 和-MM相同，将输出导入到”.d”文件里面。
	- -llibrary 定制编译的时候使用的库 
		
		```gcc -lpthread test.c //在编译的时候要依赖pthread这个库```

	- -Ldir 定制编译的时候搜索库的路径。如果是自己定制的库，可以用它来定制搜索目录，否则编译器只在标- 准库目录里面找，dir就是目录的名字
	+ -O0(字母o和数字0) 没有优化`级别越大优化越好，但编译时间边长`
		   - -O1 -O1位缺省值
		   - -O2 二级优化 
		   - -O3 最高级优化 
		
	- -g 在编译的时候假如debug调试信息，用于gdb调试
	- -share 此选项尽量的使用动态库，所以生成文件比较小，但是必须是系统有动态库。
	- -shared 生成共享目标文件，通常用在建立共享库。
	- -static 链接时使用静态链接，但是要保证系统中有静态库。
	- -w 不生成任何警告信息
	- -Wall 生成所有警告信息



# Java

java -version # 查看当前版本

javac [.java] # 编译,并生成 class 字节码文件

java [.exe] # 执行




# Python

python --version # 当前版本

python -m http.server [端口号] # 利用 python 从当前目录启动一个服务器

python test.py [arg1 arg2 ... argN] # 执行 test，sys.argv 是命令行参数列表

python -m pip uninstall pip # 卸载 pip

python setup.py install # 利用 setup.py 进行安装

# Pip

pip install [库名] # 安装库

pip uninstall [库名] # 卸载库

pip show [库名] # 查看安装

pip list # 已安装的库列表

# Scrapy 

scrapy startproject [项目名] # 初始化构建项目

scrapy genspider [爬虫名] ['http://' 爬取的域] # 生成爬虫文件

scrapy crawl [爬虫名] 	# 执行爬虫

# Tomcat

./shutdown.sh # 关闭

./shartup.sh  # 启动

ps -ef|grep java  # 查看Tomcat是否以关闭

kill -9 7010 # 杀死该进程


# Vim

+ 打开/退出
	- vim -R file1 只读打开
	- :qall 退出所有文件
	- :wq 写入并退出
	- :q! 强制退出
+ 插入

	- i 在当前位置生前插入
	- I 在当前行首插入
	- a 在当前位置后插入
	- A 在当前行尾插入
	- o 在当前行之后插入一行
	- O 在当前行之前插入一行

+ 移动
	- h 左移一个字符
	- l 右移一个字符
	- k 上移一个字符
	- j 下移一个字符
以上四个命令可以配合数字使用，比如20j就是向下移动20行，5h就是向左移动5个字符。

+ 删除
	- dd 删除当前行
	- dj 删除当前行和上一行
	- dk 删除当前行和下一行
	- 10dd 删除当前行开始的共10行
	- D 删除当前字符至行尾

+ 跳转
	- gg 跳转到文件头
	- G 跳转到文件尾
	- gg=G自动缩进 （非常有用）
	- Ctrl + d 向下滚动半屏
	- Ctrl + u 向上滚动半屏
	- Ctrl + f 向下滚动一屏
	- Ctrl + b 向上滚动一屏
+ 冒号+行号，跳转到指定行；比如:120，跳转到120行；
	- $ 跳转到行尾
	- 0 跳转到行首

+ 编辑

	- u 撤销
	- Ctrl + r 重做
	- yy 复制当前行
按v（逐字）或V（逐行）进入可视模式，然后用jklh命令移动即可选择某些行或字符，再按y即可复制任意部分
	- p 粘贴在当前位置
另外，删除在vim里面就是剪切的意思，所以dd就是剪切当前行，可以用v或V选择特定部分再按d就是任意剪切了

+ 查找
	- /text　　查找text，按n健查找下一个，按N健查找前一个
	- ?text　　查找text，反向查找，按n健查找下一个，按N健查找前一个
	- :set ignorecase　　忽略大小写的查找
	- :set noignorecase　　不忽略大小写的查找
+ 替换
	- :s/old/new/ 用old替换new，替换当前行的第一个匹配
	- :s/old/new/g 用old替换new，替换当前行的所有匹配
	- :%s/old/new/ 用old替换new，替换所有行的第一个匹配
	- :%s/old/new/g 用old替换new，替换整个文件的所有匹配
	*也可以用v或V选择指定行，然后执行*

+ 多文件操作
	- vim file1 file2 file3 ... 同时编辑多个文件
	- :split 将窗口分成上下两个子窗口，对应两个不同的文件
	- :vsplit 将窗口分成左右两个子窗口，对应两个不同的文件
	- :open file4 打开新文件
	- :bn 切换到下一个文件（当前窗口）
	- :bp 切换到上一个文件（当前窗口）
	- Ctrl-w h    移动到窗口左边
	- Ctrl-w j    移动到窗口下边
	- Ctrl-w k    移动到窗口上边
	- Ctrl-w l    移动到窗口右边




# Apache

Start Apache2 Server # 启动apache服务

Restart Apache2 Server # 重启apache服务

Stop Apache2 Server # 停止apache服务




# Mysql

mysql -h主机地址 -u用户名 -p用户密码 # 连接数据库

mysqladmin -u用户名 -p旧密码 password 新密码 # 修改密码

create database [db_name];

drop database <数据库名>;

select <字段1，字段2，...> from < 表名 > where < 表达式 >

insert into <表名> [( <字段名1>[,..<字段名n > ])] values ( 值1 )[, ( 值n )]

create table <表名> ( <字段名1> <类型1> [,..<字段名n> <类型n>]);

drop table <表名>

use <数据库名>;

rename table 原表名 to 新表名;

update 表名 set 字段=新值,… where 条件

alter table 表名 add字段 类型 其他;

// 导出整个数据库
mysqldump -u 用户名 -p 数据库名 > 导出的文件名
mysqldump -u user_name -p123456 database_name > outfile_name.sql


//导出一个表
mysqldump -u 用户名 -p 数据库名 表名> 导出的文件名
mysqldump -u user_name -p database_name table_name > outfile_name.sql

// 导出一个数据库结构
mysqldump -u user_name -p -d –add-drop-table database_name > outfile_name.sql
-d 没有数据 –add-drop-table 在每个create语句之前增加一个drop table

// 带语言参数导出
mysqldump -uroot -p –default-character-set=latin1 –set-charset=gbk –skip-opt database_name > outfile_name.sql


# Php

php -f  test.php (-f 可省略) # 执行

php -r "echo 'code'"  # 直接执行

php -m  内置及Zend加载的模块

php -i  等价于 phpinfo() 

php -i | grep php.ini 查看php配置文件加载路径 
php –ini 同上

php -v 查看php版本 
php –version 同上

php –re 查看是否安装相应的扩展 如 php –re gd


# Npm


npm -v          # 显示版本，检查npm 是否正确安装

npm install express   # 安装express模块 

npm install -g express  # 全局安装express模块 

npm list         # 列出已安装模块 

npm show express     # 显示模块详情 

npm update        # 升级当前目录下的项目的所有模块 

npm update express    # 升级当前目录下的项目的指定模块 

npm update -g express  # 升级全局安装的express模块 

npm uninstall express  # 删除指定的模块

npm init 	# 在此目录生成package.json文件，可以添加-y | --yes 参数则默认所有配置为默认yes

npm install [package] -g # 全局安装

npm install --production # 安装dependencies，不包含devDependencies

npm install [package] # 默认使用–save 参数，如果不想保存到package.json中，可以添加--no-save参数；还可以指定–save-dev 或 -g参数

npm uninstall [package] # 卸载依赖包， 默认使用–save参数，即从package.json中移除

npm ls [-g] [--depth=0] # 查看当前目录或全局的依赖包，可指定层级为0

npm outdated 查看当前过期依赖，其中current显示当前安装版本，latest显示依赖包的最新版本，wanted显示我们可以升级到可以不破坏当前代码的版本

npm root -g # 查看全局安装地址

npm update [package] # 升级依赖包版本

npm ll[la] [--depth=0] # 查看依赖包信息

npm list [package] 	 # 查看依赖的当前版本

npm search [string] # 查找包含该字符串的依赖包

npm view [package] [field] [--json] # 列出依赖信息，包括历史版本，可以指定field来查看某个具体信息，比如（versions) 可以添加–json参数输出全部结果

npm home [package] # 在浏览器端查看项目（项目主页）

npm repo [package] # 浏览器端打开项目地址（GitHub）

npm docs [packge] # 查看项目文档

npm bugs [packge] # 查看项目bug

npm prune # 移除当前不在package.json中但是存在node_modules中的依赖


# Git

git init # 初始化本地git环境

git clone XXX# 克隆一份代码到本地仓库

git pull # 把远程库的代码更新到工作台

git pull --rebase origin master # 强制把远程库的代码跟新到当前分支上面

git fetch # 把远程库的代码更新到本地库

git add . # 把本地的修改加到stage中

git commit -m 'comments here' # 把stage中的修改提交到本地库

git push -u origin master # 把本地库的修改提交到远程库中

git status # 查看当前分支有哪些修改

git reset --hard HEAD # 撤销本地修改





alias subl=\''/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl'\'
sudo ln -s "/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl"



npm install -g parcel-bundler # 安装 parcel 脚手架


SLMGR -REARM # 此 windows 7副本不是正版 命令行

// 将当前目录下所有的 mysql 库 导出到指定路径下
mysql/bin$ mysqldump -uroot -p --all-databases > [指定路径];

// 导入指定路径到当前 mysql 库中
mysql> sourced:\[指定路径];

// 让表 id 重新从 1 开始
TRUNCATE TABLE table;

// 因为 Mysql 一次只能显示 1000 条数据
// 如果数据大于了 1000 条，则看不到
// 该命令可以看出所有的数据条数
`select count(*) from 表名;`

// 利用 FB 官方的脚手架直接生产 react 的工作目录结构
npx create-react-app [项目名]

// 解决提交时输入密码
git config --gloabl credential.helper store