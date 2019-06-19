---
title: Windows Server 2016 DFS 複寫備援
date: 2019-05-22 16:30:13
tags: 
 - Server
 - NLB
 - DFS
categories: 
 - MIS
---

# DFS 複寫備援
    需先安裝 AD 及 DNS
 [Windows Server 2016 啟用 AD 及 DNS](/2019/05/24/InstallADAndDNS/)

### DFS 安裝
**Server** 設定
1. **Server Manager** → **Add roles and features**
![Architecture](1.png)

2. **Next**
![Architecture](2.png)

3. **Next**
![Architecture](3.png)

4. **Next**
![Architecture](4.png)

5. 勾選 **File Server**
   勾選 **DFS Namespaces**
   勾選 **DFS Replication**
![Architecture](5.png)

6. **Next**
![Architecture](6.png)

7. 勾選 重啟電腦 → **Install**
![Architecture](7.png)

### Namespaces A Server 設定
1. **Server Manager** → **Tools** → 點擊 **DFS Management**
![Architecture](8.png)

2. **Namespaces** 右鍵 → **New Namespace...**
![Architecture](9.png)

3. 輸入 **A Server Name**
![Architecture](10.png)

4. 輸入 名稱 → 點擊　**Edit Settings**
![Architecture](11.png)

5. 選擇 檔案路徑 → 選擇 **All users have read and write permissions**
![Architecture](12.png)

6. **Next**
![Architecture](13.png)

7. **Next**
![Architecture](14.png)

8. **Create**
![Architecture](15.png)

9. **Close**
![Architecture](16.png)

### Namespaces B Server 設定
1. **A Server** 右鍵 → **Add Namespace Server...**
![Architecture](17.png)

2. 輸入 名稱 → 點擊　**Edit Settings**
![Architecture](18.png)

3. 選擇 檔案路徑 → 選擇 **All users have read and write permissions**
![Architecture](19.png)

4. **OK**
![Architecture](20.png)

### Replication 設定
1. **Replication** 右鍵 → **New Replication Group...**
![Architecture](22.png)

2. 選擇 **Replication group for data collection**
![Architecture](23.png)

3. 輸入 名稱 → **Next**
![Architecture](24.png)

4. 輸入 **A Server Name**
![Architecture](25.png)

5. 點擊 **Add...**
![Architecture](26.png)

6. 選擇 路徑 → **OK**
![Architecture](27.png)

7. **Next**
![Architecture](28.png)

8. 輸入 **B Server Name**
![Architecture](29.png)

9. 選擇 路徑 → **Next**
![Architecture](30.png)

10. **Next**
![Architecture](31.png)

11. **Create**
![Architecture](32.png)

12. **Close**
![Architecture](33.png)

### Folder Share
1. 確認 **Share** 給正確路徑
![Architecture](34.png)

2. 加入 **Domain Users** 權限
![Architecture](35.png)

# Reference
[IT BillBoard](http://itbillboard.blogspot.com/2016/11/windows-serverdfs-rdfsdfs-ndfs.html)
[MIS的背影](https://blog.pmail.idv.tw/?p=10523)