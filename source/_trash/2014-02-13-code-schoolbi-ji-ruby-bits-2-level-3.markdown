---
layout: post
title: "Code School笔记 - Ruby Bits 2 Level 3"
date: 2014-02-13 15:40:25 +0800
comments: true
categories: 
- Code School
- Ruby
---
##Self
在class外调用self会返回`main`，在class内调用会返回class本身。  
如果在定义类中的方法时，在方法名前加`self`则这个方法会变成类似于Java的静态(static)方法
```ruby
class Tweet
  def self.f(key)
    p "#{self}"
  end
  def Tweet.f ... #与上方一致，但不常用
end
Tweet.f("Hi") # => Tweet
tweet = Tweet.new
tweet.f("dacer") # Error!
```

###Class_eval
重新打开一个class并往里面增加一些东西
```ruby
Tweet.class_eval do
	attr_accessor :user
end
```

####用 class_eval 写一个MethodLogger
就是记录方法的使用log
```ruby
class MethodLogger
	def log_method(klass, method_name) #这里用klass是因为class在ruby中是预置的词
	  klass.class_eval do
	    alias_method "#{method_name}_original", method_name
	    define_method method_name do
	      p "#{Time.now}: Called #{method_name}"
	      send "#{method_name}_original"
	    end
    end
  end
end
#使用
class Test
	def t
	end
end

logger = MethodLogger.new
logger.log_method(Test, :t)
Test.new.t
```

###Instance_eval
类似于class_eval