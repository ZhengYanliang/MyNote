[TOC]
##jquery复制一行
```
$('.create-passenger').on('click', function() {
      // 复制第一个乘客的DOM作为模板
      var template = $('.form .form-group:first-child').clone();
      // 将DOM模板插入到表单中，形成新的一行
      $('.form').append(template);
      template.find('input').val('');
  });
```
##JQUERY方法给TABLE动态增加行
var tr = "<tr><td>new</td></tr>";
$("table tr:eq(2)").after(tr);

// 删除除第一行外的所有行  
$("#table6 tr:not(:first)").remove();
```
一、数据准备  
<table id="table1">  
    <tr><th>文章标题</th><th>文章分类</th><th>发布时间</th><th>操作</th></tr>  
    <tr><td>测试</td><td>测试</td><td>测试</td><td>测试</td></tr>  
    <tr><td>测试</td><td>测试</td><td>测试</td><td>测试</td></tr>  
    <tr><td>测试</td><td>测试</td><td>测试</td><td>测试</td></tr>  
</table>  
<table id="table2">  
    <tr><td>文章标题</td><td>文章分类</td><td>发布时间</td><td>操作</td></tr>  
    <tr><td>测试</td><td>测试</td><td>测试</td><td>测试</td></tr>  
    <tr><td>测试</td><td>测试</td><td>测试</td><td>测试</td></tr>  
    <tr><td>测试</td><td>测试</td><td>测试</td><td>测试</td></tr>  
</table>  
<table id="table3">  
    <thead>  
        <tr><td>文章标题</td><td>文章分类</td><td>发布时间</td><td>操作</td></tr>  
    </thead>  
    <tbody>  
        <tr><td>测试</td><td>测试</td><td>测试</td><td>测试</td></tr>  
        <tr><td>测试</td><td>测试</td><td>测试</td><td>测试</td></tr>  
        <tr><td>测试</td><td>测试</td><td>测试</td><td>测试</td></tr>  
    </tbody>  
</table>  
<table id="table4">  
    <thead>  
        <tr><td>文章标题</td><td>文章分类</td><td>发布时间</td><td>操作</td></tr>  
    </thead>  
    <tbody>  
        <tr><td>测试</td><td>测试</td><td>测试</td><td>测试</td></tr>  
        <tr><td>测试</td><td>测试</td><td>测试</td><td>测试</td></tr>  
        <tr><td>测试</td><td>测试</td><td>测试</td><td>测试</td></tr>  
        <tr><td>测试3</td><td>测试</td><td>测试</td><td>测试</td></tr>  
    </tbody>  
</table>  

二、操作
<script type="text/javascript">
//1.鼠标移动行变色
    $("#table1 tr").hover(function(){  
        $(this).children("td").addClass("hover")  
    },function(){  
        $(this).children("td").removeClass("hover")  
    })  
    $("#table2 tr:gt(0)").hover(function() {  
        $(this).children("td").addClass("hover");  
    }, function() {  
        $(this).children("td").removeClass("hover");  
    });  

//2.奇偶行不同颜色 
    $("#table3 tbody tr:odd").css("background-color", "#bbf");  
    $("#table3 tbody tr:even").css("background-color","#ffc");   
    $("#table3 tbody tr:odd").addClass("odd")  
    $("#table3 tbody tr:even").addClass("even")  

//3.隐藏一行
    $("#table3 tbody tr:eq(3)").hide();  

//4.隐藏一列
    $("#table5 tr td::nth-child(3)").hide();  
    $("#table5 tr").each(function(){$("td:eq(3)",this).hide()});   

//5.删除一行 
// 删除除第一行外的所有行  
    $("#table6 tr:not(:first)").remove();  

//6.删除一列 
// 删除除第一列外的所有列  
    $("#table6 tr td:not(:nth-child(1))").remove();  

//7.得到（设置）某个单元格的值
//设置table7,第2个tr的第一个td的值。    
    $("#table7 tr:eq(1) td:nth-child(1)").html("value");    
    //获取table7,第2个tr的第一个td的值。    
    $("#table7 tr:eq(1) td:nth-child(1)").html();   

//8.插入一行：
//在第二个tr后插入一行  
    $("<tr><td>插入3</td><td>插入</td><td>插入</td><td>插入</td></tr>").insertAfter($("#table7 tr:eq(1)"));
</script>
```
##jquery获取table的行数、列数
$("#grd").find("tr").length; //行数
$("#grd").find("tr").find("td").length; //列数
##JQuery 动态修改Table中的内容
```
实例一：
< table   class = "table table-striped table-bordered table-hover"   id = "manual_table" >
                                         < thead >
                                             < tr >
                                               < th   > 满金额（元） </ th >
                                               < th   > 减金额（元） </ th >
                                             </ tr >
                                         </ thead >
                                         < tbody   id = "tb_condition" >
                                         </ tbody >                                      </ table >  
function  showStageTypeManual(conbody){
    
     var  conditionHtml =  " <tr><td><input name='upprice' value='10086'  " /></td></tr> ;
    $( "#tb_condition" ).append(conditionHtml);
}  

实例 二：
 省份：<select id="sl_province"></select>  
              
            <table>  
                <tr>  
                    <td>ID</td><td>城市名</td>  
                </tr>  
                <tbody id="tb_city">  
                      
                </tbody>  
            </table>


$(document).ready(function() {  
    //加载下拉框  
    $.getJSON("common!queryProvince" , function(data) {  
        //清空下拉框  
        $("#sl_province").text("");  
          
        $.each(data,function(i,ele){  
            //在下拉框中显示  
            if(i == 0) {  
                $("#sl_province").append("<option  value='0' selected>请选择省份</option><option value='"+ele.id+"'>"+ele.name+"</option>");  
            } else {  
                $("#sl_province").append("<option value='"+ele.id+"'>"+ele.name+"</option>");  
            }  
        });  
    } );  
      
    //监听改变事件  
    $("#sl_province").change(function() {  
        //清空table中的数据  
        $("#tb_city").text("");  
          
        var id = $("#sl_province").val();//获取选择的省份的ID  
        if(id > 0) {  
            //获取数据，加载  
            $.getJSON("common!queryCity?id=" + id , function(data) {  
                  
                $.each(data,function(i,ele){  
                    //在Table中显示  
                    $("#tb_city").append("<tr><td>" + ele.id + "</td><td>" + ele.name + "</td></tr>");  
                });  
            } );  
        }  
    });  
}); 
```
##jquery 清空table内容
如果表头放在了tbody中：
$("#dataTableDetail_off tbody tr:not(:first)").remove();
如果表对没有放在tbody中：
$("#dataTableDetail_off).html('');
##jquery 动态设置 tbody内容
```
<table id="tab_1"><tbody id="tab_1_tbody"></tbody></table>
方法一：$("#tab_"+index+" tbody").html("");

方法二：$("#tab_1_tbody").html("abc");
```
##jquery动态设置th的内容
```
var  thHtml;
     if (compensateType==1){
        thHtml =  "特供价(元)" ;
    } else   if (compensateType==2){
        thHtml =  "供应商承担比例(%)" ;
    }     $( '#compensate_detail thead tr' ).find( 'th:eq(4)' ).html(thHtml);   //设置第5个th的内容

```
## jQuery删除表格行且只保留前第一行
```
< table   class = "table table-striped table-bordered table-hover"   id = "manual_table" >
                                 < tbody >
                                     < tr >
                                       < td   style =" border-top-style : none ; border-bottom-style : none ; border-right-style : none " > 第     < input   type = "text"   name = "upprice"   onblur = 'javascript:checkNumber(this,0,0);'   size = "10" />     件 </ td >
                                       < td   style =" border-top-style : none ; border-bottom-style : none ; border-right-style : none " >< input   type = "text"   name = "subprice"   onblur = 'javascript:checkNumber(this,1,0);'   size = "10" />< span   class = "oven_tip" ></ span ></ td >
                                       < td   style =" border-top-style : none ; border-bottom-style : none ; border-right-style : none " >< a   class = "btn green new_rows"   onclick = "javascript:;" > 添加 </ a ></ td >
                                       < td   style =" border-top-style : none ; border-bottom-style : none ; border-right-style : none " ></ td >
                                     </ tr >
                                 </ tbody >                              </ table >  
 

function  showRuleType(){
    $( "#manual_table tbody tr" ).eq(0).nextAll().remove();//只保留首行
    $( "#manual_table tbody tr" ).eq(0).children().eq(0).find( 'input' ).first().val( '' );//清除首行第1个td内容
    $( "#manual_table tbody tr" ).eq(0).children().eq(1).find( 'input' ).first().val( '' );//清除首行第2个td内容
    showTip(); 
}   

function  showTip(){
     var  index = $( '#ruleType' ).get(0).selectedIndex;
    console.log( 'showTip:' +index);
     var  tip =  "" ;
     if (index == 0 ){                             //奇偶定价
        tip =  "   元" ;
    } else   if (index == 1){                         //奇偶打折
        tip =  "   折" ;
    }
    $( ".oven_tip" ).html(tip); 
}  
 

只保留首行
$("table tbody tr").eq(0).nextAll().remove();
var cit= $("#Tablename");
if(cit.size()>0) {
    cit.find("tr:not(:first)").remove();
}
保留首行与尾行
$("#manual_table tr:not(:first):not(:last)").remove();

```
##jquery实现隐藏指定列
```
<div id="disDiv>
< table   class = "table table-striped table-bordered table-hover"   id = "skuTable2" >
                                         < thead >
                                             < tr >
                                               < th   > 参与实体 </ th >
                                               < th   > 销售渠道 </ th >
                                               < th   > 供应商 </ th >
                                               < th   > 参考进价 </ th >
                                               < th   width = "10%" > 供应商承担比例(%) </ th >
                                               < th   style =" border-top-style : none ; border-bottom-style : none ; border-right-style : none "  width = "8%" ></ th >
                                               < th   style =" border-top-style : none ; border-bottom-style : none ; border-right-style : none "  width = "8%" ></ th >
                                             </ tr >
                                         </ thead >     
                                         < tbody >
                                             < tr >
                                               < td   > 默认 </ td >
                                               < td   > 默认 </ td >
                                               < td   id = "supplierTd2" ></ td >
                                               < td   id = "skuPriceTd2" ></ td >
                                               < td   width = "200px" >< input   id = "def_setradio"   name = "price_check"   placeholder = "10"   type = "text"   onblur = 'javascript:checkNumber(this,2);'   /></ td >
                                               < td   style =" border-top-style : none ; border-bottom-style : none ; border-right-style : none " >< a   class = "btn green new_rows"   onclick = "javascript:;" > 添加 </ a ></ td >
                                               < td   style =" border-top-style : none ; border-bottom-style : none ; border-right-style : none " ></ td >
                                             </ tr >
                                         </ tbody >                                      </ table >  
</div> 

$("#disDiv thead tr").find('th:eq(5)').hide();//隐藏disDiv的第6个th
$("#disDiv tbody tr").find('td:eq(5)').hide();//隐藏disDiv的第6个td

```
##jquery实现删除表格最后一列
```
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<title></title>
<script src="jquery.min.js" type="text/javascript"></script>
<script type="text/javascript"><!--
function addCol() {
  $th = $("<th>增加的列头</th>");
  $col = $("<td>增加的列</td>");
  $("#tab1>thead>tr").append($th);
  $("#tab1>tbody>tr").append($col);
}
function delCol() {
  //移除最后一列
  $("#tab1 tr :last-child").remove();
  //移除第一列
  //$("#tab1 tr :first-child").remove();
  //移除指定的列, 注:这种索引从1开始
  //$("#tab1 tr :nth-child(2)").remove();
  //移除第一列之外的列
  //$("#tab1 tr :not(:nth-child(1))").remove();
}
// --></script>
</head>
<body>
<input id="Button1" type="button" onclick="addCol()" value="增加列" />
<input id="Button2" type="button" onclick="delCol()" value="减少列" />
  <table id="tab1" border="1" style="width: 200px;">
    <thead>
    <tr>
      <th>
        Id</th>
      <th>
        Name</th>
    </tr>
    </thead>
    <tbody>
    <tr>
      <td>
        1</td>
      <td>
        叶子</td>
    </tr>
    <tr>
      <td>
        2</td>
      <td>
        王子</td>
    </tr>
    </tbody>
  </table>
</body>
</html>
```
##jquery实现动态删除一行
```

<a href='javascript:void(0);' onClick='delCurrentRow(this);'>删除</a> 
//删除当前行（不操作数据库）
function delCurrentRow(currentTd){
    var currentTr = $(currentTd).parent().parent();
    currentTr.remove();
} 

```
##jquery实现动态添加一行
```
方式一：
<td><a href='javascript:void(0);' onClick='addNewRow(this);'>添加</a>  </td>

//增加一行（不操作数据库）
function addNewRow(currentTd){
    var currentTr = $(currentTd).parent().parent();
    currentTr.after(currentTr.clone()); }  

方式二：
1.页面：
< tr >
< th   class = "ta_c" >< a   class = "layer_button new_rows"   onclick = "javascript:;" > 增行 </ a ></ th >
</ tr >   

2.js:
$( ".new_rows" ).on( "click" , function (){
     var  index = $( "#indexFlag" ).val();
     var  dom= '<tr><td><input class="form-control input-inline input-sm wb100 codeInputs" id="fbarCodess" name="fbarCodeName" placeholder="28603234283069" type="text"></td><td><input class="form-control input-inline input-sm wb100" id="fbarName" name="fbarName-' +index+ '"  placeholder="贝因美3段奶粉" type="text"></td><td><a class="layer_button dele" onclick="javascript:;">删行</a></td></tr>' ,
        tbody=$( this ).parents( "table" ).find( "tbody" );
    tbody.append(dom);
    index++;
    $( "#indexFlag" ).val(index);
    $( ".dele" ).on( "click" , function (){
         var  trSeq = $( this ).parent().parent().prevAll().length + 1; //获得当前行号
        console.log(trSeq)
         var  onum = $( "#oldnum" ).val();
        $( "#oldnum" ).val(onum-1);
         var  onumn = $( "#oldnum" ).val();
        goodsList.splice(trSeq-1,1); //从goodsList中第trSeq个元素开始，移除1个元素
         var  self=$( this ),
            parent=self.parents( 'tr' );
        parent.remove();
    });
})   



备注：这种方式会有bug,如果 table里面有input隐藏域，则会把隐藏域元素也算上。
解决方法，将input隐藏域放到table外面。
</ table > < input   type = "hidden"   id = "temSkuId"   />   

```
##动态添加、删除一行（新增与修改引用同一js）
```
1.页面：
修改页面：
< input   type = "hidden"   id = "oldnum"   value = " ${goodsList?size} " />
< input   type = "hidden"   id = "indexMod"   value = " ${goodsList?size } " /> < input   type = "hidden"   id = "addOrMod"   value = "1"   />
新增页面：
< input   type = "hidden"   id = "indexAdd"   value = "3" /> < input   type = "hidden"   id = "addOrMod"   value = "0"   />   
2.js:

$( ".dele" ).on( "click" , function (){
     var  flag = $( "#addOrMod" ).val();
     if (flag== "1" ){
         var  index = $( "#indexMod" ).val();
        index--;
        $( "#indexMod" ).val(index);
         var  trSeq = $( this ).parent().parent().prevAll().length + 1;
        console.log(trSeq)
         var  onum = $( "#oldnum" ).val();
        $( "#oldnum" ).val(onum-1);
         var  onumn = $( "#oldnum" ).val();
        goodsList.splice(trSeq-1,1);
        barCodeList.splice(trSeq-1,1);
    } else {
         var  index = $( "#indexAdd" ).val();
        index--;
        $( "#indexAdd" ).val(index);
         var  trSeq = $( this ).parent().parent().prevAll().length + 1;
        console.log(trSeq)
         var  onum = $( "#oldnum" ).val();
        $( "#oldnum" ).val(onum-1);
         var  onumn = $( "#oldnum" ).val();
        goodsList.splice(trSeq-1,1);
        barCodeList.splice(trSeq-1,1);
    }
     var  self=$( this ),
        parent=self.parents( 'tr' );
    parent.remove();
    
    
});
$( ".new_rows" ).on( "click" , function (){
     var  flag = $( "#addOrMod" ).val();
     if (flag== "1" ){
         var  index = $( "#indexMod" ).val();
        index++;
        $( "#indexMod" ).val(index);
    } else {
         var  index = $( "#indexAdd" ).val();
        index++;
        $( "#indexAdd" ).val(index);
    }
    
     var  dom= '<tr><td><input class="form-control input-inline input-sm wb100 codeInputs" id="fbarCodess" name="fbarCodeName" placeholder="28603234283069" type="text"></td><td><input class="form-control input-inline input-sm wb100" id="fbarName" name="fbarName"  placeholder="贝因美3段奶粉" type="text"></td><td><a class="layer_button dele" onclick="javascript:;">删行</a></td></tr>' ,
        tbody=$( this ).parents( "table" ).find( "tbody" );
    tbody.append(dom);
    
    $( ".dele" ).on( "click" , function (){
         var  flag = $( "#addOrMod" ).val();
         if (flag== "1" ){
             var  index = $( "#indexMod" ).val();
            index--;
            $( "#indexMod" ).val(index);
             var  trSeq = $( this ).parent().parent().prevAll().length + 1;
            console.log(trSeq)
             var  onum = $( "#oldnum" ).val();
            $( "#oldnum" ).val(onum-1);
             var  onumn = $( "#oldnum" ).val();
            goodsList.splice(trSeq-1,1);
            barCodeList.splice(trSeq-1,1);
        } else {
             var  index = $( "#indexAdd" ).val();
            index--;
            $( "#indexAdd" ).val(index);
             var  trSeq = $( this ).parent().parent().prevAll().length + 1;
            console.log(trSeq)
             var  onum = $( "#oldnum" ).val();
            $( "#oldnum" ).val(onum-1);
             var  onumn = $( "#oldnum" ).val();
            goodsList.splice(trSeq-1,1);
            barCodeList.splice(trSeq-1,1);
        }
         var  self=$( this ),
            parent=self.parents( 'tr' );
        parent.remove();
    });
})   
```
##动态增加table行数，然后获得总行数
```

1、$("#table_Id").children("tr").length;

       只能获得静态页面table 行数。

2、$("#table_Id tr").length;

       可以获得表格在动态增减行数后的行数。
```
##jquery获取当前行号实例1
```

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=gb2312" />


    <script type="text/javascript" src="jquery.min.js"></script>


    <script type="text/javascript" language="javascript">
        $(document).ready(function() {
            $("#MyTable").find("td").hover(function() {
var hang = $(this).parent().prevAll().length + 1;
var lie = $(this).prevAll().length + 1;
                $(":input").val("第" + hang + "行，第" + lie + "列");
            })
        })   
</script>


    <title>JQuery</title>
    <style type="text/css">
        table{
            background: #FFCC99;
}
        td{
            text-align: center;
            width: 100px;
            height: 30px;
}
</style>
</head>
<body>
    <table id="MyTable" border="1" cellpadding="2" cellspacing="0">
        <tr>
            <td>
                (1，1)
            </td>
            <td>
                (1，2)
            </td>
            <td>
                (1，3)
            </td>
        </tr>
        <tr>
            <td>
                (2，1)
            </td>
            <td>
                (2，2)
            </td>
            <td>
                (2，3)
            </td>
        </tr>
        <tr>
            <td>
                (3，1)
            </td>
            <td>
                (3，2)
            </td>
            <td>
                (3，3)
            </td>
        </tr>
    </table>
    <br />
    <input type="text" />
</body>
</html>
```
##jquery获取当前行号实例2
```
<html>
<script type="text/javascript">
</script>
<body>
<script src="jquery.min.js" type="text/javascript"></script>
<div>
    <table>
        <tr>
            <td>xxxx</td>
            <td>xxxx</td>
            <td>xxxx</td>
        </tr>
        <tr>
            <td>xxxx</td>
            <td>xxxx</td>
            <td>xxxx</td>
        </tr>
        <tr>
            <td>xxxx</td>
            <td>xxxx</td>
            <td>xxxx</td>
        </tr>
        <tr>
            <td>xxxx</td>
            <td>xxxx</td>
            <td>xxxx</td>
        </tr>
    </table>
</div>
<input type="button" onclick="trTotal()" value="trTotal">
<script type="text/javascript">
$(function(){
//点击哪行 显示哪行的行数
var myRows = $('table tbody tr').click(function(){
alert(myRows.index(this));


});
})
function trTotal(){  
//获取总行数
alert($("table tbody tr").length);
}  
</script>
</body>
</html>
```
##jquery获取当前行号
```

 <table id="bcTable" class="table table-striped table-bordered table-hover dataTable ta_c">
                                                        <thead>
                                                          <tr>
                                                            <th class="ta_c">条码</th>
                                                            <th class="ta_c">商品名称</th>
                                                            <th class="ta_c"><a class="layer_button new_rows" onclick="javascript:;">增行</a></th>
                                                          </tr>
                                                        </thead>               <tbody id="bcTbody"> 
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
                                                           <!-- 
                                                           <input type='hidden' id='skuId${good_index}' value="${good.skuId}" />
                                                           <input type='hidden' id='spuId${good_index}' value="${good.spuId}" />
                                                            -->
                                                           </#list>
                                                           </#if>
                                                        </tbody>
                                                    </table>
<input type="hidden" id="operowno" /> 


$("#bcTable").find("a:gt(0)").hover(function() {//this指当前行的删除a标签对象
       var hang = $(this).parent().parent().prevAll().length;
       $("#operowno").val(hang); }) 

//商品条码
$('.codeInputs').bind('input propertychange', function() {//this指当前行的商品条码的td标签对象
    var ff = $(this).parent().parent();
    var td1 = ff.find("td").eq(0).find("input").val()
    var hang = ff.index();
    if(td1==""){//条码为空，自动清空名称
        ff.find("td").eq(1).find("input").val('');
        dbList.remove(hang);
    } });  

```
##jquery获取当前行的第一个Td
```
<tr class="trTemp"><td><input class="form-control input-inline input-sm wb100 codeInputs" id="fbarCodess" name="fbarCodeName" placeholder="28603234283069" type="text"></td><td><input class="form-control input-inline input-sm wb100" id="fbarName" name="barName"  placeholder="贝因美3段奶粉" type="text"></td><td><a class="layer_button dele" onclick="javascript:;">删行</a></td></tr> 


当td中的值发生变化时：
$('.wb100').bind('input propertychange', function() {  //监听td的值发生变化
    var trSeq = $(this).parent().parent().index();//获取当前行号

    var ff = $(this).parent().parent();//获取当前行tr
    console.log(ff.find("td:first"))//找到第一个td
    console.log(ff.find("td:first").find("input").val())//获取当前行第一个Td中的input的value值
    console.log(ff.find("td").eq(1).find("input").val())  //获取当前行第二个Td中的input的value值

    console.log($(".a").find("td").eq(1).find("input").val()); //获取当前行第二个Td中的input的value值【第二种方式】
});  

```
##jquery实现键盘确定的事件
```
$(function() {
    document.onkeydown = function(e) {
        var ev = document.all ? window.event : e;
        if (ev.keyCode == 13) {
            userLogin();
        }
    } }) 


// 登录
function userLogin() {
    if (checkUserName() == false || checkpw() == false) {
        return false;
    }
    $("#login").attr("disabled", "disabled");
    /*$("#login").text("登录中...");*/
    var userName = $("#userName").val();
    var pw = $("#pw").val();
    var param = new Object();
    param.mobile = userName;
    param.pw = pw;
    var paramJson = encodeURI(JSON.stringify(param));
    var url = basePath + '/loginController/login.do?paramJson=' + paramJson;
    $.ajax({
        type : "post",
        url : url,
        contentType : "application/json;charset=utf-8",
        dataType : "json",
        success : function(data) { 
            $("#login").removeAttr("disabled");
            if (data.status== "1") {
                //登录成功后
                location.href = basePath + "/loginController/findSupplyRel.do?loginFlag=1";
            }
            else
            {
                errorInfo(data.errorMsg);
            }
            /*$("#login").text("立即登录");*/
        }
    });
} 

```
##jquery  点击当前行判断是第几行、第几列
```
$(document).ready(function() {

             $("#MyTable").find("td").hover(function() {

 var hang = $(this).parent().prevAll().length + 1;

 var lie = $(this).prevAll().length + 1;

                $(":input").val("第" + hang + "行，第" + lie + "列");

           })

        })   

来源：  http://www.cnblogs.com/zcttxs/archive/2012/03/29/2423677.html
JQuery获取点击一个td是该行的第几个td

$(this).par
en
ts("
tr
").find("td").index($(this));


实例：
<table>
<thead>

<tr>
<th class="ta_c">条码</th>
<th class="ta_c">商品名称</th>
<th class="ta_c"><a class="layer_button new_rows" onclick="javascript:;">增行</a></th> </tr>  

var trSeq = $(this).parent().parent().prevAll().length + 1;  

var tdSeq = $(this).parent().prevAll().length + 1;  

</thead>
</table>
```
##jquery 获取 table总行数

```
动态增加table行数，然后获得总行数，分别使用了两种方法，结果不同：：
1、$("#table_Id").children("tr").length;
     只能获得静态页面table 行数。
2、$("#table_Id tr").length;
或
var tag = $('input[name="disType"]:checked').val();
var len = $("#skuTable"+tag+" tbody tr").length;
    可以获得表格在动态增减行数后的行数。
```
##jquery 动态添加 th
```
table结构：
<table id="sample_6">
 <thead>
<tr>
     <th> 站点 </th>
</tr>
</thead>
</tr> 
//增加th
var  obj = document.createElement( "th" );
obj.innerHTML =  "组合码明细" ;
$( "#sample_6 thead tr" ).append(obj);   
```
##jquery遍历table的tr获取td的值
```
<tbody id="history_income_list">
<tr>
<td align="center"><input type="text" class="input-s input-w input-hs"></td>
<td align="center"><input type="text" class="input-s input-w input-hs"></td>
<td align="center"><input type="text" class="input-s input-w input-hs"></td>
<td align="center"><a class="" onclick="history_income_del(this);" href="###">删除</a></td>
</tr>
<tr>
<td align="center"><input type="text" class="input-s input-w input-hs"></td>
<td align="center"><input type="text" class="input-s input-w input-hs"></td>
<td align="center"><input type="text" class="input-s input-w input-hs"></td>
<td align="center"><a class="" href="###">删除</a></td>
</tr>
</tbody>
方式一：（推荐）
var trList = $("#history_income_list").children("tr")
    for (var i=0;i<trList.length;i++) {
        var tdArr = trList.eq(i).find("td");
        var history_income_type = tdArr.eq(0).find('input').val();//收入类别
        var history_income_money = tdArr.eq(1).find('input').val();//收入金额
        var history_income_remark = tdArr.eq(2).find('input').val();//    备注
        
        alert(history_income_type);
        alert(history_income_money);
        alert(history_income_remark);
    }
方式二：（缺点，会将行标题也算进来）
$("#history_income_list").find("tr").each(function(){
        var tdArr = $(this).children();
        var history_income_type = tdArr.eq(0).find('input').val();//收入类别
        var history_income_money = tdArr.eq(1).find('input').val();//收入金额
        var history_income_remark = tdArr.eq(2).find('input').val();//    备注
        
        alert(history_income_type);
        alert(history_income_money);
        alert(history_income_remark);
        
        
    });

function getOffCategory(id){
        var cate = [];
        var trList = $("#"+id)
        $("#"+id).find("tr").each(function(){
            var tdArr = $(this).children();
            var cate_code = tdArr.eq(0).find('input').val();//品类编码
            cate.push(cate_code);
        });
        return cate;
    }
    
function getOffCategory2(id){
        var cate = [];
        var trList = $("#"+id).find("tr");
        for(var i=1; i<trList.length; i++){
             var tdArr = trList.eq(i).find("td");
             var cate_code = tdArr.eq(0).find('input').val();//品类编码
             cate.push(cate_code);
        }
        return cate;
    }
```
##jquery.fixedtableheader.min.js固定表头功能
<script src="../Scripts/jquery.fixedtableheader.min.js" type="text/javascript"></script>
<script type="text/javascript">
    $(function () {
        $("#tbl1").fixedtableheader();
    });
</script>
##jquery 设置 table的内容
var th = "<td>供应商承担比例</td>";
var tc = "<td><input style='width:105px;' /></td>";
var len = $("#dataTableDetail_on tbody tr:first").find("td").length;
if(compensateType == 1){
    if(len < 4){
        $("#dataTableDetail_on tbody tr:first").find('td:eq(1)').after(th);//第一行
        $("#dataTableDetail_on tbody tr:not(:first)").find('td:eq(1)').after(tc);//非第一行
        $("#dataTableDetail_off tbody tr:first").find('td:eq(1)').after(th);
        $("#dataTableDetail_off tbody tr:not(:first)").find('td:eq(1)').after(tc);
    }
}