[TOC]
##update 只更新修改过的字段
```
< update   id = "updateSales"   parameterType = "map" >
        update t_sales_importlog
         < set >
             < if   test = "status !=null and status !='' " > status=${status}, </ if >
             < if   test = "fileName !=null and fileName !='' " > fileName=#{fileName}, </ if >
             < if   test = "filePath !=null and filePath !='' " > filePath=#{filePath}, </ if >
             < if   test = "updatePin !=null and updatePin !='' " > updatePin=#{updatePin}, </ if >
            updateTime=now(),
            yn=1
         </ set >
         < where >
            id=${id}
         </ where >      </ update >   

```
##查询 根据时间，如果有更新时间则根据更新时间查询，如果没有更新时间则根据创建时间查询
```
关键点：在 where 子句中运用 IFNULL(exp1,exp2)
SELECT 
  sl.id,
  sl.status,
  sc.status importStatus,
  sl.fileName,
  sl.filePath,
  DATE_FORMAT(
    sl.createTime,
    '%Y-%m-%d %H:%i:%S'
  ) createTime,
  DATE_FORMAT(
    sl.updateTime,
    '%Y-%m-%d %H:%i:%S'
  ) updateTime,
  sl.createPin 
FROM
  t_sales_importLog sl,
  t_sales_importcheck sc 
WHERE sl.check_id = sc.id 
  AND IFNULL(sl.`updateTime`,sl.`createTime`) LIKE CONCAT('%', '2016-11-16', '%') 
ORDER BY id DESC 
```
##查询详情
```
< select   id = "selectSalesDetail"   resultType = "map"   parameterType = "map" >
        select id,check_id,status,fileName,filePath,DATE_FORMAT(createTime, '%Y-%m-%d %H:%i:%S') createTime,createPin from t_sales_importLog 
         < include   refid = "query_where"   />      </ select >  


< sql   id = "query_where" >
         < where >
             < if   test = "createPin != null and createPin != '' " >
                and createPin = #{createPin}
             </ if >
             < if   test = "createTime != null and createTime != '' " >
                and createTime LIKE CONCAT('%',#{createTime},'%')
             </ if >
             < if   test = "id != null and id != '' " >
                and id = #{id}
             </ if >
             < if   test = "check_id != null and check_id != '' " >
                and check_id = #{check_id}
             </ if >
         </ where >
     </ sql >  

```