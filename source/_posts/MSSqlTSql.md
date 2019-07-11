---
title: MS SQL T-Sql
date: 2019-07-11 14:34:01
 - Script
categories:
 - MS SQL
---

# T-Sql

#### SELECT 固定值
    SELECT * FROM ( VALUES ( 'AA' , 'EE' ) , ( 'BB' , 'FF' ) ) AS B (欄位名A,欄位名B)

#### PIVOT 非數字轉置
    SELECT Class AS [班級],
        [1] AS [第一名],
        [2] AS [第二名],
        [3] AS [第三名]
    FROM (
        SELECT Class,
        SdtName,
        ROW_NUMBER() OVER (PARTITION BY class ORDER BY Score DESC) AS ROWNO
        FROM @Temp
    ) AS P
    PIVOT (
        MAX(SdtName) FOR ROWNO IN ([1],[2],[3])
    ) AS PV

#### 指定排序
    SELECT * FROM TABLE ORDER BY CHARINDEX(',' + CONVERT(varchar,ACCT_NO) + ',' , ',3540,3542,3100,') -- 以 '3540,3542,3100排序

#### 檢查資料是否存在，如果存在就更新，不存在就新增
    MERGE INTO 新增/修改TABLE AS A
    USING (
        比對的資料語法
    ) B
    ON  A.FUND_ID = B.FUND_ID
    AND A.DATA_DATE = B.DATA_DATE
    WHEN MATCHED THEN
        UPDATE SET A.NAV = B.NAV
    WHEN NOT MATCHED THEN
        INSERT (FUND_ID,NAV_DATE,NAV)
        VALUES (B.FUND_ID,B.NAV_DATE,B.NAV);

#### 兩筆資料合併為一筆顯示
    SELECT MainAccountID,
        (SELECT Memo+' , ' FROM #temp AS S2 WHERE S1.MainAccountID = S2.MainAccountID FOR XML PATH('')) AS Memo,
    Item
    FROM #temp  AS S1
    GROUP BY MainAccountID,Item

#### 二進位 (&2=2)
    select * from t_User where iFlag &2 = 2

#### 新增多筆資料，取得多筆的Identity
    INSERT INTO #TEMP2 (GroupItem,COUNTS)
    OUTPUT Inserted.Number,Inserted.GroupItem INTO #TEMP3
    SELECT GroupItem ,COUNT(DISTINCT ID_Member) COUNTs
    FROM #Temp
    GROUP BY GroupItem

#### 查詢EXCEL資料
    SELECT * FROM OpenDataSource('Microsoft.ACE.OLEDB.12.0',
        'Data Source=C:\Users\evatsai\Desktop\Letou_Member.xlsx;Extended Properties="Excel 12.0;HDR=Yes;IMEX=1"')...[工作表1$]

# Reference
[PIVOT 非數字轉置](http://jengting.blogspot.tw/2011/10/sql_21.html)
[二進位 (&2=2)](https://my.oschina.net/cjkall/blog/195907)