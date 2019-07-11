---
title: Python Pandas 模組
date: 2019-07-09 16:54:20
tags:
 - Python Module
categories:
 - Python
---

# Pandas 模組
1. 安裝模組 **pip install pandas**
![Architecture](1.png)
2. **pan(el)-da(ta)-s**
    2.1 主要提供三個資料結構：**Panel、DataFrame、Series**
3. 以 **numpy** 模組為基礎的函式庫
4. **Series** 就是序列，只會儲存像是 **key:value** 的型態
5. 大部分談到 **pandas** 都會用 **DataFrame**
6. 支援多種文字、二進位檔案與資料庫
    6.1 **txt、csv、excel、SQL**

## Series
1. 要先 **import pandas**

#### 基本運用
    import pandas as pd
    # 直接給一個 dictionary
    scores = pd.Series({'小明':90, '小華':80, '小李':70})
    
    # 新增資料的方法
    scores['小強'] = 55
    print(scores)

    # 統計數據
    print(scores.describe())
    # 平均值
    print(scores.mean())

    a = ['apple', 'banana', 'cat', 'dog'] #陣列一
    b = [123, 456, 789, 56789] #陣列二
    sr = pd.Series(b, index = a) # 合併陣列，指定 a 為 index
    print(sr)

    c = [1, 2, 3, 4, 5, 6, 7, 8, 9]
    # pd.date_range是內建的日期序列產生器
    sr_2 = pd.Series(c, pd.date_range(start='2018-05-01', end='2018-05-9'))
    print(sr_2)

#### 判斷大小
    scores = pd.Series({'小明':90, '小華':80, '小李':70, '小強':55})
    print(scores > 60)
    
    print(scores[scores > 60])

    print((scores > 60) & (scores < 90))

    print(scores[(scores > 60) & (scores < 90)])

    new_scores = scores ** 0.5 * 10
    print(new_scores)

## DataFrame
1. **df.shape：** 這個 **DataFrame** 有幾列有幾欄
2. **df.columns：** 這個 **DataFrame** 的變數資訊
3. **df.index：** 這個 **DataFrame** 的列索引資訊
4. **df.info()：** 關於 **DataFrame** 的詳細資訊
5. **df.describe()：** 關於 **DataFrame** 各數值變數的描述統計

#### 基本運用 (三種方法結果一樣)
    import pandas as pd

    scores = [{"姓名":"小華", "國文":80, "數學":90},
            {"姓名":"小明", "國文":55, "數學":70},
            {"姓名":"小李", "國文":75, "數學":45}]
    score_df = pd.DataFrame(scores)
    print(score_df)

    # 純字典格式 -> 利用 from_dict 功能
    scores = {"姓名":["小華", "小明", "小李"],
            "國文":[80, 55, 75],
            "數學":[90, 70, 45]}
    score_df = pd.DataFrame.from_dict(scores)
    print(score_df)

    a = {"數學":90, "國文":80}
    b = {"數學":70, "國文":55}
    c = {"數學":45, "國文":75}
    df = pd.DataFrame.from_dict([a, b, c])
    # 增加一列的方法
    df["姓名"] = ["小華", "小明", "小李"]
    print(df)

#### Series 轉 Dataframe
    scores = pd.Series({"小明":90, "小華":80, "小李":70, "小強":55})
    score_df = scores.to_frame()
    print(score_df)

## 匯入檔案
1. 匯入 Excel 檔前要先安裝 **pip install xlrd**
![Architecture](2.png)

#### CSV 檔
    import pandas as pd

    csv_file = "https://storage.googleapis.com/learn_pd_like_tidyverse/gapminder.csv"
    gapminder = pd.read_csv(csv_file)
    print(type(gapminder))
    head5 = gapminder.head()   # 抓取最前面的值，預設值 5
    head3 = gapminder.head(3)
    tail5 = gapminder.tail()   # 抓取最後面的值，預設值 5
    print(head5)
    print(head3)
    print(tail5)

#### Excel 檔
    import pandas as pd

    xlsx_file = "https://storage.googleapis.com/learn_pd_like_tidyverse/gapminder.xlsx"
    gapminder = pd.read_excel(xlsx_file)
    print(type(gapminder))
    head5 = gapminder.head()   # 抓取最前面的值，預設值 5
    tail5 = gapminder.tail()   # 抓取最後面的值，預設值 5
    print(head5)
    print(tail5)

#### Excel 結合 DataFrame 函式
    dfShape = gapminder.shape
    dfColumns = gapminder.columns
    dfIndex = gapminder.index
    gapminder.info()    # 直接 print 出來
    dfDescribe = gapminder.describe()

    print(dfShape)
    print(dfColumns)
    print(dfIndex)
    print(dfDescribe)

## dplyr 的基本功能是六個能與 SQL 查詢語法相互呼應的函數
1. **filter()** 函數：**SQL** 的 **where**
2. **select()** 函數：**SQL** 的 **select**
3. **mutate()** 函數：**SQL** 的衍生欄位
4. **arrange()** 函數：**SQL** 的 **order by**
5. **summarise()** 函數：**SQL** 的 **sum、avg**
6. **group_by()** 函數：**SQL** 的 **group by**

#### filter()
    where1 = gapminder[gapminder["country"] == "Taiwan"]
    where1 = gapminder[gapminder.country == "Taiwan"]
    print(where1)

    where2 = gapminder[(gapminder["year"] == 2007) & (gapminder["continent"] == "Asia")]
    where2 = gapminder[(gapminder.year == 2007) & (gapminder.continent == "Asia")]
    print(where2)

#### select()
    # 多筆資料
    selectMulty = gapminder[["country", "continent"]]
    print(selectMulty)

    # 單筆資料
    selectSingle = gapminder["country"]
    selectSingle = gapminder.country
    print(selectSingle)

#### mutate()
    gapminder["country_abb"] = gapminder["country"].apply(lambda x: x[:3])
    gapminder["country_abb"] = gapminder.country.apply(lambda x: x[:3])
    print(gapminder)

#### arrange()
    orderby1 = gapminder.sort_values("year")
    print(orderby1)

#### summarise()
    # 總合
    sum1 = gapminder[gapminder["year"] == 2007][["pop"]].sum()
    sum1 = gapminder[gapminder.year == 2007][["pop"]].sum()
    print(sum1)

    # 平均
    mean1 = gapminder[gapminder["year"] == 2007][["lifeExp", "gdpPercap"]].mean()
    mean1 = gapminder[gapminder.year == 2007][["lifeExp", "gdpPercap"]].mean()
    print(mean1)

#### group_by()
    groupby1 = gapminder[gapminder["year"] == 2007].groupby(by = "continent")["pop"].sum()
    groupby1 = gapminder[gapminder.year == 2007].groupby(by = "continent")["pop"].sum()
    print(groupby1)

    groupby2 = gapminder[gapminder["year"] == 2007].groupby(by = "continent")[["lifeExp", "gdpPercap"]].mean()
    groupby2 = gapminder[gapminder.year == 2007].groupby(by = "continent")[["lifeExp", "gdpPercap"]].mean()
    print(groupby2)

# Reference
[從 pandas 開始 Python 與資料科學之旅](https://medium.com/datainpoint/%E5%BE%9E-pandas-%E9%96%8B%E5%A7%8B-python-%E8%88%87%E8%B3%87%E6%96%99%E7%A7%91%E5%AD%B8%E4%B9%8B%E6%97%85-8dee36796d4a)    