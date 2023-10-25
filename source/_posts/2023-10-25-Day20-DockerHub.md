---
title: 2023 iT邦鐵人賽 Day 20 - DockerHub
date: 2023-10-25 21:42:22
categories: 
- [iThome]
- [Docker]
tags: 
- iThome
- Docker
---
# 前言

文章越來越接近末班車了，在迎接 2 字頭之時，我們先來輕鬆一下。時至今日，我們已經操作過好多次的 docker 指令，容器、映像檔、volume 等，對指令有一定的熟悉度，其中常常需要把映像檔抓下來，那到底是從哪裡抓下來的呢？之前有稍微提到 Docker Hub 這個東西，今天就讓我們來詳細介紹。

<!-- more -->

# 映像檔雲端倉庫

Docker Hub 是存放映像檔的雲端，由 Docker 官方自家營運。裡面富含了各式各樣應用程式官方所提供的映像檔。

除了 Docker Hub，還有其他像是 Google Container Registry、Amazon Elastic Container Registry 等，但 Docker Engine 預設登入的平台為 Docker Hub，所以如果沒有特別設定，都會從 Docker Hub 去找與抓映像檔。

# Image Reference

在前面有介紹到映像檔的除了映像檔名稱，還會有標籤: `httpd:latest` 
但其實這不是映像檔完整的名稱，正確的完整名稱是: `預設登入伺服器/團隊或個人帳戶/儲存庫名稱:版本`

```
docker.io/library/httpd:latest
```

雖說它有完整的名稱，但其實我們只需要 `httpd:latest` 這樣就足以讓 Docker Engine 知道你需要什麼了～

# 上傳自己的映像檔

我們除了下載映像檔，當然也可以上傳映像檔。

若要推送到別的團隊的 repository 上，帳戶必須要加入該團隊才行。

這裡只討論上傳到自己的 repository。

## 先在終端機上登入

```
docker login --username docker帳號
接著會出現輸入密碼如下
Password:
Login Succeeded
```

## 上傳映像檔的規則

- 若要將檔案上傳，須在映像檔名稱前加上自己的帳戶名稱: `帳戶名稱/映像檔名稱:版本標籤`

```jsx
foobar/httpd:iThome
```

- 我們也可以使用既存的映像檔重新給予命名( image reference )，不必重新 build 一個映像檔:

```
> docker image ls

REPOSITORY   TAG        IMAGE ID       CREATED     SIZE
i-am-image  latest    c24bb2327275   12 days ago   235MB

> docker image tag i-am-image foobar/i-am-image:v1

> docker image ls

REPOSITORY             TAG        IMAGE ID        CREATED       SIZE
i-am-image            latest      c24bb2327275   12 days ago   235MB
foobar/i-am-image   v1        c24bb2327275   12 days ago   235MB

```

- 經過 image reference 後，新的 image 其實還是原本的 image，所以 `i-am-image` 跟 `foobar/i-am-image` 都是一樣的。
- 找出相同映像檔的所有 image reference:

```
docker image ls --filter reference=i-am-image --filter reference='*/i-am-image'
```

- 上傳映像檔時，僅是上傳映像層而已：

```
docker image push wl02599509/image-of-day:v1   
The push refers to repository [[docker.io/wl02599509/image-of-day](http://docker.io/wl02599509/image-of-day)]
3bd6078b808b: Pushed
844740189b53: Pushed
568db9d7ccb7: Mounted from diamol/openjdk
590cf45d87e8: Mounted from diamol/openjdk
83a0ef36818a: Mounted from diamol/openjdk
6c748605b345: Mounted from diamol/openjdk
ff79d3e22ab0: Mounted from diamol/openjdk
v1: digest: sha256:fbf0b99708f449e3c5edd8793a15c51651f7773071d6e2787a49317aaf31ddcc size: 1789
```

- 映像檔上傳也會有快取機制，沒有的映像層才會被推上去，故考慮優化，提升上傳速度

# 總結

今天介紹了可能比較少會用到的上傳映像檔 ( 也有可能是我太菜不太用到!? )，不過我們可以從上傳發現，連上傳也都是映像層的流動，如此更加深了映像檔與映像層的關係和概念。

明天開始即將進入 Dockerfile。Dockerfile 就是定義映像檔的檔案，我們透過 Dockerfile 來建立一個映像檔。敬請期待。