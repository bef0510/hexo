---
title: SessionState 模式
date: 2019-04-30 10:56:11
tags:
 - Config
 - Session
categories: 
 - ASP.Net
---
# SessionState 模式
1. **Off**
2. **InProc** (預設模式)
3. **StateServer**
4. **SqlServer**
5. **Custom for Redis**

**StateServer**、**SqlServer**、**Redis** 除了 **.Net** 預設型別，皆需在 `class` 加上 `Serializable` 關鍵字
~~~ bash
c# [Serializable]
VB <Serializable()>
~~~
![Serializable](1.png)

### Off
關閉 **Session**
~~~ bash
<system.web>
    <sessionState mode="off" />
</sytem.web>
~~~

### InProc (預設模式)
**Session** 存在 **IIS** 中，預設 **time out** `20` 分鐘
~~~ bash
<system.web>
    <sessionState mode="InProc" timeout="30" />
</system.web>
~~~
優點：獲取 **Session** 狀態的速度快
缺點：易丟失

### StateServer
**Session** 儲存在當前應用程式域之外的 `asp.net_state.exe` 服務，獨立於 **IIS** 之外的 **Windows** 服務
~~~ bash
<system.web>
    <sessionState mode="StateServer"
        stateConnectionString="tcpip=localhost:42424"
        cookieless="false"
        timeout="20"/>
</sytem.web>
~~~
優點：**Session** 單獨存在一個程序中，不會因 **IIS** 重啟而丟失。
缺點：因是兩個不同程序，獲取速度比 `InProc` 慢
&emsp;&emsp;&emsp;過多的序列化和反序列化

### SqlServer
**Session** 序列化後，儲存到 **SQL Server** 資料庫中，可使用在 **NLB**
~~~ bash
<system.web>
    <sessionState mode="SQLServer" 
        allowCustomSqlDatabase="true" 
        sqlConnectionString="Server=.;Database=ASPState;Integrated Security=true" 
        sqlCommandTimeout="60" 
        cookieless="false" 
        cookieName="ASP.NET_SessionId" 
        timeout="60" />
</system.web>
~~~
優點：較高的擴展性及可靠性
缺點：影響 **DB** 效能
&emsp;&emsp;&emsp;獲取速度比 `StateServer` 慢
&emsp;&emsp;&emsp;過多的序列化和反序列化

### Custom for Redis
**Session** 序列化後，儲存到 **Redis** 是內存，**Redis** 是內存 `key-value` 數據存儲最快。功能最豐富的一種
~~~ bash
<system.web>
    <sessionState mode="Custom" customProvider="MySessionStateStore">
        <providers>
            <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="" accessKey="" ssl="false" />
        </providers>
    </sessionState>
</system.web>
~~~
優點：較高的擴展性及可靠性
&emsp;&emsp;&emsp;可做為 **memory server**
缺點：沒有本地數據緩衝(像 **Azure** 緩存上同步的本地數據緩衝)
&emsp;&emsp;&emsp;伺服器數量需為單數台
&emsp;&emsp;&emsp;過多的序列化和反序列化

# Reference
[ASP.NET MVC中的Session以及處理方式](https://www.itread01.com/p/603636.html)
[ASP.NET程式中Session儲存的幾種模式](https://www.itread01.com/content/1546942022.html)
[ASP.NET 多台 Web 伺服陣列透過 SQL Server 保存共同工作階段狀態](https://dotblogs.com.tw/wasichris/2017/03/14/184803)
[C# SESSION丟失問題的解決辦法](https://www.itread01.com/article/1491448567.html)