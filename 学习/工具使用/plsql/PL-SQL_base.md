## 导出表数据

选择菜单，工具->导出用户表，有三种导出方式：

```
Oracle Export，Sql Insert，pl/sql developer
第一种 是导出为.dmp的文件格式，.dmp文件是二进制的，可以跨平台，还能包含权限，效率也很不错，用得最广
第二种 是导出为.sql文件的，可用文本编辑器查看，通用性比较好，但效率不如第一种，适合小数据量导入导出。尤其注意的是表中不能有大字段（blob,clob,long），如果有，会提示不能导出(提示如下：
table contains one or more LONG columns cannot export in sql format,user Pl/sql developer format instead)，可以用第一种和第三种方式导出。
第三种 是导出为.pde格式的，.pde为Pl/sql developer自有的文件格式，只能用Pl/sql developer自己导入导出；不能用编辑器查看。

http://www.cnblogs.com/Jamesliang/articles/2794449.html

```


## 导出表 结构
```
tools->export user objects是导出表结构
这种方式可以导出当前用户拥有的所有对象，包括表、视图、触发器、同义词等等，对于表，只能导出表结构(建表语句)，不能导出数据。
导入步骤

注：导入之前最好把以前的表删除【导入另外数据库的情况除外】。

1 tools->import tables->SQL Inserts 导入.sql文件。

2 tools->import talbes->Oracle Import然后再导入dmp文件。
导入sql文件
SQL>@c:\filename1.sql【一定要在FILE-》new -》Command Window】下执行。
```


右键表后query data，报错


```


Tools
 
-->
 
Preferences
 
-->
 
Options
 
-->
 
Automatic
 
Statistics
 
选项，将其前面的小对勾去掉即可。
连接不上数据库


在
cmd
下，
sqlplus
是可以连接上的，但
pl
/
sql
去连接不上。

解决：在连接不上的情况下，新建一个实例
 
New
 
Instance
重新连接即可。
```