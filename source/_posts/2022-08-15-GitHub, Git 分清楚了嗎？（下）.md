---
title: GitHub, Git 分清楚了嗎？（下）
date: 2022-08-15 22:00:14
categories: Git
tags: 
- Git
---

# 前言

上一篇，我們主要介紹了 GitHub，這篇我們就來講講跟他名字很像，但實質上完全不同的 Git 工具。

簡單來說，我們可以透過 Git 指令，讓檔案進行版控。

<!-- more -->
---

# 基本操作

這個部分主要會講述最常被使用到的指令。

## 如何開始進行版控？

假如我們建立了一個資料夾 Taiwan，裡面有很多檔案，像是 taipei.rb, NewTaipei.js 等等。

### git init

如果我想對這個 Taiwan 資料夾開始納入版控，就在終端機上（目錄為該資料夾）輸入 git init，對該資料夾進行 git 初始化。

如此一來，Taiwan 資料夾就遭受版控命運。

#### 那可以逃脫版控命運嗎？

可以的，當 git init 時，會在該資料夾建立一個 .git 資料夾，只要刪除它，就能脫離版控囉！

### git add / git commit

當 Taiwan 資料夾進入版控，我們就可以透過指令對每個資料夾版控。

在說明指令前，我們要先解釋很重要的概念。

#### 工作目錄、暫存區、儲存庫

上述三個區域，是 Git 世界裡，檔案所儲存的不同位置，透過 git 指令把檔案移動到各區域。

##### 工作目錄

所有的檔案一開始都在這個區域。

##### 暫存區

在工作目錄裡的檔案，如果內容有任何修改，就可以使用 git add 指令，將該檔案移至這。

比方:
1. 針對特定檔案：git add NewTaipei.js
2. 針對目前終端機所在位置的所有檔案： git add .

##### 儲存庫

使用 git commit -m "文字敘述" 指令，可以讓暫存區的「所有」檔案，都移動到這，並給每個檔案都記錄上文字敘述。

比方： git commit -m "此次修改新增了新北的美食"

這個 commit，會產生一串屬於這次 commit 的編號，每一次的 commit 都有各自的編號。

如果用方向來表示，commit 是會一直往前進，然後指向前一個 commit。

比方：commit 了三次（依序為ABC）： commit A <-- commit B <-- commit C。

### git push

當檔案經過 git commit 到了儲存庫後，便能使用 git push 將檔案上傳至 GitHub 上。

（ 在 push 之前，其實還需要設定 SSH 的 key ）

---
# 分支系列

在 git 裡進行版控時，有分支的概念，可以把分支想像成是一張貼紙，貼在 commit 上，並跟著每一次的 commit 往前進。

分支的預設為 master 或 main。

## git branch

我們可以透過 git branch 來查看目前的分支有哪些，然後按下鍵盤的Q，即可離開查閱模式。

在好幾個 branch 裡，前綴有個 * 為目前所在的分支，也就是 HEAD。

那如果想要建立新的分支 dog，則可以透過 git branch dog 來建立。

刪除分支就用 git branch -d dog。

## git merge

看指令的意思可以清楚明白是要進行分支的合併。

git merge cat
將目前 HEAD 指向的落後分支合併到 cat。
---

# 反悔系列

萬一我對這次的 commit 反悔了，可能是我不小心 git add 了我不想加進來的檔案，也有可能是我 commit 的敘述沒有打好。

## git reset

直接上範例來解說：

git reset 11ed0bd^   
從 commit 編號11ed0bd 返回前一次的 commit。^的意思是前一次。而這個符號 ^ 叫 Caret。

git reset 11ed0bd^^  
兩個 Caret 則是返回前兩次。

git reset 11ed0bd~n  
多個波浪加上 n 代表返回前 n 次。

git reset 314994f  
只有編號，則是返回到指定的 commit    
白話文為，我想要變成 314994f 的狀態。
其實我們可以把 reset 解釋成 become。

git reset main / cat / HEAD  
後面接上分支名稱，返回到特定標籤、分支。

git reset HEAD^ 
這是以 HEAD 為基礎往回一次。
<em>使用 zsh 會顯示 zsh: no matches found: HEAD^，HEAD\^即可</em>

### reset 的三個模式

git reset 其實有三種模式，不同模式會對被後悔的檔案們有不同效果：

--mixed : 丟回工作目錄。（預設）

--soft : 丟回暫存區。

--hard : 直接丟掉刪掉。（但其實不會是真的刪掉）

（reset後的commit也不會真的不見哦）

### 我後悔了我剛剛的後悔！

如果 git reset 後，我後悔了，該怎麼辦？

使用 git reflog 找出 HEAD 的紀錄，知道 reset 以前 head 所在的 commit 編號後，再輸入 git reset commit_number --hard 就回來了。

---

# clone, fetch, pull

## git clone

在第一次什麼檔案都沒有的時候使用。

這個指令會把檔案複製下來，連同 git log 所有紀錄也會一起抓下來。

如果再重複執行第二次，則會顯示已經存在。

git clone http / ssh <任意字>

會將檔案抓下來後改名為 <任意字>

## git fetch

把線上有、但自己本機沒有的 commit 拉下來，並在自己本機增加新的 commit，並且 HEAD 會多一個前綴，像是 origin / M ，所以需要再另外將本機原本的 main / master 快轉到新節點（merge / reset）

## git pull

git pull = git fetch + git merge

會先 git fetch 然後再進行 git merge。

```ruby
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
展開物件中: 100% (3/3), 658 位元組 | 219.00 KiB/s, 完成.
來自 github.com:kaochenlong/dummy-git
   2286258..a414d4b  master     -> origin/master
更新 2286258..a414d4b
Fast-forward
 ThisIsAFile.rb | 1 +
 1 file changed, 1 insertion(+)
```

# 小結

本篇介紹了許多 Git 常會用到的指令，除了認識 Git 以外，更能明白與 GitHub 之間的差別到底有多大。

由於 git 指令數量眾多，且細節繁瑣，如果對 git 有興趣，不妨可以去學習。