---
layout: post
title: "怎么把windows下的txt文件转为utf-8"
date: 2014-02-10 19:15:11 +0800
comments: true
categories: 
- Windows
---
Windows真是害人不浅- -#
在Mac下可以 
```
iconv -f gbk -t utf-8 a.txt  > b.txt 
```
批量的话
```
find *.txt -exec sh -c "iconv -f gbk -t UTF8 {} > c/{}.txt" \;  
```
Ubuntu下可以
```
enconv *.txt
```
a.txt是要转换的文件，gbk是简体系统下txt默认保存编码(大概。。)