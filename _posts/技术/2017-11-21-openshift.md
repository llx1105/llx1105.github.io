---
layout: post
title: openshift basic concepts
category: 技术
tags: tech
keywords: openshift
---

## PAAS

openshift是一个基于K8s的PaaS应用，关于PaaS的概念，可以简单的参考下面的图来理解。将IT企业的服务分为九层的话，传统的企业需要维持九层服务，成本很高。后面出现了云计算服务商可以外包这部分工作的服务，根据外包的层次不同可以将其分为几种比如IaaS(Infrastructure as a Service，基础设施即服务)，PaaS，SaaS（Software as a Service）三层。  

PaaS 层为应用开发人员服务，提供支撑应用运行所需的软件运行时环境，相关的工具与服务，如数据库服务、日志服务、监控服务等，让应用开发者可以专注于交付业务价值的代码而无需关心应用所需脚手架。  

举个例子：如果我想不采用云服务来建立一个网站，我需要购买或租用服务器，安装服务器软件，编写网站程序等等。如果我采用IasS服务，那么我可能不需要服务器了，只需要个虚拟机以及自己安装服务器软件而如果采用PaaS的服务，那么意味着你既不需要买服务器，也不需要自己装服务器软件，只需要自己开发网站程序。  

![PAAS](/public/img/posts/paas.png)

## Kubernetes Infrastructure

openshift 核心部分实现容器的调度是封装的 Kubernetes，一个k8s集群有一个或多个master节点以及多个node节点。OpenShift可以利用包括Docker Hub,第三方提空的私有仓库以及内置的registry提供的images resources.  

## Containers and Images

### containers

container是openshift一个非常基本的概念，linux的容器技术是为了对正在运行中的进程隔离除了其指定交互资源的一个轻量级应用。一般情况下，一个container提供一个服务，我们可以称作微服务，比如web server或者database等。  

### images

openshift中的containers是基于docker的container镜像。image是一种包含了运行一个container时需要的所有信息的二进制文件。一个image可能被多次重复使用，这时我们通过一个特有的hash码来区分。  

### Container Registries
一个container registry是一个可以存和取docker container image的地方，如上文刚刚提到的3种类型。  

## Pods and Services  

### Pods
openshift继承使用了k8s中的pod概念，Pod可以供一个或多个container使用，每个pod有一个自己的internal IP地址，pod有自己的生命周期，它会被定义、使用以及移除。pod在运行中是不可改变的，如果要对一个pod进行修改，openshift会把这个pod干掉，再根据修改过的配置或者支持的image来新建一个pod，pods也是一个可消耗品，openshift推荐pod被higher level的controller来操纵而不是用户直接来操纵。  

### Services

K8s的service被当做一个内部的load balancer，并且提供外部可以访问的ip来提供服务，如同pod一样，service也是一种REST资源对象。..

## Projects and Users

### Projects

project的概念是继承于k8s的带有一些额外注解的namespace，可以作为资源和权限的隔离，另外，它还有name，displayName 以及 description等属性。  

### Users

openshift是通过user来控制权限的管理，有以下几种user：  

### Regular users：

user创建的时候默认的类型。

### System users:  

大多系统用户存在的意义是为了安全的与api打交道。这其中包括有集群的admin用户（拥有所有access），一个节点user,  使用routers和registries的user等等。例子：system:adminsystem:openshift-registrysystem:node:node1.example.com  

### Service accounts：

这是些特殊的与projects打交道的system users.有些是在project创建的时候就定义的。Service accounts表现形式为ServiceAccount.
例子：system:serviceaccount:default:deployersystem:serviceaccount:foo:builder  

## Builds and imageStream

### Builds
build是一个将一些输入的参数转换为输出的结果的一个过程。很多时候，这个过程是只将一些参数或者source code转换成一个可运行的环境的过程。而buildConfig就是描述这个过程需要怎么做的定义文件。..

openshift提供三种主要的build 策略：  

### docker build:
Docker build会执行 [Docker build](https://docs.docker.com/engine/reference/commandline/build/)的命令, 因此它需要一个描述了repository信息的Dockerfile文件以及所需要的artifacts来产生一个可运行的镜像环境。  

### Source-to-Image (S2I) build：

[S2I](https://docs.openshift.org/1.5/creating_images/s2i.html#creating-images-s2i) 是一个为了可重复build而产生docker格式的container的工具。它通过将application source装进一个容器镜像并组合生产出一个read-to-run的新的镜像。这个组合了builder这个基础镜像以及源代码镜像的全新镜像随时准备好接受docker run的指令。同时，S2I支持增量build，也就是支持对之前版本的image进行再build操作。  

### Customer build：

自定义的build策略允许开发者定义一个特殊的builder image来完成整个build过程。使用我们自己定义的builder image允许我们自定义我们想要的build的过程。  

### Pipeline build:

这种build方式允许开发者通过定义一个Jenkins pipeline来执行通过 Jenkins pipeline的插件。Pipeline的workflows定义在Jenkins file里，也可以直接嵌套在build configuration里。
