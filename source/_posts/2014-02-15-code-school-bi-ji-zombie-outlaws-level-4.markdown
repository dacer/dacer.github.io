---
layout: post
title: "Code School笔记 - Zombie Outlaws level 4"
date: 2014-02-15 19:10:57 +0800
comments: true
categories: 
- Code School
- Rails 4
---
##Views & Vistas
###Collection Form Helpers
Rail 4 提供了更多的helpers
```ruby
class Owner < ActiveRecord::Base
   has_many :items
end
 class Item < ActiveRecord::Base
   belongs_to :owner
end

collection_select(:item, :owner_id, Owner.all, :id, :name)
collection_radio_buttons(:item, :owner_id, Owner.all, :id, :name)
collection_check_boxes(:item, :owner_id, Owner.all, :id, :name)
```

###Date Field
Rails 4 提供了新的 `date_field`
```ruby
<%= f.date_field :return_date %>
```

###Response Type From Controller
默认返回json的格式
```ruby
def index
   @owners = Owner.all
   respond_to do |format|
     format.html
     format.json { render json: @owners }
   end
end
```
自定义json返回的格式可以用`jbuilder`或者Rails 4 中新的：
```ruby
#views/owners/index.json.ruby
owners_hashes = @owners.map do |owner|
   { name: owner.name, url: owner_url(owner) }
end
owners_hashes.to_json

#Output
[
  {"name":"Slow-draw",  "url":"http://localhost:3000/owners/1"},
  {"name":"Sheriff",    "url":"http://localhost:3000/owners/2"},
  {"name":"Walking Ned","url":"http://localhost:3000/owners/3"}
]
```