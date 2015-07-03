---
layout: post
title: include 和 extend的区别
category: 技术
tags:
keywords: include extend

include 和 extend 可以让我们把公共的方法提出去，从而使代码更简化，更加面向对象。这俩者最基本的区别就是：

include了一个module后，里面的方法我们需要当做一个实例方法来调用,而extend了一个module之后，我们需要通过类方法来调用，eg：


```
module ReusableModule
  def module_method
   puts "Module Method: Hi there!"
  end
end

class ClassThatIncludes
  include ReusableModule
end

class ClassThatExtends
  extend ReusableModule
end

# "Include"
ClassThatIncludes.new.module_method       # "Module Method: Hi there!"

# "Extend"
ClassThatExtends.module_method            # "Module Method: Hi there!"

```



这是最基本的区别吧。

to be continued ..
