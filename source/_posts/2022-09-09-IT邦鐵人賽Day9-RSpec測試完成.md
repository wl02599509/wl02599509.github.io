---
layout: IT 邦鐵人賽 Day 9 - RSpec 測試完成
title: 9-RSpec測試完成
date: 2022-09-09 00:16:26
category: iThome
tags: 
- iThome
- Test Automation
- Ruby
- Rspec
- TDD
---
不知道大家有沒有先試著自己做做看，然後自己測測看。其實可以自己玩玩看 rspec 指令所產出來的訊息，學習判斷訊息內容也是很重要的訓練，不只在測試，在任何只要是寫code的時候，都會執行出錯誤，隨之產生的錯誤訊息會告訴你錯的地方，有些甚至會說如何處理這項錯誤。

<!--more-->

好啦，我們的主題是測試，那就不説太多主題以外的東西，直接進行這篇要做的事情，把剩下的code都全部補上。

# 實作
先將 mobile_wallet_spec.rb 的第二個規格測試需要的 code 完成：

![https://ithelp.ithome.com.tw/upload/images/20220909/20149089qYiXXcmM6k.png](https://ithelp.ithome.com.tw/upload/images/20220909/20149089qYiXXcmM6k.png)

再來到 mobile_wallet.rb，針對扣款失敗的狀況，用判斷式表達，如紅匡所示。

只要扣款金額小於等於錢包餘額，就執行扣款。反之，只要扣款金額大於錢包餘額，就不進行扣款，既然不進行扣款，那錢包餘額自然就不會改變：

![https://ithelp.ithome.com.tw/upload/images/20220909/201490897eSLHgMJdM.png](https://ithelp.ithome.com.tw/upload/images/20220909/201490897eSLHgMJdM.png)

接著在終端機執行 rspec ./mobile_wallet_spec.rb 後，即可得到測試通過的畫面：

![https://ithelp.ithome.com.tw/upload/images/20220909/20149089Cm0CC1GGYr.png](https://ithelp.ithome.com.tw/upload/images/20220909/20149089Cm0CC1GGYr.png)

再來做最後一項測試，也是一樣重複步驟，先在 mobile_wallet_spec.rb 寫測試的code:

![https://ithelp.ithome.com.tw/upload/images/20220909/20149089cMBeHejnoZ.png](https://ithelp.ithome.com.tw/upload/images/20220909/20149089cMBeHejnoZ.png)

然後在 mobile_wallet.rb 寫要被測試的 code :

![https://ithelp.ithome.com.tw/upload/images/20220909/20149089vHdW0A0jTr.png](https://ithelp.ithome.com.tw/upload/images/20220909/20149089vHdW0A0jTr.png)

接著到終端機執行 rspec 進行測試：

![https://ithelp.ithome.com.tw/upload/images/20220909/20149089IMwIDin9zE.png](https://ithelp.ithome.com.tw/upload/images/20220909/20149089IMwIDin9zE.png)

這麼一來，測試都通過了！

當測試都通過後，代表的意義是，所有的程式碼都是能正常運作的。因為我們在寫 code，一定都是先求有再求好，先讓 code 能 run，接著再來思考效能、可讀性等等的程式碼優化與重構。

# 小結
在進行程式碼優化與重構時，就會更動到原本寫的程式碼，所以這時就可以邊寫邊測，如果變紅燈，記得趕快修復它，這就是測試的好處，能夠馬上發現問題。

如此一來，往後在維護這份專案，或是要針對這份專案新增功能時，都可以透過測試，來確保你的所作所為是否影響原本既有的功能。

一直到這裡，我想全程有跟著做的朋友，應該明白了測試的內容。不過測試的世界也是蠻龐大的，不同的程式語言都有不同的測試軟體或框架，而不同的專案環境與需求，可能也會需要不同的測試方法。

明天我們先來認識一下 RSpec 裡的基本語法。