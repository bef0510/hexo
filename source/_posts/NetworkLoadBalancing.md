---
title: NLB 網路負載平衡
date: 2019-04-26 17:34:29
tags: 
 - IIS
categories: 
 - MIS
---

# 網路負載平衡設定
1. **A server IP ：** `192.168.1.101`
2. **B server IP ：** `192.168.1.102`
3. **叢集 IP ：** `192.168.1.103`
![Architecture](1.gif)

### A server 管理 B server
在 **A server**
[伺服器管理員] →[管理] →[新增伺服器]
[立即尋抓] **B server**

### B server 開啟 網路負載平衡 功能
[伺服器管理員] →[管理] →[新增角色及功能]
選 **B server** [下一步]
[功能] → 勾選 [網路負載平衡]
[安裝]

### A server 開啟 網路負載平衡 功能
重覆 **B server** 動作，改選 **A server**

### 叢集設定
在 **A server** 開啟 [網路負載平衡管理]
**A server** 設定

1. 在 <span style="background-color:#f8f906;color:#0000FF;">[網路負載平衡叢集]</span> 按右鍵 →<span style="background-color:#f8f906;color:#0000FF;">[新增叢集]</span>
2. 主機：輸入 `192.168.1.101` →<span style="background-color:#f8f906;color:#0000FF;">[連線]</span>
3. <span style="background-color:#f8f906;color:#0000FF;">[下一步]</span> (不要點其他地方，<span style="background-color:#f8f906;color:#0000FF;">[下一步]</span> 會不見，**Server 2019** 也還沒修好)
4. <span style="background-color:#f8f906;color:#0000FF;">[優先順序]</span> 選 `1`
5. <span style="background-color:#f8f906;color:#0000FF;">[下一步]</span>
6. 新增 **叢集 IP** `192.168.1.103`
7. <span style="background-color:#f8f906;color:#0000FF;">[下一步]</span>
8. <span style="background-color:#f8f906;color:#0000FF;">[叢集操作模式]</span> 選 <span style="background-color:#f8f906;color:#0000FF;">[多點傳送]</span> <span style="background-color:#f8f906;color:#0000FF;">[下一步]</span> (虛擬機只能選多點，實體機能選單點、多點)
9. 等漏斗跑完就完成 <span style="background-color:#f8f906;color:#0000FF;">[交集]</span>

**B server** 設定
1. 在 IP <span style="background-color:#f8f906;color:#0000FF;">192.168.1.100</span> 按右鍵 →新增主機到叢集
2. 主機：**B server** `192.168.1.102` →連線 (因遠端，會比較久，要有耐心)
3. <span style="background-color:#f8f906;color:#0000FF;">下一步</span> (不要點其他地方，下一步會不見，把生命浪費在有意義的地方)
4. <span style="background-color:#f8f906;color:#0000FF;">優先順序</span> 選 `2`
5. <span style="background-color:#f8f906;color:#0000FF;">下一步</span>
6. <span style="background-color:#f8f906;color:#0000FF;">完成</span>
7. 等漏斗跑完就完成 <span style="background-color:#f8f906;color:#0000FF;">[交集]</span>

**A B server** 已 <span style="background-color:#f8f906;color:#0000FF;">[交集]</span> 後，兩台 **server** 都要重開機

# DNS server 設定
1. 新增主機
2. 輸入名稱
3. 輸入 **IP** `192.168.1.100`

# Reference
[網路負載平衡| Microsoft Docs](https://docs.microsoft.com/zh-tw/windows-server/networking/technologies/network-load-balancing)
[108-1網工班-Windows Server 2019-IIS Server 網路負載平衡-16-1](https://www.youtube.com/watch?v=Zx4s59nFX3E&t=1093s)