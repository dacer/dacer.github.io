---
layout: post
title: "Code School笔记 - jQuery Part2 level 3 & 4"
date: 2014-02-18 11:13:03 +0800
comments: true
categories: 
- Code School
- jQuery
---
##Ajax Forms

```javascript
$('form').on('submit', function(event) {
  event.preventDefault();  //阻止原按钮的行为
  var form = $(this);      //这样保存form为变量可以减少 DOM queries
  $.ajax('/book', {
    type: 'POST',
    data: form.serialize(), //可以合并form中所有的 fields 作为data
    success: function(result) {
      form.remove();
      $('#vacation').hide().html(result).fadeIn();
    } 
  });
});
```

###With Json

```javascript
$('form').on('submit', function(e) {
  event.preventDefault();
  var form = $(this);
  $.ajax($('form').attr('action'),{   //自动从html的form中获取要post的地址
    type: $('form').attr('method'),   //自动获取 发送方式
    contentType: 'application/json',  //让服务器返回 json 格式的数据
    dataType: 'json',                 //使发出的数据格式为 json
    data: form.serialize(),
		success: function(result) {
		  form.remove();
		  var msg = $("<p></p>");
		  msg.append("Destination: " + result.location + ". ");
		  msg.append("Price: " + result.totalPrice + ". ");
		  msg.append("Nights: " + result.nights + ". ");
		  msg.append("Confirmation: " + result.confirmation+ ".");
		  $('#vacation').hide().html(msg).fadeIn();
		}
	});
});
```

###Utility
```javascript
$('.class').find('p') //可以找到某个 .. 中的 某个 ..

$(".book-title:eq("+index+")") //可以找到 class中的第 index 项
```
