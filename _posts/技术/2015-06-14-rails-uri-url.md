---
layout: post
title: url与uri
category: 技术
tags:
keywords: uri url
description: url与uri
---

URI (uniform resource identifier)统一资源标志符；
URL (uniform resource location )统一资源定位符（或统一资源定位器）；



## 1. URL

  URL是Internet上用来描述信息资源的字符串，主要用在各种WWW客户程序和服务器程序上。采用URL可以用一种统一的格式来描述各种信息资源，包括文件、服务器的地址和目录等。
URL的格式由下列三部分组成：

 1.协议（或称为服务方式）；

 2.存有该资源的主机IP地址（有时也包括端口号）；

 3.主机资源的具体地址,如目录和文件名等。



 第一部分和第二部分之间用”：//”符号隔开，第二部分和第三部分用”/”符号隔开。第一部分和第二部分是不可缺少的，第三部分有时可以省略。
 目前最大的缺点是当信息资源的存放地点发生变化时，必须对URL作相应的改变。因此人们正在研究新的信息资源表示方法。

 在我们日常开发中经常会涉及到url的使用，很多情况下我们是直接把url拼出来，或者写死的，但是一旦有参数改动之类的情况修改起来比较麻烦。如果可以最好能用URI来表示。

## 2. URI
 URI是一个相对来说更广泛的概念，URL是URI的一种，是URI命名机制的一个子集，可以说URI是抽象的，而具体要使用URL来定位资源。
 URI是以某种统一的（标准化的）方式标识资源的简单字符串，一般由三部分组成：

 1.访问资源的命名机制。

 2.存放资源的主机名。

 3.资源自身的名称，由路径表示。

　
ruby中提供了方法将url直接转换成uri

```
2.1.1 :002 > uri = URI('http://llx1105.github.io/2015/05/21/redis-sort-01.html')
 => #<URI::HTTP:0x00000006afe348 URL:http://llx1105.github.io/2015/05/21/redis-sort-01.html> 
2.1.1 :005 > uri.scheme
 => "http" 
2.1.1 :006 > uri.host
 => "llx1105.github.io" 
2.1.1 :007 > uri.path
 => "/2015/05/21/redis-sort-01.html" 
2.1.1 :008 > uri.query
 => nil 

2.1.1 :009 > uri.query = {page: 5}.to_query
 => "page=5" 
2.1.1 :010 > uri
 => #<URI::HTTP:0x00000006afe348 URL:http://llx1105.github.io/2015/05/21/redis-sort-01.html?page=5> 
2.1.1 :011 > uri.query
 => "page=5" 
2.1.1 :013 > url = uri.to_s
 => "http://llx1105.github.io/2015/05/21/redis-sort-01.html?page=5" 
```

在coding时经常会遇到参数的改动，通过uri可以方便的修改参数，最后uri.to_s即可得到url。


to be continued...
