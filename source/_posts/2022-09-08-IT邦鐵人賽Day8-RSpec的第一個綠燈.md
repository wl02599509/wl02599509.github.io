---
title: IT 邦鐵人賽 Day 8 - RSpec 的第一個綠燈
date: 2022-09-08 00:17:08
category: iThome
tags: 
- iThome
- Test Automation
- Ruby
- Rspec
- TDD
---
現在，我們已經開始進入寫測試 code 的階段了。今天要做的是在 Rspec 的規格裡，補上要測試的 code，那我們先從第一個規格描述 it 著手吧！也就是 it "軟體錢包原本有50元，扣款10元後，錢包顯示40元"

在開始寫之前，我們必須先想一下，這整個功能裡面，需要哪些變數，要叫什麼名字，需要什麼方法，那方法又要叫什麼名字（取名才是最難的...）。

類別名稱已經確定是 MobileWallet 了，類別產出的實體就取名為 wallet 吧，那扣款所需要的扣錢方法就取名為 cost (簡單明瞭)，最後我們還需要一個方法來得知扣款後還會有多少錢，所以這個方法就叫 total 吧！

![https://ithelp.ithome.com.tw/upload/images/20220908/20149089hvPKFxzVQG.png](https://ithelp.ithome.com.tw/upload/images/20220908/20149089hvPKFxzVQG.png)

上圖為我們在測試規格的檔案裡 (mobile_wallet_spec.rb) 把 code 補上的樣子。這段 code 就是用來表示，這項受測功能要如何被測試。在終端機打上執行測試的 rspec 指令後，它就會照上面打的code去run測試：
1. 將 new 的實體 wallet 產出後，隨即帶入引數 50 
2. 呼叫 cost 方法，把引數 10 帶進去來計算
3. 期望 wallet.total 的值是 40
只要測試符合期望，wallet.total 的回傳值為 40 時，測試就會成功，反之就是失敗。

那我們先來執行一次 rspec 看看會出現什麼訊息？

![https://ithelp.ithome.com.tw/upload/images/20220908/20149089f9p8WbJsBA.png](https://ithelp.ithome.com.tw/upload/images/20220908/20149089f9p8WbJsBA.png)
![https://ithelp.ithome.com.tw/upload/images/20220908/20149089jiQU4MRcbM.png](https://ithelp.ithome.com.tw/upload/images/20220908/20149089jiQU4MRcbM.png)

Pending 的部分先略過不看，因為其他兩個還沒做，所以會出現 Pending 是正常的。

我們先看最上面的 F**，意思是第一個 example 所寫的 code 有問題，什麼樣的問題呢？

看錯誤訊息1) 的部分可以知道，我的 new 方法後面帶入引數，代表我應該是有個帶參數的 initialize 方法。因為我們現在只有寫測試規格的code，還沒把被拿來進行測試的 code 寫出來，所以會出現錯誤是正常的。

那我們連 cost 跟 total 方法都沒寫，為什麼錯誤訊息只有提到 new 而已？
因為RSpec在run規格的code時，執行第一步 wallet = MobileWallet.new(50) 就已經失敗了，所以就沒有繼續測下去，錯誤訊息當然只提到 new 的部分而已。

我們繼續把 code 補上：

![https://ithelp.ithome.com.tw/upload/images/20220908/20149089mEXxVHZJiL.png](https://ithelp.ithome.com.tw/upload/images/20220908/20149089mEXxVHZJiL.png)

完成後，執行一下 rspec ./mobile_wallet_spec.rb 應該就會顯示非常乾淨的訊息：

![https://ithelp.ithome.com.tw/upload/images/20220908/20149089wy1m60pf9f.png](https://ithelp.ithome.com.tw/upload/images/20220908/20149089wy1m60pf9f.png)

首先，看到最上面顯示 .** ，那就代表第一個規格測試已經通過囉，剩下兩個仍然為 Pending 狀態的規格還在等著我們完成它們。

到這裡不知道大家有沒有發現，一開始我們還沒有寫實際的 code ，竟然就先把實際內容的方法名稱( cost, total )都先想好了，隨後再補上它們實際的 code，這就像先取小孩名( cost, total )再生小孩( 寫出 initialize, cost, total 的code )。其實這也是寫測試的困難之一，要預先想好介面，再逐一建立 code。

# 小結
明天我們就直接把所有的 code 寫完，那大家也可以先嘗試看看如何完成這個測試。自己動手做，然後自己測試看看，如果失敗就看一下終端機出現的訊息，看能不能找出解決方法。
