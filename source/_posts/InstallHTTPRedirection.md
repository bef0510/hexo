---
title: Windows Server 2016 HTTP 重新導向
date: 2019-05-30 15:42:20
tags: 
 - Server
 - IIS
categories: 
 - MIS
---

# 啟用 HTTP Redirection
### HTTP Redirection 安裝
1. **Server Manager** → **Add roles and features**
![Architecture](1.png)

2. **Next**
![Architecture](2.png)

3. **Next**
![Architecture](3.png)

4. **Next**
![Architecture](4.png)

5. 勾選 **HTTP Redirection**
![Architecture](5.png)

6. **Next**
![Architecture](6.png)

7. 勾選 **Restart the destination server automatically if required**
![Architecture](7.png)

8. 完成
![Architecture](8.png)

### IIS 設定
1. **Server Manager** → **Tools** → 點擊 **Internet Information Services (IIS) Manager**
![Architecture](9.png)

2. 選取 **Sites** → **IIS** → **HTTP Redirect**
![Architecture](10.png)

3. 勾選 **Redirect requests to this destination**
   輸入網址
   勾選 **Redirect all requests to exact destination (instead of relative to destination)**
   選取 **Status code**
![Architecture](11.png)

### Web.config 設定
![Architecture](12.png)

# Reference
[IIS 7.5, 8 - 設定 HTTP 重新導向](http://codeplanet.me/archives/2014/10/http-redirection-on-iis/)