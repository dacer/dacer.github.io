---
author: qqwerrd
comments: true
date: 2013-09-28 14:39:27+00:00
layout: post
slug: '%e5%a6%82%e4%bd%95%e5%9c%a8action-bar%e4%b8%ad%e6%98%be%e7%a4%ba%e4%b8%80%e5%b1%82%e8%bf%9b%e5%ba%a6%e6%9d%a1%ef%bc%8c%e5%a6%827x7'
title: 如何在Action Bar中显示一层进度条，如7x7
wordpress_id: 91
categories:
- 开发笔记
---

看到这个需求第一时间我就想到了之前很沉迷的游戏 7x7, 搜了一下果然有相关的问题，链接如下：

[http://stackoverflow.com/questions/17173414/how-to-implement-a-dynamic-background-on-actionbar-like-7x7](http://stackoverflow.com/questions/17173414/how-to-implement-a-dynamic-background-on-actionbar-like-7x7)

7x7作者亲自现身说道：


> I actually made a custom Action Bar that was transparent and added the level progress bar underneath it.


然后给出了这个[windowActionBarOverlay](http://developer.android.com/reference/android/R.attr.html#windowActionBarOverlay)链接，于是开始

尝试1：

将Action Bar设置为透明且可和下部分重叠，先简单的放了个button试了下，效果如下：

![Screen-Shot-2013-09-30-at-4.49.52-PM]({{ root_url }}/images/post/screen-shot-2013-09-30-at-4-49-52-pm1.png)

之后用testin测试了下其他机型的适配情况，除了联想的一个奇葩机型之外都如图显示，于是意外的得出了个结论，**button高度=Action Bar高度**（这是歪打正着吧(╯‵□′)╯︵┻━┻）

尝试2：

但这样还是不太方便，于是兴冲冲的研究了半天这个[pull-to-refresh](https://github.com/chrisbanes/ActionBar-PullToRefresh)的源码，发现它的原理是在Action Bar上覆盖了一层，这不是完全没用嘛 (╯‵□′)╯︵┻━┻

不过发现了这个方法

```java
public int getActionBarSize(Context context) {
  int[] attrs = {android.R.attr.actionBarSize};
  TypedArray values = context.getTheme().obtainStyledAttributes(attrs);
  try {
    return values.getDimensionPixelSize(0, 0);
  } finally {
    values.recycle();
  }
}
```
最后得出的结论就是：目前看来还是用方法一，然后写个drawable作为自定义粗的进度条  
**Update**  
最后用的方法是，仿照actionbar做一个 custom view = =

