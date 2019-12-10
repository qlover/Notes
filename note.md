laravel 5.5 多字段连接条件在 on 中传入多给数组

+ 403 Forbideen
https://blog.csdn.net/qiujin_zebra/article/details/70050562

+ 419  unknown status bootstrap fileinput 419

laravel 模板文件加上 `<meta name="csrf-token" content="{{ csrf_token() }}">`
ajax 加上请求头 'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')

*tip: 注意在上传文件时将 fileinfo 扩展打开*

+ git push
```
Enumerating objects: 17, done.
Counting objects: 100% (17/17), done.
Delta compression using up to 12 threads
Compressing objects: 100% (9/9), done.
Writing objects: 100% (9/9), 948 bytes | 316.00 KiB/s, done.
Total 9 (delta 5), reused 0 (delta 0)
remote: error: insufficient permission for adding an object to repository databa
se ./objects
remote: fatal: failed to write object
error: remote unpack failed: unpack-objects abnormal exit
To 120.27.8.86:/git/Tfotrod.git
 ! [remote rejected] master -> master (unpacker error)
error: failed to push some refs to 'git@120.27.8.86:/git/Tfotrod.git'
```
服务器器上  chmod -R 777 /git/Tfotrod.git/

https://blog.csdn.net/willba/article/details/80361850


+ -bash composer: command not found
```
mv composer.phar /usr/bin/composer
```
`GET`方法请求一个指定资源的表示形式. 使用GET的请求应该只被用于获取数据.
`HEAD`方法请求一个与GET请求的响应相同的响应，但没有响应体.
`POST`方法用于将实体提交到指定的资源，通常导致在服务器上的状态变化或副作用. 
`PUT`方法用请求有效载荷替换目标资源的所有当前表示。
`DELETE`方法删除指定的资源。
`CONNECT`方法建立一个到由目标资源标识的服务器的隧道。
`OPTIONS`方法用于描述目标资源的通信选项。
`TRACE`方法沿着到目标资源的路径执行一个消息环回测试。
`PATCH`方法用于对资源应用部分修改。




























# Note

1、服务器代码合并本地代码
git stash     //暂存当前正在进行的工作。
git pull   origin master //拉取服务器的代码
git stash pop //合并暂存的代码

2、服务器代码覆盖本地代码
git reset --hard [commit_id] //回滚到上一个版本
git pull origin master

上一次拉取
git reset --hard FETCH_HEAD


1. 使用 Illuminate\Support\Facades\Request
	input() 方法报错
2. laravel 模型 get() 方法获取得是一个数组

3. 依赖注入

4. VUE requerContext()


vue-better-calnder Vue 日历



/my VIP 去掉
/菜单图标 
头像同步 
/ backtop

阅读



strtr 字符串替换可以传入数组
## Vue 父组件传递异步数据给子组件
子组件可以用 watch 监听该数据

## SQL 优化

### 索引优化

如果一个表的字段经常作为 where 条件判断
而可以将该字段作为一个索引字段
增加查询效率

### 查询优化

查询一条记录可使用 limit 1



## laravel 加解密
```php
use Illuminate\Support\Facades\Crypt;
Crypt::encrypt($request->input("pwd")),
Crypt::decrypt($request->input("pwd")),
```

子父组件传值



## css 浮动

```html
<img src="" alt="" style="float:left;">
<div style="overflow:hidden;">xxxxxxxxxxx</div>
```

## 垂直居中

## VUEX 页面刷新后状态失效
1. VUEX + cookies + localStore 
2. vuex-persistedstate

## vue 阻止事件默认行为

## $().serialize() 序列化，作为 AJAX 传递数据

## laravel 
app/Http/Middleware/VerifyCsrfToken.php 可以添加 except 规则禁用 post csrf 检查


## php 一天时间范围内

```php
$today_start = strtotime(date('Y-m-d 00:00:00'));
$today_end = strtotime(date('Y-m-d 23:59:59'));
```

## 连续登录计算

准备签到
	prev == today
		已经签到	
	签到
		prev == yestrday 
			签到，并且登录+1，连续+1
		签到+1，连续 1

## 地址跳转
location.pathname 会产生历史栈
location.replace() 不会产生历史栈

## vue-infinite-scroll
需要先安装
npm i vue-infinite-scroll -D

## 滚动底部懒加载！！！

1. 使用 `v-infinite-scroll` 指令, 需要导入 `vue-infinite-scroll`

## laravel Cookie

Cookie::queue 设置或删除 Cookie


## 公众号模板消息

1. 模板ID
	公众平台添加模板得到ID: vCAN5HP2q6VuNnENmnCnm3Q0irt4XEI7S5DpnrKDnRI
2. OPENID
	发送给谁

	Qlover871: oR0Qz5mT-vjfqcX0LKDu1aX95cAA
	清风: oR0Qz5m0y_NjNbNw98fuBXZE9qt8
3. access_token
	2个小时有效期,
	可用 redis 保存，并设置过期时间为2h
	api(get): https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET
	appid: 开发者ID, wx0aab5758c65d1762
	secret: 开发者密码, 3d2796810fc445f2299f13b08ad5e7e2

##　stream_context_create

## input autocomplete 可以取消输入框默认填充功能

php 一种可以代替请求的方法

## Rides TTl

Rides::expire(key, ttl) # ttl 毫秒

## router-link 点击事件

@click.native = "handle"

## 图片上传

Storage root 配置就是保存的路径

## 横屏滚动不出现流动条

使用 `-webkit-scrollbar` 伪元素,将滚动条设置为 `display:none;` 将会隐藏掉默认的滚动条

*仅仅在支持WebKit的浏览器 (例如, 谷歌Chrome, 苹果Safari)可以使用.*

## 扫码登录

https://open.weixin.qq.com/connect/qrconnect?appid=wxbdc5610cc59c1631&redirect_uri=https%3A%2F%2Fpassport.yhd.com%2Fwechat%2Fcallback.do&response_type=code&scope=snsapi_login&state=4881c1e62a7fc9405b6fb8e3a7b0ad6f#wechat_redirect



##　滚动到顶部

1. scroll-behavior: smooth;
2. scrollTo API
3. requestAnimationFrame

```javascript
getScrollTop(){
  return window.pageYOffset ||
    document.documentElement.scrollTop ||
    document.body.scrollTop;
}
handleToTop(timestamp) {
  const scrollTop = getScrollTop()
  if (scrollTop > 0) {
    interval = requestAnimationFrame(handleToTop);
    window.scrollTo(0, scrollTop - scrollTop / 8);
  } else {
    cancelAnimationFrame(interval)
  }
}
```

## VUE 驼峰属性传值时属性中间短线分开

## php: http_build_query() 构建请求字符串

## SVG 标签引入 svg

记得给 svg 文件加上 `xmlns="http://www.w3.org/2000/svg"`

`<svg><use xlink:href="/images/icon-pay-month.svg#icon-pay-month"></use></svg>`

## CRP

## 一个数字控制下标包括负数

```javascript
Array.prototype.get = function( num ) {
	return num < 0 ?
		this[ ( num % this.length ) + this.length ] :
		this[ num ];
}
```
## PHP 二维数组按某个字段值排序
```php
$fans_totals = array_column($result, 'xxx_column');
array_multisort($fans_totals, SORT_DESC, $result);
```

## PHP round() 可保留小数

## PHP time() + 3600 * 8 获取标准时间

## laravel 脚手架构建时需要打开一个 .env 文件

再使用 php artisan key:generate 进行加密方式选择

## 国际化 ngx-translate

npm install @ngx-translate/core --save
npm install @ngx-translate/http-loader --save

https://tc9011.com/2017/12/16/angular%E4%B8%ADngx-translate%E4%BD%BF%E7%94%A8%E7%AE%80%E4%BB%8B/





status
customerid
sdpayno
sdorderno
total_fee
paytype
remark
sign



http://local.php.com/api/pay/test?user_id=2&pay_coin=100&pay_money=10&pay_type=4&recharge_id=20

http://api.ruishengdzkj.com/newCallback/100031/zhifubaonew/alipaywap.htm?charset=utf-8&out_trade_no=201907111113115286310003127&method=alipay.trade.wap.pay.return&total_amount=10.00&sign=n0lgTbAon5q%2BokkEsZf5xTLTkSuxTFH0T9FAXYQJgv6bQKEowaFA5hFVdbx7KZEoHFXIHE%2FkEc5t76kWUtyNyJGFDmehYkfI%2FsAechNHFXQgksbgvYZ99Cn9ThOE%2BEE%2BxGEBHyKma7ai%2BEoGmKNiJNJ91EnDeyyI01Nroj4dSk8EByFfXU%2FZbn2hFNVW7%2FYXOX%2Fu574G0SI9HKoJczVKj3y8h3livuWH7t%2FdXuOQO9mwKSd%2FlTgAr290VJcF01AaqC2b2vykx9%2FvPgpKAEbheLEL7Dhn3pPsFPsBUHF4oywRff8i0JYW4Y8mQrK6%2FDFgbpOH1O4%2FdmvFr69O%2BS%2BygQ%3D%3D&trade_no=2019071122001412591041532221&auth_app_id=2019022563345192&version=1.0&app_id=2019022563345192&sign_type=RSA2&seller_id=2088331965198458&timestamp=2019-07-11+11%3A13%3A51




# shadow socks

http://www.wangchao.info/1148.html
http://www.wangchao.info/1316.html

systemctl start firewalld # 开启防火墙
systemctl stop firewalld # 关闭
sudo sslocal -c /etc/shadowsocks.json -d stop
sudo sslocal -c /etc/shadowsocks.json -d start


## shadowsocks install
```
wget --no-check-certificate -O shadowsocks.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
chmod +x shadowsocks.sh
./shadowsocks.sh 2>&1 | tee shadowsocks.log
```

./shadowsocks.sh uninstall
/etc/init.d/shadowsocks start
/etc/init.d/shadowsocks stop
/etc/init.d/shadowsocks restart
/etc/init.d/shadowsocks status

{
    "server":"34.92.173.155",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"q12345",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}

https://blog.csdn.net/lch1251680944/article/details/87975532

WantedBy=multi-user.target


## pypi

pip install shadowsocks
ssserver -p 8838 -k q12345 -m aes-256-cfb


https://www.runoob.com/w3cnote/html-css-guide.html#css
https://www.w3cschool.cn/css/css-selector.html
https://blog.csdn.net/m0_37972557/article/details/80370822
https://segmentfault.com/a/1190000018717319
https://www.w3.org/TR/css-cascade-4/#cascading






# TypeScript







# Angular 

先加载 polyfills.ts

引入服务并在构造器中注入

@childView() 获取 DOM

ngAfterViewInit() DOM加载完成生命周期

@Input() 获取父组件属性

@angular/router ActiveRoute 可以获取路由信息
@angular/router NavigationExtras 可 get 传值跳转路由

router-outlet 相当于 router-view

路由懒加载可以解决在根组件中引入组件的问题

## Web Components

## 子组件给父组件发送数据

1. 子组件引入 Output, EventEmitter
2. 利用 @Output() 指令 new 一个 	EventEmitter() 实例
3. 用 EventEmitter().emit() 方法
4. 父组件使用 (outer) 属性


## 生命周期

1、ngOnChanges - 当数据绑定输入属性的值发生变化时调用
2、ngOnInit - 在第一次 ngOnChanges 后调用
3、ngDoCheck - 自定义的方法，用于检测和处理值的改变
4、ngAfterContentInit - 在组件内容初始化之后调用
5、ngAfterContentChecked - 组件每次检查内容时调用
6、ngAfterViewInit - 组件相应的视图初始化之后调用
7、ngAfterViewChecked - 组件每次检查视图时调用
8、ngOnDestroy - 指令销毁前调用

## RxJS




## 层叠上下文

文档中的层叠上下文由满足以下任意一个条件的元素形成：

- 根元素 (HTML),
- z-index 值不为 "auto"的 绝对/相对定位，
- 一个 z-index 值不为 "auto"的 flex 项目 (flex item)，即：父元素 display: flex|inline-flex，
- opacity 属性值小于 1 的元素
- transform 属性值不为 "none"的元素，
- mix-blend-mode 属性值不为 "normal"的元素，
- filter值不为“none”的元素，
- perspective值不为“none”的元素，
- isolation 属性被设置为 "isolate"的元素，
- position: fixed
- 在 will-change 中指定了任意 CSS 属性，即便你没有直接指定这些属性的值（参考 这篇文章）
- -webkit-overflow-scrolling 属性被设置 "touch"的元素


## 基线


https://www.w3.org/TR/CSS2/visuren.html#normal-flow
https://segmentfault.com/a/1190000017273573



游城十代: KENN
万丈目准: 松野太紀
天上院明日香: 小林沙苗
丸藤亮: 前田剛
丸藤翔: 鈴木真仁
三泽大地: 增田裕生
天上院吹雪: 遊佐浩二
爱德·菲尼克斯: 石田彰
斋王琢磨: 子安武人
约翰: 入绘加..


武藤游戏/暗游戏: 風間俊介
海马濑人/赛特: 津田健次郎
城之内克也: 高橋広樹
真崎杏子: 齊藤真紀
本田ヒロト: 近藤孝行
貘良了: 井上瑶、松本梨香
海马圭平: 竹内顺子
海马乃亚: 横山智佐

不动游星: 宮下雄也
杰克·阿特拉斯: 星野貴紀
克罗·霍根: 浅沼晋太郎
十六夜秋: 木下あゆ美
龙亚: 洞内愛
龙可: 寺崎裕香
牛尾哲: 落合弘治


Iseedeadpeople = 地图全开
全开后地图上的山洞什么的一眼就能看到
1 死亡领主皇冠--第一章，埃克岛，前往巨魔基地途中打一只11级的深海幽灵
2 先祖权杖--第一章，埃克岛，完成打船任务
3 远古战斧--第一章，埃克岛，点火任务中，打一只12级的海怪
4 德雷克萨尔魔法书--第一章，雷霆山谷，完成帮德雷克萨尔找龙蛋，及第二次进入雷霆山谷
5 瑟拉思尔--第二章，完成游说食人魔的任务
6 燃灵之链--第二章，中心神殿（在大地图中间有入口），打到最后一只怪，便可得到
7 泰兰德之颅--第二章，中上山洞，打BOOS所得
8 死亡领主护盾--第二章，在大地图右下角有往上一点，打一个20级的怪
9 奥格里玛旗帜--第二章，前往风暴港湾途中，遇到萨尔，完成他的可选任务
10 灵魂之球--第二章，上述任务的前一任务，完成后，从基地巫医处得到
11 阿特利库斯力量指环--第三章，任务“最后的海军上校”中，在右边的商店中买到
12 XXXX--同上，增加400生命和魔法
特有的
1血羽之心--第一章，增加10点敏捷 完成那滋盖尔的任务（杀死血羽）获得
2雷霆蜥蜴钻石--第一章，杀死雷霆山脉最后一只蜥蜴可得雷霆蜥蜴钻石，用来释放闪电链
3萨满利爪--第一章
4荣誉护盾--第二章 极品，绝不比神器差



1.iseedeadpeople - 打开全部地图

　　2.whosyourdaddy -神化模式(无敌+攻击加1000倍)

　　3.greedisgood # 增加金子和木头(#任填默认500)

　　这三个最有用，其他可以忽略

　　1.allyourbasearebelongtous - 直接胜利

　　2.somebodysetupusthebomb - 直接失败

　　3.thereisnospoon - 无限魔法

　　4.strengthandhonor - 不能被判定失败(即使达到任务失败条件)

　　5.itvexesme - 不能胜利的模式

　　6.greedisgood - +500的金子和木头


1死亡领主皇冠--第一章，埃克岛，10级死亡领主
2先祖权杖--第一章，埃克岛，完成任务
3远古战斧--第一章，埃克岛，12级海怪
4德雷克萨尔魔法书--第一章，雷霆山谷，完成龙蛋任务
5瑟拉思尔--第二章，食人魔任务
6燃烧精灵之链--第二章，神殿中最后一只怪
7泰兰德之颅--第二章，山洞，大骷髅（10000点血，好BT）
8死亡领主护盾--第二章，右下角，20级死亡领主（10000点血，1000点魔，回血技能，更BT）
9灵魂之球--第二章，完成任务
10奥格里玛旗帜--第二章，去风暴港湾，找到萨尔
11阿特利库斯力量指环--第三章，右边商店
12泰坦之石链--左边商店

https://www.2048hd1.com/video/play/id/1851.html

