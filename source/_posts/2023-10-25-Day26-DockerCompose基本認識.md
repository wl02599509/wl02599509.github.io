---
title: 2023 iT邦鐵人賽 Day 26 - DockerCompose基本認識
date: 2023-10-25 21:49:27
categories: 
- [iThome]
- [Docker]
- [Docker Compose]
tags: 
- iThome
- Docker Compose
---
# 前言

在昨天的總結，其實就可以是今天的前言了。當我們需要建立多個互相溝通的容器時，靠指令一個一個慢慢打，還要注意彼此的啟動順序，一來一往，出錯機率大幅提升。

利用 `Docker Compose` 可以讓我們省去輸入一堆指令的麻煩，用一個檔案就能代替一堆指令。

<!-- more -->

# 範例

先讓大家看過，Docker Compse 長什麼樣子，這樣介紹起來，會比較有概念。

Docker Compose 基本上跟 Dockerfile 一樣都是 yaml 檔:

```docker
version: '3.8'

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
volumes:
  postgres_data:
networks:
  net:
```

## 執行 docker-compose

當我們定義好 `docker-compose.yml` 後，我們就可以透過 `docker-compose.yml` 開始建立並運作所有的容器。

透過 `docker-compose up` 來執行，一鍵完成所有容器的建立!

這樣就省去了我們在前言提到的，再靠 docker 指令一個一個慢慢輸入的問題。

也可以加上 `-d` 在背景處理，這樣就不會在終端機上運作了。

執行完畢後，可以使用 `docker-compose ps` 來查看透過 `docker-compose` 所建立並在運作中的容器們。

如果想要增加某個 service 的容器數量時，可以使用 `--scale` 選項，後面則指定哪個 service、多少數量: `docker-compose up --scale postgres=5`

## docker-compose 的運作

`docker-compose` 指令會透過 Docker Compose Yaml 檔案的內容，向 Docker API 發送指令，再由 Docker API，把指令內容傳遞給 Docker 應用程式。Docker 屬於容器工具，負責容器本身的生命週期，所以 Docker 本身並不負責 Docker Compose，簡單來說，他認不得 Docker Compose 的內容，所以需要透過 `docker-compose` 把內容告訴 Docker，讓 Docker 聽命行事。

Docker 會分配 IP 位置給容器，透過 IP 來發送請求，讓容器之間可以互相溝通。Docker 透過 DNS 服務器，讓 Docker 可以在不知道容器內部的 IP 的情況下，透過網域名稱找到相對應的容器，而這裡的網域名稱，就是容器的名稱。

# Docker Compose 基本要素

那接著我們要透過上面的範例來講解在 `docker-compose.yml` 裡的指令。

## Services

services 主要是定義欲運行的容器，從上面範例中擷取部分來說明:

```docker
Services:
	rails:
  postgres:
```

這是在定義會有兩個容器，分別為 rails 跟 postgres，名字隨便你取，主要是要能一看就知道有什麼樣的容器，在這邊也可以取為 web_app 跟 database，都是可以的。

## volumes & networks

他們的位階跟 `Services` 是一樣的:

```docker
services:
  - app
volumes:
  - postgres_data
networks:
 - net
```

在 services 中，我們給各個容器設定了 volumes 跟 networks，都是要在外面 (也就是如上範例所示的第一位階處) 來設定，詳細的設定方式，有興趣可以 google 一下。

在 `docker-compose.yml` 中，若 volume 或 networks 設有 `external` ，則代表是要由外部預先建立好的 volume 或 networks 來提供 Compose 的 services 使用，而該 volume 或 networks 就不受 `docker-compose` 所管理，故在使用 `docker-compose down` 刪除容器時，並不會刪除設有 `external` 的 volume 或 networks。

```docker
services:
  # ...
networks:
  network1:
    name: my-pre-existing-network
    external: true
```

# 總結

今天先把 Docker Compose 的基本概念與最上層的指令講接一次，接著明天，我們會繼續講解更細部的指令。