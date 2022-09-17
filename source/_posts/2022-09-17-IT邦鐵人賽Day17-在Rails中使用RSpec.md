---
title: IT 邦鐵人賽 Day17 - 在 Rails 中使用 RSpec
date: 2022-09-17 22:24:36
category: iThome
tags: 
- iThome
- Test Automation
- Ruby on Rails
- Rspec
---
前幾天我們介紹了 RSpec 如何進行測試，以及其各種在建立規格時會用到的方法，今天我們要把 RSpec 裝到 Rails 裡，讓 Rails 也可以進行測試。（言下之意便是，要在 Rails 裡使用 RSpec 需要另外的安裝方法）

<!--more-->

# New Project

最基本的，我們先創一個 Rails 的 新 project。

```
rails new <project_name>
```

如果想用既有的 project 來裝也可以，只是為了教學，所以從頭開始。


# Gem Rails-rspec


接著我們到 Rails 裡的 Gemfiles 檔案裡，輸入：

```
group 'development', 'test' do
 gem 'rspec-rails', '~> 5.1', '>= 5.1.2'
end
```

這段意思是，在開發與測試環境底下安裝 rspec-rails，後面的數字則是安裝的版本。

為什麼只有開發跟測試環境？因為進行測試的情境，應該不會是正式上線的時候吧！？ 


# Bundle Install


Gemfiles 裡有了剛剛輸入的 rspec-rails gem 後，就可以在終端機執行：

```
bundle install
```

來安裝在 gemfiles 的套件，也就是我們剛剛放入的 rails-rspec。


# Generate Rspec


再來一樣在終端機執行：

```
rails generate rspec:install
```

來產生下列 rails-rspec 執行測試時，需要的資料夾：

```
create  .rspec
create  spec
create  spec/spec_helper.rb
create  spec/rails_helper.rb
```

比較重要的是 spec 資料夾，前面提過，測試會在這邊進行。

再來是 spec_helper.rb 是跟 rspec 使用的設定有關，詳細的設定方法與細節，可以再到官網上查詢。

往後如果是使用指令在建立 model 或 controller 時，spec 資料夾就會自動產生相關測試檔案。

假如輸入： 

```
rails g model article title
```

就會產生：

```
create      spec/models/article_spec.rb
```

從資料夾的結構可以看得出來，要用來測試的標的，會放在 model 資料夾裡，同理，如果是 controller 則會在 spec/controller/XXXX_spec.rb。

那在測試時，只需要在終端機輸入 rspec 他就會自己去抓 spec 資料夾裡面的所有 _spec.rb 檔。

如果想測試特定檔案，就是把該檔案的目錄接在 rspec 指令的後面:

```
rspec spec/models/article_spec.rb
```

# Delete Test 

當我們裝好 rails-rspec 後，Rails 內建的 test 就可以手動把它刪除掉了，也就是一個就叫做「test」的資料夾。沒錯不用怕，刪掉他不會讓你的 Rails 壞掉。

# NOTE

要特別留意，每個 XXX_spec.rb 都會有一段 require ‘rails_helper.rb’，這段很重要不能刪掉哦

當 rspec 執行時，會先去跑 rails_helper.rb 跟 spec_helper.rb，跑完這兩個跟設定有關的檔案後，才會去跑 XXX_spec.rb，少了其中一個都會影響到 rspec 的執行。

# 小結

rails-rspec 算是一套很好裝的套件，跟著步驟一步一步做，都能裝好。

在使用 rails-rspec 對 Rails 測試時，通常還會需要其他套件來輔助，單靠 rails-rspec 單挑整個測試實在太累了。

接下來會陸續介紹相關的好用套件。
