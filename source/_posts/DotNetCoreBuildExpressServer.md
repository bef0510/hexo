---
title: .Net Core 3 + ngrok 對外 domain
date: 2019-12-24 17:32:36
tags:
 - .Net Core
 - Express Server
categories: 
 - .Net Core
---

# .Net Core 3 + ngrok 對外 domain
1. 下載 [ngrok](https://dashboard.ngrok.com/get-started)
![Architecture](1.png)

2. 設定 **auth token**
~~~ bash
ngrok authtoken 1VPY...........................
~~~
![Architecture](2.png)

3. 啟動程式
![Architecture](3.png)

4. 設定 **IP**
~~~ bash
ngrok http 5000 -host-header="localhost:5000"
~~~
![Architecture](4.png)
![Architecture](5.png)

# 多重站台
1. 修改 **folder .ngrok2** 底下的 **ngrok.yml**
![Architecture](6.png)

2. 修改 **ngrok.yml**
~~~ bash
authtoken: 1VPYX0MJGGwnVXHE8htea80qr8c_6CVoNuuoiw4iKEo4pJ8fw
tunnels:
  test-web1:
    addr: 9090
    proto: http
    host_header: localhost
    bind_tls: true
  test-web2:
    addr: 64443
    proto: http
    host_header: localhost
    bind_tls: true
~~~

# Reference
[[Tools] 透過 ngrok 讓外網能連到localhost服務](https://harry-lin.blogspot.com/2019/01/tools-ngrok-localhost.html)