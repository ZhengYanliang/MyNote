##参与商家
```


sellerId:
["24","25","35","45","0"]

[24, 25, 35, 45, 0]
checkSku bug
branch:
[6, 45, 59, 0]   

当sellerid = 6,0时，都可将商品添加到已选商品中。


stable:同样的bug
解决：将商品for循环放到外面，并且 增加break;
for  (SkuInfoPo  skuinfo  :  resp .getSkuInfoList()) {
                 for  (String  sellid  :  sellerIdList ) {
                     if  ( sellid .equals(String. valueOf ( skuinfo .getOrdinaryInfo().getCooperatorId()))) {
                         sbSku .append( skuinfo .getSkuId()).append( "," );
                         break ;
                    }  else   if  ( sellModelArray .contains(PromotionEnum. SELLER_SELF .getValue())
                            && Integer. parseInt ( sellid ) >= 0 && Integer. parseInt ( sellid ) <= 7) { // 自营
                         sbSku .append( skuinfo .getSkuId()).append( "," );
                         break ;
                    }  else  {
                         boolean   global  =  skuinfo .getOrdinaryInfo().getSkuProperty().get(6);
                         if  ( global ) { // 全球购
                             sbSku .append( skuinfo .getSkuId()).append( "," );
                             break ;
                        }
                    }
                }             }   



```
##促销条件查询bug
```
测试一：
按商家ID查询 
限制商家：
<option value="59">流水验证12</option>

ruleId = 5329
测试二：
<option value="28">好孩子中国有限公司</option>

ruleId=5334




```
##跨店铺bug
```
1.销售模式的更改后，促销规则类型没有作出相应的改变


2.自营、限商家 都选择后，销售模式的改变，已选商家不能变为空。


3.在2的基础上，取消已选商家的选择个数，促销类型没有变回来，store_count的计算不准确（不能是全局变量，要由select option 中计算得出）
4.  缺了一个反向设置的场景，就是说，先选了普通的促销，然后反过来想改成跨店铺的


比如先选择了促销类型（满返），上面的销售模式（不能全选），已选商家只能支持1个。
5.添加已选商家，执行 removeSeller方法时，促销规则类型（如果有值）不应该变动。
6.当只勾选 自营时，促销类型都可以支持。


```
##新增字段JoinType、JoinSellerId
```
 加了个 JoinTyoe 字段
1 表示商家 =
0 表示 商家 in



仅自营：joinType=1&joinSellerId=3
仅商家：joinType=1&joinSellerId='input'
跨商家：joinType=0&joinSellerId='input'


 查询促销 ，仅自营 , sellId字段 是传 0 还是 3 ? （传0）

```
##跨店铺 反选的情况




##满件数与 奇偶 促销暂时不支持跨店铺


##页面风格 checkbox 
```
md-checkbox-inline  
class = 'md-checkboxbtn'


<span class="box"></span>
 

```
##跨店铺促销 自营商家 由 3 改为 1到7


```
String  sellerId  = getParam( "sellerId" );
        JSONArray  sellerIdList  = JSONArray. parseArray ( sellerId );
        Vector<uint64_t>  sellerVector  =  new  Vector<uint64_t>();
         if  ( sellerIdList  !=  null  &&  sellerIdList .size() > 0) {
             for  ( int   i  = 0;  i  <  sellerIdList .size();  i ++) {
                Long  seller  =  sellerIdList .getLongValue( i );
                 sellerVector .add( new  uint64_t( seller ));
            }
        }
         if  ( sellModelArray .contains(PromotionEnum. SELLER_SELF .getValue())) { // 自营
            List<String>  modelSelfList  = PropertiesUtil. getAllModelSelf ();
             for  ( int   i  = 0;  i  <  modelSelfList .size();  i ++) {
                Long  seller  = Long. parseLong ( modelSelfList .get( i ));
                 sellerVector .add( new  uint64_t( seller ));
            }
        }          rulePo .setSellerInclude( sellerVector ); // 参与商家   

```
##去除销售类型，增加商家类型


##老版本code
```
备注：已不能再用老版本的code了，因为po协议文件也换了，会报错。
/**
     * 获得商家
     * 
     *  @throws  ParseException
     *  @throws  UnsupportedEncodingException
     */
     public   void  getSellers() {
        WebStubCntl  stub  = CppClientProxyFactory. createCppWebStubCntl ();
        RespJson  resjon  =  new  RespJson();
        String  sellerMode  = getParam( "sellerMode" ); // 参与商家
        String  sellerCode  = getParam( "sellerCode" ); // 商家编码
         final  GetSellerInfoByFilterReq  req  =  new  GetSellerInfoByFilterReq();
         req .setMachineKey( this . MachineKey  +  "haiziwang123123" );
         req .setSceneId( this . SceneId );
         req .setSource( this . Source );
        SellerFilterPo  sellerFilter  =  new  SellerFilterPo();
        JSONArray  sellerModeList  = JSONArray. parseArray ( sellerMode );
         if  ( sellerModeList  !=  null  &&  sellerModeList .size() > 0) {
             if  (! sellerModeList .contains(PromotionEnum. SELLER_SELF .getValue())
                    &&  sellerModeList .contains(PromotionEnum. SELLER_JOIN .getValue())) { // 联营
                 sellerFilter .setChannelType(3l);
            }  else   if  ( sellerModeList .contains(PromotionEnum. SELLER_SELF .getValue())
                    &&  sellerModeList .contains(PromotionEnum. SELLER_JOIN .getValue())) { // 自联营
                 sellerFilter .setChannelType(3l);
            }  else  {
                 sellerFilter .setChannelType(Long. parseLong (PromotionEnum. CHANNELTYPE_ON .getValue()));
            }
        }
         if  (!NormalUtil. isNullOrEmpty ( sellerCode )) {
             sellerFilter .setPartnerCode( sellerCode );
        }
         req .setSellerFilter( sellerFilter );
         final  GetSellerInfoByFilterResp  resp  =  new  GetSellerInfoByFilterResp();
         try  {
             stub .invoke( req ,  resp , 1024 * 1024);
             logger .info( "result:"  +  resp .getResult() +  "errormsg:"  +  resp .getErrmsg());
             if  ( resp .getResult() == 0 &&  resp . getVecsellerInfoDdo () !=  null  &&  resp . getVecsellerInfoDdo ().size() > 0) {
                 resjon .setResultCode( "1" );
                 resjon .setDatabody( resp . getVecsellerInfoDdo ());
            }  else  {
                 resjon .setResultCode( "0" );
                 resjon .setMsg( resp .getErrmsg());
            }
        }  catch  (Exception  e ) {
             resjon .setResultCode( "0" );
             resjon .setMsg( "Query Seller Error" );
             e .printStackTrace();
        }
        renderJson(JSON. toJSONString ( resjon ));     }   





//将所有商家保存到隐藏域中
     function  getsellerSelectOption(){
         var  sellModel = [];
        $( "input[name='sellModel']:checked" ).each( function (){
            sellModel.push($( this ).val());
        });
        console.log( "sellModel:" +sellModel);
         if (sellModel.length == 0){
            alert( "请选择商家类型" );
             return ;
        }
        
        $.ajax({
             url:$ctx + '/oprom/getSellers.do' ,
             data:{ "sellModel" :sellModel},
             dataType: 'json' ,
             cache: false ,
             type: 'get' ,
             success: function (data){
                  if (data.resultCode ==  "1" )
                 {
                      var  sellerId =  "" ;
                      var  sellerName =  "" ;
                      var  sellersList = data.databody;
                      var  sellModelType = 0;
                      var  index = 0;
                      for ( var  i=0;i<sellersList.length;i++)
                     {
                        sellerId += sellersList[i][ "partnerId" ] +  "," ;
                         sellerName += sellersList[i][ "partnerName" ] +  "," ;
                     }
                      if (sellerId !=  ""  && sellerName !=  "" ){
                         $( "#displaySellerId" ).val(sellerId.substring(0, sellerId.length-1));
                         $( "#displaySellerName" ).val(sellerName.substring(0, sellerName.length-1));
                     }
                 } else {
                     alert( "商家不存在!" );
                      return   false ;
                 }
             }
         });     }   

```








```
function  isCross(seller_count,ruleType){
         var  flag =  true ;
         if (ruleType != 0 && ruleType != 1){ //非满减类型
             if (seller_count > 1){
                $( "#ruletype" ).val(1);
                $( "#ruletype" ).attr( "disabled" , "disabled" );
            }
        } else { //不限或满减
            $( "#ruletype" ).val(1);
            $( "#ruletype" ).attr( "disabled" , "disabled" );
        }
        showRuleType(); //促销规则
        showSetMoney(); //金额设置
         return  flag;     }   

```
##（跨店铺促销，全渠道促销）孩子王自营商家 id ,商家类型均为3. 由（1到7改为3）配置文件可以保留

