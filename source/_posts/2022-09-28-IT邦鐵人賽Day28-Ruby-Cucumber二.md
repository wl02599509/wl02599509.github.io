---
title: IT 邦鐵人賽 Day 28 - Ruby Cucumber(二)
date: 2022-09-28 00:12:47
category: iThome
tags: 
- iThome
- Test Automation
- Ruby
- BDD
- Cucumber
---
昨天我們安裝了 Cucumber 套件，然後也把規格寫好後，run 一遍測試，出現了訊息告訴我們有幾個 scenario 與 step，因為我們還沒有在 step_definations 資料夾裡新增規格要測試的檔案，所以訊息也有出現測試檔案內容的雛形 (太方便了)。

<!--more-->

今天我們就要將程式碼通通補上，來看看過程中的測試與訊息變化。

# 實作

先將 step 每個描述，轉化成程式碼，並寫在 step 底下：

![https://ithelp.ithome.com.tw/upload/images/20220928/201490894TFvyHD6Pn.png](https://ithelp.ithome.com.tw/upload/images/20220928/201490894TFvyHD6Pn.png)

執行 cucumber：

![https://ithelp.ithome.com.tw/upload/images/20220928/20149089g7HPPJTRtZ.png](https://ithelp.ithome.com.tw/upload/images/20220928/20149089g7HPPJTRtZ.png)

很棒，這是很正確的錯誤訊息，跟之前介紹 RSpec 時一樣，一堆的紅色錯誤訊息，上面寫著 unitialized constant MobileWallet…..，這樣就對了，因為我們根本還沒寫到實作的程式碼，測試碼當然找不到。

已經講解過這個錯誤訊息，這裡就不多廢話，我們就直接把實際的程式碼補上：

![https://ithelp.ithome.com.tw/upload/images/20220928/20149089x68DKa3K0b.png](https://ithelp.ithome.com.tw/upload/images/20220928/20149089x68DKa3K0b.png)

執行 cucumber 看看跑出什麼：

![https://ithelp.ithome.com.tw/upload/images/20220928/20149089gF9vKhsiQH.png](https://ithelp.ithome.com.tw/upload/images/20220928/20149089gF9vKhsiQH.png)

第一個 scenario 的 steps 已經全部 pass 了，剩下兩個錯誤的部分，我們在針對錯誤的部分修正一下實作的測試碼：

![https://ithelp.ithome.com.tw/upload/images/20220928/20149089AI6uNIW3kX.png](https://ithelp.ithome.com.tw/upload/images/20220928/20149089AI6uNIW3kX.png)

修改好後執行 cucumber ：

![https://ithelp.ithome.com.tw/upload/images/20220928/20149089qtSab3Ajhy.png](https://ithelp.ithome.com.tw/upload/images/20220928/20149089qtSab3Ajhy.png)

大功告成！所有的 scenario 都 pass 了！

# 小結

測試完成後，接著就可以開始進行測試碼的優化了，讓可讀性上升。

還記得為什麼每做一步就要 run 一次測試嗎？為了知道你的測試框架有沒有正常。如果你剛剛 cucumber 後，他跑出一堆 pass ，但你根本還沒寫，這豈不是測試框架出了問題嗎？（可能是小精靈幫我寫完了）

如果沒有發現到這個框架出現了問題，等全部寫完，才 run 第一次，那眼前出現的 pass 是真的 pass 嗎？


