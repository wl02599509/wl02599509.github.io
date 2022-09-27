---
title: IT 邦鐵人賽 Day 25 - BDD 測試框架 - Cucumber
date: 2022-09-27 01:10:53
category: iThome
tags: 
- iThome
- Test Automation
- Ruby on Rails
- BDD
- Cucumber
- Gherkin
---
昨天介紹了 BBD 的存在是為了讓專案開發的所有相關人，無論是否有程式背景，都能夠了解測試的內容。那麼今天我們來介紹 BDD 的測試框架：

# Cucumber

<!--more-->

Cucumber 是一款 Ruby 常用的 BDD 測試框架（所以前幾天安裝的 Ruby 還是用得到唷），用於功能自動化測試 (廢話) ，但跟前面提到的 TDD 測試框架 Rspec 不一樣的是，Cucumber 的規格撰寫，可以使用中英日韓法等等四十種以上的人類語言來表達，而這份規格文件，也可以拿來跑測試，所以相較 TDD 含有程式碼的部分，BDD 確實能增加團隊間的溝通順暢，以降低溝通不良造成的龐大成本。

除了 Ruby 以外，也支援其他多種程式語言：（圖片來源：Cucumber官方）


![https://ithelp.ithome.com.tw/upload/images/20220925/20149089548PVIP5NU.png](https://ithelp.ithome.com.tw/upload/images/20220925/20149089548PVIP5NU.png)


# Gherkin

Cucumber 的規格書所使用的撰寫語法就是 Gherkin，其支援多國語言，撰寫方式是以狀況情境，一個一個區分開來，把每個情境的每個條件，步驟都用人類語文描述出來。

其使用的 Keywords 為：Feature (功能)、Scenario (情境)、Given (條件)、When (開始執行時的操作)、Then (輸出的結果)、And (當需要一個以上的 Given, When, Then 時，用 And )、But (同 And，但語氣(意)不一樣)。

一個 Feature 底下可以有多個 Scenario，每個 Scenario 裡含一個 Given、多個 And 或 But (如果需要的話)，一個 when，一個 then，多個 And 或 But (如果需要的話)。 

![https://ithelp.ithome.com.tw/upload/images/20220925/2014908957ASTQGOcd.png](https://ithelp.ithome.com.tw/upload/images/20220925/2014908957ASTQGOcd.png)

（規格書範例）

由上圖規格書的範例可以很直接地感受到，什麼叫溝通順暢，在座的讀者請爸爸媽媽來看一定也都看得懂，這就是為何 BBD 能夠讓非工程師背景的人員也能清楚理解測試的狀況：情境、條件、結果會是什麼。

# 小結
今天先介紹到這邊，接下來會詳細介紹 Gherkin 的每個 Keywords 以及規格的設計原則。