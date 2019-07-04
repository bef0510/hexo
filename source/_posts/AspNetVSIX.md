---
title: Visual Studio 2015 使用 SideWaffle 建立 Project Templat
date: 2019-07-04 13:08:13
tags:
 - Visual Studio
 - VSIX
 - SideWaffle
categories: 
 - ASP.Net
---

# 使用 SideWaffle 建立 VSIX

1. 安裝 **Visual Studio** 擴充性工具
![Architecture](1-1.png)
![Architecture](1-2.png)

2. **Extensions and Updates** 安裝 **SideWaffle Template Pack**
![Architecture](2.png)

3. 建立 **VSIX** 專案
![Architecture](3.png)

4. 移除 `index.html` 及 `stylesheet.css`
![Architecture](4.png)

5. 加入一個專案
![Architecture](5.png)

6. **VSIX** 專案右鍵 → **Add** → **Add Template Reference (SideWaffle project)** → 加入要當 **Template** 專案
![Architecture](6-1.png)
![Architecture](6-2.png)

7. 產生檔案
![Architecture](7.png)

8. 修改 `_project.vstemplate.xml` 檔案，修改 `File 屬性` 改為專案名稱，刪除 `WizardExtension tag`
![Architecture](8-1.png)
![Architecture](8-2.png)

9. `_preprocess.xml` 檔案，`TemplateInfo` 路徑為 範本位置
![Architecture](9.png)

10. 修改 **Configuration Manager**，取消 **Template** 專案 **Build**
![Architecture](10-1.png)
![Architecture](10-2.png)

11. **Run** 專案，會 **Build** 起另一個 **Visual Studio - Experimental Instance**
![Architecture](11-1.png)
![Architecture](11-2.png)

12. 新增專案
![Architecture](12.png)

13. 新增的專案，直接有 **Template** 程式
![Architecture](13.png)

14. 複製 **.vsix** 檔
![Architecture](14.png)

15. 安裝 **.vsix** 檔
![Architecture](15.png)

16. 安裝後 **Visual Studio** 就有自訂義的 **Template**
![Architecture](16.png)

# 刪除 VSIX
1. 刪除檔案
`%userprofile%\AppData\Local\Microsoft\VisualStudio\14.0\Extensions\gtan0kof.kp4\Output\ProjectTemplates\%template路徑%`
![Architecture](17.png)

2. **Template** 已移除
![Architecture](18.png)

# Reference
[SideWaffle建立Project Template](https://blog.alantsai.net/posts/2017/09/buildyourowntemplate-use-sidewaffle-to-create-project-template)