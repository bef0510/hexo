---
title: Session 使用 Redis
date: 2019-04-29 15:12:56
tags:
 - Config
 - Session
 - Redis
categories: 
 - ASP.Net
---

# 安裝 Redis
### 下載
[Redis](https://github.com/MicrosoftArchive/redis/releases/download/win-3.0.504/Redis-x64-3.0.504.msi)
[Redis Desktop Manager](https://sourceforge.net/projects/redis-desktop-manager.mirror/files/0.8.8/redis-desktop-manager-0.8.8.384.exe/download)

### 安裝
預設使用 `6379` **port**，勾選加入防火牆例外清單
![DefaultPort](1.png)

### 設定使用記憶體上限
依實際最大值設定，而不是平均值，官方建議要設，防止 **out of max memory**
![MaxMemory](2.png)

# 安裝套件
### NuGet GUI
使用 **NuGet** 下載 `Microsoft.Web.RedisSessionStateProvider`
**Web.Config** 自動產生下列資訊

      <sessionState mode="Custom" customProvider="MySessionStateStore">
      <providers>
        <!-- For more details check https://github.com/Azure/aspnet-redis-providers/wiki -->
        <!-- Either use 'connectionString' OR 'settingsClassName' and 'settingsMethodName' OR use 'host','port','accessKey','ssl','connectionTimeoutInMilliseconds' and 'operationTimeoutInMilliseconds'. -->
        <!-- 'throwOnError','retryTimeoutInMilliseconds','databaseId' and 'applicationName' can be used with both options. -->
        <!--
          <add name="MySessionStateStore" 
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            throwOnError = "true" [true|false]
            retryTimeoutInMilliseconds = "5000" [number]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "1000" [number]
            connectionString = "<Valid StackExchange.Redis connection string>" [String]
            settingsClassName = "<Assembly qualified class name that contains settings method specified below. Which basically return 'connectionString' value>" [String]
            settingsMethodName = "<Settings method should be defined in settingsClass. It should be public, static, does not take any parameters and should have a return type of 'String', which is basically 'connectionString' value.>" [String]
            loggingClassName = "<Assembly qualified class name that contains logging method specified below>" [String]
            loggingMethodName = "<Logging method should be defined in loggingClass. It should be public, static, does not take any parameters and should have a return type of System.IO.TextWriter.>" [String]
            redisSerializerType = "<Assembly qualified class name that implements Microsoft.Web.Redis.ISerializer>" [String]
          />
        -->
        <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="" accessKey="" ssl="true" />
      </providers>
    </sessionState>

### 啟用 redis-server
點擊 `C:\Program Files\Redis\redis-server.exe` 啟用
![RedisServer](3.png)

出現 **No connection is available** 錯誤
![ConnectionError](4.png)
修改 **ssl="<font color="red">false</font>"**
~~~ bash
<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="" accessKey="" ssl="false" />
~~~

### Command 驗證 Session
1. 加入 **Session**
![AddSession](5.png)

2. 開啟 **redis-cli.exe**
![OpenRedis-cli](6.png)

3. 查看所有 **key**
輸入 **keys ***
![ListKeys](7.png)

4. 查看特定 **key** 內容
輸入 **hgetall key**
![SpecificKey](8.png)

### Redis Desktop Manager 驗證 Session
1. 開啟 **Redis Desktop Manager 工具**
![InputInformation](9.png)

2. 查看 **Session** 資訊
![OpenInformation](10.png)

# Reference
[適用於 Azure Cache for Redis 的 ASP.NET 工作階段狀態提供者](https://docs.microsoft.com/zh-tw/azure/azure-cache-for-redis/cache-aspnet-session-state-provider)
[使用 Redis 當做 ASP.NET MVC 的 Session State Server](https://blog.yowko.com/redis-aspnet-mvc-session-state-server/)
[Windows 安裝 Redis](https://dotblogs.com.tw/zackmyself/2017/04/27/005621)