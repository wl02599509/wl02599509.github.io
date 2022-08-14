---
title: node & nodejs導致rails專案建立失敗
date: 2022-08-14 19:15:08
categories: Rails
tags: 
- Rails
- Installation
- Problems
---

# 前言

在第一次建立rails專案時，通常都會遇到不少問題，像我是使用windows系統，所以也經歷了一些麻煩。如果各位第一次建立rails專案就可以看到下圖綠色字體的成功字樣，那真的如天選之人般幸運阿~

<!-- more -->

<img>https://images.vocus.cc/85a5e902-1254-4734-8521-5eadc45ad863.jpg</img>

如果你跟我一樣注定要經歷windows系統的苦痛，就請繼續看下去吧QQ

---

# 遇到狀況

以下是我在建立rails專案時，所遇到的狀況：

$ rails new "project's name" 建立新專案，出現了「Node.js not installed. Please download and install Node.js」的錯誤訊息。

(記得：錯誤訊息不能忽略，否則會影響到日後的操作。)

<img>https://images.vocus.cc/11a096eb-00b0-4751-8775-df259a6a5a98.jpg</img>

## 解決辦法

這時只要按照後面給的網址下載node跟nodejs就好。

進入該網址後，往下拉點入install on windows subsystem for linux (wsl)，這是為了讓你在你的終端機上進行安裝。

<img>https://images.vocus.cc/0be7c56b-aba9-4256-aa93-91f61911f608.jpg</img>

<img>https://images.vocus.cc/ddfca0e3-ad87-4da0-aa49-c464e83ef3d6.jpg<img>

點進去後，往下拉到下圖的標題，開始按照步驟執行即可完成。

<img>https://images.vocus.cc/38342a17-8f73-45c2-baed-02aed737d521.jpg</img>

完成以上node跟nodejs安裝後，回到終端機上輸入rails new的指令建立專案應該就沒問題了。

---

# 但我有遇到新的問題：

<img>https://images.vocus.cc/5ad84922-fa56-41d0-b271-c768508bd975.jpg</img>

系統在建立rails專案過程中，在安裝 webpacker 時發生失敗，其中「nod的'GLIBC_2.28' not found」的問題，找了很多網站但還是找不到解決方法。

然而下面一行的「Webpacker 的安裝需要nodejs為10.17.0版本以上才能執行，而目前系統使用的是8.10.0版本」。                                                                       
(這部分不知道各位是不是都顯示8.10.0，但沒關係，因為你會看到這則Webpacker的錯誤訊息，就代表你的nodejs版本就是不符合他nodejs需要10.17.0版本以上的需求)

這個部分的問題，我嘗試了好幾遍後發現，只需要解決nodejs版本的問題就能夠順利建立專案(也就是不再有任何錯誤訊息，包含'GLIBC_2.28' not found)。

如果你在這時輸入指令確認版本 $ node -v 及 $ nodejs -v，可能會發現前者給你的訊息是剛剛看到的'GLIBC_2.28' not found，後者給你的訊息則是v8.10.0(也就是剛剛webpacker的錯誤訊息指出你nodejs正在使用的版本)。

## 解決辦法

那我們先把終端機關掉，重新開一個新的終端機，接著輸入指令$ nvm use --lts 將系統使用的node版本變成最新版本，輸入後便會看到系統說，正在使用 XXXX版(剛剛安裝的最新版)。

這時你在輸入指令確認版本 $ node -v 及 $ nodejs -v，前者不再出現'GLIBC_2.28' not found，而會出現 XXXX版，而nodejs還是顯示使用8.10.0版本。

<img>https://images.vocus.cc/5be5cad6-16d2-41be-aecc-a3bc99e52990.jpg</img>

確定了node -v 有出現最新版本，你就可以輸入rails new建立專案，此時應該就可以成功了。
但是使用 nvm use --lts 只是讓你變更當前使用的版本，所以你重開終端機，版本會回到原來的舊版本!!!

因此建議使用nvm alias default <version> 將預設版本變更成最新版本，往後建立專案就不必再遇到同樣的問題。

#設定v16.15.0為預設版本      #nvm alias default v16.15.0

即便你現在變更當前版本讓你可以順利建立專案，日後再啟動 rails server 時，會因為你當前使用的node版本太老舊(因預設是舊的)，而產生以下狀況：

<img>https://images.vocus.cc/6d41b45f-b44a-4a5d-a923-87ff69bad155.jpg</img>

所以建議更改node的預設版本，才能徹底解決問題。

---

### Reference
另外推薦這位KEN 陳建權大大寫的關於使用 NVM 管理 Node.js 版本文章。

因為上述的問題，會導向NVM / NODE / NODE.JS 這些背景知識，而該文章有簡單介紹NVM是什麼，以及相關的設定變更指令。我覺得非常清楚好懂，對新手非常友善。

如果你有遇到其他問題~希望你能分享上來一起想辦法~

希望這篇文章能夠幫助到各位閱讀者，祝大家順利~

