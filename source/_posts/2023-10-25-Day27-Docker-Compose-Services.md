---
title: 2023 iT邦鐵人賽 Day 27 - Docker Compose Services
date: 2023-10-25 21:50:30
categories: 
- [iThome]
- [Docker]
- [Docker Compose]
tags: 
- iThome
- Docker Compose
---
# 前言

昨天我們介紹了什麼是 Docker Compose，讓大家對 Docker Compose 有個基本的認識。

今天我們來仔細介紹，在 Docker Compose 裡，定義容器的 Services 裡，到底是用什麼指令來設定每個容器。

<!-- more -->

# Services 容器設定

我們在昨天有介紹到 Services 是什麼，也就是用來定義需要的容器，如其字面上的意思，服務。

如何定義服務，用容器，容器提供服務。那在 Docker Compose 定義容器的指令，跟之前在介紹容器指令時，用到的差不多。

## image

定義好 services 的種類後，再來我們就是要針對各個 service 去定義各自的該如何被建立。既然 Services 是用來定義容器，那我們就必須要給予映像檔:

```docker
services:
  postgres:
    image: postgres:16-alpine
```

## volumes

只要需要存放資料的容器，都需要掛載 `volumes` ，所以在範例上，身為資料庫容器的 postgres service 就需要這個酷東西:

```docker
services:
  postgres:
    image: postgres:16-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
```

## ports

這裡就不多解釋連接阜了，要特別留意的是，在資料庫對接的應用程式裡，有無設定資料庫的 port。可能要特別留意設定是多少，在 rails 中，透過 `database.yml` 來設定資料庫，由於我沒特別設定，所以是預設的 `5432` :

```docker
services:
  postgres:
    image: postgres:16-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
     - 5432:5432
```

## networks

為了讓容器之間能夠在同個網絡下彼此聯繫，我們要將每個容器都設定相同的 `networks` :

```docker
services:
  postgres:
    image: postgres:16-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
     - 5432:5432
    networks:
     - net
```

## environment

這個非常重要，用來設定建立容器時，環境變數要帶入什麼值，在範例中，由於資料庫都會需要輸入上述三項環境變數，其實就很像資料庫有設定登入的帳號密碼，我們如果要讓他運作，就必須要填上正確的帳號密碼:

```docker
 postgres:
    image: postgres:16-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
     - 5432:5432
    networks:
      - net
    environment:
      - POSTGRES_DB=Foo_production
      - POSTGRES_USER=Foo
      - POSTGRES_PASSWORD=Foobar
```

不過要注意，通常不太會把這麼重要的資訊大辣辣的直接寫在 `docker-compose` 上，此為文章範例，為求清晰才這樣寫來示範。

不一定只有資料庫，其他的映像檔若有設定環境變數，也會有需要帶入環境變數的時候。像是設定執行環境為 `test` 、 `development` 、 `production` 。

## build

這個太重要了 (有哪個不重要)，當我們在定義 Services 時，我們有說這是在定義我們想要運作的容器。那容器是用映像檔產生的，所以我們會用 `image` 來設定要使用什麼映像檔。那 `Dockerfile` 是用來建立映像檔的，我可以讓 `docker-compose` 使用我剛做好的 `Dockerfile` 嗎? 

當然沒問題!! `build` 來幫你!

```docker
services:
  rails:
    build:
      context: .
      dockerfile: Dockerfile
```

我用剛剛做好的 `Dockerfile` 來建立我的 rails 容器， `context` 是用來定義 `Dockerfile` 的目錄位置，因為跟 `docker-compose` 相同目錄，所以就 `.` 即可。接著用 `dockerfile` 告訴 `docker-compose` ，它需要找到的 `dockerfile` 檔案名稱叫什麼。沒錯 `dockerfile` 的檔案名稱不一定要稱作為 `Dockerfile` 。

## depond_on

這個主要是設定容器之間的相依性。在解釋相依性之前，我們要先知道，當現在有 APP 容器跟資料庫容器時，當容器啟動時，如果是 APP 容器先啟動，再啟動資料庫容器，這樣會造成 APP 容器再啟動時，因為連不上資料庫容器，而導致異常並讓容器退出，這時就只會有資料庫容器是在運作中，而 APP 容器處於退出狀態。

在這個案例中，一定要先讓資料庫容器先啟動，完成後，再啟動 APP 容器，所以我們必須在 `docker-compose` 中設定這種順序，這種 APP 依賴於資料庫的設定:

```docker
services:
  rails:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 3000:3000
    networks:
      - net
    depends_on:
      - postgres
```

我們讓 rails 依賴 postgres，代表 rails 會在 postgres 建立並啟動完成後，才去換它建立並啟動。

# 總結

今天把一些基本的設定都介紹完了，不過當然不只這些，還有更多，但礙於篇幅無法一次全部講完，所以就挑我在 side project 上所寫的來說。

明天開始，我們將進入新的題目，容器管理。