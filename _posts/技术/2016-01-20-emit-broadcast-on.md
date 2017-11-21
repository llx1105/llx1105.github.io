---
layout: post
title: Angular emit broadcast on事件触发
category: 技术
tags: tech
keywords: Angularjs
---


## oradcast

简单的说

$scope.$broadcast is for the $scope itself and its children

$rootScope.$broadcast is a method that lets pretty much everything hear it include the rootscope

## mit

$scope.$emit is when you want that $scope and all its parents and $rootScope to hear the event.

$rootScope.$emit only lets other $rootScope listeners catch it

## n
to catch event and data
