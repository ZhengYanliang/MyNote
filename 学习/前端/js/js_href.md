##js中"window.location.href"、"location.href"、"parent.location.href"、"top.location.href"的用法
```
实例 一：
"window.location.href"、"location.href"是本页面跳转


"parent.location.href"是上一层页面跳转


"top.location.href"是最外层的页面跳转


举例说明：
如果A,B,C,D都是aspx，D是C的iframe，C是B的iframe，B是A的iframe，如果D中js这样写



"window.location.href"、"location.href"：D页面跳转


"parent.location.href"：C页面跳转


"top.location.href"：A页面跳转


如果D页面中有form的话,


<form>:  form提交后D页面跳转


<form target="_blank">:  form提交后弹出新页面


<form target="_parent">:  form提交后C页面跳转


<form target="_top"> :  form提交后A页面跳转


实例二：
js 控制 窗口 跳转 问题！本来 parent.location.href='';是可以让父窗口跳转的，可是我一共两个框架，第一个分上下，上为导航，不动，第二个是在下面，再分一个左右，下左为导航，不动，下右为具体的内容。而如果超时需要跳转回登录页面的时候，如果仅使用win... 展开
window.parent.parent.location.href=“你定义要跳转的页面”
```
 


##关于页面刷新，D 页面中这样写：
```
"parent.location.reload();": C页面刷新  （当然，也可以使用子窗口的 opener 对象来获得父窗口的对象：window.opener.document.location.reload(); ）



"top.location.reload();": A页面刷新
```