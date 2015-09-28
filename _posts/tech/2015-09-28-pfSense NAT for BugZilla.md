---
layout: post
title: pfSense路由器配置NAT，支持内网\外网同时访问BugZilla
category : technology
tagline: ""
tags : [WEB]
---
{% include JB/setup %}

接上一篇文章.....

前几天，办公室使用的pfSense路由器电源挂了，临时拿一个TP-LINK顶缸。

TP-LINK虽然配置起来简单，但是性能实在是垃圾的紧，二十几个终端，基本上一天需要重启一次。

换回pfSense以后，发现内网无法通过“http://xxx:8080/”访问BugZilla了。

后来改了下配置，就OK了。很简单，记录一下：

![配置截图](https://github.com/ZhaoShixinCN/ZhaoShixinCN.github.io/raw/master/img/pfSense_NAT.png)
