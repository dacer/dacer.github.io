---
layout: post
title: "Code School笔记 - Ruby Bits 2 Level 4"
date: 2014-02-14 15:40:25 +0800
comments: true
categories: 
- Code School
- Ruby
---
##Handling Missing Methods
在class中可以定义一个`method_missing`方法，当调用这个class中不存在的方法时会变为调用此方法。
```ruby
class Tweet
  def method_missing(method_name, *args)
    puts "You tried to call #{method_name} with these arguments: #{args}"
    #super 可以调用ruby default方法， raises a NoMehodError.
  end
end
Tweet.new.submit(1, "Here's a tweet.") # You tried to call submit with arguments: [1, "Here's a tweet."]
```
利用`method_missing`可以简化很多代码，下例中就是把这个方法所在的class中的方法链接到`@user`的同名方法中了
```ruby
def method_missing(method_name, *args)
    @user.send(method_name, *args)
end
```
把上面的代码改进一下可以限定方法名
```ruby
class Tweet
  DELEGATED_METHODS = [:username, :avatar]
  def initialize(user)
    @user = user
	end
  def method_missing(method_name, *args)
    if DELEGATED_METHODS.include?(method_name)
      @user.send(method_name, *args)
    else
			super 
		end
	end 
end
```
Ruby也自带了一个`delegate`可以很方便的实现上述代码
```ruby
require 'delegate'
class Tweet < SimpleDelegator
  def initialize(user)
    super(user)
  end
end
```

###一个跟本节不相关的知识点
正则表达式提取方法：
```ruby
match = name.to_s.match(/^hash_(/w+)/)
match[1] if match #返回符合的首个子字符串，如果存在的话
                  #0为返回整个
```

###Respond_to?
Tells us if an object responds to a given method  
似乎可以翻译成回应一个object是否有所给的方法？
```ruby
tweet = Tweet.new 
tweet.respond_to?(:to_s) # => true
```
需要注意的是，用`method_missing`实现的方法在`respond_to?`下会返回false的，所以要做如下修改：
```ruby
def respond_to?(method_name)
    method_name =~ /^hash_\w+/ || super # ||的作用是在||之前不为nil的时候返回之前的，为nil的时候返回后者
    #LIST.include?(method_name.to_s) || super #或者是从一个array中判断
end
```
但是这又带来了一个问题，`.method`方法仍是error
```ruby
tweet.method(:hash_ruby)
```
在1.9.3以上版本中可以这样解决：变为`respond_to_missing?`，这样`method`就能正确返回一个 method object
```ruby
def respond_to_missing?(method_name) 
	method_name =~ /^hash_\w+/ || super
end
```

###在 call 一个未定义方法时定义它
其实我还不太理解这种用法的意义在哪儿。。
```ruby
def method_missing(method_name, *args)
	match = method_name.to_s.match(/^hash_(\w+)/)
	if match
		self.class.class_eval do
			define_method(method_name) do
			@text << " #" + match[1]
			end
		end
		send(method_name)
	else 
		super
	end 
end
```
