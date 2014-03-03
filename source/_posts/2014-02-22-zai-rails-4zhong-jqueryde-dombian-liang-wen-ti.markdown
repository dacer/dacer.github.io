---
layout: post
title: "在Rails 4中jquery的Dom变量问题"
date: 2014-02-22 12:09:49 +0800
comments: true
categories: 
- Rails 4
---
今天在开发的时候发现在Rails 4中
```javascript
var selectUploadBtn = $("#fileupload");
var startUploadBtn = $(".start-upload");

$(document).ready(function(){
  selectUploadBtn.on("change", handleFiles);
  startUploadBtn.on("click",onUploadClicked);
});
```
是无效的，第一反应就是 Turbolinks 搞的鬼，然后使用了
```javascript
var selectUploadBtn = $("#fileupload");
var startUploadBtn = $(".start-upload");

function initialize() {
  selectUploadBtn.on("change", handleFiles);
  startUploadBtn.on("click",onUploadClicked);
}

$(document).ready(initialize);
$(document).on('page:load', initialize);
```
还是没反应，然后突然发现是不是 DOM 的变量没有初始化？  
最后
```javascript
var selectUploadBtn = $("#fileupload");
var startUploadBtn = $(".start-upload");

function initialize() {
  selectUploadBtn = $("#fileupload");
  startUploadBtn = $(".start-upload");
  selectUploadBtn.on("change", handleFiles);
  startUploadBtn.on("click",onUploadClicked);
}
终于解决了