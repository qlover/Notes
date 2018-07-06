++++++++++++++++
# 路由交换 #
++++++++++++++++

+ int v 进入VLAN
+ int f 进入端口
+ int s 进入路由器
+ ip default-getway 	# 配置网关
+ line console 0 		# 是因为console 端口只有一个
+ line vty [0 15] 		# 因为端口不只一个，所以可以多写

*每台 Cisco 网络设备两个配置文件*

*(运行配置文件(running-configuration[RAM])-用于设备的当前 工作过程中*
*启动配置文件(startup-configuration[ROM])-用作备份配置，在设备启动时加载*

`.bin 存放在 flash 区域`

*交换机 VLAN 和 端口都需要  no shutdown*

+ 用户执行
+ 特权执行
+ 全局配置



#### 错误消息 ####
+ 不明确
+ 不完整
+ 不正确

`switch> // 用户模式`

`switch# // 特权模式`

`switch(config) // 全局配置`



### 口令 ###



#### 使能口令，控制台口令
`$ line console 0`
#### 使能加密口令
`$ enable secret  # 与 enable password 都可以设置，但是只能用 secret `
#### VTY 口令
`$ line vty 0 15`

`$ banner motd "welcome to this " 	# 设置登录欢迎标语	`
 
`copy running-config startup-config  # 存盘配置信息`

`service password-encryption 		# 对明文密码做简单加密`


## 路由命令 ##

	(config)# int vlan x 					# 进入 VLAN 端口

	(config)# int f 0/x  					# 进入 交换机 端口，并且这两个端口要 no shutdown 

	(config)# int s x 	 					# 进入 路由器 端口

	(config)# int g 0/1 					# 进入 千兆 端口

	(config)# int range f 0/1-5 			# 进入属性相同的一组端口

	(config)# int loopback 					# 进入回环口

	(config)# line vty  					# 远程登录密码

	(config)# line console  				# 控制台口令

	(config)# enable password 				# 键入保护密码，使能加密口令

	(config)# ip default-getway 			# 为交换机配置网关

	(config)# service password-encryption 	# 对密码加密

	# banner motd 'welcome' 				# 设置标语

	(config)# service password-encryption 	# 对明文密码做简单加密

	# copy running-config starup-config 	# 将操作过的命令存盘 

	(config-if)# history size Nubmer 		# 查看线路历史输入过的记录条数 

### copy A B  ###			# 从 A 复制到 B
	# copy running-config startup-config 	# 将操作过的命令存盘 
	# copy running-config  tftp:  			# 将操作过的命令保存在目的 

	(config-if)# password 123 				# 输入密码
	(config-if)# login 						# 记得输入密码后要能够登录

### ssh ###
	(config)# ip domain-name s.com 			# 
	(config)# crypto key generate rsa 		# 启用 ssh 协议，之后选择模数
	(config)# username xxx password 123 	# 创建用户和密码用于 ssh

+ 进入虚拟线路加载协议 
	
	(config)# line vty 0 15 				# 进入虚拟线路
	(config-if)# transport intput ssh 		# 加载 ssh 协议 
	(config-if)# login local 				# 进入本地数据库验证

+ 登录 SSH

	# ssh -l 用户名 设备地址 


## 端口安全 ##
* 交换机与终端设置(二层交换机，三层交换机，路由器)之间用 trunk  *

	(config-if)# switchport port-security 						# 打开端口安全
	(config-if)# switchport mode access 						# 将接口模式设置为接口模式
	(config-if)# switchport port-security 						# 开启端口安全功能
		# 有三种，一是 mac绑定，二是限制，三是违例管理
		(config-if)# sw port-s mac-address MAC地址 				# 手动绑定
			(config-if)# sw port-s mac-address stiky 			# 粘滞绑定
		(config-if)# sw port-s maximun Number 					# 限制最大可以有多少个 MAC 地址
		(config-if)# sw port-s violation shutdown 				#违规后直接  DOWM 掉 	


### 更改加载启动文件 ###
	(config)# boot system flash:XXX 			# XXX 文件名


###　更改时钟为当前时间 ###
	# clock set hh:mm:ss day month year 		# 将时钟配置为当前时间
	(config-if)# clock rate 64000 				# 设置时钟频率

### 如果双工的速率不匹配，则需要更改速率   ###
	(config-if)# duples full 
	(config-if)# speed 100 						# 将两端的端口设置相同的速率 


##########
## VLAN ##
##########

VLAN 1 可以充当四个不同的 VLAN ，
在不同的设备中当中，传输过程中需要 将两台 SW 做 Trunk
而 SW 两端的端口默认属于 VLAN 1 ,因为 VLAN 1 会默认将它设为 本征 VLAN 
当然，如果不指定特定的 本征 VLAN 那么默认用的是 万能 VLAN 1

`(config-if)# sw native VLAN XX 			# 将VLAN XX 做为 本征VLAN 用来 Trunk 传输`

### 将 VLAN 分配给端口 ###
	(config-if)# sw mode acc 					# 将端口模式设置为接口模式
	(config-if)# sw acc vlan Number 			# 将 vlan 号


### 进入 VLAN 配置模式方法一 ###
	# vlan database   							# 进入 VLAN 配置模式
	(vlan)# vlan Number [name String] 			# 进入  VLAN ，可给 VLAN 添加 名字

### 进入 VLAN 配置模式方法二 ###
	(coinfig-vlan)# vlan Number 				# 新建 VLAN 
	(coinfig-vlan)# name String 				# 为 VLAN 添加名字

### 中继，也就是 trunk ###

+ 连接主机用 access 模式
+ 连接中间设备用 trunk 模式

用于交换机与交换机相连接时 VLAN 不通时给相连的交换机作 trunk ，并允许那个 vlan 可以通过
	(config-if)# sw mo tr 								# 将端口作为 trunk
	(config-if)# sw tr all vlan Number[,Number ] 		# 允许那个 vlan 可以流量经过

`# delete valn.dat 								# 删除 VLAN 配置`


--------------
## VTP 协议 ##

* 用于管理多台交换机之间命令执行 *


###  该模式用于 trunk 端口交换机 ###
	(config)# vtp domain [域名] 				# 设置同网络中的同域名
	(config)# vtp mode server 					# 设置 vtp 模式
	(config)# vtp password 123					# 设置 vtp 密码

`(config)# no ip domain-lookup  				# 禁用 DNS 查询`



### 三层交换机与二层相连  ###

三层交换机需要于端口连接的二层交换机 vlan 配置一样,
并且三层交换机的每个 vlan 可以给 ip,
该 ip 也就是二层交换机下端口的网关
	
`(config)#　ip routing 							# 启用三层交换机路由功能`

--------------
## 单臂路由 ##

`R -> S1 -> pc `

在这个 拓扑 图中，称为 单臂路由，实现不同 VLAN 中的 PC 通信
单臂路由中 S1 的 PC 接口为 VLAN 接入 VLAN 接口
而 R1 上的 S1 接口则为整个网络的网络
而每个网络上的子接口则为每一个 S1 下的 VLAN 的网关
** 子接口编号的地址是 VLAN 编号地址的网关  **

	(config)# int f 0/0.11 							# 进入路由器接口下的子接口
	(config-subif)# ip adder xxx.xxx  				# 给路由子接口配置 Ip
	(config-subif)# encapsulation dot1Q	[VLAN ID]	# 封装 ieee 协议
			# 如果为本征 VLAN 则最后要要加上 native 


## 静态路由 ##

+ 当一个包从这一端网关到不了另一端后，可能是因为两端不在同个网段
+ 此时就需要在路由表上指向一条路到另一端，所以就可以用静态路由也就是手动给
+ 这里用默认路由，默认路由是静态路由的特殊路由
+ 直连与非直连
+ 1.静态路由 2.下一跳地址（出口编号 ）


(config)# ip route  0.0.0.0  0.0.0.0   1.1.1.2		# 给默认路由
		  ip route 目的网络  子网掩码 下一跳地址(出口编号)

* 如果方向只有一个方向就首选默认路由 *
* 三层交换机需要先 acc 才能 trunck *

## 端口的三种角色 ##
1.根端口
2.阻塞端口
3.指定端口  # 根桥上的所有端口是指定端口


`(coinfig)# spannin-tree mode R 			# 生成树模式`

### 生成树 SPT ###

	(config)# spanning-tree vlan Number p		# 更改VLAN 的优先级
	(config)# spanning-tree vlan Number m 		# 更改 VLAN 的MAC 大小
	(config)# spanning-tree vlan Number root  	# 选择根桥
			(config)# spanning-tree vlan Number root primary 		# 主根桥
			(config)# spanning-tree vlan Number root secondary		# 备用根桥

### DHCP 四个工作原理 ####
优先级越小就作为根桥，阻塞大的
MAC 地址越小就作为根桥,

### PVST ###
思科保留协议，私有协议



## DHCP ##
用  255.255.255.255 去找服务器，服务器单播寻 mac 地址到源主机，最后源主机以广播发送数据到所有 DHCP 服务器
带有路由功能的设备可以充当 DHCP 服务器

### 配置 DHCP ##
	(config)# ip dhcp excluded-address 地址 - 地址 			# 新建地址库，这个地址是连续的,并且这里给的地址是会排除的
	(config)# ip dhcp pool v [Vlan ID]						# 进入 VLAN ID 号的地址库
	(dhcp-config)# network 网络地址 子网掩码			   	# 确定 DHCP 自动分配的网络大小，以 掩码区分 
	(dhcp-config)# default-router 	IP 地址					# 给 DHCP 默认网关
	(dhcp-config)# dns-server 0.0.0.0						# 给 DNS 地址

### DHCP 中继 ###
在于 DHCP 服务器间单播帧丢掉则需要用 DHCP 中继将这个单播帧转发出去

	(config)# int valn Number 					  # 进入需要转发的 vlan 通道
	(config-if)# ip helper-adderss 服务器地址	  # 需要转发到的服务器地址

每个 vlan 地址作为每个 dhcp 对应 vlan 的地址池的网关
三层交换机接口地址是路由器下一跳的地址，同样路由器下接口地址是三层交换机下一跳的地址
路由器的接口地址不能在同一个网段

### NAT(网络地址转换) 扩展网络 ###


### IPV6 ###

------------
## 无线网 ##

RIP IGRP EIGRP OSPF		# 动态路由协议

度量越小路由越优先

VPN FRP 协议可以让两个不同的私有局域网相互通信


## 动态路由 ##

+ 动态分享两个路由之间的信息
+ 当拓扑改变时自动更新
+ 确定到

组播：所有路由器只有参与的路由器才能接收到
广播：所有路由器都可以接收到，即使是没有参与的

### 路由协议的内容  ###
+ 数据结构
+ 算法 
+ 路由协议通告

### 动态路由配置  ###
* 将自己的直连路由 network(通告) 就行了 *



1. 内部网关协议 (IGP)
	+ 距离失量协议
		+ RIPV1
		+ RIPV2
		+ IGRP
		+ EIGRP
	+ 连接状态协议
		+ OFSP
		+ IS-IS
2. 外部网关协议  (EGP)
	+ BGP



度量：是由路由协议用来分配到达远程网络的路由开销，来用选路
管理距离：用来选协议
接口上的：开销，带宽，延迟，负载，可靠性，跳数 可用来做度量参数

+ RIP  			- 跳数
+ IGRP & EIGRP 	- 带宽，延迟，负载，可靠性
+ IS-IS & OSPF 	- 开销 (CISCO 采用 OSPF 使用的是带宽)


负载均衡：数据分析会使用所有数据开销相等的路径

### 管理 AD ###
路由来源		管理距离 
粗连			0
* 静态			1
EIGRP 总结路由	5
外部BGP			20
* 内部 EIGRP	90
IGRP 			100
* OSPF 			110
is-is 			115
* RIP			120
* 外部 EIGRP 	170
内部 BGP		200
`show ip protoclos    # 查看网络协议 `


直连网络默认管理距离 	0
默认管理距离是 			1
造价路由条目最多支持 4 条


### 距离失量协议  ###
+ RIP	路由信息协议： 最大跳数 15 
+ IGRP  内部网关协议
+ EIGRP 增强型
	部分更新
	拓扑改变触发更新
	局限
	不定期

计时器：
更新，无效，抑制，清除

rip1 只支持有类地址
rip2 可支持无类地址


`debug ip router		# 实现重新更新路由条目过程`



(config)# int lo[Number] 		# 进入回环端口


(config)# router rip 			# 启动 RIP 动态路由协议
(config-router)# version 1 				# 版本号，1 版本不支持变掩码，，默认 1
(config-router)# network 直接网段Ip 	# 通告直接网段

(config)# router rip				
(config-router)# version 2			# 使用版本2
(config-router)# no auto-summary  	# 关闭自动汇总
(config-router)# network IP 		# 通告直连网段
(config-router)# passive-int 		# 在没有路由表的接口下要禁用更新路由条目

### 静态注入 ###
由于有多个路由器，出口路由有静态，但是里面的路由器没有这个条静态的信息，所以要注入

(config-router)# redistribute static			# 将 静态 注入到 rip
(config-router)# default-infomation originate	# 将 静态 注入到 rip，但 ospf 只有用这个

当两个同级的路由器，并且两个路由器可知的条目不一样就需要做【路由重分布】，在出口路由
## 链路状态路由  ##
最短路径优先协议
+ OSPF  开放最短路径优先
+ IS-IS 中间系统到中间系统



### 路由表 [value1, value2] 的两个值 ###
value1 		# 优先级
value2		# 度量值，是带宽值 / 开销值得到的结果


链路状态数据库的内存要求
SPF 算法的 CPU 

点对点 10 S
多路 30 S
共同的死亡时间是自身 4 倍


## OSPF ##

(config)# route ospf [process-id]  # 启用 OSFP 协议，1-65535 之间的数字，由网络管理员选定
	# 仅在本地有效，

(config)# router-id [id]		# 指定 路由 ID

# 有三种方式给 Route-id

(config)# network [网络地址] [mask] area [area-id]  #
	# mask: 		可用子网掩码，也可以用通配符掩码
	# 通配符掩码：	网络地址和通配符掩码一起,用 掩码 减去 IP 地址
	# 用于指定此 network 命令启用接口或接口范围
	# area: OSPF 区域是共享 0 


1. 使用通过 OSPF router-id 命令配置的 ip 地址
2. 如果未配置 router-id ，则路由器会选择其所有回环的最高 Ip 地址
3. 如果未配置一环回接口，则路由器会选择其所有物理接口的最高活动 ip 地址
* router-id 先从 router-id 选择->环回接口->物理接口   *

`(config)# show ip protoclos 		# 难路由器 ID `

### 修改路由器 ID 必须通过重新加载路由器或使用以下命令   ###
`# clear ip ospf process  			# 清除路由器原来的 router-id 配置`

(config-if)# bandwidth [number] kb 		# 修改拓扑中串行接口开销

(config-if)# ip ospf cost [number] 		# 直接修改开销

(config-router)# auto-cost reference-bandwidth [number]  # 命令调整参考带宽值

`# clear ip ospf process 		# 修改路由器ID需用这个命令`

### hello 和 dead 间隔 ###

hello 间隔和Dead 间隔分别更改为 5 秒和 20 秒。

# 在接口中
(config-if)# ip ospf hello-interval		# 设置 hello 间隔

(config-if)# ip ospf dead-interval		# 设置 dead 间隔

(config-if)#ip ospf priority 255 		# 设置优先级
	255: 最高级
	0: 不参与
	1：默认



## EIGRP ##

单播/组播 用 224.0.0.10



(config-if)# network [ip-address] [mark]  		# ERIGP 通告网段
	mark 通配符掩码 : 8x 可以不用带除了 32,其它位的必须带上掩码
* ERIGP 在有类边界会自动汇总 *

如果启用了自动汇总 并且 至少有一个通过 ERIGP 获知的子网,就会出现 ERIGP 的NULL0 ,空接口


### 路由重分布 ###
(config-router)# redistribute ospf 1 metric [复合参数] 		# rip 路由重分布
(config-router)# redistribute eigrip 1 metric [复合参数] 	# rip 路由重分布
(config-router)# redistribute rip subnets  					# ospf 路由重分布




## ACL ##
如果只有允许条件，则默认会有拒绝条件，且不能写拒绝条目
如果先有拒绝条件，则需要将允许条件补充在后面

不同协议，不同方向，不同接口，不能用同一个编号 

`(config)# access-list [规则编号] [ deny | permit ] [SThing] [网段 | ip mark] [host]`

(config)# access-list 100 deny icmp 192.168.1.0 0.0.0.255 host 192.168.5.10
		# 拒绝主机 ping 192.168.5.10

(config-if)# ip access-group 100 in		# 将规则 100 应用于这个接口


### 扩展 ACL ###

(config)# ip access-list extended 100 		# 进入扩展 100 号规则

(config-ext-nacl)# permit icmp host 1.1.1.1 host 2.2.2.2 echo 
	# 允许 1.1.1.1 ping 2.2.2.2

(config-ext-nacl)# permit tcp host 1.1.1.1 host 2.2.2.2 eq ftp
	# 允许 1.1.1.1 访问 2.2.2.2 的 ftp 服务

(config)# permit 100 ip any any 			# 任何 ip 允许访问任何 ip 

*以名称做要在命令前加 ip*

(config)# access-list 100 deny tcp host 192.168.10.2 host 2.2.2.2 eq ftp
(config)# access-list 100 permit any
(config-if)# ip access-group 100 in

## ppp ##

(config)# username HQ password 123 	# 建立用户名和密码
(config)# int s0/0/0 				# 进入接口应用 ppp
(confi-if)# en ppp 					# 封装 ppp 协议
(config-if)# ppp autho c 			# 开启 ppp 认证



## NAT  ##

### 手动转换 NAT ###
(config)# ip nat inside source static [内网的本地地址] [公有地址] 	# 将 内网 的地址伪装成 公有地址

接口下
(config-if)# ip nat outside 	# 将这个接口面向公网接口
(config-if)# ip nat inside	 	# 将这个接口面向内网接口

### 动态 NAT ###

配置公有地址池
(config)# ip nat pool [name] [起始地址] [末尾地址] netmask [mask] 


映射公有地址池
(config)# ip nat inside soure list 10 pool [name] 
	# 这里的 list 10 用 ACL 的 编号 

## PAT （NAT 过载） ##

(config)# ip nat inside soure list 10 pool [name] overload   # 过载

(config)# ip nat inside soure list 10 int s0/0/0 overload 	 # 将出口接口的地址作 NAT 
	# 因为内网有许多，所以还要是用 过载

# 虚拟连接 #

IA 标签
(config-router)# area [number] virtual-link [router-id]

# IS-IS #

根据路由器的个数

ISIS 协议基于路由器的接口，所以要在接口下启用协议

(config-router)#net xx.[四个四位相同编号].xx  # 给编号

(config-if)# int F   				# 进入接口
(config-if)# ip router isis   		# 在接口下启用协议

HDLC 默认的封装格式


(config-router)# red os 1   					# isis 注入 osft 
(config-router)# red isis subnets 				# ospf 注入非直接的所有 isis 网段
(config-router)# red isis connetced subnets 	# ospf 注入 isis 的直连网段

不可靠协议

# BGP #

TCP 179 作为默认端口

# EGP #

不再是用试题值参数，也是用它独有的属性


(config)# router bgg [number] 					# 启动 bgp 进程
(config-router)# no sync 						# 关闭同步
(config-router)# bgp router-id x.x.x.x  		# 指定 id 
(config-router)# nei [邻居 router-id]  ra [邻居所在 bgp 进程号] 		# 指定邻居路由器及所在的AS
(config-router)# nei [邻居 router-id] up-so [邻居的源接口] 			# 指定更新源
(config-router)# network [ip 地址] mask [掩码 ] 						# 宣告网络
(config-router)# no au 												# 关闭自动汇总

$ show ip bgp 	# 查看 bgp 协议路由表



+ 公认必遵
	- ORIGIN (起源)
	- AS_PATH （AS 路径)
	- Next_HOP (下一跳)

# HSRP #

HSRP 组就是一将多个网关存放的地方

活动路由

(config-if)# standby 1 ip 192.168.13.254  		# 启用 HSRP 
(config-if)# standby 1 priority 120				# 设置优先级
(config-if)# standby 1 preempt					# 允许优先级最高时为活动路由器

1. 内部每个路由器接口地址，一个网段
2. 宣告网段
3. 在冗余的路由上做 pass 接入内网的操作

# 负载均衡 #
## VRRP ##

(config-if)# vrrp 1 ip 192.168.13.254  		# 启用 VRRP 
(config-if)# vrrp 1 priority 120			# 设置优先级
(config-if)# vrrp 1 preempt					# 允许优先级最高时为活动路由器


## GLBP ##



# 认证 #

## HDLC 
默认的认证协议


## ppp 

### PAP ###
单向认证
(config)# username XXX password xxx   	# 建立用户名和密码
(config)# int f0/1
(config-if)# encapsulation ppp			# 封装
(config-if)# ppp authentication pap  	# 启用 pap 认证模式，验证合法性

(config-if)# ppp pap sent-username xxx password xxx		# 另一个验证 pap 认证


`# deg ppp authentication` 		# 跟踪认证

### CHAP ###

*双向认证*

# 帧中继 #

+ 不提供纠错机制
+ 有检测到错误时只是丢弃数据包，而不发出任何通知
+ 单个物理电路提供多个逻辑连接

- 数据末端 DTE
- 与数据末端相连 DCE
- 数据传输 NNI

## DLCI ##

虚电路以 DLCI 标识，只在本地有效，用于标识虚电路的条数

DLCI 通常范围 16 - 1007

### 动态映射 ###

`frame-relay map protocol protocol-address dlci b[roadcast] [ietf] [cisco].command`

(config)# frame-relay switching 		# 将路由器变在帧中继交换机

(config-if)# frame-relay intf-type [dce|nni] 	# 帧中继类型

(config-if)# encapsulation frame-relay  		# 封装帧中继协议

入口线路
(config-if)# frame-relay route xx1 int [s0/1] xx2
	# 进该接口，接收 xx1 DLCI 号 从 s0/1 出 变成 xx2 DLCI

出口线路
(config-if)# frame-relay route xx2 int [s0/0] xx1
	# 出该接口，接收 xx2 DLCI 号 从 s0/0 进的 xx1 DLCI 号


(config-if)# no frame-relay inverse-arp 		# 关闭动态映射

### 手动映射 ###

(config-if)# frame-relay map ip [目标 ip address] [可出口的 DLCI 编号] broadcast  
	# 手动与目标地址建立虚电路 
(config-if)# frame-relay map ip [目标 ip address] [可出口的 DLCI 编号] 
	# 给自己建立一条，保证自己 Ping 自己能通

### 静态帧中继连接 ###

(config-if)# encapsulation frame-relay		# 封装 frame-relay
(config-if)# no frame-felay inv				# 关闭动态映射
(config-if)# no shu							# 打开端口
(config-if)# frame-rel map ip [目标 ip address] [可出口的 DLCI 编号] broadcast  
	# 手动与目标地上建立虚电路 

(config-if)# ip ospf net ptp

(config-if)# ip ospf priority [Number] 		# 将当前路由器的优先级更改

(config-if)# nei [邻居地址] 					# 手动指定邻居关系

### 多网段帧中继 ###

物理端口
(config-if)# encapsulation frame-relay		# 封装 frame-relay
(config-if)# no frame-felay inv				# 关闭动态映射
(config-if)# no shu							# 打开端口
子接口
(config)# int s0/0.1 point-to-point		 				# 为子接口
(config-subif)# ip add
(config-subif)# frame-relay interface-dlci [dlci 号] 	# 子接口下映射

## vpn 隧道 ##
(config-if)# int tunnel [编号] 				# 进入隧道接口
(config-if)# tun source [当前接口]   		# 指定出口的接口
(config-if)# tun destinaction [公网地址] 	# 目标的接口
(config--f)# ip add 						# 给隧道配地址，两边要一样





## 点对点 vpn `基于协议 ipsec` ##

(config-if)# crypto isakmp policy [编号]  # 创建策略
(config-isamkp)# encrytication aes			# 选择加密算法
(config-isamkp)# authentication pre-share   # 共享密钥
(config-isamkp)# hash md5					# 保护算法
(config-isamkp)# group 5					# 密码交换方式 3 -> 远程

(config)# cry isa key 0 [password] address [目的地址] 
	# 双方共享密钥
(config)# cry ipsec tranform-set [转换名字]	esp-[加密算法] esp-[保护算法]
	# 创建转换集

(config)# cry map [map 名] [map 编号] ipsec-isa 					# 创建静态加密图
(config-crypto-map)# set peer [目的地址] 			# 点对点的隧道，设置对等实体
(config-crypto-map)# set transform-set [转换名字] 	# 将转换集加入到 加密图 中
(config-crypto-map)# match address [acl 编号] 		# 将 acl 加入到 加密图 中
(config-crypto-map)# reverse-reouer static 			# 反向注入默认路由

进入接口，将加密图
(config-if)# cry map [map 名字] 						# 接口上应用加密图


## 语音

将交换机 vlan 1 变成语音 vlan 
(config-if)# sw mo ac					# 将所有接口变成接入模式
(config-if)# switch voice vlan 1		# 将默认 vlan 变成语音信道

分配 DHCP
(config)# ip dhcp pool [名字]
(dhcp-config)# network [网段地址] [掩码]
(dhcp-config)# defau
(dhcp-config)# option 150 ip [DHCP 地址] 	# 设置地址

配置语音
(config-if)# telephony-service 			# 启用路由器的语音功能
(config-telephony)# max-ephones 4
(config-telephony)# max-dn 4
(config-telephony)# ip source-address [注册地址] port [端口号] # 到注册地址得到电话号码
(config-telephony)# auto assign 1 to 4 	# 自动分配方式

配置号码
(config-if)# ephone-dn 1
(config-ephone-dn)# number [电话号码] 	# 分配电话号码

## 注册双方号码，实现两边电话通信

(config)# dial-peer voice [n] voip 	# 注册第 n 个对方的号码
(config-dial-peer)# destination-pattern [对方号码] 				#
(config-dial-peer)# session target ipv4:[对方路由出口方的地址] 	# 利用 ipv4 实现通信



## ipv6

(config)# ipv6 unicast-routing 			# 启用 ipv6


(config-if)# ipv6 address [ipv6地址]

(config)# ipv6 route ::/0 [下一跳地址]  	# 手动指定默认路由

(config)# ipv6 route [网段地址] [下一跳地址]  	# 手动指定静态路由


(config)# ipv6 router rip [编号] 		# 启用 ipv6 的 rip 协议

防止路由形成回路
(config-if)# split-horizon			# 水平分隔
(config-if)# posison-reverse 		# 独向逆转


(config-if)# ipv6 rip [编号] enable  		# 接口下启用 rip 协议

*ipv6 的动态协议都启用在接口下*

rip 协议注入静态
(config-if)# ipv6 rip [编号] d o 			# 方式一，接口下注入静态

(config-rtr)# red s 						# 方式二，在协议下注入静态

ospf 协议注入静态
(config-rtr)# def o always 					# 在协议下注入静态


## ipv6 跨越 ipv4 隧道

(config)# int tun 1
(config-if)# ipv6 add 100::1/64
(config-if)# run sou [出口编号]
(config-if)# tun des [目的地址]
(config-if)# tun mode ipv6ip 				# 选择模式 ipv6 走 ipv4

(config-if)# ipv6 ospf 1 area 0				# 分别宣告隧道两边的接口地址
(config-if)# ipvr rip [编号] en 				# rip 宣告


















