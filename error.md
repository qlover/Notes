

+ Reason: Failed while changing version of java to 1.7
	+ 修改该工程 .setting 目录下的 org.eclipse.wst.common.project.facet.core.xml 文件中的
		- <installed facet="java" version="1.7"/>
	+ [01](http://blog.csdn.net/nly19900820/article/details/51788083)


+ Target runtime com.genuitec.runtime.generic.jee60 is not defined
	+ 去掉工程 .setting/ 目录下的 org.eclipse.wst.common.project.facet.core.xml
		- <runtime name="com.genuitec.runtime.generic.jee60"/>
	+ [02](https://jingyan.baidu.com/article/d7130635338e3f13fdf47518.html)

+ Project configuration is not up-to-date with pom.xml. Select: Maven->Update Project... from the project context menu or use Quick Fix.
	+ 右键项目，【Maven】-->【Update Project Configuration...】

