---
title: ASP.NET NuGet 打包，使用 NuGet Packager
date: 2019-06-20 15:35:06
tags:
 - NuGet
 - Visual Studio
categories: 
 - ASP.Net
---

# 使用 NuGet Packager 打包
先建置 [NuGet Server](/2019/06/20/AspNetCreateNugetServer/)

1. **Visual Studio** → 工具 → 擴充功能和更新
![Architecture](1.png)

2. 下載 **NuGet Packager**
下載後要重啟 **Visual Studio**
![Architecture](2.png)

3. 選擇 **NuGet** 範本
![Architecture](3.png)

4. **NuGet.config** 修改 `packageSources` 值
![Architecture](4.png)

5. 同方案底下，新增一個類別庫
![Architecture](5.png)
![Architecture](5-1.png)

6. 增加 **Class1** 屬性
![Architecture](6.png)

7. 建置 **TestLibrary2** 專案，產生 **dll** 檔
![Architecture](7.png)

8. 修改 **NuGet.Packager** 專案 → **Package.nuspec** 資訊
將 **TestLibrary2** 引用進來
![Architecture](8.png)

9. 選擇 **Release**，重建 **NuGet.Packager** 專案
![Architecture](9.png)

10. 輸入 **A** 及 **API Key**
![Architecture](10.png)

11. 發佈成功
![Architecture](11.png)

12. **NuGet Server** 專案 → **Packages** 產生 **.nupkg** 檔案
![Architecture](12.png)

13. 到其他專案選擇 封裝來源，可以看剛發佈的 **package**
![Architecture](13.png)

14. 可正常引用從 **NuGet** 下載的物件
![Architecture](14.png)

# Reference
[從Visual Studio發布NuGet Package的好幫手－NuGet Packager](https://blog.darkthread.net/blog/nuget-packager/)