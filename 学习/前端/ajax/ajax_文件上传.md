##$.ajaxFileUpload errorThrown message: "Unexpected token <"
```
原因是 结果返回的json数据如猜测，json数据被包含在一个<pre></pre>的标签中
$.ajax FileUpload({ 


                        url:url,
                        secureuri: false ,
                        fileElementId: 'excel' ,
                        dataType:  'json' ,//这里可以先设为"content" 回调函数就可以执行到success了.
                        success:  function  (data){
                             if (data.resultCode ==  "1" ){
                                $( "#excel" ).val( "" );
                                $( "#cancel" ).click();
                                $( "#query" ).click();
                                alert( "导入成功" );
                            }
                             else {
                                $( "#autocomplete" ).click();
                                alert( "导入失败,"  + data.msg);
                            }
                        },
                        error:  function (XMLHttpRequest, textStatus, errorThrown) {
                            alert(XMLHttpRequest.status);
                            alert(XMLHttpRequest.readyState);
                            alert(textStatus);
                        },
                        complete:  function (XMLHttpRequest, textStatus) {
                             this ;  // 调用本次AJAX请求时传递的options参数
                        }                     });   





查了下原因，是因为Server端的Response上加上了contentType="application/json"。但有时后端这么做是必须的。

方法一：修改 源码

所以修改ajaxFileUpload源码，将<pre></pre>标签去掉，如下：


 uploadHttpData:  function ( r, type ) {
         var  data = !type;
        data = type ==  "xml"  || data ? r.responseXML : r.responseText;
         // If the type is "script", eval it in global context
         if  ( type ==  "script"  )
            jQuery.globalEval( data );
         // Get the JavaScript object, if JSON is used.
         if  ( type ==  "json"  )
                 //从这里开始是新增内容

            data = r.responseText;
             var  start = data.indexOf( ">" );
             if  (start != -1) {
                 var  end = data.indexOf( "<" , start + 1);
                 if  (end != -1) {
                    data = data.substring(start + 1, end);
                }
            }
                //新增内容结束

            eval(  "data = "  + data );
         // evaluate scripts within html
         if  ( type ==  "html"  )
            jQuery( "<div>" ).html(data).evalScripts();
         return  data;     },   


至此，大工告成，ajaxFileUpload的dataType正常使用json。


方法二：

或者是在返回的“content”类型数据后 得到 JSON 数据。







导入实例：

1.页面

< a   href = "#"   onclick = "toImport('0');"   id = "autocomplete"   data-target = "#myModal_autocomplete"   role = "button"   class = "btn red"   data-toggle = "modal"   > 导入 </ a >  
< div   id = "myModal_autocomplete"   class = "modal fade"   role = "dialog"   aria-hidden = "true" >
             < div   class = "modal-dialog" >
                 < div   class = "modal-content" >
                     < div   class = "modal-header" >
                         < button   type = "button"   class = "close"   data-dismiss = "modal"   aria-hidden = "true" ></ button >
                         < h4   class = "modal-title" > 多价导入 </ h4 >
                     </ div >
                     < div   class = "modal-body form" >
                         < div   class = "form-group" >
                             < br />
                             < div   class = "row" >
                                 < div   class = "col-md-12" >
                                     < div   class = "form-group" >
                                         < label   class = "control-label col-md-3" > 导入模版: </ label >
                                         < div   class = "col-md-4" >
                                             < a   href = " ${ctx} /template/multiPrice.xlsx" > 多价批量导入模版.xlsx </ a >
                                         </ div >
                                     </ div >
                                 </ div >
                             </ div >
                             < br />
                             < div   class = "row" >
                                 < div   class = "col-md-12" >
                                     < div   class = "form-group" >
                                             < label   class = "control-label col-md-3" > 导入文件: </ label >
                                         < div   class = "col-md-4" >
                                             < input   type = "file"   class = "col-md-4 form-control"   id = "excel"   name = "excel" >
                                         </ div >
                                     </ div >
                                 </ div >
                             </ div >
                         </ div >
                     </ div >
                     < div   class = "modal-footer" >
                         < button   type = "button"   class = "btn btn-default"   id = "confirm" > 确认 </ button >
                         < button   type = "button"   class = "btn btn-default"   data-dismiss = "modal"   id = "cancel" > 取消 </ button >
                     </ div >
                 </ div >
             </ div >

         </ div >




2.js
function  toImport(id){
    $( "#fileIdHid" ).val(id);
    console.log( 'fileId:' +id);

}
$( "#confirm" ).click( function (){
         var  excel = $( "#excel" ).val(); 
         if ( ""  == excel){
            alert( "请选择要上传的多价EXCEL" );
             return ;
        }
        
         if (!endWith( ".xlsx" ,excel)){
            alert( "只支持导入EXCEL2007及以上版本" );
             return ;
        }
        
         //var siteUserName = getCookie("siteUserName");
         //var userName = window.Base64.decode(siteUserName); 
        
         var  userName =  "Frank" ;
        
         var  fileId = $( "#fileIdHid" ).val();
         var  url =  '${ctx}/ocanal/multiRuleImport.do?createPin='  + encodeURI(encodeURI(userName))+ "&fileId=" +fileId;
        
         //用户登录状态失效判断
         /*var siteUserId = getCookie("siteUserId");
        var siteToken = getCookie("siteToken");
        $.ajax({
            url:'http://test.site.haiziwang.com/sso-web/checkuser/checkUserLogin.do',
            data:{"appCode":"PRO","siteUserId":siteUserId,"siteToken":siteToken},
            dataType:'json',
            cache:false,
            type:'post',
            async:false,
            success:function(data)
            {
                if(!data.success)
                {
                    alert("请重新登录");
                    parent.location.href="http://test.site.haiziwang.com";
                }
                else
                {
                    $.blockUI({ message: '<h1><img src="../images/loading.gif" />正在导入......</h1>' });
                    $("#cancel").click();*/
                    $.ajaxFileUpload({ 
                        url:url,
                        secureuri: false ,
                        fileElementId: 'excel' ,
                        dataType:  'json' ,
                        success:  function  (data){
                             if (data.resultCode ==  "1" ){
                                $( "#excel" ).val( "" );
                                $( "#cancel" ).click();
                                $( "#query" ).click();
                                alert( "导入成功" );
                            }
                             else {
                                $( "#autocomplete" ).click();
                                alert( "导入失败,"  + data.msg);
                            }
                        },
                        error:  function (XMLHttpRequest, textStatus, errorThrown) {
                            alert(XMLHttpRequest.status);
                            alert(XMLHttpRequest.readyState);
                            alert(textStatus);
                        },
                        complete:  function (XMLHttpRequest, textStatus) {
                             this ;  // 调用本次AJAX请求时传递的options参数
                        }
                    });
                  /*}
            }
        });*/
    });
    
    $( "#cancel" ).click( function (){
        $( "#excle" ).val( "" );

    });




3.controller
     /**
     * 多价导入
     */
     public   void  multiRuleImport() {
        RespJson  resjon  =  new  RespJson();
         logger .info( "导入多价开始........." );
        String  createPin  = NormalUtil. decodeToString (NormalUtil. decodeToString (getParam( "createPin" )));
        String  fileId  = getParam( "fileId" );
        Map<String, Object>  param  =  new  HashMap<String, Object>();
         param .put( "createPin" ,  createPin );
         try  {
            importSales( this .getReq(),  param ,  fileId );
             resjon .setResultCode( "1" );
            renderJson(JSON. toJSONString ( resjon ));
        }  catch  ( final  Exception  e1 ) {
             logger .error( "导入多价出错："  +  e1 .toString());
             resjon .setResultCode( "0" );
             resjon .setMsg( "Import Error" );
            renderJson(JSON. toJSONString ( resjon ));
             e1 .printStackTrace();
        }


    }  



private   void  importSales(HttpServletRequest  request , Map<String, Object>  param , String  fileId )
             throws  ParseException {
         // 文件上传
        List<String>  retLs  = RequestUtil. uploadFile ( request );
         // 文件上传根路径
        String  savePath  =  request .getSession().getServletContext().getRealPath( "/WEB-INF/upload/" );
         // 获取上传文件名称
        String  fileBasic  =  "" ;
         // SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        List<List<List<Object>>>  sheetList  =  null ;
         if  ( null  !=  retLs  &&  retLs .size() > 0) {
             try  {
                String  fileName  =  retLs .get(0);
                 if  ( retLs .get(0).split( "," ). length  > 1) {
                     fileName  =  retLs .get(0).split( "," )[0];
                }
                 fileBasic  =  savePath  +  "/"  +  fileName ;
                String  extension  =  fileName .lastIndexOf( "." ) == -1 ?  ""
                        :  fileName .substring( fileName .lastIndexOf( "." ) + 1);
                 // //2003版本excel时间格式解析格式为2015/11/24
                 // if ("xls".equals(extension)) {
                 // sdf = new SimpleDateFormat("yyyy/MM/dd");
                 // //2007版本excel时间格式解析格式为2015-11-24
                 // }else{
                 // sdf = new SimpleDateFormat("yyyy-MM-dd");
                 // }
                File  excel  =  new  File( fileBasic );
                 sheetList  = MultiPriceExcelUtil. readExcel ( excel ,  "multiprice" );
                 boolean   checked  =  false ;
                 // 校验Excel文件
                 checked  = checkMultisheet( sheetList );
                 fileName  = System. currentTimeMillis () +  "."  +  extension ;
                 // 上传文件到ftp服务器
                String  filePath  = MultiPriceExcelUtil. doExport ( savePath ,  sheetList ,  fileName );
                 // PropertiesUtil.get("StockSerIp"),
                FtpClientUtil. upLoadFromDisk (PropertiesUtil. get ( "ftp.hostname" ),
                        Integer. parseInt (PropertiesUtil. get ( "ftp.port" )), PropertiesUtil. get ( "ftp.username" ),
                        PropertiesUtil. get ( "ftp.password" ), PropertiesUtil. get ( "ftp.path" ),  fileName ,  filePath );
                 new  File( filePath ).delete();
                ISalesCheckService  checkService  = RuleServiceFactory. getRuleService (ISalesCheckService. class );
                ISalesService  svc  = RuleServiceFactory. getRuleService (ISalesService. class );
                 if  (SaleCenterConstants. ZERO .equals( fileId )) { // 新增
                     // 插入校验表
                    SaleCheckEntity  checkEntity  =  new  SaleCheckEntity();
                     checkEntity .setCreatePin( param .get( "createPin" ).toString()); // 创建人
                     checkEntity .setStatus( checked  ==  true  ? Integer. parseInt (MultiPriceEnum. CHECK_ONE .getValue())
                            : Integer. parseInt (MultiPriceEnum. CHECK_TWO .getValue())); // 验证是否通过
                     checkEntity .setFileName( retLs .get(0).split( "," )[1]);
                     checkEntity .setFilePath(PropertiesUtil. get ( "ftp.path" ) +  "/"  +  fileName );
                     int   check_id  =  checkService .insertSalesCheck( checkEntity );
                     if  ( check_id  > 0) {
                         logger .info( "插入校验表:"  +  checkEntity .getId());
                    }
                     // 插入审核表
                     param .put( "check_id" ,  checkEntity .getId());
                     param .put( "status" , MultiPriceEnum. VERIFY_NONE .getValue());
                     param .put( "fileName" ,  retLs .get(0).split( "," )[1]);
                     param .put( "filePath" , PropertiesUtil. get ( "ftp.path" ) +  "/"  +  fileName );
                     int   log_id  =  svc .insertSales( param );
                     if  ( log_id  > 0) {
                         logger .info( "插入审核表:"  +  checkEntity .getId());
                    }
                }  else  { // 更新
                     // 更新校验表
                     param .put( "check_id" ,  fileId );
                    Map<String, Object>  salesLog  =  svc .selectSalesDetail( param );
                    SaleCheckEntity  checkEntity  =  new  SaleCheckEntity();
                     checkEntity .setId(Integer. parseInt (String. valueOf ( salesLog .get( "check_id" ))));
                     checkEntity .setStatus( checked  ==  true  ? Integer. parseInt (MultiPriceEnum. CHECK_ONE .getValue())
                            : Integer. parseInt (MultiPriceEnum. CHECK_TWO .getValue())); // 验证是否通过
                     checkEntity .setFileName( retLs .get(0).split( "," )[1]);
                     checkEntity .setFilePath(PropertiesUtil. get ( "ftp.path" ) +  "/"  +  fileName );
                     checkEntity .setUpdatePin( param .get( "createPin" ).toString()); // 更新人
                     int   check_update  =  checkService .updateSalesCheck( checkEntity );
                     if  ( check_update  > 0) {
                         logger .info( "更新校验表成功:"  +  checkEntity .getId());
                    }
                     // 更新审核表
                    Map<String, Object>  paramUpdate  =  new  HashMap<String, Object>();
                     paramUpdate .put( "check_id" ,  fileId );
                     paramUpdate .put( "status" , MultiPriceEnum. VERIFY_NONE .getValue());
                     paramUpdate .put( "fileName" ,  retLs .get(0).split( "," )[1]);
                     paramUpdate .put( "filePath" , PropertiesUtil. get ( "ftp.path" ) +  "/"  +  fileName );
                     // paramUpdate.put("updatePin",
                     // param.get("createPin").toString());
                     int   log_update  =  svc .updateSales( paramUpdate );
                     if  ( log_update  > 0) {
                         logger .info( "更新审核表成功:"  +  checkEntity .getId());
                    }
                }
            }  catch  (IOException  e ) {
                 e .printStackTrace();
                 logger .error( "多价导入失败" ,  e );
            }
        }     }  


/**
 * 获取request流工具类
 * 
 *  @Description :
 *  @Author :ojj
 *  @Since :2015年11月11日
 *  @Version :1.1.0
 */
public   final   class  RequestUtil {
     private   static   final  Logger  log  = Logger. getLogger (RequestUtil. class );
     /**
     * 字符集编码格式
     */
     private   final   static  String  CODE  =  "UTF-8" ;
     /**
     * 获取post请求内容
     */
     public   static  String getReqContent(HttpServletRequest  request ) {
        StringBuilder  sb  =  new  StringBuilder();
         try  {
            BufferedReader  br ;
             br  =  new  BufferedReader( new  InputStreamReader( request .getInputStream(),  CODE ));
            String  line  =  null ;
             while  (( line  =  br .readLine()) !=  null ) {
                 sb .append( line );
            }
        }  catch  (IOException  e ) {
             e .printStackTrace();
             log .error( "读取流失败!" );
        }
         return   sb .toString();
    }
     /**
     * 文件上传
     */
     public   static  List<String> uploadFile(HttpServletRequest  request ) {
        List<String>  reLs  =  null ; 
        String  savePath  =  request .getSession().getServletContext().getRealPath( "/WEB-INF/upload" );
        File  file  =  new  File( savePath );
         // 判断上传文件的保存目录是否存在
         if  (! file .exists() && ! file .isDirectory()) {
             log .info( savePath  +  "目录不存在，需要创建" );
             // 创建目录
             file .mkdir();
        }
         try  {
             // 使用Apache文件上传组件处理文件上传步骤：
             // 1、创建一个DiskFileItemFactory工厂
            DiskFileItemFactory  factory  =  new  DiskFileItemFactory();
             // 2、创建一个文件上传解析器
            ServletFileUpload  upload  =  new  ServletFileUpload( factory );
             // 解决上传文件名的中文乱码
             upload .setHeaderEncoding( CODE );
             // 3、判断提交上来的数据是否是上传表单的数据
             if  (!ServletFileUpload. isMultipartContent ( request )) {
                 // 按照传统方式获取数据
                 return   reLs ;
            }
             // 4、使用ServletFileUpload解析器解析上传数据，解析结果返回的是一个List<FileItem>集合，每一个FileItem对应一个Form表单的输入项
             reLs  =   new  ArrayList<String>();
             @SuppressWarnings ( "unchecked" )
            List<FileItem>  list  =  upload .parseRequest( request );
             for  (FileItem  item  :  list ) {
                String  oldFileName  =  "" ;
                 // 如果fileitem中封装的是普通输入项的数据
                 if  ( item .isFormField()) {
                    String  name  =  item .getFieldName();
                    String  value  =  item .getString( CODE );
                     log .info( name  +  ":"  +  value );
                } 
                 // 如果fileitem中封装的是上传文件
                 else  
                 {
                     // 得到上传的文件名称，
                    String  filename  =  item .getName();
                     if  ( filename  ==  null  ||  filename .trim().equals( "" )) {
                         continue ;
                    }
                     /**
                     * 注意：不同的浏览器提交的文件名是不一样的，有些浏览器提交上来的文件名是带有路径的，如：
                     * c:\a\b\1.txt，而有些只是单纯的文件名，如：1.txt
                     * 处理获取到的上传文件的文件名的路径部分，只保留文件名部分
                     */
                     filename  =  filename .substring( filename .lastIndexOf( "\\" ) + 1);
                     // 获取item中的上传文件的输入流
                    InputStream  in  =  item .getInputStream();
                     // 创建一个文件输出流
                     log .info( "文件创建的目录内------------------------------------------" );
                    String  extension  =  filename .lastIndexOf( "." ) == -1 ?  ""  :  filename
                            .substring( filename .lastIndexOf( "." ) + 1);
                     oldFileName  =  filename ;
                     filename  = System. currentTimeMillis ()+ "."  +  extension ;
                     log .info( savePath  +  "/"  +  filename );
                    FileOutputStream  out  =  new  FileOutputStream( savePath  +  "/"  +  filename );
                     // 创建一个缓冲区
                     byte   buffer [] =  new   byte [1024];
                     // 判断输入流中的数据是否已经读完的标识
                     int   len  = 0;
                     // 循环将输入流读入到缓冲区当中，(len=in.read(buffer))>0就表示in里面还有数据
                     while  (( len  =  in .read( buffer )) > 0) {
                         // 使用FileOutputStream输出流将缓冲区的数据写入到指定的目录(savePath + "\\"
                         // + filename)当中
                         out .write( buffer , 0,  len );
                    }
                     // 关闭输入流
                     in .close();
                     // 关闭输出流
                     out .close();
                     // 删除处理文件上传时生成的临时文件
                     item .delete();
                     reLs .add( filename  +  ","  +  oldFileName );
                }
            }
        }  catch  (Exception  e ) {
             e .printStackTrace();
             log .error( "文件上传失败!" ,  e );
        }  finally {
             log .info( "文件上传成功!" );
        }
         return   reLs ;
    }
}


package com.haiziwang.jplugin.study.Util;


import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.text.DecimalFormat;
import java.text.SimpleDateFormat;
import java.util.LinkedList;
import java.util.List;


import org.apache.poi.hssf.usermodel.HSSFCell;
import org.apache.poi.hssf.usermodel.HSSFDataFormat;
import org.apache.poi.hssf.usermodel.HSSFDateUtil;
import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.hssf.util.Region;
import org.apache.poi.xssf.usermodel.XSSFCell;
import org.apache.poi.xssf.usermodel.XSSFCellStyle;
import org.apache.poi.xssf.usermodel.XSSFFont;
import org.apache.poi.xssf.usermodel.XSSFRow;
import org.apache.poi.xssf.usermodel.XSSFSheet;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;


/**
 * Excel 2003、2007数据解析工具类
 */


public final class MultiPriceExcelUtil {


/**
* 对外提供读取excel 的方法 目前多价导入专用
* 
* @param action
*/
public static List<List<List<Object>>> readExcel(File file, String type) throws IOException {
String fileName = file.getName();
String extension = fileName.lastIndexOf(".") == -1 ? "" : fileName.substring(fileName.lastIndexOf(".") + 1);
if ("xls".equals(extension)) {
return read2003Excel(file);
} else if ("xlsx".equals(extension)) {
if ("multiprice".equals(type)) {
return read2007Excel(file);
} else if ("pmsingle".equals(type)) {
return read2007ExcelPmSingle(file);
} else {
throw new IOException("不支持的导入类型");
}
} else {
throw new IOException("不支持的文件类型");
}
}


/**
* 读取 office 2003 excel
* 
* @throws IOException
* @throws FileNotFoundException
*/
private static List<List<List<Object>>> read2003Excel(File file) throws IOException {
List<List<List<Object>>> res = new LinkedList<List<List<Object>>>();
HSSFWorkbook hwb = new HSSFWorkbook(new FileInputStream(file));


HSSFSheet sheet = null;
for (int i = 0; i < hwb.getNumberOfSheets(); i++) {// 获取每个Sheet表
List<List<Object>> list = new LinkedList<List<Object>>();
sheet = hwb.getSheetAt(i);


Object value = null;
HSSFRow row = null;
HSSFCell cell = null;
for (int j = sheet.getFirstRowNum(); j <= sheet.getPhysicalNumberOfRows(); j++) {
row = sheet.getRow(j);
if (row == null) {
continue;
}
List<Object> linked = new LinkedList<Object>();
for (int k = row.getFirstCellNum(); k <= row.getLastCellNum(); k++) {
cell = row.getCell(k);
if (cell == null) {
continue;
}
DecimalFormat df = new DecimalFormat("0");// 格式化 number
// String
// 字符
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");// 格式化日期字符串
DecimalFormat nf = new DecimalFormat("0.00");// 格式化数字
switch (cell.getCellType()) {
case XSSFCell.CELL_TYPE_STRING:
// System.out.println(i + "行" + j + " 列 is String
// type");
value = cell.getStringCellValue();
break;
case XSSFCell.CELL_TYPE_NUMERIC:
// System.out.println(i + "行" + j
// + " 列 is Number type ; DateFormt:"
// + cell.getCellStyle().getDataFormatString());
if ("@".equals(cell.getCellStyle().getDataFormatString())) {
value = df.format(cell.getNumericCellValue());


} else if ("General".equals(cell.getCellStyle().getDataFormatString())) {
value = nf.format(cell.getNumericCellValue());
} else {
value = sdf.format(HSSFDateUtil.getJavaDate(cell.getNumericCellValue()));
}
break;
case XSSFCell.CELL_TYPE_BOOLEAN:
// System.out.println(i + "行" + j + " 列 is Boolean
// type");
value = cell.getBooleanCellValue();
break;
case XSSFCell.CELL_TYPE_BLANK:
// System.out.println(i + "行" + j + " 列 is Blank type");
value = "";
break;
default:
// System.out.println(i + "行" + j + " 列 is default
// type");
value = cell.toString();
}
if (value == null || "".equals(value)) {
continue;
}
linked.add(value);
}
list.add(linked);
}
res.add(list);
}


return res;
}


/**
* 读取Office 2007 excel
* 
*/


private static List<List<List<Object>>> read2007Excel(File file) throws IOException {


List<List<List<Object>>> res = new LinkedList<List<List<Object>>>();
// 构造 XSSFWorkbook 对象，strPath 传入文件路径
XSSFWorkbook xwb = new XSSFWorkbook(new FileInputStream(file));
XSSFSheet sheet = null;
int firstCellNum;
int lastCellNum;
for (int i = 0; i < xwb.getNumberOfSheets(); i++) {// 获取每个Sheet表
List<List<Object>> list = new LinkedList<List<Object>>();
sheet = xwb.getSheetAt(i);
if (i == 2) {
break;
}
if (i == 0) {
firstCellNum = 0;
lastCellNum = Integer.parseInt(MultiPriceEnum.IMPORTRULECOUNT.getValue());
list = proFirstSheet(sheet, firstCellNum, lastCellNum);
} else if (i == 1) {
firstCellNum = 0;
lastCellNum = Integer.parseInt(MultiPriceEnum.IMPORTCOMPCOUNT.getValue());
list = proSecondSheet(sheet, firstCellNum, lastCellNum);
}
res.add(list);
}


file.delete();
return res;
}


private static List<List<Object>> proSecondSheet(XSSFSheet sheet, int firstCellNum, int lastCellNum) {
List<List<Object>> list = new LinkedList<List<Object>>();
Object value;
XSSFRow row;
XSSFCell cell;
for (int i = sheet.getFirstRowNum(); i <= sheet.getPhysicalNumberOfRows(); i++) {
row = sheet.getRow(i);
if (row == null) {
continue;
}


List<Object> linked = new LinkedList<Object>();
int k = 0;
for (int j = firstCellNum; j <= lastCellNum; j++) {
cell = row.getCell(j);
if (cell == null) {
// continue;
linked.add("");
k++;
} else {
DecimalFormat df = new DecimalFormat("0");// 格式化 number
// String
// 字符
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");// 格式化日期字符串
DecimalFormat nf = new DecimalFormat("0.00");// 格式化数字


switch (cell.getCellType()) {
case XSSFCell.CELL_TYPE_STRING:
value = cell.getStringCellValue();
break;
case XSSFCell.CELL_TYPE_NUMERIC:
if ("@".equals(cell.getCellStyle().getDataFormatString())) {
if ((cell.getNumericCellValue() + "").indexOf(".") > -1 && cell.getNumericCellValue() != 0.0
&& !(cell.getNumericCellValue() + "").endsWith(".0")) {
value = cell.getNumericCellValue();
} else {
value = df.format(cell.getNumericCellValue());
}


} else if ("General".equals(cell.getCellStyle().getDataFormatString())) {
if ((cell.getNumericCellValue() + "").indexOf(".") > -1 && cell.getNumericCellValue() != 0.0
&& !(cell.getNumericCellValue() + "").endsWith(".0")) {
value = cell.getNumericCellValue();
} else {
value = df.format(cell.getNumericCellValue());
}
// value = df.format(cell.getNumericCellValue());
} else {
value = sdf.format(HSSFDateUtil.getJavaDate(cell.getNumericCellValue()));
}
break;
case XSSFCell.CELL_TYPE_BOOLEAN:
// System.out.println(i + "行" + j + " 列 is Boolean
// type");
value = cell.getBooleanCellValue();
break;
case XSSFCell.CELL_TYPE_BLANK:
// System.out.println(i + "行" + j + " 列 is Blank type");
value = "";
// System.out.println(value);
break;
default:
// System.out.println(i + "行" + j + " 列 is default
// type");
value = cell.toString();
}
if (value == null || "".equals(value)) {
// continue;
value = "";
k++;
}
linked.add(value);
}


}


if (k == (lastCellNum - firstCellNum + 1)) {
continue;
}
list.add(linked);
}
return list;
}


private static List<List<Object>> proFirstSheet(XSSFSheet sheet, int firstCellNum, int lastCellNum) {
List<List<Object>> list = new LinkedList<List<Object>>();
Object value;
XSSFRow row;
XSSFCell cell;
for (int i = sheet.getFirstRowNum(); i <= sheet.getPhysicalNumberOfRows(); i++) {
row = sheet.getRow(i);
if (row == null) {
continue;
}


// if (i == 0) {
// firstCellNum = row.getFirstCellNum();
// lastCellNum = row.getLastCellNum();
// }


List<Object> linked = new LinkedList<Object>();
int k = 0;
for (int j = firstCellNum; j <= lastCellNum; j++) {
cell = row.getCell(j);
if (cell == null) {
// continue;
linked.add("");
k++;
} else {
DecimalFormat df = new DecimalFormat("0");// 格式化 number
// String
// 字符
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");// 格式化日期字符串
DecimalFormat nf = new DecimalFormat("0.00");// 格式化数字


switch (cell.getCellType()) {
case XSSFCell.CELL_TYPE_STRING:
value = cell.getStringCellValue();
break;
case XSSFCell.CELL_TYPE_NUMERIC:
if ("@".equals(cell.getCellStyle().getDataFormatString())) {
if ((cell.getNumericCellValue() + "").indexOf(".") > -1 && cell.getNumericCellValue() != 0.0
&& !(cell.getNumericCellValue() + "").endsWith(".0")) {
value = cell.getNumericCellValue();
} else {
value = df.format(cell.getNumericCellValue());
}


} else if ("General".equals(cell.getCellStyle().getDataFormatString())) {
if ((cell.getNumericCellValue() + "").indexOf(".") > -1 && cell.getNumericCellValue() != 0.0
&& !(cell.getNumericCellValue() + "").endsWith(".0")) {
value = cell.getNumericCellValue();
} else {
value = df.format(cell.getNumericCellValue());
}
// value = df.format(cell.getNumericCellValue());
} else {
value = sdf.format(HSSFDateUtil.getJavaDate(cell.getNumericCellValue()));
}
break;
case XSSFCell.CELL_TYPE_BOOLEAN:
// System.out.println(i + "行" + j + " 列 is Boolean
// type");
value = cell.getBooleanCellValue();
break;
case XSSFCell.CELL_TYPE_BLANK:
// System.out.println(i + "行" + j + " 列 is Blank type");
value = "";
// System.out.println(value);
break;
default:
// System.out.println(i + "行" + j + " 列 is default
// type");
value = cell.toString();
}
if (value == null || "".equals(value)) {
// continue;
value = "";
k++;
}
linked.add(value);
}
}


if (k == (lastCellNum - firstCellNum + 1)) {
continue;
}
list.add(linked);
}
return list;
}


private static List<List<List<Object>>> read2007ExcelPmSingle(File file) throws IOException {
List<List<List<Object>>> res = new LinkedList<List<List<Object>>>();
List<List<Object>> list = new LinkedList<List<Object>>();
// 构造 XSSFWorkbook 对象，strPath 传入文件路径
XSSFWorkbook xwb = new XSSFWorkbook(new FileInputStream(file));


// 读取第一章表格内容
XSSFSheet sheet = xwb.getSheetAt(0);
int firstCellNum = 0;
int lastCellNum = 10;
list = proFirstSheet(sheet, firstCellNum, lastCellNum);
res.add(list);
file.delete();
return res;
}


/**
* Do export.
* 
* @param response
*            the response
* @param httpServletRequest
*            the data list
* @param execlName
*            the execl name
* @param tmpContent
*            the tmp content
* @param tmpContentCn
*            the tmp content cn
* 
* @throws Exception
*             the exception 目前多价导入专用
*/
public static String doExport(String savePath, List<List<List<Object>>> dataList, String execlName) {
// 生成excel
XSSFWorkbook workbook = printExcel2007(dataList);
// 导出excel
return writeExecl(savePath, workbook, execlName);
}


public static String writeExecl(String savePath, XSSFWorkbook workbook, String execlName) {
// 通过文件输出流生成Excel文件
File file = new File(savePath + execlName);
FileOutputStream outStream;
try {
outStream = new FileOutputStream(file);
workbook.write(outStream);
outStream.flush();
outStream.close();
} catch (Exception e) {
e.printStackTrace();
}


System.out.println("Excel 2007文件导出完成！导出文件路径：" + file.getPath());
return file.getPath();
}


private static XSSFWorkbook printExcel2007(List<List<List<Object>>> sheetList) {
// 创建工作簿实例
XSSFWorkbook workbook = new XSSFWorkbook();
for (int k = 0; k < sheetList.size(); k++) {
List<List<Object>> osh = sheetList.get(k);
XSSFSheet sheet = null;
// int cellNum = osh.get(0).size();
int cellNum = 0;
// 创建工作表实例
if (0 == k) {
cellNum = Integer.parseInt(MultiPriceEnum.IMPORTRULECOUNT.getValue());
sheet = workbook.createSheet("规则信息");


} else if (1 == k) {
cellNum = Integer.parseInt(MultiPriceEnum.IMPORTCOMPCOUNT.getValue());
sheet = workbook.createSheet("补偿信息");
}
cellNum += 1;// 结果列
// 设置列宽
setSheetColumnWidth(cellNum, sheet);
// 获取样式
XSSFCellStyle style = createTitleStyle(workbook);
if (osh != null) {
// 给excel填充数据
for (int i = 0; i < osh.size(); i++) {
// 将dataList里面的数据取出来
List<Object> line = osh.get(i);
int lineSize = line.size();
XSSFRow row1 = sheet.createRow(i);// 建立新行
if (i == 0) {
for (int j = 0; j < lineSize; j++) {
createCell(row1, j, style, XSSFCell.CELL_TYPE_STRING, line.get(j));
}
} else {
if (k == 0) {
for (int j = 0; j < lineSize; j++) {
createCell(row1, j, style, XSSFCell.CELL_TYPE_STRING, line.get(j));
}
} else {
for (int j = 0; j < lineSize; j++) {
createCell(row1, j, style, XSSFCell.CELL_TYPE_STRING, line.get(j));
}
}
}
}
} else {
createCell(sheet.createRow(0), 0, style, XSSFCell.CELL_TYPE_STRING, "查无资料");
}
}
return workbook;
}


private static void setSheetColumnWidth(int cellNum, XSSFSheet sheet) {
// 根据你数据里面的记录有多少列，就设置多少列
for (int i = 0; i < cellNum; i++) {
sheet.setColumnWidth((short) i, (short) 3000);
}


}


private static XSSFCellStyle createTitleStyle(XSSFWorkbook wb) {
XSSFFont boldFont = wb.createFont();
boldFont.setFontHeight((short) 200);
XSSFCellStyle style = wb.createCellStyle();
style.setFont(boldFont);
style.setDataFormat(HSSFDataFormat.getBuiltinFormat("###,##0.00"));
// style.setFillPattern(HSSFCellStyle.SOLID_FOREGROUND);
// style.setFillBackgroundColor(HSSFColor.LIGHT_ORANGE.index);
return style;
}


private static void createCell(XSSFRow row, int column, XSSFCellStyle style, int cellType, Object value) {
XSSFCell cell = row.createCell(column);
if (style != null) {
cell.setCellStyle(style);
}
String res = (value == null ? "" : value).toString();
switch (cellType) {
case HSSFCell.CELL_TYPE_BLANK: {
}
break;
case HSSFCell.CELL_TYPE_STRING: {
cell.setCellValue(res + "");
}
break;
case HSSFCell.CELL_TYPE_NUMERIC: {
cell.setCellType(HSSFCell.CELL_TYPE_NUMERIC);
cell.setCellValue(Double.parseDouble(value.toString()));
}
break;
default:
break;
}


}


public static void main(String[] args) throws Exception {


FileOutputStream fileOut = new FileOutputStream("D:\\excel.xls");
HSSFWorkbook workbook = null;
String filePath = null;
InputStream fis = new FileInputStream(filePath);
workbook = new HSSFWorkbook(fis);
for (int i = 0; i < 3; i++) {
HSSFSheet newsheet = null;
HSSFSheet fromsheet = workbook.getSheet("sheet1");
newsheet = workbook.createSheet("tt_" + (String.valueOf(i + 1)));
copyRows(workbook, fromsheet, newsheet, fromsheet.getFirstRowNum(), fromsheet.getLastRowNum());
}
}


@SuppressWarnings("deprecation")
private static void copyRows(HSSFWorkbook workbook, HSSFSheet fromsheet, HSSFSheet newsheet, int firstrow,
int lastrow) {
if ((firstrow == -1) || (lastrow == -1) || lastrow < firstrow) {
return;
}
// 拷贝合并的单元格
Region region = null;
for (int i = 0; i < fromsheet.getNumMergedRegions(); i++) {
region = fromsheet.getMergedRegionAt(i);
if ((region.getRowFrom() >= firstrow) && (region.getRowTo() <= lastrow)) {
newsheet.addMergedRegion(region);
}
}
HSSFRow fromRow = null;
HSSFRow newRow = null;
HSSFCell newCell = null;
HSSFCell fromCell = null;
// 设置列宽
for (int i = firstrow; i <= lastrow; i++) {
fromRow = fromsheet.getRow(i);
if (fromRow != null) {
for (int j = fromRow.getLastCellNum(); j >= fromRow.getFirstCellNum(); j--) {
int colnum = fromsheet.getColumnWidth((short) j);
if (colnum > 100) {
newsheet.setColumnWidth((short) j, (short) colnum);
}
if (colnum == 0) {
newsheet.setColumnHidden((short) j, true);
} else {
newsheet.setColumnHidden((short) j, false);
}
}
break;
}
}
// 拷贝行并填充数据
for (int i = 0; i <= lastrow; i++) {
fromRow = fromsheet.getRow(i);
if (fromRow == null) {
continue;
}
newRow = newsheet.createRow(i - firstrow);
newRow.setHeight(fromRow.getHeight());
for (int j = fromRow.getFirstCellNum(); j < fromRow.getPhysicalNumberOfCells(); j++) {
fromCell = fromRow.getCell((short) j);
if (fromCell == null) {
continue;
}
newCell = newRow.createCell((short) j);
newCell.setCellStyle(fromCell.getCellStyle());
int cType = fromCell.getCellType();
newCell.setCellType(cType);
switch (cType) {
case HSSFCell.CELL_TYPE_STRING:
newCell.setCellValue(fromCell.getRichStringCellValue());
break;
case HSSFCell.CELL_TYPE_NUMERIC:
newCell.setCellValue(fromCell.getNumericCellValue());
break;
case HSSFCell.CELL_TYPE_FORMULA:
newCell.setCellFormula(fromCell.getCellFormula());
break;
case HSSFCell.CELL_TYPE_BOOLEAN:
newCell.setCellValue(fromCell.getBooleanCellValue());
break;
case HSSFCell.CELL_TYPE_ERROR:
newCell.setCellValue(fromCell.getErrorCellValue());
break;
default:
newCell.setCellValue(fromCell.getRichStringCellValue());
break;
}
}
}
}
} 



```