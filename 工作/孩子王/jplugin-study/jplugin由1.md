1.pom.xml




2.plugin.cfg


3.增加database.config.properties 与 base-config.properties
```
##database-config.properties
#jdbc driver class name
driverClassName= com.mysql.jdbc.Driver
#the database url
#the database url for trunck
url= jdbc:mysql://172.172.178.18:3310/core?characterEncoding=utf8&autoReconnect=true&autoReconnectForPools=true&useUnicode=true&
dbuser= wrh
dbpassword= haiziwang@wrh
#the database url for stable
#url=jdbc:mysql://172.172.230.23:3306/core?characterEncoding=utf8&autoReconnect=true&autoReconnectForPools=true&useUnicode=true&
#dbuser=core_read_wirte
#dbpassword=ixt8jhgbbu
maxActive= 100
#max idle connections 
maxIdle= 50
#max wait seconds till time out
maxWait= 2000
#redis host
#redis.host=172.172.178.100
#redis port
#redis.port=19000
#redis key #redis.key=plat_20000002_css_hand_  






##base-config.properties
#trunk
app-center-url= http://test.appcenter.haiziwang.com/appcenter/
app-sign= TRAilSK2wMik8oMqEkLFYYyqOteywwgf
app-code= MidCenter
esf-port= 58081
#stable
#app-center-url=http://appcenter.haiziwang.com/appcenter/
#app-sign=TRAilSK2wMik8oMqEkLFYYyqOteywwgf
#app-code=MidCenter #esf-port=78081  
 

```
4. plugin.java





5.修改控制类中的 WebStubCntl方法
比如： BetrayController中的 getSkuById方法
改动一：

由
ReportClient.report( "Core" , "rpc" , "BetrayController.GetSkuById" ,1,11);  
改为
WebStubCntl stub = CppClientProxyFactory . createCppWebStubCntl ();


改动二：

 

去除：
stub.setPeerIPPort(PropertiesUtil.get( "ProductNaviSerIp" ),                 Integer.parseInt(PropertiesUtil.get( "ProductNaviSerPort" )));  


6.改造service中的IMybatisService方法
比如：BetrayServiceImpl  

由：
private  IMybatisService s = ServiceFactory.getService(IMybatisService. class );  
改为：
private  IMybatisService dbService() {
         return   MyBatisServiceFactory.getService(DbConstant.DB_NAME);
}


7.baseService中增加一个封装方法：
BaseService： renderJson
```
public   void  renderJson(String json) {
        getRes().setContentType( "application/json; charset=utf-8" );
         try  {
            getRes().getWriter().print(json);
        }  catch  (IOException e) {
             throw   new  RuntimeException( "render json error"  + json, e);
        }     }   

``` 
7.优化工具类： PropertiesUtil


















##更新后启动报错

原因是hosts文件没有配置

