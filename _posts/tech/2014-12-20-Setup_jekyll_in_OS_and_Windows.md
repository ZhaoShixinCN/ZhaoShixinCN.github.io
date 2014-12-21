---
layout: post
title: 在OS和Windows系统下搭建Jekyll
category : technology
tagline: "备忘"
tags : [WEB]
---
{% include JB/setup %}

纯粹为了备忘～～

##［OS 10.9.5］

系统是自带ruby 2.0.0的，主要参考了一下两篇网页：

[MAC OS X 10.8 下安装jekyll](http://www.blogways.net/blog/2013/04/08/install-jekyll-on-mac.html "With a Title").

[mac os gem安装json出现error failed的解决办法](http://www.jb51.net/article/51590.htm  "With a Title").

此外，下发命令前最好都带上sudo

##［WIN7］

[Windows 上安装 Jekyll](http://blog.csdn.net/kong5090041/article/details/38408211 "With a Title").

[Jekyll安装及写静态博客](http://www.tuicool.com/articles/7Vz6BzJ "With a Title").

主要遇到的问题：

1. 可能是由于Jekyll以来的包版本较老，ruby不能使用2.1或以上的版本。安装Ruby 2.0.0-p598则没有问题。

2. 似乎不需要手工安装Python以及pygments（安装完Jekyll后，在C:\Ruby200-x64\lib\ruby\gems\2.0.0\gems目录下找到了pygments，且版本为0.6.0）。