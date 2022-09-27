---
title: IT 邦鐵人賽 Day 27 - Cucumber with Ruby
date: 2022-09-27 01:13:54
category: iThome
tags: 
- iThome
- Test Automation
- Ruby on Rails
- BDD
- Cucumber
- Gherkin
---
前幾天帶大家了解了 BDD 的 Cucumber 以及 Gherkin，相信大家對 BDD 的特質已有所了解。今天我們來安裝 Cucumber 套件，並寫寫看 cucumber。

<!--more-->

# 安裝
首先，在終端機上輸入指令：

```
gem install cucumber
```
 
安裝好後，可以透過這個指令

```
cucumber --init
```

產生 cucumber 所需要的資料夾與檔案：

![https://ithelp.ithome.com.tw/upload/images/20220927/20149089ZikC0jnDn7.png](https://ithelp.ithome.com.tw/upload/images/20220927/20149089ZikC0jnDn7.png)

說明一下這些資料夾的用意：

Gherkin 的文檔規格，檔名取為 xxx.features，是放在 features 資料夾裡。

在 Features 的 scenario 底下的 steps 則是對應到 step_definitions 資料夾裡的檔案，在 step_definitions 裡則是開始寫實作的程式碼。

# 規格

那當我們要開始撰寫時，先在 features 資料夾裡新增一個檔案叫 mobile.feature，這就是用來撰寫 Gherkin 文檔的規格：

![https://ithelp.ithome.com.tw/upload/images/20220927/20149089EVVHL1dtlo.png](https://ithelp.ithome.com.tw/upload/images/20220927/20149089EVVHL1dtlo.png)

接著我們在終端機執行 cucumber 來 run 測試，正確會得到下圖這樣子的訊息：

![https://ithelp.ithome.com.tw/upload/images/20220927/20149089PttJbbvyF8.png](https://ithelp.ithome.com.tw/upload/images/20220927/20149089PttJbbvyF8.png)
![https://ithelp.ithome.com.tw/upload/images/20220927/20149089D3a0GAZRvK.png](https://ithelp.ithome.com.tw/upload/images/20220927/20149089D3a0GAZRvK.png)

這段訊息蠻容易閱讀的，有我們打好的規格，以及統計幾個 scenario 與 steps。

下面黃色的部分真的超貼心的，那一堆 Given, When, Then 就是我們需要打在測試檔 xxx_steps.rb 的內容，系統直接幫我們產出來，我們只需要複製貼上：

![https://ithelp.ithome.com.tw/upload/images/20220927/20149089jhIaMB5ene.png](https://ithelp.ithome.com.tw/upload/images/20220927/20149089jhIaMB5ene.png)

把 block (do…end) 裡的 pending….. 刪掉後，就可以在 block 寫我們的程式碼了。

然後我們再執行一次 cucumber：

![https://ithelp.ithome.com.tw/upload/images/20220927/20149089SIQt8mori6.png](https://ithelp.ithome.com.tw/upload/images/20220927/20149089SIQt8mori6.png)

現在看起來更乾淨了，而且訊息一目瞭然！

# 小結

我想大家可以很清楚的知道，Cucumber 跟 RSpec 明顯的差異在哪。

那我們今天就先處理到這邊，明天我們再來把程式碼補上，然後順便也來看看錯誤的訊息會是長什麼樣子。