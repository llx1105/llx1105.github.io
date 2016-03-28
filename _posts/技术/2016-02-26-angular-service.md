---
layout: post
title: Angular 服务
category: 技术
tags: tech
keywords: Angularjs service
---

服务是Angular的一个关键特性。在Angular中，服务是一种单例对象。所谓单例，就是服务在每一个应用中只会被实例化一次，并且是在需要时异步加载。
按照功能的不同，它又可以分为内置和自定义的服务。

Angular提供了许多内置的服务，例如常用的$scope、$http、$window、$location等。

但是大多数情况下我们需要自定义服务，定义服务的方法也很简单，主要有两种，一种是使用内置的$provider，另一种是调用模块中的服务注册方法，
如 service、factory、constant、和value等方法。

我们来看下源码：

```
function factory(name, factoryFn, enforce) {
  return provider(name, {
    $get: enforce !== false ? enforceReturnValue(name, factoryFn) : factoryFn
  });
}

function service(name, constructor) {
  return factory(name, ['$injector', function($injector) {
    return $injector.instantiate(constructor);
  }]);
}

function value(name, val) { return factory(name, valueFn(val), false); }

function constant(name, value) {
  assertNotHasOwnProperty(name, 'constant');
  providerCache[name] = value;
  instanceCache[name] = value;
}

```
我们可以看到 factory 和 service有些类似，而service可以接受一个构造函数，其实这就是他们最明显的差别。上面factory的源码中，return了一个
provider，我们也看到这里用到了$get，因为当一个依赖注入进来时，angular会找到对应的provider通过$get来拿到一个instance,这就是我们直接使用provider时要使用get的原因。
换句话说，当我们在某个地方注入了一个service时，背后做的就是：

```
MyServiceProvider.$get(); // return the instance of the service
```

可以看这里：
http://stackoverflow.com/questions/23074875/angularjs-factory-and-service

factory 已经new好的对象 最佳实践return
service 需要new 最佳实践 this
