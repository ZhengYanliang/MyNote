##解决空指针问题
跟踪代码发现，如果要使用本地的数据源，db_name必须设置为database.




##解决 java.lang.IllegalStateException : getInputStream() has already been called for this request


## java.lang.RuntimeException: Can't find method [getStoreInfoList] with arg:javax.servlet.http.HttpServletRequestjavax.servlet.http.HttpServletResponse in com.haiziwang.jplugin.study.ctrl.OcanalController
at net.jplugin.ext.webasic.impl.helper.ObjectCallHelper.getMethod(ObjectCallHelper.java:141)
```
//add webex controller  ��չ��webcontroller
public static void addWebExControllerExtension(AbstractPlugin plugin,String path,Class beanClz){
plugin.addExtension(Extension.create(net.jplugin.ext.webasic.Plugin.EP_WEBEXCONTROLLER, path, ClassDefine.class,new String[][]{{"clazz",beanClz.getName()}} ));
}




//add web controller  Web����
public static void addWebControllerExtension(AbstractPlugin plugin,String path,Class beanClz){
plugin.addExtension(Extension.create(net.jplugin.ext.webasic.Plugin.EP_WEBCONTROLLER, path, ObjectDefine.class,new String[][]{{"objType","javaObject"},{"objClass",beanClz.getName()}} ));
}
```


解决方法：将 addWebControllerExtension改为 addWebExControllerExtension

##