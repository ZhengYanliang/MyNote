##select2完成编辑的功能
```
< input   type = "text"   id = "kid2"   class = "form-control"   title = ''   value = "6"   >  <!--设置一个默认值-->
$(function(){
     initDemo1();

}) 

function  initDemo1(){
    $( '#kid2' ).select2({
        placeholder :  "请选择" ,
         // minimumInputLength: 1,
        
        initSelection :  function (element, callback) { //要放在ajax方法之前
              var  ev = element.val();//获取<input type="text" id="kid2" />元素的值
             callback({ //自定义属性名称要与formatSelection返回的属性名称一致
                 id: ev,
                 name: ev
             });
        },
        id :  function (priv) {
             return  priv.id;
        },
        formatResult :  function (word) { 
             if  (word.id){
                 var  markup =  "<div id='"
                        + word.id
                        +  "'>"
                        + word.name +  "</div>" ;
                 return  markup;
            }
        },  // omitted for brevity, see the source of this page  
        formatSelection :  function (repo) {
             return  repo.name;
        },  // omitted for brevity, see the source of this page
        dropdownCssClass :  "bigdrop" ,  // apply css that makes the
                                         // dropdown taller
        ajax : {  
            url :  "${base}/front/list.do" ,
            dataType :  'json' ,
            quietMillis : 250,
            data :  function (term, pageNumber) {
                 return  {
                    name : term,  // search term
                    pageSize:10,
                    pageNumber:pageNumber
                };
            },
            results :  function (data, pageNumber) { // data = Object { page :  1 ,  rows :  Array[9] ,  total :  9 } ,  pageNumber =  1; rows[0] = Object {id:"1",name:"name01"}
                 if (data&&data.rows&&data.rows.length){     
                     var  more = (pageNumber*10)<data.total;     //用来判断是否还有更多数据可以加载
                     return  {
                        results:data.rows,more:more    
                    };
                } else {
                     return  {
                        results : data.rows
                    };
                }                
            },
            cache :  true
        },
        escapeMarkup :  function (m) {  return  m;    }
    }); }   

``` http://my.oschina.net/5say/blog/311622
##自动完成下拉框 Select2 关键字搜索的实例（本地数据与异步获取）
```
<input name="test" type="hidden" id="userSelect" style="width: 600px" value="上海^漳州" />
使用本地数据的写法


$('#userSelect').select2({
    placeholder          : "请输入",
    minimumInputLength   : 1,
    multiple             : true,
    separator            : "^",                             // 分隔符
    maximumSelectionSize : 5,                               // 限制数量
    initSelection        : function (element, callback) {   // 初始化时设置默认值
        var data = [];
        $(element.val().split("^")).each(function () {
            data.push({ id: this, text: this });
        });
        callback(data);
    },
    createSearchChoice   : function(term, data) {           // 创建搜索结果（使用户可以输入匹配值以外的其它值）
        return { id: term, text: term };
    },
    formatSelection : function (item) { return item.id; },  // 选择结果中的显示
    formatResult    : function (item) { return item.id; },  // 搜索列表中的显示
    data: {
        results: [
            { id: "北京", text: "bj beijin 北京" },
            { id: "厦门", text: "xm xiamen 厦门" },
            { id: "福州", text: "fz fuzhou 福州" }
        ]
    }
});
使用异步数据的写法


脚本


$('#userSelect').select2({
    placeholder          : "请输入",
    minimumInputLength   : 1,
    multiple             : true,
    separator            : "^",                             // 分隔符
    maximumSelectionSize : 5,                               // 限制数量
    initSelection        : function (element, callback) {   // 初始化时设置默认值
        var data = [];
        $(element.val().split("^")).each(function () {
            data.push({id: this, text: this});
        });
        callback(data);
    },
    createSearchChoice   : function(term, data) {           // 创建搜索结果（使用户可以输入匹配值以外的其它值）
        return { id: term, text: term };
    },
    formatSelection : function (item) { return item.id; },  // 选择结果中的显示
    formatResult    : function (item) { return item.id; },  // 搜索列表中的显示
    ajax : {
        url      : "test-api",              // 异步请求地址
        dataType : "json",                  // 数据类型
        data     : function (term, page) {  // 请求参数（GET）
            return { q: term };
        },
        results      : function (data, page) { return data; },  // 构造返回结果
        escapeMarkup : function (m) { return m; }               // 字符转义处理
    }
});
服务端（这里以 Laravel 为例）


Route::get('test-api', function () {


    $q = Input::get('q');


    // do something ...


    return array(
        // 'more' => false,
        'results' => array(
            array('id' => '北京', 'text' => 'bj beijin 北京'),
            array('id' => '厦门', 'text' => 'xm xiamen 厦门'),
            array('id' => '福州', 'text' => 'fz fuzhou 福州'),
        ),
    );


});
```