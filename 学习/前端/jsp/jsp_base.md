[TOC]
##在JSP页面跳转到其他页面
--页面自动跳转：
<head>
<meta http-equiv="refresh" content="20;url=http://baidu.com">//20s后跳转到百度
</head>
##JSP页面自动刷新
--方式1：
<head>
  <meta http-equiv="refresh" content="20"> //每隔20秒刷新一次页面
</head>
--方式2：
通过js实现
<script language="JavaScript"> 
  function myrefresh(){ 
    window.location.reload(); 
  } 
  setTimeout('myrefresh()',1000); //指定1秒刷新一次 
</script> 
##在JSP页面显示当前时间
--静态显示时间
index.jsp
<head>
<% java.text.SimpleDateFormat simpleDateFormat = new java.text.SimpleDateFormat(  
     "yyyy-MM-dd HH:mm:ss");  
   java.util.Date currentTime = new java.util.Date();  
   String time = simpleDateFormat.format(currentTime).toString();
%>
</head>
<body>
  时间：<% =time %>
</body>
--静态显示时间2
<%=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Java.util.Date())%>
--动态显示当前时间
<body>
    <input id="showTime" readnoly/>
    <script type="text/javascript"> 
        function getNowTime(){
        var now = new Date();
        document.getElementById("showTime").value=now.getFullYear()+"-"+(now.getMonth()+1)+"-"+now.getDate();
        var second = now.getSeconds();
        if(second < 10){
            second = "0"+second;
        }
        document.getElementById("showTime").value+=" "+now.getHours()+":"+now.getMinutes()+":"+second;
        }
        window.setInterval("getNowTime()",1000);
    </script>
</body>

##在JSP页面获取当前项目名称的方法
方法1： <%= this.getServletContext().getContextPath() %>
方法2： 使用EL表达式${pageContext.request.contextPath}
form表单传值
<form name="form1" method="post" action="${pageContext.request.contextPath}/GetPmsServlet">
    <select id="city" name="city">
       <option value="nj">南京</option>
   </select>
   <input type="submit" value="提交">
</form>
点击提交后，city的值 就自动提交到了action后面的URL地址
在GetPmsServlet中获取form表单传过来的值：String city = request.getParameter("city");
##在JSP页面获取当前项目名称的方法：
方法1： <%= this.getServletContext().getContextPath() %>
方法2： 使用EL表达式（如果不清楚EL表达式是什么，大家可以百度一下）
${pageContext.request.contextPath}
