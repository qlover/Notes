

## canvas

默认 300x150 

基于状态的绘制

*！！！坐标 Y 轴向下*
*！！！坐标 Y 轴向下*
*！！！坐标 Y 轴向下*

+ moveTo(x,y) 画笔起始点，相当于 SVG M x,y 命令
+ lineTo(x,y)
+ beginPath() 重新开始一个路径
+ closePath() 如果没有封闭的路径，则会自动封闭
+ lineWidth
+ strokeStyle
+ fillStyle
+ stroke()
+ fill()
+ arc(rx, ry, r, startingAngle, endingAnge, anticlockwise = false) 绘制弧线
	- rx,ry 圆心坐标
	- r 圆半径
	- angle 弧度的开始值与结束值
	- anticlockwisefalse 绘制方向  false 顺时针
+ arcTo(x1, y2, x2, y2, r)
	- 接收两个坐标
	- 接收一个半径
+ canvas 
+ clearRect(x, y, widht, height) 刷新当前画布
+ isPointInPath(x, y) 点击检测

+ rect(x, y, width, height)  绘制一个矩形
+ fillRect(x, y, width, height) 填充一个矩形
+ strokeRect(x, y, width, height) 绘制出了矩形
+ lineCap 线条的开始与结束的样式
	- round 圆头
	- square 方头
	- butt(default) 
+ lineJoin 线条相交时的样式
	- miter(default)
	- bevel 
	- round 圆角
+ miterLimit 当 lineJoin 为 miter 时
	- 10 默认 10 个像素
	- 是有宽度的线的中心线与最外边缘线，边缘间的距离 
+ tranlate(x, y) 移动元素到 x,y
	- 会叠加多次过后的 tranlate() 值
+ rotate(deg) 旋转 deg 度数
+ scale(sx, sy) 
	- 会将图形的坐标及其它的属性也会改变
+ save() 保存当前图形状态
+ restore() 与 svae() 成对出现，

+ transform(a,b,c,d,e,f) 变换矩阵
	- 会有级联的作用，也就是会将所有的 transform 会变成一次设置
+ setTransform() 
	- 该方法会将所有的 transform() 都忽略
```
a c e
b d f
0 0 1

a 水平绽放 1
b 水平倾斜 0
c 垂直倾斜 0
d 垂直缩放 1
e 水平位移 0
f 垂直位移 0
```
+ createLinearGradient(x, y, xend, yend) 线性渐变
	+ addColorStop(stop, color)
		stop 0 - 1 之间的位置
+ createRadialGradient(x0, y0, r0, x1, y1, r1) 径向渐变
	+ addColorStop(stop, color)
		stop 0 - 1 之间的位置
+ createPattern([img|canvas|video], repeat-style)
	img 一个 Image() 类的实例
	canvas 可以是一个 canvas 画布
	video 可以是一个视频
	repeat-style 如何重复
		no-repeat
		repeat-x
		repeat-y
		repeat
+ quadraticCurveTo(x1, y1, x2, y2) 二次贝塞尔曲线
	(贝塞尔二次曲线)[http://tinyurl.com/html5quadratic]
+ bezierCurveTo(x1, y1, x2, y2, x3, y3) 三次贝塞尔曲线
	(贝塞尔三次曲线)[http://tinyurl.com/html5bezier]
	- 开始点都是 moveTo()
	- 最后的坐标都是结束
+ font 类似 css font 属性的值
	- '20px sans-serif' 默认值
	- [font-style fint-variant font-weight font-size font-family]
		参数设置与 css 一致
+ fillText(string, x, y, [maxlen]) 
	- string 书写的串,填充
	- x,y 坐标
+ strokeText(string, x, y, [maxlen])
	- string 书写的串, 描边
	- x,y 坐标
+ textAlien 文本水平对齐方式
	- left center right
+ textBaseline 文本垂直对齐， 默认值  alphabetic
	- top middle bottom alphabetci ideographic hanging
+ measureText(string)
	- 返回一个有 widht 的对象，表示该字符串的宽度

+ shadowColor 阴影颜色
+ shadowOffsetX & shadowOffsetY
	- 阴影位移
+ shadowBlur
	- 阴影模糊度
+ globalAlpha 全局透明度, 默认值 1
+ globalCompositeOperation
	- source-over 后覆盖前
	- source-atop
	- source-in 保留后绘制内部的部分
	- source-out 保留后绘制外面的部分
	- destination-over 前覆盖后
	- destination-atop
	- destination-in
	- destination-out
	- lighter 层叠部分颜色会重新计算
	- copy 只绘制最后一个图形
	- xor 异或操作，去掉重复部分
+ clip()

+ DOMElement.getBoundingClientRect() 返回相对于当前元素的矩形

+ drawImage([image|canvas], sx, sy, sw, sh, dx, dy, dw, dh)
	- drawImage(, dx, dy)
	- drawImage(, dx, dy, dw, dh)
	- drawImage(, sx, sy, sw, sh, dx, dy, dw, dh)

	+ sx, sy 原图像的坐标位置
	+ sw, sh 原图像坐标位置上的宽高
	+ dx, dy 定义的坐标位置 
	+ dw, dh 定义后的坐标位置上的宽高

+ getImageData(x, y, w, h) 返回一个 ImageData 对象 (width, height, data)
	- x,y 坐标
	+ data 存放图像相关的像素信息
		- 将图像的所有像素信息存放到一个数组中
		- 其中每四个元素表示一个像素
		- 每个像素的四个元素分别对应 rgba 的 r g b a 的值
+createImageData(w, h) 直接创建一个空白的 imageData 的区域

+ putImageData(imageData, dx, dy, dirtyX, dirtyY, dirtyW, dirtyH)
	- dirtyX, dirtyY 会累加 dx,dy 原始的坐标

+ 离屏
	- 将第二个 canvas 中的内容加载到第一个 canvas 上

+ Viewport 元信息
```z
<meta name="viewport"
	content="
		width=[pixel|device],
		hieght[pixel|device],
		initial-scale = [float_value],  // 应用初始的缩放
		minimum-scale = [float_value],  // 如果可以缩放，最小的缩放尺寸
		maxinmum-scale = [float_value], // 如果可以缩放，最大的缩放尺寸
		user-scalable = [yes|no] // 是否允许缩放
	"
/>
```

有描边和填充时，先填充再描边

(IE8- 的兼容)[https://code.google.com/p/explorercanvas]

Artisan.js


