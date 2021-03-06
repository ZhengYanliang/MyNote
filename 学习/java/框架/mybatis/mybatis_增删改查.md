[TOC]
##批量添加（参数为map）
```
public  int  addCategory(Map<String, Object>  params );  
<insert  id = "addCategory">
        insert into 
        t_core_salecenterCategory (salePriceId,categoryType,channelType,category_big,category_middle,category_small,category_little,category_show_name,createTime,creator) 
        
        values 
        
        <foreach  collection = "categoryList"  item = "item"  separator = ","   index = "index" >
                (#{item.salePriceId},#{item.categoryType},
                #{item.channelType},#{item.category_big},
                #{item.category_mid},#{item.category_small},
                #{item.category_little},#{item.category_show_name},
                #{item.createTime},#{item.creator})
        </foreach >
    /insert>   
```
##批量添加（参数为list）
```
Integer batchSaveCity(List<City> cityList);
<insert id="batchSaveCity" parameterType="java.util.List">
    insert into
        city(id,province_id,city_name,description)
    values
        <foreach collection="list" index="index" item="item" separator=",">
            (#{item.id},#{item.provinceId},#{item.cityName},#{item.description})
        </foreach>
</insert>

```
##foreach详解
```
参考：http://www.cnblogs.com/liaojie970/p/5577018.html
foreach的主要用在构建in条件中，它可以在SQL语句中进行迭代一个集合。foreach元素的属性主要有 item，index，collection，open，separator，close。item表示集合中每一个元素进行迭代时的别名，index指 定一个名字，用于表示在迭代过程中，每次迭代到的位置，open表示该语句以什么开始，separator表示在每次进行迭代之间以什么符号作为分隔 符，close表示以什么结束，在使用foreach的时候最关键的也是最容易出错的就是collection属性，该属性是必须指定的，但是在不同情况 下，该属性的值是不一样的，主要有一下3种情况：
 
1.如果传入的是单参数且参数类型是一个List的时候，collection属性值为list
2.如果传入的是单参数且参数类型是一个array数组的时候，collection的属性值为array
3.如果传入的参数是多个的时候，我们就需要把它们封装成一个Map了，当然单参数也可以封装成map
```
##查询
```
public  List<Map<String, Object>> selectShowCategory(Map<String, Object>  params );  
< select   id = "selectShowCategory"   parameterType = "map"   resultType = "map" >
        select salePriceId,categoryType,channelType,category_big,category_middle,category_small,category_little,category_show_name,createTime,creator 
            from t_core_salecenterCategory 
             < where >
                categoryType = ${categoryType}
                 < if   test = 'channelType !=null and channelType !=""' >
                    and channelType = ${channelType}
                 </ if >
                 < if   test = 'salePriceId != "" and salePriceId != "0"' >
                    and salePriceId = ${salePriceId}
                 </ if >
             </ where >
     </ select >  

```