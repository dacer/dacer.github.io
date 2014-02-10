---
author: qqwerrd
comments: true
date: 2013-11-15 13:55:09+00:00
layout: post
slug: androidcharts-%e5%bc%80%e5%8f%91%e7%ac%94%e8%ae%b0
title: AndroidCharts 开发笔记
wordpress_id: 173
categories:
- Android
---

最近在为 [www.pomotodo.com](http://www.pomotodo.com) 做 Android版的时候其中有三个设计的非常精致的统计视图，费了很大到劲把这三个图做出来之后想了想还是开源分享出来吧，其中 LineView 还是有些繁杂的，故写了本笔记。

虽然 Android 上已经有一个很棒到开源统计图了，但还是自己手写遍更有趣些不是嘛-,- 于是没怎么看那个就动手自己写了个这个。

Github: [https://github.com/dacer/AndroidCharts](http://https://github.com/dacer/AndroidCharts)


## LineView:




### 目标：





	
  * 大部分的可见元素都能任意随时修改

	
  * 可以修改一格所代表的数据大小（默认为1）

	
  * view 的高度和方格到宽度为固定值，这样便于之后的动画效果和保证在不同大小的设备上的一致性

	
  * 在不同设备上完全的自适应




### 实现方式：


最重要的为这三个 ArrayList:



	
  * ArrayList<Integer> xCoordinateList : 背景中点的所有X坐标

	
  * ArrayList<Integer> yCoordinateList : 背景中点到所有Y坐标

	
  * ArrayList<Point> drawPointList;      : 数据所表示的点的坐标


在每次更新数据后刷新这三个 List，然后根据这三个 List 可以很轻易的画出所有的元素。

但是点击后出现到 popup 会造成两个问题，

	
  1. 怎么样画一个这个出来，而且中间的字还要在正中央：这个的话已经写了个解法在之前的博文中，

	
  2. 顶部的空间可能会不够显示最上方的点显示出的 popup：我先是在当前列表中的最大值的上方再加上一个格子，但这样有时还是不够，于是。。待续


