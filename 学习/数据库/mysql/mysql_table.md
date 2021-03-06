[TOC]
##mysql修改自增长的值
```
--修改原来的主键id(varchar(25)) 为 int 自增长
 ALTER TABLE `tab_common` CHANGE COLUMN `id` `id` INT(11) NOT NULL AUTO_INCREMENT ;

--获取表的当前的自增长的值
SHOW TABLE STATUS;

得出的结果里边对应表名记录中有个Auto_increment字段，里边有下一个自增ID的数值就是当前该表的最大自增ID.

-- 修改自增长的值
ALTER TABLE tb_name AUTO_INCREMENT=值
ALTER TABLE user12 AUTO_INCREMENT=100;

--mysql设置 自增主键
alter table `user` AUTO_INCREMENT = 1000;//修改user表的主键从1000开始

使用AUTO_INCREMENT时，应注意以下几点：
AUTO_INCREMENT是数据列的一种属性，只适用于整数类型数据列。
设置AUTO_INCREMENT属性的数据列应该是一个正数序列，所以应该把该数据列声明为UNSIGNED，这样序列的编号个可增加一倍。
AUTO_INCREMENT数据列必须有唯一索引，以避免序号重复。
AUTO_INCREMENT数据列必须具备NOT NULL属性。
AUTO_INCREMENT数据列序号的最大值受该列的数据类型约束，如TINYINT数据列的最大编号是127,如加上UNSIGNED，则最大为255。一旦达到上限，AUTO_INCREMENT就会失效。
```
##mysql修改表的字段
```
修改字段属性：
-- 修改字段属性
-- ALTER TABLE tb_name MODIFY 字段名称 字段类型 [完整性约束条件]
-- 将email字段 VARCHAR(50)修改成VARCHAR(200)
-- 注意，修改时如果不带完整性约束条件，原有的约束条件将丢失，如果想保留修改时就得带上完整性约束条件
ALTER TABLE user10 MODIFY email VARCHAR(200) NOT NULL DEFAULT 'a@a.com';

-- 将card移到test后面
ALTER TABLE user10 MODIFY card CHAR(10) AFTER test;

-- 将test放到第一个，保留原完整性约束条件
ALTER TABLE user10 MODIFY test CHAR(32) NOT NULL DEFAULT '123' FIRST;

修改字段名称和属性：
-- 将test字段改为test1
-- ALTER TABLE 表名 CHANGE 原字段名 新字段名 字段类型 约束条件
ALTER TABLE user10 CHANGE test test1 CHAR(32) NOT NULL DEFAULT '123';
 

添加删除默认值：
-- 创建新表
CREATE TABLE user11(
id TINYINT UNSIGNED KEY AUTO_INCREMENT,
username VARCHAR(20) NOT NULL UNIQUE,
age TINYINT UNSIGNED
);

-- 给age添加默认值
ALTER TABLE user11 ALTER age SET DEFAUTL 18;
-- 添加一个字段
ALTER TABLE user11 ADD email VARCHAR(50);
-- 给email添加默认值
ALTER TABLE user11 ALTER email SET DEFAULT 'a@a.com';

-- 删除默认值
ALTER TABLE user11 ALTER age DROP DEFAULT;
ALTER TABLE user11 ALTER email DROP DEFAULT;
 
添加主键：
-- 创建一个表
CREATE TABLE test12(
id INT
);
-- 添加主键
-- ALTER TABLE tb_name ADD [CONSTRAINT [sysmbol]] PRIMARY KEY [index_type] (字段名称,...)
ALTER TABLE test12 ADD PRIMARY KEY(id);

-- 添加复合主键
-- 先创建个表
CREATE TABLE test13(
id INT,
card CHAR(18),
username VARCHAR(20) NOT NULL
);
-- 添加复合主键
ALTER TABLE test13 ADD PRIMARY KEY(id,card);

删除主键：
-- 删除主键
ALTER TABLE test12 DROP PRIMARY KEY;

-- 再给test12添加主键, 完整形式
ALTER TABLE test12 ADD CONSTRAINT symbol PRIMARY KEY index_type(id);
在删除主键时，有一种情况是需要注意的，我们知道具有自增长的属性的字段必须是主键，如果表里的主键是具有自增长属性的；那么直接删除是会报错的。如果想要删除主键的话，可以先去年自增长属性，再删除主键

-- 再创建一个表，
CREATE TABLE test14(
id INT UNSIGNED KEY AUTO_INCREMENT
);

-- 删除主键，这样会报错，因为自增长的必须是主键
ALTER TABLE test14 DROP PRIMARY KEY;

-- 先用MODIFY删除自增长属性，注意MODIFY不能去掉主键属性
ALTER TABLE test14 MODIFY id INT UNSIGNED;
-- 再来删除主键
ALTER TABLE test14 DROP PRIMARY KEY;
 

唯一索引：
-- 添加唯一性约束
-- ALTER TABLE tb_name ADD [CONSTANT [symbol]] UNIQUE [INDEX | KEY] [索引名称](字段名称,...)


-- 创建测试表
CREATE TABLE user12(
id TINYINT UNSIGNED KEY AUTO_INCREMENT,
username VARCHAR(20) NOT NULL,
card CHAR(18) NOT NULL,
test VARCHAR(20) NOT NULL,
test1 CHAR(32) NOT NULL
);

-- username添加唯一性约束，如果没有指定索引名称，系统会以字段名建立索引
ALTER TABLE user12 ADD UNIQUE(username);
-- car添加唯一性约束
ALTER TABLE user12 ADD CONSTRAINT symbol UNIQUE KEY uni_card(card);
-- 查看索引
SHOW CREATE TABLE user12;

-- test,test1添加联合unique
ALTER TABLE user12 ADD CONSTRAINT symbol UNIQUE INDEX mulUni_test_test1(test, test1);


-- 删除唯一
-- ALTER TABLE tb_name DROP {INDEX|KEY} index_name;
-- 删除刚刚添加的唯一索引
ALTER TABLE user12 DROP INDEX username;
ALTER TABLE user12 DROP KEY uni_card;
ALTER TABLE user12 DROP KEY mulUni_test_test1;
 


修改表的存储引擎：
-- 修改表的存储引擎
-- ALTER TABLE tb_name ENGINE=存储引擎名称
ALTER TABLE user12 ENGINE=MyISAM;
ALTER TABLE user12 ENGINE=INNODB;
```
##mysql 复制表结构 或 表数据
```
1.只复制表结构不复制表数据
方式一：
CREATE TABLE 新表 SELECT * FROM 旧表 WHERE 1=2 
或CREATE TABLE 新表  LIKE 旧表 
方式二：
SELECT * INTO 表2 FROM 表1 WHERE 1=2 


2.复制表结构+数据
方式一：
CREATE TABLE 新表 SELECT * FROM 旧表 
delete from newtable;来删除考贝过来的数据
，但是新表中没有了旧表的primary key、Extra（auto_increment）等属性 

方式二：
INSERT INTO 新表 SELECT * FROM 旧表  (假设两个表结构一样) 

方式三：
INSERT INTO 新表(字段1,字段2,.......) SELECT 字段1,字段2,...... FROM 旧表  (假设两个表结构不一样) 

3.将表1数据复制到表2
方式一：
SELECT * INTO 表2 FROM 表1 
方式二：
show create table 旧表; 
这样会将旧表的创建命令列出。我们只需要将该命令拷贝出来，更改table的名字，就可以建立一个完全一样的表 

方式三：
mysqldump 
用mysqldump将表dump出来，改名字后再导回去或者直接在命令行中运行
```
##table相关
```
ALTER TABLE `tab_common` ADD COLUMN sex INT(2) NOT NULL AFTER username; //增加一列
ALTER TABLE `tab_common` MODIFY age VARCHAR(200) AFTER sex; //调整列的顺序
ALTER TABLE `tab_common` MODIFY COLUMN age INT(3); //改变字段类型及长度
```
##获取表各个字段的注释
```
方式一：
SELECT TABLE_NAME, COLUMN_NAME, DATA_TYPE, COLUMN_COMMENT FROM information_schema.columns WHERE table_name = 'tab_common';

TABLE_NAME  COLUMN_NAME  DATA_TYPE  COLUMN_COMMENT  
----------  -----------  ---------  ----------------
tab_common  id           varchar    用户Id        
tab_common  deptCode     varchar    部门编码    
tab_common  username     varchar    姓名          
tab_common  sex          int        性别          
tab_common  age          int        年龄          
tab_common  passwd       varchar    密码    
方式二：
SHOW FULL COLUMNS FROM tab_common;
```