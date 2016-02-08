---
layout: post
title: "Code School笔记 - Ruby Bits 2 Level 2"
date: 2014-02-13 15:40:25 +0800
comments: true
categories: 
- Code School
- Ruby
---
##Dynamic Classes & Methods

###Struct
在一个Class中如果包含的变量都在 initialize 中定义时可以用`Struct`简化，以下两种定义方式完全相同：
```ruby
class Tweet
  attr_accessor :user, :status
  def initialize(user, status)
    @user, @status = user, status
	end
  def to_s
    "#{user}: #{status}"
  end 
end
```
```ruby
Tweet = Struct.new(:user, :status) do 
  def to_s
    "#{user}: #{status}"
  end
end
```
如果Class中无需其它方法，其中`do...end`部分也可以省略

###Alias_Method
在一个Class中复制一个方法，仅改变方法名可以用`alias_method`  
读取一个变量的方法可以用`attr_reader`简写

```ruby
def tweets
	@tweets
end

def contents
  @tweets
end

#上下两组定义相等
attr_reader :tweets
alias_method :contents, :tweets
```
###动态生成方法
```ruby
class Tweet
  def draft
	@status = end
	def posted
	@status = :posted
	end
	  def deleted
	    @status = :deleted
	end
end

#上下两个Class完全一致
class Tweet
  states = [:draft, :posted, :deleted]
  states.each do |status|
    define_method status do
      @status = status
    end
	end 
end
```
如果动态生成的方法需要参数则可以：
```ruby
define_method m do |var|
  games.send(m, var)
end
```

###Send
`send`方法可以调用class中同名的方法(包括private,protected中的)  
`public_send`则只能调用公共方法
```ruby
class Timeline
  #...

  private
  
  def private_method
  end
end
t = Timeline.new
t.private_method #ERROR
t.public_send(:private_method)
t.send(:private_method)
t.send("private_method")

t.send(:private_method,"",var)#有参数时
```

###The Method Method
`.method`方法可以得到一个class的方法并保存起来,再次调用的方式为`call`，类似于`Proc`
```ruby
show_method = timeline.method(:show_tweet)
show_method.call
```
Turn the Method object into a Proc object:
```ruby
(0..1).each(&show_method)
#上下相同
show_method.call(0)
show_method.call(1)
```