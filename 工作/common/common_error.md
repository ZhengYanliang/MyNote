##Caused by: java.lang.IllegalArgumentException: Could not resolve placeholder 'queryWeather.key' in string value "${queryWeather.key}"
```
service中：
@Value ( "${queryWeather.key}" )
     private  String  secretKey ;
     @Value ( "${queryWeather.url}" )      private  String  queryUrl ;  
 原因：service中的property文件中没有定义 。
解决：
1.pom中：
#============queryWeather==============
queryWeather.key= DJOYnieT8234jlsK
queryWeather.url= http://php.weather.sina.com.cn/xml.php   

2.service中的main-setting.properties:
#============queryWeather==============
queryWeather.key= ${queryWeather.key}
queryWeather.url= ${queryWeather.url}   

```