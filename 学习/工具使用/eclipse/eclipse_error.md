##Cannot change version of project facet Dynamic Web Module to 2.5.
One or more constraints have not been satisfied.
```
.settings中的org.eclipse.wst.common.project.facet.core.xml中的
<installed facet="jst.web" version="2.3"/> version的值改为2.5即可，Version的值要与
web.xml中的
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee"
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
id="WebApp_ID" version="2.5">
version的值保持一致。
同时项目右击->Properties->Project Facets中的Dynamic Web Module也要一样为2.5。
最后右击->Maven Update Project即可。
```
##Cannot change version of project facet Dynamic Web Module to 3.0
```


找到 .setting文件夹内的 org.eclipse.wst.common.project.facet.core.xml 文件，文件格式大致如下：

<?xml version="1.0" encoding="UTF-8"?>
<faceted-project>
<runtime name="Apache Tomcat v5.5"/>
<fixed facet="jst.web"/>
<fixed facet="jst.java"/>
<installed facet="jst.java" version="5.0"/>
<installed facet="jst.web" version="3.0"/>
<installed facet="wst.jsdt.web" version="1.0"/>
</faceted-project>

直接手动修改jst.web对应的version即可。 最后重启tomcatX就可以正常使用了。




dynamic web module 版本之间的区别：
 Servlet 3.0 December 2009 JavaEE 6, JavaSE 6 Pluggability, Ease of development, Async Servlet, Security, File Uploading
 Servlet 2.5 September 2005 JavaEE 5, JavaSE 5 Requires JavaSE 5, supports annotations
 Servlet 2.4 November 2003 J2EE 1.4, J2SE 1.3 web.xml

错误Cannot change version of project facet Dynamic Web Module to 3.0：
在项目右键属性的Project facts中把Dynamci  Web  Module设置为3.0，如果报错则直接修改项目文件：工程.settings目录下的org.eclipse.wst.common.project.facet.core.xml，同时把web.xml开头设置为：

       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">
然后执行项目右键Maven的Update Project...即可。



```
## eclipse 方法上加上@Override就报错
```


这是jdk的问题，@Override是JDK5就已经有了，但是不支持对接口的实现，认为这不是Override而报错。JDK6修正了这个Bug，无论是对父类的方法覆盖还是对接口的实现都可以加上@Override。

要解决该问题，首先要确保机器上安装了jdk 1.6，

然后，选择eclipse菜单Windows->Preferences-->java->Compiler-->compiler compliance level选择 1.6，刷新工程，重新编译。

如果问题还没解决，就在报错的工程上，鼠标右键选择 Properties-->Java Compiler-->compiler compliance level 中选择 1.6,刷新工程，重新编译。

来源：  http://blog.csdn.net/jjunjoe/article/details/6927148
```
## The declared package does not match the expected package
```


错误的原因是：

eclipse中包的定义一般是通过package包名产生，而不是通过文件的层次来定义。eclipse使用import导入源码时，导入的是文件结构而不是包形式，故报错。

解决方法：

点击> properties > java build path > source > add folder > select src/XXXX

然后重新编译就ok了。
```