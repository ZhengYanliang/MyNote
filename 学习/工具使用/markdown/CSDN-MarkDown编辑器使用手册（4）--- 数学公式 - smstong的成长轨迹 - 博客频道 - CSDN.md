
输入数学公式
数学公式的web解决方案
MathJax渲染过程简单模拟
1 MathJax最简示例
2 模拟MathJax渲染原理

CSDN-MarkDown编辑器常用数学公式输入教程
1 公式定界符与关键字
2 上下标
3 括号和分隔符
4 分数
5 开方
6 省略号
7 矢量
8 积分
9 极限
10 累加累乘
11 希腊字母
12 数学符号大汇总
13 需要转义的字符
14 使用指定字体




1 数学公式的web解决方案

在网页上显示漂亮的数学公式，是多年来数学工作者和学者的愿望。最容易实现的方式就是使用离线编辑器如word，Latex等编写完公式，然后截图作为图片在html网页中显示。然而这种方式存在很多缺点：
无法在线修改，离线修改后必须重新截图
放大显示会失真，这是位图的天生缺陷
不同的离线编辑器生成的显示效果不同，很难统一
由于无法直接编辑，所以即使看到了公式，也无法在此基础上进一步修改，不利于交流


当然，位图显示公式也有一个最大的优点，那就是兼容所有浏览器，不需要任何插件就可以浏览。

随着html, css的持续发展，使用纯html+css来显示公式已经非常可行，于是大名鼎鼎的 MathJax 出现了。它是一个开源的JavaScript库，用来把特定格式的公式描述转换为html+css或者svg代码，从而在浏览器上显示数学公式。 2 MathJax渲染过程简单模拟 2.1 MathJax最简示例

先来看一个带公式的最简网页实例mathjax.html。
<!DOCTYPE html>
<html>
    <head>
        <title>MathJax TeX Test Page</title>
        <script type="text/x-mathjax-config">
            MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
        </script>
        <script type="text/javascript"
            src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
            </script>
        </head>
        <body>
            When $a \ne 0$, there are two solutions to \(ax^2 + bx + c = 0\) and they are
            $$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$
        </body>
    </html>

在浏览器中打开mathjax.html，会显示如下图：
<img src="images/img1.png" height="124" width="730">


2.2 模拟MathJax渲染原理

从前面的例子可以看出，MathJax中数学公式是用一些特殊字符串表示的，这些字符串被特定的边界 $ $ 和 $$ $$ 包围。然后MathJax引擎会根据边界提取公式表达式，然后把它们替换成用户显示公式的html+css代码。

下面我们来模拟这一过程。用math.js模拟MathJax.js，如下所示：
window.onload = function()
{
    var body = document.getElementsByTagName('body')[0];
    var oldBody = body.innerHTML;
    var newBody = oldBody.replace(/[^$]\$([^$]+)\$[^$]/g, function(str, r1){
            return MathJax_inline(r1);
            });
    newBody = newBody.replace(/\$\$([^$]+)\$\$/g, function(str, r1){
            return MathJax_block(r1);
            });
    body.innerHTML  = newBody;
}

// 把公式内容描述转换为显示描述
function MathJax_inline(r1)
{
    return '<span style="color:red">' + r1 + '</span>';
}

function MathJax_block(r1)
{
    return '<div style="color:red">' + r1 + '</div>';
}

html页面相应修改：
<!DOCTYPE html>
<html>
    <head>
        <title>MathJax TeX Test Page</title>
        <script type="text/javascript" src="math.js"></script>
    </head>
    <body>
        When $a \ne 0$, there are two solutions to $ax^2 + bx + c = 0$ and they are
        $$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$
    </body>
</html>

来看看效果：
<img src="images/img2.png" height="124" width="730">

虽然没有正确显示出公式，但是已经识别出了公式边界，并把公式部分用红色显示出来。真正的MathJax是把公式表达式替换成显示公式的html代码，而不是简单的设置为红色，但是这其中的处理原理是一致的。 

3 CSDN-MarkDown编辑器常用数学公式输入教程

MathJax支持多种公式输入输出规范，输入格式可以是MathML, TeX，ASCIImath中的任何一种，输出格式可以是html+css，或svg，或MathML。下面仅对最常用的Tex输入规范进行说明。 3.1 公式定界符与关键字

CSDN-MarkDown编辑器使用的公式定界符为 $ 和 $$ ，单美元符号包围的是行内公式，双美元符号包围的是块公式。
Tex关键字（字符转义序列）表示特殊显示符号，如\frac表示分数，其后面可以跟随参数，参数多少与关键字有关。 3.2 上下标

^表示上标，_表示下标，如果上（下）标内容多于一个字符就需要使用{}， 注意不是( ), 因为( )经常是公式本身组成部分，为避免冲突，所以选用了{ } 将其包起来。

示例： $x^{y^z}=(1+e^x)^{-2xy^w}$

效果： x y z = ( 1 + e x ) − 2 x y w

上面输入的上下标都是在字符的右侧，要想在左侧或者两侧都写上下标，那么需要使用\sideset语法。

示例： $\sideset{^1_2}{^3_4}\bigotimes$

效果： ⨂ 1 2 ⨂ 3 4 3.3 括号和分隔符

( )和[ ]就是自身了，由于{ } 是Tex的元字符，所以表示它自身时需要转义。

示例： $f(x,y) = x^2 + y^2,  x\epsilon[0,100]$

效果： f ( x , y ) = x 2 + y 2 , x ϵ [ 0 , 100 ] ， y = { 3 , 4 , 5 }

有时候括号需要大号的，普通括号不好看，此时需要使用\left和\right加大括号的大小。

示例： $(\frac{x}{y})^8$，$\left(\frac{x}{y}\right)^8$

效果： ( x y ) 8 ， ( x y ) 8

\left和\right必须 成对 出现，对于不显示的一边可以使用 . 代替。

示例： $\left.\frac{{\rm d}u}{{\rm d}x} \right| _{x=0}$

效果： d u d x ∣ ∣ x = 0 3.4 分数

使用\frac{分子}{分母}格式，或者 分子\over 分母。

示例： $\frac{1}{2x+1}$ 或者 $1\over{2x+1}$

效果： 1 2 x + 1 或者 1 2 x + 1 3.5 开方

示例： $\sqrt[9]{3}$ 和 $\sqrt{3}$

效果： 3 √ 9 和 3 √ 3.6 省略号

有两种省略号，\ldots 表示语文本底线对其的省略号，\cdots表示与文本中线对其的省略号。

示例： $f(x_1, x_2, \ldots, x_n)=x_1^2 + x_2^2+ \cdots + x_n^2$

效果： f ( x 1 , x 2 , … , x n ) = x 2 1 + x 2 2 + ⋯ + x 2 n 3.7 矢量

示例： $\vec{a} \cdot \vec{b}=0$

效果: a ⃗  ⋅ b ⃗  = 0 3.8 积分

示例： $\int_0^1x^2{\rm d}x $

效果： ∫ 1 0 x 2 d x 3.9 极限

示例： $\lim_{n\rightarrow+\infty}\frac{1}{n(n+1)}$

效果： lim n → + ∞ 1 n ( n + 1 ) 3.10 累加、累乘

示例： $\sum_1^n\frac{1}{x^2}$， $\prod_{i=0}^n\frac{1}{x^2}$

效果： ∑ n 1 1 x 2 ， ∏ n i = 0 1 x 2 3.11 希腊字母

希腊字符示例： $$\alpha　A　\beta　B　\gamma　\Gamma　\delta　\Delta　\epsilon　E \varepsilon　　\zeta　Z　\eta　H　\theta　\Theta　\vartheta \iota　I　\kappa　K　\lambda　\Lambda　\mu　M　\nu　N \xi　\Xi　o　O　\pi　\Pi　\varpi　　\rho　P \varrho　　\sigma　\Sigma　\varsigma　　\tau　T　\upsilon　\Upsilon \phi　\Phi　\varphi　　\chi　X　\psi　\Psi　\omega　\Omega$$

效果：
α 　 A 　 β 　 B 　 γ 　 Γ 　 δ 　 Δ 　 ϵ 　 E ε 　 　 ζ 　 Z 　 η 　 H 　 θ 　 Θ 　 ϑ ι 　 I 　 κ 　 K 　 λ 　 Λ 　 μ 　 M 　 ν 　 N ξ 　 Ξ 　 o 　 O 　 π 　 Π 　 ϖ 　 　 ρ 　 P ϱ 　 　 σ 　 Σ 　 ς 　 　 τ 　 T 　 υ 　 Υ ϕ 　 Φ 　 φ 　 　 χ 　 X 　 ψ 　 Ψ 　 ω 　 Ω

3.12 数学符号大汇总

± ：\pm
× ：\times
÷ ：\div
∣ ：\mid

⋅ ：\cdot
∘ ：\circ
∗ : \ast
⨀ ：\bigodot
⨂ ：\bigotimes
⨁ ：\bigoplus
≤ ：\leq
≥ ：\geq
≠ ：\neq
≈ ：\approx
≡ ：\equiv
∑ ：\sum
∏ ：\prod
∐ ：\coprod

集合运算符：
∅ ：\emptyset
∈ ：\in
∉ ：\notin
⊂ ：\subset
⊃ ：\supset
⊆ ：\subseteq
⊇ ：\supseteq
⋂ ：\bigcap
⋃ ：\bigcup
⋁ ：\bigvee
⋀ ：\bigwedge
⨄ ：\biguplus
⨆ ：\bigsqcup

对数运算符：
log ：\log
lg ：\lg
ln ：\ln

三角运算符：
⊥ ：\bot
∠ ：\angle
30 ∘ ：30^\circ
sin ：\sin
cos ：\cos
tan ：\tan
cot ：\cot
sec ：\sec
csc ：\csc

微积分运算符：
y ′ x ：\prime
∫ ：\int
∬ ：\iint
∭ ：\iiint
∬∬ ：\iiiint
∮ ：\oint
lim ：\lim
∞ ：\infty
∇ ：\nabla

逻辑运算符：
∵ ：\because
∴ ：\therefore
∀ ：\forall
∃ ：\exists
≠ ：\not=
≯ ：\not>
⊄ ：\not\subset

戴帽符号：
y ^ ：\hat{y}
\check{y} y ˇ y ˇ ：\check{y}
y ˘ ：\breve{y}

连线符号：
a + b + c + d ¯ ¯ ¯ ¯ ¯ ¯ ¯ ¯ ¯ ¯ ¯ ¯ ¯ ¯ ¯ ¯ ¯ ¯ ：\overline{a+b+c+d}
a + b + c + d − − − − − − − − − − ：\underline{a+b+c+d}
a + b + c      1.0 + d                2.0 ：\overbrace{a+\underbrace{b+c}_{1.0}+d}^{2.0}

箭头符号：
↑ ：\uparrow
↓ ：\downarrow
⇑ ：\Uparrow
⇓ ：\Downarrow
→ ：\rightarrow
← ：\leftarrow
⇒ ：\Rightarrow
⇐ ：\Leftarrow
⟶ ：\longrightarrow
⟵ ：\longleftarrow
⟹ ：\Longrightarrow
⟸ ：\Longleftarrow 3.13 需要转义的字符

要输出字符　空格　#　$　%　&　_　{　}　，用命令：　\空格　#　\$　\%　\&　_　{　} 3.14 使用指定字体

{\rm text}如：
使用罗马字体： t e x t ${\rm text}$
其他的字体还有：
\rm　　罗马体　　　　　　　\it　　意大利体
\bf　　黑体　　　　　　　　\cal 　花体
\sl　　倾斜体　　　　　　　\sf　　等线体
\mit 　数学斜体　　　　　　\tt　　打字机字体
\sc　　小体大写字母