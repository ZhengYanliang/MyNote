首页 > 数据库 > Oracle >  正文 用Navicat连接Oracle数据库时报错ORA-28547:connection to server failed,probable Oracle Net admin error 2015-05-04       0  个评论     来源：高迎——廊坊师范学院信息技术提高班第九期   收藏 我要投稿

用Navicat连接Oracle数据库时出现如下错误



上网一查原来是oci.dll版本不对。因为Navicat是通过Oracle客户端连接Oracle服务器的，Oracle的客户端分为两种，一种是标准版，一种是简洁版，即Oracle Install Client。而我们用Navicat时通常会在自己的安装路径下包含多个版本的OCI，如果使用Navicat连接Oracle服务器出现ORA-28547错误时，多数是因为Navicat本地的OCI版本与Oracle服务器服务器不符造成的。所以我们要做的就是 下载 OCI使之与我们所安装的Oracle服务器相符合。

OCI下载地址： http://www.oracle.com/technetwork/database/features/instant-client/index-097480. html






值得注意的是不管你使用的是32位系统还是64位系统，都应该下载32位的Install Client.






还有一点要注意， Oracle 9i或以上版本的，要安装Install Client11或以下；Oracle8或8i服务器，需要安装Install Client10或以下。这个问题不大，因为我们现在的Oracle都是10或11了，注意一下就好。

然后在Navicat中配置一下，选择工具-选项






然后选择左边选项卡中的其他-OCI.






在OCI library中找到刚刚下载的文件夹中的oci.dll



这样就完成了Navicat配置，也就使得Navicat中的oci.dll版本和Oracle中的版本一致了，必须重启Navicat才能生效。这样整个配置就完成了。
Oracle开启archivelo Oracle数据库存储结构 Oracle架构实现原理、 Oracle存储过程和自定

