

1. Reason: Failed while changing version of java to 1.7
	+ 修改该工程 .setting 目录下的 org.eclipse.wst.common.project.facet.core.xml 文件中的
		- <installed facet="java" version="1.7"/>
	+ [01](http://blog.csdn.net/nly19900820/article/details/51788083)


2. Target runtime com.genuitec.runtime.generic.jee60 is not defined
	+ 去掉工程 .setting/ 目录下的 org.eclipse.wst.common.project.facet.core.xml
		- <runtime name="com.genuitec.runtime.generic.jee60"/>
	+ [02](https://jingyan.baidu.com/article/d7130635338e3f13fdf47518.html)

3. Project configuration is not up-to-date with pom.xml. Select: Maven->Update Project... from the project context menu or use Quick Fix.
	+ 右键项目，【Maven】-->【Update Project Configuration...】

4. ModuleNotFoundError: No module named 'win32api'
	[03](https://sourceforge.net/projects/pywin32/files/pywin32/Build%20221/)
	安装相同版本的 pywin32 

5. Object of type 'bytes' is not JSON serializable
	[04](https://blog.csdn.net/z564359805/article/details/80599126)
	[05](https://blog.csdn.net/bear_sun/article/details/79397155)






# 禁止选中该元素的文本内容

`onselectstart="return false"`

# IE6 3px bug

`_margin-left:-3px;	//针对IE6 生效`

#  ul 去掉点时

只是将页面中的 ul li 的默认的项目符号去掉，而没有将项目符号前面的空白去掉，在这里需要加上`padding:0;`这个时候可以直接改变 li 的高，宽了

# IE6 双倍边距 bug

当前元素浮动时，在设置边距时，其边距会是此基础上的两倍，解决方法：`display:inline;`

# p 元素加边框

在 p 元素的子是 标题或者其它的块级元素加边框的时候，只会有上下，边框，原因，则是，两个块元素重合了，子把父的左右边框挡着了

# ==运算

两边都相等返回 true
两边不相等返回 false


*== 只比较数值而不比较数的数类型*

在 check.html 文件中，有一行
`allCheck.checked = (items.length==n);`

常规的想，是想把 allCheck 的 checked  这个属性变为 true，就是选中这个一个复选框，代码就是`allcheck.checked = true;`

但是，这里是想要当 items 的所以被选中，才选中 allCheck 复选框，所以这里多了一个运算，就是将，所以选中的所以的数量与 items 的总数量作个比较，因为， == 运算是返回一个布尔值，所以如果这两个值相等也就是返回了 true ，那么在 
`allCheck.checked = (items.length==n);`

这个片段中， (items.length==n) = ture ，则间接性的将 
`allCheck.checked = true;`

写成了这样，也就是 将 (items.length==n) 写成了 true，那么，如果这个两个数值都相等就返回 true ，所以就选中 allCheck 这个复选框 


# ===运算

两边都相等 并且 数据类型也相等返回 true
两边不相等 并且 数据类型不相等返回 false

*===】 比较的是数值和数据的类型*


# setTimeout()

在用该方法的时候，需要特别注意，该函数对变量处理的作用域
比如需要在一秒内随机输出一个数字

```js
var r = Math.round(Math.random()*255);
setTimeout(function(){
	alert(r);		//是不是在这里就会觉得可以达到
				//但是不行，因为第一次的 ran 的变量驻留在了 setTimeout() 方法中了
				//所以第二次还是会输出第一次的结果
},1000);

所以应该用其它的方法来解决

var r = Math.round(Math.random()*255);
setTimeout(ran(r),1000);

//用一个函数来传递，并用 r 变量作为参数传递进来就行了
function ran(c){
	alert(c);
}
```
所以最好在用该方法的时候用函数封装进来 


# 元素居中

如果只是要两个块元素的其中一个居中，【不需要】将外面的层设高、宽，只需要将四周内边距设
相等就行

```html
<style>
.link{ background:#03a1c7;text-align:center; padding:20px; float:left;}
.link a{ width:150px;height:150px; background:#b6f1fe;line-height:150px; display:block;text-decoration:none;}
</style>
<div class="link"><a href="#a">Coming</a></div>
```
像上面的 CSS 一样设置就行了


# opacity
在对元素全用 opacity 时，如果元素后面一层有背景则可以在改变 opaicty 时加上背景效果
```html
<style>
li{background:#f00; width:200px; height:200px; list-style:none;}
li img{opacity:0.888; width:200px; height:200px; transition:1s;}
li img:hover{ opacity:0.999; transform:scale(1.05);}
</style>
<li><img src="http://pic21.nipic.com/20120530/4639117_110740722000_2.jpg" /></li>
```

# flex
用了该属性的元素加了其它的一些属性会没用，比如是 width,height .....


# 连续赋值的问题

```javascript
var a = b =5; //虽然这样看，看不出啥子问题
alert(a);
alert(b);	//两个照样打印出来，但是

(function(){
 var a = b = 5;	//简单的来看也看不出啥子问题，但是
 alert(b);
 alert(a);	//打印不出来，会出错
})();
```
上面在匿名函数中，的变量 a 因为是用 var 关键字声明，也就是说变量 a 它在这个匿名函数中被声明了，说穿了，就是 a 是匿名函数中的变量，而 b 在全局作用下赋值，所以结果是  变量 a 没得值



## TP5 中闭包查询中不能使用上层作用域变量问题
```
$checkEmployeeInfo = OrganizationEmployeeModel::where('organization_employee_id', '<>', $organization_employee_id)
    ->where(function ($query) use($employeeInfo) { // 闭包中需要使用 use 得外层变量
        $query->where('id_number', $employeeInfo['id_number'])
            ->whereOr('mobile', $employeeInfo['mobile']);
    })
    ->find();
```


保存员工时，机构部门显示问题









SQLSTATE[23000]: Integrity constraint violation: 1062 Duplicate entry '' for key 'id_number'
索引不能重复

注册机构时，不知道是注册分支还是总部不清楚父机构ID有没有
注册员工时和机构时不知道角色


# php7 could not find diver

1extension=php_pdo.dll这个文件是否存在
2打开windows下的php.ini，查找 extension_dir = "地址"，查看这个地址是否有文件夹
3看该文件夹中是否包含上述文件
4新建一个php页面，输入<?php phpinfo(); ?>预览，查看是否已经开启了先关扩展

如果发现 no value, 则改变 php.ini extension_dir = "E:/wamp/php7/ext" 绝对路径








































