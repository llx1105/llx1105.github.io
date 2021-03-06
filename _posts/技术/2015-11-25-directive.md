---
layout: post
title: Angular directive
category: 技术
tags: tech
keywords: Angularjs
---

## 概述

  指令是Angular的一个特殊标志，也是有别于其他框架的一个重要特征，同时也是另我这个新手有点头疼的部分。不过，Angular之所以功能强大，
在很大程度上得益于它拥有大量的内置的指令，也能通过语法去自定义指令去去实现自带指令所没有覆盖的地方。可以说指令是开发Angular应用
时一个非常重要的工具，尤其是自定义的指令。

  从字面意义来说，directive是一种执行的信号，一旦发布了这个指令，就要执行某项动作。比如在HTML中，书写一个a标记，实质上也是一个指令，
告知浏览器要创建一个超链接。在Angular中，指令要复杂许多，它不仅要创建元素，还要给元素附加一些特定的行为，可以理解Angular的指令是一个
在特定的DOM元素上执行的函数，即对DOM的操作。


## directive基础

  定义一个directive很简单，只需要调用directive方法即可，该方法可以接受两个参数，一个例子：

```
 <body>
   <div id="frame">
     <div hello> </div>
     <div class="hello"> </div>
   </div>
 </body>

 app.directive('hello',function(){
  return {
   restrict: 'EAC',
   template: '<h3>hello,angular</h3>',
   }
 })

```

  上面是一个简单的例子，名字叫做hello，命名时为了避免与原生指令冲突，我们不要使用ng开头的作为名字。directive方法的第二个参数是一个函数，
该函数返回一个对象，在对象中，设置指令需要执行的全部动作

## directive属性

在这个例子中添加了两个简单的属性，其中
### restrict
  该属性指出在HTML中得使用方式，共有E 标签，A 属性，C class，M 注释（不常用）等四种，属性值可以是单个也可以多个字母组合，该属性的默认值是A。

### template
  表示指令在编译和链接后生成的HTML标记，可以是string，可以是复杂的字符集合，还可以是包含大括号，动态获取变量的值。如果是一个内容模板，则可以将template
属性改为templateUrl。

### replace
  属性为布尔类型，默认值为false，为true时表示将模板中得内容替换为指令内标记,此时，无论指令标记中是否存在内容，都将被清空；当属性值为false时，表示不替换指令标记，而是将内容插入到指令标记中。

### templateUrl
  属性的值是一个URL地址，该地址将指向一个模板页面，该属性与template属性不能同时使用，只能取其中之一。

### transclude
  布尔值，默认为false，设置为true时则开启了该属性，当开启了transclude属性后，就可以在模板中通过ng-transclude方式替换指令元素中得内容。我们可以将整个模板，包括其中的指令通过嵌入式全部传入一个指令中。这样做可以将任意内容和作用域传递给指令。

  假设我们有这样一段代码:

  ```
  <div my-directive>
    <button>some button</button>
    <a href="#">and a link</a>
  </div>

  ```
  然后不同的directive的内容会有不同的结果，当directive如下没有添加transclude时：

  ```
  app.directive('myDirective', function(){
      return{
        template: '<div class="something"> This is my directive content</div>'
      }
  });

  ```
  html是这样:

  ```
  <div my-directive>
    <div class="something"> 
      This is my directive content
    </div>
  </div>
  ```

  当directive加上了transclude为true，且在div中加入ng-transclude时：

  ```
  app.directive('myDirective', function(){
      return{
      transclude: true，
        template: '<div class="something" ng-transclude> This is my directive content</div>'
      }
  });

  ```
  html是这样的：

  ```
  <div my-directive=""><div class="something" ng-transclude="">
      <button class="ng-scope">some button</button>
      <a href="#" class="ng-scope">and a link</a>
  </div></div>
  ```

  以上的例子可以很好地理解transclude的作用。


### link
  link属性的值是一个函数，在该函数中可以操控DOM元素对象，包括绑定元素的各类事件，定义事件触发时执行的内容，函数定义的代码如下：

```
  link:function(scope, iEle, iAttrs, controller){
     ...
  }

```

link函数包含3个主要参数，其中scope表示指令所在的作用域，它的功能与页面中控制器注入的作用域是相同的，iEle参数表示指令中的元素，该元素可以通过Angular内部封装的jqLite
框架进行调用，语法与jQuery相同。此外，iAtrrs参数表示指令元素的属性集合，通过这个参数可以获取元素中得各类属性。controller参数指向require选项定义的控制器。如果没有设置require选项，controller的值为undefined

### compile
  这个属性使用的没有link那么多，该属性返回一个函数或者对象，当返回一个函数时，该函数的名称为post，而当返回一个对象时，对象中则包含两个名为 pre 和 post的方法函数。当添加compile属性后就不能添加link属性了，因为其实无论compile属性返回的是一个函数还是一个对象，实际上都创建了一个名为post的链接函数。因此在编译过程中会自动忽略其他的链接函数。
  下面有个例子让我们更好地理解compile函数的执行顺序。

```
<div id="test">
  <ts-a>
    <ts-a>
    </ts-b>
  </ts-b>
</div>

.directive('tsA',function(){
  return {
    restrict: 'EAC',
    compile: function (ele,attrs, trans) {
     console.log('正在编译A指令');
       return {
        pre: function (scope, ele, attrs){
         console.log('正在执行A指令中的pre函数');
        },
        post: function (scope, ele, attrs){
         console.log('正在执行A指令中的post函数');
        },
       }
    }
  }
}
.directive('tsB',function(){
  return {
    restrict: 'EAC',
    compile: function (ele,attrs, trans) {
     console.log('正在编译B指令');
       return {
        pre: function (scope, ele, attrs){
         console.log('正在执行B指令中的pre函数');
        },
        post: function (scope, ele, attrs){
         console.log('正在执行B指令中的post函数');
        },
       }
    }
  }
})

)

```

### require
  该属性可以为字符串或数组，字符串代表另外一个指令的名字:

  require参数的值可以用下面的前缀进行修饰，这会改变查找控制器的行为：

  ? 如果在当前指令中没有找到所需的控制器，会将null作为传给link函数的第四个参数。

  ^ 如果添加了^前缀，指令会在上游的指令链中查找require参数所指定的控制器。

  ?^ 将前面两个选项的行为组合起来。

  没有前缀，如果没有前缀，指令将会在自身所提供的控制器中进行查找，如果没有找到任何控制（或具有指定名字的指令）就抛出一个错误。


```
最终打出的日志会是：

正在编译A指令

正在编译B指令

正在执行A指令中的pre函数

正在执行B指令中的pre函数

正在执行B指令中的post函数

正在执行A指令中的post函数
```


to be continued...
