##input 置灰的两种方式
```
方式一：

1、<input type="text" name="name" value="xxx" disabled="true"/>
方式二：
2、<input type="text" name="name" value="xxx" readonly="true"/>


这两种写法都会使显示出来的文本框不能输入文字，
但disabled会使文本框变灰，而且通过request.getParameter("name")得不到文本框中的内容（如果有的话），
而readonly只是使文本框不能输入，外观没有变化，而且通过request.getParameter("name")可以得到内容。
```
##JQuery让input从disabled变成enabled
document.getElementById("removeButton").disabled = false;//普通Js写法
$("#removeButton").removeAttr("disabled");//要变成Enable，JQuery只能这么写
$("#removeButton").attr("disabled","disabled");//再改成disabled
##jquery获取td中的input
<tr class='manual'><td><input onkeydown='javascript:searchCate(event,this,"+t+");' style='width:105px;' onblur='javascript:getMeta(this," + t + ");'></td><td><button onclick='delTRow(this.parentNode.parentNode)'>删除</button></td></tr>
$(obj).parent().parent().children("td").eq(0).find("input").val('');
$("#tbl tr").each(function(trindex, tritem) {
    alert($(tritem).find("td").eq(0).find("input").val());
});
function searchCate(event,obj,t){
    if (event.keyCode == "13") {
        getMeta(obj,t);
    }
}
