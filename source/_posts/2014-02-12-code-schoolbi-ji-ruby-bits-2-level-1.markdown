---
layout: post
title: "Code School笔记 - Ruby Bits 2 Level 1"
date: 2014-02-12 19:02:51 +0800
comments: true
categories: 
- Code School
- Ruby
---
##Block

Block保存可以使用`Proc.new`和`Lambda`.

###Proc.new
Proc可以用`{}`或者`do end`来定义，`call`来执行
```ruby
my_proc = Proc.new{ p "proc" }
my_proc = Proc.new do
	p "proc"
end 
my_proc.call
```

###Lambda
Lambda可以直接用`= ->`来定义**(Ruby1.9以上)**
```ruby
my_proc = lambda { p "proc" }
my_proc = -> { p "proc" }
my_proc.call
```

###使用场景
可以把Proc直接作为变量传入方法的参数中
```ruby
def print(proc)
	proc.call
end

my_proc = -> { p "proc" }
print(my_proc) # "proc"
```
###需要注意
`array.each( )`括号中放入的proc前必须加`&`,`&`可以把proc变为block
```ruby
tweets.each(&printer)
```
当定义的方法中需要用到 block 时可以：
```ruby
 def each(&block)
   tweets.each(&block)
 end
```
###其它
* 在方法中用`block_given?`可以知道调用此方法时是否提供了block
* lambda在生成时可以记住。。什么 **需要回顾一下**
```ruby
def tweet_as(user)
  lambda { |tweet| p "#{user}: #{tweet}"}
end

dacer_tweet = tweet_as("dacer")
dacer_tweet.call("Hello") # => dacer: Hello
```
