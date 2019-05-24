---
title: Windows Server 2016 啟用 AD 及 DNS
date: 2019-05-24 11:56:08
tags: 
 - Server
categories: 
 - MIS
---

# 啟用 AD 及 DNS
### AD 及 DNS 安裝
1. **Server Manager** → **Add roles and features**
![Architecture](1.png)

2. **Next**
![Architecture](2.png)

3. **Next**
![Architecture](3.png)

4. **Next**
![Architecture](4.png)

5. 勾選 **Active Directory Domain Services**
   勾選 **DNS Server**
![Architecture](5.png)

6. **Next**
![Architecture](6.png)

7. **Next**
![Architecture](7.png)

8. **Next**
![Architecture](8.png)

9. 勾選 **Restart the destination server automatically if required**
   點選 **Specify an alternate source path**
![Architecture](9.png)

10. 輸入 **SxS** 路徑
![Architecture](10.png)

11. 點擊 **Install**
![Architecture](11.png)

12. 完成
![Architecture](12.png)

### 設定 DNS
1. **Server Manager** → 點擊 旗標 → 點擊 **Promote this server to a domain controller**
![Architecture](13.png)

2. 點選 **Add a new forest** → 輸入 **Root domain name**
![Architecture](14.png)

3. 輸入自訂 **Password**
![Architecture](15.png)

4. **Next**
![Architecture](16.png)

5. **Next**
![Architecture](17.png)

6. **Next**
![Architecture](18.png)

7. **Next**
![Architecture](19.png)

8. **Install**
![Architecture](20.png)

### 查看 AD
1. 重開機後，已有 **Domain**
![Architecture](21.png)

2. 查看本機 **Domain**
![Architecture](22.png)

3. **Server Manager** → **Tools** → 點擊 **Active Directory Users and Computers**
![Architecture](23.png)

4. 查看 **Domain Controllers**
![Architecture](24.png)

### 查看 DNS
1. **Server Manager** → **Tools** → 點擊 **DNS**
![Architecture](25.png)

2. 查看 **Forward Lookup Zones**
![Architecture](26.png)

### Client 端設定 Domain
1. 修改 **Domain**
![Architecture](27.png)

2. 至 **Server** 查看
![Architecture](28.png)

# Reference
[Windows Server 2016 安裝 ADDS (AD網域服務) 及 DNS Server](http://weysnote.blogspot.com/2017/07/windows-server-2016-adds-ad.html)