---
layout: post
title: "Code School笔记 - jQuery Part2 level 1 & 2"
date: 2014-02-16 19:10:57 +0800
comments: true
categories: 
- Code School
- jQuery
---
###Ajax Basics
使用Ajax时需要在网站后加上`?dc=123`之类的参数时可以用`data`
```javascript
$.ajax('confirmation.html', {
   success: function(response) {
     $('.ticket').html(response).slideDown();
   },
   data: { "confNum": 1234 }
   }
});
//confirmation.html?confNum=1234
```
timeout
```javascript
$.ajax('confirmation.html', {
   ...,
   timeout: 3000 //3000ms = 3 seconds
});
```
还有`beforeSend``complete`等

###Event Delegation
当你用jquery监听点击事件时只会在网页加载完毕后执行监听，如果是用ajax创造出来的是无法监听到的，所以要用到`Event Delegation`
```javascript
$('.confirmation .view-boarding-pass').on('click', function(){ ... }); //无法监听到
$('.confirmation').on('click', '.view-boarding-pass', function(){ ... });
```
以上代码是监听`.confirmation`中的点击事件，每次`.confirmation`中有元素被点击了之后都会检查是否是`.view-boarding-pass`

###Javascript Object
用`var`可以声明一个 Object ，然后将其中的function提取出来使代码更整洁
```javascript
var confirmation = {
  init: function() {
    $('.confirmation').on('click', 'button', this.loadConfirmation);
$('.confirmation').on('click', '.view-boarding-pass', this.showBoardingPass); },
  loadConfirmation: function() {
    $.ajax('confirmation.html', { ... });
   ,
  showBoardingPass: function(event) { ... }
};
$(document).ready(function() {
  confirmation.init();
});
```