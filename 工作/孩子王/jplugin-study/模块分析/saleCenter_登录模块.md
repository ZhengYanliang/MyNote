##web.xml
##index.jsp


```
１）采用绝对路径，但为了解决不同部署方式的差别，在所有非struts标签的路径前加${pageContext.request.contextPath}，如原路径为： 
”/images/title.gif”，改为 
“${pageContext.request.contextPath}/images/title.gif”。 
代码” ${pageContext.request.contextPath}”的作用是取出部署的应用程序名，这样不管如何部署，所用路径都是正确的。
 
缺点： 
操作不便，其他工具无法正确解释${pageContext.request.contextPath} 
２） 采用相对路径，在每个JSP文件中加入base标签，如： 
<base href="http://${header['host']}${pageContext.request.contextPath}/pages/cust/relation.jsp" /> 
这样所有的路径都可以使用相对路径。


缺点： 
对于被包含的文件依然无效。 
    真正使用时需要灵活应用１）和２），写出更加健壮的代码。


可以使用${pageContext.request.contextPath}，也同时可以使用<%=request.getContextPath()%>达到同样的效果，同时，也可以将${pageContext.request.contextPath}，放入一个JSP文件中，将用C：set放入一个变量中，然后在用的时候用EL表达式取出来。  


如： <c:set var="ctx" value="${pageContext.request.contextPath}" /> 
```
index.html页面中引入了app.js



