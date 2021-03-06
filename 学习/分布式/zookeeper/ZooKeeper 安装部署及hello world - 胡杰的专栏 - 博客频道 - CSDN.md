ZooKeeper  安装部署及hello world
先给一堆学习文档，方便以后查看
官网文档地址大全：

OverView(概述)
http://zookeeper.apache.org/doc/r3.4.6/zookeeperOver.html

Getting Started(开始入门)
http://zookeeper.apache.org/doc/r3.4.6/zookeeperStarted.html

Tutorial（教程）
http://zookeeper.apache.org/doc/r3.4.6/zookeeperTutorial.html

Java Example(Java示例)
http://zookeeper.apache.org/doc/r3.4.6/javaExample.html

Programmer's Guide(开发人员指南)
http://zookeeper.apache.org/doc/r3.4.6/zookeeperProgrammers.html

Recipes and Solutions(技巧及解决方案)
http://zookeeper.apache.org/doc/r3.4.6/recipes.html

3.4.6 API online(在线API速查)

http://zookeeper.apache.org/doc/r3.4.6/api/index.html

另外推荐园友sunddenly的zookeeper系列
http://www.cnblogs.com/sunddenly/category/620563.html

一、安装部署
本文在一台机器上模拟3个 zk server的集群安装

1.1 下载解压

解压到3个目录（模拟3台zk server）：

　　/home/hadoop/zookeeper-1
　　/home/hadoop/zookeeper-2
　　/home/hadoop/zookeeper-3
1.2 创建每个目录下conf/zoo.cfg配置文件 

/home/hadoop/zookeeper-1/conf/zoo.cfg 内容如下：

tickTime=2000
initLimit=10
syncLimit=5
dataDir=/home/hadoop/tmp/zk1/data
dataLogDir=/home/hadoop/tmp/zk1/log
clientPort=2181
server.1=localhost:2287:3387
server.2=localhost:2288:3388
server.3=localhost:2289:3389

/home/hadoop/zookeeper-2/conf/zoo.cfg 内容如下：

tickTime=2000
initLimit=10
syncLimit=5
dataDir=/home/hadoop/tmp/zk2/data
dataLogDir=/home/hadoop/tmp/zk2/log
clientPort=2182
server.1=localhost:2287:3387
server.2=localhost:2288:3388
server.3=localhost:2289:3389

/home/hadoop/zookeeper-3/conf/zoo.cfg 内容如下：

tickTime=2000
initLimit=10
syncLimit=5
dataDir=/home/hadoop/tmp/zk3/data
dataLogDir=/home/hadoop/tmp/zk3/log
clientPort=2183
server.1=localhost:2287:3387
server.2=localhost:2288:3388
server.3=localhost:2289:3389

注：因为是在一台机器上模拟集群，所以端口不能重复，这里用2181~2183，2287~2289，以及3387~3389相互错开。另外每个zk的instance，都需要设置独立的数据存储目录、日志存储目录，所以dataDir、dataLogDir这二个节点对应的目录，需要手动先创建好。

另外还有一个灰常关键的设置，在每个zk server配置文件的dataDir所对应的目录下，必须创建一个名为myid的文件，其中的内容必须与zoo.cfg中server.x 中的x相同，即：

/home/hadoop/tmp/zk1/data/myid 中的内容为1，对应server.1中的1
/home/hadoop/tmp/zk2/data/myid 中的内容为2，对应server.2中的2
/home/hadoop/tmp/zk3/data/myid 中的内容为3，对应server.3中的3

生产环境中，分布式集群部署的步骤与上面基本相同，只不过因为各zk server分布在不同的机器，上述配置文件中的localhost换成各服务器的真实Ip即可。分布在不同的机器后，不存在端口冲突问题，可以让每个服务器的zk均采用相同的端口，这样管理起来比较方便。

1.3 启动验证 

/home/hadoop/zookeeper-1/bin/zkServer.sh start

/home/hadoop/zookeeper-2/bin/zkServer.sh start

/home/hadoop/zookeeper-3/bin/zkServer.sh start
启用成功后，输入 jps 看下进程

20351 ZooKeeperMain
20791 QuorumPeerMain
20822 QuorumPeerMain
20865 QuorumPeerMain

应该至少能看到以上几个进程。

可以启动客户端测试下：

bin/zkCli.sh -server localhost:2181
(注：如果是远程连接，把localhost换成指定的IP即可)

成功后，应该会进到提示符下，类似下面这样:

[zk: localhost:2181(CONNECTED) 0]  

然后，就可以用一些基础命令，比如 ls ,create ,delete ,get 来测试了（关于这些命令，大家可以查看文档），特别提一个很有用的命令rmr 用来递归删除某个节点及其所有子节点

二、java 与 zk的连接示例 2.1 maven项目的pom.xml中先添加以下依赖项
        <!--zk-->
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.4.6</version>
        </dependency>

2.2 最基本的示例程序

package yjmyzz;
 
import java.io.IOException;
import org.apache.zookeeper.*;
import org.apache.zookeeper.data.Stat;
 
public class ZooKeeperHello {
 
    public static void main(String[] args) throws IOException, InterruptedException, KeeperException {
        ZooKeeper zk = new ZooKeeper("172.28.20.102:2181", 300000, new DemoWatcher());//连接zk server
        String node = "/app1";
        Stat stat = zk.exists(node, false);//检测/app1是否存在
        if (stat == null) {
            //创建节点
            String createResult = zk.create(node, "test".getBytes(), ZooDefs.Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);
            System.out.println(createResult);
        }
        //获取节点的值
        byte[] b = zk.getData(node, false, stat);
        System.out.println(new String(b));
        zk.close();
    }
 
    static class DemoWatcher implements Watcher {
        @Override
        public void process(WatchedEvent event) {
            System.out.println("----------->");
            System.out.println("path:" + event.getPath());
            System.out.println("type:" + event.getType());
            System.out.println("stat:" + event.getState());
            System.out.println("<-----------");
        }
    }
}
2.3 与zk集群的连接

zk的优点之一，就是高可用性，上面的代码连接的是单台zk server，如果这台server挂了，自然代码就会出错，事实上zk的API考虑到了这一点，把连接代码改成下面这样：

 ZooKeeper zk = new ZooKeeper("172.28.20.102:2181,172.28.20.102:2182,172.28.20.102:2183", 300000, new DemoWatcher());//连接zk server
 
 即：IP1:port1,IP2:port2,IP3:port3...  用这种方式连接集群就行了，只要有超过半数的zk server还活着，应用一般就没问题。但是也有一种极罕见的情况，比如这行代码执行时，刚初始化完成，正准备连接ip1时，因为网络故障ip1对应的server挂了，仍然会报错（此时，zk还来不及选出新leader），这个问题详见：http://segmentfault.com/q/1010000002506725/a-1020000002507402，参考该文的做法，改成：

         ZooKeeper zk = new ZooKeeper("172.28.20.102:2181,172.28.20.102:2182,172.28.20.102:2183", 300000, new DemoWatcher());//连接zk server
         if (!zk.getState().equals(ZooKeeper.States.CONNECTED)) {
             while (true) {
                 if (zk.getState().equals(ZooKeeper.States.CONNECTED)) {
                     break;
                 }
                 try {
                     TimeUnit.SECONDS.sleep(5);
                 } catch (InterruptedException e) {
                     e.printStackTrace();
                 }
             }
         }


 但是这样代码未免太冗长，建议用开源的zkClient，官方地址: https://github.com/sgroschupf/zkclient，使用方法很简单：

        <!--zkclient-->
        <dependency>
            <groupId>com.github.sgroschupf</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.1</version>
        </dependency>

pom.xml先加这一坨，然后这样用：

     @Test
     public void testZkClient() {
         ZkClient zkClient = new ZkClient("172.28.20.102:2181,172.28.20.102:2182,172.28.20.102:2183");
         String node = "/app2";
         if (!zkClient.exists(node)) {
             zkClient.createPersistent(node, "hello zk");
         }
         System.out.println(zkClient.readData(node));
     }