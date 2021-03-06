

##ng-repeat

``` 重复 HTML 元素

ng-repeat  指令会重复一个 HTML 元素：
AngularJS 实例
<div ng-app="" ng-init="names=['Jani','Hege','Kai']">
  <p>使用 ng-repeat 来循环数组</p>
  <ul>
    <li ng-repeat="x in names">
      {{ x }}
    </li>
  </ul>
</div>

来源：  http://www.runoob.com/angularjs/angularjs-directives.html
实例二:


ng-repeat  指令用在一个对象数组上：
AngularJS 实例
<div ng-app="" ng-init="names=[
{name:'Jani',country:'Norway'},
{name:'Hege',country:'Sweden'},
{name:'Kai',country:'Denmark'}]">

<p>循环对象：</p>
<ul>
  <li ng-repeat="x	in names">
    {{ x.name + ', ' + x.country }}
  </li>
</ul>

</div>

```

##ng-module

``` AngularJS 指令

AngularJS 指令是扩展的 HTML 属性，带有前缀  ng- 。

ng-app  指令初始化一个 AngularJS 应用程序。

ng-init  指令初始化应用程序数据。

ng-model  指令把元素值（比如输入域的值）绑定到应用程序。

完整的指令内容可以参阅  AngularJS 参考手册 。
实例一:
<div ng-app="" ng-init="firstName='John'">

 	<p>在输入框中尝试输入：</p>
 	<p>姓名：<input type="text" ng-model="firstName"></p>
 	<p>你输入的为： {{ firstName }}</p>

</div>

实例二:
<div ng-app="" ng-init="quantity=1;price=5">

<h2>价格计算器</h2>

数量： <input type="number"	ng-model="quantity">
价格： <input type="number" ng-model="price">

<p><b>总价：</b> {{ quantity * price }}</p>

</div>


```

##ng-init

``` 定义和用法

ng-init  指令执行给定的表达式。

ng-init  指令添加一些不必要的逻辑到 scope 中，建议你可以在控制器中  ng-controller  指令执行它 。

初始化应用时创建一个变量:
实例一:
< div   ng-app= ""   ng-init= "myText='Hello World!'" >

< h1 > {{myText}} < /h1 >


实例二:
<div ng-app="" ng-init="quantity=1;cost=5">

<p>总价： <span ng-bind="quantity * cost"></span></p>

</div>

实例三:
<div ng-app="" ng-init="person={firstName:'John',lastName:'Doe'}">

<p>姓为 {{ person.lastName }}</p>

</div>

实例四:
<div ng-app="" ng-init="person={firstName:'John',lastName:'Doe'}">

<p>姓为 <span ng-bind="person.lastName"></span></p>

</div>


```

## ng-controller

```



为应用变量添加控制器:
< div   ng-app= "myApp"   ng-controller= "myCtrl" >

Full Name:  {{firstName + " " + lastName}}

< /div >

< script >
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.firstName = "John";
    $scope.lastName = "Doe";
});
< /script >

来源：  http://www.runoob.com/angularjs/ng-ng-controller.html



```

##ng-change当输入框的值改变时执行函数:

```
< body   ng-app= "myApp" >

< div   ng-controller= "myCtrl" >
     < input   type= "text"   ng-change= "myFunc()"   ng-model= "myValue"   / >
     < p > The input field has changed  {{count}}  times. < /p >
< /div >

< script >

angular.module( 'myApp' , [])
.controller( 'myCtrl' , [ '$scope' ,  function ($scope) {
    $scope.count =  0 ;
    $scope.myFunc =  function () {
        $scope.count++;
    };
}]);

< /script >

< /body >

来源：  http://www.runoob.com/angularjs/ng-ng-change.html
``` ##ng-click用法
```


<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
</head>
<body ng-app="">


<p>点击按钮:</p>


<button ng-click="count = count + 1" ng-init="count=0">OK</button>


<p>按钮被点击了 {{count}} 次。</p>


<p>实例中，按钮每被点击一次变量 "count" 会自动加 1。</p>


</body>
</html>
```



