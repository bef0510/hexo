---
title: Create Azure Pipeline (CI)
date: 2020-07-13 10:30:02
tags: 
 - Server
categories: 
 - Azure
---

# Create Azure Pipeline

## Create New Project To Git

1. **Create New Organization**
![Architecture](1.png)
![Architecture](2.png)
![Architecture](3.png)
![Architecture](4.png)

2. **Click Title**
![Architecture](5.png)

3. **Input information**
![Architecture](6.png)

4. **Click Repos Files**
![Architecture](7.png)

5. **Create new project**
![Architecture](8.png)

6. **Add to git**
~~~ bash
git init
~~~
![Architecture](9.png)

7. **Add new files into git**
~~~ bash
git add --all
~~~
![Architecture](10.png)

8. **Add to local repository**
~~~ bash
git commit -m "create new project"
~~~
![Architecture](11.png)

9. **Copy Push an existing repository from command line**
![Architecture](12.png)

10. **Paste to Command mode**
![Architecture](13.png)

11. **Command mode Enter**
![Architecture](14.png)
![Architecture](15.png)

12. **Refresh Repos Files**
![Architecture](16.png)

## Create New Pipelines
1. **Click Pipelines**
![Architecture](17.png)

2. **Click Azure Repos Git**
![Architecture](18.png)

3. **Click repository**
![Architecture](19.png)

4. **Click Show more**
![Architecture](20.png)

5. **Click Type**
![Architecture](21.png)

6. **Click Save and run**
![Architecture](22.png)
![Architecture](23.png)

7. **Save and run Done**
![Architecture](24.png)
![Architecture](25.png)

8. **Pull yaml from git**
~~~ bash
git pull
~~~
![Architecture](26.png)

9. **Open Visual Studio Code**
![Architecture](27.png)

## Run Pipelines
1. **Click Pipelines Project**
![Architecture](28.png)

2. **Click Run pipeline**
![Architecture](29.png)

3. **Click Run**
![Architecture](30.png)

4. **Run Done**
![Architecture](31.png)
![Architecture](32.png)

5. **Get mail**
![Architecture](33.png)

6. **VS Code Commint (error test)**
![Architecture](34.png)
![Architecture](35.png)

7. **git push**
~~~ bash
git push
~~~
![Architecture](36.png)
![Architecture](37.png)

8. **Get mail**
![Architecture](38.png)

9. **Fixed error**
![Architecture](39.png)
![Architecture](40.png)
![Architecture](41.png)

10. **Download logs**
![Architecture](42.png)

# Reference
[Create Build Pipeline in Azure DevOps | Azure DevOps tutorial for beginners](https://youtu.be/2CZVIxRF22c?list=PLaFzfwmPR7_Ifxq-udm66fhReFeGOe2x_)