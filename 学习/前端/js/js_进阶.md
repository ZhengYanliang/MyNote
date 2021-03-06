##js浮点数精度问题
```
大多数语言在处理浮点数的时候都会遇到精度问题，但是在JS里似乎特别严重，来看一个例子
alert(45.6*13);
结果居然是592.800000000001，当然加法之类的也会有这个问题
那这是js的错误吗？
当然不是，你的电脑做着正确的二进制浮点运算，但问题是你输入的是十进制的数，电脑以二进制运算，这两者并不是总是转化那么好的，有时候会得到正确的结果，但有时候就不那么幸运了
alert(0.7+0.1);//输出0.7999999999999999
alert(0.6+0.2);//输出0.8


你输入两个十进制数，转化为二进制运算过后再转化回来，在转化过程中自然会有损失了
但一般的损失往往在乘除运算中比较多，而JS在简单的加减法里也会出现这类问题，你也看到了，这个误差也是非常小的，但是却是不该出现的
那该怎么解决呢，ECMA4似乎给了解决方法，但是现在倒不是那么实用的
一种方法，比如0.7+0.1，先把0.1和0.7都乘10，加完之后再除10
另外可以自己写点函数来解决这个问题，自己百度谷歌一下应该有很多，但是最好还是不要用JS做一些复杂的浮点运算，毕竟JS更多的作用不在于此
 
  <script type="text/javascript">


    // 两个浮点数求和
    function accAdd(num1,num2){
       var r1,r2,m;
       try{
           r1 = num1.toString().split('.')[1].length;
       }catch(e){
           r1 = 0;
       }
       try{
           r2=num2.toString().split(".")[1].length;
       }catch(e){
           r2=0;
       }
       m=Math.pow(10,Math.max(r1,r2));
       // return (num1*m+num2*m)/m;
       return Math.round(num1*m+num2*m)/m;
    }
    
    // 两个浮点数相减
    function accSub(num1,num2){
       var r1,r2,m;
       try{
           r1 = num1.toString().split('.')[1].length;
       }catch(e){
           r1 = 0;
       }
       try{
           r2=num2.toString().split(".")[1].length;
       }catch(e){
           r2=0;
       }
       m=Math.pow(10,Math.max(r1,r2));
       n=(r1>=r2)?r1:r2;
       return (Math.round(num1*m-num2*m)/m).toFixed(n);
    }
    // 两数相除
    function accDiv(num1,num2){
       var t1,t2,r1,r2;
       try{
           t1 = num1.toString().split('.')[1].length;
       }catch(e){
           t1 = 0;
       }
       try{
           t2=num2.toString().split(".")[1].length;
       }catch(e){
           t2=0;
       }
       r1=Number(num1.toString().replace(".",""));
       r2=Number(num2.toString().replace(".",""));
       return (r1/r2)*Math.pow(10,t2-t1);
    }
    
    function accMul(num1,num2){
       var m=0,s1=num1.toString(),s2=num2.toString(); 
    try{m+=s1.split(".")[1].length}catch(e){};
    try{m+=s2.split(".")[1].length}catch(e){};
    return Number(s1.replace(".",""))*Number(s2.replace(".",""))/Math.pow(10,m);
    }
    
  </script>
  
    <script>
    document.write("使用js原生态方法");
    document.write("<br/> 1.01 + 1.02 ="+(1.01 + 1.02));
    document.write("<br/> 1.01 - 1.02 ="+(1.01 - 1.02));
    document.write("<br/> 0.000001 / 0.0001 ="+(0.000001 / 0.0001));
    document.write("<br/> 0.012345 * 0.000001 ="+(0.012345 * 0.000001));
    document.write("<br/><hr/>");
    document.write("<br/>使用自定义方法");
    document.write("<br/> 1.01 + 1.02 ="+accAdd(1.01,1.02));
    document.write("<br/> 1.01 - 1.02 ="+accSub(1.01,1.02));
    document.write("<br/> 0.000001 / 0.0001 ="+accDiv(0.000001,0.0001));
    document.write("<br/> 0.012345 * 0.000001 ="+accMul(0.012345,0.000001));
    </script>
```
##js精度改进
```
上面的方法实在是不靠谱，有的可以，有的不可以。比如98.3/1就不可以。


http://div.io/topic/1349

0.1 + 0.2 =？
0.1 + 0.2 = 0.3？


我们先来看一段 JS。


console.log( 0.1+ 0.2);


输出为 0.30000000000000004。是不是很奇葩
其实对于浮点数的四则运算，几乎所有的编程语言都会有类似精度误差的问题，只不过在 C++/C#/Java 这些语言中已经封装好了方法来避免精度的问题，而 JavaScript 是一门弱类型的语言，从设计思想上就没有对浮点数有个严格的数据类型，所以精度误差的问题就显得格外突出。下面就分析下为什么会有这个精度误差，以及怎样修复这个误差。


首先，我们要站在计算机的角度思考 0.1 + 0.2 这个看似小儿科的问题。我们知道，能被计算机读懂的是二进制，而不是十进制，所以我们先把 0.1 和 0.2 转换成二进制看看：


0.1 => 0.0001 1001 1001 1001…（无限循环）
0.2 => 0.0011 0011 0011 0011…（无限循环）


双精度浮点数的小数部分最多支持 52 位，所以两者相加之后得到这么一串 0.0100110011001100110011001100110011001100110011001100 因浮点数小数位的限制而截断的二进制数字，这时候，我们再把它转换为十进制，就成了 0.30000000000000004。


原来如此，那怎么解决这个问题呢？我想要的结果就是 0.1 + 0.2 === 0.3 啊！！！


有种最简单的解决方案，就是给出明确的精度要求，在返回值的过程中，计算机会自动四舍五入，比如：


var numA = 0.1;
var numB = 0.2;
alert( parseFloat((numA + numB).toFixed(2)) === 0.3 );


但是明显这不是一劳永逸的方法，


多次计算后，我发现整数运算没有这个问题
对每个小数乘以10的N次方，计算完成后再除以10的N次方


var formatFloat = function(num, digit) {
    var m = Math.pow(10, Math.abs(digit));
    if (digit < 0) {
        return num / m;
    } else if (digit > 0) {
        return num * m;
    } else {
        return num;
    }
}
//alert(formatFloat(formatFloat(0.1, 1) + formatFloat(0.2, 1), -1));
//alert(formatFloat(formatFloat(29.7, 1) / 2,-1));
//alert(formatFloat(formatFloat(98.3, 1) / 1,-1));
//alert(formatFloat(formatFloat(10, 1) / 3.3,-1));
//alert(formatFloat(formatFloat(6.36, 1) / 3,-1));
//alert(0.1+0.2)
//alert(0.1*0.2)
//alert(formatFloat(formatFloat(0.1, 1) * formatFloat(0.2, 1), -2));
//alert(formatFloat(formatFloat(0.1, 1) + formatFloat(0.2, 1), -1));
//alert(formatFloat(formatFloat(5000, 1) / 10000,-1));
//alert(formatFloat(formatFloat(15.36, 1) / 0.3,-1));
//alert(formatFloat(formatFloat(15.36, 1) * 0.3,-1));
alert(formatFloat(formatFloat(4.608, 1) / 0.3,-1));//15.36
alert(formatFloat(formatFloat(4.608, 1) / formatFloat(0.3, 1)),-2);//15.36
```
##Javascript 浮点运算问题分析与解决
```
分析
JavaScript 只有一种数字类型 Number ，而且在Javascript中所有的数字都是以IEEE-754标准格式表示的。 浮点数的精度问题不是JavaScript特有的，因为有些小数以二进制表示位数是无穷的：


十进制           二进制
0.1              0.0001 1001 1001 1001 ...
0.2              0.0011 0011 0011 0011 ...
0.3              0.0100 1100 1100 1100 ...
0.4              0.0110 0110 0110 0110 ...
0.5              0.1
0.6              0.1001 1001 1001 1001 ...
所以比如 1.1 ，其程序实际上无法真正的表示 ‘1.1’，而只能做到一定程度上的准确,这是无法避免的精度丢失：


1.09999999999999999
在JavaScript中问题还要复杂些，这里只给一些在Chrome中测试数据：


 输入               输出
1.0-0.9 == 0.1     False
1.0-0.8 == 0.2     False
1.0-0.7 == 0.3     False
1.0-0.6 == 0.4     True
1.0-0.5 == 0.5     True
1.0-0.4 == 0.6     True
1.0-0.3 == 0.7     True
1.0-0.2 == 0.8     True
1.0-0.1 == 0.9     True
解决
那如何来避免这类 ` 1.0-0.9 != 0.1 ` 的非bug型问题发生呢？下面给出一种目前用的比较多的解决方案, 在判断浮点运算结果前对计算结果进行精度缩小，因为在精度缩小的过程总会自动四舍五入:


(1.0-0.9).toFixed(digits)                   // toFixed() 精度参数须在 0 与20 之间
parseFloat((1.0-0.9).toFixed(10)) === 0.1   // 结果为True
parseFloat((1.0-0.8).toFixed(10)) === 0.2   // 结果为True
parseFloat((1.0-0.7).toFixed(10)) === 0.3   // 结果为True
parseFloat((11.0-11.8).toFixed(10)) === -0.8   // 结果为True
方法提炼
// 通过isEqual工具方法判断数值是否相等
function isEqual(number1, number2, digits){
digits = digits == undefined? 10: digits; // 默认精度为10
return number1.toFixed(digits) === number2.toFixed(digits);
}


isEqual(1.0-0.7, 0.3);  // return true


// 原生扩展方式，更喜欢面向对象的风格
Number.prototype.isEqual = function(number, digits){
digits = digits == undefined? 10: digits; // 默认精度为10
return this.toFixed(digits) === number.toFixed(digits);
}


(1.0-0.7).isEqual(0.3); // return true
```
##js关于小数点失精算法修正0.07*100竟然=7.000000000000001
```
// 关于js失精算法你都遇到哪些，让我们一起来细数一下吧  
console.log(0.07*100); // 7.000000000000001  
console.log(0.1+0.2); // 0.30000000000000004 

wiki:https://en.wikipedia.org/wiki/IEEE_floating_point#Basic_formats
oracle失精算法专栏：http://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html
 
解决方案：
如果是取整数，我们可以这样：
Math.round((0.07*100))  
 
需要保留小数的，我们可以这样：
parseFloat((0.07*100).toPrecision(12)) // = 7  
parseFloat((0.01+0.02).toPrecision(12)) // = 0.03  
```

