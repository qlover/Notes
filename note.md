
界面设计


数据模型



# elemenetUI

<el-checkbox/>
true-label  点击时的传值
false-label 取消后的传值
label 传递

<el-collapse-item> 下面不能直接循环表格
<el-table-column>  中需要使用 <template> 作额外逻辑操作
<el-table/> data 属性绑定是一个数组，即使只有一个对象，也应该 push 进该数组中

先过滤条件，再关联表


判断除开当前时间的所有记录
 1. sql 不在当前时间
 2. 或者在程序中判断(程序判断比sql语句快)

json_decode() 参数二传递为 true ，解析成一个数组

new \stdClass 创建一个普通 php 对象

/organization/course/getCommentList

分支机构课程从
1. 线下单门课程机构
2. 线下系列课程每一场的机构信息
3. 网络课程

要点，
1. 课程是属于一个机构的
2. 总部机构和分支机构有不同的课程
3. 课程只属于总部
4. 分支机构课程则是由上课信息得到

<el-pagination/> 点击页数获取时，有时会有bug

当没有数据时，下拉菜单会可能会报错


% 运算，结果符号为左边数的符号 

php getdate([time()]) 返回时间戳相关联的时间数组信息

allowField(true) 过滤不是数表中的字段
data([]) 方法批量赋值

strstr() 查找字符串，成功返回，失败返回false
strrpos() 成功大于-1
strcmp(str1, str2) 比较字符串大小，相等返回0
tp5 Route 中可用变量替换
多个需要可选判断的字体用数组循环

get 和 post 可传输内容大小不一样，一个有限制，一个没有限制，一个在 header,一个在 body

find()->value() 一起使用会出现问题



allowField(true) 过滤不是数表中的字段
data([]) 方法批量赋值

strstr() 查找字符串，成功返回，失败返回false
strrpos() 成功大于-1
tp5 Route 中可用变量替换
多个需要可选判断的字体用数组循环


1. 保存工资时的字段验证，和可选字段，X(前后端一起验证)
2. 新建员工不填写字段跳转教育，工资页面时没有数据 X
3. 讲师新增教育和工作经历拉取不到 X
4. 前端城市验证，绑定一个值 X
5. 学员个人档案获取缺少发票抬头
6. 前端员工城市验证 X
7. 营销政策验证，循环元素不行 x
8. form数据二层以上验证问题，添加角色 X
9. 线下课程验证 X
10. 金额验证问题 X
11. 促销管理拼团价格不能高于课程原价 X
like ID 可提高效率

12. 拼团列表，H5学员课程列表,page=&limit= [A non-numeric value encountered
]

分支机构课程评价获取列表X

讲师管理

$store.getters 获取状态管理中的数据

申请发票可多次

写一个二维码

课程审核，首次不设置佣金问题

opcache.enable 服务器缓存

不用 pagante() 分页时，$page $limit 报错

时间戳减1就是前一秒

strtotime("time() + 1 month") 当前时间戳向后加一个月


```php
<<<sql
// 必须顶格
sql; // 后面不能有空格
```


// mode = 1 
//  classify = 1
//    场次讲师
//  classify = 2
//    多节课程 -> 课程讲师
// mode = 2
//  classify = 1
//    课程讲师
//  classify = 2
//    不存在讲师

后台返回 code 状态码

H5讲师签到管理,原 lesson 改变开始结束时间,结果会获取不到

课程创建时场次时间选择，只能在当前时间范围内(组件)

课程创建时报名项，全选问题(组件)

签到管理
1.讲师对应
2.课程对应
3.时间对应

查询条件在跟在 on 后面

佣金政策等级过期显示后面加一个已过期

