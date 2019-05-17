---
title: Windows Server 2016 啟用 IIS
date: 2019-05-17 11:09:12
tags: 
 - Server
 - IIS
categories: 
 - MIS
---

# 啟用 IIS
1. 開啟 `Server Manager` → 點選 `Add roles and features`
![Architecture](1.png)

2. 下一步
![Architecture](2.png)

3. 選擇 `Role-base or feature-based installation`
![Architecture](3.png)

4. 點選 `Server Pool`
![Architecture](4.png)

5. 點選 `Add Features`
![Architecture](5.png)

6. 勾選 `Web Server (IIS)`
![Architecture](6.png)

7. 勾選 `.NET Framework 3.5`
   勾選 `ASP.NET 4.6`
![Architecture](7.png)

8. 下一步
![Architecture](8.png)

9. 勾選 `.NET Extensibility 3.5`
   勾選 `.NET Extensibility 4.6`
   勾選 `Application Initialization`
   勾選 `ASP`
   勾選 `ASP.NET 3.5`
   勾選 `ASP.NET 4.6`
![Architecture](9.png)

10. 勾選 `Restart the destination server automatically if required`
    點選 `Specify an alternate source path`
![Architecture](10.png)

11. 輸入 `SxS` 路徑
![Architecture](11.png)

12. 點選 `Install`
![Architecture](12.png)

13. Installing
![Architecture](13.png)

14. 完成
![Architecture](14.png)

# Reference
[How To Install IIS In Windows Server 2016](https://www.rootusers.com/how-to-install-iis-in-windows-server-2016/)