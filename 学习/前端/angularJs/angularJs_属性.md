##$.scope
```


Scope(作用域) 是应用在 HTML (视图) 和 JavaScript (控制器)之间的纽带。

Scope 是一个对象，有可用的方法和属性。

Scope 可应用在视图和控制器上。 如何使用 Scope

当你在 AngularJS 创建控制器时，你可以将  $scope  对象当作一个参数传递:
AngularJS 实例

控制器中的属性对应了视图上的属性：
< div   ng-app= "myApp"   ng-controller= "myCtrl" >

< h1 > {{carname}} < /h1 >

< /div >

< script >

var  app = angular.module( 'myApp' , []);

app.controller( 'myCtrl' ,  function ($scope) {
    $scope.carname =  "Volvo" ;
});

< /script >

来源：  http://www.runoob.com/angularjs/angularjs-scopes.html
```