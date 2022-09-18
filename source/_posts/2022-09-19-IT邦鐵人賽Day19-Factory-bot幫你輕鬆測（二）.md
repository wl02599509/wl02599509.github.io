---
title: IT 邦鐵人賽 Day 19 - Factory_bot 幫你輕鬆測（二）
date: 2022-09-19 00:34:56
category: iThome
tags: 
- iThome
- Test Automation
- Ruby on Rails
- Rspec
- Factory_bot
---
昨天介紹了超好用的測試輔助套件 factory_bot_rails，但內容太多，一天講完實在太冗長，所以我們分兩篇來講。

今天我們來講一些細節的東西。

<!--more-->

# Create / Build 的差別：

這部分我想有使用 Rails 的朋友應該都不是問題。

Build 會建立實體但不會寫入資料庫，如此便不會有 id 產生 ( id: nil )
Create 會建立實體，並寫入資料庫，所以會有 id 產生 (id: 1)

這在工廠也是一樣的，昨天我們提到用工廠建立出物件的方法是 build(:user) / create(:user)，那我們就來看他們的結果：

```ruby
build(:user):
<User id: nil, email: "laurice@crooks-graham.io", created_at: nil, updated_at: nil>
```

```ruby
create(:user):
<User id: 1, email: "marc.robel@glover.org", created_at: "2022-09-04 09:15:07.449708000 +0000", updated_at: "2022-09-04 09:15:07.449708000 +0000">
```

夠明顯了吧～

雖然 build 不會寫入資料庫，但使用 build 還是能進行測試驗證，不會因為他沒有寫入資料庫就無法驗證。

但這樣有差嗎？既然都能驗證，那我就都用 build 就好啦，但假如今天要測試的標的需要 id，那就必須使用 create 了。

比如，Article has_many Comments, Comment belongs_to Article，routes 為 resources :articles 包 resources :comments 這時如果要到 Article_comment_path 等等需要有 Article id 的 情況，就必須針對 article 使用 create 產生 id，否則測試到不了那個網址。


# Data_cleaner-active_record

我們在進行很多測試時，都會希望每一項的測試都是在乾淨的環境下執行，當我們使用 create 時，讓很多被 create 的標的一直留存下來。

可以試著開啟很多不同的 spec 測試，然後每個都 create 同一個東西，接著執行個好幾次測試後，再其中一個測試裡放入 binding.pry (類似 debugger 的套件)，去抓現在的 id 是不是好幾十號了。

這就代表，之前的測試留下了很多不必要的痕跡，這樣就會影響到測試。為了讓每一次的測試都是獨立又乾淨的環境，可以安裝 Data_cleaner-active_record
套件，讓這個套件幫你在每一次執行測試前先將資料清除。

裝好後記得把這段 code 貼到 spec_helper.rb 的 RSpec.config block 裡面：

```ruby
 config.before(:suite) do
   DatabaseCleaner.strategy = :transaction
   DatabaseCleaner.clean_with(:truncation)
 end
  config.around(:each) do |example|
   DatabaseCleaner.cleaning do
     example.run
   end
 end
 ```

裝了它之後，再重複用上述方法測試 id 是不是都是 1。

# Faker
在進行任何的測試，最好不要把資料寫死，這樣是不好的習慣，因此我們可以使用 Faker 套件，幫我們在工廠裡製造假資料。

```ruby
FactoryBot.define do
 factory :project do
   user
   organizer { Faker::Name.name   }
   email { Faker::Internet.email }
   phone { Faker::PhoneNumber }
   project_title { Faker::Food.dish }
   project_amount_target { Faker::Number.number(digits: 4) }
   project_description { Faker::Food.description }
 end
end
```

它可以幫你製造很多假東西，名字、電話、信箱、食物等等，至於他的詳細使用方法，可以到官網查詢～
# 小結
不知道大家有沒有覺得一個測試竟然要使用這麼多套件來輔佐，沒錯，我一開始就是這樣的感受，但沒有這些套件，測試會寫的很困難。

明天我們要介紹的測試框架，不屬於 TDD，但卻是使用 RSpec，不得不知道的一套框架：Capybara。 
