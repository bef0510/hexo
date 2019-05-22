---
title: NLB 網路負載平衡
date: 2019-04-26 17:34:29
tags: 
 - Server
 - IIS
categories: 
 - MIS
---

# 網路負載平衡設定
    IP 皆為固定，叢集 IP 不能被使用
1. **A server IP ：** `172.27.59.161`
2. **B server IP ：** `172.27.59.162`
3. **叢集 IP ：** `172.27.59.163`
![Architecture](0.gif)

### 安裝 NLB
**Server** 設定

1. **Server Manager** → **Add roles and features**
![Architecture](1.png)

2. **Next**
![Architecture](2.png)

3. **Next**
![Architecture](3.png)

4. **Next**
![Architecture](4.png)

5. 勾選 **Network Load Balancing** → **Next**
![Architecture](5.png)

6. 勾選 重啟電腦 → 點擊 **Specify an alternate source path**
![Architecture](6.png)

7. 輸入 `sxs` **Path**
![Architecture](7.png)

8. 點擊 **Install**
![Architecture](8.png)

### 設定 A Server 及 叢集
1. **Server Manager** → **Tools** → 點擊 **Network Load Balancing Manager**
![Architecture](10.png)

2. 右鍵 → 點擊 **New Cluster**
![Architecture](11.png)

3. 輸入 **A Server IP**
![Architecture](12.png)

4. **Next**
![Architecture](13.png)

5. 點擊 **Add**
![Architecture](14.png)

6. 輸入 **叢集 IP**
![Architecture](15.png)

7. **Next**
![Architecture](16.png)

8. 選擇 **Multicast**
![Architecture](17.png)

9. **Finish**
![Architecture](18.png)

### 設定 B Server
1. 叢集右鍵 → **Add Host To Cluster**
![Architecture](20.png)

2. 輸入 **B Server IP**
![Architecture](21.png)

3. **Priority** 選 **2**
![Architecture](22.png)

4. **Finish**
![Architecture](23.png)

5. 設定完成
![Architecture](24.png)

# Reference
[網路負載平衡| Microsoft Docs](https://docs.microsoft.com/zh-tw/windows-server/networking/technologies/network-load-balancing)
[108-1網工班-Windows Server 2019-IIS Server 網路負載平衡-16-1](https://www.youtube.com/watch?v=Zx4s59nFX3E&t=1093s)