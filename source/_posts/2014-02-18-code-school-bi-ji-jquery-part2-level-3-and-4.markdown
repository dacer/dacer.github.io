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

###Utility Methods
```javascript
$('.class').find('p') //可以找到某个 .. 中的 某个 ..
$(".book-title:eq("+index+")") //可以找到 class中的第 index 项
```
`each`

```javascript
success: function(result) {
  $.each(result, function(index, city) {...});
}
```

`getJSON`专为json而简化后的ajax
```javascript
￼$.getJSON('/status', function(result) { });
```

`map`将一个 array 中每个数据处理之后整合出一个新的array
```javascript
$.map(collection, function(<item>, <index>){});

//example
var myNumbers = [1,2,3,4];
var newNumbers = $.map(myNumbers, function(item, index){ return item + 1 });
// myNumbers  -> [1,2,3,4]
// newNumbers -> [2,3,4,5]
```
用map将json数据转化为html
```
$.map(result, function(status, i) {
  var listItem = $('<li></li>');
  $('<h3>'+status.name+'</h3>').appendTo(listItem); //用appendTo可以将 .. 插入到 listItem其中
  $('<p>'+status.status+'</p>').appendTo(listItem);
  return listItem;
});
//结果：
<li>
  <h3>Dacer</h3>
  <p>Blog</p>
</li>
```
map返回的array可以直接放入html中
```javascript
var statusElements = $.map(...);
$('.status-list').html(statusElements);
```
`each`返回的是原 array，而`map`返回的是 return 的值的 array

###Detach
.detach() removes an element from the DOM, preserving all data and events.  
This is useful to minimize DOM insertions with multiple html elements.  
.detach() removes the list from the DOM, then it can be modified and reinserted into the status element.
我对此的理解是detach能将一个 element 移出 DOM ，然后对它进行任何操作都不会对网页造成影响，修改完这个 element 之后再将其放回 DOM 。
```javascript
$('.status-list').detach()
                 .html(statusElements)
                 .appendTo('.status');
```