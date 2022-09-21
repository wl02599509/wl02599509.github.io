---
title: IT邦鐵人賽Day22-Capybara額外補充
date: 2022-09-22 00:48:22
category: iThome
tags: 
- iThome
- Test Automation
- Ruby on Rails
- Rspec
- Capybara
---
在使用 Capybara 時，雖然語法看起來就是單單純純的去哪個頁面、按什麼按鈕、填什麼表單、然後驗證。這樣聽起來似乎並不難，但只要操作到外網或是使用套件，就沒有那麼單純了。

<!--more-->

我們接下來就來談談，如果某些操作流程必須跳轉到外網進行操作，以及如果有彈跳視窗時，該如何撰寫測試。

# 測試過程須操作外網

當我們操作網頁時，大多數都在自己的網站內操作，不過有時候還是會跳轉其他的網站進行操作，像是台灣的三大顏色金流，就是其中一個例子。

只要使用到該金流，都會在結帳刷卡時，跳到他們的網頁，選擇交易方式，填寫卡號，並進行交易。

所以在進行測試時，勢必會經過這樣的流程，那我們要如何讓 Capybara 進入外網呢？

## Rake::Test

在知道如何讓 Capybara 進入外網前，我們必須知道為什麼進不去？

Capybara 在預設的情況下是以 RakeTest 進行測試，因為 RakeTest，無法使用請求 http，所以測試就沒辦法連到外網。

而且在之前提到，Capybara 沒辦法觸發 JS，也是 Rake::Test 的關係。

那為什麼還要它？因為 Rake::Test 的測試速度很快。

## Selenium

在之前有介紹的設定瀏覽器模式，這是 Selenium 的功勞，讓 Capybara 能夠透過瀏覽器進行測試。

那我們如果想要讓 Capybara 進入外網，就是設定 Selenium，講白話就是設定成瀏覽器模式，如此便能進入外網:

```
visit ‘https://ithelp.ithome.com.tw/users/20149089/ironman/4955’
```

至於設定的方法前一篇有教到，這裡就不多說了。

# JS 的彈跳視窗 ( alarm )

當網頁出現彈跳視窗時，通常會是重要的提醒，或是防呆機制、再次詢問你的操作。

最常見的就是「確定要刪除」，底下有是與否的選項。

當網頁有這樣的互動出現時，要如何讓 Capybara 去選擇按鈕呢？

原本以為只要 click_on 那個是或否的 button 就好了，但我想的太天真了，Capybara 會找不到按鈕。

針對這個彈跳視窗，Capybara 有提供另外的方法：

```
accept_alarm
dismiss_alarm
accept_confirm
dismiss_confirm
accept_prompt
dismiss_prompt
```

這樣全部列出來應該蠻好懂的：

針對彈跳視窗的取消或確認去選 dismiss 還是 accept

然後依照彈跳視窗是什麼類型，alarm? confirm? prompt? 在 Rails 裡，應該比較常見 confirm。

最後就是寫出按下什麼連結或按鈕會出現彈跳視窗：

```
accept_confirm do
   click_link '取消交易訂單'
end
```

這意思就是，我要按下「取消交易訂單」，接著會出現 confirm 彈跳視窗，那我想要選擇  accept。

# 小結

以上是我在進行專案測試時，遇到的其中的問題。雖然 Capybara 很容易寫，但很多測試的方法，還是要碰到問題後才會知道的。

那關於 TDD 的測試與套件就介紹到這邊，明天我們來談談，TDD 的缺陷，並開始進入 BDD。

# 測試表單上傳圖片

當測試標的是需要上傳圖片的表單，針對上傳圖片，會有個按鈕讓你 upload files，並產生圖片。

這部分也有特定的方法：

```
attach_file('欄位名稱', File.absolute_path('圖片的絕對位置'))
```

欄位名稱可以用 class 或 id
 
以這為例的話
```
<input class="ml-5 input-focus" type="file" name="project[avatar]" id="project_avatar">
```

```
attach_file('project_avatar', File.absolute_path('app/assets/images/wubay.png'))
```

# 小結
今天補充了很多關於 Capybara 的一些狀況與用法，所以說，簡單的東西也是會有很細節很困難的操作，但其實一旦知道就覺得沒什麼。