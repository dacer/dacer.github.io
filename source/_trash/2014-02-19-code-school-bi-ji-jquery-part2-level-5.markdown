---
layout: post
title: "Code School笔记 - jQuery Part2 level 5"
date: 2014-02-19 11:13:03 +0800
comments: true
categories: 
- Code School
- jQuery
---
##Advanced Events

###Removing event handlers
使用`off(<event name>)`可以取消监听
```javascript
$('button').off('click'); //可以取消所有对 $('button') 的监听
```

###Namespacing Events
可以给 event 取一个后缀的别名：
```javascript
$('button').on('click.image', picture);
$('button').on('click.details', status);
$('button').off('click.image');  //仅取消对第一个的监听
$('button').off('.image');       //取消所有后缀是.image的监听
```

###Triggering Events
可以用来模拟用户行为：
```javascript
$('button').trigger('click'); //相当于点击了 button
```

###Custom Event
可以自定义一个 event 的名字，然后哦那个 trigger 调用
```javascript
$(<dom element>).on("<event>.<namespace>", <method>)
$('.vacation').on('show.price', showPrice);
$('.vacation').trigger('show.price');
$('.vacation:last').trigger('show.price');
```

###jQuery Plugins
```javascript
$.fn.priceify = function() {
	console.log('Pricify Called');
	console.log(this); //Within the plugin, ‘this’ will be the jQuery object the plugin was called on
};

$('.vacation').priceify();
```
然后简单的研究了一下`fn`

> In jQuery, the `fn` property is just an alias to the `prototype` property.

然后`$.fn`似乎就是构造出了一个所有 jQuery Object 都能使用的方法，然后这个 Object 会以 `this` 传入，不过需要注意的是调用它的有可能是多个 Object ，所以要用 `each` 来保证每个 Object 会单独的调用这个方法:
```javascript
$.fn.priceify = function() {
	this.each(function(){
		...
	})
};
```

###Plugins with Parameters
就是传入了一个Hash
```javascript
$.fn.priceify = function(options) {
    ...
    var details = $('<p>Book ' + options.days + ' days for $' +
                   (options.days * price) + '</p>');
}
$('.vacation').priceify({ days: 5 });  //调用
```

如果想让options变成可选项，就是有一个默认值的话就先要了解 `$.extend`

###Using $.extend
`$.extend`方法可以传入数个Hash，然后返回的值会合并相同的并且去除空的，如：

```javascript
$.extend({ days: 3 }, {}); // -> { days: 3 }
$.extend({ days: 3 }, { days: 5 }); // -> { days: 5 }
```   
使用`$.extend`来设置默认值的方法：
```javascript
$.fn.priceify = function(options) {
	this.each(function(){
		var settings = $.extend({ days: 3 }, options);
	})
};
```

还可以扩展一下：
```javascript
$.fn.priceify = function(options) {
	this.each(function(){
		var settings = $.extend({ days: 3 }, options);
	})
};
```