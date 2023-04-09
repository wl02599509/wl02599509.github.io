---
title: EC2 instance 當 bundle install 遇到 killed
date: 2023-04-10 00:18:28
category: AWS
tags:
- rails
- aws
- deploy
---
接續前一篇，在 rails_project clone 到 EC2 instance 並 cd rails_project 執行 bundle install 時，run 到一半卡住，最後顯示 **killed**，強制結束的意思。
會有 killed 的主要原因是執行 bundle install 安裝特定的 gem 所佔用的記憶體空間過大，導致環境內的記憶體空間不足，而被迫結束 bundle install。

解決方法為：
<!--more-->
# 擴充設備或環境的記憶體空間。
  這個方法顧名思義，應該不需多做討論，且這方法不是本篇主要探討的。

# 增加 swapfile ：
    1. 建立一個空檔案 **`swapfile`**，並設定為 **`root`** 用戶擁有：
    
    ```jsx
    sudo fallocate -l 2G /swapfile
    sudo chmod 600 /swapfile
    ```
    
    b. 格式化 **`swapfile`**，並啟用：
    
    ```jsx
    sudo mkswap /swapfile
    sudo swapon /swapfile
    ```
    
    c. 將 **`swapfile`** 設定為開機自動啟用，打開 **`/etc/fstab`**，加入以下行：
    
    ```jsx
    /swapfile swap swap defaults 0 0
    ```
    
    ps. 若 etc 內沒有 fstab file：
    
    ```jsx
    sudo nano /etc/fstab
    ```
    
    在文件的末尾添加 **`/swapfile swap swap defaults 0 0`**
    
    d. 啟用新文件：
    
    ```jsx
    sudo swapon -a
    ```
    
    e. 確認 swapon 狀態，查看是否啟用正常：
    
    ```jsx
     swapon -s
    ```
    
    完成上述執行過程後，便能再次執行 bundle install，問題就能解決了。