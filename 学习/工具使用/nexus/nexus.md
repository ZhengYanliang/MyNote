## Sonatype Nexus 库被删除的恢复方法
http://blog.csdn.net/lisq037/article/details/43938961
http://blog.csdn.net/tony168hongweigan/article/details/23256585
##Windows下使用Nexus创建私服
###Nexus列出了默认的几个仓库：
Public Repositories：仓库组，将所有策略为Release的仓库聚合并通过一致的地址提供服务。

3rd party：一个策略为Release的宿主类型仓库，用来部署无法从公共仓库获得的第三方发布版本构件。

Apache Snapshots：策略为Snapshots的代理仓库，用来代理Apache Maven仓库的快照版本构件。

Central：该仓库代理Maven的中央仓库，策略为Release，只会下载和缓存中央仓库中的发布版本构件。

Central M1 shadow：maven1格式的虚拟类型仓库。

Codehaus Snapshots：代理Codehaus Maven仓库快照版本的代理仓库。

Release：策略为Release的宿主类型仓库，用来部署组织内部的发布版本构件。

Snapshots：策略为Snapshots的宿主类型仓库，用来部署组织内部的快照版本构件。
###下载索引
点击左侧的Views/Repositories->下面出现Repositories->点击右边的Central->点击下面的Configuration
<img src="images/n_1.png" height="418" width="407">
将Download Remote Indexes值改为true，点击“save”后，点击左边的“Administration”->"Scheduled Tasks"链接，如果没有出现“Update Repositories Index”处于Running状态，那么需要在Public Repositories行右击，点击"Update Index"。
<img src="images/n_2.png" height="153" width="314">
然后再点击Schedule Tasks就可以看到有任务处于Running状态了。
<img src="images/n_3.png" height="435" width="296">
等到索引下载完成之后，就可以在"Repositories"界面中，选择Browser Index选项卡，可以看到Maven中央仓库内容的树形结构，如下图所示。
<img src="images/n_4.png" height="464" width="642">
在左边的搜索框中输入Spring关键字，然后搜索，会出现一大堆与Spring相关的结果。
<img src="images/n_5.png" height="798" width="1440">
###私有仓库配置
```
在Settings.xml中配置远程仓库，Maven提供的profile是一组可选的配置，可以用来设置或者覆盖配置默认值，在Setting.xml文件中加入以下代码，
<profiles>
<profile>
<id>local_nexus</id>
<repositories>
<repository>
<id>local_nexus</id>
<name>local_nexus</name>
<url>http://localhost:8081/nexus/content/groups/public/</url>
<releases>
    <enabled>true</enabled>
</releases>
<snapshots>
    <enabled>true</enabled>
</snapshots>
</repository>
</repositories>
<pluginRepositories>
<pluginRepository>
<id>local_nexus</id>
<name>local_nexus</name>
<url>http://localhost:8081/nexus/content/groups/public/</url>
<releases>
    <enabled>true</enabled>
</releases>
<snapshots>
    <enabled>true</enabled>
</snapshots>
</pluginRepository>
</pluginRepositories>
</profile>
</profiles>
<activeProfiles>
    <activeProfile>local_nexus</activeProfile>
</activeProfiles>
上面的配置中，使用了一个id为local_nexus的profile，这个profile包含了相关的仓库配置，同时配置中又使用了activeProfiles元素将nexus这个profile激活，这样当执行Maven构建的时候，激活的profile会将仓库配置应用到项目中去。
通过上面的配置，我们会发现Maven除了从Nexus下载构件外还会从中央仓库下载构件。既然是私服，那么我们就只希望Maven下载请求都仅仅通过Nexus。我们可以通过镜像实现这一需求。可以创建一个匹配任何仓库的镜像，镜像的地址是私服，这样Maven对任何仓库的构件下载请求都会转到私服中。把上面的配置修改为如下配置：
<profiles>
<profile>
<id>local_nexus</id>
<repositories>
<repository>
<id>local_nexus</id>
<name>local_nexus</name>
<url>http://localhost:8081/nexus/content/groups/public/</url>
<releases>
    <enabled>true</enabled>
</releases>
<snapshots>
    <enabled>true</enabled>
</snapshots>
</repository>
<repository>
<id>central</id>
<url>http://repo.maven.apache.org/maven2</url>
<releases>
    <enabled>true</enabled>
</releases>
<snapshots>
    <enabled>true</enabled>
</snapshots>
</repository>
</repositories>
<pluginRepositories>
<pluginRepository>
<id>local_nexus</id>
<name>local_nexus</name>
<url>http://localhost:8081/nexus/content/groups/public/</url>
<releases>
    <enabled>true</enabled>
</releases>
<snapshots>
    <enabled>true</enabled>
</snapshots>
</pluginRepository>
<pluginRepository>
<id>central</id>
<url>http://repo.maven.apache.org/maven2</url>
<releases>
    <enabled>true</enabled>
</releases>
<snapshots>
    <enabled>true</enabled>
</snapshots>
</pluginRepository>
</pluginRepositories>
</profile>
</profiles>
<activeProfiles>
    <activeProfile>local_nexus</activeProfile>
</activeProfiles>
```
###部署构件到私服
```
我们在实际开发过程是多个人的，那么总有一些公共模块或者说第三方构件是无法从Maven中央库下载的。我们需要将这些构件部署到私服上，供其他开发人员下载。用户可以配置Maven自动部署构件至Nexus的宿主仓库，也可以通过界面手动上传构件。

使用Maven部署构件到Nexus私服上
日常开发的快照版本部署到Nexus中策略为Snapshot的宿主仓库中，正式项目部署到策略为Release的宿主仓库中，POM的配置方式如下：
<distributionManagement> 
<repository> 
<id>local_nexus_releases</id> 
<name>core Release Repository</name> 
<url>http://localhost:8081/nexus/content/repositories/releases/</url> 
</repository> 
<snapshotRepository> 
<id>local_nexus_snapshots</id> 
<name>core Snapshots Repository</name> 
<url>http://localhost:8081/nexus/content/repositories/snapshots/</url> 
</snapshotRepository> 
</distributionManagement>
Nexus的仓库对于匿名用户只是只读的。为了能够部署构件，我们还需要再settings.xml中配置验证信息：
<pre name="code" class="plain"><servers>
  <server>
  <id>local_nexus_releases</id>
  <username>admin</username>
  <password>admin123</password>
  </server>
  <server>
  <id>local_nexus_snapshots</id>
  <username>admin</username>
  <password>admin123</password>
  </server>
</servers>
其中，验证信息中service的id应该与POM中repository的id一致。
在Nexus界面上手动部署第三方构件至私服
我们除了自己的构件要部署到Nexus私服上外，我们有可能还要将第三方构件（如：SQLService的JDBC）部署到Nexus上。这个时候，在Nexus界面上选择一个宿主仓库（如3rd party），再在页面下方选择Artifact Upload选项卡。填写对应的Maven坐标。然后点击“Select Artifact(s) for Upload”按钮从本机选择要上传的构件，然后点击“Add Artifact”按钮将其加入到上传列表中。最后，单击页面底部的“Upload Artifact(s)”按钮将构件上传到仓库中。
```
##通过nexus私服将jar包发布到远程服务器
```
看下部署至远程仓库：
1.编辑项目的pom.xml文件
<project>
   <distributionManagement>
      <repository>
<id>adanac-nexus-releases</id>
<name>adanac Release Repository</name>
<url> http://192.168.1.162:8081/nexus/content/repositories/releases/
</repository>
<snapshotRepository>
<id>adanac-nexus-snapshots</id>
<name>adanac Snapshot Repository</name>
<url> http://192.168.1.162:8081/nexus/content/repositories/snapshots/
</snapshotRepository>
</distributionManagement>
</project>
2.往远程仓库部署构件时，往往要配置认证，在setting.xml中，创建一个server元素，id与仓库的id匹配
<server> 
<id>adanac-nexus-releases</id>
<username>admin</username>
<password>admin123</password>
</server>
3.配置正确后，在执行mvn clean deploy ,maven就将项目构建输出的构件部署到配置对应的远程仓库
（如果项目当前的版本是快照版本，则部署到快照版本仓库地址）

```
###配置第三方插件
就是 3rd party、Snapshots、Releases这三个， 分别用来保存第三方jar（典型的oracle数据库的j驱动包），项目组内部的快照、项目组内部的发布版.
Repositories -> 3rd Party -> artifact Upload(GAV Definition 选择 GAV Parameters)

    

 



 






