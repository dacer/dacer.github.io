---
layout: post
title: "Code School笔记 - Zombie Outlaws level 6"
date: 2014-02-17 11:10:57 +0800
comments: true
categories: 
- Code School
- Rails 4
---
##ETags,Dalli, & Cache-Digests

###ETags
在client请求网页时Rail 4会生成ETags一并发送回去，且client在第二次请求时会将ETags返回回来，此时Rails会再次生成ETags并与之比对，如果相同则返回304让client直接读取缓存的内容。  
换句话说就是ETags是用来判断一个page是否有改变。

###Setting Custom ETags
可以自定义ETags，`fresh_when`就是将Etags与`@item`相关联，在`@item.cache_key`不变时返回的Etags也相同，和上一节一样返回304
```ruby
class ItemsController < ApplicationController
  def show
    @item = Item.find(params[:id])
￼    fresh_when(@item) # => headers['ETag'] = Digest::MD5.hexdigest(@item.cache_key)
  end
end

# @item.cache_key
# = 'item/2-2011224150000'
# = <model name>/ <id>-<updated_at>
```

###Declarative Etags
可以简化Etags的代码：
```ruby
class ItemsController < ApplicationController
  etag { current_user.id }
  etag { current_user.age }
  def show
    @item = Item.find(params[:id])
    fresh_when(@item)
	end
	def edit
    @item = Item.find(params[:id])
    fresh_when(@item)
  end
  def most_recent
    @item = Item.find(params[:id])
￼    fresh_when(@item)  # => fresh_when([@item, current_user.id, current_user.age])
  end
end
```

##Dalli & Cache-Digests
Dalli is a high performance pure Ruby client for accessing memcached servers.  
启用：
```ruby
#config/environments/production.rb
config.cache_store = :mem_cache_store

#Gemfile
gem 'dalli'

#Cache Store API
Rails.cache.read('key')
Rails.cache.write('key', value)
Rails.cache.fetch('key') { value }

#config/environments/production.rb
config.action_controller.perform_caching = true
```

###Fragment Caching
在某个 Client 第一次获取 fragments 时 Rail 4同时将其放入了 Cache中，之后有同样的 Request 时 Rails 就会直接从 Cache 中读取。  
使用方式：
```ruby
<ul>
 <%= render document.comments %>
</ul>

# 变为

<% cache comment do %>
	<li><%= comment %></li>
<% end %>

#app/models/comment.rb
class Comment < ActiveRecord::Base
  belongs_to :document
end
```
上述方法和 ETags 类似，就是通过生成，保存，读取，对比 cache_key 来判断是否从 cache 读取还是重新 render ，  
但是这样会出现一个问题：如果上面代码中的 comment 没有改变，但是网页中`<% cache comment do %>`中的内容进行了改变，如增加了`<%= comment.author %>`，就会造成之前的Client仍被缓存在之前的版本中，所以需要一个 template version :
```ruby
<% cache ['v1', comment] do %>
  <li><%= comment %></li>
<% end %>

#变为

<% cache ['v2', comment] do %>
  <li><%= comment %> - <%= comment.author %></li>
<% end %>
```
但是 Rails 4 中的 cache 会自动保存和对比本文件的 MD5 Hash ，当这个改变时会同样重新缓存  
**所以 Rails 4 无需 version**

###Cache 嵌套出现的问题
```html
# app/views/projects/show.html.erb
<%= render @project.documents %>

# app/views/documents/_document.html.erb
<% cache document do %>
 <article>
   <h3><%= document.title %></h3>
   <ul>
     <%= render document.comments %>
   </ul>
   <%= link_to 'View details', document %>
 </article>
 <% end %>

# app/views/comments/_comment.html.erb
<% cache comment do %>
   <li><%= comment %></li>
<% end %>
```
现在模拟下述流程：
```
First Request to get Document 1:
	document/1     sets cache 
	  comment/1    sets cache
	  comment/2    sets cache

Second Request to get Document 1:
	document/1     reads cache 

Comment 2 gets Updated in Database

Third Request to get Document 1:
	document/1     reads cache 
```
这样会发现被修改的`Comment 2`因为在`document`之内，所以无论怎么修改都仍会显示`document`的 cache，解决方法：
```ruby
#app/models/comment.rb
class Comment < ActiveRecord::Base
  belongs_to :document, touch: true
  belongs_to :dacer 
end
``` 
增加了`touch: true`之后comment在更新了`updated_at`时会将所`belongs_to`的document的`updated_at`也一同更新了

###Cache Digests
当使用`helper methods`时必须要用`partial`:
```html
<% cache document do %>
 <article>
   <h3><%= document.title %></h3>
   <ul>
   <%= render document.recent_comments %> #需要替换为下面这个
   <%= render partial: "comments/comment",
       collection: document.recent_comments %>
   </ul>
   <%= link_to 'More details', document %>
 </article>
 <% end %>

```