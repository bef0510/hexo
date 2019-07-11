---
title: MS SQL Partition Table
date: 2019-07-11 14:15:12
 - Structrue
 - Partition
categories:
 - MS SQL
---

# Partition Table

#### 步驟及說明
1. 建立分割函數 **(Partition Functions)**
    作用 : 用來定義每個 **partition** 的臨界值

2. 建立分割配置 **(Partition Schemes)**
    作用 : 用來說明上一步驟定義的 **partition** 應該如何對應到檔案群組

3. 設定索引
    原因 : 分割表中，除了分割欄位以外，其餘不可設為叢集索引 
    作法 : 若原本的 **Table** 已有叢集索引(或 **PK** 值)，要先刪除叢集索引，再建立成非叢集索引(一樣是 **Primary Key** )

4. 分割資料
~~~ bash
--建立Partition Scheme叢集索引
    CREATE CLUSTERED INDEX [Ps_AACCT120_BAL_DATE] ON [AACCT120]
    (
        [BAL_DATE] ASC  (用來分割的欄位)
    ) WITH (SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF) ON [分割配置] ([分割欄位])
~~~

#### 新增分割區
1. 手動新增
~~~ bash
ALTER PARTITION SCHEME P_Scheme NEXT USED [檔案群組];   
ALTER PARTITION FUNCTION P_Func() SPLIT RANGE ('臨界點');   --新分割區
~~~

2. 動態新增
~~~ bash
Create Trigger
~~~

#### 查詢 Partition Table 相關資訊
1. 方法一
~~~ bash
SELECT t.name AS TableName, 
        i.name AS IndexName,        
        p.partition_number,             -- 分割編號
        p.rows,                         -- 區間筆數
        --r.boundary_id,            
        r.value AS BoundaryValue        -- 分割臨界值
        --,p.partition_id,
        --i.data_space_id, 
        --f.function_id, 
        --f.type_desc,       
FROM sys.tables AS t
JOIN sys.indexes AS i
    ON t.object_id = i.object_id
JOIN sys.partitions AS p
    ON i.object_id = p.object_id AND i.index_id = p.index_id 
JOIN  sys.partition_schemes AS s 
    ON i.data_space_id = s.data_space_id
JOIN sys.partition_functions AS f 
    ON s.function_id = f.function_id
LEFT JOIN sys.partition_range_values AS r 
    ON f.function_id = r.function_id and r.boundary_id = p.partition_number
WHERE t.name = 'AACCT120' AND i.type <= 1
ORDER BY p.partition_number;
~~~

2. 方法二
~~~ bash
select $PARTITION.Pf_AACCT120(BAL_DATE)  as 分區編號,
        count(1) as 記錄數 
from  dbo.AACCT120
group by $PARTITION.Pf_AACCT120(BAL_DATE)
~~~

#### SWITCH 轉移分割區
作用：可將資料從原本的 **Table** 轉移到另一個 **Table**。因為這種作業只是變換系統目錄的指標，所以不管資料有多少筆，都是一瞬間就完成的事
限制： 
&nbsp;&nbsp;&nbsp;&nbsp;• 必須在一樣的檔案群組裡
&nbsp;&nbsp;&nbsp;&nbsp;• 兩個TABLE 結構要一樣
&nbsp;&nbsp;&nbsp;&nbsp;• 索引或條件約束也需建立
&nbsp;&nbsp;&nbsp;&nbsp;• 非叢集索引必須加上包含分割欄位
語法：
~~~ bash
ALTER TABLE [被轉移的TABLE] SWITCH TO [轉移的目的TABLE] PARTITION 5
~~~

#### MERGE  合併 (刪除) 分割區
作用：在分割函數中將多餘的臨界點刪除
語法：
~~~ bash
ALTER PARTITION FUNCTION [分割函數()]  MERGE RANGE ('臨界點')
~~~

#### 分割資料表 轉為普通資料表
作法：
&nbsp;&nbsp;&nbsp;&nbsp;• 刪除所有臨界點，將分割區合併為一區
&nbsp;&nbsp;&nbsp;&nbsp;• 刪除Partition Index (叢集索引)
&nbsp;&nbsp;&nbsp;&nbsp;• 將原本TABLE的非叢集索引改為叢集索引(先刪除再新建)

# Reference
[效能調校(5)-使用資料表分割](http://vito-note.blogspot.tw/2013/05/blog-post_30.html)