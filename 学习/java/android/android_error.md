##Unable to resolve target 'android-18'



原因：导入的程序和你的开发环境版本不一样导致，或者是你导入的
jar
包没有被引入进来。

解决方法：

第一步：如下图在
project
中找到
 
Android
然后将对应的
jar
包选上。

第二步：在
project
中找到
 java 
Buid
 
Path
 
-->
 order and 
export
 
然后将对应的
jar
包选上。

或者，

修改project.properties ,把target=android-8改成 target=android-19.

##Installation error: INSTALL_FAILED_VERSION_DOWNGRADE



安装过一个开发的
APP
之后，需要把应用程序的安装包中的包文件目录修改一下，然后就出现了这个问题了，以前也出现过没有太注意，仔细查了一下资料，按其字面意思就是安装版本太低了。所以就想到了
android
:
versionCode
=
"1"
    android
:
versionName
=
"1.0"
 
这两个属性
    

在网上对这两个属性的解读为：
android
:
versionCode
为整数值。
android
:
versionName
为字符串类型。所以只要提高版本号就好了，我这里将
VersionCode
的值提高一，
android
:
versionCode
=
"2"
##ActivityManager: Warning: Activity not started, its current task has been brought to the front



原因:因为你的模拟器中还有东西在运行，也就是你要运行的
activity
已经有一个在模拟器中运行了。
 

不要以为模拟器退出到桌面了就没有东西在跑了。在你调试的时候异常关闭的程序有可能就有
activity
在运行。

解决:
project
->
clean
。


##Failed to find provider info for com.leadcore.sdb



it
's time to deal.
##Error executing aapt: Return code -1073741819



clean
-------没用

fix project properties
---------没用

关闭
xml
的各种语法检查-------没用

检查各种自己工程的源代码------------好吧，关它们什么事了。。。当然也没用

资源冲突-------------------同一个工程中两个名为
username
的
EditText
也没出过问题是吧。

只要不在同一个
layout
什么的里面就好。

--
aapt
专门对付资源的，如果它报错肯定是资源问题。。。虽然不知到底会是什么样的问题，
R
还得从资源文件上找错误原因。

  
修改下：
values
/
strings
.
xml
:
 
<
string name
=
"app_name"
>
MyImageView
</
string
>的
app_name
即可。


--出现：
invalid resource directory name	crunch

解决方法一：

直接在项目中删除报错的
crunch
文件夹:/
bin
/
res
/
crunch
,

解决方法二：

修改项目版本到
1.6
以上。
##Activity not started, its current task has been brought to the front



--原因分析：

  
因为你的模拟器中还有东西在运行，也就是你要运行的
activity
已经有一个在模拟器中运行了。不要以认为模拟器退出到桌面了就没有东西在跑了。在你调试的时候异常关闭的程序有可能就有
activity
在运行。


--解决方法：

  
--
project
->
clean
。

  
--退出虚拟机的程序从新运行一遍

