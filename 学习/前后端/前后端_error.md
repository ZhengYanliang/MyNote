[TOC]
##前台根据后台的list 动态 生成 checkbox
```
1.后台获取 list

price.user.properties:

银卡会员=1
金卡会员=2
铂金卡会员=3
黑金卡会员=4


package com.haiziwang.jplugin.study.Util;


import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.List;
import java.util.Map.Entry;
import java.util.Properties;
import java.util.Set;


public class PropertiesUtil {


private static Properties priceUserProperties = new Properties();


private static Properties modelSelfProperties = new Properties();


private PropertiesUtil() {
}


static {
try {
InputStream isUser = PropertiesUtil.class.getResourceAsStream("/config/price.user.properties");
BufferedReader bfUser = new BufferedReader(new InputStreamReader(isUser, "UTF-8"));
priceUserProperties.load(bfUser);
} catch (Exception e) {
e.printStackTrace();
}
}


public static String get(String key) {
String value = "";
if (properties.containsKey(key)) {
value = properties.getProperty(key);
}
return value;
}


public static String get(String key, String defaultValue) {
return properties.getProperty(key, defaultValue);
}


public static void reomve(String key) {
properties.remove(key);
}


public static String userGet(String key) {
String value = "";
if (userProperties.containsKey(key)) {
value = userProperties.getProperty(key);
}
return value;
}


public static String userGet(String key, String defaultValue) {
return userProperties.getProperty(key, defaultValue);
}


public static void userRemove(String key) {
userProperties.remove(key);
}


public static List<String> getAllUserLevel() {
List<String> res = new LinkedList<String>();
Set<Entry<Object, Object>> entrySet = priceUserProperties.entrySet();
for (Entry<Object, Object> entry : entrySet) {
res.add(entry.getKey() + ":" + entry.getValue());
}
return res;
}


public static String priceUserPropertiesGet(String key) {
String value = "";
if (priceUserProperties.containsKey(key)) {
value = priceUserProperties.getProperty(key);
}
return value;
}


public static boolean priceUserPropertiesContainsKey(String key) {
return priceUserProperties.containsKey(key);
}
}


package com.haiziwang.jplugin.study.dbo.model;
import java.io.Serializable;
public class UserLevelEntity implements Serializable {
    /**
     * 
     */
    private static final long serialVersionUID = 4647282480206966874L;
    private String levelName;
    private String levelValue;
    public String getLevelName() {
        return levelName;
    }
    public void setLevelName(String levelName) {
        this.levelName = levelName;
    }
    public String getLevelValue() {
        return levelValue;
    }
    public void setLevelValue(String levelValue) {
        this.levelValue = levelValue;
    }
}


/**
     * 获取用户等级
     */
    public void getUserLevel() {
        RespJson resjon = new RespJson();
        try {
            List<UserLevelEntity> levelList = new LinkedList<>();
            List<String> userLevel = PropertiesUtil.getAllUserLevel();
            if (userLevel != null && userLevel.size() > 0) {
                resjon.setResultCode("1");
                for (String level : userLevel) {
                    UserLevelEntity levelEntity = new UserLevelEntity();
                    levelEntity.setLevelName(level.split(":")[0]);
                    levelEntity.setLevelValue(level.split(":")[1]);
                    levelList.add(levelEntity);
                }
                resjon.setDatabody(levelList);
            } else {
                resjon.setResultCode("0");
                resjon.setMsg("no data");
            }
        } catch (Exception e) {
            resjon.setResultCode("0");
            resjon.setMsg("Query userLevel error");
        }
        renderJson(JSON.toJSONString(resjon));     } 


http://localhost:8083/jplugin-study/oprom/getUserLevel.do
{"databody":[{"levelName":"黑金卡会员","levelValue":"4"},{"levelName":"铂金卡会员","levelValue":"3"},{"levelName":"金卡会员","levelValue":"2"},{"levelName":"银卡会员","levelValue":"1"}],"msg":"","resultCode":"1","size":0,"totalRecord":0}


2.页面组织数据
<div class="row">
                <div class="col-md-12">
                    <div class="form-group">
                        <label class="control-label col-md-2" style="margin-top:10px;">促销对象:</label>
                        <div class="col-md-8 md-checkbox-inline" id="userLevelDiv">
                        
                            <%--
                            <c:forEach items="${userLevelList}" var="userLevel">
                                <div class="md-checkbox">
                                <input type="checkbox" id="ckMem1" name="promobj" class="md-checkboxbtn" value="${userLevel.levelValue}" />
                                <label for="ckMem1">
                                <span></span>
                                <span class="check"></span>
                                <span class="box"></span>
                                ${userLevel.levelName}</label>
                            </div>
                            </c:forEach>
                       --%>

                         
                        </div>
                    </div>
                </div>             </div>  


$(document).ready(function(){
        var userLevelList = getUserLevel();         console.log('userLevelList:'+userLevelList);  

});


function getUserLevel(){
        var levelList = [];
        $.ajax({
            url:$ctx +'/oprom/getUserLevel.do',
            dataType:'json',
            cache:false,
            type:'get',
            async:false,
            success:function(data){
                if(data.resultCode == "1"){
                    var divContent = "";
                    var userList = data.databody;
                    for(var i = 0; i<userList.length; i++){
                        levelList.push(userList[i]);
                        divContent += "<div class='md-checkbox'><input type='checkbox' id='ckMem"+i+"' name='promobj' class='md-checkboxbtn' value='"+userList[i].levelValue+"' /><label for='ckMem"+i+"'><span></span><span class='check'></span><span class='box'></span>"+userList[i].levelName+"</label></div>"
                    }
                    console.log('divContent:'+divContent);
                    $("#userLevelDiv").html(divContent);
                }else{
                    alert(data.msg);
                }
            }
        });
        return levelList;     }  

```
##设置全局变量

##前台传多个参数到后台
```
方式一：以字符串的方式如"12,13,14"
1.前台组织数据：
//循环select下拉框，获得所有的text值
var sellerids = "";
$("#selectedSellers option").each(function(){
sellerids += $(this).val() + ",";
});
sellerids = sellerids.substring(0,sellerids.lastIndexOf(","));
sellerids = JSON.stringify(sellerids);
console.log("sellerids:"+sellerids);
2.后台解析数据
String sellerids = getParam("sellerids");
String[] selleridArray = sellerids.split(",");
for (String sellid : selleridArray) {
                for (SkuInfoPo skuinfo : resp.getSkuInfoList()) {
                    if (sellid.equals(String.valueOf(skuinfo.getOrdinaryInfo().getCooperatorId()))) {
                        sbSku.append(skuinfo.getSkuId()).append(",");
                        break;
                    }
                }
            } 



方式二：以字符数组的形式如["12","13","14"]
1.前台组织数据：
//循环select下拉框，获得所有的text值
        var sellerids = [];
        $("#selectedSellers option").each(function(){
            sellerids.push($(this).val());
        });
        sellerids = JSON.stringify(sellerids);         console.log("sellerids:"+sellerids);  

2.后台解析数据：
String sellerInclude = getParam("sellerids"); JSONArray sellerIncludeList = JSONArray.parseArray(sellerInclude); 
Vector<uint64_t> sellerVector = new Vector<uint64_t>();
        if (sellerIncludeList != null) {
            for (int i = 0; i < sellerIncludeList.size(); i++) {
                sellerVector.add(new uint64_t(sellerIncludeList.getLongValue(i)));
            }
        } 



```
 
##保存列表信息
```
var paramJson = encodeURI(JSON.stringify(goodsList));  //前端编码
$.ajax({  
url:'${base}/cr/addCouponGoods.do?ruleId='+ruleId,// 跳转到 action
data:{"paramJson":paramJson},
}


/**
* 批量添加条码券商品
* 
* @param request
* @param response
* @return
*/
@RequestMapping(value = "addCouponGoods", produces = "text/html;charset=UTF-8")
public void addCouponGoods(HttpServletRequest request, HttpServletResponse response,
@RequestParam String paramJson) {
// 获取参数
Map<String, Object> result = new HashMap<String, Object>();
try {
CompanyInfoDto merchantInfo = getMerchantInfo(request);
String companyId = Long.toString(merchantInfo.getCompanyId());
String ruleId = request.getParameter("ruleId");
paramJson = java.net.URLDecoder.decode(paramJson, "utf-8");
log.info("DisRuleAction====>addCouponGoods====>paramJson:" + paramJson.toString());
// JSONObject jsonObject = JSONObject.fromObject(paramJson);
net.sf.json.JSONArray data = net.sf.json.JSONArray.fromObject(paramJson);


BaseResult br = disRuleService.addCouponGoods(data, ruleId, companyId);
if (br.getStatus().equals(MmcConstants.REG_SUCC)) {
result.put(STATUS, SUCCESS);
result.put(MESSAGE, "操作成功");
} else {
result.put(STATUS, ERROR);
result.put(MESSAGE, "操作失败");
}
```

## AJAX POST请求中参数以form data和request payload形式在servlet中的获取方式
HTTP请求中，如果是get请求，那么表单参数以name=value&name1=value1的形式附到url的后面，如果是 post请求，那么表单参数是在请求体中，也是以name=value&name1=value1的形式在请求体中。通过chrome的开发者工 具可以看到如下（这里是可读的形式，不是真正的HTTP请求协议的请求格式）：

```
get请求：
RequestURL:http://127.0.0.1:8080/test/test.do?name=mikan&address=street  
Request Method:GET  
Status Code:200 OK  
   
Request Headers  
Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8  
Accept-Encoding:gzip,deflate,sdch  
Accept-Language:zh-CN,zh;q=0.8,en;q=0.6  
AlexaToolbar-ALX_NS_PH:AlexaToolbar/alxg-3.2  
Connection:keep-alive  
Cookie:JSESSIONID=74AC93F9F572980B6FC10474CD8EDD8D  
Host:127.0.0.1:8080  
Referer:http://127.0.0.1:8080/test/index.jsp  
User-Agent:Mozilla/5.0 (Windows NT 6.1)AppleWebKit/537.36 (KHTML, like Gecko) Chrome/33.0.1750.149 Safari/537.36  
   
Query String Parameters  
name:mikan  
address:street  
   
Response Headers  
Content-Length:2  
Date:Sun, 11 May 2014 10:42:38 GMT  
Server:Apache-Coyote/1.1  

Post请求：
RequestURL:http://127.0.0.1:8080/test/test.do  
Request Method:POST  
Status Code:200 OK  
   
Request Headers  
Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8  
Accept-Encoding:gzip,deflate,sdch  
Accept-Language:zh-CN,zh;q=0.8,en;q=0.6  
AlexaToolbar-ALX_NS_PH:AlexaToolbar/alxg-3.2  
Cache-Control:max-age=0  
Connection:keep-alive  
Content-Length:25  
Content-Type:application/x-www-form-urlencoded  
Cookie:JSESSIONID=74AC93F9F572980B6FC10474CD8EDD8D  
Host:127.0.0.1:8080  
Origin:http://127.0.0.1:8080  
Referer:http://127.0.0.1:8080/test/index.jsp  
User-Agent:Mozilla/5.0 (Windows NT 6.1)AppleWebKit/537.36 (KHTML, like Gecko) Chrome/33.0.1750.149 Safari/537.36  
   
Form Data  
name:mikan  
address:street  
   
Response Headers  
Content-Length:2  
Date:Sun, 11 May 2014 11:05:33 GMT  
Server:Apache-Coyote/1.1  


这里要注意post请求的Content-Type为application/x-www-form-urlencoded，参数是在请求体中，即上面请求中的Form Data。

```

 在servlet中，可以通过request.getParameter(name)的形式来获取表单参数。

 而如果使用原生AJAX POST请求的话：

```

function getXMLHttpRequest() {  
          var xhr;  
          if(window.ActiveXObject) {  
                   xhr= new ActiveXObject("Microsoft.XMLHTTP");  
          }else if (window.XMLHttpRequest) {  
                   xhr= new XMLHttpRequest();  
          }else {  
                   xhr= null;  
          }  
          return xhr;  
}  
  
function save() {  
          var xhr = getXMLHttpRequest();  
          xhr.open("post","http://127.0.0.1:8080/test/test.do");  
          var data = "name=mikan&address=street...";  
          xhr.send(data);  
          xhr.onreadystatechange= function() {  
                   if(xhr.readyState == 4 && xhr.status == 200) {  
                            alert("returned:"+ xhr.responseText);  
                   }  
          };  
}  


通过chrome的开发者工具看到请求头如下：

RequestURL:http://127.0.0.1:8080/test/test.do  
Request Method:POST  
Status Code:200 OK  
   
Request Headers  
Accept:*/*  
Accept-Encoding:gzip,deflate,sdch  
Accept-Language:zh-CN,zh;q=0.8,en;q=0.6  
AlexaToolbar-ALX_NS_PH:AlexaToolbar/alxg-3.2  
Connection:keep-alive  
Content-Length:28  
Content-Type:text/plain;charset=UTF-8  
Cookie:JSESSIONID=C40C7823648E952E7C6F7D2E687A0A89  
Host:127.0.0.1:8080  
Origin:http://127.0.0.1:8080  
Referer:http://127.0.0.1:8080/test/index.jsp  
User-Agent:Mozilla/5.0 (Windows NT 6.1)AppleWebKit/537.36 (KHTML, like Gecko) Chrome/33.0.1750.149 Safari/537.36  
   
Request Payload  
name=mikan&address=street  
   
Response Headers  
Content-Length:2  
Date:Sun, 11 May 2014 11:49:23 GMT  
Server:Apache-Coyote/1.1  



注意请求的Content-Type为text/plain;charset=UTF-8，而请求表单参数在RequestPayload中。

```

 那么servlet中通过request.getParameter(name)却是空。为什么呢？而这样的参数又该怎么样获取呢？

为了搞明白这个问题，查了些资料，也看了Tomcat7.0.53关于请求参数处理的源码，终于搞明白了是怎么回事。

HTTP POST表单请求提交时，使用的Content-Type是application/x-www-form-urlencoded，而使用原生AJAX的 POST请求如果不指定请求头RequestHeader，默认使用的Content-Type是text/plain;charset=UTF-8。

```

 由于Tomcat对于Content-Type multipart/form-data（文件上传）和application/x-www-form-urlencoded（POST请求）做了“特殊处理”。下面来看看相关的处理代码。

Tomcat的HttpServletRequest类的实现类为 org.apache.catalina.connector.Request（实际上是org.apache.coyote.Request），而它对 处理请求参数的方法为protected void parseParameters()，这个方法中对Content-Type multipart/form-data（文件上传）和application/x-www-form-urlencoded（POST请求）的处理代码 如下：





protectedvoid parseParameters() {  
           //省略部分代码......  
           parameters.handleQueryParameters();// 这里是处理url中的参数  
           //省略部分代码......  
           if ("multipart/form-data".equals(contentType)) { // 这里是处理文件上传请求  
                parseParts();  
                success = true;  
                return;  
           }  
   
           if(!("application/x-www-form-urlencoded".equals(contentType))) {// 这里如果是非POST请求直接返回，不再进行处理  
                success = true;  
                return;  
           }  
           //下面的代码才是处理POST请求参数  
           //省略部分代码......  
           try {  
                if (readPostBody(formData, len)!= len) { // 读取请求体数据  
                    return;  
                }  
           } catch (IOException e) {  
                // Client disconnect  
                if(context.getLogger().isDebugEnabled()) {  
                    context.getLogger().debug(  
                            sm.getString("coyoteRequest.parseParameters"),e);  
                }  
                return;  
           }  
           parameters.processParameters(formData, 0, len); // 处理POST请求参数，把它放到requestparameter map中（即request.getParameterMap获取到的Map，request.getParameter(name)也是从这个Map中获取的）  
           // 省略部分代码......  
}  
   
   protected int readPostBody(byte body[], int len)  
       throws IOException {  
   
       int offset = 0;  
       do {  
           int inputLen = getStream().read(body, offset, len - offset);  
           if (inputLen <= 0) {  
                return offset;  
           }  
           offset += inputLen;  
       } while ((len - offset) > 0);  
       return len;  
    }  
从上面代码可以看出，Content-Type不是application/x-www-form-urlencoded的POST请求是不会读取请求体数据和进行相应的参数处理的，即不会解析表单数据来放到request parameter map中。所以通过request.getParameter(name)是获取不到的。
```

 那么这样提交的参数我们该怎么获取呢？

```

当然是使用最原始的方式，读取输入流来获取了，如下所示：

privateString getRequestPayload(HttpServletRequest req) {  
          StringBuildersb = new StringBuilder();  
          try(BufferedReaderreader = req.getReader();) {  
                   char[]buff = new char[1024];  
                   intlen;  
                   while((len = reader.read(buff)) != -1) {  
                            sb.append(buff,0, len);  
                   }  
          }catch (IOException e) {  
                   e.printStackTrace();  
          }  
          returnsb.toString();  
}  



当然，设置了application/x-www-form-urlencoded的POST请求也可以通过这种方式来获取。

 所以，在使用原生AJAX POST请求时，需要明确设置Request Header，即：

xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");  



另外，如果使用 jQuery ，我使用1.11.0这个版本来测试，$.ajax post请求是不需要明确设置这个请求头的，其他版本的本人没有亲自测试过。相信在1.11.0之后的版本也是不需要设置的。不过之前有的就不一定了。这个没有测试过。

```

2015-04-17后记：

最近在看书时才真正搞明白，服务器为什么会对表单提交和文件上传做特殊处理，因为表单提 交数据是名值对的方式，且Content-Type为application/x-www-form-urlencoded，而文件上传服务器需要特殊处 理，普通的post请求（Content-Type不是application/x-www-form-urlencoded）数据格式不固定，不一定是 名值对的方式，所以服务器无法知道具体的处理方式，所以只能通过获取原始数据流的方式来进行解析。

jquery在执行post请求时，会设置Content-Type为 application/x-www-form-urlencoded，所以服务器能够正确解析，而使用原生ajax请求时，如果不显示的设置 Content-Type，那么默认是text/plain，这时服务器就不知道怎么解析数据了，所以才只能通过获取原始数据流的方式来进行解析请求数 据。

来源：  http://blog.csdn.net/mhmyqn/article/details/25561535


##  provisional headers are shown 原因分析
```
情景再现：


在发送http请求时，审查元素查看网络，有时会出现provisional headers are shown。与此同时，点击preview、response你都会发现是空的。

原因：

client发送请求后，由于各种原因，比如网络延迟，server端逻辑错误，导致client端长时间未收到响应。


chrome下右键---->审查元素---->network---->查看请求，能看到此种错误

原因还有可能：断点调试时间过长导致，重启项目即可。

后台迟迟未给予响应带来的影响：
后续对这一url的请求都不会发送，浏览器给拦截了，这个情况是我在点击发送验证码时发现的，后台插入 MongoDB 时阻塞了，导致没有给予前台响应。从而导致后续请求被浏览器忽略掉。

解决方案

改正 占用很长时间的server端程序。

来源：  http://blog.csdn.net/wangjun5159/article/details/46912803

```
##414Request uri too large
```
原因：使用$.ajax的get方式。

$.ajax({
      url:$ctx+'/oprom/getOfflineBrandSelect.do',
      data:{"erpBrandName":encodeURI(brandname)},
      dataType:'json',
      cache:false,
      type:'get',
      success:function(data){
        if(data.resultCode == "0"){
        }
      });
解决：设置type='post'即可。
```