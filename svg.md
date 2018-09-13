
## 操作 API

+ document.createElementNS(ns, name)
	- ns 命名空间
	- name 元素节点名

## 世界，视野，视窗

+ SVG 控制世界

+ width, height 控制视窗
+ viewBox 控制视野大小
	`viewBox=x,y,width, height`
+ preserveAspectRation 
+ meetOrSlice 

## 分组 `<g>`

标签创建分组

属性继承,其实就是用来标记共用的属性的标记

## 坐标系统

+ 笛卡尔直角坐标系
+ 原点
+ 互相垂直的两条数轴
+ 角度定义

## 四个坐标系

+ 用户坐标系(世界的坐标)
+ 自身坐标系(每个图形元素或分组独立与生俱来)
	*每一个元素定义的 x,y 都是基于自身的坐标*
+ 前驱坐标系(父容器的坐标系)
+ 参考坐标系(使用其它坐标系数来考究自身情况使用时)

## 坐标变换

+ 定义
+ 线性变换
+ 线性变换列表
+ transform 属性

	父容器的坐标系，定义前驱坐标系到自身坐标系的线性变换
	- rotate(dig)
	- tranalate(x,y)
	- scale(sx,sy)
	- matrix(a,b,c,d,e,f)

## RGB & HSL

+ rbg([0-255],[0-255],[0-255])
	- 红，绿，蓝
+ hsl([0, 359],[%s], [%l])
	- 颜色，饱和度，亮度
+ 透明度 raga() | hsla()
+ opacity 属性透明

##　线性渐变 `<lineargradient>`

+ `<stop>` 两个标记来定义一个渐变
+ lineargradient: gradientUnits 属性可以用来指定渐变的开始坐标系

## 径向渐变 `<radialGradient>`

## 笔刷 `<pattern>` 
+ patternUnits
+ patternContentUnits

## Path `<path>`

Path 可以绘制任意图形，属性 d 可以接命令字符串

### 命令字符串
+ 属性 d 跟命名，坐标字符串
+ 大小绝对位置
+ 小写相对位置
+ 最后的参数表示最张要到达的位置
+ 结束位置是下一个命令开始位置(默认0,0,自身坐标系) 

+ 直线与移动 M L H V Z 
+ 二次贝塞尔曲线 `M 起始绝对坐标 Q x,y, 结束坐标`
+ 三次贝塞尔曲线 `M 起始绝对坐标 Q x1,y2,x2,y2 结束坐标`
+ 光滑贝塞尔曲线 T

## 文本 `<text> <tspan>`

+ dx,dy 属性可设置基线的坐标，而且这个属性的值可以是数组形式
	- dx = 10 10 10 10
	- tspan 中的 dy 偏移会有传递的作用
+ text-anchor 水平居中
+ dominant-baseline 垂直属性
+ getBBox() 可获取元素的渲染矩形区域
+ y

## 路径文本 `<textPath>`

+ 使用方式
```SVG
<path id="path1" d="M 100 200 Q 200 100 300 200 T 500 200" />
<text>
	<textPath xlink:href="#path1">文本内容文本内容</textPath>
</text>
```
+ x, text-anchor, startOffset 

## 超链接 `<a>`

+ xlink:href
+ xlink:title
+ target


## 引用 `<use>`

## 裁切 `<clip>`

## 蒙版 `<mask>` 


## 动画 SMIL

### animate

```svg
<animate xlink:href="url(#aim)"
	attributeType="XML"
	attributeName="x"
	from="10"
	to="110"
	durtaion="3s"
</animate>
```

### animateTransform
```svg
<animateTransform id="toRight"
	attributeType="XML"
	attributeName="transform"
	type="rotate"
	from="100"
	to="500"
	dur="1s"
	fill="freeze"> 
</animateTransform>
```

### animateMotion



