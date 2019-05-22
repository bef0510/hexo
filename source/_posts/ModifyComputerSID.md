---
title: Windows Server 2016 變更 SID
date: 2019-05-22 10:14:49
tags: 
 - Server
categories: 
 - MIS
---

# 變更 SID
### 查看原 SID
`cmd` 輸入
~~~ bash
$ whoami /user
~~~
![Architecture](1.png)

### 修改 SID
`cmd` 輸入
~~~ bash
$ cd C:\Windows\System32\Sysprep
$ Sysprep /generalize /oobe /reboot
~~~
![Architecture](2.png)

# Reference
[德瑞克：SQL Server 學習筆記](http://sharedderrick.blogspot.com/2017/02/sysprep-sid-windows-server-2016.html)