---
title: IT鐵人幫Day5-Ruby與RSpec安裝
date: 2022-09-05 21:15:47
category: iThome
tags: 
- iThome
- Test Automation
- Ruby
- Rspec
---
昨天解說了什麼是 TDD、TDD 的流程與原則 ，今天我們要先來安裝之後 TDD 實際測試示範時需要的 Ruby 跟 RSpec 框架。

# Ruby
首先我們到 [Ruby Version Manager (RVM)](https://rvm.io/) 
並複製紅匡的code，到終端機貼上並執行來下載RVM，Ruby版本管理套件

<!--more-->

![https://ithelp.ithome.com.tw/upload/images/20220905/20149089hWDMm35xO2.jpg](https://ithelp.ithome.com.tw/upload/images/20220905/20149089hWDMm35xO2.jpg)

下載完 RVM 後，在終端機上輸入指令 rvm list known 可以看到眾多 Ruby 版本可以下載

![https://ithelp.ithome.com.tw/upload/images/20220905/20149089P42RtX5NrB.jpg](https://ithelp.ithome.com.tw/upload/images/20220905/20149089P42RtX5NrB.jpg)

我們主要是使用 MRI Rubies，版本主要用 2.7.2 或 3.0.0，前者是較多人使用的版本，但裝最新的 3.0.0 也不會影響本系列的操作。

輸入指令 rvm install 2.7.2  /  rvm install 3.0.0 （端視你想安裝的版次）

跑完安裝後，可以輸入指令 rvm list 確認目前已經擁有的 Ruby 版次，預設與正在使用的版次為何。

![https://ithelp.ithome.com.tw/upload/images/20220905/20149089Rv6jPfdrJv.jpg](https://ithelp.ithome.com.tw/upload/images/20220905/20149089Rv6jPfdrJv.jpg)

接下來我們來安裝 Ruby 的測試框架 Rspec！

# RSpce
在終端機上輸入指令 gem install rspec 進行安裝。（測試套件有很多，有興趣可以另外找找～ Ruby on Rails 的主要測試套件是 RSpec 跟 minitest，對 minitest 有興趣可以去找找～）

![https://ithelp.ithome.com.tw/upload/images/20220905/20149089HChS8mJfKq.jpg](https://ithelp.ithome.com.tw/upload/images/20220905/20149089HChS8mJfKq.jpg)

看到終端機出現 gems installed 就代表安裝完成囉～

今天就忙忙安裝環境，如果安裝過程有什麼問題，都可以留言詢問，那麼接下來會慢慢進入測試的世界，希望大家都能不討厭測試ＸＤ