br = new BaseResult(BaseResult.SUCCESS, MmcErrorCode.ERROR_MSG_UNKNOW);  

##freemarker根据状态处理跳转url
```
function reApply(){
        var url =  '${base}/pubapply/modStatus.do?id=${serviceApply.id}';
        $.ajax({
            type : "post",
            url : url,
            contentType : "application/json;charset=utf-8",
            dataType : "json",
            success : function(data) {
                if(data.status == "1"){
                    var isDetail = '${detail}';
                    var url = isDetail != '' ? "${base}/pubapply/toPubapply.do" : "${base}/admin.do?showProgress=toPubapply";
                    location.href =  url;
                   }else{
                       alert(data.errorMsg);
                   }
            }
        });     }  

```
##freemarker 解析后台传来的json字符串

```
1.java代码：
JSONArray oldGoodsList = JSONArray.fromObject(dataList); mav.addObject("oldGoodsList", oldGoodsList.toString());  

数据格式：[{"barCode":"110","name":"chenjianihao","ruleId":"4504f5b9e4c24b358458752427102b04","skuId":"201604171758069628","spuId":"f5ce4cf302fb405384e6127d7ce60712"},{"barCode":"11","name":"小白好美的","ruleId":"4504f5b9e4c24b358458752427102b04","skuId":"7632f2ed211b42bc9ea9d963c19e98df","spuId":"2c6d16e703ee462b98099c6296009e10"}] 


2.页面接收：
方式一：<input type="text" id="oldGoodsList" value='eval("("+${oldGoodsList}+")")' /> 【格式不正确，要处理】
数据格式:
eval(+"[{"barCode":"110","name":"chenjianihao","ruleId":"4504f5b9e4c24b358458752427102b04","skuId":"201604171758069628","spuId":"f5ce4cf302fb405384e6127d7ce60712"},{"barCode":"11","name":"小白好美的","ruleId":"4504f5b9e4c24b358458752427102b04","skuId":"7632f2ed211b42bc9ea9d963c19e98df","spuId":"2c6d16e703ee462b98099c6296009e10"}] ）“+）
方式二：
<#assign text>
        ${oldGoodsList}
    </#assign>
     <#assign jsonBar=text?eval />
    <#list jsonBar as item>
        <input type="hidden" id="oldBarCode" name="oldBarCode" value="${item.barCode}" />110
        <input type="hidden" id="oldBarName" name="oldBarName" value="${item.name}" /> chenjianihao
        <input type="hidden" id="oldRuleId" name="oldRuleId" value="${item.ruleId}" />
        <input type="hidden" id="oldSkuId" name="oldSkuId" value="${item.skuId}" />
        <input type="hidden" id="oldSpuId" name="oldSpuId" value="${item.spuId}" />     </#list> 
数据格式： 

[{"barCode":"110","name":"chenjianihao","ruleId":"4504f5b9e4c24b358458752427102b04","skuId":"201604171758069628","spuId":"f5ce4cf302fb405384e6127d7ce60712"},{"barCode":"11","name":"小白好美的","ruleId":"4504f5b9e4c24b358458752427102b04","skuId":"7632f2ed211b42bc9ea9d963c19e98df","spuId":"2c6d16e703ee462b98099c6296009e10"}]  

```
###判断变量存在

```
方式一：
<#if mouse?exists>
     Mouse found
<#else>
     No mouse found
</#if>

方式二：
<#if mouse??>
     Mouse found
<#else>
     No mouse found
</#if>



对与多级访问变量也一样：
<#if (animals.python.price)!0>....</#if> 
【注：加了括号，3个变量只要有一个不存在，就会赋值为0；如果不加括号，则animals.python存在，而仅仅最后一个子变量不存在时，赋值为0】。
    或者
<#if (animals.python.price)??>....</#if>

```
###判断变量不存在
```
<#if !mouse??>
     no mouse found
< /#if>

```
###freemarker动态设置select的值
```
下拉列表：
<select id="pushHour" value="${pushHour}"  data-placeholder="时">
    <option value="00" <#if pushHour == '00'>selected="selected"</#if>>00</option>
    <option value="01" <#if pushHour == '01'>selected="selected"</#if>>01</option>     <option value="02" <#if pushHour == '02'>selected="selected"</#if>>02</option>
</select> 



单选框：
<input type="radio"  name="pushmode" id="pushmode" value="1" <#if msg.pushmode!=null && msg.pushmode == 1>checked</#if> >  

```


###根据状态不同跳转不同的链接
```
<#if roleMap.m3??>
   <#if roleMap.m3=='1'>
       <p><a href="" class="button" >立即申请</a></p>
   <#else>
      <p><a href="" class="button " >立即使用</a></p>
   </#if>
<#else>
    <p><a href="" class="button  disable" >立即申请</a></p> </#if> 
``` 

##freemarker 判断list是否为空（要点利用长度）及if ..else ..用法
```
注意：这种方式只能用在对象存在（不为空）的情况 下。
<#if (appr?size > 0)>非空<#else>空</#if> 
<#if (appr?size > 0)>非空<#elseif appr?size > 1>长度大于1<#else>空</#if> 
```
##freemarker判断对象是否为空
```
freemarker中显示某对象使用${name}.
 
但如果name为null，freemarker就会报错。如果需要判断对象是否为空：
<#if name??>
……
</#if>
 
当然也可以通过设置默认值${name!''}来避免对象为空的错误。如果name为空，就以默认值（“!”后的字符）显示。
 
对象user，name为user的属性的情况，user，name都有可能为空，那么可以写成${(user.name)!''},表示user或者name为null，都显示为空。


<#if (user.name)??>
    不为空

<#else>
    为空

</#if>
```
##freemarker格式化日期
```
1、格式化日期${updated ? string("yyyy-MM-dd HH:mm:ss")}
如果指定的变量不一定存在：${(dateMap.beginTime?string("yyyy.MM.dd"))!''}


2、显示boolean值 
<#assign foo = true />
${ foo ? string("yes", "no")}


3、截取字符串长度 
<#if (userVO.cnname)??&&((userVO.cnname)?length > 10) > 
    ${userVO.cnname ? substring(0, 10)}
 <#else> 
    ${(userVO.cnname)!''} 
</#if>  




4、数字格式 
Freemarker中预订义了三种数字格式：number,currency（货币）和percent(百分比)其中number为默认的数字格式转换 
例如：
<#assign tempNum=20>  
${tempNum}      
${tempNum?string.number}或${tempNum?string(“number”)}  结果为20  
${tempNum?string.currency}或${tempNum?string(“currency”)}  结果为￥20.00  
${tempNum?string. percent}或${tempNum?string(“percent”)}  结果为2,000%


5.${a.releaseDate?string("dd MMMM yyyy")}，结果显示：18 十月 2014


更多请参考： http://freemarker.org/docs/ref_builtins_date.html
```
##处理时间格式
```
1.在js中截取长度的方式：
{
    field: "createTime",
    title: "<div style='min-width:180px'>创建日期</div>",
    render: function(data) {
        var res = "";
        var time = data.value;
        if (time != null) {
            res = time.substring(0, time.length - 2);
        }
        return res;
    }
},  



2.在ftl文件中格式化成时间（yyyy-mm-dd HH:mm:ss）：（去掉秒后面的.0）

<#if soDetailOut.afterApply.createTime !="">
    ${soDetailOut.afterApply.createTime?datetime}
</#if>
<input type="text" class="form-control today validate[required]" id="startDttm"
name="startDttm" value="${cr.startTime?datetime!'' }" size="16" value=""
placeholder="">【可避免对象不存在的情况】  



3.在ftl中格式化成日期（yyyy-mm-dd）
value="${cr.startTime?date!'' }"



4.取日期或时间
date:只使用年、月、日
time:只使用时、分、秒和毫秒部分
datetime:日期和时间两部分都被使用


${dateVar?time}得到的是08:00:54 PM
```
##关于列表的字段展示问题
此问题可分为两种：
1.展示字段所在层级比较浅：


Controller中返回DataGrid<MainOrderOut>


实体类：


页面：




2.展示字段所在层级比较深：
controller返回实体又封装了一个实体：





页面：




## freemarker遍历list的用法
1.最常用的用法 ：
```
<#list users as user>
      <span>${user.name}</span>
      <span>${user.age}</span></br>
</#list>
这里，是假设java类里有一个users的数组，或者Map,或者List等等，它的里面放的是user类，每个user有自己name,age属性。
最后显示的结果就是users里面所有user的姓名和年龄。
``` 2.如果需要显示当前循环到第几项，可以这样写
```
<#list ["hello","welcome","hi"] as word>
    <span>${word_index+1},${word}</span></br>
</#list>
as 后面的那个变量，加上_index，就可以表示当前循环到第几项
结果是：
1,hello
2,welcome
3,hi
```
3.有时候，最后一项在显示的时候可能要做特殊处理，怎么判断最后一项？
```
<#list ["hello","welcome","hi"] as word>
    <span>${word}</span><#if word_has_next>,</#if></#list>
as 后面的那个变量，加上_has_next，就可以判断是否最后一项
结果是：
hello,welcome,hi
```
如果想在循环中判断到某一项时退出，可以这样做
```
   <span>${user.name}</span>
   <#if user.name == "pxx"><#break></#break>
</#list>

```
##freemarker的index
```
<#if goodsList?? && (goodsList?size>0)>
                                                            <#list goodsList as good>
                                                           <tr class="trTemp">
                                                             <td>
                                                                <input class="form-control input-inline input-sm wb100 codeInputs" id="fbarCodess" name="fbarCodeName" value="${good.barCode}" placeholder="2603234283069" type="text">
                                                             </td>
                                                             <td>
                                                                <input class="form-control input-inline input-sm wb100 barname" id="fbarName" name="barName" value="${good.name}" placeholder="贝因美1段奶粉" type="text">
                                                             </td>
                                                             <td><a class="layer_button dele" onclick="javascript:;">删行</a></td>
                                                           </tr>
                                                           <input type='hidden' id='skuId${good_index}' value="${good.skuId}" />
                                                           <input type='hidden' id='spuId${good_index}' value="${good.spuId}" />
                                                           </#list>                                                            <#else> 
error
</#if> 

```
## freemarker遍历对象的引入属性用法
一、java代码
```
FreightTemplateOut templateOut = supGoodsService.findTemplateOutById(id);             mv.addObject("templateOut", templateOut);
```
FreightTemplateOut.java
```
public class FreightTemplateOut implements Serializable {
private List<AreaFreightOut> areaList;
    public List<AreaFreightOut> getAreaList() {
        return areaList;
    }
    public void setAreaList(List<AreaFreightOut> areaList) {
        this.areaList = areaList;     }
}
```
部分(省略getter/setter方法)AreaFreightOut.java
```
public class AreaFreightOut implements Serializable {
/**
     * 区域列表.
     */
    private String area;
    /**
     * 首重（体积、件）.
     */
    private BigDecimal first;
    /**
     * 首费.
     */
    private BigDecimal firstFreight;
    /**
     * 续重（体积、件）.
     */
    private BigDecimal continued;
    /**
     * 续费.
     */     private BigDecimal continuedFreight;
}
```
二、页面ftl文件
```
<#list templateOut.areaList as tempList>
 <tr>
   <td>${tempList.area }<a href="javascript:openAttrs();">编辑</a></td>
   <td><input id="" name="first" value="${tempList.first }" style="width: 50px;display: inline-block" type="text" class="form-control"></td>
   <td><input id="" name="firstFreight" value="${tempList.firstFreight }" style="width: 50px;display: inline-block" type="text" class="form-control"></td>
   <td><input id="" name="continue" value="${tempList.continued }" style="width: 50px;display: inline-block" type="text" class="form-control"></td>
   <td><input id="" name="continueFreight" value="${tempList.continuedFreight }" style="width: 50px;display: inline-block" type="text" class="form-control"></td>
   <td><a href="">删除</a></td>
</tr>
</#list>                             
```
##freemarker遍历map
一、写Map<String,Map<String,Object>>
```
Map<String,Object>  userInfo = new HashMap<String,Object>();
userInfo.put("username","allen");


Map<String,Object> map = new HashMap<String,Object>();
map.put("key","value");
userInfo.put("map",map);
这样就构造出了一个复杂的Map
```


二、ftl模板中对这个map进行遍历
```
因为${userInfo.username}是个字符串这样就可以得到值，
而userinfo.map是个Map对象所以采用以下方式遍历，
这些方法可以写在html区域的代码中也可以写在javascript区域的代码中
<#assign map = userinfo.map>
   <#list map?keys as key> 
      ${key}   ------   ${map[key]}
   </#list>
```
##ftl判断list是否为空
```
要点：进行两次if决断即可
<#list soDetailOut.skuOutList as sku>
<#if sku!=null>
    <div class="caption">
           <i class="fa fa-cogs"></i>商品信息
    </div>
     <div style="border:1px solid #3598DC;padding:8px;">
                            <table>
                                <thead>
                                    <tr class="sou-head">
                                        <td>商品</td>
                                    </tr>
                                </thead>
                                 <#list soDetailOut.skuOutList as sku>
                                 <#if sku!=null>
                                  <tr>
                                    <td>
                                        <div class="sou-inline">
                                                <div class="sou-pic">
                                                    <img src="${sku.skuPic }" alt="">
                                                </div>
                                                <div class="sou-name">
                                                    <p>${sku.skuTitle}</p>
                                                </div>
                                          </div>
                                    </td>
                                   </tr>
                                   </#if>
                                  </#list>
                                </tbody>
                            </table>
 </#if> </#list>  

```