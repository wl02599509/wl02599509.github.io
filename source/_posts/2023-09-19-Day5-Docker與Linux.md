---
title: 2023 iT邦鐵人賽 Day4 - Docker 與 Linux
date: 2023-09-19 00:12:49
categories: 
- [iThome]
- [Docker]
tags: 
- iThome
- Docker
- 容器
---
# 前言

在前一篇提到，空的容器是由 Linux OS 的映像檔所建立而成的容器，會稱之為空的原因是因為它本身並未提供任何服務，只是一個單純的作業系統環境。

今天我們要講的重點是，整個 Docker 運行的基礎作業系統就是 Linux，好的，一言以蔽之，今天就到這裡了（誤），但這確實是本日重點（汗）。

<!-- more -->
# Docker 運行基礎

最初，Docker 是專為 Linux 設計，所以 Docker 本身是以 Linux 為基礎來運作，所以其映像檔也幾乎是以 Linux 為基礎建立而成的。

# Linux 快又有效

雖然比較作業系統的快慢是沒意義的，因為不同的作業系統，有各自適合自己的使用情境，但 Linux OS 的特性：高效與輕量，讓它在 Docker 的使用上，確實會比較有效率。

# Docker 裡滿滿的 Linux

透過上述可知，我們在使用 Docker 時，無論是在設計映像檔、建立容器、或是其他操作，其實都跟 Linux OS 脫不了關係。

# 總結

這樣聽下來，是不是只有在 Linux OS 的系統才能使用 Docker ?

答案是否定的，我們可以在 Windows 或 Mac 等非 Linux OS 的系統上，自由自在地使用 Docker，至於為什麼，就讓我們明天揭曉~