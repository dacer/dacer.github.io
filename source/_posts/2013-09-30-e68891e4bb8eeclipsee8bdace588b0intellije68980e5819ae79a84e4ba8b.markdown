---
author: qqwerrd
comments: true
date: 2013-09-30 08:17:48+00:00
layout: post
slug: '%e6%88%91%e4%bb%8eeclipse%e8%bd%ac%e5%88%b0intellij%e6%89%80%e5%81%9a%e7%9a%84%e4%ba%8b'
title: 我从eclipse转到IntelliJ所做的事
wordpress_id: 107
categories:
- Android
---

早就不想用那个蜗牛速的eclipse了，于是着手准备转到Android studio开发

1.鼠标悬停看javaDoc - 找到安装目录下的 bin/idea.properties ，填上下面一行

    
    <code>auto.show.quick.doc=true</code>


但是实际测试无用。又找到一个方法可显示，如下

Settings -> Editor -> Show quick doc on mouse move
