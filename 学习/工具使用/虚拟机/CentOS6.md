1 CentOS6.7 mini版安装
下载地址


http://mirror.citynethost.com/centos/6.7/isos/i386/CentOS-6.7-i386-minimal.iso

分区
Text | 复制

 1
 2
 3
 /boot：200M - 用来存放与Linux系统启动有关的程序，比如启动引导装载程序等。
 /swap：交换分区，建议大小是物理内存的1倍左右。
 /：剩余空间 - Linux系统的根目录，所有的目录都挂在这个目录下面。

2 设置固定IP地址及网关
设置IP
Text | 复制

 1
 vi /etc/sysconfig/network-scripts/ifcfg-eth0


修改如下内容
Text | 复制

 1
 2
 3
 4
 5
 6
 7
 8
 9
 DEVICE=eth0
 HWADDR=08:00:27:BD:9D:B5  #不用改
 TYPE=Ethernet
 UUID=53e4e4b6-9724-43ab-9da7-68792e611031 #不用改
 ONBOOT=yes  #开机启动
 NM_CONTROLLED=yes
 BOOTPROTO=static  #静态IP
 IPADDR=192.168.30.50  #IP地址
 NETMASK=255.255.255.0 #子网掩码

设置网关
Text | 复制

 1
 vi /etc/sysconfig/network


添加内容

Text | 复制

 1
 2
 3
 NETWORKING=yes
 HOSTNAME=Hadoop.Master
 GATEWAY=192.168.30.1 #网关

设置DNS

Text | 复制

 1
 vi /etc/resolv.conf


添加内容

Text | 复制

 1
 2
 nameserver xxx.xxx.xxx.xxx #根据实际情况设置
 nameserver 114.114.114.114 #可以设置多个

重启网卡

Text | 复制

 1
 service network restart

测试网络
Text | 复制

 1
 ping www.baidu.com #ping不通一般是DNS问题

设置主机名对应IP地址
Text | 复制

 1
 2
 3
 vi /etc/hosts
 #添加如下内容
 192.168.30.50 Hadoop.Master

3 添加Hadoop用户
添加用户组
Text | 复制

 1
 groupadd hadoop

添加用户并分配用户组
Text | 复制

 1
 useradd -g hadoop hadoop

修改用户密码
Text | 复制

 1
 passwd hadoop

4 关闭服务
关闭防火墙
Text | 复制

 1
 2
 3
 4
 service iptables stop #关闭防火墙服务
 chkconfig iptables off #关闭防火墙开机启动
 service ip6tables stop
 chkconfig ip6tables off

关闭SELinux
Text | 复制

 1
 2
 3
 4
 5
 6
 vi /etc/sysconfig/selinux
 #修改如下内容
 SELINUX=enforcing -> SELINUX=disabled
 #再执行如下命令
 setenforce 0
 getenforce

关闭其他服务

Text | 复制

 1
 for SERVICES in abrtd acpid auditd avahidaemon cpuspeed haldaemon mdmonitor messagebus udevpost;do chkconfig ${SERVICES} off;done

5 VSFTP安装与配置

检查是否安装

Text | 复制

 1
 chkconfig --list|grep vsftpd

安装vsftp

Text | 复制

 1
 yum -y install vsftpd


创建日志文件
Text | 复制

 1
 touch /var/log/vsftpd.log


配置vsfpd服务
Text | 复制

 1
 2
 3
 4
 5
 6
 7
 8
 vi /etc/vsftpd/vsftpd.conf
 #修改如下内容
 anonymous_enable=NO #关闭匿名访问
 xferlog_file=/var/log/vsftpd.log #设置日志文件 -- 我们上一步所创建的文件
 idle_session_timeout=600 #会话超时时间
 async_abor_enable=YES  #开启异步传输
 ascii_upload_enable=YES #开启ASCII上传
 ascii_download_enable=YES #开启ASCII下载


查看vsftp运行状态
Text | 复制

 1
 service vsftpd status

启动vsftp

Text | 复制

 1
 2
 3
 service vsftpd start
 #重启 service vsftpd restart
 #关闭 service vsftpd stop


查看vsftpd服务启动项
Text | 复制

 1
 chkconfig --list|grep vsftpd

设置vsftp开机启动
Text | 复制

 1
 2
 chkconfig vsftpd on
 #或 chkconfig --level 2345 vsftpd on

6 SSH无密码配置

查看ssh与rsync安装状态

Text | 复制

 1
 2
 rpm -qa|grep openssh
 rpm -qa|grep rsync

安装ssh与rsync

Text | 复制

 1
 2
 yum -y install ssh
 yum -y install rsync


切换hadoop用户
Text | 复制

 1
   su - hadoop

生成ssh密码对

Text | 复制

 1
 ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa


将id_dsa.pub追加到授权的key中
Text | 复制

 1
 cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

设置授权key权限

Text | 复制

 1
 2
 chmod 600 ~/.ssh/authorized_keys
 #权限的设置非常重要，因为不安全的设置安全设置，会让你不能使用RSA功能

测试ssh连接

Text | 复制

 1
 2
 ssh localhost
 #如果不需要输入密码，则是成功

7 安装Java
下载地址

http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html

    注： 我这里使用的是：jdk-7u80-linux-i586.tar.gz
安装Java

切换至root用户

Text | 复制

 1
 su -


创建/usr/java文件夹
Text | 复制

 1
 mkdir /usr/java


使用FTP工具上传至服务器

将压缩包上传至/home/hadoop目录


    注： 我这里使用的是FlashFXP，使用hadoop用户连接

将压缩包解压至/usr/java 目录
Text | 复制

 1
 tar zxvf /home/hadoop/jdk-7u80-linux-i586.tar.gz -C /usr/java/

设置环境变量

Text | 复制

 1
 2
 3
 4
 5
 vi /etc/profile
 #追加如下内容
 export JAVA_HOME=/usr/java/jdk1.7.0_80
 export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib
 export PATH=$PATH:$JAVA_HOME/bin


使环境变量生效

Text | 复制

 1
 source /etc/profile


测试环境变量设置

Text | 复制

 1
 java -version

8 Hadoop安装与配置
下载地址

http://hadoop.apache.org/releases.html


     注: 我下载的是hadoop-2.7.1.tar.gz

安装Hadoop

使用FTP工具上传至服务器

将压缩包上传至/home/hadoop目录

将压缩包解压至/usr目录
Text | 复制

 1
 tar zxvf /home/hadoop/hadoop-2.7.1.tar.gz -C /usr/


修改文件夹名称

Text | 复制

 1
 mv /usr/hadoop-2.7.1/ /usr/hadoop





创建hadoop数据目录

Text | 复制

 1
 mkdir /usr/hadoop/tmp


将hadoop文件夹授权给hadoop用户

Text | 复制

 1
 chown -R hadoop:hadoop /usr/hadoop/





设置环境变量

Text | 复制

 1
 2
 3
 4
 5
 6
 vi /etc/profile
 #追加如下内容
 export HADOOP_HOME=/usr/hadoop
 export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
 export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
 export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"


使环境变量生效

Text | 复制

 1
 source /etc/profile


测试环境变量设置

Text | 复制

 1
 hadoop version

配置HDFS

切换至Hadoop用户
Text | 复制

 1
 su - hadoop


修改hadoop-env.sh
Text | 复制

 1
 2
 3
 4
 cd /usr/hadoop/etc/hadoop/
 vi hadoop-env.sh 
 #追加如下内容
 export JAVA_HOME=/usr/java/jdk1.7.0_80


修改core-site.xml
Text | 复制

 1
 2
 3
 4
 5
 6
 7
 8
 9
 10
 11
 12
 13
 vi core-site.xml 
 #添加如下内容
 <configuration>
      <property>
          <name>fs.defaultFS</name>
          <value>hdfs://Hadoop.Master:9000</value>
      </property>
      <property>
          <name>hadoop.tmp.dir</name>
          <value>/usr/hadoop/tmp/</value>
          <description>A base for other temporary directories.</description>
      </property>
 </configuration>


修改hdfs-site.xml
Text | 复制

 1
 2
 3
 4
 5
 6
 7
 8
 vi hdfs-site.xml
 #添加如下内容
 <configuration>
      <property>
          <name>dfs.replication</name>
          <value>1</value>
      </property>
 </configuration>


格式化hdfs
Text | 复制

 1
 hdfs namenode -format


注： 出现Exiting with status 0即为成功

启动hdfs

Text | 复制

 1
 2
 start-dfs.sh
 #停止命令 stop-dfs.sh


注： 输出如下内容
Text | 复制

 1
 2
 3
 4
 5
 6
 7
 8
 9
 10
 11
 15/09/21 18:09:13 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
 Starting namenodes on [Hadoop.Master]
 Hadoop.Master: starting namenode, logging to /usr/hadoop/logs/hadoop-hadoop-namenode-Hadoop.Master.out
 Hadoop.Master: starting datanode, logging to /usr/hadoop/logs/hadoop-hadoop-datanode-Hadoop.Master.out
 Starting secondary namenodes [0.0.0.0]
 The authenticity of host '0.0.0.0 (0.0.0.0)' can't be established.
 RSA key fingerprint is b5:96:b2:68:e6:63:1a:3c:7d:08:67:4b:ae:80:e2:e3.
 Are you sure you want to continue connecting (yes/no)? yes
 0.0.0.0: Warning: Permanently added '0.0.0.0' (RSA) to the list of known hosts.
 0.0.0.0: starting secondarynamenode, logging to /usr/hadoop/logs/hadoop-hadoop-secondarynamenode-Hadoop.Master.out
 15/09/21 18:09:45 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicab


查看进程

Text | 复制

 1
 jps


注： 输出类似如下内容

Text | 复制

 1
 2
 3
 4
 1763 NameNode
 1881 DataNode
 2146 Jps
 2040 SecondaryNameNode


使用web查看Hadoop运行状态

Text | 复制

 1
 http://你的服务器ip地址:50070/


在HDFS上运行WordCount

创建HDFS用户目录

Text | 复制

 1
 2
 hdfs dfs -mkdir /user
 hdfs dfs -mkdir /user/hadoop #根据自己的情况调整/user/<username>


复制输入文件（要处理的文件）到HDFS上
Text | 复制

 1
 hdfs dfs -put /usr/hadoop/etc/hadoop input


查看我们复制到HDFS上的文件

Text | 复制

 1
 hdfs dfs -ls input


运行单词检索（grep）程序
Text | 复制

 1
 2
 3
 4
 hadoop jar /usr/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.1.jar grep input output 'dfs[a-z.]+'
 #WordCount
 #hadoop jar /usr/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.1.jar wordcount input output
 #说明：output文件夹如已经存在则需要删除或指定其他文件夹。


查看运行结果

Text | 复制

 1
 hdfs dfs -cat output/*

配置YARN

修改mapred-site.xml

Text | 复制

 1
 2
 3
 4
 5
 6
 7
 8
 9
 10
 cd /usr/hadoop/etc/hadoop/
 cp mapred-site.xml.template mapred-site.xml
 vi mapred-site.xml
 #添加如下内容
 <configuration>
      <property>
          <name>mapreduce.framework.name</name>
          <value>yarn</value>
      </property>
 </configuration>


修改yarn-site.xml
Text | 复制

 1
 2
 3
 4
 5
 6
 7
 8
 vi yarn-site.xml
 #添加如下内容
 <configuration>
      <property>
          <name>yarn.nodemanager.aux-services</name>
          <value>mapreduce_shuffle</value>
      </property>
 </configuration>


启动YARN

Text | 复制

 1
 2
 start-yarn.sh
 #停止yarn stop-yarn.sh


查看当前java进程
Text | 复制

 1
 2
 3
 4
 5
 6
 7
 8
 jsp
 #输出如下
 4918 ResourceManager
 1663 NameNode
 1950 SecondaryNameNode
 5010 NodeManager
 5218 Jps
 1759 DataNode


运行你的mapReduce程序

配置好如上配置再运行mapReduce程序时即是yarn中运行。


使用web查看Yarn运行状态
Text | 复制

 1
 http://你的服务器ip地址:8088/

9 HDFS常用命令
创建HDFS文件夹

在根目录创建input文件夹
Text | 复制

 1
 hdfs dfs -mkdir -p /input


在用户目录创建input文件夹


说明： 如果不指定“/目录”，则默认在用户目录创建文件夹

Text | 复制

 1
 2
 hdfs dfs -mkdir -p input
 #等同于 hdfs dfs -mkdir -p /user/hadoop/input

查看HDFS文件夹

查看HDFS根文件夹
Text | 复制

 1
 hdfs  dfs  -ls /


查看HDFS用户目录文件夹

Text | 复制

 1
 hdfs  dfs  -ls


查看HDFS用户目录文件夹下input文件夹

Text | 复制

 1
 2
 hdfs  dfs  -ls input
 #等同与 hdfs  dfs  -ls /user/hadoop/input

复制文件到HDFS
Text | 复制

 1
 hdfs dfs -put /usr/hadoop/etc/hadoop input

删除文件夹

Text | 复制

 1
 hdfs  dfs  -rm -r input

10 参考资料

单机伪分布式搭建教程： http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html


集群环境搭建教程： http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/ClusterSetup.html
