##字符串反转char转int
```
char a = 'a';
int i = Integerp.parseInt(a+"");
```
##byte类型的取值范围是-128~127
```
public byte[] getBytes(Charset charset)
使用给定的 charset 将此 String 编码到 byte 序列，并将结果存储到新的 byte 数组.
```
##assert的用法
```
assert关键字语法很简单，有两种用法：
　　1、assert <boolean表达式>
　　如果<boolean表达式>为true，则程序继续执行。如果为false，则程序抛出AssertionError，并终止执行。
   eg: assert !file.getName().contains(".");
　　2、assert <boolean表达式> : <错误信息表达式>
　　如果<boolean表达式>为true，则程序继续执行。如果为false，则程序抛出java.lang.AssertionError，并输入<错误信息表达式>。
   eg: assert !file.getName().contains(".") : "文件名中不包含."
```
##Exception in thread "main" java.util.ConcurrentModificationException
```
在使用iterator.hasNext()操作迭代器的时候，如果此时迭代的对象发生改变，比如插入了新数据，或者有数据被删除。
则使用会报以下异常：
java.util.ConcurrentModificationException
at java.util.HashMap$HashIterator.nextEntry(HashMap.java:922)
at java.util.HashMap$KeyIterator.next(HashMap.java:956)
at org.allen.test.TestHashMap.testHashMap(TestHashMap.java:22)
public class TestHashMap {
public static void main(String[] args) {
testHashMap();
}
public static void testHashMap() {
Map<String, String> map = new HashMap<String, String>();
map.put("1", "bj");
map.put("2", "nj");
map.put("3", "dj");
Set<String> keySet = map.keySet();
Iterator<String> it = keySet.iterator();
while (it.hasNext()) {
String next = it.next();
map.remove(next); // wrong
// 遍历map
for (String key : keySet) {
System.out.println(key + ":" + map.get(key));
}
}
System.out.println("end");
}
}
--原因：Iterator做遍历的时候，HashMap被修改(bb.remove(ele), size-1)，Iterator(Object ele=it.next())会检查HashMap的size，size发生变化，抛出错误ConcurrentModificationException。
--解决1：
public static void test2() {
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(2);
Iterator<Integer> iterator = list.iterator();
while (iterator.hasNext()) {
Integer integer = iterator.next();
System.out.println(list.size());
if (integer == 2)
iterator.remove(); // 注意这个地方
}
System.out.println(list.size());
}
--解决2：
根据实际程序，您自己手动给Iterator遍历的那段程序加锁，给修改HashMap的那段程序加锁。
--解决3：
使用“ConcurrentHashMap”替换HashMap，ConcurrentHashMap会自己检查修改操作，对其加锁也可针对插入操作。
public static void test3() {
Map<String, String> map = new ConcurrentHashMap<String, String>();
map.put("1", "bj");
map.put("2", "nj");
Set<String> keySet = map.keySet();
Iterator<String> it0 = keySet.iterator();
System.out.println(map.size());
while (it0.hasNext()) {
String str = it0.next();
if ("1".equalsIgnoreCase(str)) {
map.remove(str);
}
}
System.out.println(map.size() + ":" + map.get("2"));
}
```
##java中int,char,string三种类型的相互转换
```
// String --> int
public static int str2Int(String str) {
int i = 0;
// i = Integer.valueOf(str).intValue();
i = Integer.parseInt(str);
return i;
}
// int --> string
public static String int2Str(int i) {
String str = null;
// str = String.valueOf(i);
// str = Integer.toString(i);
str = "" + i;
return str;
}
// int --> Integer
public static Integer int2Integer(int i) {
Integer integer = null;
integer = new Integer(i);
return integer;
}
// Integer --> int
public static int integer2Int(Integer integer) {
int i = integer.intValue();
return i;
}
// Integer --> String
public static String integer2Str(Integer integer) {
String str = integer.toString();
return str;
}
// String --> BigDecimal
public static BigDecimal str2BigDecimal(String str) {
BigDecimal bigDecimal = new BigDecimal(str);
return bigDecimal;
}
// String --> char[]
public static char[] str2Chars(String str) {
char[] chs = str.toCharArray();
return chs;
}
// char[] --> String
public static String char2Str(char[] chs) {
return new String(chs);
        // return String.valueOf(chs);
}
// String --> char
public static Set<char[]> str2CharSet(String str) {
Set<char[]> set = new HashSet<char[]>();
char[] chs = new char[str.length()]; // 初始化char[]
for (int i = 0; i < str.length(); i++) {
chs[i] = str.charAt(i);
}
set.add(chs);
return set;
}
// char --> String
public static String char2Str(char ch) {
return ch + "";
}
    
    // char --> int
    public static int char2Int(char ch) {
        return ch + 0;
    }
    
    // int --> char
    public static char int2Char(int i){
        return (char)i;
    }
```
##Collection ， List ， Set 和 Map 用法和区别
```
Collection 是对象集合， Collection 有两个子接口 List 和 Set
List 可以通过下标 (1,2..) 来取得值，值可以重复
而 Set 只能通过游标来取值，并且值是不能重复的
ArrayList ， Vector ， LinkedList 是 List 的实现类
ArrayList 是线程不安全的， Vector 是线程安全的，这两个类底层都是由数组实现的
LinkedList 是线程不安全的，底层是由链表实现的   
Map 是键值对集合
HashTable 和 HashMap 是 Map 的实现类   
HashTable 是线程安全的，不能存储 null 值   
HashMap 不是线程安全的，可以存储 null 值  
总结一：比较
1 ，数组 (Array) ，数组类 (Arrays)
Java 所有“存储及随机访问一连串对象”的做法， array 是最有效率的一种。但缺点是容量固定且无法动态改变。 array 还有一个缺点是，无法判断其中实际存有多少元素， length 只是告诉我们 array 的容量。
 
Java 中有一个数组类 (Arrays) ，专门用来操作 array 。数组类 (arrays) 中拥有一组 static 函数。
equals() ：比较两个 array 是否相等。 array 拥有相同元素个数，且所有对应元素两两相等。
fill() ：将值填入 array 中。
sort() ：用来对 array 进行排序。
binarySearch() ：在排好序的 array 中寻找元素。
System.arraycopy() ： array 的复制。
 
若编写程序时不知道究竟需要多少对象，需要在空间不足时自动扩增容量，则需要使用容器类库， array 不适用。
 
2 ，容器类与数组的区别
容器类仅能持有对象引用（指向对象的指针），而不是将对象信息 copy 一份至数列某位置。一旦将对象置入容器内，便损失了该对象的型别信息。
 
3 ，容器 (Collection) 与 Map 的联系与区别
Collection 类型，每个位置只有一个元素。
Map 类型，持有 key-value 对 (pair) ，像个小型数据库。
 
Collections 是针对集合类的一个帮助类。提供了一系列静态方法实现对各种集合的搜索、排序、线程完全化等操作。相当于对 Array 进行类似操作的类—— Arrays 。
如， Collections.max(Collection coll); 取 coll 中最大的元素。
    Collections.sort(List list); 对 list 中元素排序
 
List ， Set ， Map 将持有对象一律视为 Object 型别。
Collection 、 List 、 Set 、 Map 都是接口，不能实例化。继承自它们的 ArrayList, Vector, HashTable, HashMap 是具象 class ，这些才可被实例化。
vector 容器确切知道它所持有的对象隶属什么型别。 vector 不进行边界检查。
 
 
总结二：需要注意的地方
1 、 Collection 只能通过 iterator() 遍历元素，没有 get() 方法来取得某个元素。
2 、 Set 和 Collection 拥有一模一样的接口。但排除掉传入的 Collection 参数重复的元素。
3 、 List ，可以通过 get() 方法来一次取出一个元素。使用数字来选择一堆对象中的一个， get(0)... 。 (add/get)
4 、 Map 用 put(k,v) / get(k) ，还可以使用 containsKey()/containsValue() 来检查其中是否含有某个 key/value 。
HashMap 会利用对象的 hashCode 来快速找到 key 。
哈希码 (hashing) 就是将对象的信息经过一些转变形成一个独一无二的 int 值，这个值存储在一个 array 中。我们都知道所有存储结构中， array 查找速度是最快的。所以，可以加速查找。发生碰撞时，让 array 指向多个 values 。即，数组每个位置上又生成一个梿表。
5 、 Map 中元素，可以将 key 序列、 value 序列单独抽取出来。
使用 keySet() 抽取 key 序列，将 map 中的所有 keys 生成一个 Set 。
使用 values() 抽取 value 序列，将 map 中的所有 values 生成一个 Collection 。
为什么一个生成 Set ，一个生成 Collection ？那是因为， key 总是独一无二的， value 允许重复。
 
总结三：如何选择 
从效率角度：
在各种 Lists ，对于需要快速插入，删除元素，应该使用 LinkedList （可用 LinkedList 构造堆栈 stack 、队列 queue ），如果需要快速随机访问元素，应该使用 ArrayList 。最好的做法是以 ArrayList 作为缺省选择。 Vector 总是比 ArrayList 慢，所以要尽量避免使用。
在各种 Sets 中， HashSet 通常优于 HashTree （插入、查找）。只有当需要产生一个经过排序的序列，才用 TreeSet 。 HashTree 存在的唯一理由：能够维护其内元素的排序状态。
 
在各种 Maps 中 HashMap 用于快速查找。
最后，当元素个数固定，用 Array ，因为 Array 效率是最高的。
所以结论：最常用的是 ArrayList ， HashSet ， HashMap ， Array 。
 
更近一步分析：
如果程序在单线程环境中，或者访问仅仅在一个线程中进行，考虑非同步的类，其效率较高，如果多个线程可能同时操作一个类，应该使用同步的类。 
要特别注意对哈希表的操作，作为 key 的对象要同时正确复写 equals 方法和 hashCode 方法。 
尽量返回接口而非实际的类型，如返回 List 而非 ArrayList ，这样如果以后需要将 ArrayList 换成 LinkedList 时，客户端代码不用改变。这就是针对抽象编程。
StringBuffer.delete()方法
         String str = "abcdef";
StringBuffer sb = new StringBuffer(str);
sb.delete(1, 3);
System.out.println(sb);// adef
delete(int start, int end);//删除从第start个开始，到第end个结束【包括start,不包括end】
```


##java分割数字和汉字
```
String s = "1字符132串123456哈哈441";
Pattern p = Pattern.compile("[\\u4e00-\\u9fa5]+|\\d+");
Matcher m = p.matcher(s);
while (m.find()) {
    System.out.println(m.group());
}
```
##Eclipse中输入系统变量和运行参数
```
运行的方法是，右键——》run as——》run configuration——》Arguments——》program arguments.
--Program arguments栏里可以输入程序运行所需的参数，也就是main方法的参数，如果参数为多个，则用空格分开。
--VM arguments里接收的是系统变量参数，系统变量输入格式为：-Dargname=argvalue，同样，多个参数之间用空格隔开。另外如果参数值中间有空格，则用引号括起来
```

##Integer与int互转
```
--int转Integer
int i = 0;  
Integer wrapperi = new Integer(i);  
--Integer转int
Integer wrapperi = new Integer(0);  
int i = wrapperi.intValue();
```
##字符串反转
```
--方式1：
  String str = "abcd";
  System.out.println(new StringBuffer(str).reverse()); //dcba
--方式2：
  public static void reverse(String str) {
for (int i = str.length() - 1; i >= 0; i--) {
char c = str.charAt(i);
System.out.print(c);
}
}
--方式3：
public static void reverseWords() {
Scanner sc = new Scanner(System.in);
String str = sc.nextLine();
String[] sArr = str.split(" ");// I love java
List<String> list = new ArrayList<String>();
list = Arrays.asList(sArr);
// for(int i=0;i<sArr.length;i++){
// list.add(sArr[i]);
// }
Collections.reverse(list);
for (int i = 0; i < list.size(); i++) {
String word = list.get(i);
if (i == list.size() - 1) {
System.out.println(word);
} else {
System.out.print(word + " ");
}
}
}
```
##String转为boolean类型
```
System.out.println(Boolean.parseBoolean("true") ? "yes" : "no"); //yes
System.out.println("true" != null && "true".equalsIgnoreCase("true") ? "1" : "0");//1
```
##spring读取xml文件
```
1.比如：有一个javaBean(User.java)
public class User {
private String name;
private int age;
public String getName() {
return name;
}
public void setName(String name) {
this.name = name;
}
public int getAge() {
return age;
}
public void setAge(int age) {
this.age = age;
}
}
2.构造一个配置文件(user-config.xml)
<beans>  
 <bean id="userBean" class="org.allen.domain.User">  
  <property name="name">  
   <value>allenvalue>  
  property>  
 bean>  
beans> 
3.读取xml文件
--通过ClassPathXmlApplicationContext
ApplicationContext context = new ClassPathXmlApplicationContext("user-config.xml");   
User user = (User)context.getBean("userBean");   
System.out.println(user.getName());  
--通过FileSystemResource
Resource rs = new FileSystemResource("D:/software/tomcat/webapps/springWebDemo/WEB-INF/classes/beanConfig.xml"); 
BeanFactory factory = new XmlBeanFactory(rs);
User user = (User)factory.getBean("userBean");
System.out.println(user.getName());
```
##时间转换问题
```
将当前时间转换为yyyy-MM-dd的格式：
String currentTime = new SimpleDateFormat("yyyy-MM-dd").format(new Date());
SimpleDateFormat类中parse()方法用于将输入的特定字符串转换成Date类的对象,就是把字符串按照约定的格式转换成一个日期类型
Date date = new SimpleDateFormat("yyyy-MM-dd").parse("2015-8-9"); // Sun Aug 09 00:00:00 CST 2015
Date date = new SimpleDateFormat("yyyyMMddHHmmss")
.parse("20150809142511");  // Sun Aug 09 14:25:11 CST 2015
```

##获取字符/字符串在指定字符串中首次出现的位置
```
String path = "d:/test/abc.zip";
System.out.println(path.indexOf('t')); //3
System.out.println(path.indexOf("t")); //3
System.out.println(path.indexOf('t', 2)); //3
System.out.println(path.indexOf("t", 2)); //3
下标均是从0开始。
```





##Java代码取数据库中的第一个字段的值
```
在ResultSet中，下标是从1开始的。 
ResultSet中的rs.getInt(1)出来的数据是int型(前提是要保证数据库里的第一个字段也是整数) 
通过索引或者列名来获得查询结果集中的某一列的值
rs:数据集。
rs.getInt(int index);
rs.getInt(String columName); 
比如：现有表User：列有id,name.
String sql="select * from User";
ResultSet rs = null;
rs = st.executeQuery(sql);
while(rs.next){ // 必须要有rs.next()才可以
    rs.getInt(1); //等价于rs.getInt("id");
    rs.getString(2); //等价于rs.getString("name");
}
```
##取出ResultSet中查询结果的总记录数
```
String sql = "select count(*) num  from emp_message; // 注意在oracle中起别名是没有as关键字的
PreparedStatement pstm = OperaterOracle.getPstm(sql);
ResultSet rs = pstm.executeQuery();
rs.next();
int count = rs.getInt(1); 
// count = Integer.parseInt(rs.getString("num"));
return count;
为什么取不出来数据 ？代码没有问题，原因很有可能连接的是测试数据库，测试数据库里没有数据。
jdbc操作oracle数据库
public static Connection getConnection() {
String className = "oracle.jdbc.driver.OracleDriver";
String url = "jdbc:oracle:thin:@localhost:1521/orcl";
String user = "scott";
String password = "tiger";
Connection conn = null;
try {
Class.forName(className);
conn = DriverManager.getConnection(url, user, password);
} catch (ClassNotFoundException e) {
e.printStackTrace();
} catch (SQLException e) {
e.printStackTrace();
}
return conn;
}
public static String getValue(String key) {
return props.getProperty(key);
}
public static Connection getConnection2() {
InputStream inputStream = JdbcOracle.class.getClassLoader()
.getResourceAsStream("config2.properties");
props = new Properties();
try {
props.load(inputStream);
if (conn == null) {
// 加载驱动
Class.forName(getValue("jdbc.driver"));
String url = getValue("jdbc.url");
String user = getValue("jdbc.user");
String password = getValue("jdbc.password");
conn = DriverManager.getConnection(url, user, password);
}
} catch (IOException e) {
e.printStackTrace();
} catch (ClassNotFoundException e) {
e.printStackTrace();
} catch (SQLException e) {
e.printStackTrace();
}
return conn;
}
public static int insert(String username, String location) {
Connection conn = getConnection();
int i = 0;
String sql = "insert into dept(deptno,dname,loc) values(?,?,?)";
PreparedStatement pstmt;
try {
pstmt = conn.prepareStatement(sql);
pstmt.setInt(1, getMaxno() + 10);
pstmt.setString(2, username);
pstmt.setString(3, location);
i = pstmt.executeUpdate();
System.out.println("result:" + i);
pstmt.close();
conn.close();
} catch (SQLException e) {
e.printStackTrace();
}
return i;
}
private static int getMaxno() {
int maxno = 0;
Connection conn = getConnection();
String sql = "select max(deptno) from dept";
PreparedStatement pstmt;
ResultSet rs;
try {
pstmt = conn.prepareStatement(sql);
rs = pstmt.executeQuery();
while (rs.next()) {
maxno = rs.getInt(1);
System.out.println("max:" + maxno);
}
rs.close();
pstmt.close();
conn.close();
} catch (SQLException e) {
e.printStackTrace();
}
return maxno;
}
public static void query() {
Connection conn = getConnection();
String sql = "select * from dept";
PreparedStatement pstmt;
try {
pstmt = conn.prepareStatement(sql);
ResultSet rs = pstmt.executeQuery();
System.out.println("deptno" + "\t\t" + "depname" + "\t\t"
+ "location");
while (rs.next()) {
System.out.println(rs.getInt(1) + "\t" + rs.getString(2) + "\t"
+ rs.getString(3));
}
rs.close();
pstmt.close();
conn.close();
} catch (SQLException e) {
e.printStackTrace();
}
}
public static int update(String name, int id) {
Connection conn = getConnection();
String sql = "update dept set dname = '" + name + "' where deptno = "
+ id;
int i = 0;
PreparedStatement pstmt;
try {
pstmt = conn.prepareStatement(sql);
i = pstmt.executeUpdate();
System.out.println(i);
pstmt.close();
conn.close();
} catch (SQLException e) {
e.printStackTrace();
}
return i;
}
public static int update2(String name, int id) {
Connection conn = getConnection();
String sql = "update dept set dname = ? where deptno = ?";
int i = 0;
PreparedStatement pstmt;
try {
pstmt = conn.prepareStatement(sql);
pstmt.setString(1, name);
pstmt.setInt(2, id);
i = pstmt.executeUpdate();
System.out.println(i);
pstmt.close();
conn.close();
} catch (SQLException e) {
e.printStackTrace();
}
return i;
}
public static int delete(String id) {
Connection conn = getConnection();
PreparedStatement pstmt;
String sql = "delete from dept where deptno = ?";
int i = 0;
try {
pstmt = conn.prepareStatement(sql);
pstmt.setString(1, id);
i = pstmt.executeUpdate();
System.out.println(i);
pstmt.close();
conn.close();
} catch (SQLException e) {
e.printStackTrace();
}
return i;
}
public static int delete2(int id) {
Connection conn = getConnection();
Statement st;
String sql = "delete from dept where deptno = " + id;
int i = 0;
try {
st = conn.createStatement();
i = st.executeUpdate(sql);
System.out.println(i);
st.close();
conn.close();
} catch (SQLException e) {
e.printStackTrace();
}
return i;
}
```

##jdbc操作oracle数据库优化后
```
config2.properties:
jdbc.driver=oracle.jdbc.driver.OracleDriver
jdbc.url=jdbc:oracle:thin:@127.0.0.1:1521:orcl
jdbc.user=scott
jdbc.password=tiger
JdbcOracle.java:
public class JdbcOracle {

private static Properties props;
private static Connection conn;
static {
JdbcOracle.class.getClassLoader().getResourceAsStream(
"config.properties");
InputStream inputStream = JdbcOracle.class.getClassLoader()
.getResourceAsStream("config2.properties");
props = new Properties();
try {
props.load(inputStream);
if (conn == null) {
// 加载驱动
Class.forName(getValue("jdbc.driver"));
String url = getValue("jdbc.url");
String user = getValue("jdbc.user");
String password = getValue("jdbc.password");
conn = DriverManager.getConnection(url, user, password);
}
} catch (Exception e) {
e.printStackTrace();
}
}

private static String getValue(String key) {
return props.getProperty(key);
}
public static int insert(String username, String location) {
int i = 0;
String sql = "insert into dept(deptno,dname,loc) values(?,?,?)";
PreparedStatement pstmt = null;
try {
pstmt = conn.prepareStatement(sql);
pstmt.setInt(1, getMaxno() + 10);
pstmt.setString(2, username);
pstmt.setString(3, location);
i = pstmt.executeUpdate();
System.out.println("result:" + i);
} catch (SQLException e) {
e.printStackTrace();
} finally {
try {
if (pstmt != null) {
pstmt.close();
}
if (conn != null) {
conn.close();
}
} catch (SQLException e) {
e.printStackTrace();
}
}
return i;
}
}
```
## java.sql.Date转为java.util.Date
```
java.sql.Date date=new java.sql.Date();
java.util.Date d=new java.util.Date (date.getTime());
```
##java.util.Date转为java.sql.Date
```
java.util.Date utilDate=new Date();
java.sql.Date sqlDate=new java.sql.Date(utilDate.getTime());
 java.util.Date utilDate=new Date();
java.sql.Date sqlDate=new java.sql.Date(utilDate.getTime());
java.sql.Time sTime=new java.sql.Time(utilDate.getTime());
java.sql.Timestamp stp=new java.sql.Timestamp(utilDate.getTime());
 这里所有时间日期都可以被SimpleDateFormat格式化format()
SimpleDateFormat f=new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
f.format(stp);
f.format(sTime);
f.format(sqlDate);
f.format(utilDate)
java.sql.Date sqlDate=java.sql.Date.valueOf(" 2005-12-12");
utilDate=new java.util.Date(sqlDate.getTime());
```


##另类取得年月日的方法：
```
import java.text.SimpleDateFormat;
import java.util.*;
java.util.Date date = new java.util.Date();
//如果希望得到YYYYMMDD的格式SimpleDateFormat
sy1=new SimpleDateFormat("yyyyMMDD");
String dateFormat=sy1.format(date);
//如果希望分开得到年，月，日SimpleDateFormat
sy=new SimpleDateFormat("yyyy");
SimpleDateFormat sm=new SimpleDateFormat("MM");
SimpleDateFormat sd=new SimpleDateFormat("dd");
String syear=sy.format(date);
String smon=sm.format(date);
String sday=sd.format(date);
```


##字符串操作
```
fileDataEntity.getData(); // #1200^PMS_南京变^224^223
String[] str = fileDataEntity.getData().split("\\^"); // str=[#1200, PMS_南京变, 224, 223]
String id = str[0].replace("#", ""); // id = 1200 ,将str[0]中的#变为""
```


##启动一个线程是用run()还是start()
```
启动线程肯定要用start()方法。当用start()开始一个线程后，线程就进入就绪状态，使线程所代表的虚拟处理机处于可运行状态，这意味着它可以由JVM调度并执行。这并不意味着线程就会立即运行。当cpu分配给它时间时，才开始执行run()方法(如果有的话)。START()是方法,它调用RUN()方法.而RUN()方法是你必须重写的. run()方法中包含的是线程的主体。
继承Thread类的启动方式：
public class ThreadStartTest {
public static void main(String[] args) {
ThreadTest tt = new ThreadTest(); // 创建一个线程实例
tt.start(); // 启动线程
}
}
实现Runnable接口的启动方式：
public class RunnableStartTest {
public static void main(String[] args) {
Thread t = new Thread(new RunnableTest()); // 创建一个线程实例
t.start(); // 启动线程
}
}
实际上这两种启动线程的方式原理是一样的。首先都是调用本地方法启动一个线程，其次是在这个线程里执行目标对象的run()方法。那么这个目标对象是什么呢？为了弄明白这个问题，我们来看看Thread类的run()方法的实现：
public void run() { 
    if (target != null) { 
        target.run(); 
    } 
} 
总结起来就一句话，如果我们采用的是继承Thread类的方式，那么这个target就是线程对象自身，如果我们采用的是实现Runnable接口的方式，那么这个target就是实现了Runnable接口的类的实例。
而如果直接用Run方法，这只是调用一个方法而已，程序中依然只有主线程这一个线程，其程序执行路径还是只有一条，这样就没有达到写线程的目的。
```


##java 启动线程三种方式
```
1.继承Thread
public class JavaThread extends Thread {
public static void main(String args[]) {
(new JavaThread()).run();
System.out.println("main thread run ");
}
public synchronized void run() {
System.out.println("sub thread run ");
}
}
2.实现Runnable接口
public class JavaThread implements Runnable {
public static void main(String args[]) {
(new Thread(new JavaThread())).start();
System.out.println("main thread run ");
}
public void run() {
System.out.println("sub thread run ");
}
}
3.直接在函数体使用
void threadMethod() {
Thread t = new Thread(new Runnable() {
public void run() {
System.out.println("hello world");
}
});
t.start();
}
4.比较：
实现Runnable接口优势：
1）适合多个相同的程序代码的线程去处理同一个资源
2）可以避免java中的单继承的限制
3）增加程序的健壮性，代码可以被多个线程共享，代码和数据独立。
```
##继承Thread类优势
```
1）可以将线程类抽象出来，当需要使用抽象工厂模式设计时。
2）多线程同步
在函数体使用优势：
1）无需继承thread或者实现Runnable，缩小作用域。
用接口、类来描述关系
有一个“人”类，它有一个方法是GoHome回家，这个方法需要借助交通工具，现在人可以使用两种交通工具回家，一种是小汽车Car类，另一种是自行车Bicycle类。
//这是一个接口,Vehicle是交通工具的意思，这个接口定义了交通工具的一个共有的方法 
//drive（）驾驶 
public interface InVehicle {
public void drive();
}
//小汽车是交通工具，实现交通工具的接口 
public class Car implements InVehicle {
public void drive() {
// 这里具体实现小汽车的驾驶方法
}
}
//自行车也是交通工具，实现了交通工具的接口 
public class Bicycle implements InVehicle {
public void drive() {
// 这里具体实现自行车的驾驶方法
}
}
//这是一个“人”类，它有一个方法是 goHome，回家需要一种交通工具，所以他有一个交通工具,他回家时使用这个工具
public class Man {
private InfVehicle vehicle;
public InfVehicle getVehicle() {
return this.vehicle;
}
public void setVehicle(InfVehicle vehicle) {
this.vehicle = vehicle;
}
// 回家方法
public void goHome() {
this.vehicle.drive();
}
public static void main(String[] args) {
Man aMan = new Man(); // 创建一个 "人 "
InfVehicle car = new Car(); // 创建一个小汽车
InfVehicle bicycle = new Bicycle();// 创建一个自行车
// 比如今天这个人想开车回家，我们就
aMan.setVehicle(car);
aMan.goHome();
// 如果他开车开腻了，想换一种方式，他这可以骑车回家
aMan.setVehicle(bicycle);
aMan.goHome();
}
}
这样的代码的好处就是，如果有一天那个人说：“我不想开小汽车了，没意思，我也不想骑车，太累。我想开大卡车下班回家，那样够帅！”（注意：客户完全可以提出类似的要求）那我们怎么办呢？
在这里，人这个类使用交通工具的服务，人即是客户端，交通工具是服务端我们不想对客户端造成影响。因为在这个例子中使用了接口，我们就很好办。 比如我们可以添加一个卡车类Sixby实现交通工具的接口，这样如果这个人想开卡车下班回家，就传给它一个Sixby的实例，他就可以够帅了^_^
接口(Interface)是什么，为什么使用接口，如何恰当的使用接口
接口就是当你的模块需要外面提供一个功能的时候，你用来定义你期望的功能是什么样的。 
它作为对你的需要的一个规范，一个描述。 
你通过接口，精确描述你需要的“功能”，然后从外界接收一个该接口的对象。 
然后，你就可以使用这个接口了。 
它让你关注“接口”，关注“规范”，而不是“实现”。别人怎么实现，那是别人的事，与我无关。你可以对你自己需要实现的东西编码，编译，测试，即使你需要的功能别人还没有实现出来。 
根据需要，别人可以向你的模块传递不同的接口实现，从而达到重用的目的。 
举个例子： 
你给人写一个旅游指南：“先乘车去北海，在此吃午餐，下午去故宫。” 
至于乘bus还是taxi,   吃包子还是麦当劳，下午怎么去故宫，不要越俎代庖，使用者可以根据自己的客观情况决定。 
于是，乘车，吃午餐，去故宫，全是接口。你只管写你关心的逻辑，至于一些你不关心的实现细节，用接口抽象出来。 
最后：接口和多重继承没有关系。
```
##值类型和引用类型的区别
```
引用类型表示你操作的数据是同一个，也就是说当你传一个参数给另一个方法时，你在另一个方法中改变这个变量的值，那么调用这个方法是传入的变量的值也将改变.
值类型表示复制一个当前变量传给方法，当你在这个方法中改变这个变量的值时，最初生命的变量的值不会变.
    通俗说法: 值类型就是现金，要用直接用；引用类型是存折，要用还得先去银行取现。----(摘自网上)
```
##基本数据类型常被称为四类八种
```
四类：   1，整型 2，浮点型 3，字符型 4，逻辑型
八种：   1，整型3种 byte，short，int，long
         2，浮点型2种 float，double
         3，字符型1种 char
         4，逻辑型1种 boolean
-引用类型
除了四类八种基本类型外，所有的类型都称为引用类型
在弄清楚值类型与引用类型之后,最后一点就是值传递与引用传递,这才是关键
-值传递
    基本数据类型赋值都属于值传递,值传递传递的是实实在在的变量值,是传递原参数的拷贝,值传递后，实参传递给形参的值，形参发生改变而不影响实参。
-引用传递
引用类型之间赋值属于引用传递。引用传递传递的是对象的引用地址,也就是它的本身(自己最通俗的理解)。引用传递：传的是地址，就是将实参的地址传递给形参，形参改变了，实参当然被改变了，因为他们指向相同的地址。
引用和我们的指针差不多,但是它不又不需要我们去具体的操作
-内存分配
一个具有值类型（value type）的数据存放在栈内的一个变量中。即是在栈中分配内存空间，直接存储所包含的值，其值就代表数据本身。值类型的数据具有较快的存取速度。
一个具有引用类型（reference type）的数据并不驻留在栈中，而是存储于堆中。即是在堆中分配内存空间，不直接存储所包含的值，而是指向所要存储的值，其值代表的是所指向的地址。当访问一个具有引用类型的数据时，需要到栈中检查变量的内容，该变量引用堆中的一个实际数据。引用类型的数据比值类型的数据具有更大的存储规模和较低的访问速度。
```


##[java 中的垃圾回收机制]
```
       当一个堆内存中的对象没有被栈内存中表示地址的值“引用”时，这个对象就被称为垃圾对象，它无法被使用但却占据着内存中的区域，好比这样：
     String s = new String(“person”); s = new String(“man”); s本来是指向堆内存中值为person的对象的，但是s突然讨厌person了，它指向了堆内存中的man对象了，person就像一个孤儿一样被s遗弃了，但是person比孤儿还要惨，因为没有什么能找的到它，除了位高权重的‘垃圾回收器’，不过被当官的找到往往没什么好事，尤其是这个‘垃圾回收器’，它会豪不留情把‘垃圾’们清理走，并且无情的销毁，以便释放内存。
[装箱与拆箱]
其实装箱就是值类型到引用类型的转化过程。将一个值类型变量装箱成一个引用类型变量，首先会在托管堆上为新的引用类型变量分配内存空间，然后将值类型变量拷贝到托管堆上新分配的对象内存中，最后返回新分配的对象内存地址。装箱操作是可逆的，所以还有拆箱操作。拆箱操作获取只想对象中包含值类型部分的指针，然后由程序员手动将其对应的值拷贝给值类型变量。
```


##BufferedReader中文乱码解决
```
原来的代码是：BufferedReader buffer = new BufferedReader(new InputStreamReader(in)); 
修改后的是：BufferedReader buffer = new BufferedReader(new InputStreamReader(in,"GB2312"));
或 BufferedReader buffer = new BufferedReader(new InputStreamReader(in,"GBK")); 
```


##去掉数组中最后一个元素
```
ArrayList al = new ArrayList(Arrays.asList(myArray));
al.remove(al.size() - 1);
或者直接把你需要的数据挎贝到另一个数组中
System.arraycopy(Object src, int srcPos, Object dest, int destPos, int length) 
从指定源数组中复制一个数组，复制从指定的位置开始，到目标数组的指定位置结束
```


##[Ljava.lang.Object; cannot be cast to [Ljava.lang.String;
```
ArrayList<String> alist = new ArrayList<String>(Arrays.asList(cmdarray));
alist.remove(alist.size() - 1);
Object[] array = alist.toArray(); 
String[] arr = new String[array.length];
for (int i = 0; i < array.length; i++) { // 逐个转换
arr[i] = (String) array[i];
}
首先要观察被转换的对象的原来类型是什么,然后根据这个对象再去操作里面的元素，再一次的转换类型,而且有的时候被分析的对象可能有多层的包装，在转换的过程中需要多层的解开她,直到获取到对象的最终类型.
解压缩
public static void extractZipFiles(String zipSrcPath, String zipDestPath) {
// String root = "d:/test/";
// URL url = TestZip.class.getResource("/abc.zip"); // 加载项目src目录下的文件
        // 或 URL url = TestZip.class.getClassLoader().getResource("abc.zip"); // 不用加 '/'
        // 或 InputStream inputStream = TestZip.class.getClassLoader().getResourceAsStream("abc.zip");
        // 或     inputStream = Thread.currentThread().getContextClassLoader().getResourceAsStream("abc.zip");
// String filename = url.getFile();
try {
byte[] buf = new byte[1024];
ZipInputStream zipInputStream = null;
ZipEntry zipEntry;
zipInputStream = new ZipInputStream(new FileInputStream(zipSrcPath));
            // 如果采用getResourceStream的方式，zipInputStream = new ZipInputStream(inputStream);
zipEntry = zipInputStream.getNextEntry();
while (zipEntry != null) {
// for each entry to be extracted
String entryName = zipEntry.getName();
System.out.println(entryName);
int n;
File newFile = new File(entryName);
if (zipDestPath == null) {
if (newFile.isDirectory()) {
break;
}
} else {
new File(zipDestPath).mkdirs();
}
FileOutputStream fos;
if (!zipEntry.isDirectory()) {
System.out.println("File to be extracted....." + entryName);
fos = new FileOutputStream(zipDestPath + entryName);
while ((n = zipInputStream.read(buf, 0, 1024)) > -1) {
fos.write(buf, 0, n);
}
fos.close();
}
zipInputStream.closeEntry();
zipEntry = zipInputStream.getNextEntry();
}
zipInputStream.close();
} catch (Exception e) {
e.printStackTrace();
}
}
压缩
    /**
* 压缩
* 
* @param srcFile
*            要压缩的文件
* @param destPath
*            压缩后的文件存放的位置
* @author allen
*/
public static void zip(String srcFile, String destPath) {
ZipEntry entry = new ZipEntry(srcFile);
InputStream is = null;
String destFile = srcFile.substring(srcFile.lastIndexOf("\\") + 1,
srcFile.lastIndexOf(".")) + ".zip";
System.out.println(destFile);
ZipOutputStream zos = null;
try {
zos = new ZipOutputStream(new FileOutputStream(new File(destPath
+ destFile)));
zos.putNextEntry(entry);
is = new FileInputStream(new File(srcFile));
int BUFFERSIZE = 2 << 10;
int length = 0;
byte[] buffer = new byte[BUFFERSIZE];
while ((length = is.read(buffer, 0, BUFFERSIZE)) >= 0) {
zos.write(buffer, 0, length);
}
zos.flush();
zos.closeEntry();
} catch (IOException ex) {
ex.printStackTrace();
} finally {
if (null != is) {
try {
is.close();
} catch (IOException e) {
e.printStackTrace();
}
}
}
}
```
##为什么Runtime.exec("ls"）没有任何输出
```
调用Runtime.exec方法将产生一个本地的进程，并返回一个Process子类的实例，
该实例可用于控制进程或取得进程的相关信息。
由于调用Runtime.exec方法所创建的子进程没有自己的终端或控制台，因此该子进程的标准IO(如stdin，stdou，stderr)都通过Process.getOutputStream()，Process.getInputStream()，Process.getErrorStream()方法重定向给它的父进程了。用户需要用这些stream来向子进程输入数据或获取子进程的输出。
所以正确执行Runtime.exec("ls"）的例程如下: 
        try {
            Process process = Runtime.getRuntime().exec(command);
            LineNumberReader input = 
                     new LineNumberReader(new InputStreamReader(process.getInputStream()));
            String line = null;
            while ((line = input.readLine()) != null)
                System.out.println(line);
        } catch (java.io.IOException e) {
            System.err.println("IOException " + e.getMessage());
        }
```
##Java是如何实现跨平台的？

跨平台是怎样实现的呢？这就要谈及Java虚拟机（Java Virtual Machine，简称 JVM）。

JVM也是一个软件，不同的平台有不同的版本。我们编写的Java源码，编译后会生成一种 .class 文件，称为字节码文件。Java虚拟机就是负责将字节码文件翻译成特定平台下的机器码然后运行。也就是说，只要在不同平台上安装对应的JVM，就可以运行字节码文件，运行我们编写的Java程序。

而这个过程中，我们编写的Java程序没有做任何改变，仅仅是通过JVM这一”中间层“，就能在不同平台上运行，真正实现了”一次编译，到处运行“的目的。

JVM是一个”桥梁“，是一个”中间件“，是实现跨平台的关键，Java代码首先被编译成字节码文件，再由JVM将字节码文件翻译成机器语言，从而达到运行Java程序的目的。

注意：编译的结果不是生成机器码，而是生成字节码，字节码不能直接运行，必须通过JVM翻译成机器码才能运行。不同平台下编译生成的字节码是一样的，但是由JVM翻译成的机器码却不一样。

所以，运行Java程序必须有JVM的支持，因为编译的结果不是机器码，必须要经过JVM的再次翻译才能执行。即使你将Java程序打包成可执行文件（例如 .exe），仍然需要JVM的支持。

注意：跨平台的是Java程序，不是JVM。JVM是用C/C++开发的，是编译后的机器码，不能跨平台，不同平台下需要安装不同版本的JVM。
##equals和hashCode
###equals()方法详解
equals()方法是用来判断其他的对象是否和该对象相等.
  equals()方法在object类中定义如下： 
public boolean equals(Object obj) {  
    return (this == obj);  
}  
很明显是对两个对象的地址值进行的比较（即比较引用是否相同）。但是我们知道，String 、Math、Integer、Double等这些封装类在使用equals()方法时，已经覆盖了object类的equals()方法。

比如在String类中如下：
```
public boolean equals(Object anObject) {  
    if (this == anObject) {  
        return true;  
    }  
    if (anObject instanceof String) {  
        String anotherString = (String)anObject;  
        int n = count;  
        if (n == anotherString.count) {  
            char v1[] = value;  
            char v2[] = anotherString.value;  
            int i = offset;  
            int j = anotherString.offset;  
            while (n– != 0) {  
                if (v1[i++] != v2[j++])  
                    return false;  
            }  
            return true;  
        }  
    }  
    return false;  
}  
```
很明显，这是进行的内容比较，而已经不再是地址的比较。依次类推Math、Integer、Double等这些类都是重写了equals()方法的，从而进行的是内容的比较。当然，基本类型是进行值的比较。

它的性质有：

自反性（reflexive）。对于任意不为null的引用值x，x.equals(x)一定是true。

对称性（symmetric）。对于任意不为null的引用值x和y，当且仅当x.equals(y)是true时，y.equals(x)也是true。

传递性（transitive）。对于任意不为null的引用值x、y和z，如果x.equals(y)是true，同时y.equals(z)是true，那么x.equals(z)一定是true。

一致性（consistent）。对于任意不为null的引用值x和y，如果用于equals比较的对象信息没有被修改的话，多次调用时x.equals(y)要么一致地返回true要么一致地返回false。

对于任意不为null的引用值x，x.equals(null)返回false。

对于Object类来说，equals()方法在对象上实现的是差别可能性最大的等价关系，即，对于任意非null的引用值x和y，当且仅当x和y引用的是同一个对象，该方法才会返回true。

需要注意的是当equals()方法被override时，hashCode()也要被override。按照一般hashCode()方法的实现来说，相等的对象，它们的hash code一定相等。
###hashcode() 方法详解
hashCode()方法给对象返回一个hash code值。这个方法被用于hash tables，例如HashMap。

它的性质是：

在一个Java应用的执行期间，如果一个对象提供给equals做比较的信息没有被修改的话，该对象多次调用hashCode()方法，该方法必须始终如一返回同一个integer。

如果两个对象根据equals(Object)方法是相等的，那么调用二者各自的hashCode()方法必须产生同一个integer结果。

并不要求根据equals(java.lang.Object)方法不相等的两个对象，调用二者各自的hashCode()方法必须产生不同的integer结果。然而，程序员应该意识到对于不同的对象产生不同的integer结果，有可能会提高hash table的性能。

大量的实践表明，由Object类定义的hashCode()方法对于不同的对象返回不同的integer。

在object类中，hashCode定义如下：

public native int hashCode();
 说明是一个本地方法，它的实现是根据本地机器相关的。当然我们可以在自己写的类中覆盖hashcode()方法，比如String、Integer、Double等这些类都是覆盖了hashcode()方法的。
例如在String类中定义的hashcode()方法如下：
```
public int hashCode() {  
    int h = hash;  
    if (h == 0) {  
        int off = offset;  
        char val[] = value;  
        int len = count;  
  
        for (int i = 0; i < len; i++) {  
            h = 31 * h + val[off++];  
        }  
        hash = h;  
    }  
    return h;  
} 
``` 
解释一下这个程序（String的API中写到）：s[0]*31^(n-1) + s[1]*31^(n-2) + … + s[n-1]
使用 int 算法，这里 s[i] 是字符串的第 i 个字符，n 是字符串的长度，^ 表示求幂（空字符串的哈希码为 0）。

###与集合的关系
Java中的集合（Collection）有两类，一类是List，再有一类是Set。前者集合内的元素是有序的，元素可以重复；后者元素无序，但元素不可重复。这里就引出一个问题：要想保证元素不重复，可两个元素是否重复应该依据什么来判断呢？

###Hashset、Hashmap、Hashtable与hashcode()和equals()的密切关系
Hashset是继承Set接口，Set接口又实现Collection接口，这是层次关系。那么Hashset、Hashmap、Hashtable中的存储操作是根据什么原理来存取对象的呢？

下面以HashSet为例进行分析，我们都知道：在hashset中不允许出现重复对象，元素的位置也是不确定的。在hashset中又是怎样判定元素是否重复的呢？在java的集合中，判断两个对象是否相等的规则是：
 1.判断两个对象的hashCode是否相等

     如果不相等，认为两个对象也不相等，完毕
     如果相等，转入2
   （这一点只是为了提高存储效率而要求的，其实理论上没有也可以，但如果没有，实际使用时效率会大大降低，所以我们这里将其做为必需的。）

 2.判断两个对象用equals运算是否相等
    如果不相等，认为两个对象也不相等
    如果相等，认为两个对象相等（equals()是判断两个对象是否相等的关键）
    为什么是两条准则，难道用第一条不行吗？不行，因为前面已经说了，hashcode()相等时，equals()方法也可能不等，所以必须用第2条准则进行限制，才能保证加入的为非重复元素。

###重写equals()和hashcode()小结：

　　1.重点是equals，重写hashCode只是技术要求（为了提高效率）
      2.为什么要重写equals呢？因为在java的集合框架中，是通过equals来判断两个对象是否相等的
      3.在hibernate中，经常使用set集合来保存相关对象，而set集合是不允许重复的。在向HashSet集合中添加元素时，其实只要重写equals()这一条也可以。但当hashset中元素比较多时，或者是重写的equals()方法比较复杂时，我们只用equals()方法进行比较判断，效率也会非常低，所以引入了hashCode()这个方法，只是为了提高效率，且这是非常有必要的。比如可以这样写：

public int hashCode(){  
   return 1; //等价于hashcode无效  
}  
这样做的效果就是在比较哈希码的时候不能进行判断，因为每个对象返回的哈希码都是1，每次都必须要经过比较equals()方法后才能进行判断是否重复，这当然会引起效率的大大降低。
##快速理解Java中的五种单例模式
解法一：只适合单线程环境（不好）

复制代码
package test;
/**
 * @author xiaoping
 *
 */
public class Singleton {
    private static Singleton instance=null;
    private Singleton(){
        
    }
    public static Singleton getInstance(){
        if(instance==null){
            instance=new Singleton();
        }
        return instance;
    }
}
复制代码
注解:Singleton的静态属性instance中，只有instance为null的时候才创建一个实例，构造函数私有，确保每次都只创建一个，避免重复创建。
缺点：只在单线程的情况下正常运行，在多线程的情况下，就会出问题。例如：当两个线程同时运行到判断instance是否为空的if语句，并且instance确实没有创建好时，那么两个线程都会创建一个实例。

解法二：多线程的情况可以用。（懒汉式，不好）

复制代码
public class Singleton {
    private static Singleton instance=null;
    private Singleton(){
        
    }
    public static synchronized Singleton getInstance(){
        if(instance==null){
            instance=new Singleton();
        }
        return instance;
    }
}
复制代码
注解：在解法一的基础上加上了同步锁，使得在多线程的情况下可以用。例如：当两个线程同时想创建实例，由于在一个时刻只有一个线程能得到同步锁，当第一个线程加上锁以后，第二个线程只能等待。第一个线程发现实例没有创建，创建之。第一个线程释放同步锁，第二个线程才可以加上同步锁，执行下面的代码。由于第一个线程已经创建了实例，所以第二个线程不需要创建实例。保证在多线程的环境下也只有一个实例。
缺点：每次通过getInstance方法得到singleton实例的时候都有一个试图去获取同步锁的过程。而众所周知，加锁是很耗时的。能避免则避免。

解法三：加同步锁时，前后两次判断实例是否存在（可行）

复制代码
public class Singleton {
    private static Singleton instance=null;
    private Singleton(){
        
    }
    public static Singleton getInstance(){
        if(instance==null){
            synchronized(Singleton.class){
                if(instance==null){
                    instance=new Singleton();
                }
            }
        }
        return instance;
    }
}
复制代码
注解：只有当instance为null时，需要获取同步锁，创建一次实例。当实例被创建，则无需试图加锁。
缺点：用双重if判断，复杂，容易出错。

解法四：饿汉式（建议使用）

复制代码
public class Singleton {
    private static Singleton instance=new Singleton();
    private Singleton(){
        
    }
    public static Singleton getInstance(){
        return instance;
    }
}
复制代码
注解：初试化静态的instance创建一次。如果我们在Singleton类里面写一个静态的方法不需要创建实例，它仍然会早早的创建一次实例。而降低内存的使用率。

缺点：没有lazy loading的效果，从而降低内存的使用率。

解法五：静态内部内。（建议使用）

public class Singleton {
    private Singleton(){
        
    }
    private static class SingletonHolder{
        private final static Singleton instance=new Singleton();
    }
    public static Singleton getInstance(){
        return SingletonHolder.instance;
    }
}
注解：定义一个私有的内部类，在第一次用这个嵌套类时，会创建一个实例。而类型为SingletonHolder的类，只有在Singleton.getInstance()中调用，由于私有的属性，他人无法使用SingleHolder，不调用Singleton.getInstance()就不会创建实例。
优点：达到了lazy loading的效果，即按需创建实例。
##java class文件
注意不是jvm将源文件编译为class文件，而是java编译器
当Java编译器编译好.class文件之后，我们需要使用JVM来运行这个class文件。那么最开始的工作就是要把字节码从磁盘输入到内存中，这个过程我们叫做【加载 】。加载完成之后，我们就可以进行一系列的运行前准备工作了，比如： 为类静态变量开辟空间
##java异常链
异常需要封装和传递，我们在进行系统开发的时候，不要“吞噬”异常，也不要“赤裸裸”的抛出异常，封装后在抛出，或者通过异常链传递，可以达到系统更健壮、友好的目的。
```
import java.io.IOException;


public class ExceptionDemo {
    public void f() throws MyException {
        throw new MyException("自定义异常");
    }

    public void g() throws Exception  {
        try {
            f();
        } catch (MyException e) {
            e.printStackTrace();
            throw new Exception("重新抛出的异常1");
        }
    }

    public  void h() throws IOException    {
        try {
            g();
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
            throw new IOException("重新抛出异常2");
        }
    }
    public static void main(String[] args) {
        try {
            new ExceptionDemo().h();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}

public class MyException extends Exception {
    public MyException(String message) {
        super(message);
    }

    public MyException(String message, Throwable cause) {
        super(message, cause);
    }
}
```