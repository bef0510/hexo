---
title: Windows Server 2016 DFS 設定 Ⅱ
date: 2019-08-21 11:02:35
tags: 
 - Server
 - NLB
 - DFS
categories: 
 - MIS
---

# DFS 設定 Ⅱ
#### Server A & Server B
1. **Folder** 右鍵 → **Properties**
![Architecture](1.png)

2. **Sharing** → **Advanced Sharing...**
![Architecture](2.png)

3. **checked Share this folder** → **Permissions**
![Architecture](3.png)

4. 勾選 **Full Control**
![Architecture](4.png)

5. **Security** → **Edit**
![Architecture](5.png)

6. **Add...**
![Architecture](6.png)

7. 輸入 **Domain Users** → **EdiCheck Names**
![Architecture](7.png)

8. 輸入 **Domain** 帳密
![Architecture](8.png)

9. **Domain Users** → 勾選 **Full Control**
![Architecture](9.png)

10. 檔案總管輸入 **\\\\serverName\shareFolder**
![Architecture](10.png)

#### DFS Server
1. **Namespaces** 右鍵 → **New Namespace...**
![Architecture](11.png)

2. 輸入 **Server A Name** → **Next**
![Architecture](12.png)

3. 輸入 **Name** → **Edit Settings...**
![Architecture](13.png)

4. 選取 **Use custom permissions** → **Customize...**
![Architecture](14.png)

5. 勾選 **Full Control**
![Architecture](15.png)

6. **Next**
![Architecture](16.png)

7. **Next**
![Architecture](17.png)

8. **Create**
![Architecture](18.png)

9. **Close**
![Architecture](19.png)

10. 檔案總管輸入 **\\\\serverName\shareName**
![Architecture](20.png)

11. **Name** 右鍵 → **New Folder...**
![Architecture](21.png)

12. 輸入 **Name** → **Add...**
![Architecture](22.png)

13. 輸入 **Server A** → **Browse...**
![Architecture](23.png)

14. 點選 **Shared folders** → **OK**
![Architecture](24.png)

15. **OK**
![Architecture](25.png)

16. **Add...**
![Architecture](26.png)

17. 輸入 **Server B** → **Browse...**
![Architecture](27.png)

18. 點選 **Shared folders** → **OK**
![Architecture](28.png)

19. **OK**
![Architecture](29.png)

20. **OK**
![Architecture](30.png)

21. **Yes**
![Architecture](31.png)

22. **Next**
![Architecture](32.png)

23. **Next**
![Architecture](33.png)

24. 選取 **Server A** → **Next**
![Architecture](34.png)

25. 選取 **Full mesh** → **Next**
![Architecture](35.png)

26. 選取 **Replicate continuously using the specified bandwidth** → **Next**
![Architecture](36.png)

27. **Create**
![Architecture](37.png)

28. **Close**
![Architecture](38.png)

29. **OK**
![Architecture](39.png)

# Reference
[How to configure Distributed File System in Windows Server 2016](https://www.youtube.com/watch?v=CNL-hAC7CYA)