---
title: 2023 iT邦鐵人賽 Day 21 - Dockerfile 簡介
date: 2023-10-25 21:43:22
categories: 
- [iThome]
- [Docker]
tags: 
- iThome
- Docker
---
# 前言

接下來的十天，將會開始依序介紹 `Dockerfile` 、 `Docker Compose` 、 `Docker Swarm` 與 `K8S` 。

那麼今天，我們會先稍微介紹 `Dockerfile` 是什麼。

<!-- more -->

# 什麼是 Dockerfile

Dockerfile 是一個檔案名稱，沒錯，就是叫 `Dockerfile` 的一個檔案，它是由 Docker 內建提供的腳本，由 Docker 指令與 Linux 指令所組成，用來打包應用程式並產生映像檔。

簡單來說，這個檔是用來**定義映像檔**的，也就是用來**製造出映像檔**的一個檔案。

以下為我 Rails 的 side project 中的 Dockerfile:

```docker
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

我們會在 `Dockerfile` 裡面編寫一個映像檔所需要的所有映像層。每一行指令都構成一個映像層，所以 Dockerfile 裡的每行指令，是建立映像檔時，所需建立的步驟，依序排列下去，就像前幾天介紹 image 提到的，映像檔是由映像層一層一層堆疊而成的，這一層又一層的映像層，就是來自 `Dockerfile` 裡定義的每一行指令。

# Image Build

 `Dockerfile` 檔案會放在 Rails 專案目錄中。只要有了這個檔案，就能夠透過 `build` 指令產生出我的 Rails 專案的映像檔。

在下 `build` 指令時，記得要將 `Dockerfile` 的目錄位置當參數加上去。

因為底下示範時，終端機的目錄位置就在專案裡，所以就只有 `.` ：

```docker
> docker image build -t ithome .

[+] Building 45.7s (14/14) FINISHED                              docker:desktop-linux
 => [internal] load build definition from Dockerfile                             0.0s
 => => transferring dockerfile: 389B                                             0.0s
 => [internal] load .dockerignore                                                0.0s
 => => transferring context: 2B                                                  0.0s
 => [internal] load metadata for docker.io/library/ruby:3.1.3                    3.4s
 => [auth] library/ruby:pull token for registry-1.docker.io                      0.0s
 => CACHED [1/8] FROM docker.io/library/ruby:3.1.3@sha256:c4d28f375a0addcf2d6fc  0.0s
 => [internal] load build context                                                1.3s
 => => transferring context: 585.91kB                                            1.2s
 => [2/8] RUN apt-get update && apt-get install -y build-essential libpq-dev no  8.4s
 => [3/8] RUN mkdir -p /src/app                                                  0.2s 
 => [4/8] WORKDIR /src/app                                                       0.0s 
 => [5/8] COPY Gemfile Gemfile.lock /src/app/                                    0.0s 
 => [6/8] RUN gem install bundler:2.3.26                                         2.2s 
 => [7/8] RUN bundle install                                                    28.6s 
 => [8/8] COPY . /src/app                                                        1.7s 
 => exporting to image                                                           1.1s 
 => => exporting layers                                                          1.0s 
 => => writing image sha256:3a9ce6c9fc49cda222868c7f058c87ef431ae2f6115dd8102e4  0.0s
 => => naming to docker.io/library/ithome                                        0.0s
```

上述為建立映像檔的過程，這是建立完成後的靜態樣子，實際上，在建立的過程中，會一步一步建立（一行一行跑）。

如果 `Dockerfile` 裡的定義沒有問題，就能建立成功，若有哪裡不對，就會在建立映像檔過程中報錯。

# 用自己建立的映像檔建立容器

既然已經建立映像檔了，那我們來確認能否正常建立並運行容器。

```docker
> docker image ls  先確認有沒有 Dockerfile 建立的映像檔
> docker container run -d -p 3000:3000 --name iThome ithome
```

為什麼是 `3000` ? 映像檔預設不是 `80` 嗎? 為什麼不是 `-p 3000:80` ? 這個明天會解釋。

`run` 之後，要記得先檢查容器的狀態，如果建立的映像檔沒問題，那就會看到容器顯示 `Up` ，如果映像檔裡有定義錯誤，狀態可能就會顯示 `exit` 或 `create` 。如果有錯誤，可以使用 `logs` 確認目前哪裡有錯誤，如果沒錯誤顯示 `Up` ，也可以用 `logs` 查看：

```docker
=> Booting Puma
=> Rails 7.0.7.2 application starting in production 
=> Run `bin/rails server --help` for more startup options
Puma starting in single mode...
* Puma version: 5.6.6 (ruby 3.1.3-p185) ("Birdie's Version")
*  Min threads: 5
*  Max threads: 5
*  Environment: production
*          PID: 8
* Listening on http://0.0.0.0:3000
Use Ctrl-C to stop
```

這個就是很典型的 `Rails` 伺服器啟動的 `logs` ，代表 `Dockerfile` 的定義從建立映像檔到建立容器都沒問題。

# 沒有 Dockerfile 也可以建立映像檔

除了用 `Dockerfile` 建立映像檔以外，我們其實也能用容器建立映像檔。

映像檔建立容器，而容器經過改造，可能增加或減少功能，這時，我們想要重複生產這個改造後的容器時，就需要有該容器的映像檔，好讓我們不斷產生改造後容器，所以我們可以將容器打包成映像檔。這樣我們就能夠不斷產生出改造後的容器。

也就是說， `A映像檔` —製造—> `A容器`—改造( 增減容器內套件或應用程式功能 )—> `A-1容器` — commit—> `A-1映像檔`

如此一來，便有 A映像檔與 A-1映像檔，分別可以製造 A容器跟 A-1容器。

- 可透過 `docker commit` 將指定的容器，建立成映像檔。
- `docker commit 容器名稱 映像檔名稱` : 以該容器現存狀態，建立成一個映像檔。

# 總結

今天我們介紹了 Dockerfile 是什麼，內容長什麼樣子。

這邊簡單總結一下，Dockerfile 是一個 Docker 提供的腳本檔案，檔案裡面用 Docker + Linux 指令構成，主要是定義出製作出一個映像檔所需要的每一步驟，而這每一行指令 ( 每一步驟 )，就是構成映像檔的映像層，所以我們才說，映像檔是由映像層一層一層堆疊 ( 指令一行一行執行 ) 來的。

明天我們來詳細解釋 Dockerfile 裡的指令是什麼，有哪些常見的指令。