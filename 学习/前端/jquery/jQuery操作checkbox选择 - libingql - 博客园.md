Libing的博客

博客园
首页
新随笔


管理

随笔 - 316  文章 - 0  评论 - 154

jQuery操作checkbox选择


1、checkbox list选择

效果图：



代码：

?
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
 14
 15
 16
 17
 18
 19
 20
 21
 22
 23
 24
 25
 26
 27
 28
 29
 30
 31
 32
 33
 34
 35
 36
 37
 38
 39
 40
 41
 42
 43
 44
 45
 46
 47
 48
 49
 50
 51
 52
 53
 54
 55
 56
 <! DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
 < html xmlns="http://www.w3.org/1999/xhtml">
 < head runat="server">
      < title ></ title >
      < script src="Scripts/jquery-1.7.min.js" type="text/javascript"></ script >
      < script type="text/javascript">
          $(function () {
              // 全选
              $("#btnCheckAll").bind("click", function () {
                  $("[name = chkItem]:checkbox").attr("checked", true);
              });
  
              // 全不选
              $("#btnCheckNone").bind("click", function () {
                  $("[name = chkItem]:checkbox").attr("checked", false);
              });
  
              // 反选
              $("#btnCheckReverse").bind("click", function () {
                  $("[name = chkItem]:checkbox").each(function () {
                      $(this).attr("checked", !$(this).attr("checked"));
                  });
              });
  
              // 全不选
              $("#btnSubmit").bind("click", function () {
                  var result = new Array();
                  $("[name = chkItem]:checkbox").each(function () {
                      if ($(this).is(":checked")) {
                          result.push($(this).attr("value"));
                      }
                  });
  
                  alert(result.join(","));
              });
          });
      </ script >
 </ head >
 < body >
      < div >
          < input name="chkItem" type="checkbox" value="今日话题" />今日话题
          < input name="chkItem" type="checkbox" value="视觉焦点" />视觉焦点
          < input name="chkItem" type="checkbox" value="财经" />财经
          < input name="chkItem" type="checkbox" value="汽车" />汽车
          < input name="chkItem" type="checkbox" value="科技" />科技
          < input name="chkItem" type="checkbox" value="房产" />房产
          < input name="chkItem" type="checkbox" value="旅游" />旅游
      </ div >
      < div >
          < input id="btnCheckAll" type="button" value="全选" />
          < input id="btnCheckNone" type="button" value="全不选" />
          < input id="btnCheckReverse" type="button" value="反选" />
          < input id="btnSubmit" type="button" value="提交" />
      </ div >
 </ body >
 </ html >



2、checkbox table选中

效果图：



代码：

?
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
 14
 15
 16
 17
 18
 19
 20
 21
 22
 23
 24
 25
 26
 27
 28
 29
 30
 31
 32
 33
 34
 35
 36
 37
 38
 39
 40
 41
 42
 43
 44
 45
 46
 47
 48
 49
 50
 51
 52
 53
 54
 55
 56
 57
 58
 59
 60
 61
 62
 63
 64
 65
 66
 67
 68
 69
 70
 71
 72
 73
 74
 75
 76
 77
 78
 79
 80
 81
 82
 83
 84
 85
 86
 87
 88
 89
 90
 91
 92
 93
 94
 95
 96
 97
 98
 99
 100
 101
 102
 103
 <! DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
 < html xmlns="http://www.w3.org/1999/xhtml">
 < head runat="server">
      < title ></ title >
      < style type="text/css">
          table
          {
              border-collapse: collapse;
          }
          td
          {
              border: 1px solid #ccc;
          }
      </ style >
      < script src="Scripts/jquery-1.7.min.js" type="text/javascript"></ script >
      < script type="text/javascript">
          $(function () {
              // chkAll全选事件
              $("#chkAll").bind("click", function () {
                  $("[name = chkItem]:checkbox").attr("checked", this.checked);
              });
  
              // chkItem事件
              $("[name = chkItem]:checkbox").bind("click", function () {
                  var $chk = $("[name = chkItem]:checkbox");
                  $("#chkAll").attr("checked", $chk.length == $chk.filter(":checked").length);
              })
          });
      </ script >
 </ head >
 < body >
      < table id="tb">
          < thead >
              < tr >
                  < td >
                      < input id="chkAll" type="checkbox" />
                  </ td >
                  < td >
                      分类名称
                  </ td >
              </ tr >
          </ thead >
          < tbody >
              < tr >
                  < td >
                      < input name="chkItem" type="checkbox" value="今日话题" />
                  </ td >
                  < td >
                      今日话题
                  </ td >
              </ tr >
              < tr >
                  < td >
                      < input name="chkItem" type="checkbox" value="视觉焦点" />
                  </ td >
                  < td >
                      视觉焦点
                  </ td >
              </ tr >
              < tr >
                  < td >
                      < input name="chkItem" type="checkbox" value="财经" />
                  </ td >
                  < td >
                      财经
                  </ td >
              </ tr >
              < tr >
                  < td >
                      < input name="chkItem" type="checkbox" value="汽车" />
                  </ td >
                  < td >
                      汽车
                  </ td >
              </ tr >
              < tr >
                  < td >
                      < input name="chkItem" type="checkbox" value="科技" />
                  </ td >
                  < td >
                      科技
                  </ td >
              </ tr >
              < tr >
                  < td >
                      < input name="chkItem" type="checkbox" value="房产" />
                  </ td >
                  < td >
                      房产
                  </ td >
              </ tr >
              < tr >
                  < td >
                      < input name="chkItem" type="checkbox" value="旅游" />
                  </ td >
                  < td >
                      旅游
                  </ td >
              </ tr >
          </ tbody >
      </ table >
 </ body >
 </ html >


分类: jQuery
好文要顶 关注我 收藏该文

libingql
关注 - 2
粉丝 - 384
+加关注
7
0
« 上一篇： Javascript实现table隔行换色
» 下一篇： implement

posted @ 2011-11-07 00:45 libingql 阅读( 258450 ) 评论( 0 ) 编辑 收藏


刷新评论 刷新页面 返回顶部
注册用户登录后才能发表评论，请 登录 或 注册 ， 访问 网站首页。
【推荐】50万行VC++源码: 大型组态工控、电力仿真CAD与GIS源码库
【推荐】融云发布 App 社交化白皮书 IM 提升活跃超 8 倍
【福利】Microsoft Azure给博客园的你专属三重大礼
【推荐】BPM免费下载


最新IT新闻 :
· 宝马带来HoloActive触控技术 虚拟界面取代物理接触
· 悄无声息 Apple Watch已经等于智能手表业？
· 理想主义者从不坚持到底——从大众软件停刊说开去
· Wifi新标准到来：提速4倍，下载一部蓝光电影只要46秒
· Unity引擎新版本：让开发者在VR里开发VR游戏
» 更多新闻...

最新知识库文章 :

· 高质量的工程代码为什么难写
· 循序渐进地代码重构
· 技术的正宗与野路子
· 陈皓：什么是工程师文化？
· 没那么难，谈CSS的设计模式
» 更多知识库文章...

 < 2011年11月 >

日 一 二 三 四 五 六
 30 31 1 2 3 4 5
 6 7 8 9 10 11 12
 13 14 15 16 17 18 19
 20 21 22 23 24 25 26
 27 28 29 30 1 2 3
 4 5 6 7 8 9 10

最新随笔
1. Bootstrap3插件系列：bootstrap-select2
2. UEditor编辑器使用示例
3. Bootstrap3系列：导航
4. Bootstrap3系列：输入框组
5. Bootstrap3系列：按钮式下拉菜单
6. Bootstrap3系列：按钮组
7. Bootstrap3系列：下拉菜单
8. CSS系列：CSS常用样式
9. Entity Framework中使用IEnumerable<T>、IQueryable<T>及IList<T>的区别
10. ASP.NET中Session的sessionState 4种mode模式

随笔分类
ASP.NET(4)
ASP.NET MVC(16)
Autofac(1)
Bootstrap(6)
C#(14)
CSS(12)
Entity Framework(20)
ExtJS(41)
HTML5(6)
JavaScript(18)
jQuery(11)
KendoUI(10)
LINQ(29)
MongoDB(7)
Oracle(6)
Python(3)
SQL(53)
UML基础(2)
平时积累(7)
设计模式(25)
文本编辑(1)
依赖注入(3)

随笔档案
2016年12月 (1)
2016年9月 (6)
2015年12月 (1)
2015年7月 (7)
2015年6月 (3)
2015年5月 (3)
2015年4月 (8)
2015年3月 (5)
2014年12月 (18)
2014年11月 (27)
2014年10月 (27)
2014年8月 (1)
2014年7月 (2)
2014年6月 (16)
2014年5月 (3)
2014年4月 (9)
2014年3月 (17)
2014年1月 (3)
2013年12月 (7)
2013年10月 (10)
2013年9月 (2)
2013年5月 (1)
2013年4月 (2)
2013年3月 (15)
2013年1月 (2)
2012年12月 (2)
2012年10月 (5)
2012年9月 (10)
2012年7月 (1)
2012年5月 (1)
2012年4月 (42)
2012年3月 (12)
2011年12月 (1)
2011年11月 (3)
2011年9月 (4)
2011年8月 (6)
2011年7月 (3)
2011年6月 (9)
2010年9月 (1)
2010年8月 (8)
2010年5月 (2)
2010年3月 (3)
2010年1月 (2)
2009年6月 (1)
2009年5月 (2)
2009年4月 (2)


Copyright ©2016 libingql