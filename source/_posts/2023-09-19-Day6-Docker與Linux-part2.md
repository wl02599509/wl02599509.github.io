---
title: 2023 iT邦鐵人賽 Day6 - Docker 與 Linux part 2
date: 2023-09-19 00:14:05
categories: 
- [iThome]
- [Docker]
tags: 
- iThome
- Docker
- 容器
---
# 前言

在前兩篇，我們可以了解，Docker 是一個以 Linux 為基礎的技術，Docker 的所有運行都跟 Linux OS 有關，就連容器裡的環境，也是 Linux OS。

那使用 Docker 的系統，如 Windows、Mac OS，是怎麼有辦法使用 Docker 呢？

那又為什麼 Docker 容器裡的 Linux OS 會是只有部分，而不是全部？

<!--more-->
# 人人都能使用 Docker

我們在使用 Docker 之前，都會先依照主機的作業系統去安裝符合的 Docker 應用程式( Docker Desktop )。當我們安裝了 Docker Desktop 時，會**在我們的作業系統上，再安裝 Linux 作業系統**，然後我們的 Docker 就會在這個被安裝進來的 Linux OS 運行。

所以無論你是 Windows 或 MacOS，都可以順暢的使用 Docker，因為它幫你處理了它需要而你沒有的東西。

當主機有了 Linux OS，Docker 容器與主機之間，就能共享 Linux OS 的核心資源。

# 容器內只有部分 Linux OS，好讓容器輕量有效率

在前幾篇提到，容器內部的作業系統是最小化的 Linux OS，也就是只含有「殼」的部分，並不包含「核心」的部分。

在前述提到，容器可以與主機共享 Linux OS 的核心資源。

因為容器可以與主機共享 Linux OS 的核心資源，所以容器本身就不需要有「核心」的部分，**所有的處理都交給底層，而非容器環境內**，因此讓容器能達到輕量與快速。

# 總結

這幾天都圍繞在 Docker 與 Linux 之間的關係，今天我們來做個整理。

Docker 是以 Linux OS 為運作基礎的工具，**非 Linux OS 不可**，其他 OS 使用 Docker 時，就必須要有 Linux OS，也因為容器內的環境，只有 Linux OS 的殼，仍需要有核心才能夠讓 Docker 有完整的 Linux OS 使之正常運行。

容器裡的 Linux OS 只有殼、沒有核心的原因是要讓容器輕量化與效率化，好讓 Docker 能**佔據最少的電腦資源**。
