---
title: IT 邦鐵人賽 Day 6 - 規格與第一個紅燈
date: 2022-09-06 23:36:23
category: iThome
tags: 
- iThome
- Test Automation
- Ruby
- Rspec
---
昨天我們在終端機上執行了指令，進行我們一直提到的「測試」。沒錯，在終端機上操作的指令與出現的訊息就是我們測試執行與結果。

接下來，我們就是要寫 code，測試，沒過，修改，寫 code，測試，一步一步循環到通過為止（但其實通過測試不能算是終點，因為要進行程式碼的優化與重構）。

<!--more-->

那我們繼續將昨天錯誤訊息所說缺少的 MobileWallet 的常數類別與測試需要的 examples 做出來。

# 實作
在一樣的目錄（資料夾）裡做一個檔案叫 mobile_wallet.rb，並在裡面寫一個 MobileWallet 的類別：

![https://ithelp.ithome.com.tw/upload/images/20220907/20149089R477O54jii.png](https://ithelp.ithome.com.tw/upload/images/20220907/20149089R477O54jii.png)

接著我們在到 mobile_wallet_spec.rb 裡加上 require “./mobile_wallet.rb” 
將 mobile_wallet.rb 引渡到 mobile_wallet_spec.rb 裡，讓mobile_wallet_spec.rb 知道要對哪個檔案進行測試：

![https://ithelp.ithome.com.tw/upload/images/20220907/20149089wK3wrbA6VE.png](https://ithelp.ithome.com.tw/upload/images/20220907/20149089wK3wrbA6VE.png)

完成後，再一次到終端機上輸入 rspec ./mobile_wallet_spec.rb 執行測試，這次出現的訊息完全不一樣了，沒那麼紅，多了很多美麗的色彩以及友善的文字：

![https://ithelp.ithome.com.tw/upload/images/20220907/20149089IIafu1zWgC.png](https://ithelp.ithome.com.tw/upload/images/20220907/20149089IIafu1zWgC.png)


最上面的三個米字＊是 pending 狀態的意思，也就是你的 code 都還沒有寫，提醒你要記得把code補上，而顯示幾個米字＊就代表你有幾個 examples（也就是 it )。

最下面黃色字體的部分，3 example，也就是有3個 it ，每一個 it 就是一個又一個的 example。

如果你把 code 補上，測試發生失敗時，米字＊就會變成 F，如果測試成功，米字＊就會變成一個點「 . 」。第幾個 it 的 code 測試失敗或成功，第幾個米字＊就會變成 F 或 . 。

假如上面顯示 .*F，代表你有三個 it 要測試，第一個 it 已經通過測試，第二個 it 仍然為 pending 狀態，你還沒針對第二個 it 寫上 code，第三個 it 有寫code 了，但內容不正確，code 有錯誤，要頭痛啦！


# 小結
目前為止，對於測試的樣子應該是更清楚了些，不會像一開始還搞不懂，測試是什麼，在測什麼，用什麼測，測了會怎樣。希望這樣一篇一點點內容可以讓大家容易理解並吸收，那我們接下來要把實際內容慢慢填上，看看過程會有什麼狀況發生。
