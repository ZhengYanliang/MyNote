##关于string[]
```
将"1001,1002" 转为String[] strs = new String[2]{"1001","1002"};
不如转为List<String> strList = new ArrayList<String>(); 因为list 有好多特有的方法 remove() 等。。。
```
##string[] 的初始化及声明 
```
1.String[] 初始化
String[] str = {"1","2","3"}  
内存中是静态初始化
String[] str = new String[]{"1","2","3"}
则是动态分配内存

2.数组的声明由几种方式：  
  方式一：
String []a = new String[length];再赋值  
a[0]=”frank";
方式二：
new完就直接初始化：  
String []a = new String[]{?,?...};  

实例：
@Test
     public   void  test_declare() {
        String[]  res_arr  =  new  String[] {};
        String  str  =  "a1,a2,a3" ;
         res_arr  =  new  String[ str .split( "," ). length ]; // 必须声明长度，否则
                                                     // java.lang.ArrayIndexOutOfBoundsException:
                                                     // 0
         res_arr [0] =  "a1" ;
         res_arr [1] =  "a2" ;
         res_arr [2] =  "a3" ;
        System. out .println( res_arr );     }   

```

##字符串比较 
```
String s1 = new String("java");
String s2 = new String("java");


System.out.println(s1==s2);            //false
System.out.println(s1.equals(s2));    //true
```
##去掉String中的空格   
```
1. String.trim()  
trim()是去掉首尾空格  


2.str.replace(" ", ""); 去掉所有空格，包括首尾、中间  
String str = " hell o ";  
String str2 = str.replaceAll(" ", "");  
System.out.println(str2);
   
3.或者replaceAll(" +",""); 去掉所有空格  


4.str = .replaceAll("\\s*", "");  
可以替换大部分空白字符， 不限于空格   
\s 可以匹配空格、制表符、换页符等空白字符的其中任意一个 
```
##字符串数组声明
```
String[] res = new String[3];  

``` ##string转boolean
```

public static boolean parseBoolean(
String
 s)
将字符串参数解析为 boolean 值。如果 String 参数不是 null 且在忽略大小写时等于"true"，则返回的 boolean 表示 true 值。
示例：Boolean.parseBoolean("True") 返回 true。
示例：Boolean.parseBoolean("yes") 返回 false。
```
##关于String.valueOf()方法
String intervalB = String.valueOf(map.get("beginInterval"));// 促销时间段
这里有个问题，如果map.get("beginInterval")为null，则intervalB="null";

