---
title: MS SQL DB 結構
date: 2019-07-11 14:05:35
tags:
 - Structrue
categories:
 - MS SQL
---

# DB 結構
1. 檔案
![Architecture](1.png)
• 每個資料檔皆可設定檔案存放的位置，用以存放在不同的磁碟內，可改善效能，且減少Lock

2. 檔案群組
![Architecture](2.png)
• 建立TABLE -->指定檔案群組 1
  * SQL Server會依照各檔案大小，依比例將資料表的資料平均寫入各資料檔中
  * 此TABLE的資料會分散在不同磁碟(假設次要資料檔分別位於不同磁碟)，提高效能