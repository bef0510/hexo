---
title: ASP.NET 無法啟動 IIS Express Web 伺服器
date: 2019-06-21 15:09:51
tags:
 - Visual Studio
categories: 
 - ASP.Net
---

# 無法啟動 IIS Express Web 伺服器

1. **Build VS Error**
![Architecture](1.png)

2. 刪除資料夾隱藏檔 `.vs`
![Architecture](2.png)

3. 專案 右鍵 → 屬性
![Architecture](3.png)

4. **Web** → 修改 專案 **URL(J)** [Port號](https://zh.wikipedia.org/wiki/TCP/UDP%E7%AB%AF%E5%8F%A3%E5%88%97%E8%A1%A8)
![Architecture](4.png)

# Reference
[Visual Studio 2012 無法啟動IIS Express Web 伺服器](http://marco.easyusing.com/2012/12/visual-studio-2012-iis-express-web.html)