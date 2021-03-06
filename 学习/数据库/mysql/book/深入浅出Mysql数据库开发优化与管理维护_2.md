[TOC]
##视图
```
视图是一个虚拟表，其内容由查询定义。同真实的表一样，视图包含一系列带有名称的列和行数据。但是，视图并不在 数据库 中以存储的数据值集形式存在。行和列数据来自由定义视图的查询所引用的表，并且在引用视图时动态生成。

```
##视图优势
```
视图对于普通表的优势表现在：
1.简单：使用视图的用户完全不用关心后面对应表的结构 关联条件 和筛选条件，对于用户来说已经是过滤好的复合条件的结果集。
2.安全：使用视图的用户只能访问他们被允许查询的结果集，对表的权限管理并不能限制到某个行某个列，但是通过视图就可以简单的实现。
3.数据独立，一旦视图的结构确定了，可以屏蔽表结构变化对用户的影响，源表增加列对视图没有影响，源表修改列名，则可以通过修改视图来解决，不会造成对访问者的影响。
```

##创建视图
```
create or replace view staff_list_view as 
select s.staff_id, s.first_name,s.last_name , a.address
from staff s, address a
where s.address_id = a.address_id;
```
##视图的可更新性与视图中查询的定义有关系
```

以下类型的视图是不可更新的


包含以下关键字的SQL:
SUM /MIN /MAX /COUNT/ DISTINCT /GROUP BY/ HAVING / UNION/ UNION ALL
--包含聚合函数
create or replace view payment_sum as 
select staff_id,sum(amount) from payment group by staff_id;

--常量视图
create or replace view pi as
select 3.1415926 as pi;

--select 中包含子查询
create view city_view as
select (select city from city where city_id = 1);


join

from 一个不能更新的视图
where 子句的子查询引用了from 子句中的表


```
##删除视图
drop view staff_list;
##查看视图
```
show tables 不仅显示表的名字，同时也显示视图的名字；
show table status like 'staff_list';
```
##lock table 和 unlock table
```
lock tables 可以锁定用于当前线程的表，如果表被其他线程锁定，则当前线程会等特，直到可以获取所有锁定为止。
unlock tables 可以释放当前线程获得的任何锁定。当前线程执行另一个 lock tables时，或当与服务器的连接被关闭时，所有由当前线程锁定的表被隐含地解锁。
```


##防sql注入的方法
```
/**
 * @desc  防大多数的sql注入
 * @param paraName 参数名称
 * @param paraType 参数类型(1表示数字或字符，0表示其他)
 */
function SafeRequest(paraName, paraType){
var $re = "";
if(paraType == 1){
$re = "/[^\w+$]/";
}else{
$re = "/(|\' |(\%27)|\;|(\%3b)|\=|(\%3d) |\(%28)|\)|(\%29)|(\/*)|(\%2f%2a)|(\*/)|(\%2a%2f)|\+(\%2b)|\<|(\%3c)|\>|(\%3e)|\(--))|\[|\%5b|\]|\%5d)/";
}

if(preg_match($re, paraName) > 0){
echo("参数不符合要求，请重新输入！");
return 0;
}else{
return 1;
}
}
```
##存储过程和函数的优势
```
可以将数据的处理放在数据库服务器上进行，避免将大量的结果集传输给客户端，减少数据的传输。
但在数据库服务器上进行大量的复杂运算，会占用服务器的CPU，造成数据库服务器的压力，所以不要在存储过程和函数中进行大量的复杂运算，并尽量将这些运算操作分摊到应用服务器上执行。
```


