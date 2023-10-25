---
title: 2023 iT邦鐵人賽 Day 23 - Dockerfile快取與優化
date: 2023-10-25 21:45:47
categories:
- [iThome]
- [Docker]
- [Dockerfile]
tags: 
- iThome
- Dockerfile
---
# 前言

昨天跟前天已經將 Dockerfile 介紹完了，但學會建立後，我們再來要考慮的是效能。就像寫 code 一樣，不只能動，還要注重效能。

今天我們來講解，為何連 Dockerfile 也有效能問題? Docker 是怎麼處理快取? 那我們該怎麼解決效能上的問題?

<!-- more -->

# 為什麼 Dockerfile 的效能很重要呢？

當我們有使用 Docker 執行 `CI/CD` 時，那勢必會在 `pipeline` 依照 `Dockerfile` 跟 `docker-compose` 建立專案。我們也知道， `CI/CD` 都會花上很大一段時間（我的經驗是快一個小時），假使，我們在 `Dockerfile` 在建立 `image` 時，如果每次都要從同 build 到尾，把每一個映像層逐一建立起來，就會花上不少時間，尤其在某些安裝應用程式的階段 ( 比方 `RUN apt-get update && apt-get install -y build-essential libpq-dev nodejs postgresql-client imagemagick` )，我自己是有過，在這類的階段花上一小時的時間。

如果每次推一個新的 PR，都要重來一次，那真的會挺崩潰的。

# Docker cache

## 快取了什麼?

Docker 的快取機制主要是將**映像層**儲存下來。

其實這也不難理解。我們從第一天到現在，所有的操作中，只有兩個指令會花時間並產生一堆 log： `docker image pull` 、 `docker image build` 。還記得他們都會下載跟建立一堆映像層嗎？

由此可知，在 Docker 的世界裡，映像層是檔案傳輸的元素，所以將映像層快取，以便在下載或是建立映像檔時，能夠使用既存的映像層。

## 是誰負責?

Docker cache 主要是透過 Docker Engine 將映像層儲存下來。

## 如何辨別每一層?

每層映像層都有一組由該層的指令所構成的 `SHA256` 命名，所以每一層都是獨一無二的名字，Docker Engine 會將這些命名儲存下來，等同於將指令內容存下來，下次遇到一樣內容所構成一樣的 `SHA256` 時，就可以直接取用快取，不必再下載或是執行該行指令。

只要是在快取內的映像層，都會有 `Already exists` 的訊息:

```jsx
4ee097f9a366: Already exists
94662ba12854: Pull complete
595a05b7c0ad: Pull complete
03682c916abd: Pull complete
7e88a38e211a: Pull complete
```

# Dockerfile 的快取

基本上 Dockerfile 的快取機制跟上述說明的概念雷同，只要指令內容相同，Docker Engine 就會透過 `SHA256` 確認該層有無快取。

但是 Dockerfile 跟 `docker image pull` 時，對於快取的記憶還是有落差。Dockerfile 的快取還多了**順序性**。

在 Dockerfile 裡，我們會定義每一步驟要執行的指令為何，像這樣排列而成的順序，也是快取機制在識別時的認定基準。

假如 Dockerfile 有 7 行指令，7 個步驟，那我們如果在第三行的指令做了些更動，那再更動後執行 `docker image build` 時，從第三行開始，就會重新執行一遍，而不會發生第三行重新執行，接下來的第四到第七行拉快取過來。

# Dockerfile 優化

當我們談到 Dockerfile 的優化時，就是在說，我們該如何將 Dockerfile 的快取機制使用拉到最高最極限。

最重要的就是注意執行指令的順序。我們盡量把變動頻繁的指令往下放，讓變動較不頻繁的指令往上放，如此一來便能避免更動後，造成大部分指令要重新執行的窘境。這樣當我們有更動時，便能在再次執行時，只要重新執行有更動的部分就好，其餘就交給快取，以節省時間。

# 總結

今天的內容不會太難，算是一個簡單但又很重要的概念。我想大家都知道，時間很寶貴，應該不會有人希望花太多時間在等待上，所以未來如果有機會撰寫 Dockerfile 時，別忘了盡量做到優化。