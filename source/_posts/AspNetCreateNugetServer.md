---
title: ASP.NET 建置 NuGet Server
date: 2019-06-20 14:01:56
tags:
 - NuGet
 - Visual Studio
categories: 
 - ASP.Net
---

# 建置 NuGet Server

1. 建立一個 **Web** 專案
![Architecture](1.png)

2. 選擇 **Empty**
![Architecture](2.png)

3. 安裝 **NuGet.Server**
![Architecture](3.png)

4. 成功後 `Web.config` 會自動載入所需 **XML**
![Architecture](4.png)

5. 輸入 **apiKey value**，建議使用 **GUID**
![Architecture](5.png)

6. 建置 **IIS**，開啟網頁
![Architecture](6.png)

7. 點選 **here**，可正常顯示頁面
![Architecture](7.png)

8. **Visual Studio** → 工具 → **NuGet** 封裝管理員 → 套件管理員設定
![Architecture](8.png)

9. 套件來源 → 新增 → 輸入資訊 → 更新
![Architecture](9.png)

# Reference
[Hosting a Simple Read-Only NuGet Package Feed on the Web](https://haacked.com/archive/2011/03/31/hosting-simple-nuget-package-feed.aspx/)