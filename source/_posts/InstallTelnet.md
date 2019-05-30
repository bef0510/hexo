---
title: Windows Server 2016 啟用 Telnet
date: 2019-05-30 10:06:50
 - Server
categories: 
 - MIS
---

# 啟用 Telnet
### Telnet Client 安裝
1. **Server Manager** → **Add roles and features**
![Architecture](1.png)

2. **Next**
![Architecture](2.png)

3. **Next**
![Architecture](3.png)

4. **Next**
![Architecture](4.png)

5. **Next**
![Architecture](5.png)

6. 勾選 **Telnet Client**
![Architecture](6.png)

7. 勾選 **Restart the destination server automatically if required**
![Architecture](7.png)

8. 完成
![Architecture](8.png)

### Telnet 執行
1. 開啟 **Command Prompt** 或 **PowerShell**
輸入 `telnet`
![Architecture](9.png)

2. 連線成功
![Architecture](10.png)

# Reference
[Install Telnet Client: 安裝 Telnet用戶端程式](http://sharedderrick.blogspot.com/2017/09/install-telnet-client-telnet.html)