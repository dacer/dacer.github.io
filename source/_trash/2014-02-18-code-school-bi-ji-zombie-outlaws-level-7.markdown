---
layout: post
title: "Code School笔记 - Zombie Outlaws level 7"
date: 2014-02-18 11:10:57 +0800
comments: true
categories: 
- Code School
- Rails 4
---
##ActionController Live & Turbolinks
使用`ActionController::Live`可以让服务器和 client 保持
```ruby
#controllers/items_controller.rb
class ItemsController < ApplicationController
  include ActionController::Live
end
```
默认的 WEBrick 会断开连接，所以要使用  Puma or Rainbows or other compatible servers
```ruby
gem 'puma'
```
使用方法：
```ruby
#controllers/items_controller.rb
def show
	response.headers["Content-Type"] = "text/event-stream" 
	3.times {
		response.stream.write "Hello, browser!\n" #write一定要在设置了 headers 之后
		sleep 1 
	}
  response.stream.close
end
```

###EventSource
使用js的方法：
```javascript
$(document).ready(initialize);
function initialize() {
  var source = new EventSource('/items/events');
  source.addEventListener('message', update);
};

function update(event) {
  var item = $('<li>').text(event.data);
  $('#items').append(item);
}
```

###使用Redis
```ruby
# controllers/items_controller.rb
def events
  response.headers["Content-Type"] = "text/event-stream"
  redis = Redis.new
  redis.subscribe('item.create') do |on|
    on.message do |event, data|
      response.stream.write("data: #{data}\n\n")
		end 
	end
  response.stream.close
end
```

##Turbolinks
Rails 4 中默认使用 Turbolinks 通过 ajax 来加快点击 link 之后的跳转速度  

Turbolinks 会自动判断载入的 css js 有无改动
```
<%= stylesheet_link_tag "application", media:
  "all", "data-turbolinks-track" => true %>
<%= javascript_include_tag "application",
  "data-turbolinks-track" => true %>
```

###启用方法
以下在 Rails 4 中默认启用
```
#Gemfile
gem 'turbolinks' 

#assets/javascripts/application.js
 //= require turbolinks
```

###Turbolinks Events
用 js 监听时要注意使用`page:load`，来初始化监听，以免点击link后监听失效
```javascript
function initialize() {
  $('#owner_active').click( function() {
    alert(this.checked);
  });
}
$(document).ready(initialize);
$(document).on('page:load', initialize);
```

或者可以用一个 gem 自动实现上述代码，ready() will always fire after "page:load"
```
#Gemfile
gem 'jquery-turbolinks'

assets/javascripts/application.js
 //= require jquery
 //= require jquery.turbolinks  #order is important!
 //= require jquery_ujs
 //= require turbolinks
```
￼
另一种监听方式：
```javascript
function checkAlert() {
  alert(this.checked);
}
$(document).on('click', '#owner_active', checkAlert);
```

监听`page:fetch`和`page:change` 可以用来显示loading
```javascript
$(document).on('page:fetch', function() {
  $('#loading').show();
});
$(document).on('page:change', function() {
  $('#loading').hide();
});
```

###取消 Turbolinks
Turbolinks affect all internal links by default, but you may not want all links to use it:
```
<%= link_to 'Requests', requests_path, "data-no-turbolink" => true %>
```
If a parent has the attribute, all children will be opted-out of Turbolinks:
```
<div id="navigation" data-no-turbolink="true">
  <%= link_to 'Show', @request %> |
  <%= link_to 'Back', requests_path %>
</div>
```