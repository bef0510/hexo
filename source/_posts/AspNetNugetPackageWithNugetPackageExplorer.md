---
title: ASP.NET NuGet 打包，使用 NuGet Package Explorer
date: 2019-06-20 14:50:42
tags:
 - NuGet
 - NuGet Package Explorer
categories: 
 - ASP.Net
---

# 使用 NuGet Package Explorer 打包
先建置 [NuGet Server](/2019/06/20/AspNetCreateNugetServer/)

1. 下載 [NuGet Package Explorer](https://www.microsoft.com/zh-tw/p/nuget-package-explorer/9wzdncrdmdm3?activetab=pivot:overviewtab)
![Architecture](1.png)

2. 開啟 **NuGet Package Explorer** → **Create a new package**
![Architecture](2.png)

3. 編輯資訊
![Architecture](3.png)
![Architecture](3-1.png)

4. **Package contents** 右鍵 → **Add Lib Folder**
![Architecture](4.png)

5. 新增 類別庫 專案
![Architecture](5.png)

6. 增加 **Class1** 屬性
![Architecture](6.png)

7. 將建好的 **dll** 拉進 **NuGet Package Explorer** → **Package contents** → **lib**
![Architecture](7.png)

8. **Publish**
![Architecture](8.png)

9. 輸入 **Url** 及 **key**
![Architecture](9.png)

10. 發佈成功
![Architecture](10.png)

11. **NuGet Server** 專案 → **Packages** 產生 **.nupkg** 檔案
![Architecture](11.png)

12. 到其他專案選擇 封裝來源，可以看剛發佈的 **package**
![Architecture](12.png)

13. 可正常引用從 **NuGet** 下載的物件
![Architecture](13.png)

# Reference
[Using the NuGet Package Explorer to Create, Explore and Publish Packages](http://rion.io/2014/07/20/using-the-nuget-package-explorer-to-create-explore-and-publish-packages/)