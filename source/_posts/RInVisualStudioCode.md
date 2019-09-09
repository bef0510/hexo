---
title: R In Visual Studio 2017
date: 2019-09-09 08:55:01
tags:
 - R Basic
categories:
 - R
---

# R In Visual Studio 2017
1. [安裝 Visual Studio 2017](https://docs.microsoft.com/zh-tw/visualstudio/releasenotes/vs2017-relnotes)
![Architecture](1.png)
![Architecture](1-1.png)

2. [下載 Microsoft R Open](https://mran.revolutionanalytics.com/download)
![Architecture](2.png)

3. 安裝 **Microsoft R Open**
![Architecture](3.png)

4. 開啟 **Visual Studio 2017**
![Architecture](4.png)

5. 選取 **R**
![Architecture](5.png)

6. **script.R** 輸入
~~~ bash
say_hello <- function(name, df) {
    print(paste("Hello, ", name, "!", sep = ""))
}
~~~

7. 編譯
全部選取 → `Ctrl + Enter` 或
全部選取 → 右鍵
![Architecture](7.png)

8. 編譯成功
![Architecture](8.png)

9. 呼叫函式
輸入後按 `Enter`
~~~ bash
say_hello("World")
~~~
![Architecture](9.png)

10. **Debug**
**F5** 或 執行 **Source startup file**
![Architecture](10.png)
![Architecture](11.png)

# Reference
[Build 2017 A lap around R Tools 1 0 for Visual Studio 2017](https://www.youtube.com/watch?v=Swi8lwqoG0U)