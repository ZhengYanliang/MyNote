##代码从svn更新到本地
1.屏蔽各个页面的 getCookie方法


2.导入时，屏蔽登录判断逻辑





3.验证登录


4.common.jsp中的验证是否登录逻辑
```
<!-- 
    <script type="text/javascript" src="http://test.site.haiziwang.com/scripts/sso.js"></script>
    
    <script type="text/javascript">
        $(document).ready(function()
        {
            var siteUserId = getCookie("siteUserId");
            var siteToken = getCookie("siteToken");
            $.ajax({
                url:'http://test.site.haiziwang.com/sso-web/checkuser/checkUserLogin.do',
                data:{"appCode":"PRO","siteUserId":siteUserId,"siteToken":siteToken},
                dataType:'json',
                cache:false,
                type:'post',
                success:function(data)
                {
                    if(!data.success)
                    {
                        alert("请重新登录");
                        parent.location.href="http://test.site.haiziwang.com";
                    }
                }
            });
            
        })          -->   

```
##代码从测试环境合并到生产环境
10.12要合并的代码列表
```
http://172.172.177.22/svn/back/sale_center/trunk/saleCenter-web/src/main/webapp/views/salesdetail.jsp
http://172.172.177.22/svn/back/sale_center/trunk/saleCenter-web/src/main/webapp/views/advanceddetail.jsp
http://172.172.177.22/svn/back/sale_center/trunk/saleCenter-web/src/main/webapp/views/pmSingleImport.jsp
http://172.172.177.22/svn/back/sale_center/trunk/saleCenter-web/src/main/webapp/views/salesImport.jsp
http://172.172.177.22/svn/back/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/ctrl/CustomerController.java
http://172.172.177.22/svn/back/sale_center/trunk/saleCenter-web/src/main/webapp/views/sales.jsp
http://172.172.177.22/svn/back/sale_center/trunk/saleCenter-web/src/main/resources/config/power.properties
http://172.172.177.22/svn/back/sale_center/trunk/saleCenter-web/src/main/webapp/js/ajaxfileupload.js
http://172.172.177.22/svn/back/sale_center/trunk/saleCenter-web/src/main/webapp/views/pmSingleDetail.jsp
http://172.172.177.22/svn/back/sale_center/trunk/saleCenter-web/pom.xml
```


右键，选择文件夹后，可以直接导出修改的所有的文件及所在目录（推荐）。


##最新列表
```
/sale_center/trunk/saleCenter-web/src/main/webapp/views/salesdetail.jsp
/sale_center/trunk/saleCenter-web/src/main/webapp/views/advanceddetail.jsp
/sale_center/trunk/saleCenter-web/src/main/webapp/views/pmSingleImport.jsp
/sale_center/trunk/saleCenter-web/src/main/webapp/views/salesImport.jsp
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/ctrl/CustomerController.java
/sale_center/trunk/saleCenter-web/src/main/webapp/views/sales.jsp
/sale_center/trunk/saleCenter-web/src/main/resources/config/power.properties
/sale_center/trunk/saleCenter-web/src/main/webapp/js/ajaxfileupload.js
/sale_center/trunk/saleCenter-web/src/main/webapp/views/pmSingleDetail.jsp
/sale_center/trunk/saleCenter-web/src/main/webapp/views/groupbuy_createrule.jsp
/sale_center/trunk/saleCenter-web/src/main/webapp/views/groupbuy_info.jsp
/sale_center/trunk/saleCenter-web/pom.xml
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/Plugin.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/Util/DbConstant.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/Util/PropertiesUtil.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/ctrl/BetrayController.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/ctrl/GroupBuyController.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/ctrl/InterfaceController.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/ctrl/OrderDetailController.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/dbo/PricePubRefPo.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/dbo/PriceRuleCreatePramPo.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/dbo/PriceRuleDdo.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/dbo/SkuCombInfoPo.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/dbo/SkuGlobalPo.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/dbo/SkuInfoPo.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/dbo/VIPPriceDdo.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/dbo/VIPPricePo.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/dbo/VIPPriceRefPo.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/dbo/protocol/GetSkuInfoReq.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/dbo/protocol/GetSkuInfoResp.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/dbo/protocol/ModifyCombSkuRelationReq.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/dbo/protocol/ModifyCombSkuRelationResp.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/service/BaseService.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/service/BetrayServiceImpl.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/service/PmSingleServiceImpl.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/service/PromotionServiceImpl.java
/sale_center/trunk/saleCenter-web/src/main/java/com/haiziwang/jplugin/study/service/SalesServiceImpl.java
/sale_center/trunk/saleCenter-web/src/main/resources/config/basic-config.properties
/sale_center/trunk/saleCenter-web/src/main/resources/config/configcenter.properties
/sale_center/trunk/saleCenter-web/src/main/resources/config/database.config.properties
/sale_center/trunk/saleCenter-web/src/main/resources/config/plugin.cfg
/sale_center/trunk/saleCenter-web/src/main/resources/config/price.scene.properties
/sale_center/trunk/saleCenter-web/src/main/webapp/template/multiPrice.xlsx


```
要改动的地方
1.各个配置文件中，数据库将trunk改为stable中的配置，及去除test
2.页面中也有请求方法中用到test环境的也改为site.


##最终的提交记录
```
D:\hzw_res\sale_center\stable\saleCenter-web\pom.xml
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\ctrl\BetrayController.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\ctrl\CustomerController.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\ctrl\GroupBuyController.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\ctrl\InterfaceController.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\ctrl\OrderDetailController.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\dbo\PricePubRefPo.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\dbo\PriceRuleCreatePramPo.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\dbo\PriceRuleDdo.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\dbo\protocol\GetSkuInfoReq.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\dbo\protocol\GetSkuInfoResp.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\dbo\SkuInfoPo.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\Plugin.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\service\BaseService.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\service\BetrayServiceImpl.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\service\PmSingleServiceImpl.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\service\PromotionServiceImpl.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\service\SalesServiceImpl.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\Util\PropertiesUtil.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\resources\config\configcenter.properties
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\resources\config\monitor_report.xml
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\resources\config\plugin.cfg
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\resources\config\power.properties
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\resources\config\price.scene.properties
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\webapp\js\ajaxfileupload.js
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\webapp\template\multiPrice.xlsx
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\webapp\views\advanceddetail.jsp
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\webapp\views\groupbuy_createrule.jsp
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\webapp\views\groupbuy_info.jsp
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\webapp\views\pmSingleDetail.jsp
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\webapp\views\pmSingleImport.jsp
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\webapp\views\sales.jsp
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\webapp\views\salesdetail.jsp
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\webapp\views\salesImport.jsp
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\dbo\protocol\ModifyCombSkuRelationReq.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\dbo\protocol\ModifyCombSkuRelationResp.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\dbo\SkuCombInfoPo.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\dbo\SkuGlobalPo.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\dbo\VIPPriceDdo.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\dbo\VIPPricePo.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\dbo\VIPPriceRefPo.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\java\com\haiziwang\jplugin\study\Util\DbConstant.java
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\resources\config\basic-config.properties
D:\hzw_res\sale_center\stable\saleCenter-web\src\main\resources\config\database.config.properties


```


##发布到线上注意事项

备注：当发布到线上时，也要登录sso.haiziwang.com去创建资源（注意区分是菜单还是按钮） 、 角色等操作。

1.特供价sso配置相应的权限

http://sso.haiziwang.com/


添加资源 、创建角色










2. 由于此次更新中在Plugin.java中注册了新的数据源 salecenter_database    ; 登录sso-web配置应用中心【 http://site.haiziwang.com/main.html 】，配置数据库：salecenter _database










