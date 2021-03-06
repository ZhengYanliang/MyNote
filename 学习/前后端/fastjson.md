##  fastJson将json数组转list对象
```
List<Person> list = new ArrayList<Person>();  
 list = com.alibaba.fastjson.JSONObject.parseArray(jasonArray, Person.class); 
jsonArray里是前台传的json数组，可以它转成List<Person>对象。


{"category_attribute_search_response":{"code":"0","total":14,"attributes":[{"aid":3201,"name":"品牌","cid":"1351"},{"aid":7716,"name":"类型","cid":"1351"},{"aid":8668,"name":"价格","cid":"1351"},{"aid":8664,"name":"款式","cid":"1351"},{"aid":8665,"name":"领形","cid":"1351"},{"aid":8667,"name":"材质","cid":"1351"},{"aid":10649,"name":"尺码","cid":"1351"},{"aid":14225,"name":"布种","cid":"1351"},{"aid":14224,"name":"板型","cid":"1351"},{"aid":14227,"name":"工艺","cid":"1351"},{"aid":14226,"name":"风格","cid":"1351"},{"aid":14223,"name":"厚薄","cid":"1351"},{"aid":1000000041,"name":"尺寸","cid":"1351"},{"aid":1000000027,"name":"颜色","cid":"1351"}]}}
这是json格式的数据，我想借助jar包转换成list或者javaBean实体，应该怎么弄？


JSONObject.toBean


String list = request.getParameter("json");
JSONArray data = JSONArray.fromObject(list);
for(int i=0;i<data.size();i++){
     JSONObject jobj =  (JSONObject) data.get(i);
     String name = jobj.get("name");
}


JSON-lib或者google的Gson


import java.text.ParseException;
import org.json.JSONArray;
import org.json.JSONObject;
public class TestJsonArray {
 public static void main(String[] args) throws ParseException {
  String jsonStr = "[{\"a\":123, \"b\":\"hello\", \"x\":[{\"inner\":\"Inner JSONObject\"}]}]";
   
  JSONArray jsonArray = new JSONArray(jsonStr);
  JSONObject jsonObj = jsonArray.getJSONObject(0);
  System.out.println(jsonObj);
   
  int a = jsonObj.getInt("a");
  String b = jsonObj.getString("b");
  JSONArray jsonArrayX = jsonObj.getJSONArray("x");
   
  System.out.println(a);
  System.out.println(b);
  System.out.println(jsonArrayX);
  System.out.println(jsonArrayX.getJSONObject(0).getString("inner"));
 }
}
``` ##fastJson在java后台转换json格式数据探究（二）--处理数组/List/Map
```
package com.allen.common.json;


import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;


import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONArray;


public class FastJsonTest1 {


/**
* 数组转json格式字符串
*/
public void array2Json() {
String[] arr = { "bill", "green", "maks", "jim" };
String jsonText = JSON.toJSONString(arr, true);
System.out.println("array2Json()方法：jsonText==" + jsonText);
// 输出结果：jsonText==["bill","green","maks","jim"]
}


/**
* json格式字符串转数组
*/
public void json2Array() {
String jsonText = "[\"bill\",\"green\",\"maks\",\"jim\"]";
JSONArray jsonArr = JSON.parseArray(jsonText);
System.out.println("json2Array()方法：jsonArr==" + jsonArr);
// 输出结果：jsonArr==["bill","green","maks","jim"]
}


/**
* 数组转json格式字符串
*/
public void array2Json2() {
User user1 = new User("P001", "TOM", 16);
User user2 = new User("P002", "JACKSON", 21);
User user3 = new User("P003", "MARTIN", 20);
User[] userArr = { user1, user2, user3 };
String jsonText = JSON.toJSONString(userArr, true);
System.out.println("array2Json2()方法：jsonText==" + jsonText);
// 输出结果：jsonText==[{"age":16,"id":"P001","name":"TOM"},{"age":21,"id":"P002","name":"JACKSON"},{"age":20,"id":"P003","name":"MARTIN"}]
}


/**
* json格式字符串转数组
*/
public void json2Array2() {
String jsonText = "[{\"age\":16,\"id\":\"P001\",\"name\":\"TOM\"},{\"age\":21,\"id\":\"P002\",\"name\":\"JACKSON\"},{\"age\":20,\"id\":\"P003\",\"name\":\"MARTIN\"}]";
JSONArray jsonArr = JSON.parseArray(jsonText);
System.out.println("json2Array2()方法：jsonArr==" + jsonArr);
// 输出结果：jsonArr==[{"age":16,"id":"P001","name":"TOM"},{"age":21,"id":"P002","name":"JACKSON"},{"age":20,"id":"P003","name":"MARTIN"}]
}


/**
* list集合转json格式字符串
*/
public void list2Json() {
List<User> list = new ArrayList<User>();
User user1 = new User("L001", "TOM", 16);
list.add(user1);
User user2 = new User("L002", "JACKSON", 21);
list.add(user2);
User user3 = new User("L003", "MARTIN", 20);
list.add(user3);
String jsonText = JSON.toJSONString(list, true);
System.out.println("list2Json()方法：jsonText==" + jsonText);
// 输出结果：jsonText==[{"age":16,"id":"L001","name":"TOM"},{"age":21,"id":"L002","name":"JACKSON"},{"age":20,"id":"L003","name":"MARTIN"}]
}


/**
* list集合转json格式字符串
*/
public void list2Json2() {
List<User> list = new ArrayList<User>();
Address address1 = new Address("广东省", "深圳市", "科苑南路", "580053");
User user1 = new User("L001", "TOM", 16, address1);
list.add(user1);
Address address2 = new Address("江西省", "南昌市", "阳明路", "330004");
User user2 = new User("L002", "JACKSON", 21, address2);
list.add(user2);
Address address3 = new Address("陕西省", "西安市", "长安南路", "710114");
User user3 = new User("L003", "MARTIN", 20, address3);
list.add(user3);
String jsonText = JSON.toJSONString(list, true);
System.out.println("list2Json2()方法：jsonText==" + jsonText);
// 输出结果：jsonText==[{"address":{"city":"深圳市","post":"580053","province":"广东省","street":"科苑南路"},"age":16,"id":"L001","name":"TOM"},{"address":{"city":"南昌市","post":"330004","province":"江西省","street":"阳明路"},"age":21,"id":"L002","name":"JACKSON"},{"address":{"city":"西安市","post":"710114","province":"陕西省","street":"长安南路"},"age":20,"id":"L003","name":"MARTIN"}]
}


/**
* map转json格式字符串
*/
public void map2Json() {
Map<String, Address> map = new HashMap<String, Address>();
Address address1 = new Address("广东省", "深圳市", "科苑南路", "580053");
map.put("address1", address1);
Address address2 = new Address("江西省", "南昌市", "阳明路", "330004");
map.put("address2", address2);
Address address3 = new Address("陕西省", "西安市", "长安南路", "710114");
map.put("address3", address3);
String jsonText = JSON.toJSONString(map, true);
System.out.println("map2Json()方法：jsonText==" + jsonText);
// 输出结果：jsonText=={"address1":{"city":"深圳市","post":"580053","province":"广东省","street":"科苑南路"},"address2":{"city":"南昌市","post":"330004","province":"江西省","street":"阳明路"},"address3":{"city":"西安市","post":"710114","province":"陕西省","street":"长安南路"}}
}


public static void main(String[] args) {
FastJsonTest1 test = new FastJsonTest1();
// 数组转json格式字符串
test.array2Json();


// json格式字符串转数组
test.json2Array();


// 数组转json格式字符串
test.array2Json2();


// json格式字符串转数组
test.json2Array2();


// list集合转json格式字符串
test.list2Json();


// list集合转json格式字符串
test.list2Json2();


// map转json格式字符串
test.map2Json();
}
}


package  com.allen.common.json;
import  java.io.Serializable;
public   class  Address  implements  Serializable {
     private   static   final   long   serialVersionUID  = 1L;
     private  String  province ;
     private  String  city ;
     private  String  street ;
     private  String  post ;
     public  Address() {
         super ();
    }
     public  Address(String  province , String  city , String  street , String  post ) {
         super ();
         this . province  =  province ;
         this . city  =  city ;
         this . street  =  street ;
         this . post  =  post ;
    }
     public  String getCity() {
         return   city ;
    }
     public   void  setCity(String  city ) {
         this . city  =  city ;
    }
     public  String getPost() {
         return   post ;
    }
     public   void  setPost(String  post ) {
         this . post  =  post ;
    }
     public  String getProvince() {
         return   province ;
    }
     public   void  setProvince(String  province ) {
         this . province  =  province ;
    }
     public  String getStreet() {
         return   street ;
    }
     public   void  setStreet(String  street ) {
         this . street  =  street ;
    }
}


package  com.allen.common.json;
import  java.io.Serializable;
public   class  User  implements  Serializable {
     private   static   final   long   serialVersionUID  = 1L;
     private  String  id ;
     private  String  name ;
     private   int   age ;
     private  Address  address ;
     public  User() {
         super ();
    }
     public  User(String  id , String  name ,  int   age ) {
         super ();
         this . id  =  id ;
         this . name  =  name ;
         this . age  =  age ;
    }
     public  User(String  id , String  name ,  int   age , Address  address ) {
         super ();
         this . id  =  id ;
         this . name  =  name ;
         this . age  =  age ;
         this . address  =  address ;
    }
     public   int  getAge() {
         return   age ;
    }
     public   void  setAge( int   age ) {
         this . age  =  age ;
    }
     public  String getId() {
         return   id ;
    }
     public   void  setId(String  id ) {
         this . id  =  id ;
    }
     public  String getName() {
         return   name ;
    }
     public   void  setName(String  name ) {
         this . name  =  name ;
    }
     public  Address getAddress() {
         return   address ;
    }
     public   void  setAddress(Address  address ) {
         this . address  =  address ;
    }
}
```
##前后端传list
1.前端传参数


```
if(ruleType == 1 && stageType == 1){//手动梯度
var uppriceList = getUppriceList();
upprices = encodeURI(JSON.stringify(uppriceList));
}
console.log('upprices:'+upprices);
upprices:%5B%7B%22upprice%22:%223%22,%22subprice%22:%222%22%7D,%7B%22upprice%22:%225%22,%22subprice%22:%224%22%7D%5D

2.后端接收
PersonDto:
public   class  PriceDto  implements  Serializable {
     /**
     * 
     */
     private   static   final   long   serialVersionUID  = 6123003794162230074L;
     private  String  upprice ; // 满金额      private  String  subprice ;  // 减金额  


后端Controller.
com . alibaba . fastjson .JSONObject  

String  upprices  = getParam( "upprices" );
                 upprices  = java.net.URLDecoder. decode ( upprices ,  "UTF-8" );
                 if  ( upprices  !=  null ) {
                    System. err .println( upprices );
                    List<PriceDto>  uppriceList  = JSONObject. parseArray ( upprices , PriceDto. class );
                     if  ( uppriceList  !=  null  &&  uppriceList .size() > 0) {
                         for  ( int   i  = 0;  i  <  uppriceList .size();  i ++) {
                            PriceDto  priceDto  =  uppriceList .get( i );
                            System. out .println( priceDto .getSubprice());                             System. out .println( priceDto .getUpprice());   

                                                                     }

                                                 }



```