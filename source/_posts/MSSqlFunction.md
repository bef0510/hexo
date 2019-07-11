---
title: MS SQL 函式
date: 2019-07-11 14:47:15
tags:
 - Script
categories:
 - MS SQL
---

# 函式

#### 根據 A 欄位分割 以 B 欄位排序
    ROW_NUMBER() OVER (PARTITION BY A欄位 ORDER BY B欄位)
    
    DENSE_RANK() OVER (PARTITION BY A欄位 ORDER BY B欄位)
    
    RANK() OVER (PARTITION BY A欄位 ORDER BY B欄位)

#### 加上條件
    ROW_NUMBER() OVER (PARTITION BY  A欄位 ,(CASE WHEN 條件式 THEN 1 ELSE 0 END ORDER BY B欄位)

#### EOMONTH 回傳月最後一天  
    SELECT EOMONTH(GETDATE(),-1)  --上個月最後一天
    
    SELECT EOMONTH(GETDATE())      --這個月最後一天
    
    SELECT EOMONTH(GETDATE(),1)   --下個月最後一天

#### SUM OVER (累計)
    SELECT SUM(AMT) OVER (ODDER BY YYYMM)

#### 識別碼
• **SCOPE_IDENTITY**
&nbsp;&nbsp;&nbsp;&nbsp;使用方式：**SELECT SCOPE_IDENTITY()**
&nbsp;&nbsp;&nbsp;&nbsp;說明：傳回當前作業(預存程序、觸發程序、函數或批次)中，最後一個識別值(索引值)。
• **@@IDENTITY**
&nbsp;&nbsp;&nbsp;&nbsp;使用方式：**SELECT @@IDENTITY**
&nbsp;&nbsp;&nbsp;&nbsp;說明：傳回資料庫所有作業中，最後一個識別值(索引值)。
• **IDENT_CURRENT**
&nbsp;&nbsp;&nbsp;&nbsp;使用方式：**SELECT IDENT_CURRENT**('資料表名稱')
&nbsp;&nbsp;&nbsp;&nbsp;說明：傳回指定資料表中最後一個識別值(索引值)。**P.S.** 無法對暫存表使用

#### WITH(XLOCK, ROWLOCK)
    SELECT 時 LOCK ROW

#### XACT_ABORT + XACT_STATE()
    SET XACT_ABORT ON;
    BEGIN TRY  
        BEGIN TRANSACTION;  
     
        COMMIT TRANSACTION;  
    END TRY  
    BEGIN CATCH   
        IF (XACT_STATE()) = -1      -- 有錯誤、需要做 ROLLBACK
        BEGIN       
            ROLLBACK TRANSACTION;  
        END;  
    
                                    --(XACT_STATE()) = 0 表示沒有 TRANSACTION，不需 ROLLBACK 或 COMMIT
        IF (XACT_STATE()) = 1       -- 沒有錯誤且有T RANSACTION，需要做 COMMIT
        BEGIN       
            COMMIT TRANSACTION;     
        END;  
    END CATCH; 
假設 **INSERT** 三筆，第 1、3 筆成功，第 2 筆失敗
&nbsp;&nbsp;&nbsp;&nbsp;• **SET XACT_ABORT ON : RollBack** 3 筆資料
&nbsp;&nbsp;&nbsp;&nbsp;• **SET XACT_ABORT OFF :** 寫入第 1、3 筆，**RollBack** 第 2 筆資料
      
#### CURSOR
    要加 CURSOR LOCAL FORWARD_ONLY STATIC READ_ONLY FOR

#### 修改發送MAIL的預設大小
    EXECUTE msdb.dbo.sysmail_configure_sp 'MaxFileSize', '5242880';