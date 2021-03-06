[TOC]
##dependencyManagement
```
parent.pom.xml中使用dependencyManagement
如果子模块中没有声明某个dependcy,那么这个dependency依赖就不会被引入，这正是dependencyManagement的灵活性所在。
```
##将maven工程打包为jar
```
1. maven-shade-plugin
2. maven-assembly-plugin
3. maven-onejar-plugin

maven-shade-plugin是我在ebay时前辈介绍给我的，我觉得它使用方便且没有出现过问题。但是我看别人的源代码，发现大家用的更多的是assembly，所以这里总结下这两种插件的用法。至于第三个，先留个坑在这，以后用到再总结。

使用插件maven-shade-plugin可以方便的将项目已jar包的方式导出，插件的好处在于它会把项目所依赖的其他jar包都封装起来，这种jar包放在任何JVM上都可以直接运行，我最初使用eclipse的maven-build直接打包，转移到intellij idea后没有这个按钮了，就只能用命令行搞了
 
使用步骤 ：
将插件添加到pom.xml中，需要改的地方就是mainClass，在这里指定main方法的位置
使用mvn package打包，最后到projectName/target/下查找目标jar包
    
<build>
<plugins>
<plugin>
<groupId>
  org.apache.maven.plugins
</groupId>
<artifactId>
maven-shade-plugin
</artifactId>
<version>
2.3
</version>
<executions>
<execution>
<phase>
package
</phase>
<goals>
<goal>
shade
</goal>
</goals>
<configuration>
<transformers>
<transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
<mainClass>
core.Test
</mainClass>
</transformer>
</transformers>
</configuration>
</execution>
</executions>
</plugin>
</plugins>
</build>
```
##Assembly 插件
```
来源：  http://www.cnblogs.com/xinsheng/p/4109573.html
```
##maven命令创建webapp
```
Creating a webapp

Use  maven-archetype-webapp  to start a simple webapp maven project. The command is as follows

mvn archetype:create 
  -DgroupId=[your project's group id]
  -DartifactId=[your project's artifact id]
  -DarchetypeArtifactId=maven-archetype-webapp

This would then create a maven project.
.
 |-- src
 |   `-- main
 |       `-- java
 |           |-- resources
 |           |-- webapp
 |           |   `-- WEB-INF
 |           |       `-- web.xml
 |           `-- index.jsp
  `-- pom.xml

来源：  https://maven.apache.org/plugins-archives/maven-archetype-plugin-1.0-alpha-7/examples/webapp.html
```
##maven常用命令
```
# 创建项目 mvn archetype:create -DgroupId=packageName -DartifactId=projectName # 创建Maven的Web项目： mvn archetype:create -DgroupId=packageName -DartifactId=webappName -DarchetypeArtifactId=maven-archetype-webapp # 查看项目依赖树 mvn dependency:tree # 打印出已解决依赖的列表 mvn dependency:resolve # 编译源代码 mvn compile # 打包 mvn package # 在本地Repository中安装jar mvn install # 删除再编译，打包不测试 mvn clean install  -Dmaven.test.skip= true # 生成eclipse项目 mvn eclipse:eclipse # 清除eclipse的一些系统设置 mvn eclipse:clean # 启动Jetty 服务 mvn jetty:run # 将项目发行到仓库 mvn deploy

来源：  http://blog.csdn.net/zy416548283/article/details/46709705
```
## 激活Maven profile的几种方式
```
对于 Maven 来说又是怎样呢？整个项目定义好了项目对象模型（POM），就像论坛为每个人提供了默认的行为功能，如果我想改变我机器上的 POM 呢？这时就可以使用 profile。

<profiles>  
  <profile>  
    <id>jdk16</id>  
    <activation>  
      <jdk>1.6</jdk>  
    </activation>  
    <modules>  
      <module>simple-script</module>  
    </modules>  
  </profile>  
</profiles>  
这个 profile 的意思是，当机器上的 JDK 为1.6的时候，构建 simple-script 这个子模块，如果是1.5或者1.4，那就不构建，这个 profile 是由环境自动激活的。
1. 根据环境自动激活。
如前一个例子，当 JDK 为1.6的时候，Maven 就会自动构建 simple-script 模块。除了 JDK 之外，我们还可以根据操作系统参数和 Maven 属性等来自动激活 profile，如：
<profile>  
  <id>dev</id>  
  <activation>  
    <activeByDefault>false</activeByDefault>  
    <jdk>1.5</jdk>  
    <os>  
      <name>Windows XP</name>  
      <family>Windows</family>  
      <arch>x86</arch>  
      <version>5.1.2600</version>  
    </os>  
    <property>  
      <name>mavenVersion</name>  
      <value>2.0.5</value>  
    </property>  
    <file>  
      <exists>file2.properties</exists>  
      <missing>file1.properties</missing>  
    </file>  
  </activation>  
  ...  
</profile>  

2通过命令行参数激活。
这是最直接和最简单的方式，比如你定义了一个名为 myProfile 的 profile，你只需要在命令行输入 mvn clean install -Pmyprofile 就能将其激活，这种方式的好处很明显，但是有一个很大的弊端，当 profile 比较多的时候，在命令行输入这写 -P 参数会让人觉得厌烦，所以，如果你一直用这种方式，觉得厌烦了，可以考虑使用其它自动激活的方式。
 
3. 配置默认自动激活。
方法很简单，在配置 profile 的时候加上一条属性就可以了，如：
<profile>  
  <id>dev</id>  
  <activation>  
    <activeByDefault>true</activeByDefault>  
  </activation>  
  ...  
</profile>  
在一个特殊的环境下，配置默认自动激活的 profile 覆盖默认的 POM 配置，非常简单有效。
 
4. 配置 settings.xml 文件 profile 激活。
settings.xml 文件可以在 ~/.m2 目录下，为某个用户的自定义行为服务，也可以在 M2_HOME/conf 目录下，为整台机器的所有用户服务。而前者的配置会覆盖后者。同理，由 settings.xml 激活的 profile 意在为用户或者整个机器提供特定环境配置，比如，你可以在某台机器上配置一个指向本地数据库 URL 的 profile，然后使用该机器的 settings.xml 激活它。激活方式如下：
<settings>  
  ...  
  <activeProfiles>  
    <activeProfile>local_db</activeProfile>  
  </activeProfiles>  
</settings>  

```
##eclipse通过maven根据不同的环境打包
```
install -DskipTests -Denv=DEV  开发环境
install -DskipTests -Denv=TEST 测试环境
``` 
##maven打包或测试时跳过测试包下的junit类命令
```
方式1：mvn install -DskipTests  
方式2：(推荐)maven.test.skip属性跳过测试的编译,mvn install -Dmaven.test.skip=true  （不有要多余空格）
```
##maven依赖关系中Scope的作用 
```
Dependency Scope 

在POM 4中，<dependency>中还引入了<scope>，它主要管理依赖的部署。目前<scope>可以使用5个值： 

* compile，缺省值，适用于所有阶段，会随着项目一起发布。 
* provided，类似compile，期望JDK、容器或使用者会提供这个依赖。如servlet.jar。 
* runtime，只在运行时使用，如JDBC驱动，适用运行和测试阶段。 
* test，只在测试时使用，用于编译和运行测试代码。不会随项目发布。 
* system，类似provided，需要显式提供包含依赖的jar，Maven不会在Repository中查找它。

依赖范围控制哪些依赖在哪些 classpath  中可用，哪些依赖包含在一个应用中。让我们详细看一下每一种范围：

compile  （编译范围）

compile 是默认的范围；如果没有提供一个范围，那该依赖的范围就是编译范围。编译范围依赖在所有的 classpath  中可用，同时它们也会被打包。

provided  （已提供范围）

provided  依赖只有在当 JDK  或者一个容器已提供该依赖之后才使用。例如，   如果你开发了一个 web  应用，你可能在编译  classpath  中需要可用的 Servlet API  来编译一个 servlet ，但是你不会想要在打包好的 WAR  中包含这个 Servlet API ；这个 Servlet API JAR  由你的应用服务器或者 servlet  容器提供。已提供范围的依赖在编译 classpath  （不是运行时）可用。它们不是传递性的，也不会被打包。

runtime  （运行时范围）

runtime  依赖在运行和测试系统的时候需要，但在编译的时候不需要。比如，你可能在编译的时候只需要 JDBC API JAR ，而只有在运行的时候才需要 JDBC
驱动实现。

test  （测试范围）

test 范围依赖   在一般的编译和运行时都不需要，它们只有在测试编译和测试运行阶段可用。

system  （系统范围）

system 范围依赖与 provided  类似，但是你必须显式的提供一个对于本地系统中 JAR  文件的路径。这么做是为了允许基于本地对象编译，而这些对象是系统类库的一部分。这样的构件应该是一直可用的， Maven  也不会在仓库中去寻找它。如果你将一个依赖范围设置成系统范围，你必须同时提供一个  systemPath  元素。注意该范围是不推荐使用的（你应该一直尽量去从公共或定制的  Maven  仓库中引用依赖）。
```