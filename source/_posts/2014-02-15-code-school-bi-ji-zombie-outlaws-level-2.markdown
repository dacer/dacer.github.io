---
layout: post
title: "Code School笔记 - Zombie Outlaws level 2"
date: 2014-02-15 15:10:57 +0800
comments: true
categories: 
- Code School
- Rails 4
---
###Finders
米什么好说的
```ruby
Post.where(author: 'admin') # returns an ActiveRecord::Relation
```

###Find_By
```ruby
Post.find_by(title: 'Rails 4') #returns a single model object, or possibly a collection of model objects in an Array (not a Relation). If nothing is found, an ActiveRecord::RecordNotFound exception is raised.
```

####Find_by 与 where
用源码解释：
```ruby
def find_by(*args)
	where(*args).take
end
```
所以可以接受和 where 一样的 arguments
```ruby
Post.find_by("published_on < ?", 2.weeks.ago)
```

###Find_or_*
Rail 4 简化了 3 的`Find_or_*`方法
```ruby
Post.find_or_initialize_by(title: "rails 4")
Post.find_or_create_by(title: "rails 4")
```
其中`find_or_create_by`可以和`after_create`相结合使用：
```ruby
Post.find_or_create_by(title: 'Rails 4')
#if Post not found ↓
Post.create(title: 'Rails 4')

class Post < ActiveRecord::Base
    after_create :foo
    def foo
      posts = Post.where(author: 'admin')
      ...
		end
end
```

###Update & Update_column
```ruby
@post.update(post_params) #update attribute
@post.update_columns(post_params)
```

###Model.all
```ruby
@tweets = Tweet.all # returns an ActiveRecord::Relation
```

##第二部分
###Scopes
```ruby
scope :sold, ->{ where(state: 'sold') } #scopes should take a proc object

default_scope { where(state: 'available') }
default_scope ->{ where(state: 'available') } #defaults scopes should take proc object or a block￼￼
```
###Eager-Loaded Scopes

```ruby
scope :recent, ->{ where(published_at: 2.weeks.ago) } 
scope :recent_red, ->{ recent.where(color: 'red') }
```

###Relation#None
`.none`可以返回一个空的 ActiveRecord::Relation，避免在使用空的`array`时出现错误
```ruby
case role
when "Reviewer"
  Post.published
when "Bad User"
  Post.none
end
```

###Relation#Not
在使用`where`寻找类似`user != ?`的不等于时如果参数是`nil`的话会报 SQL 错误，使用`.where.not`则会自动判别参数是否为空，且在非空时才执行SQL语句
```ruby
Post.where.not(author: author)
```

###Relation#Order
在 Rails 4 中如果`default_scope`已经有了`order()`后，再执行`.order`时这两者的先后顺序与 3 相反， Rails 4 会将`default_scope`中的放在后面：
```ruby
class User < ActiveRecord::Base
    default_scope { order(:name) }
end

User.order("created_at DESC")

SELECT * FROM users ORDER BY name asc, created_at desc # Rails 3
SELECT * FROM users ORDER BY created_at desc, name asc # Rails 4
```
####手动排序
Rails 4 可以使用 Hash 来排序
```ruby
User.order(:name, created_at: :desc)
User.order(created_at: :desc)
```

####后面缩写的意思
Asc : ascending order 递增  
Desc : descending order 递减  

###Relation#References
```ruby
Post.includes(:comments).
  where("comments.name = 'foo'").references(:comments)
```
上例中，`.include`使返回值包含了与 Post 相关联的 comments，
在一下情况则无需使用`references`
```ruby
Post.includes(:comments).where(comments: { name: 'foo' })
Post.includes(:comments).where('comments.name' => 'foo') #hash-based conditions

Post.includes(:comments).order('comments.name') #no conditions
```

###ActiveModel::Model 
```ruby
class SupportTicket
  include ActiveModel::Conversion
  include ActiveModel::Validations
  extend ActiveModel::Naming
  attr_accessor :title, :description
  validates_presence_of :title
  validates_presence_of :description
end
```
可以简化为：
```ruby
class SupportTicket
  include ActiveModel::Model
  attr_accessor :title, :description
  validates_presence_of :title
  validates_presence_of :description
end
```