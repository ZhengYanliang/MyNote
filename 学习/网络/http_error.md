##HTTP Status 406 
```
可能引起的原因： 
 1.由于设置了@ResponseBody,要把对象转换成json格式，缺少转换依赖的jar包，故此错。

 2. 返 回的是json但是现在制定是text/plain;charset=UTF-8，文本        @RequestMapping(value = "acct/save", method = RequestMethod.POST, produces = "application/json")
```
##ajax页面提交数据，后台接收不到数据
```
contentType  是客户端到服务器端
dataType是 服务器端返回

//contentType: "application/json; charset=utf-8",//(不可以)
//contentType: "text/xml",//(不可以)
contentType:"application/x-www-form-urlencoded",//(可以)
当ajax提交数据，没有设置contentType属性时，默认为 contentType:"application/x-www-form-urlencoded".
```

## HTTP/1.1 505 HTTP Version Not Supported
```
今天在用短信接口时，出现“HTTP/1.1 505 HTTP Version Not Supported”错误，但是有些的短信能顺利发送成功，经过查找发现是因为短信内容中含有空格。

百度后找到原因是因为经过查找发现是因为“HTTP写的格式是非常严谨的，只要格式不匹配，就会报错误，而空格肯定匹配格式”，所以如果有空格得用”%20″代替！

解决方法：

比如 http://baidu.com?name=han&content=hello world则这样处理
url = “http://baidu.com?name=han&content=”+URLEncoder.encode(“hello world”，“utf-8”);

```
##GET请求中URL的最大长度限制总结
```
浏览器
1、IE
IE浏览器（Microsoft Internet Explorer） 对url长度限制是2083（2K+53），超过这个限制，则自动截断（若是form提交则提交按钮不起作用）。

2、firefox
firefox（火狐浏览器）的url长度限制为 65 536字符，但实际上有效的URL最大长度不少于100,000个字符。

3、chrome
chrome（谷歌）的url长度限制超过8182个字符返回本文开头时列出的错误。

4、Safari
Safari的url长度限制至少为 80 000 字符。

5、Opera
Opera 浏览器的url长度限制为190 000 字符。Opera 9 地址栏中输入190 000字符时依然能正常编辑。

服务器
1、Apache
Apache能接受url长度限制为8 192 字符

2、IIS
Microsoft Internet Information Server(IIS)能接受url长度限制为16 384个字符。
这个是可以通过修改的（IIS7）configuration/system.webServer/security/requestFiltering/requestLimits@maxQueryStringsetting.<requestLimits maxQueryString="length"/>

3、Perl HTTP::Daemon
Perl HTTP::Daemon 至少可以接受url长度限制为8000字符。Perl HTTP::Daemon中限制HTTP request headers的总长度不超过16 384字节(不包括post,file uploads等)。但当url超过8000字符时会返回413错误。
这个限制可以被修改，在Daemon.pm查找16×1024并更改成更大的值。

4、ngnix
可以通过修改配置来改变url请求串的url长度限制。

client_header_buffer_size 默认值：client_header_buffer_size 1k

large_client_header_buffers默认值 ：large_client_header_buffers 4 4k/8k

由于jsonp跨域请求只能通过get请求，url长度根据浏览器及服务器的不同而有不同限制。
若要支持IE的话，url长度限制为2083字符，若是中文字符的话只有2083/9=231个字符。
若是Chrome浏览器支持的最大中文字符只有8182/9=909个。
```