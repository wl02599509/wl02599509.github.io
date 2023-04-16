---
title: AWS EC2 使用 AWS RDS
date: 2023-04-16 21:53:55
category: 
- AWS
- RDS
- database
tags:
- rails
- aws
- deploy
- database
---
在前一篇記錄了使用 AWS EC2 部署 Rails 專案的過程，在部署裡，除了專案本身外，另一個遇到難處理的就是資料庫。

因為 EC2 instance 環境使用 Amazon Linux 2023，而 package 裡的 pg 版本太舊，只有 9 開頭，導致有很多相容性問題，於是決定外接 RDS，沒想到非常方便，就讓我們來看看到底要如何設置 RDS 吧！
<!--more-->

# 建立 AWS RDS

1. 到 RDS 服務左側介面選擇「儀表板」或「資料庫」，右側選擇「建立資料庫」
2. 選擇資料庫建立方法：依需求選擇標準建立跟輕鬆建立。本篇選擇標準建立。
3. 引擎類型：選擇專案需要的資料庫類型，要注意這邊選擇的 RDS 資料庫類型要跟 Gemfile 裡裝的資料庫套件要一致。本篇選擇 postgreSQL。
4. 引擎版本：選擇資料庫的版本。
5. 範本：依照需求選擇符合的方案。本篇選擇免費方案。
6. 資料庫執行個體識別符：RDS 建立好後，顯示在服務介面上的實體名稱。
7. 主要使用者名稱：也就是資料庫使用者名稱，對應到 rails 的 database.yml 裡的 `username` 參數。
8. 主要密碼：資料庫使用者的密碼，對應到 rails  的 database.yml 裡的 `password` 參數。
9. 運算資源：選擇**連接至 EC2 運算資源**並選擇EC2 執行個體，將此 RDS 串接到需要的 EC2 instance。
10. 設定好後即可創立資料庫。

RDS 在創立後需要進行初始化一段時間，完成後即可使用。

# 設置 database.yml

因為部署環境為 production，所以將以下參數到 production 內：

```jsx
username: <%= ENV['RDS_USERNAME'] %>
password: <%= ENV['RDS_PASSWORD'] %>
host: <%= ENV['RDS_HOSTNAME'] %>
port: <%= ENV['RDS_PORT'] %>
```

這些參數的值為前一步驟創立 RDS 後所獲得的值，以下介紹各參數所對應的值為何：

1. username: 上一步驟所建立的**主要使用者名稱**
2. password: 上一步驟所建立的**主要密碼**
3. host: 在資料庫建立完成後，點擊資料庫的名稱，在連線與安全性內**，**端點與連接埠的**端點**  `rdsexample.foobar123.us-west-2.rds.amazonaws.com`
4. port: 在連線與安全性內**，**端點與連接埠的**連接阜** `5432`

# 設置環境變數

上述的參數與值，通常會使用環境變數，而非直接將攸關資安的資訊寫在上面讓大家知道。在開發階段，我想大家都知道可以建立一個 `.env` 檔，並在裡面建立各個環境變數以供開發，但 `.env` 並不會被上傳 github，所以在 ec2 instance pull 時，專案內是沒有環境變數的。

本篇解決方法就是在 ec2 instance 的系統裡建立環境變數：

1. 先建立並編輯 `/etc/environment`

```jsx
sudo nano /etc/environment
```

1. 將以下內容寫入

```jsx
  RDS_DB_NAME="db_name"
  RDS_USERNAME="mike"
  RDS_PASSWORD="foobar123"
  RDS_PORT=5432
  RDS_HOSTNAME="examplerds.foofoo.us-west-2.rds.amazonaws.com"
```

# 設置 private_ip 至 RDS 的傳入規則

以上步驟皆完成後，此時執行 `RAILS_ENV=production rails db:create` 若出現以下錯誤訊息，代表 EC2 instance 連線至 RDS 失敗，原因是 RDS 並不認得目前嘗試連線的 EC2 instance ip：

```jsx
PG::ConnectionBad: connection to server at "123.666.0.77", port 5432 failed: Connection timed out
	Is the server running on that host and accepting TCP/IP connections?
```

所以我們必須讓 RDS 認得 EC2 instance 的私有 ip，才能成功連線：

1. 到 EC2 instance 介面找到**私有 IPv4 地址**並複製
2. 到 RDS instance 找到**安全性**，並點擊**VPC 安全群組**
3. 進入安全群組後，選擇**傳入規則**，並點擊**編輯傳入規則**
4. 按下**新增規則，類型**選擇資料庫的類型，**來源**選擇自訂，並將步驟 1 所複製的 ip 貼上，按下儲存規則。

如此一來 RDS 就能讓 EC2 instance 連線進來，再次執行  `RAILS_ENV=production rails db:create` 後，就沒問題了。