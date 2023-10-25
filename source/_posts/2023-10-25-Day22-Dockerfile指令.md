---
title: 2023 iT邦鐵人賽 Day 22 - Dockerfile指令
date: 2023-10-25 21:44:36
categories: 
- [iThome]
- [Docker]
- [Dockerfile]
tags: 
- iThome
- Dockerfile
---
# 前言

昨天我們介紹了 Dockerfile，知道 Dockerfile 是定義映像檔的 Docker 腳本，那我們該用什麼來定義呢？今天我們會介紹常見的 Dockerfile 指令。

<!-- more -->

# 指令

## FROM

用來載入映像檔，在建立一個新的映像檔時，必須以其他的映像檔為基礎。比方，在 Rails 專案中，想把整份 Rails 用 Dockerfile 打包成映像檔時，通常會載入 Ruby 映像檔。

```sql
From ruby:3.2.2-alphine
```

## ARG

在 Dockerfile 中設置變數

```docker
ARG APP_ROOT=/src/app
```

要使用 ARG 所設置的變數時，用 `${}` :

```docker
COPY . ${APP_ROOT}   --> Copy . /src/app
```

## RUN

執行指令，但必須為 FROM 映像檔中的程式所支援的指令。

```jsx
RUN apt-get update && apt-get install -y build-essential libpq-dev nodejs postgresql-client imagemagick
```

像這個就是執行後面那一串指令，而這串指令主要就是先更新 `apt-get` ，確保目前是最新的，然後再用 `apt-get` 安裝這個映像檔環境內，執行你的應用程式時，需要的所有應用程式。

## ENV

在 Dockerfile 中設置環境變數，並給予預設值。

```jsx
ENV RAILS_ENV production
```

我們可以在啟動容器時，透過 `-e` 改變環境變數的值。

```jsx
docker run -e RAILS_ENV="test" rails_images
```

## WORKDIR

在 Dockefile 中定義目錄，並會在映像檔的檔案系統中，建立目錄，並設為工作目錄。

```sql
WORKDIR /src/app
```

## COPY

透過 Dockerfile，在建立映像檔的過程中，複製指定的檔案到指定的映像檔檔案系統中的目錄裡。

```sql
COPY Gemfile /src/app/

在映像檔裡便會有 /src/app/gemfile 這個檔案
```

## --from

通常與 `COPY` 搭配，將指定階段的檔案複製到指定目錄或檔案上。

```ruby
FROM diamol/base AS build-stage    
RUN echo 'Building...' > /build.txt

FROM diamol/base AS test-stage
COPY --from=build-stage /build.txt /build.txt   
RUN echo 'Testing...' >> /build.txt

FROM diamol/base
COPY --from=test-stage /build.txt /build.txt    
CMD cat /build.txt
```

## EXPOSE

設定映像檔的連接阜。前一天在 Dockerfile 範例裡有 `EXPOSE 3000` ，所以在建立容器時，是 `-p 3000:3000` 而不是 `-p 3000:80` 。

## ENTERYPOINT

跟 `CMD` 相似，也是會在容器啟動，自動執行設定的指令。

```jsx
ENTRYPOINT ['rails']
CMD ['server']
```

啟動容器時，會執行 `rails server` 指令。

`ENTRYPOINT` 有分兩種格式： `shell form` 跟 `exec form` ，前者無法跟 `CMD` 合用，後者可以，而官方推薦使用 `exec form` 。

既然 `ENTRYPOINT` 跟 `CMD` 一樣可以自動執行指令，那幹嘛還需要有這兩個？

其實 `ENTRYPOINT` 還可以讓我們執行 `Dockerfile` 外的 script 檔案。

```jsx
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 6379
CMD ["redis-server"]
```

這時，相同目錄下會有一個 script 檔案叫 `[docker-entrypoint.sh](http://docker-entrypoint.sh)` ， 在上述例子，啟動容器時會先執行 `docker-entrypoint.sh` ，接著再執行 `redis-server` 。
有興趣可以參考 https://github.com/docker-library/redis/blob/master/docker-entrypoint.sh 看一下內容。

## HEALTHCHECK

設定容器運作時的健康檢查機制，用來確認容器內應用程式是否有正常運作。

### CMD

在 Docker 啟動容器時，會自動執行該命令。

```sql
CMD ["server"]

在容器啟動後會執行 server 命令以啟動 rails 伺服器
```

# 總結

今日介紹的指令都是在 Dockerfile 中常見的指令，從基本的建立、檢查到執行。礙於篇幅無法做太深入的解釋，如果對 `shell form` 跟 `exec form` 好奇，可以參考[https://medium.com/@xyz030206/dockerfile-中的-entrypoint-9653c3b2d2f8](https://medium.com/@xyz030206/dockerfile-%E4%B8%AD%E7%9A%84-entrypoint-9653c3b2d2f8) 獲得更詳細的解答。

如果對各個應用程式的 Dockerfile 有興趣的話，可以參考 https://github.com/docker-library，這裡提供了官方釋出的 Dockerfile。

接下來的幾天，我們會繼續介紹 Dockerfile 的一些篇進階知識，劇透一下，像是 Dockerfile 快取機制、檢查機制、在 Dockerfile 中建立多階段，就像在 CI/CD 的 pipeline 上有分階段，初步建立或設定、測試，最終建立或部署。