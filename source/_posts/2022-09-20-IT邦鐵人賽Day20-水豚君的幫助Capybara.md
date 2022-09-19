---
title: IT 邦鐵人賽 Day 20 - 水豚君的幫助 Capybara
date: 2022-09-20 01:48:17
category: iThome
tags: 
- iThome
- Test Automation
- Ruby on Rails
- Rspec
- Capybara
---
今天我們來介紹這款我超喜歡的測試框架：capybara。Capybara 是用來跟 Rspec-rails 搭配使用，也就是 Rspec-rails 的輔助工具，主要是以「使用者」的角度，以網頁操作進行測試。

與其文字解釋，不如直接看 code，因為 capybara 的 method 非常簡單明瞭，所以一看就知道到底在幹嘛：

<!--more-->

```ruby
RSpec.describe "member_login", type: :feature do
  before do
    user = create(:user)
  end
 
  it "登入成功" do
    visit '/'
    click_on '登入'
    within "div#sign_in_form" do
      fill_in '電子信箱', with: 'bot@bot.bot'
      fill_in '密碼', with: '123456'
    end
    click_on 'Log in'
    expect(page).to have_content "成功登入了。"
  end
end
```

是不是非常清楚？我想這個英文程度應該難不倒大家，我也不需要多作解釋，那今天就到這邊吧～

沒有啦～不免俗的還是要從安裝套件開始說起啦～


# 安裝 Capybara

其實也不需要特別去 RubyGem 找套件，因為 Rails 的 gemfile 裡面就有了(今天這篇好好賺)。

# 使用 Capybara

若要在 rspec 裡使用 capybara 的 methods，那就必須要在 spec_helper.rb 裡，把 require 'Capybara' 加進去，讓 rspec 能夠抓到 capybara 。

接下來就可以盡情使用 capybara 了！

# Capybara methods

因為 capybara 是透過 code 來模擬人在使用網站，所以大部分的 methods 都會跟前往某個網址、點擊某個按鈕連結、填表單有關係，也就是大部分操作網站會做的動作。

但這些操作網站的動作要如何得到驗證？畢竟這還是在測試，一定是要有個執行結果與預期之間的比較。

讓我們用剛剛的 code 來說明一下：

```ruby
RSpec.describe "member_login", type: :feature do
  before do
    user = create(:user)
  end
 
  it "登入成功" do
    visit '/'
    click_on '登入'
    within "div#sign_in_form" do
      fill_in '電子信箱', with: 'bot@bot.bot'
      fill_in '密碼', with: '123456'
    end
    click_on 'Log in'
    expect(page).to have_content "成功登入了。"
  end
end
```

看到 type: :feature 很明顯可以知道在測試功能。

用 before hook 先在資料庫新增使用者，也就是先註冊好，才能登入。

it group 開始測試

visit 後面接的是要前往的網站網址，也可以使用 prefix

click_on 是 click_link 跟 click_button 的結合，後面接的是，你希望它按的 link 或 
button 的 html class, id 或是 value

within block: within 的範圍內，fill_in 什麼欄位 with 什麼值，這步驟就是在填寫表單

填寫完後，按下 Log in button or link

預期頁面裡的內容會有 “登入成功了。”

基本上語法就是這麼簡單，而 method 後面可以透過 html 的 class, id 來找到表單、欄位等等跟 html 有關的東西。

# 小結

capybara 屬於 Acception test 驗收測試的測試框架，用來作為專案收尾所使用的測試。

如果想知道更多的詳細方法，可以到官網上看，這裡就不多贅述了，不過方法大致上都脫離不了 visit, click, within, fill, has/have_content。


下一個章節會說明一些有趣的小設定、一些常見的問題，以及我在使用上遇到的問題。
