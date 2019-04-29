---
title: Session 使用 SqlServer
date: 2019-04-29 12:05:44
tags: 
 - Config
categories: 
 - ASP.Net
---

# 建置 Database
### 使用aspnet_regsql.ext 工具建立

複製路徑 `C:\Windows\Microsoft.NET\Framework64\v4.0.30319`
命令提示字元輸入
~~~ bash
$ cd C:\Windows\Microsoft.NET\Framework64\v4.0.30319
$ aspnet_regsql.exe -S <serverIP> -U <userName> -P <strongPassword> -ssadd -sstype p
~~~
![SQLConnectionOptions](1.png)
![SessionStateOptions](2.png)

可看到資料庫已建置完成
![CreateTable](3.png)

# 修改 Web.Config
**Web.Config** 裡的 `system.web` 區塊 `seesionState` 修改

    <sessionState mode="SQLServer" allowCustomSqlDatabase="true" sqlConnectionString="data source=<serverIP>;initial catalog=ASPState;user id=<userName>;password=<strongPassword>"  sqlCommandTimeout="60" cookieless="false" cookieName="ASP.NET_SessionId" timeout="60"/>

![SelectTable](4.png)

# Reference
[ASP.NET SQL Server Registration Tool](https://docs.microsoft.com/en-us/previous-versions/ms229862(v=vs.100))
[SQLServer asp.net session state mode management Part 66](https://www.youtube.com/watch?v=o-8vjQu6SmI)
[如何設定與啟用 ASP.NET 的 SQLServer 工作階段狀態模式](https://blog.miniasp.com/post/2011/09/13/Configure-SQL-Server-Session-State-Modes-for-ASPNET)
[相同的IIS，讓多個網站共用Session資訊](http://kyleap.blogspot.com/2013/12/aspnetiissession.html)