[TOC]
##安装dubbo-admin
```
配置dubbo-admin的管理页面，方便我们管理页面
    (1)下载dubbo-admin-2.5.4.war包，在Linux的tomcat部署，先把dubbo-admin-2.5.4.war放在tomcat的webapps下，然后进行解压：
        #unzip -xvf dubbo-admin-2.4.1.war
    (2)然后到webapps/dubbo-admin/WEB-INF/下，有一个dubbo.properties文件，里面指向Zookeeper ，使用的是Zookeeper 的注册中心:
[root@hadoop1 WEB-INF]# vim dubbo.properties 


dubbo.registry.address=zookeeper://127.0.0.1:2181
dubbo.admin.root.password=root
dubbo.admin.guest.password=guest
        
   (3)然后启动tomcat服务，访问localhost:8080/dubbo-admin 并输入用户名和密码：root,并访问服务，显示登陆页面，说明dubbo-admin部署成功
```
##dubbo 与 webservice
webservice适合做服务
服务适合多项目共享

dubbo做服务,可能会出现版本不一致等问题



