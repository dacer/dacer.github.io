---
layout: post
title: "简化Octopress发布步骤"
date: 2014-02-16 17:43:24 +0800
comments: true
categories: 
- Octopress
---
用了一段时间 Octopress 确实挺好用的，就是发布需要两个步骤
```
bundle exec rake generate
bundle exec rake deploy  
```
灰常麻烦，下面说说怎么简化：  
首先`bundle exec`前缀是因为Gemfile中rake版本号被限定在旧的版本号了，把那行简化为
```ruby
#Gemfile
gem 'rake'
```
然后再
```
bundle update
```
就行了，  
下一步是合并生成和发布，打开`Rakefile`,随便找个地方copy
```ruby
desc "Generate + Deploy"
task :d do
  Rake::Task[:generate].execute
  Rake::Task[:deploy].execute
end
```
然后以后要发布的时候用`rake d`就行了，灰常舒服  
顺便也可以把preview简化一下
```ruby
desc "Preview"
task :p do
  Rake::Task[:preview].execute
end
```
然后用`rake p`即可



