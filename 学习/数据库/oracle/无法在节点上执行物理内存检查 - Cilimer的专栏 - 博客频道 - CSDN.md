
原文来自： http://mahilion.blog.163.com/blog/static/183087295201243112739831/


安装Oracle 11g r2出现如下错误：
//物理内存
物理内存 - 此先决条件将测试系统物理内存总量是否至少为 922MB (944128.0KB)。
预期值
 : N/A
实际值
 : N/A
 错误列表:  
 - 

//可用物理内存
PRVF-7531 : 无法在节点 "LENOVO-F4F9938F" 上执行物理内存检查  - Cause:  无法在指示的节点上执行物理内存检查。  - Action:  确保可以访问指定的节点并可以查看内存信息。  

可用物理内存 - 此先决条件将测试系统可用物理内存是否至少为 50MB (51200.0KB)。
预期值
 : N/A
实际值
 : N/A
 错误列表:  
 - 
PRVF-7563 : 无法在节点 "LENOVO-F4F9938F" 上执行可用内存检查  - Cause:  无法在指示的节点上执行可用内存检查。  - Action:  确保可以访问指定的节点并可以查看内存信息。

//交换空间大小
交换空间大小 - 此先决条件将测试系统是否具有足够的总交换空间。
预期值
 : N/A
实际值
 : N/A
 错误列表:  
 - 
PRVF-7574 : 无法在节点 "LENOVO-F4F9938F" 上执行交换空间大小检查  - Cause:  无法在指示的节点上执行交换空间检查。  - Action:  确保可以访问指定的节点并可以查看交换空间信息。  
 - 
PRVF-7531 : 无法在节点 "LENOVO-F4F9938F" 上执行物理内存检查  - Cause:  无法在指示的节点上执行物理内存检查。  - Action:  确保可以访问指定的节点并可以查看内存信息。

解决方法：
1. 在命令提示符下 net share c$=c:



补充


如果这个命令提示错误：“发生系统错误 5，拒绝访问的时候”，那我们可以修改注册表，检查AutoShareServer和AutoShareWks注册表值，以确保未将它们设置为0，让C盘进行共享。

步骤：

1.依次点击“开始→运行”，输入regedit，然后按回车键进入注册表编辑器。

2.找到并单击HKEY_LOCAL_MACHINE-->System-->CurrentControlSet-->Services-->LanmanServer-->Parameters,将AutoShareServer和AutoShareWks的值改为1。

3.重启电脑。再次运行net share ,显示结果如图即可：