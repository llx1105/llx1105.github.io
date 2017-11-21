---
layout: post
title: Angular directive scope
category: 技术
tags: tech
keywords: Angularjs
---

## 么是隔离

   AngularJS 的 directive 默认能共享父 scope 中定义的属性，例如在模版中直接使用父 scope 中的对象和属性。
通常使用这种直接共享的方式可以实现一些简单的 directive 功能。当你需要创建一个可重复使用的 directive，只是偶尔需要访问或者修改父 scope 的数据，就需要使用隔离 scope。
当使用隔离 scope 的时候，directive 会创建一个没有依赖父 scope 的 scope，并提供一些访问父 scope 的方式.

## 什么使用隔离scope

   当想要写一个可重复使用的 directive，不能再依赖父 scope，这时候就需要使用隔离 scope 代替。共享 scope 可以直接共享父 scope，而隔离 scope 无法共享父scope。下图解释了共享 scope 和隔离 scope 的区别：


![1](/public/img/posts/scope.png)

例子：

示例可看：

共享 scope

使用共享 scope 的时候，可以直接从父 scope 中共享属性。因此下面示例可以将name属性的值输出出来。使用的是父 scope 中定义的值。

js代码：

```
app.controller("myController", function ($scope) {
  $scope.name = "hello world";
  }).directive("shareDirective", function () {
  return {
    template: 'Say:{{name}}'
  }
  });
```

html代码

```
<div ng-controller="myController">
<div share-directive=""></div>
</div>
```

输出结果：

Say:hello world

隔离 scope:

使用隔离 scope 的时候，无法从父 scope 中共享属性。因此下面示例无法输出父 scope 中定义的 name 属性值。

js代码：

```
app.controller("myController", function ($scope) {
  $scope.name = "hello world";
  }).directive("isolatedDirective", function () {
  return {
    scope: {},
    template: 'Say:{{name}}'
   }
});
```
html代码：

```
<div ng-controller="myController">
<div isolated-directive=""></div>
</div>
```

输出结果：

Say:

可以看出共享 scope 允许从父 scope 渗入到 directive 中，而隔离 scope 不能，在隔离 scope 下，给 directive 创造了一堵墙，使得父 scope 无法渗入到 directive 中。

## 建隔离 scope

在 Directive 中创建隔离 scope 很简单，只需要定义一个 scope 属性即可，这样，这个 directive 的 scope 将会创建一个新的 scope，如果多个 directive 定义在同一个元素上，只会创建一个新的 scope。

## 离 scope 与父scope交互

directive 在使用隔离 scope 的时候，提供了三种方法同隔离之外的地方交互。这三种分别是

@ 绑定一个局部 scope 属性到当前 dom 节点的属性值。结果总是一个字符串，因为 dom 属性是字符串。
& 提供一种方式执行一个表达式在父 scope 的上下文中。如果没有指定 attr 名称，则属性名称为相同的本地名称。
= 通过 directive 的 attr 属性的值在局部 scope 的属性和父 scope 属性名之间建立双向绑定。

###  局部 scope 属性

@ 方式局部属性用来访问 directive 外部环境定义的字符串值，主要是通过 directive 所在的标签属性绑定外部字符串值。这种绑定是单向的，即父 scope 的绑定变化，
directive 中的 scope 的属性会同步变化，而隔离 scope 中的绑定变化，父 scope 是不知道的。

如下示例：directive 声明未隔离 scope 类型，并且使用@绑定 name 属性，在 directive 中使用 name 属性绑定父 scope 中的属性。当改变父 scope 中属性的值的时候，directive 会同步更新值，当改变 directive 的 scope 的属性值时，父 scope 无法同步更新值。

js 代码：

```
app.controller("myController", function ($scope) {
    $scope.name = "hello world";
    }).directive("isolatedDirective", function () {
      return {
    scope: {
    name: "@"
 },
    template: 'Say：{{name}} <br>改变隔离scope的name：<input type="buttom" value="" ng-model="name" class="ng-pristine ng-valid">'
 }
})
```
html 代码：

```
<div ng-controller="myController">
  <div class="result">
    <div>父scope：
      <div>Say：{{name}}<br>改变父scope的name：<input type="text" value="" ng-model="name"/></div>
    </div>
    <div>隔离scope：
      <div isolated-directive name="{{name}}"></div>
    </div>
    <div>隔离scope（不使用{{name}}）：
      <div isolated-directive name="name"></div>
    </div>
  </div>
```

这里有个例子:
[the plunker demo](http://plnkr.co/edit/0vwa72InAuAACTBKykAa)

###  局部 scope 属性

= 通过 directive 的 attr 属性的值在局部 scope 的属性和父 scope 属性名之间建立双向绑定。
意思是，当你想要一个双向绑定的属性的时候，你可以使用=来引入外部属性。无论是改变父 scope 还是隔离 scope 里的属性，父 scope 和隔离 scope 都会同时更新属性值，因为它们是双向绑定的关系。

例子同上。把@改成=号即可。

###  局部 scope 属性

& 方式提供一种途经是 directive 能在父 scope 的上下文中执行一个表达式。此表达式可以是一个 function。
比如当你写了一个 directive，当用户点击按钮时，directive 想要通知 controller，controller 无法知道 directive 中发生了什么，也许你可以通过使用 angular 中的 event 广播来做到，但是必须要在 controller 中增加一个事件监听方法。
最好的方法就是让 directive 可以通过一个父 scope 中的 function，当 directive 中有什么动作需要更新到父 scope 中的时候，可以在父 scope 上下文中执行一段代码或者一个函数.

例子：

如下示例在 directive 中执行父 scope 的 function。

js代码：

```
app.controller("myController", function ($scope) {
    $scope.value = "hello world";
    $scope.click = function () {
       $scope.value = Math.random();
    };
    }).directive("isolatedDirective", function () {
      return {
        scope: {
          action: "&"
        },
        template: '<input type="button" value="在directive中执行父scope定义的方法" ng-click="action()"/>'
      }
})
```

html 代码：

```
<div  ng-controller="myController">
  <div>父scope：
    <div>Say：{{value}}</div>
  </div>
  <div>隔离scope：
    <div isolated-directive action="click()"></div>
  </div>
</div>
```


