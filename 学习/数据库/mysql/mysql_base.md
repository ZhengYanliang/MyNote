

##增加一列或一个字段
```




向已有的表t1中加入一列addr，这一列在表的最后一列位置:

alter table t1 add column addr varchar(20) not null;




如果我们希望添加在指定的一列，可以用：


alter table t1 add column addr varchar(20) not null after user1;


注意，上面这个命令的意思是说添加addr列到user1这一列后面。




如果想添加到第一列的话，可以用： alter table t1 add column addr varchar(20) not null first;



```
##mysql 字符串加变量拼接
```
INSERT INTO tb(content) VALUES(CONCAT("update ta set aname='",new.aname,"' where aid='",new.aid,"'"));

SELECT   CONCAT (  CAST (1  as   char ) ,  '2' )  AS   test;

```
##根据多个字段排序
```
分两种情况:
1.先根据状态升序排序再根据修改时间降序排序
SELECT *  FROM wx_push_task t   WHERE 1 = 1     and  t.PUBNUM_NUM = '123'  ORDER BY t.STATUS ASC， t.UPDATE_TIME DESC
2.先根据修改时间降序排序再根据状态升序排序
SELECT *  FROM wx_push_task t   WHERE 1 = 1     and  t.PUBNUM_NUM = '123'  ORDER BY  t.UPDATE_TIME DESC， t.STATUS ASC

``` ##blog相关类型
```
BLOB类型的字段用于存储二进制数据
MySQL中，BLOB是个类型系列，包括：TinyBlob、Blob、MediumBlob、LongBlob，这几个类型之间的唯一区别是在存储文件的最大大小上不同。
MySQL的四种BLOB类型

类型 大小(单位：字节)
TinyBlob 最大 255
Blob 最大 65K
MediumBlob 最大 16M
LongBlob 最大 4G
```