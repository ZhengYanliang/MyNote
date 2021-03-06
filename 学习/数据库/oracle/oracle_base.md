##登录oracle
```
登陆oracle的三种方法
1、在DOS窗口中，输入sqlplus,回车后，输入用户名scott，密码：tiger
2、在浏览器中输入http://127.0.0.1:端口号/isqlplus,之后输入用户名和密码
3、在“应用程序开发”中选择SQL Plus，输入用户名和密码
使用超级管理员登陆
--在dos中，输入sqlplus sys/密码  as sysdba
```

##查看用户权限
```
--oracle用户查看自己的权限和角色
  select * from user_tab_privs;
  select * from user_role_privs;
--sys用户查看任一用户的权限和角色
  select * from dba_tab_privs;
  select * from dba_role_privs;
--授权
grant create view to scott;
```


##启动服务
```
mysql
net start mysql
oracle

有三种方式：
--通过控制面板启动oracle服务
  1）选择开始 > 控制面板 〉管理工具 --〉服务
  2）找到你所要启动的oracle服务，单击启动
  
--通过MS-DOS命令启动oracle服务
  1）打开DOS窗口
  2）在窗口中输入：NET START OracleServiceName
  或：
  net start OracleCSService
  net start OracleDBConsoleorcl
  net start OracleOraDb10g_home1iSQL*Plus
  net start OracleOraDb10g_home1TNSListener
  
--通过Oracle Administration Assistant for WindowsNT启动oracle服务
  
  1）选择开始 〉程序 〉Oracle - HOME_NAME > Configuration and Migration
    Tools > Oracle Administration Assistant for Windows NT.
  2）找到并右键单击oracle sid
  3）选择启动
```

##关闭服务
```
mysql

net stop mysql
oracle

net stop OracleCSService
net stop OracleDBConsoleorcl
net stop OracleOraDb10g_home1iSQL*Plus
net stop OracleOraDb10g_home1TNSListener
```

##查看当前连接数
```
mysql

首先在cmd命令下进入到mysql的bin所在的目录下：
D:\mysql-5.0\bin>mysqladmin -uroot -proot processlist
oracle

首先以管理员身份登录：
C:\>sqlplus sys/password@192.168.123.101:1521/orcl as sysdba
SQL> show user
USER 为 "SYS"
SQL> select count(*) from v$session #当前的连接数
SQL> Select count(*) from v$session where status='ACTIVE' #并发连接数
SQL> select value from v$parameter where name = 'processes' --数据库允许的最大连接数
SQL> show parameter processes #最大连接
SQL> select username,count(username) from v$session where username is not null group by username; #查看不同用户的连接数
```

##修改最大连接数
```
alter system set processes = 300 scope = spfile;
```

##查看当前有哪些用户正在使用数据
```
SELECT osuser, a.username,cpu_time/executions/1000000||'s', sql_fulltext,machine
from v$session a, v$sqlarea b
where a.sql_address =b.address order by cpu_time/executions desc;
```

##查询当前登录用户
```
mysql

sql> select user();
oracle:

一、查看当前用户信息：

1、查看当前用户拥有的角色权限信息：select * from role_sys_privs;

2、查看当前用户的详细信息：select * from user_users;

3、查看当前用户的角色信息：select * from user_role_privs;
```

##查询所有的数据库
```
mysql
mysql> show databases;
oracle
选择数据库
mysql
use db_name; // db_name表示数据库的名字
oracle
首先管理员身份登录:
C:\>sqlplus sys/password@192.168.123.101:1521/orcl as sysdba
SQL> show user
USER 为 "SYS"
SQL> show parameter db_name;
```


##代码注释
```
mysql
mysql> SELECT 1+1;     # 这个注释直到该行结束
mysql> SELECT 1+1;     -- 这个注释直到该行结束
mysql> SELECT 1   /* 这是一个在行中间的注释 */ + 1;
oracle
-- select * from user;
java
// /* code */
```

##查看当前用户建立的所有存储过程
```
--查看当前用户建立的所有存储过程
Select * From User_Procedures;
Select * From user_objects Where object_type='PROCEDURE';
查询当前数据下所有的表
mysql
mysql> show tables;

oracle
查询当前数据库中的所有表空间和对应的数据文件语句命令(前提要以管理员身份登录)
 
C:\>sqlplus sys/password@192.168.123.101:1521/orcl as sysdba
SQL> show user
USER 为 "SYS"
SQL>col file_name for a60;
SQL>set linesize 160;
SQL>select file_name,tablespace_name,bytes from dba_data_files;
 
FILE_NAME                                          TABLESPACE_NAME                     BYTES
-------------------------------------------------- ------------------------------ ----------
C:\ORACLE\PRODUCT\10.2.0\ORADATA\ORCL\USERS01.DBF  USERS                             5242880
C:\ORACLE\PRODUCT\10.2.0\ORADATA\ORCL\SYSAUX01.DBF SYSAUX                          283115520
C:\ORACLE\PRODUCT\10.2.0\ORADATA\ORCL\UNDOTBS01.DB UNDOTBS1                         41943040
C:\ORACLE\PRODUCT\10.2.0\ORADATA\ORCL\SYSTEM01.DBF SYSTEM                          513802240
C:\ORACLE\PRODUCT\10.2.0\ORADATA\ORCL\EXAMPLE01.DB EXAMPLE                         104857600
C:\ORACLE\PRODUCT\10.2.0\ORADATA\ORCL\HM14.DBF     HM14                             33554432
C:\ORACLE\PRODUCT\10.2.0\ORADATA\ORCL\BJ0515.DBF   BJ0515                           33554432
已选择 7 行。
查询用户所有表,一般使用：
SQL> col comments for a30;
SQL> set linesize 120;
SQL> select t.table_name,t.comments from user_tab_comments t;
 
TABLE_NAME                     COMMENTS
------------------------------ ---------------
DEPT
EMP
BONUS
SALGRADE
TABLE3
 
---------------------------------------------------------
select * from all_tab_comments
-- 查询所有用户的表,视图等
  
select * from user_tab_comments 
-- 查询本用户的表,视图等
  
select * from all_col_comments
  
--查询所有用户的表的列名和注释.
  
select * from user_col_comments
或
select t.TABLE_NAME,t.COLUMN_NAME,t.COMMENTS from user_col_comments t group by t.TABLE_NAME,t.COLUMN_NAME,t.COMMENTS order by t.table_name
（select后面的列名要全部出现在 group by 子句中）
-- 查询本用户的表的列名和注释
  
select * from all_tab_columns
--查询所有用户的表的列名等信息(详细但是没有备注).
  
select * from user_tab_columns
--查询本用户的表的列名等信息(详细但是没有备注).
查询表结构 (比如有表user)
mysql


mysql> desc uesr;
oracle


pl/sql的command window中
SQL>desc user;
或
右键-》view-》column/view sql.
```


##取前10条记录
```
mysql


mysql 没有top的用法。取而代之的是limit m,n
取前2条记录：   mysql> select * from t_test1 limit 2;
取第2到4条记录：   mysql> select * from t_test1 limit 1,4;(不包括m,包括n)
oracle


取前4条记录：
    SQL> select * from emp where rownum < 5;
取出第3到5条记录：
    select *
    from (select t.*,rownum rn
             from emp t
             order by empno desc)
    where rn >=3 and rn <=5
```


##日期格式转换
```
mysql


mysql> select DATE_FORMAT(now(),'%Y-%m-%d');
+-------------------------------+
| DATE_FORMAT(now(),'%Y-%m-%d') |
+-------------------------------+
| 2015-07-25                    |
+-------------------------------+
mysql> select DATE_FORMAT(now(),'%Y-%M-%d');
+-------------------------------+
| DATE_FORMAT(now(),'%Y-%M-%d') |
+-------------------------------+
| 2015-July-25                  |
+-------------------------------+
mysql> select DATE_FORMAT(now(),'%H:%i:%s');
+-------------------------------+
| DATE_FORMAT(now(),'%H:%i:%s') |
+-------------------------------+
| 12:57:47                      |
+-------------------------------+
1 row in set (0.07 sec)
```


##now()和sysdate()的区别
```
now() 在执行开始时值就得到了， sysdate() 在函数执行时动态得到值
 
mysql> select now(),sleep(2),sysdate();
+---------------------+----------+---------------------+
| now()               | sleep(2) | sysdate()           |
+---------------------+----------+---------------------+
| 2015-07-25 12:53:58 |        0 | 2015-07-25 12:54:00 |
+---------------------+----------+---------------------+
1 row in set (2.08 sec)
 
mysql> select now(),sleep(2),now();
+---------------------+----------+---------------------+
| now()               | sleep(2) | now()               |
+---------------------+----------+---------------------+
| 2015-07-25 12:54:16 |        0 | 2015-07-25 12:54:16 |
+---------------------+----------+---------------------+
1 row in set (2.55 sec)
oracle


将2015-07-15 11:01:23 转换为2015-07-15:
to_char('2015-07-15 11:01:23','yyyy-MM-dd')
 
select sysdate from dual;  //获取当前时间
select to_char(sysdate,'yyyy-MM-dd') as time from dual //转换为年月日
select to_char(sysdate,'yyyy-mm-dd hh24:mi:ss') as nowTime from dual;   //日期转化为字符串  
select to_char(sysdate,'yyyy') as nowYear   from dual;   //获取时间的年  
select to_char(sysdate,'mm')    as nowMonth from dual;   //获取时间的月  
select to_char(sysdate,'dd')    as nowDay    from dual;   //获取时间的日  
select to_char(sysdate,'hh24') as nowHour   from dual;   //获取时间的时  
select to_char(sysdate,'mi')    as nowMinute from dual;   //获取时间的分  
select to_char(sysdate,'ss')    as nowSecond from dual;   //获取时间的秒
to_char 是把日期或数字转换为字符串
to_date 是把字符串转换为数据库中得日期类型
```


##Java代码取数据库中的第一个字段的值
```
在ResultSet中，下标是从1开始的。
 ResultSet中的rs.getInt(1)出来的数据是int型(前提是要保证数据库里的第一个字段也是整数)
  
通过索引或者列名来获得查询结果集中的某一列的值
rs:数据集。
rs.getInt(int index);
rs.getInt(String columName)
  
比如：
现有表User：列有id,name.
String sql="select * from User";
ResultSet rs = null;
rs = st.executeQuery(sql);
while(rs.next){ // 必须要有rs.next()才可以
    rs.getInt(1); //等价于rs.getInt("id");
    rs.getString(2); //等价于rs.getString("name");
}
```


##增加字段
```
ALTER TABLE 表名 ADD 系统时间字段 DATE DEFAULT SYSDATE; --新增 
ALTER TABLE 表名 MODIFY 要修改的字段 VARCHAR2（12）; --修改
查看建表语句
mysql


mysql> show create table user;


oracle
打开pl/sql，右击表-》view-》view sql，就可看到
修改表字段(字段名或长度)
--修改字段名:alter table <table_name> rename column <column_old> to <column_new>;
--修改字段长度：alter table <table_name> modify <column> <datatype>;
例如：
--修改字段名,修改student表的t_name字段为stu_name
  alter table student rename column t_name to stu_name;
--修改student表的stu_name字段长度为64
  alter table student modify stu_name varchar2(64) 
--修改student表的stu_id字段长度为32，同时修改stu_name字段长度为64
  alter table student modify (stu_id varchar(32) default null,stu_name varchar2(64));
 
--Oracle修改字段类型和长度语句：
  ALTER TABLE tableName modify(columnName 类型); 例如 alter table test modify(name varchar(255));
更新表字段对应的值
mysql
同oracle


oracle
--简洁用法
  update user set name = 'adanac' where id = 101
--使用别名
  update a set a.name=b.name from table1 a inner join table2 b on a.id=b.id
```


##查询出最新的一条记录
```
比如：user_common表中有synflag 和 time字段，字段synflag相同的多条记录，查询出最新的一条记录
sql> select max(time),synflag from user_common t where t.id='134189' group by synflag
sql> select * from tabname t1
     where t1.data_id = ?
     and t1.time = (select max(t2.time)
                    from tabname t2
                    where t2.data_id = ?
                    and t2.time > sysdate - 1/24/60);
sql> select * from user_common t where t.updatetime = to_date('最大时间:2015-7-30 14:23:31','yyyy-mm-dd hh24:mi:ss');
sql> select * from user_common t where t.id='178312' and t.updatetime = (select max(t2.updatetime) from user_common t2 where t2.id='178312')
更新当前id下时间最新的一条记录
背景：查询当前id，出现n条记录，但只想更新时间最新的那条记录。
sql> select * from user_common t where t.id = '1776902';
sql> update user_common t set t.synflag = 0 where t.id = '1776902' and t.updatetime = (select max(t2.updatetime) from user_common t2 where t2.id = '1776902')
```


##TRUNC函数的用法
```
TRUNC函数用于对值进行截断。
用法有两种：TRUNC（NUMBER）表示截断数字，TRUNC（date）表示截断日期。
--截断数字：
  格式：TRUNC（n1,n2），n1表示被截断的数字，n2表示要截断到那一位。n2可以是负数，表示截断小数点前。
  注意，TRUNC截断不是四舍五入。
  SQL> select TRUNC(15.79) from dual;  --15
  SQL> select TRUNC(15.79,1) from dual;  --15.7
  SQL> select trunc(15.79,-1) from dual;  --10
--截断日期：
  先执行命令：alter session set nls_date_format='yyyy-mm-dd hh24:mi:hh';
 --截取今天
  SQL> select sysdate,trunc(sysdate,'dd') from dual;  
 --截取本周第一天：
  SQL> select sysdate,trunc(sysdate,'d') from dual;
 --截取本月第一天： 
  SQL> select sysdate,trunc(sysdate,'mm') from dual;
 --截取本年第一天：
  SQL> select sysdate,trunc(sysdate,'y') from dual;
 --截取到小时：
  SQL> select sysdate,trunc(sysdate,'hh') from dual;
 --截取到分钟：
  SQL> select sysdate,trunc(sysdate,'mi') from dual;
 --获取上月第一天：
  SQL> select TRUNC(add_months(SYSDATE,-1),'MM') from dual
group by
--根据transform_line表中的flag字段分组，统计不同的flag有多少条记录
   sql> select t.flag count(t.id) from transform_line t group by t.flag
```


##sql之left join、right join、inner join的区别
```
left join(左联接) 返回包括左表中的所有记录和右表中联结字段相等的记录 
right join(右联接) 返回包括右表中的所有记录和左表中联结字段相等的记录
inner join(等值连接) 只返回两个表中联结字段相等的行
举例如下： 
--------------------------------------------
表A记录如下：
aID　　　　　aNum
1　　　　　a20050111
2　　　　　a20050112
3　　　　　a20050113
4　　　　　a20050114
5　　　　　a20050115
表B记录如下:
bID　　　　　bName
1　　　　　2006032401
2　　　　　2006032402
3　　　　　2006032403
4　　　　　2006032404
8　　　　　2006032408
--------------------------------------------
1.left join
sql语句如下: 
select * from A
left join B 
on A.aID = B.bID
结果如下:
aID　　　　　aNum　　　　　bID　　　　　bName
1　　　　　a20050111　　　　1　　　　　2006032401
2　　　　　a20050112　　　　2　　　　　2006032402
3　　　　　a20050113　　　　3　　　　　2006032403
4　　　　　a20050114　　　　4　　　　　2006032404
5　　　　　a20050115　　　　NULL　　　　　NULL
（所影响的行数为 5 行）
结果说明:
left join是以A表的记录为基础的,A可以看成左表,B可以看成右表,left join是以左表为准的.
换句话说,左表(A)的记录将会全部表示出来,而右表(B)只会显示符合搜索条件的记录(例子中为: A.aID = B.bID).
B表记录不足的地方均为NULL.
2.right join 与 left join正好相反。
3.inner join
sql语句如下: 
select * from A
innerjoin B 
on A.aID = B.bID
结果如下:
aID　　　　　aNum　　　　　bID　　　　　bName
1　　　　　a20050111　　　　1　　　　　2006032401
2　　　　　a20050112　　　　2　　　　　2006032402
3　　　　　a20050113　　　　3　　　　　2006032403
4　　　　　a20050114　　　　4　　　　　2006032404
结果说明:
很明显,这里只显示出了 A.aID = B.bID的记录.这说明inner join并不以谁为基础,它只显示符合条件的记录.
```


##pl/sql执行了drop table 命令如何恢复
```
对误删的表，只要没有使用PURGE永久删除选项，那么从flashback区恢复回来希望是挺大的。
一般步骤有：
1、从flashback里查询被删除的表
select * from recyclebin
2.执行表的恢复
flashback table  tb  to before drop,这里的tb代表你要恢复的表的名称【ORIGINAL_NAME】。
或
SQL>flashback table "BIN$b+XkkO1RS5K10uKo9BfmuA==$0" to before drop;
注：必须9i或10g以上版本支持，flashback无法恢复全文索引
```


##oracle 查看用户所在的表空间
```
   查看当前用户的缺省表空间
　　SQL>select username,default_tablespace from user_users;
　　查看当前用户的角色
　　SQL>select * from user_role_privs;
　　查看当前用户的系统权限和表级权限
　　SQL>select * from user_sys_privs;
　　SQL>select * from user_tab_privs;
　　查看用户下所有的表
　　SQL>select * from user_tables;
　　1、用户
　　查看当前用户的缺省表空间
　　SQL>select username,default_tablespace from user_users;
　　查看当前用户的角色
　　SQL>select * from user_role_privs;
　　查看当前用户的系统权限和表级权限
　　SQL>select * from user_sys_privs;
　　SQL>select * from user_tab_privs;
　　显示当前会话所具有的权限
　　SQL>select * from session_privs;
　　显示指定用户所具有的系统权限
　　SQL>select * from dba_sys_privs where grantee='GAME';
　　2、表
　　查看用户下所有的表
　　SQL>select * from user_tables;
　　查看名称包含log字符的表
　　SQL>select object_name,object_id from user_objects
　　where instr(object_name,'LOG')>0;
　　查看某表的创建时间
　　SQL>select object_name,created from user_objects where object_name=upper('&table_name');
　　查看某表的大小
　　SQL>select sum(bytes)/(1024*1024) as "size(M)" from user_segments
　　where segment_name=upper('&table_name');
　　查看放在ORACLE的内存区里的表
　　SQL>select table_name,cache from user_tables where instr(cache,'Y')>0;
　　3、索引
　　查看索引个数和类别
　　SQL>select index_name,index_type,table_name from user_indexes order by table_name;
　　查看索引被索引的字段
　　SQL>select * from user_ind_columns where index_name=upper('&index_name');
　　查看索引的大小
　　SQL>select sum(bytes)/(1024*1024) as "size(M)" from user_segments
　　where segment_name=upper('&index_name');
　　4、序列号
　　查看序列号，last_number是当前值
　　SQL>select * from user_sequences;
　　5、视图
　　查看视图的名称
　　SQL>select view_name from user_views;
　　查看创建视图的select语句
　　SQL>set view_name,text_length from user_views;
　　SQL>set long 2000; 说明：可以根据视图的text_length值设定set long 的大小
　　SQL>select text from user_views where view_name=upper('&view_name');
　　6、同义词
　　查看同义词的名称
　　SQL>select * from user_synonyms;
　　7、约束条件
　　查看某表的约束条件
　　SQL>select constraint_name, constraint_type,search_condition, r_constraint_name
　　from user_constraints where table_name = upper('&table_name');
　　SQL>select c.constraint_name,c.constraint_type,cc.column_name
　　from user_constraints c,user_cons_columns cc
　　where c.owner = upper('&table_owner') and c.table_name = upper('&table_name')
　　and c.owner = cc.owner and c.constraint_name = cc.constraint_name
　　order by cc.position;
　　8、存储函数和过程
　　查看函数和过程的状态
　　SQL>select object_name,status from user_objects where object_type='FUNCTION';
　　SQL>select object_name,status from user_objects where object_type='PROCEDURE';
　　查看函数和过程的源代码
　　SQL>select text from all_source where owner=user and name=upper('&plsql_name');
```


##查询某字段长度
```
--查询出user表中字段id长度大于4的名字
select t.name from user t where length(t.id)>4
查询字段为null的情况
--1.从表中查询出字段sex值为‘null’的情况
  select * from T_WORKER t where t.sex='null'
  --对立语句
  select * from T_WORKER t where t.sex<>'null' or nvl(t.sex,0)='0'
  等价于：
  select * from T_WORKER t where t.sex!='null' or nvl(t.sex,1)='1'
--2.查询出字段sex任何值都没有写的情况
  select * from T_WORKER t where nvl(t.sex,1)='1' 
  等价于：
  select * from T_WORKER t where t.sex is null
  --对立语句
  select * from T_WORKER t where t.sex is not null
--3.查询出字段sex值为‘null’或任何值都没有的情况
  select * from T_WORKER t where t.sex='null' or t.sex is null 
  --对立语句
  select * from T_WORKER t where t.sex<>'null'
  --等价于：
  select * from T_WORKER t where t.sex!='null'
```


##关于授权
```
grant all on tablename to A;
这样，A就拥有了B下面 tablename 这个表的所有权限。
同理如果只是想赋某种权限的话，以下语句可供参考：
grant create tablespace to A;
grant select on tabelname to A;
grant update on tablename to A;
grant execute on procedurename to A; 授权存储过程
grant update on tablename to A with grant option; 授权更新权限给A用户，A用户也可以将此权限继续授权给别人；
修改字段类型和长度
--Mysql的修改字段类型语句：
ALTER TABLE tableName modify column columnName 类型;
alter table test modify column name varchar(255);
--Oracle修改字段类型和长度语句：
ALTER TABLE tableName modify(columnName 类型);
alter table test modify(name varchar(255));
oracle 如何在网络映射器上创建表空间
```

## PLSQL连接远程ORACLE数据库 
```
首先要安装oracle客户端

1.进入oracle安装目录
C:\oraclexe\app\oracle\product\10.2.0\server\NETWORK\ADMIN下找到tnsnames.ora文件
2.在该文件中添加如下代码片段
orcl =
(DESCRIPTION =
    (ADDRESS_LIST =                    
      (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.199)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SERVICE_NAME = orcl)
    )
)
其中orcl为数据库名称，host后面为远程服务器的地址，port为oracle的端口
3.再次打开PLSQL则会在database中有orcl选项输入用户名密码就可以登陆了
```
## Oracle数据远程连接的四种设置方法和注意事项
```
　　第一种情况：
　　若oracle服务器装在本机上，那就不多说了，连接只是用户名和密码的问题了。不过要注意环境变量%ORACLE_HOME%/network/admin/是否设置。
　　第二种情况：
　　本机未安装oracle服务器，也未安装oracle客户端。但是安装了pl sql development、toad sql development、sql navigator等管理数据库的工具。在虚拟机或者另一台电脑上安装了oracle服务器，也就是虚拟机或者另一台电脑此时作为服务器。
　　这种情况下，本人以pl sql development远程连接ORACLE服务端数据库为例：
　　1、在安装oracle服务器的机器上搜索下列文件：


oci.dll
ocijdbc10.dll
ociw32.dll
orannzsbb10.dll
oraocci10.dll
oraociei10.dll
sqlnet.ora
tnsnames.ora
classes12.jar
ojdbc14.jar


　　把这些找到的文件复制放到一个文件夹,如 oraclient，将此文件夹复制到客户端机器上。如放置路径为 D:\oraclient。
　　2、配置tnsnames.ora，修改其中的数据库连接串。




oracledata =
(DESCRIPTION =
(ADDRESS_LIST =
(ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.0.58)(PORT = 1521))
(CONNECT_DATA =
(SERVICE_NAME = oracledata)
)


　　其中，oracledata是要连接的服务名；HOST = 192.168.0.58，是服务器IP地址；PORT = 1521是端口号。
　　3、添加第一个环境变量，名为TNS_ADMIN，值为tnsnames.ora文件所在路径（如：D:\oraclient，特别是重装后或其它操作，忘了TNS_ADMIN变量，plsql登陆就会报无法解析指定的连接标识符)，这是为了能够找到上面说的tnsnames.ora。这步是最重要的。
　　添加第二个环境变量（可有可无）：“NLS_LANG = SIMPLIFIED CHINESE_CHINA.ZHS16GBK”，(AMERICAN_AMERICA.US7ASCII 是ASCII编码类型，其它类型可自己到服务器看一下或网上查找一下)（本步骤暂时要做对，如果编码不对，会产生乱码）。
　　4、下载并安装PL SQL Developer配置应用：
　　打开PL SQL Developer，登入界面点取消，进入后选择菜单栏 tools->preferences->connection ：
　　Oracle Home=D:\oracleclient
　　OCI library=D:\oracleclient\oci.dll
　　5、再次打开plsql则会在database中有oracledata 选项输入用户名密码就可以登陆。
　　第三种情况：
　　本机未安装ORACLE服务器，但是安装了oracle客户端，也安装了pl sql development、toad sql development、sql navigator等管理数据库的工具。在虚拟机或者另一台电脑上安装了oracle服务器，也就是虚拟机或者另一台电脑此时作为服务器。
　　这种情况下，本人以pl sql development远程连接oracle服务端数据库为例：
　　1、打开oracle客户端中的net manager，配置要远程连接的数据库名、IP地址等，如果net manager中没有要远程连接的数据库名，则新建即可。
　　2、其他步骤与第二种情况中的2---5相同。　
    第四种情况：
　　本机未安装oracle服务器，也未安装pl sql development、toad sql development、sql navigator等管理数据库的工具，但是安装了oracle客户端。在虚拟机或者另一台电脑上安装了ORACLE服务器，也就是虚拟机或者另一台电脑此时作为服务器。
　　这种情况下，本人以oracle客户端中的sqlplus远程连接oracle服务端数据库为例：
　　1、打开oracle客户端中的net manager，配置要远程连接的数据库名、IP地址等，如果net manager中没有要远程连接的数据库名，则新建即可。
　　2、同第二种情况中的步骤二。
　　3、同第二种情况中的步骤三。
　　4、打开sqlplus：
　　（1）如果用sys用户登入，则用户名：sys 密码：xxxxxx 主机字符串：要连接的数据库名 as sysdba，登入即可。
　　（2）如果用其他用户登入，则用户名：xxx 密码：xxxxxx 主机字符串：要连接的数据库名，登入即可。
　　注意事项：
　　1、服务器端和客户端防火墙需要关闭；
　　2、我们经常会遇到监听器服务无法启动，那么需要打开Net Configuration Assistant修复，或者新建监听器服务。
　　3、数据库密码如果忘了怎么办？按照以下方法修改密码即可：
　　开始-->运行-->cmd
　　输入 ：sqlplus /nolog 回车
　　输入 ：connect / as sysdba 回车
　　用户解锁 : alter user system account unlock 回车
　　修改密码：alter user system identified by manager
　　4、怎样判断数据库是运行在归档模式下还是运行在非归档模式下？
　　进入dbastudio，历程--〉数据库---〉归档查看。
　　5、另外，如果本机和别的机子均安装了oracle服务器端，那么本机如果要连接别的机子，就必须修改环境变量。
```