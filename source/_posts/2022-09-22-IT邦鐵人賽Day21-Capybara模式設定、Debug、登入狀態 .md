---
title: IT 邦鐵人賽 Day 21 - Capybara 模式設定、Debug、登入狀態 
date: 2022-09-22 00:45:16
category: iThome
tags: 
- iThome
- Test Automation
- Ruby on Rails
- Rspec
- Capybara
---
昨天介紹了 Capybara，提到這款套件是以「使用者」角度操作網頁來進行測試，所以它也可以真實呈現操作頁面的視覺畫面！

# Capybara 測試模式

<!--more-->
分為兩種，一種叫無頭模式，headless，也就是預設，在終端機上顯示而已。另一種是瀏覽器模式，也就是前言所說，像真人在操作畫面。

我們只要把：

```ruby
Capybara.default_driver = :selenium_chrome
```
放到 spec_helper.rb 裡就好。

Capybara 就會用 chrome 來展現測試給你看。當然還有其他瀏覽器的設定，有興趣可以到官網看看。

趕快試試看吧！

# Sleep 停下來多看幾眼

畢竟是測試，所以每一行 code 都 run 的很快，網頁也是操作的飛快。如果你想看清楚他的每一步，或是想在哪個步驟多停留一點時間，可以加 sleep method：

```ruby
  it "登入成功" do
    visit '/'
    click_on '登入'
    sleep 2
    within "div#sign_in_form" do
      fill_in '電子信箱', with: 'bot@bot.bot'
	sleep 1
      fill_in '密碼', with: '123456'
    end
    click_on 'Log in'
    sleep 60
    expect(page).to have_content "成功登入了。"
  end
```

這樣它就會在按下登入到登入畫面時，先停兩秒，在開始填表單，填完電子信箱後，停一秒，再填寫密碼，按下 Log in，到新頁面後停 60 秒，結束。

# Debug

如果用 capybara 測試時，網頁操作途中，某個步驟有問題，可以用 page.save_and_open_page ，把當時的網頁截圖下來並開啟給你看，讓你知道當下的網頁是哪裡。
使用這個方法必須安裝 launchy 套件。

# 登入狀態驗證

在做 web 時，通常會設定某些網址、某些功能的使用必須為登入狀態才能進入與使用，測試時，也不外乎也會受到登入驗證的阻擋。
假如我們要測發表文章，但發表文章必須先登入，那我在測試裡面，先寫登入，在寫發表文章就好啦～
  
```ruby
  it "文章新增成功"  do
    visit '/'
    click_on '登入'
    within "div#sign_in_form" do
      fill_in '電子信箱', with: 'bot@bot.bot'
      fill_in '密碼', with: '123456'
    end
    click_on 'Log in'
 
    visit '/projects'
    click_on "前往提案!!"
 
    within '.new_project' do
      fill_in '電子信箱 *', with: '123@123.123'
      fill_in '聯絡電話 *', with: '0980222222'
      fill_in '提案名稱 *', with: 'test'
      find("trix-editor").set("New value")     
    end
 
    click_on '新增Project'
    expect(page).to have_content '提案成功!!'
 end
```

似乎很合理，但這樣不對，這樣測試在跑發表文章時，一樣是未登入狀態，因為他每一步的操作都是獨立的，狀態並不會延續。
為了解決這個登入者驗證的問題，可以安裝 warden 套件，在測試裡面加個 before hook:

```
 before do
   user = create(:user)
   login_as user
 end
```

這樣他就會以這個使用者進行登入，然後開始進行後續測試。

除了放在 before hook 以外，也可以放在 it group 裡。

# 水豚君找不到 action_text 

當欄位使用 action_text 時，使用 fill_in 是找不到那個欄位的，這時要用：

```ruby
find("trix-editor").set("贊助")
```

這樣水豚君就找得到 action_text 的欄位，並且在該欄位內填上贊助兩個字了～ 

其實這個 find 蠻好用的，我會再另外寫一篇給大家看看常見的問題跟常用的方法。

# 小結

Capybara 是一套非常簡單但好用的測試套件，但有些細節還是需要遇到找到才知道的。

這裡主要是透過我自己在做專案寫測試時，碰到的問題與解決，記錄下來，但我想這些問題應該也蠻常見的，畢竟這些問題的操作都是很基本的使用者操作。