---
title: SQL Server - 查看執行計畫
date: 2019-06-14 14:20:36
tags:
 - Performance
categories: 
 - MS SQL
---

# 查看執行計畫

1. 查看方式
從右到左(平行處理)
![Architecture](1.png)

2. Index 是否正確
一般來說 `Seek` 比 `Scan` 快，若撈出的資料量很大，資料庫覺得掃描比搜尋更有效率，會自行用掃描
![Architecture](8.png)

3. 減少資料量
資料量越大，箭頭會越粗，盡可能篩選出所需資料
![Architecture](2.png)

4. 耗費成本
針對成本比例高的地方進行調效
![Architecture](3.png)

5. 避免型態轉換
型別不同 `Join` 條件或 `Where` 條件
![Architecture](4.png)
![Architecture](5.png)

6. 警告訊息
![Architecture](6.png)

7. 避免排序
![Architecture](7.png)

# Reference
[SSMS--執行計畫](https://blog.kkbruce.net/2011/01/ssms.html#.XQNsE4gzaUk)
[SQL Server 學習筆記](http://sharedderrick.blogspot.com/2017/12/sql-server-look-at-execution-plan-and.html)
[效能調校(2)-分析執行計畫](http://vito-note.blogspot.com/2013/05/blog-post_2862.html)
[[SQL]MSSQL隱含轉換造成的效能影響](https://dotblogs.com.tw/eganblog/2019/05/07/sql_datatypeconversion)