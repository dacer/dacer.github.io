---
layout: post
title: "Code School笔记 - jQuery Part2 level 1"
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