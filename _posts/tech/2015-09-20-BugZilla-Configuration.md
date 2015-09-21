---
layout: post
title: 如何内网\外网同时访问BugZilla
category : technology
tagline: ""
tags : [WEB]
---
{% include JB/setup %}

最近外包了一个项目给外面的公司，为了做缺陷管理，新搭了一个BugZilla系统。

安装好以后，通过内网IP登陆的，没有任何问题。

但后来为了让外部公司也能够直接登录这个系统，于是在路由器上做了一个端口映射，结果发现永远登陆不上去。

在网上搜索了一番，发现BugZilla的网页间跳转逻辑，是依赖浏览器输入的网址来判断的。

如果浏览器输入的网址，和BugZilla配置的不一致，那么跳转后就会判决为未登录。

假设外网登陆地址为http://xxx:8080/，那么需要修改\bugzilla\data\目录下的params文件。

将'urlbase'的一行改为   

	'urlbase' => 'http://xxx:8080/',

登陆就正常了。

为了避免有诡异的问题发生，顺手将'cookiedomain'的一行，改为   

	'cookiedomain' => 'genew-hz.oicp.net',
	
修改之后，内网/外网就能够同时用“http://xxx:8080/”来访问BugZilla了。