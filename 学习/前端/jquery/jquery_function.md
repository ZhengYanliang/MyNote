[TOC]
##jquery 返回顶部
```
<script language="javascript" type="text/javascript" src="jquery-1.4.2.min.js"></script>
<script language="javascript" type="text/javascript" src="scrollSilde.js"></script>
<script language="javascript" type="text/javascript" >
    $(function () {
        $(window).gotoTop({
            showHeight: 150, //设置滚动高度时显示
            speed: 200 //返回顶部的速度以毫秒为单位
        });
    });
</script>
其中scrollSilde.js代码为：
//返回顶部
$(function () {
    $.fn.gotoTop = function (options) {
        var defaults = {
            showHeight: 150,
            speed: 1000
        };
        var options = $.extend(defaults, options);
        $(document.body).prepend("<div id='totop'><a></a><p></p></div>");
        var $toTop = $(this);
        var $top = $("#totop");
        var $ta = $("#totop a");
        var $bt = $("#totop p");
        $toTop.scroll(function () {
            var scrolltop = $(this).scrollTop();
            if (scrolltop >= options.showHeight) {
                $top.show();
            }
            else {
                $top.hide();
            }
        });
        $ta.click(function () {
            $("html,body").animate({ scrollTop: 0 }, options.speed);
        });
        $bt.click(function () {
            $("#mess").show();
        });
    }
});
```
##jQuery 克隆对象
```
开始以为可以使用jQuery的clone（） 但后发现这个不能用了，于时gg之后发现现在jquery克隆对象使用extend插件了，下面来看看吧。

简单例子
// 浅层复制（只复制顶层的非 object 元素）  
var newObject = jQuery.extend({}, oldObject);  
  
// 深层复制（一层一层往下复制直到最底层）  
var newObject = jQuery.extend(true, {}, oldObject);
测试如下：
var obj1 = {
  'a': 's1',
  'b': [1,2,3,{'a':'s2'}],
  'c': {'a':'s3', 'b': [4,5,6]}
}
var obj2 = $.extend(true, {}, obj1);
obj2.a='s1s1';
obj2.b[0]=100;
obj2.c.b[0]=400;
console.log(obj1);
console.log(obj2);
obj2 内部元素的值改变之后，如果 obj1 的相应值保持不变，就说明复制成功。
ExtJS 也有类似的方法 Ext.apply，因此单独用 ExtJS 应该也能实现同样的功能。
后面在项目中原来使用了前端同学自己写的一个扩展 jQuery 的类库，而里边有些方法却没有实现，如live等。加上我们后台开发人员在项目中又使用了 jQuery，感觉显得很冗余。里边有个扩展的克隆对象的方法，如下：
/**
复制一个Object对像
* @param {Object} obj 要复制的Object对像
* @return {Object} 返回新对像
* @static
*/
clone: function (obj) {
    var objClone;
    if (obj instanceof Array) {
        objClone = [];
        for (var i = 0; i < obj.length; i++)
            objClone[i] = Js.clone(obj[i]);
        return objClone;
    } else if (obj instanceof Object) {
        if (obj.constructor == Object) {
            objClone = new obj.constructor();
        } else {
            objClone = new obj.constructor(obj.valueOf());
        }
        for (var key in obj) {
            if (objClone[key] != obj[key]) {
                if (typeof (obj[key]) == 'object') {
                    objClone[key] = Js.clone(obj[key]);
                } else {
                    objClone[key] = obj[key];
                }
            }
        }
        objClone.toString = obj.toString;
        objClone.valueOf = obj.valueOf;
        return objClone;
    }
    return obj;
}
于是又得后台开发人员将所有引用的地方换成了使用jQuery实现，将这个方法改成了jQuery扩展，如下：
$.extend({
    cloneObj: function (obj) {
        var objClone;
        if (obj instanceof Array) {
            objClone = [];
            for (var i = 0; i < obj.length; i++)
                objClone[i] = $.cloneObj(obj[i]);
            return objClone;
        } else if (obj instanceof Object) {
            if (obj.constructor == Object) {
                objClone = new obj.constructor();
            } else {
                objClone = new obj.constructor(obj.valueOf());
            }
            for (var key in obj) {
                if (objClone[key] != obj[key]) {
                    if (typeof (obj[key]) == 'object') {
                        objClone[key] = $.cloneObj(obj[key]);
                    } else {
                        objClone[key] = obj[key];
                    }
                }
            }
            objClone.toString = obj.toString;
            objClone.valueOf = obj.valueOf;
            return objClone;
        }
        return obj;
    }
});
试了下是没问题的，但总感觉有些多余。在What is the most efficient way to clone a JavaScript object?看到jQuery作者John Resig给出的回答是这样的。
// Shallow copy
var newObject = jQuery.extend({}, oldObject);
// Deep copy
var newObject = jQuery.extend(true, {}, oldObject);
可见，又是我们在造轮子了。自己扩展jQuery类库想法是好的，但想维护好不是一个人所能完成的，毕竟很多时候还要忙其他的项目。
```
##jquery最佳实践
```
1、使用CDN及其回退地址（fallback）

CDN代表内容传递网络（Content Delivery Network），是一个缓存了JavaScript文件的服务器。使用CDN之后，每当一个新用户发起请求的时候，你的应用程序可以使用CDN缓存，而不用从你的服务器上重新加载库文件。Google、Microsoft和JQuery都提供CDN服务。

鉴于网络并不总是100%可靠，服务器也可能因为一些原因宕机，你必须要确保即使这些事情发生，你的应用程序依然能正常运行。这时候我们就要用到回退地址：当应用程序无法找到缓存库的时候，它就会回退回来，使用服务器文件。

Google CDN 是这样的：
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"> </script>

Microsoft CDN是这样的：
<script src="//ajax.aspnetcdn.com/ajax/jquery/jquery-1.9.0.min.js"> </script>

需要注意的是，我们没有指定URL协议为http而是使用的//。这是因为CDN服务器支持http和https，如果你的网站拥有SSL认证，你无须修改就可以正常加载文件。

另外，就像我之前提到的那样，我们还需要一个回退地址，以防CDN服务器出现问题。
<script>Window.JQuery || document.write(&lsquo;<script src=&rdquo;script/localsourceforjquery&rdquo;></script>&rsquo;)

当然，你也可以用Require来配置需要的jQuery，不过我觉得就这样也不错。 2、限制DOM交互

用JavaScript操作DOM树是存在性能消耗的。jQuery也一样。所以，尽量减少与DOM的交互吧。当我帮助我一个同事提高数据显示速度的时候，我看见他在一个循环里面使用了选择器。这简直是性能杀手！他是这样写的：
containerDiv = $("#contentDiv");
for(var d =0; d < TotalActions; d++)
{
  containerDiv.append("<div><span class='brilliantRunner'>" +   d + "</span></div>");
}

有什么问题呢？咋一看没啥问题。而且我的同事也说这段代码跑得很欢乐呢！我真是哔了狗了！当TotalActions小于50时，察觉不到任何问题；但是其达到25000的时候，速度便降低了很多，原因（我也是google到的）就是DOM交互放到了循环当中。

对于这个功能，（多次尝试失败之后）我将循环中的直接DOM交互替换成了一个数组的push操作，然后用一个空字符串作为分隔符将数组连接（join）起来。最后，程序当然变得更加流畅和高效了。
var myContent=[];
for(var d = 0; d < TotalActions; d++)
{
  myContent.push("<div><span class='brilliantRunner'>" + d + "</span></div>");
}
containerDiv.html(myContent.join("")); 3、缓存

jQuery最重要也是最有特色的地方，就是它的选择器以及在DOM树中查找HTML元素的方式。但是，我多次看到，一些开发者在同一个函数中，多次调用相同的选择器，比如  $(“#divId”) 。尽管jQuery选择元素非常快，但也不要每次都去查找相同的元素吧。所以，你可以像这样缓存的你元素：
var $divId = $("#divId")

然后在接下来的代码中，就可以用$divId了。

对于下面的代码：
var thefunction = function()
{
    $("#mydiv").ToggleClass("zclass");
    $("#mydiv").fadeOut(800);
}

var thefunction2 = function()
{
    $("#mydiv").addAttr("name");
    $("#mydiv").fadeIn(400);
}

我们可以对它做这样的修改，并且使用链式语法，使其看起来更加漂亮：
var mydiv =$("#mydiv");
var thefunction = function()
{
  mydiv.ToggleClass("zclass").fadeOut(800);
}

var thefunction2 = function()
{
  mydiv.addAttr("name").fadeIn(400);
}

但是话又说回来，你也不用每次把所有东西都缓存起来。看下面的例子：
$("#link").click(function()
{
     $(this).addClass("gored");
}

在这里，我既没有用  $(“#link”) ，或者将其缓存起来，而是使用的 $(this) 。因为在这个例子中，我操作的对象就是这个链接本身。 4、find 和 filter

最近，在使用find()来获取jQuery对象结合的时候，我产生了一些困惑。然后我发现，这个操作可以替换为用filter()方法来实现。理解这两者的区别非常重要：
find: 将会从选定的元素开始，一直向下查找DOM树

filter: 是在jQuery集合当中查找 5、end()

当在jQuery集合中进行链式操作的时候，我有时候需要回到父对象去进行一些操作。比如你正在一个表格的第二行应用CSS，然后希望回到表格对象，对其添加一些样式。在你对行应用完样式之后，只要使用end()方法，你就会自动回到表格对象，然后随意的对其添加样式吧！

（译者注：find()、filter()和end()原文是大写，其实应该是小写） 6、对象字面量

当你通过链式语法来操作元素的CSS属性的时候，你可以使用对象字面量方式来提升性能。比如这段代码：
$("#myimg").attr("src", "thepath").attr("alt", "the alt text");

变成下面这样之后，不仅避免了操作DOM元素，而且还不用多次调用相关的设置方法：
$("#myimg").attr({"src": "thepath", "alt": "the alt text"}); 7、善用CSS类

尽可能使用CSS类而不要写内联CSS代码。我想这一点就不需要举例说明了吧。
```
##jquery小技巧
```
http://caibaojian.com/t/jquery


1. 返回顶部按钮
通过使用jQuery中的animate 和scrollTop 方法，不用插件就可以创建一个滚动到顶部的简单动画：

// Back to top

$('.top').click(function (e) {
  e.preventDefault();
  $('html, body').animate({scrollTop: 0}, 800);
});
<!-- Create an anchor tag -->
<a class="top" href="#">Back to top</a>
改变scrollTop 的值可以更改你想要放置滚动条的位置。所有你真正需要做的是在800毫秒的时间内设置文档主体的动画，直到它滚动到文档的顶部。
注：小心scrollTop的一些错误行为。


2.预加载图像
如果你的网页要使用大量开始不可见的（例如，悬停的）图像，那么可以预加载这些图像：
$.preloadImages = function () {
  for (var i = 0; i < arguments.length; i++) {
    $('<img>').attr('src', arguments[i]);
  }
};

$.preloadImages('img/hover-on.png', 'img/hover-off.png');

3.检查图像是否加载
有时为了继续脚本，你可能需要检查图像是否全部加载完毕：
$('img').load(function () {
  console.log('image load successful');
});
你也可以用ID或类替换<img>标签来检查某个特定的图像是否被加载。

4.自动修复破坏的图像
逐个替换已经破坏的图像链接是非常痛苦的。不过，下面这段简单的代码可以帮助你：
$('img').on('error', function () {
  if(!$(this).hasClass('broken-image')) {
    $(this).prop('src', 'img/broken.png').addClass('broken-image');
  }
});
即使没有任何断掉的链接，加上这一段代码也不会让你有任何损失。

5.悬停切换类
假设你希望当用户将鼠标悬停在可点击的元素上时，它会改变颜色。那么你可以在用户悬停的时候添加类到元素中，反之则删除类：
$('.btn').hover(function () {
  $(this).addClass('hover');
}, function () {
  $(this).removeClass('hover');
});
你只需要添加必要的CSS即可。更简单的方法是使用toggleClass 方法：
$('.btn').hover(function () {
  $(this).toggleClass('hover');
});
注：可能在这种情况下，CSS这种解决方案更快，不过了解这个方法很有必要。


6.禁用输入字段
有时候，你可能想要禁用表格的提交按钮或它的某一项文字输入直到用户执行了特定操作（例如，勾选“我已阅读相关条款”复选框）。添加 disabled属性到你的输入就可以在你想要的时候才启用它：

$('input[type="submit"]').prop('disabled', true);
然后你只需要运行输入的prop 方法就可以了，不过disabled 的值要设置为false：

$('input[type="submit"]').prop('disabled', false);

7.停止加载链接
有时候，你既不需要链接到某个特定的网页，也不想要重新加载页面——你可能希望链接做点别的事情，例如说触发一些其他脚本。这就要在阻止默认动作上做文章了：
$('a.no-link').click(function (e) {
  e.preventDefault();
});

8.淡入/滑动切换
滑动和淡入都是我们用jQuery做动画的时候大量运用的东西。如果你只是想在用户点击之后展示一个元素的话，那么用fadeIn 和slideDown 方法就很完美。但是，如果你想要元素在第一次点击的时候出现，然后在第二次点击的时候消失的话，那么可以试试下面的代码：
// Fade
$('.btn').click(function () {
  $('.element').fadeToggle('slow');
});
// Toggle
$('.btn').click(function () {
  $('.element').slideToggle('slow');
});

9.简单的手风琴
这是一个可快速生成手风琴的简单方法：
// Close all panels
$('#accordion').find('.content').hide();
// Accordion
$('#accordion').find('.accordion-header').click(function () {
  var next = $(this).next();
  next.slideToggle('fast');
  $('.content').not(next).slideUp('fast');
  return false;
});
通过添加这个脚本，你真正需要做的仅仅是在页面上添加必要的HTML元素，这样它就可以运行工作了。


10.让两个div高度相同
有时候，你需要让两个div无论包含什么内容都拥有相同的高度：
$('.div').css('min-height', $('.main-div').height());
设置 min-height，这意味着它可以比主div大但绝对不能比主div小。不过，还有一种更灵活的方法是遍历一组元素，然后将高度设置为最高的那个元素的高度：
var $columns = $('.column');
var height = 0;
$columns.each(function () {
  if ($(this).height() > height) {
    height = $(this).height();
  }
});
$columns.height(height);
如果你希望所有列的高度相同：


var $rows = $('.same-height-columns');
$rows.each(function () {
  $(this).find('.column').height($(this).height());
});


11.在新标签页/窗口打开外部链接


在一个新的浏览器tab或窗口中打开外部链接，并确保同一个来源的链接能在同一个tab或者窗口中打开：


$('a[href^="http"]').attr('target', '_blank');
$('a[href^="//"]').attr('target', '_blank');
$('a[href^="' + window.location.origin + '"]').attr('target', '_self');
注意：window.location.origin 在IE10中无效。修复的时候要小心这个问题。


12.通过文本查找元素
通过使用jQuery中的contains() 选择器，你可以找到元素内容的文本。如果文本不存在，那就隐藏该元素：
var search = $('#search').val();
$('div:not(:contains("' + search + '"))').hide();
在改变Visibility时触发

当用户不再关注某个tab，或重新聚焦原来的那个tab上时，触发JavaScript：

$(document).on('visibilitychange', function (e) {
  if (e.target.visibilityState === "visible") {
    console.log('Tab is now in view!');
  } else if (e.target.visibilityState === "hidden") {
    console.log('Tab is now hidden!');
  }
});


13.AJAX调用错误处理

当Ajax调用返回404或500错误时，就执行错误处理程序。如果没有定义处理程序，其他的jQuery代码或会就此罢工。定义一个全局的Ajax错误处理程序：

$(document).ajaxError(function (e, xhr, settings, error) {
  console.log(error);
});


14.链式插件调用
jQuery允许“链式”插件的方法调用，以减轻反复查询DOM并创建多个jQuery对象的过程。比方说，下面的代码片段代表了你的插件方法调用：

$('#elem').show();
$('#elem').html('bla');
$('#elem').otherStuff();
通过使用链式，可以大大改善：


$('#elem')
  .show()
  .html('bla')
  .otherStuff();
还有一种方法是在（前缀$）变量中高速缓存元素：


var $elem = $('#elem');
$elem.hide();
$elem.html('bla');
$elem.otherStuff();
链式和高速缓存的方法都是jQuery中可以让代码变得更短和更快的代最佳做法。
```
##jquery实现input输入框实时输入触发事件

```
<input id="productName" name="productName" class="wid10" type="text" value="" />

//绑定商品名称联想 
$('#productName').bind('input propertychange', function() {
    searchSkubyCode();
});

function searchSkubyCode(){
    var bcode = $("#fbarCodess").val();
    console.log(bcode)
    $.ajax({
        url:basePath + "/cr/findSkusByCode.do?code="+bcode,
        type: "POST",
        dataType: "json",
        success: function(data) {
            console.log(data)
        }
    }); }  

```
##jquery实现input输入框实时输入触发事件(应用实例)
```
1.引入样式：
< link   href = " ${resRoot} /common/css/widgets.css"   rel = "stylesheet"   type = "text/css"   />  
< script   src = " ${resRoot} /assets/plugins/bootstrap 3.0.0/dist/js/typeahead.js" type = "text/javascript" ></ script >
< script   src = " ${resRoot} /pages/sale/coupon/couponMod.js"   type = "text/javascript" ></ script >       

2.页面：
< div class="form-group">
    < label class="f_l discount_goods">
        < span class="required" aria-required="true">
            *
        </ span>
        活动商品
    </ label>
    < div class="help-block pa_t3 col-md-7">
        < span class="help-block f_l ma_t0 ma_b10 pa_t1" id="select_link" href="BC_priceDiscount.html">
            指定条码
        </ span>
        < table class="table table-striped table-bordered table-hover dataTable ta_c"
        id="bcTable">
            < thead>
                < tr>
                    < th class="ta_c">
                        条码
                    </ th>
                    < th class="ta_c">
                        商品名称
                    </ th>
                    < th class="ta_c">
                        < a class="layer_button new_rows" onclick="javascript:;">
                            增行
                        </ a>
                    </ th>
                </ tr>
            </ thead>
            < tbody id="bcTbody">
                < tr>
                    < td>
                        < input class="form-control input-inline input-sm wb100 codeInputs" id="fbarCodess"
                        name="barCode-0" placeholder="2603234283069" type="text">
                        </ td>
                        < td>
                            < input class="form-control input-inline input-sm wb100" id="fbarName"
                            name="barName-0" placeholder="贝因美1段奶粉" type="text">
                            </ td>
                            < td>
                                < a class="layer_button dele" onclick="javascript:;">
                                    删行
                                </ a>
                            </ td>
                        </ tr>
                        < tr>
                            < td>
                                < input class="form-control input-inline input-sm wb100 codeInputs" id="fbarCodess"
                                name="barCode-1" placeholder="2703234283069" type="text">
                                </ td>
                                < td>
                                    < input class="form-control input-inline input-sm wb100" id="fbarName"
                                    name="barName-1" placeholder="贝因美2段奶粉" type="text">
                                    </ td>
                                    < td>
                                        < a class="layer_button dele" onclick="javascript:;">
                                            删行
                                        </ a>
                                    </ td>
                                </ tr>
                            </ tbody>
                        </ table>
                    </ div>
                </ div>


3.实现：
动态增行、删行的实现
$( ".new_rows" ).on( "click" , function (){
             var  index = $( "#indexFlag" ).val();
             var  dom= '<tr><td><input class="form-control input-inline input-sm wb100 codeInputs" id="fbarCodess" name="barCode-' +index+ '" placeholder="28603234283069" type="text"></td><td><input class="form-control input-inline input-sm wb100" id="fbarName" name="fbarName-' +index+ '"  placeholder="贝因美3段奶粉" type="text"></td><td><a class="layer_button dele" onclick="javascript:;">删行</a></td></tr>' ,
                tbody=$( this ).parents( "table" ).find( "tbody" );
            tbody.append(dom);
            index++;
            $( "#indexFlag" ).val(index);
            $( ".dele" ).on( "click" , function (){
                 var  self=$( this ),
                    parent=self.parents( 'tr' );
                parent.remove();
            });
        })   

$(function(){
    initBind();
}); 


自己页面的js:couponMod.js  :
var  basePath =  '${base}' ;
//存放条码编码集合
var  barCodeList =  new  Array();
//存放条码编码对应商品信息的集合
var  goodsList =  new  Array();

function  initBind(){
     // 选择供应商编码
    $( "#bcTbody" ).delegate( "#fbarCodess" , "focus" , function () {
         var  status =  true ;
         var  f = $( this );
        console.log($( this ).parent().parent().parent().attr( "id" ));
         var  index = $( this ).parent().parent().index();
         var  flags = $( "#bcTable" ).find( "tr" ).eq(index).find( "td" ).eq($( this ).parent().index()).find( "input" );
         if ($( this ).parent().parent().parent().attr( "id" )== 'bcTbody' )
        {
            flags=f;
        }
         //查询编码
        $(f).typeahead({//f表示当前对象
                  source:  function (query, process) {
                        $( f ).val(query.trim());
                        $.ajax({
                            type:  "POST" ,
                            url:basePath +  "/cr/findGoodsByCode.do" ,
                            data:{ "query" :query, "flag" :0},
                            async:  false ,
                            success:  function (data) {
                                console.log(data)
                                 var  source = eval( '(' +data+ ')' );
                                console.log(source)
                                process(source);
                            }
                        });
                  },
                  updater :  function (item) {
                       var  bcode = $( "#fbarCodess" ).val();
                         if  (bcode !=  "" ) {
                            $.each(barCodeList, function (i,barName) {
                                 if  (barName == item) {
                                    status =  false ;
                                }
                            });
                             // 判断是否重复添加
                             if  (status) {
                                $.ajax({
                                    type :  "post" ,
                                    url:basePath +  "/cr/getOneByCode.do" ,
                                    data:{ "query" :bcode},
                                    dataType :  "json" ,
                                    success :  function (data) {
                                        console.log(data)
                                        barCodeList.push(item);
                                         var  content = data;
                                        goodsList.push(content); //往数组中放入一个对象
                                        values(f,1,content.fspuTitle);//设置第二个td的value值
//                                        $("#fbarCodess").attr("readonly","readonly");
         //                                flags.attr("readonly","readonly");
         //                                f.attr("readonly","readonly");
                                    }
                                });
                            }
                             else {
                                alert( "您已经添加过该供应商编码了，请重新添加" );
                                status =  true ;
                                item =  "" ;
                            }
                        }
                         return  item;
                    }
            });
    });
}
//给input赋值
function  values($flags, index, value) {
    $flags.parent().parent().find( "td" ).eq(index).find( "input" ).attr( "value" ,value);
}  


批量保存的实现：
< button   type = "button"   class = "btn btn-circle blue btn-md"   style =" padding :  8px 25px ;"  id = "couponGoodsM" > 保存 </ button >  
$( "#couponGoodsM" ).on( "click" , function () {
     var  ruleId = $( "#ruleId" ).val();
    console.log(barCodeList)
    console.log(goodsList)
    
     var  couponGoodsForm = $( '#couponGoodsForm' );
     var  param = $.serializeObject(couponGoodsForm);
     var  paramJson = encodeURI(JSON.stringify(goodsList));   //前端编码
    $.ajax({  
        url: '${base}/cr/addCouponGoods.do?ruleId=' +ruleId, // 跳转到 action
        data:{ "paramJson" :paramJson},
        type: 'post' ,  
        dataType: 'json' ,
        success:  function (data) {
             if (data.status ==  "1"  ){
                console.log( "修改优惠券商品成功！" );
                layer.msg( '修改成功！' , {time: 1500});
                goBack();
            } else {
                console.log( "修改优惠券商品失败！" );
                layer.msg( '修改失败！' , {time: 1500});
            }  
         },  
         error :  function () {
             console.log( "修改优惠券商品异常！" );
             layer.msg( '修改异常！' , {time: 1500});
         }  
    });
});


java代码：
@RequestMapping (value =  "addCouponGoods" , produces =  "text/html;charset=UTF-8" )
     public   void  addCouponGoods(HttpServletRequest  request , HttpServletResponse  response ,
             @RequestParam  String  paramJson ) {
         // 获取参数
        Map<String, Object>  result  =  new  HashMap<String, Object>();
         try  {
            String  ruleId  =  request .getParameter( "ruleId" );
             paramJson  = java.net.URLDecoder. decode ( paramJson ,  "utf-8" );
             log .info( "DisRuleAction====>addCouponGoods====>paramJson:"  +  paramJson .toString());
            CompanyInfoDto  companyInfoDto  =  this .getMerchantInfo( request );
            String  suppId  =  companyInfoDto .getCompanyId().toString();
             // JSONObject jsonObject = JSONObject.fromObject(paramJson);
            JSONArray  data  = JSONArray. fromObject ( paramJson );
            
            List<CouponGoodsDto>  couponGoodsList  =  data .toList( data , CouponGoodsDto. class ) ;//将数组转为list
            BaseResult  br  =  disRuleService .addCouponGoods( couponGoodsList ,  ruleId );
             if  ( br .getStatus().equals(MmcConstants. REG_SUCC )) {
                 result .put( STATUS ,  SUCCESS );
                 result .put( MESSAGE ,  "新增成功" );
            }  else  {
                 result .put( STATUS ,  ERROR );
                 result .put( MESSAGE ,  "新增失败" );
            }
        }  catch  (Exception  e ) {
             log .error( "DisRuleAction====>addCouponGoods====>error：" ,  e .toString());
             result .put( STATUS ,  ERROR );
             result .put( MESSAGE ,  "新增异常" );
        }
        ajaxJson( response , JSONObject. fromObject ( result ).toString());
    }
}  


```

##检测Internet Explorer版本
```
$(document).ready(function() {
      if (navigator.userAgent.match(/msie/i) ){
        alert('I am an old fashioned Internet Explorer');
      }
});
```
##平稳滑动到页面顶部
```

$("a[href='#top']").click(function() {
  $("html, body").animate({ scrollTop: 0 }, "slow");
  return false;
});
```
##固定在顶部
```
非常有用的代码片段，它允许一个元素固定在顶部。对导航按钮、工具栏或重要信息框是超级有用的。
$(function(){
	var $win = $(window)
	var $nav = $('.mytoolbar');
	var navTop = $('.mytoolbar').length && $('.mytoolbar').offset().top;
	var isFixed=0;

	processScroll()
	$win.on('scroll', processScroll)

	function processScroll() {
	var i, scrollTop = $win.scrollTop()

	if (scrollTop >= navTop && !isFixed) { 
		isFixed = 1
		$nav.addClass('subnav-fixed')
	} else if (scrollTop <= navTop && isFixed) {
		isFixed = 0
 		$nav.removeClass('subnav-fixed')
	}
}
来源：  http://netsmell.com/post/10-jquery-snippets-for-efficient-web-development.html
```
##用其他内容取代html标志
```
$('li').replaceWith(function(){
  return $("<div />").append($(this).contents());
});
```
##检测视窗宽度
```
现在移动设备比过时的电脑更普遍，能够方便去检测一个更小的视窗宽度会很有帮助。幸运的是，用jQuery来做超级简单。
var responsive_viewport = $(window).width();

/* if is below 481px */
if (responsive_viewport < 481) {
    alert('Viewport is smaller than 481px.');
} /* end smallest screen */
```
##自动定位并修复损坏图片
```
如果你的站点比较大而且已经在线运行了好多年，你或多或少会遇到界面上某个地方有损坏的图片。这个有用的函数能够帮助检测损坏图片并用你中意的图片替换它，并会将此问题通知给访客。
$('img').error(function(){
	$(this).attr('src', 'img/broken.png');
});
```
##检测复制、粘贴和剪切的操作
```
使用jQuery可以很容易去根据你的要求去检测复制、粘贴和剪切的操作。
$("#textA").bind('copy', function() {
    $('span').text('copy behaviour detected!')
}); 
$("#textA").bind('paste', function() {
    $('span').text('paste behaviour detected!')
}); 
$("#textA").bind('cut', function() {
    $('span').text('cut behaviour detected!')
});
```
##遇到外部链接自动添加target=”blank”的属性
```
当链接到外部站点时，你可能使用 target=”blank”的属性去在新界面中打开站点。问题在于target=”blank”属性并不是W3C有效的属性。让我们用jQuery来补 救：下面这段代码将会检测是否链接是外链，如果是，会自动添加一个target=”blank”属性。
var root = location.protocol + '//' + location.host;
$('a').not(':contains(root)').click(function(){
    this.target = "_blank";
});
```
##在图片上停留时逐渐增强或减弱的透明效果
```
$(document).ready(function(){
    $(".thumbs img").fadeTo("slow", 0.6); // This sets the opacity of the thumbs to fade down to 60% when the page loads

    $(".thumbs img").hover(function(){
        $(this).fadeTo("slow", 1.0); // This should set the opacity to 100% on hover
    },function(){
        $(this).fadeTo("slow", 0.6); // This should set the opacity back to 60% on mouseout
    });
});
```
##在文本或密码输入时禁止空格键
```
在很多表格领域都不需要空格键，例如，电子邮件，用户名，密码等等等。这里是一个简单的技巧可以用于在选定输入中禁止空格键。
$('input.nospace').keydown(function(e) {
  if (e.keyCode == 32) {
    return false;
  }
});
```
##jQuery - 基于serializeArray的serializeObject
```
jQuery方法$.fn.serialize，可将表单序列化成字符串；
方法$.fn.serializeArray，可将表单序列化成数组。
如果需要其序列化为JSON对象，那么可以基于serializeArray编写方法serializeObject轻松实现：
jQuery.prototype.serializeObject = function() {
    var obj = new Object();
    $.each(this.serializeArray(),
    function(index, param) {
        if (! (param.name in obj)) {
            obj[param.name] = param.value;
        }
    });
    return obj;
};
注：当表单中参数出现同名时，serializeObject会取第一个，而忽略后续的。


设有
<form>  
    <input type="text" name="username" />  
    <input type="text" name="password" />  
</form>  
则
jQuery("form").serialize(); //"username=&password="  
jQuery("form").serializeArray(); //[{name:"username",value:""},{name:"password",value:""}]  
jQuery("form").serializeObject(); //{username:"",password:""} 


+ 此版本不再兼容IE8
//work with jQuery 2.x  
jQuery.prototype.serializeObject=function(){  
    var hasOwnProperty=Object.prototype.hasOwnProperty;  
    return this.serializeArray().reduce(function(data,pair){  
        if(!hasOwnProperty.call(data,pair.name)){  
            data[pair.name]=pair.value;  
        }  
        return data;  
    },{});  
};  


+ 减少方法依赖，扩大兼容范围
+ 改用原生循环，提升代码性能
//work with jQuery Compact 3.x  
jQuery.prototype.serializeObject=function(){  
    var a,o,h,i,e;  
    a=this.serializeArray();  
    o={};  
    h=o.hasOwnProperty;  
    for(i=0;i<a.length;i++){  
        e=a[i];  
        if(!h.call(o,e.name)){  
            o[e.name]=e.value;  
        }  
    }  
    return o;  
};  
```



