---
title: 2023 iT邦鐵人賽 Day 24 - 多階段Dockerfile
date: 2023-10-25 21:47:14
- [iThome]
- [Docker]
- [Dockerfile]
tags: 
- iThome
- Dockerfile
---
# 前言

今天是我們最後一天在 Dockerfile 的主題上圍繞了，能講想講的東西太多，只能挑一些我覺得蠻基礎且必須知道的東西來說說。

今天的主題 - 多階段 Dockerfile，其實就是在 Dockerfile 中建立不同的階段，讓映像檔能有個更完整的建立過程。

<!-- more -->

# 範例

我們直接來看範例說明：

```jsx
FROM ruby:3.1.3 AS build

ARG ROOT=/src/app

RUN apt-get update && apt-get install -y build-essential nodejs yarn libpq-dev postgresql-client imagemagick

WORKDIR ${ROOT}
  COPY Gemfile Gemfile.lock ${ROOT}/
  RUN gem install bundler:2.3.26
  RUN bundle install

FROM ruby:3.1.3 

WORKDIR ${ROOT}
  COPY . ${ROOT}
  COPY --from=build ${ROOT}/ ${ROOT}/

ENV RAILS_ENV=production

EXPOSE 3000

CMD sh -c "rails server"
```

## AS

這是用來定義「階段」的指令。會擺在 `FROM` 映像檔後面，用來定義目前在這映像檔底下所建立的每一層映像層是屬於什麼階段。

如範例第一行 `FROM ruby:3.1.3 AS build` 就是將底下指定為 `build` 階段。階段的定義 (取名) 都是隨自己，沒有固定或規定的格式，也可以叫 `gem` 、 `install` 等等符合該階段的行事。

通常會將整份 Dockerfile 依照行為來分階段，如範例，將建置環境定義在 `build` 階段。

## 範例解釋

除了 `AS` 剩下的指令都算是熟悉的，那我們就來解釋上面的範例在做什麼。

首先定義 `build` 階段，並在該階段使用 `ruby:3.1.3` 映像檔，接著設定變數、執行 `apt-get` 安裝應用程式執行時，在環境內需要的其他應用程式，然後設定工作目錄，並在工作目錄底下將專案的 `Gemfile` 跟 `Gemfile.lock` 複製到指定的變數目錄下，接著安裝並執行 `bundle install` 。

再來定義最終階段 ( 第二個 `FROM ruby:3.1.3` ，這時可用 `AS` 命名階段也可不命名 )，設定工作目錄，並將目前專案的目錄跟特定階段 `--from=build` 的目錄複製到指定目錄。如此一來，我們在最終階段，只需要把前面階段所建置好的東西都 `COPY` 過來就好。

# 多階段優點

透過多階段，我們可以將環境建置跟一般設定區分開來，這樣在不同的階段之間，不會影響彼此的快取。

也就是說在 `build` 階段如果有更動，像是增加了 `gem` 套件，那就會影響該映像層，這樣就會產生前一天提到的快取順序性被破壞，導致更動後的所有映像層都要重新 run 過，浪費時間。

所以透過多階段將行為分開，我們把容易變動的放在 `build` ，不常變動的設定放在最終階段處理，這樣就可以享受到 Docker 快取的機制，又可以讓 Dockerfile 裡的行為更明確區分。

# 總結

今天的內容不多，大家可以輕鬆學，明天我們來談談容器的自我檢測。其實是透過 Dockerfile 讓被建立起來的容器，透過檢測的指令與回傳的結果，來決定容器的狀態。藉此，我們可以及早發現問題，及早解決問題。