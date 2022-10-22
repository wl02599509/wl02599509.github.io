---
title: IT 邦鐵人賽第 29 天 - Cucumber in Rails
date: 2022-10-05 11:07:39
category: iThome
tags: 
- iThome
- Test Automation
- Ruby
- BDD
- Cucumber
---
前幾天介紹了 Cucumber，我想大家都對 Cucumber 有了認識，那今天我們要來在 Rails 裝上 Cucumber 以及其他相關測試套件。

<!--more-->

# Cucumber-rails

首先，打開 Rails 的專案，並在 Gemfile 裡加上:

```ruby
group :development, :test do
 gem 'cucumber-rails', '~> 1.4', '>= 1.4.3', require: false
 gem 'database_cleaner'
end
```

接著在終端機上執行 bundle 安裝套件，然後在終端機輸入 rails g cucumber:install 安裝 cucumber 測試相關檔案，主要就是 features 資料夾，以及 step_definitions 資料夾。

```ruby
      create  config/cucumber.yml
      create  script/cucumber
      chmod  script/cucumber
      create  features/step_definitions
      create  features/step_definitions/.gitkeep
      create  features/support
      create  features/support/env.rb
      create  lib/tasks
      create  lib/tasks/cucumber.rake
```

至於 database_cleaner 非必要，之前有提過也解釋過這個套件的用途。

# Cucumber with capybara

沒錯～又是 Capybara，由此可知 Capybara 真的好用。

不論是 RSpec 或是 Cucumber，都是會使用到 Capybara。

而 Cucumber 的套件在安裝時，也就包含 Capybara 了，不需要像 RSpec 一樣額外要再 require ‘capybara’。

不過 Capybara 當然是看需求使用。

並不是每次使用 Cucumber 或 RSpec 都要用到 Capybara。

# 小結

基本上，只要需要用到的套件安裝好，就可以開始執行測試，而寫法也都跟前面提到的差不多，如果想瞭解更多細節，可以到官網探個究竟。

明天就是最後一天了，讓我們來歸納總結吧～