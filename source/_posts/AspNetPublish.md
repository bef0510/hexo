---
title: ASP.NET 發行網站
date: 2019-06-19 14:29:49
tags:
 - IIS
 - Visual Studio
categories: 
 - ASP.Net
---

# ASP.NET 發行網站
1. 先建好 **IIS**
[Windows Server 2016 啟用 IIS](/2019/05/17/InstallIIS/)
2. 專案右鍵 → 發行
![Architecture](1.png)

3. 選擇自訂
![Architecture](2.png)

4. 輸入 設定檔名稱
![Architecture](3.png)

5. 輸入資訊、點選 **Validate Connection**
![Architecture](4.png)

6. 選擇 **Release**
![Architecture](5.png)

7. 點選 發行
![Architecture](6.png)

8. **Visual Studio** 輸出框可看到發行資訊
![Architecture](7.png)

# Reference
[使用 Visual Studio 的 ASP.NET Web 部署：部署到測試環境](https://docs.microsoft.com/zh-tw/aspnet/web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis)