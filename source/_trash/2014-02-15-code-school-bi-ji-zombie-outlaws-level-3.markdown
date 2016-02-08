---
layout: post
title: "Code School笔记 - Zombie Outlaws level 3"
date: 2014-02-15 19:10:57 +0800
comments: true
categories: 
- Code School
- Rails 4
---
##Strong Parameters and Remote Forms & Filters, Session, Custom Flash Types
###Strong Parameters
在rails 4的controller中必须指定可接受的params
```ruby
	def user_params
	  params.require(:user).permit(:name)
	end
```
其中`rquire`会检查是否有参数中的key，没有则返回400错误，但并不会将这个key的值返回到`user_params`中，`permit`则会将参数中key对应的值返回  
add this line if you want to raise errors for unpermitted params:
```ruby
#config/application.rb
config.action_controller.action_on_unpermitted_parameters = :raise
```

###Authenticity Token
Rails 会自动在form中的value加入token来防止机器人的spam

###Action Controller Filters/Actions
3中的`before_filter`变为了`before_action`,可以让Controller在部分方法call之前做出一些动作：
```ruby
class PeopleController < ActionController::Base
	before_action :set_person,      except: [ :index, :new, :create ]
	before_action :ensure_permission, only: [ :edit, :update ]
end
```

###Flash Types
以下方法可以生成flash信息：
```ruby
if @item.save
  flash[:notice] = 'Item created.'
  redirect_to @item
else
  render action: 'new'
end
```
```html
<p id="notice"><%= flash[:notice] %></p>
<p> <strong>Name:</strong> <%= @item.name %>
</p>
```
其中erb部分可以简写为
```html
<p id="notice"><%= notice %></p>
<p id="alert"><%= alert %></p>
```
你也可以自定义flash类型
```ruby
class ApplicationController < ActionController::Base
  add_flash_types :grunt, :snarl
end

#...
flash[:grunt] = 'braaains...'
#或者简写为
redirect_to @user, grunt: 'I'm Dacer...'
```
```html
<div id="grunt"><%= grunt %></div>
```
