---
title: Create Azure Virtual Machine
date: 2020-07-09 09:41:13
tags: 
 - Server
categories: 
 - Azure
---

# Create Azure Virtual Machine

1. **Login** [Azure](https://portal.azure.com/)

2. **Click Virtual machines**
![Architecture](1.png)

3. **Click Add**
![Architecture](2.png)

4. **Input Basics Information**
![Architecture](3.png)

5. **Select Size**
![Architecture](4.png)

6. **Input Disks Information**
![Architecture](5.png)

7. **Make Sure Networking Information**
![Architecture](6.png)

8. **Make Sure Management Information**
![Architecture](7.png)

9. **Change Advanced Information**
**Click Select an extension to install**
![Architecture](8.png)
![Architecture](9.png)
![Architecture](10.png)
![Architecture](11.png)

10. **Input Tags Information**
![Architecture](12.png)

11. **Create Virtual Machine**
![Architecture](13.png)

12. **Creating**
![Architecture](14.png)

13. **Create Done**
![Architecture](15.png)

14. **Search Virtual Machines**
![Architecture](16.png)

# Remote Virtual Machines
1. **Select Virtual Machines**
![Architecture](17.png)

2. **Copy Public IP Address**
![Architecture](18.png)

3. **Hint Windows key And Type mstsc**
![Architecture](19.png)

4. **Paste IP Address**
![Architecture](20.png)

5. **Input Account And Password**
![Architecture](21.png)

# Instal IIS
1. **Click Add roles and features**
![Architecture](22.png)

2. **Click Next**
![Architecture](23.png)

3. **Click Next**
![Architecture](24.png)

4. **Click Next**
![Architecture](25.png)

5. **Select Web Server (IIS)**
![Architecture](26.png)

6. **Click Add Features**
![Architecture](27.png)

7. **Click Next**
![Architecture](28.png)

8. **Select Features**
![Architecture](29.png)

9. **Click Next**
![Architecture](30.png)

10. **Click Next**
![Architecture](31.png)

11. **Select Restart And Click Install**
![Architecture](32.png)

12. **Click Close**
![Architecture](33.png)

13. **After Restart Input http://localhost/ via Browser**
![Architecture](34.png)

14. **Click Networking And Click Add inbound port rule**
![Architecture](35.png)

15. **Modify Add inbound security rule**
![Architecture](36.png)

16. **Waiting For Few Second**
![Architecture](37.png)

17. **Input http://23.993.98.59 on local Browser**
![Architecture](38.png)

18. **Create index.html in Remote Server**
![Architecture](39.png)

19. **Input http://localhost/ in Remote Browser**
![Architecture](40.png)

20. **Input http://23.993.98.59 in local Browser**
![Architecture](41.png)

# Reference
[Azure Virtual Machine Tutorial | Creating A Virtual Machine In Azure | Azure Training | Simplilearn](https://www.youtube.com/watch?v=QOv_-xBXkpo)