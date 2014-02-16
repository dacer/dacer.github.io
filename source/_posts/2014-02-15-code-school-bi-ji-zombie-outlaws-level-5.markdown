---
layout: post
title: "Code School笔记 - Zombie Outlaws level 5"
date: 2014-02-15 19:10:57 +0800
comments: true
categories: 
- Code School
- Rails 4
---
##Test Your Mettle
###Rake Tasks
```
rake test:models
rake test:controllers
rake test:helpers
rake test:mailers
```
#需回顾：
```
ruby -Itest test/models/location_test.rb  #raises pending migration error
```

###Re-Adding Performance Testing
在 Rails 4 中需要添加 gem 实现
```
gem 'rails-perftest'
gem 'ruby-prof'
```

###Skip

```ruby
class ItemTest < ActiveSupport::TestCase
  test "can shorten its name" do
    item = Item.new
    item.name = "Super-duper long name"
    assert_equal "Super-duper", item.short_name
  end
  test "doesn't shorten short names" do
		skip #Use "skip" method if you don't want to run a test right now
    item = Item.new
    item.name = "Name"
    assert_equal "Name", item.short_name
	end 
end
```

###Minitest Test_Opts
get passed as test parameters
Verbose mode shows run time and skip status for each test
```
rake test:models TEST_OPTS="--verbose"
Run options: --verbose --seed 57945
# Running tests:
ItemTest#test_can_shorten_its_name = 0.07 s = .
ItemTest#test_doesn't_shorten_short_names = 0.00 s = S
Finished tests in 0.076938s, 25.9950 tests/s, 12.9975
assertions/s.
2 tests, 1 assertions, 0 failures, 0 errors, 1 skips
```