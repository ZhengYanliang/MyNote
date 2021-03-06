##MySQL root用户忘记密码解决方案(安全模式,修改密码的三种方式)
```
原文： http://blog.csdn.net/shawyeok/article/details/41725619
1.关闭正在运行的MySQL 
2.启动MySQL的安全模式,命令如下: 

mysqld 
--skip-grant-tables
mysqld-nd 
--skip-grant-tables

3.使用root用户[免密码]登陆MySQL
mysql -u root -p

输入密码时,直接回车 4.选择MySQL系统库
use
 mysql

5.查看当前系统用户root的密码
select user, host, password from user where user = "root";
查看的password是经过加密的,若以后想要恢复当前密码可以先运行这条命令备份一下当前的密码

6.修改root用户的密码
update user set password=PASSWORD("your_password") where user = "root";

这里是直接修改了root用户在所有登陆位置的密码,若你仅仅只想修改root在某一处的密码,可以在上一条命令中增加一个限定条件host='somewhere'
比如,下面的命令修改了root用户在本机localhost的登陆密码
update user set password=PASSWORD("your_password") where user = "root" 
    and host = "localhost";
上面的操作是直接对MySQL系统库mysql进行修改,安全性较低,一旦出现误操作,成本高,难恢复,并且仅限于对mysql库有UPDATE权限的用户,MySQL本身为我们提供了一种更加简便的操作方式,在此作一下简单的介绍

修改当前登陆用户的密码,使用SELECT CURRENT_USER();可查看当前登陆用户

SET PASSWORD = PASSWORD('cleartext password');

修改bob用户在%.example.org位置上的登陆密码,注意这里的host地址%.example.org是必须要存在的
SET
 PASSWORD 
FOR
 
'bob'
@
'%.example.org'
 = PASSWORD(
'cleartext password'
);


当然我们也可以通过GRANT的方式修改密码
GRANT USAGE ON *.* TO 'bob' @'%.example.org' IDENTIFIED BY 'cleartext password';

关于修改密码的详细内容还是请见官方文档(5.6)
http://dev.mysql.com/doc/refman/5.6/en/set-password.html

7.刷新一下系统的权限
flush privileges;

8.关闭MySQL的安全模式,重新启动即可 注:

在第2步,启动安全模式的时候,命令行可能会一直处于挂起状态,此时Ctrl+c也不能终止运行,这时候只要通过netstat -ao查看MySQL端口是否处于监听状态,如是即代表MySQL已经进入了安全模式,出现这种现象是主要因为MySQL不提倡安全模式长时间运行
```

##MySQL root 用户密码重置
```
1、首先停止正在运行的MySQL进程
Linux下,运行 killall -TERM MySQLd
Windows下，如果写成服务的 可以运行：net stop MySQL,如未加载为服务，可直接在进程管理器中进行关闭。
 
2、以安全模式启动MySQL
Linux下，运行 /usr/local/mysql/bin/mysqld_safe --skip-grant-tables &
Windows下，在命令行下运行 X:/MySQL/bin/mysqld-nt.exe --skip-grant-tables  
 
3、完成以后就可以不用密码进入MySQL了
Linux下，运行 /usr/local/mysql/bin/mysql -u root -p 进入
Windows下，运行 X:/MySQL/bin/mysql -u root -p 进入
 
4、更改MySQL数据库密码
>use mysql     
[sql] view plain copy print?
>update user set password=password("new_pass") where user="root";  
>flush privileges;    
```

##重新安装
```
在注册DotNetWinService服务时，再使用 "sc delete 服务器名称" 命令删除服务就出现“指定的服务已经标记为删除”的异常。

原因是运行删除服务项命令的时候，服务管理窗口未关闭引起的。

关闭服务管理窗口，重新删除、安装服务项即可。
```

##安装
```
1.下载MYSQL安装包

可以去官网下载ZIP包 http://www.mysql.com/downloads/mysql/

我安装的是mysql-5.5.23-winx64

2.解压到本地目录

D:\mysql

3.添加系统环境变量

添加系统环境变量是为了在命令控制窗口里更方便点。

新建：MYSQL_HOME ==> D:\mysql 
追加：PATH==>;%MYSQL_HOME%\bin

4. 修改MySQL5.5的配置文件，把my-small.ini改名为my.ini进行编辑
在[mysqld]下追加
-------
basedir = "d:\\mysql"
datadir = "d:\\mysql\\data"
-------

5.启动服务

保存my.ini的配置， 然后打开命令行（开始菜单==>运行==>cmd ）
输入: mysqld --console 然后回车将看到如下类似内容:
-------
C:\Windows\system32>mysqld --console
120410 14:25:22 [Note] Plugin 'FEDERATED' is disabled.
120410 14:25:22 InnoDB: The InnoDB memory heap is disabled
120410 14:25:22 InnoDB: Mutexes and rw_locks use Windows interlocked functions
120410 14:25:22 InnoDB: Compressed tables use zlib 1.2.3
120410 14:25:22 InnoDB: Initializing buffer pool, size = 128.0M
120410 14:25:22 InnoDB: Completed initialization of buffer pool
120410 14:25:22 InnoDB: highest supported file format is Barracuda.
120410 14:25:22 InnoDB: Waiting for the background threads to start
120410 14:25:23 InnoDB: 1.1.8 started; log sequence number 1595675
120410 14:25:23 [Note] Event Scheduler: Loaded 0 events
120410 14:25:23 [Note] mysqld: ready for connections.
Version: '5.5.22' socket: '' port: 3306 MySQL Community Server (GPL)
-------
==> 证明mysql服务已启动

6. 设置登陆mysql root帐号的的密码
打开新的命令行，输入 mysql -uroot 回车
-------
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 1
Server version: 5.5.22 MySQL Community Server (GPL)

Copyright (c) 2000, 2011, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners. www.2cto.com 

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
-------
==〉看到上面类似内容说明登陆成功，此时的root帐号是没有密码的
执行命令修改密码：

use mysql; 
update user set password=PASSWORD("这里填写你要设置的密码") where user='root';

执行完成后退出mysql操作,然后关闭mysql服务（ctrl+C 关闭另一个命令窗口），然后重启mysql服务
然后使用你的root帐号登录 

mysqladmin -u root password 你的密码

网上是这么写的，但我这样出现了下面的错误

Error: Access denied for user 'root'@'localhost' (using password: YES)

原因是ROOT  的密码没设，或 者有错误，网上搜了许多的方法都不行，最后这个成功了，不过必须是主机上执行。

直接运行命令行窗口输入下面的
mysqladmin -u root password 你的密码

这样就行了，然后再使用 mysqladmin -u root password 你的密码 就可以正常登录了。

7.安装WINDOWS服务

命令行窗口 CD 进入D:\MySql\bin
执行mysqld.exe --install MySQL5.5 --defaults-file="D:\MySql\my.ini"

net start mysql5.5

到服务器里把 MYSQL5.5改成自动，这样每次开机MYSQL服务就会自动启动了。

C:\Users\allen>mysqld --install MYSQL5.7 --defaults-file="D:\soft_work\mysql-5.7

.15\my.ini"

Install/Remove of the Service Denied!

cmd以管理员身份运行就可以了。
来源：  http://www.cnblogs.com/hikarusun/archive/2012/04/26/2471039.html
```

