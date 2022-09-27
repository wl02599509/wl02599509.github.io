---
title: IT 邦鐵人賽 Day 26 - Gherkin 的 Keywords
date: 2022-09-27 01:12:52
category: iThome
tags: 
- iThome
- Test Automation
- Ruby on Rails
- BDD
- Cucumber
- Gherkin
---
前一天介紹了 BDD 的測試框架 Cucumber，而該測試框架的規格書所使用的是 Gherkin，利用人類自己的文字，提升團隊的參與。其中 Gherkin 裡的 Keywords 正是扮演了舉足輕重的角色，今天我們就來詳細介紹這些 Keywords：

<!--more-->

# Feature

主要是描述測試的功能，通常後面會接一個清楚的功能名稱，接著底下可以選擇性地寫些描述，用來說明功能意義或是寫上一些商業邏輯。

![https://ithelp.ithome.com.tw/upload/images/20220926/20149089tk1pMq7hx2.png](https://ithelp.ithome.com.tw/upload/images/20220926/20149089tk1pMq7hx2.png)

# Scenario / Example

在這部分，主要是在後面接上一個簡明扼要的標題，用來敘述這個 Feature 的具體情境。

![https://ithelp.ithome.com.tw/upload/images/20220926/20149089tzk9xto4Qu.png](https://ithelp.ithome.com.tw/upload/images/20220926/20149089tzk9xto4Qu.png)

使用 Scenario 或是 Example 都一樣，兩者是同意的。圖片範例中的 Scenario 也可以換成 Example 。

通常一個 Feature 底下，可能會有好幾個 Scenario / Example ，而每一個 Scenario / Example 是由 3 - 5	個 steps 所組成。建議 steps 最多 5 個就好，以免導致可讀性下降。

這裡指的 steps 是接下來要介紹的 given, when, then, and, but。

# Given

用作描述情境條件，或是一個在行為開始之前系統須先準備好的資料，也就是行為開始前的條件預設。

在開始測試時，系統會先將 given 給予的條件產生相對應的物件或資料庫內的資料，好讓系統先處於一個準備好的狀態。

![https://ithelp.ithome.com.tw/upload/images/20220926/20149089APVVAdixoo.png](https://ithelp.ithome.com.tw/upload/images/20220926/20149089APVVAdixoo.png)

# When

用來描述行為的開始、執行或是事件的發生。
這可以是使用者與系統之間的互動，也可以是被其他系統觸發所產生的事件。

![https://ithelp.ithome.com.tw/upload/images/20220926/20149089bWjUkxqMhC.png](https://ithelp.ithome.com.tw/upload/images/20220926/20149089bWjUkxqMhC.png)

建議讀者在每個 Scenario 裡，只要有一個 When 就好，如果覺得一個 When 不夠用的話，那可能要考慮將這個 Scenario 拆成兩個以上的 Scenario。

# Then

這個比較單純一點，就是用來描述輸出的結果、預期的值。

![https://ithelp.ithome.com.tw/upload/images/20220926/20149089k9maFYvXm5.png](https://ithelp.ithome.com.tw/upload/images/20220926/20149089k9maFYvXm5.png)

# And, But

## 當我們的 Given, When, Then 需要兩個以上時

![https://ithelp.ithome.com.tw/upload/images/20220926/20149089FZ85N5xq2y.png](https://ithelp.ithome.com.tw/upload/images/20220926/20149089FZ85N5xq2y.png)

## 就會使用 And 或 But 來代替，並依照語意，決定要用 And 還是 But。

![https://ithelp.ithome.com.tw/upload/images/20220926/20149089ZkHTXrh5P0.png](https://ithelp.ithome.com.tw/upload/images/20220926/20149089ZkHTXrh5P0.png)

## 此外，我們還可以使用星號 * 來取代 And 跟 But。 

![https://ithelp.ithome.com.tw/upload/images/20220926/201490896vjAhnlhz6.png](https://ithelp.ithome.com.tw/upload/images/20220926/201490896vjAhnlhz6.png)

# 小結
今天主要是介紹最主要的 Keywords，如果想瞭解更多，可以到 Gherkin 的官網閱讀文件。接下來我們要來介紹 Cucumber 框架啦～