---
title: MS SQL SSIS 佈署
date: 2019-07-11 15:26:46
tags:
 - Server
 - SSIS
categories: 
 - MS SQL
---

# SSIS 佈署

#### 建置部署公用程式
1. 開啟 **SQL Server Business Intelligence Development Studio**
![Architecture](1.png)

2. 開啟 **.sln** 檔
![Architecture](2.png)

3. 設定參數
![Architecture](3.png)

4. 建置
![Architecture](4.png)

5. 檢查是否有產生部署公用程式
![Architecture](5.png)

## 建立封裝組態
#### 目的
若佈署到 **SQL Server** 上，要多設定一個「封裝組態」，否則會因為帳號密碼權限的問題，不能執行封裝

#### Source DB 新增欄位，重新部署
1. 先將原始檔案複製一份，留存

2. 開啟 **.sln** 檔
![Architecture](6.png)

3. 將資料來源 設定帳密
&nbsp;&nbsp;&nbsp;&nbsp;目的：開啟 **Source DB**、**Targer DB** 可以看到欄位，並建立關連
![Architecture](7.png)

4. 檢查 **Source DB** 的 **SCHEMA** 是否已建立
![Architecture](8.png)

5. 新增 **Target DB SCHEMA**
![Architecture](9.png)

6. 建立 **Source DB & Target DB** 對應
![Architecture](10.png)

7. 依照 `Z:\經營總表\10-BI\系統文件\BI 平台系統文件-SSIS封裝佈署.doc` 步驟，重新部署
    7.1 啟用封裝組態
    ![Architecture](11.png)

    7.2 切換至「屬性」，於 **ProtectionLevel** 下拉選單，選取 **DontSaveSensitive**
    ![Architecture](12.jpg)

    7.3 CUBE封裝以外的其他三個封裝皆完成以上動作後，請切換回「方案總管」，並於資料庫上按右鍵，選取「建置」
    ![Architecture](13.jpg)

    7.4 佈署封裝檔：
        封裝建置完成後，請開啟檔案總管，至 `D:\BI\SSIS\禮儀資料轉檔\bin\Deployment` 目錄下，點選檔案「禮儀資料轉檔 **.SSISDeploymentManifest** 」
    ![Architecture](14.jpg)

    7.5 點選後會開啟以下的封裝安裝精靈，請按下一步
    ![Architecture](15.jpg)

    7.6 封裝安裝位置請選「SQL Server佈署」，請按下一步
    ![Architecture](16.jpg)

    7.7 伺服器名稱預設為 ( **local** ) 無需修改，帳密資訊輸入後，封裝路徑請按右方「**…**」按鈕，再點選「**SSIS**封裝」目錄，即根目錄，按確定後回到主畫面按下一步
    ![Architecture](17.jpg)

    7.8 安裝資料夾依預設值即可，無需修改，請持續按下一步至設定封裝畫面
    ![Architecture](18.jpg)

    7.9 設定封裝畫面，可看到有三個組態檔，每個組態檔都要點進去，並點開下方的 **Property** 以輸入密碼，三個組態檔皆完成後，請按下一步進行佈署
    ![Architecture](19.jpg)

    7.10 出現以下畫面即代表封裝已佈署完成，請按完成按鈕
    ![Architecture](20.jpg)

8. 回復原始設定，才能直接用 **SSIS** 介面執行工作
    8.1 再將啟用封裝組態 勾勾 拿掉
    ![Architecture](21.png)

    8.2 「屬性」，於 **ProtectionLevel** 下拉選單，選取「**EncrptSensitiveWithUserKey**」
    ![Architecture](22.png)

# Reference
[建置部署公用程式](https://docs.microsoft.com/zh-tw/sql/integration-services/lesson-2-1-building-the-deployment-utility?view=sql-server-2017)
[建立封裝組態](http://tsengpm.blogspot.com/2011/09/ssis-sql-server.html)