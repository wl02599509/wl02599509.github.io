---
title: 2023 iT邦鐵人賽 Day 25 - Dockerfile 容器應用程式可靠性
date: 2023-10-25 21:48:27
categories: 
- [iThome]
- [Docker]
- [Dockerfile]
tags: 
- iThome
- Dockerfile
---
# 前言

當我們容器在運行時，理想是狀況是希望容器內的應用程式能夠一直安然無恙、平平安安、順順利利、健健康康的一直運行著。但偏偏事與願違，總是有那麼一些狀況會發生，那我們要怎麼樣主動確認應用程式還有在正常提供服務呢？

<!-- more -->

# 狀態檢查 `health check`

當我們在建立容器時，我們需要確認容器內的應用程式是否仍處於正常狀態，這時我們可以透過狀態檢查，來持續地幫我們確認，容器內應用程式是否正常。

假設目前容器為 web 應用程式，實作上，我們將 `HEALTHCHECK` 加入到 Dockerfile 中，當容器建立並啟動之後，定期 ( 該指令的預設是每 30 秒進行一次的檢查) 向 `http://localhost/health` 發送 request，並確認 response 的 status 是否為 200，來確認容器內應用程式是否有正常運作。

```docker
HEALTHCHECK CMD curl --fail http://localhost/health
```

如果今天狀態回傳 500 等錯誤或不正常狀態，則會使容器的狀態從 `healthy` 變成 `unhealthy` 。

```docker
CONTAINER ID   IMAGE                       COMMAND                   CREATED        STATUS                      PORTS                  NAMES
a8811cdff483   diamol/ch08-number-ail:v2   "dotnet /app/Numbers…"   2 hours ago    Up 2 hours (unhealthy)      0.0.0.0:8010->80/tcp   lucid_cohen
```

我們可以發現，雖然他狀態顯示 `unhealthy` ，但仍然運作中，而不會自行退出，因為 Docker 不具有相關的安全性操作，若希望容器在狀態改變時，能夠替換一個新的容器，則須透過像 `docker swarm` 或 `K8S` 這類的容器管理平台。

此外，透透過 `docker container logs a8811cdff483` 也可以看到 `unhealthy` 的警告：

```docker
warn: Numbers.Api.Controllers.RngController[0]
      Unhealthy!
```

# 相依性檢查

有別於狀態檢查，相依性檢查主要是用來確認容器之間，啟動的順序是否如預期。白話文就是，我的 A 容器要在 B 容器啟動後，才能接續啟動，不然我的 A 容器會無法跟 B 容器連上，導致 A B 容器失聯。

之後在介紹 `docker-compose` 會有個 `depends_on` 的指令，也是用來定義容器間的相依性，也就是在該容器啟動前，必須先運行哪些容器，如果沒有先運行那些容器，該容器即使運作，其應用程式也會產生錯誤。

但我們在 `docker-compose` 裡面用 `depends_on` 就好啦，幹嘛要再 `Dockerfile` 搞這個？因為在跨叢集執行時，會讓 `depends_on` 無法控制容器的啟動順序。

所以透過相依性檢查，當有多項容器運作時，若需依賴其他容器的容器損毀並需要換一個新的容器時，事先確認它的相依容器是否都有啟動，是一件非常重要的事情。

但相依性檢查不像狀態檢查，有專門的指令 `HEALTHCHECK` 可以使用，必須要去依照情境去寫相關的檢察方式。

這裡我們用《Docker 隊長》一書裡的範例來說明：

```docker
FROM diamol/dotnet-sdk AS builder

WORKDIR /src
COPY src/Numbers.Web/Numbers.Web.csproj .
RUN dotnet restore

COPY src/Numbers.Web/ .
RUN dotnet publish -c Release -o /out Numbers.Web.csproj

# app image
FROM diamol/dotnet-aspnet

ENV RngApi:Url=http://numbers-api/rng

CMD curl --fail http://numbers-api/rng && \
    dotnet Numbers.Web.dll

WORKDIR /app
COPY --from=builder /out/ .
```

這是一個 web app，在這裡，&& 的意思是，左邊執行成功後，才會去執行右邊。在上述例子，是透過卻日 http://numbers-api/rng 有無執行成功，有成功就會透過 dotnet 來驗證應用程式是否仍然在執行。

如果沒有在執行，那就會讓容器處於退出狀態。

# 總結

我們終於講完了 Dockerfile。現在我們了解 Dockerfile 可以製作映像檔，讓我們能夠產生專案的映像檔，而在產生的過程中，要盡量利用 Docker 的快取機制，讓 image build 的時程大幅降低。

但我們有提過，一個完整的服務，通常會透過多個容器互相連接溝通而形成。如果我們想要讓 Rails 提供服務，光是只有 Rails 本身的容器是不夠的，資料庫是基本的，所以我需要在建立一個資料庫的容器，然後讓兩個容器能夠對到同個網絡 `network` ，而且必須要先啟動資料庫容器後才能啟動 Rails 容器，種種細節都要注意，如果一行一行指令慢慢輸入，實在是太麻煩了！更何況，若是更龐大的系統，要用到好幾個容器，那更是沒完沒了。

好在有個東西叫 `Docker Compose` ，透過一個檔案，讓我們完成所有容器之間的啟動與連結，省去那些繁雜的指令。明天我們來介紹這個非常重要的工具吧!

鐵人賽即將接近尾聲，我們也即將要進入最後要介紹的容器管理了。這是一趟長途漫漫的 Docker 旅程，就讓我們用簡單的方式來介紹，讓大家對容器管理有個初步的概念即可。