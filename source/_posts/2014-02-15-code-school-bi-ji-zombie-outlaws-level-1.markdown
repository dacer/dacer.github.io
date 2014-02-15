---
layout: post
title: "Code School笔记 - Zombie Outlaws level 1"
date: 2014-02-15 15:10:57 +0800
comments: true
categories: 
- Code School
- Rails 4
---
##Match Routes
有两种方式
```
post '/somewhere', to: 'controller#action'
match '/somewhere', to: 'controller#action', via: :post
match '/somewhere', to: 'controller#action', via: :all
```

##Concern
用来简化route
```
concern :sociable do
  resources :comments
  resources :tags
end

resources :posts, concerns: :sociable
resources :items, concerns: :sociable
```
可以加上选项
```
concern :sociable do |options|
  resources :comments, options
  resources :tags, options
end

resources :msgs, concerns: :sociable
resources :posts, concerns: :sociable
resources :items do
  concerns :sociable, only: :create
end
```
可以单独封装起来
```ruby
#app/concerns/sociable.rb
class Sociable
	def self.call(mapper, options)
	  mapper.resources :comments, options
    mapper.resources :tags, options
  end
end

#routes.rb
concern :sociable， Sociable
#...

###Thread-Safety
In Rails 4
```ruby
MyApp::Application.configure do
  config.cache_classes = true
  config.eager_load = true
end
```
```