##dubbo实战一
```
一、简介
Dubbo不单单只是高性能的RPC调用框架，更是SOA服务治理的一种方案。


核心：


1. 远程通信，向本地调用一样调用远程方法。


2. 集群容错


3. 服务自动发现和注册，可平滑添加或者删除服务提供者。


二、快速入门
环境：Maven，git，jdk


1. 克隆dubbo开源项目


cd ~
git clone https://github.com/alibaba/dubbo.git
2. Maven编译项目


cd ~/dubbo
mvn clean install -Dmaven.test.skip ## 跳过测试
下面核心点有：zookeeper作为注册中心（服务订阅和发布依托于注册中心）、服务生产者（提供服务）项目、服务生产者（提供服务）项目和监控Web项目。


过程如下：


3. 下载启动zk


cd ~
## 下载解压
wget http://www.apache.org/dist//zookeeper/zookeeper-3.3.3/zookeeper-3.3.3.tar.gz
tar zxvf zookeeper-3.3.3.tar.gz
## 启动
cd ../bin
./zkServer.sh start
下面项目遇到target目录中编译好的项目为xxx.tar.gz。请自行用下面命令解压：


tar zxvf XXX.tar.gz
4. 启动服务消费者


cd ~/dubbo/dubbo-demo/dubbo-demo-consumer/target/dubbo-demo-consumer-2.5.4-SNAPSHOT/conf
vim dubbo.properties
   - edit: dubbo.registry.adddress=zookeeper://127.0.0.1:2181 ## 更改注册中心为zk
cd ../bin
sh ./start.sh
5. 启动服务生产者


cd ~/dubbo/dubbo-demo/dubbo-demo-provider/target/dubbo-demo-provider-2.5.4-SNAPSHOT/conf
vim dubbo.properties
  - edit: dubbo.registry.adddress=zookeeper://127.0.0.1:2181
cd ../bin
sh ./start.sh
其实到这里已经o了，可以打开生产者消费者项目的log进行查看：


## 打开消费者的log
cd dubbo-demo-consumer/target/dubbo-demo-consumer-2.5.4-SNAPSHOT/logs
tail -f dubbo-demo-consumer.log
熟悉的Hello,World的案例coming…


6. 启动监控Web项目


cd ~/dubbo/dubbo-simple/dubbo-monitor-simple/target/dubbo-monitor-simple-2.5.4-SNAPSHOT/conf
vim dubbo.properties
   - edit: dubbo.registry.adddress=zookeeper://127.0.0.1:2181
cd ../bin./start.sh
## 浏览器访问
http://127.0.0.1:8080
可以在监控中看到消费者，生产者实例等信息
```