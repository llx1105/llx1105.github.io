---
layout: post
title: include 和 extend的区别
category: 技术
tags: ruby
keywords: include extend
---

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
注意 include后只能接Module，如果直接include一个路径是不行的，会报这个错：

```
wrong argument type String (expected Module) (TypeError)
```

所以引用前需要将常量的文件先require进来，extend同理。


这是最基本的区别吧。

但通常情况下，一般我们通过一条include的语句想把module中的实例方法和类方法都引用过来。这时我们需要在module中定义一个included的钩子方法来处理module中定义的类方法。例如：

```
module A
  def self.included(base)
   base.extend(ClassMethods)
  end

  module ClassMethods
    def class_methods
      p 'this is class methods'
    end
  end
end

B.class_methods
"this is class methods"
```

rails中通过一个叫做ActiveSupport::Concern的模块可以将module中的类方法帮我们处理
rails中通过在module中嵌套一个叫做ClassMethods的模块自动将其内定义的方法转化为类的方法。
当然，我们也可以采用自己写钩子方法的方式处理关于该问题。


to be continued ..
