---
title: 常用 TCP Port 及 netstat 命令
date: 2019-06-21 15:59:06
tags: 
 - Server
categories: 
 - MIS
---

# 常用 TCP Port 列表
~~~ bash
  20:   FTP Data
  21:   FTP Connect
  22:   SFTP
  23:   Telnet 遠端登錄
  25:   SMTP 簡單郵件傳輸協定
  53:   DNS
  67:   DHCP
  68:   DHCP
  80:   HTTP
 109:   POP2 郵局協定 第2版 接收電子郵件
 110:   POP3 郵局協定 第3版 接收電子郵件
 111:   Sun遠端程序呼叫協定
 113:   Windows Authentication Service 驗證服務
 119:   NEWS 網路新聞傳輸協定
 137:   NetBIOS NetBIOS 名稱服務
 139:   NetBIOS NetBIOS 對談服務
 443:   HTTPS
1433:   Microsoft SQL 資料庫系統
1434:   Microsoft SQL 活動監視器
8080:   HTTP 代理伺服器
~~~

# Netstat 命令
~~~ bash
-a Displays all connections and listening ports
-e Displays Ethernet statistics. This may be combined with the -s option
-n Displays addresses and port numbers in numerical form
-o Displays the owning process ID associated with each connection
~~~

# Reference
[TCP/UDP埠列表](https://zh.wikipedia.org/wiki/TCP/UDP%E7%AB%AF%E5%8F%A3%E5%88%97%E8%A1%A8)
[常用 TCP Port作用(各種Port介紹)](https://yun1450.pixnet.net/blog/post/47494172-%E5%B8%B8%E7%94%A8-tcp-port%E4%BD%9C%E7%94%A8%28%E5%90%84%E7%A8%AEport%E4%BB%8B%E7%B4%B9%29)