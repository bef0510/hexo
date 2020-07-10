---
title: CreateAzureProjectWithGitOrTFVC
date: 2020-07-10 14:11:36
tags: 
 - Server
categories: 
 - Azure
---

# Create Azure Project With Git Or TFVC

1. **How TFVC Works**
![Architecture](46.png)

2. **How Git Works**
![Architecture](47.png)

## Using command mode

1. **Click New project**
![Architecture](1.png)

2. **Input information**
![Architecture](2.png)

3. **Click Repos**
![Architecture](3.png)

4. **Copy URL**
![Architecture](4.png)

5. **Open command mode**
![Architecture](5.png)
![Architecture](6.png)
![Architecture](7.png)

6. **Paste URL**
~~~ bash
git clone https://darren-sample-org@dev.azure.com/darren-sample-org/Project%20with%20git/_git/Project%20with%20git
~~~

7. **Login**
![Architecture](8.png)

8. **Done**
![Architecture](9.png)

## Using Visual Studio

1. **Open Visual Studio**
![Architecture](10.png)

2. **Click Clone**
![Architecture](11.png)

3. **Input information**
![Architecture](12.png)

## Using TortoiseGit

1. **Right click**
![Architecture](13.png)

2. **Input information**
![Architecture](14.png)

3. **Done**
![Architecture](15.png)

# Git Commit Push & Pull
## Use Command

1. **Open Command mode**
![Architecture](16.png)

2. **Create project**
~~~ bash
dotnet new mvc
~~~
![Architecture](17.png)

3. **git add**
~~~ bash
git add --all
~~~
![Architecture](18.png)

4. **git commit**
~~~ bash
git commit -m "Created new dot net core project"
~~~
![Architecture](19.png)

5. **git commit Done**
![Architecture](20.png)

6. **git push**
~~~ bash
git push
~~~
![Architecture](21.png)

7. **Click Files**
![Architecture](22.png)

8. **Open Command mode**
![Architecture](23.png)

9. **git pull**
~~~ bash
git pull
~~~
![Architecture](24.png)

## Use TortoiseGit

8. **Open Visual Studio Code**
![Architecture](25.png)

9. **Add File**
![Architecture](26.png)

10. **TortoiseGit Add**
![Architecture](27.png)

11. **Check File**
![Architecture](28.png)

12. **Check File Done**
![Architecture](29.png)

13. **TortoiseGit Git Copmmit**
![Architecture](30.png)

14. **Input information**
![Architecture](31.png)

15. **TortoiseGit Git Copmmit Done**
![Architecture](32.png)

16. **Check Azure Repos Files**
![Architecture](33.png)

17. **TortoiseGit Git Pull**
![Architecture](34.png)
![Architecture](35.png)
![Architecture](36.png)

18. **TortoiseGit Git Pull Done**
![Architecture](38.png)

## Use Visual Studio
1. **Open Visual Studio**
![Architecture](39.png)

2. **Select Folder**
![Architecture](40.png)

3. **Modify File**
![Architecture](41.png)

4. **Check File and input information**
![Architecture](42.png)

5. **Sync**
![Architecture](43.png)

6. **Sync Done**
![Architecture](44.png)

7. **Check Azure Repos Files**
![Architecture](45.png)

# Reference
[Create project with Git or TFVC | Azure devops tutorial for beginners](https://youtu.be/r6tgTdfdSnU?list=PLaFzfwmPR7_Ifxq-udm66fhReFeGOe2x_)