---
title: GitHub, Git 分清楚了嗎？（上）
date: 2022-08-15 17:08:14
categories: 
- GitHub
tags: 
- GitHub
- Git
---

# 前言

GitHub 是業界中廣為人知的版本控制網站，它是使用 Rails 建構（一定要提一下ＸＤ）。

只要輕輕地使用 git 指令，便能將檔案推上網站，或是從網站上拉下來。

等等，一下 GitHub 一下 git，有什麼差嗎，為什麼會用兩個不同的詞？

就讓我們繼續看下去。

<!-- more -->
---

# GitHub vs Git

如前言所說，GitHub 是版本控制網站，是一個網站，但 git 是一套工具，兩者大相徑庭，但又不能說完全沾不上邊，不然怎麼會都有個 git 字呢？

git 這套工具有很多簡單好用的指令，讓我們能妥善管理檔案 <strong>(git add / git commit)</strong>，而 GitHub 這個版本控制網站，則是要透過 git 指令，將受管理的檔案們，上傳 <strong>(git push)</strong> 到 GitHub 上面的 <strong>Repositories</strong>。

這樣看來，兩者便是彼此獨立的個體，但有著密不可分卻的關係。

# GitHub

經由上述說明，感覺 GitHub 只是一個放置檔案的平台而爾，但其實網站本身的功能非常多且強大。

## count contributions

只要我們將檔案推上（git push）GitHub，個人首頁下的小小灰色格子就會變成綠色。

該上傳檔案什麼時候被 git commit，那麼，什麼時候的灰色格子變成綠色，所以你在 2022.12.31 23:47 對Ａ檔案 commit，然後在 2023.01.01 00:01 推到 GitHub 上，那變成綠色的格子就會是 2022.12.31 唷，<strong>不會跟你一起跨年唷～</strong>

至於綠色的深淺，會依照 commit 的數量決定。

## Fork

這個功能可以讓我們把其他用戶的 repository 分享到自己的 Repositories 裡。

## Sync fork

當我們把別人的 repository 抓過來後，對方在自己的 GitHub 上更新了該 repository，我們可以在自己 GitHub 的該 repository 的 <> Code 頁面，按下 Sync fork，讓自己這邊能同步更新一次對方檔案的進度。

## Pull requests

當我們把對方的repository Fork 過來並進行修改時，可以使用 Pull requests，像對方發出請求，請求對方將自己修改的部分，納入對方的 repository。

這個功能常常在團隊開發時會使用到，而且可以設定要有多少人 approve 更動的部分後，才能 pull。

另外，如果多人對同個檔案做了不同的修改，而發送 Pull requests 時，第二個以後的 request，都有可能會因為內容衝突導致無法 pull。

舉例來說:

如果 A, B, C 都對 Wubay 這個 repository 的 index.html 進行修改並送出 Pull requests，C 的 requests 先被接受，其後的 A 跟 B 就有可能會因為內容有衝突導致無法 pull 的狀況。

如此，就要退回 A 跟 B 的 requests，並請他們重寫（可憐）。

## Self-Profile

只要新增一個名稱與自己帳號一樣的 repository，並勾選 <strong>Add a README file</strong>，就能在自己的 Overview 上產生一個大欄位，讓你可以在上面打上一些自我介紹等資訊。

這部分的編輯，因為是 Markdown 文檔，所以可以了解一下 Markdown 的 語法。

---

# 小結

本篇介紹了 GitHub 的功能，我想大家應該多少對 GitHub 有點認識了，下一篇會介紹 Git，兩篇讀完後，應該更能理解雙方的差異。