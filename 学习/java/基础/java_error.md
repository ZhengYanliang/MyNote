##javax.mail.MessagingException: [EOF]
```
mail-config.properties:
serverhost=smtp.sina.com
serverport=25
validate=true
发现10分钟内发送了30封邮件，后报错：
javax.mail.MessagingException: [EOF]
at com.sun.mail.smtp.SMTPTransport.issueCommand(SMTPTransport.java:1481)
at com.sun.mail.smtp.SMTPTransport.issueSendCommand(SMTPTransport.java:1512)
at com.sun.mail.smtp.SMTPTransport.data(SMTPTransport.java:1308)
at com.sun.mail.smtp.SMTPTransport.sendMessage(SMTPTransport.java:636)
at javax.mail.Transport.send0(Transport.java:189)
at javax.mail.Transport.send(Transport.java:118)
at org.allen.utils.mail.SimpleMailSender.sendTextMail(SimpleMailSender.java:84)
at org.allen.utils.mail.SimpleMailSender.send(SimpleMailSender.java:42)
分析原因：有可能是邮件服务器发现适时间内发送了大量了邮件，就自动断开了连接。
```
##java.io.FileNotFoundException: D:\xxx\yyy (拒绝访问)
```
原因在实例化File file=new File(fileAllName);的时候fileAllName是一个目录
解决办法，将fileAllName具体到文件名字。
```

##服务器启动后，定时任务没有自动启动
```
问题，服务器启动后，需要请求一次服务，定时任务才能执行。
原因：查看日志发现，请求一次服务后，容器会初始化 Initializing Spring FrameworkServlet 'springmvc'。
解决：初始化Spring WebApplicationContext.
在web.xml中添加   <load-on-startup>1</load-on-startup> 即可。
 <!-- 配置前端控制器 -->
  <servlet>
   <servlet-name>springmvc</servlet-name>
   <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
   <!-- 加载springmvc的配置文件 
   如果不配置文件的位置，默认就是servlet-name + “-servlet.xml”
   springmvc-servlet.xml
   默认路径是：WEB-INF/springmvc-servlet.xml
   -->
   <init-param>
   <param-name>contextConfigLocation</param-name>
   <param-value>classpath:quartz-config.xml</param-value>
   </init-param>
   <load-on-startup>1</load-on-startup>
  </servlet>
```
##Runtime.getRuntime().exec()
```
Runtime.getRuntime().exec()执行后报错：
The CATALINA_HOME environment variable is not defined correctly
This environment variable is needed to run this program
错误原因：
statup.bat找不到CATLINA_HOME
解决方法：
d:\tomcat 是tomcat的地址 
1、在系统配置中添加$CATALINA_HOME的环境变量（开发使用多个tomcat不推荐）
2、在执行命令之前添加set "CATALINA_HOME=d:\tomcat " 
3、执行如下代码：
   Runtime.getRuntime().exec(String,null,new File($CATALINA_HOME));
   API中对于最后一个参数的解释为：    
   dir - 子进程的工作目录；
   如果子进程应该继承当前进程的工作目录，则该参数为 null 。
java代码：
Process process = Runtime.getRuntime().exec("D:\\java\\tomcat7.63\\bin\\startup.bat", 
null, new File("D:\\java\\tomcat7.63"));
或
Process process = Runtime.getRuntime().exec("cmd /c D:\\java\\tomcat7.63\\bin\\startup.bat", 
null, new File("D:\\java\\tomcat7.63"));
注：/c 就是"执行后面字符串的命令"
BufferedReader buffReader = new BufferedReader(new InputStreamReader(process.getInputStream(), "gbk"));
while ((line = buffReader.readLine()) != null) {
System.out.println(line);
}
执行后的命令：
Using CATALINA_BASE:   "D:\java\tomcat7.63"
Using CATALINA_HOME:   "D:\java\tomcat7.63"
Using CATALINA_TMPDIR: "D:\java\tomcat7.63\temp"
Using JRE_HOME:        "D:\java\jdk1.7.0_79"
Using CLASSPATH:       "D:\java\tomcat7.63\bin\bootstrap.jar;D:\java\tomcat7.63\bin\tomcat-juli.jar"
```