##利用Hudson自动构建时的命令
```
cd mmc-pom
mvn clean
mvn install package -Dmaven.test.skip=true -P dev
ssh root@192.168.2.102 '/usr/local/tomcat-8080/bin/stop.sh mmc-web'
scp ../mmc-web/target/mmc-web.war root@192.168.2.102:/usr/local/tomcat-8080/webapps/mmc-web.war
ssh root@192.168.2.102 '/usr/local/tomcat-8080/bin/startup.sh'
```