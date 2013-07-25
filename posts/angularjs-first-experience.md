---
title: angularjs first experience
date: '2013-07-12'
description: angularjs first experience, 一篇angular经验帖
categories: 'code/angularjs'
tags: [angularjs,experience,javascript]
author: Dejian Xu
---

### Summary
这是一篇经验帖。

项目使用angularjs重构功能代码，这个过程中遇到了一些问题，
也有jQuery使用者的思维习惯需要转变的地方，文中会列出相应的处理方法，
处理方法未必是最合适的，本文一并记录下来。

### 背景介绍
项目某功能基于jQuery, jQuery
plugin，外加纯手工代码过千行，希望通过引入angularjs，能减少代码量和使业务逻辑更加清晰。
该功能是一个传统CRUD类的事务，所以暂未使用angularjs的route。

### 经验总结

1. angular jqLite 与 jQuery

    虽说angular实现了一个简版jQuery，但是并不像你想的那样。
    没有css选择器，事件绑定只能使用bind，虽不够便利，但足够使用。

    ```
    angular.element(document.getElementById('id')).bind('change', function(){
    });
    ```

2. 载入数据
   - 原始方式  
     php直接将数据写到input上，或者php端循环生成html
   - angular方式  
     在相应控制器的构造函数中载入数据。

        ```
        app.controller('yourCtrl', ['$scope', '$http', function($scope, $http){
          $http.get('/path/to/your/data/api')
            .success(function(data) {
              $scope.data = info.data;
            });
        }]);
        ```

    在项目实现中，后台没有对应的api，为了简化工作，把数据以JSON形式写在了页面中。

    ```
    <script>
    window.mydata = {
      note: {$note|json_encode},
      other: ['blablabla']
    };
    </script>
    ```
    将数据保存在window名下，在需要使用的地方，通过$window来访问:

    ```
    app.controller('yourCtrl', ['$scope', '$window', function($scope, $window){
        $scope.data = $window.mydata;
    }]);
    ```

3. 数据双向绑定
   * 原始方式  
     `<input type="text" name="title" id="title" />`
   * angular方式  
     `<input type="text" ng-model="title" />`

    双向绑定是一个很好的编码体验。

    之前常使用jQuery选择器来定位DOM，然后处理数据。  
    在angular里则直接用ngModel进行绑定，model view 层数据同步发生改变，
    极大的减少这类js处理代码。

4. $watch的触发时机

    angular的$watch相当便利。它就像DOM上的onchange一样，但又有不同。
    它的触发时机就像ng-change一样，以改变的最小单位为触发单元。

    当你一个字符一个字符输入时，它会每个字符输入后触发一次。

    当你通过函数直接修改时，它可能只触发一次。

    在$watch语句被执行后，它马上触发一次，这时输入的两个参数完全一样，
    是对同一对象的引用，可以使用`===`判断。

    ```
    app.controller('demoCtrl', ['$scope', function($scope){
      $scope.title = 'hello';
      $scope.$watch('title', function(nv,ov){
        if (nv === ov) {
          return;
        }
        console.log(nv);
      });
    }]);
    ```

5. onChange 事件与 ng-change 事件

    如果习惯使用jQuery定位到input，然后绑定change事件，或者更原始的直接写属性onchange，
    那么这里将有一个蛮大的转变。
    * 原始方式  
        `<input onchange="change(this)" />`
    * angular方式  
        `<input ng-model="title" ng-change="change()" />`

    这里有几点需要注意：

    #### 事件触发时机不同

    HTML的onchange，是value发生`变化后`触发一次事件。
    angular的ng-change，是当value发生`变化时`，触发。
    最后一个输入字符是空格时，ng-change不会触发，input丢失焦点后onchange会触发。
    假设value原本为空，手工输入hello，onchange 触发一次，而ng-change触发5次。

    #### DOM与angular互访问题

    在`ng-change="change()"`这里不能使用`this`，如果习惯于在这里使用`this`来访问`input`的话，
    这里将有一个不适点，不过这里可以拿到`event`，像这样`ng-change="change($event)"`。

     > 如果您需要访问`input`的话，可以考虑使用`directive`。

     在DOM中访问angular，如果想在`onchange="change(this)"`这里访问angular的话，将不会那么便利，
     从DOM可以访问到DOM所属的scope，像这样`angular.element(this).scope()`，
     但是这里不可以直接对绑定的ng-model进行赋值操作：

     __对input的赋值，将不会保存到ng-model。__

     __对ng-model赋值也不会更新到input上。__

     DOM或者jQuery相应代码，属于angular运行周期外的代码，当修改scope内数据时，
     需要使用`scope.$apply()`将执行代码包裹起来。

     像这样：

     ```
     $('your input').change(function(){
       scope.$apply(function(){
         scope.title = 'new val';
       });
     });
     ```

6. $scope.$apply 调用的时机

    如果你还没弄明白angular的运行周期，多半会遇到这个问题。
    当你在错误的时机调用了`$apply`时，浏览器会给你一个
    `Error: $apply already in progress`。

    [官网文档](http://docs.angularjs.org/api/ng.$rootScope.Scope#$apply)有很明确的解释，
    > `$apply()` 是用来执行来自angular框架外的代码。(比如浏览器DOM事件，setTimeout，XHR或者第三方插件)。
    > 当外部代码执行进入angular框架内部时，我们需要让相关scope下的异常处理，监听等正常运转。

7. 如何取消$timeout任务

    ```
    var promise = $timeout(function(){
        // bla bla bla 
    });
    $timeout.cancel(promise);
    ```

8. $http 与 jQuery ajax

    两者都是使用Deferred机制来实现的。不过angular的不是一个完全的Deferred实现。
    在接口上，$http跟jQuery ajax有不小的差别，沿用jQuery的方式，肯定会发生错误。

    $http.post的第二个参数data如果是一个对象，它将会以json的方式发给服务端。

    ```
    $http.post('/path/to/api', {id:1, city:'beijing'})
    ```
    GET 方式访问 `/path/to/api?id=1&city=beijing`，要写成这样。

    ```
    $http.get('/path/to/api', {
        params: {id:1,city:'beijing'}
    })
    ```
    POST 方式访问 `/path/to/api`，发送数据`id=1&city=beijing`，
    POST方式以key=value&amp;这种字符串发送数据的形式，在angular中不被默认支持。
    需要指定headers和预先生成data字符串(比如使用`jQuery.param()`)才可以。

    ```
    $http.post('/path/to/api', 'id=1&city=beijing', {
        headers: {'Content-Type': 'application/x-www-form-urlencoded'}
    })
    ```

9. jQuery 插件改造

    jQuery插件的改造，建议使用`directive`来重写，或者用directive做成适配器来使用。
