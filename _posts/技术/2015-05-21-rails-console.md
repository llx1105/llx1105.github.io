---
layout: post
title: rails console
category: 技术
tags:
keywords: console
description: console调试小技巧
---

Rails console是Rails开发者的忠实小伙伴，对开发的帮助很大，我们可以在console中调试代码，查询数据库等等，对于我来说，已经是比较依赖了，每天工作的时候我先把console打开。用好了console可以让你的开发效率更高。下面记录下Rails console的小技巧。



## 1. 执行数据库操作：
可以在console里执行各种对数据库的操作，Rails的activerecord提供了各种方法给你使用，具体的就不在这里介绍了。
这里我们在操作时，经常会出现大量的sql语句，有时候我们并不想让他们出现，怎么办呢?

可以用下面的方法关闭：
ActiveRecord::Base.logger = Rails.logger
想要再次打开可以这样：
ActiveRecord::Base.logger = Logger.new STDOUT 

## 2. 模拟controller，helper请求
刚开始学习Rails的时候（其实这段时间还挺长的），遇到controller或者helper里的方法就比较无能为力了，不知道怎么调试，遇到短的还可以复制下来手动的执行，但是遇到长一点的方法这么做就很麻烦了，浪费了不少时间。
其实调试controller和helper也很简单，下面简单说一下：

### 1)controller 调试
假设我们有个controller叫做BookController，里面有个实例方法show，我们可以:

```
2.1.1 :009 > BookController.new.show
```

如果这是个类方法：

```
2.1.1 :010 > BookController.show
```

如果这是个private方法：

```
2.1.1 :011 > BookController.new.send(:method_name)
```

初学ruby的时候也经常搞不清这几者的区别，导致遇到很多问题。。


### 2)helper 调试
有时候我们需要调试helper里的方法:
也很简单，我们只用helper.method_name就可以了

```
2.1.1 :014 > helper.list_price(100)
```

有的时候helper方法中会用到controller里传来的参数，怎么办呢？没关系，我们可以用Openstruct来完成。
假设我们需要一个name参数:


```
2.1.1 :022 > helper.controller = OpenStruct.new(params: {name: 'jack'})
 => #<OpenStruct params={:name=>"jack"}> 
2.1.1 :023 > helper.controller
 => #<OpenStruct params={:name=>"jack"}> 
2.1.1 :024 > helper
 => #<ActionView::Base:0x00000007d17390 @_config={}, @view_renderer=#<ActionView::Renderer:0x00000007d67868 @lookup_context=#<ActionView::LookupContext:0x00000007d67a48 @details_key=nil, @details={:locale=>[:"zh-CN"], :formats=>[:html, :text, :js, :css, :ics, :csv, :png, :jpeg, :gif, :bmp, :tiff, :mpeg, :xml, :rss, :atom, :yaml, :multipart_form, :url_encoded_form, :json, :pdf, :zip, :xls], :handlers=>[:erb, :builder]}, @skip_default_locale=false, @cache=true, @prefixes=[], @rendered_format=nil, @view_paths=#<ActionView::PathSet:0x00000007d67980 @paths=[]>>>, @_assigns={}, @_controller=#<OpenStruct params={:name=>"jack"}>, @view_flow=#<ActionView::OutputFlow:0x00000007d733c0 @content={}>, @output_buffer=nil, @virtual_path=nil> 

```

利用OpenStruct就可以轻松实现了。


## 3.app
app本质是调用session()并且创建一个ActionDispatch::Integration::Session。

### 1)查看路径：

调用routes的helper：

```
2.1.1 :083 > app.o_bd_path
 => "/cn/o/bd"
```

### 2)发起请求： app.get(path, parameters = nil, headers = nil), 同样适用于post,put等


```
2.1.1 :030 > app.get(cn/o/deals) #or app.get app.o_deals_path
```

这里会返回请求 o_deals_path 路由后的执行日志以及状态码。
然后通过 app.response.header 和 app.response.body 可以看到上次请求的返回头和结果。

## 4.--sandbox参数

有时候我们在console里构造数据的时候并不想影响到原来的数据，这时候我们可以在启动console的时候 加上--sandbox ，这样在我们退出console时会回滚console中的数据库操作，会非常有用。


```
rails git:(develop) rails c --sandbox
Loading development environment in sandbox (Rails 3.2.13)
Any modifications you make will be rolled back on exit
2.1.1 :001 > 
```

## 5. _方法

_ 返回上次表达式执行的结果



to be continued...
