---
title: Rake/in/Rails
date: 2022-08-14 20:57:37
categories: Ruby
tags:
- Rake
- Rails
- Ruby
---
# 將 Rails 環境帶入 rake 裡，讓 rake 可以使用 Rails 的元素，像是 Model：

```ruby
namespace :user do
  desc "建立使用者"
  task :init => :environment do # 讓 rake 可以引入所有 rails 的東西（ model, migration 等等）  
    User.create(email: Faker::Internet.email ,password: '123456')
    puts "user created"
  end
end
```
<!-- more -->

如果沒有導入環境給 rake ，rake 就找不到 User ，就沒辦法 new 了。

終端機輸入：
```
rake user:init
```

# 透過回圈，一個指令，產生多筆資料：

```ruby
namespace :user do
  desc "建立使用者"
  task :init => :environment do 
    5.times {
      user = User.create(email: Faker::Internet.email ,password: '123456')
      puts "user: #{user.email} created!"
    }
  end
end
```

# 更複雜的：

```ruby
namespace :blog do
  desc "建立初始資料"
  task :init => :environment do
    # init user & articles
    5.times {
      u = User.create(email: Faker::Internet.email, password: "123456")
      blog = u.create_blog(
        handler: Faker::Name.unique.name.downcase.gsub(" ", "_").delete("."),
        title: Faker::Lorem.sentence,
        description: Faker::Lorem.paragraphs(number: 2).join
      )
      puts "blog #{blog.handler} created"
      10.times {
        article = u.articles.create(
          title: Faker::Lorem.sentence,
          content: Faker::Lorem.paragraphs(number: 5).join
        )
        puts "  title: #{article.title} created"
      }
      puts "user: #{u.email} created!"
    }
  end
end
```