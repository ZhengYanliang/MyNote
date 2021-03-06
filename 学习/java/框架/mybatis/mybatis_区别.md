##mapper文件：变量中#加与不加的区别
```
<insert id="insertSales" parameterType="map">
insert into t_sales_importLog 
(check_id,status,fileName,filePath,createTime,createPin) 
values 
(${check_id},${status},#{fileName},#{filePath},now(),#{createPin})
</insert>
如果变量的值为数值('1')可以用$，也可以用#；
如果变量的值为字符串('Frank')，不能用${createPin},只能用#{createPin}.
否则 Error updating database.  Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Unknown column 'Frank' in 'field list'
打印出来的sql: update .... set createPin=Frank ....
```
##mybatis # 与 $
```
1. #将传入的数据都当成一个字符串，会对自动传入的数据加一个双引号。如：order by #user_id#，如果传入的值是111,那么解析成sql时的值为order by "111", 如果传入的值是id，则解析成的sql为order by "id". 　
2. $将传入的数据直接显示生成在sql中。如：order by $user_id$，如果传入的值是111,那么解析成sql时的值为order by user_id,  如果传入的值是id，则解析成的sql为order by id. 　　
3. #方式能够很大程度防止sql注入。 　
4.$方式无法防止Sql注入。
5.$方式一般用于传入数据库对象，例如传入表名.
6.一般能用#的就别用$.

[#]可以自动帮我们拼接好字符串，
比如update t_sales_importcheck set entityInclude='8017,8018,8021,8048,8074,8099,8185,5876,11,1233123,1299479,9511,8011,5986'
而如果此时用$，就会报错。

MyBatis排序时使用order by 动态参数时需要注意，用$而不是#


字符串替换

默认情况下，使用#{}格式的语法会导致MyBatis创建预处理语句属性并以它为背景设置安全的值（比如?）。这样做很安全，很迅速也是首选做法，有时你只是想直接在SQL语句中插入一个不改变的字符串。比如，像ORDER BY，你可以这样来使用：
ORDER BY ${columnName}
这里MyBatis不会修改或转义字符串。
重要：接受从用户输出的内容并提供给语句中不变的字符串，这样做是不安全的。这会导致潜在的SQL注入攻击，因此你不应该允许用户输入这些字段，或者通常自行转义并检查。


ps:在使用mybatis中还遇到<![CDATA[]]>的用法，在该符号内的语句，将不会被当成字符串来处理，而是直接当成sql语句，比如要执行一个存储过程。

```
##Mybatis中jdbcType和javaType的对应关系
JDBC Type           Java Type  
CHAR                String  
VARCHAR             String  
LONGVARCHAR         String  
NUMERIC             java.math.BigDecimal  
DECIMAL             java.math.BigDecimal  
BIT                 boolean  
BOOLEAN             boolean  
TINYINT             byte  
SMALLINT            short  
INTEGER             int  
BIGINT              long  
REAL                float  
FLOAT               double  
DOUBLE              double  
BINARY              byte[]  
VARBINARY           byte[]  
LONGVARBINARY               byte[]  
DATE                java.sql.Date  
TIME                java.sql.Time  
TIMESTAMP           java.sql.Timestamp  
CLOB                Clob  
BLOB                Blob  
ARRAY               Array  
DISTINCT            mapping of underlying type  
STRUCT              Struct  
REF                         Ref  
DATALINK            java.net.URL[color=red][/color]

##Mybatis JdbcType与Oracle、MySql数据类型对应列表
Mybatis    JdbcType    Oracle    MySql
JdbcType    ARRAY        
JdbcType    BIGINT               BIGINT
JdbcType    BINARY        
JdbcType    BIT        BIT
JdbcType    BLOB       BLOB      BLOB
JdbcType    BOOLEAN        
JdbcType    CHAR       CHAR      CHAR
JdbcType    CLOB       CLOB      CLOB
JdbcType    CURSOR        
JdbcType    DATE       DATE      DATE
JdbcType    DECIMAL    DECIMAL   DECIMAL
JdbcType    DOUBLE     NUMBER    DOUBLE
JdbcType    FLOAT      FLOAT     FLOAT
JdbcType    INTEGER    INTEGER   INTEGER
JdbcType    LONGVARBINARY        
JdbcType    LONGVARCHAR  LONG    VARCHAR    
JdbcType    NCHAR      NCHAR    
JdbcType    NCLOB      NCLOB    
JdbcType    NULL        
JdbcType    NUMERIC   NUMERIC/NUMBER  NUMERIC/
JdbcType    NVARCHAR        
JdbcType    OTHER        
JdbcType    REAL       REAL       REAL
JdbcType    SMALLINT   SMALLINT   SMALLINT
JdbcType    STRUCT        
JdbcType    TIME       TIME
JdbcType    TIMESTAMP  TIMESTAMP  TIMESTAMP/DATETIME
JdbcType    TINYINT    TINYINT
JdbcType    UNDEFINED        
JdbcType    VARBINARY        
JdbcType    VARCHAR    VARCHAR    VARCHAR