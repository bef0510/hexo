---
title: Create Azure Release Pipeline (CD)
date: 2020-07-13 15:47:18
tags: 
 - Server
categories: 
 - Azure
---

# Create Azure Release Pipeline
## Connect DevOps And VM

1. **Create Deployment groups**
![Architecture](1.png)
![Architecture](2.png)
![Architecture](3.png)

2. **Open Virtual Machine**
![Architecture](4.png)
![Architecture](5.png)

3. **Run Windows PowerShell**
![Architecture](6.png)

4. **Paste value**
![Architecture](7.png)

5. **Press Key Enter**
![Architecture](8.png)

6. **Press Key Enter**
![Architecture](9.png)

7. **Run Windows PowerShell Done**
![Architecture](10.png)

8. **Refresh Deployment groups**
![Architecture](11.png)

## Create New Release Pipeline
1. **Click Releases**
![Architecture](12.png)
![Architecture](13.png)

2. **Create Artifacts**
![Architecture](14.png)

3. **Input information**
![Architecture](15.png)

4. **Create Artifacts Done**
![Architecture](16.png)

5. **Create Stages**
![Architecture](17.png)

6. **Slect IIS website deployment**
![Architecture](18.png)

7. **Input information**
![Architecture](19.png)

8. **Click stage tasks**
![Architecture](20.png)

9. **Change Website name**
![Architecture](21.png)

10. **Add bindings**
![Architecture](22.png)

11. **Click IIS Deployment**
![Architecture](23.png)

12. **Input information**
![Architecture](24.png)

13. **Checked Enable IIS**
![Architecture](25.png)

14. **Click Save**
![Architecture](26.png)

## Set CI Trigger
1. **Click Edit**
![Architecture](27.png)

2. **Enabled Trigger**
![Architecture](28.png)

3. **Click Save**
![Architecture](29.png)

4. **Click Create release**
![Architecture](30.png)
![Architecture](31.png)
![Architecture](32.png)

5. **Refresh Releases and Click Title**
![Architecture](33.png)
![Architecture](34.png)

6. **Click View logs**
![Architecture](35.png)
![Architecture](36.png)

7. **Error occur**
![Architecture](37.png)

8. **Delete port 80**
![Architecture](38.png)

9. **Click Save**
![Architecture](39.png)
![Architecture](40.png)
![Architecture](41.png)

10. **Refresh Releases**
![Architecture](42.png)

11. **Goto VM IIS**
![Architecture](43.png)

12. **Run Web localhost**
![Architecture](44.png)
![Architecture](45.png)

13. **Modify and commit**
![Architecture](46.png)

14. **CD ing**
![Architecture](47.png)

15. **CD Done**
![Architecture](48.png)

16. **Remote Server Refresh Browser**
![Architecture](49.png)

# Reference
[Create Release pipeline in Azure DevOps | Create CD Pipeline | Azure DevOps tutorial](https://youtu.be/ZSynootLhCk?list=PLaFzfwmPR7_Ifxq-udm66fhReFeGOe2x_)