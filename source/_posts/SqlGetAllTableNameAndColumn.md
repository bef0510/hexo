---
title: SQL Server - 取得所有資料表及欄位
date: 2019-06-14 10:25:44
tags:
 - Script
categories: 
 - MS SQL
---

# 取得所有資料表及欄位
#### **All Tables**
    SELECT * FROM INFORMATION_SCHEMA.Tables

#### **All Columns**
    SELECT * FROM INFORMATION_SCHEMA.Columns Where Table_Name = 'TableName'

#### **Combine Tables and columns**
    Declare @tableName varchar(200) = 'TableName'
    SELECT
        a.TABLE_NAME                AS 表格名稱,
        b.COLUMN_NAME               AS 欄位名稱,
        b.DATA_TYPE                 AS 資料型別,
        b.CHARACTER_MAXIMUM_LENGTH  AS 最大長度,
        b.COLUMN_DEFAULT            AS 預設值,
        b.IS_NULLABLE               AS 允許空值,
        (
            SELECT value
            FROM fn_listextendedproperty (NULL, 'schema', 'dbo', 'table', a.TABLE_NAME, 'column', default)
            WHERE name = 'MS_Description' 
	    		AND objtype = 'COLUMN' 
                AND objname Collate Chinese_Taiwan_Stroke_CI_AS = b.COLUMN_NAME
        )							AS 欄位備註
    FROM INFORMATION_SCHEMA.TABLES a
    LEFT JOIN INFORMATION_SCHEMA.COLUMNS b ON (a.TABLE_NAME = b.TABLE_NAME)
    WHERE TABLE_TYPE='BASE TABLE' AND a.TABLE_NAME = @tableName
    ORDER BY a.TABLE_NAME, ordinal_position

# Reference
[在 SQL Server 2005 中取得所有欄位定義的方法(含備註欄位)](https://blog.miniasp.com/post/2007/11/05/How-to-get-detailed-Data-Dictionary-in-SQL-Server-2005)