---
title: Angular Build Express Json Server
date: 2019-07-19 10:08:17
tags:
 - Express Server
categories: 
 - Angular
---

# Build Express Server
1. **Create Folder**

2. **Install**
`npm install -g json-server`
![Architecture](1.png)

3. **Run Server**
`json-server db.json`
![Architecture](2.png)
![Architecture](3.png)

# Change listening port number
1. **Create json-server.json file**
![Architecture](4.png)

2. **Input port number in json-server.json**
~~~ bash
{
    "port": 3020
}
~~~
![Architecture](5.png)

3. **Run Server**
`json-server db.json`
![Architecture](6.png)

# PostMan
1. **GET (Get Data)** 
![Architecture](7.png)
![Architecture](8.png)

2. **POST (Create Data)**
![Architecture](9.png)
![Architecture](10.png)

3. **PUT (Modify full Data)**
![Architecture](11.png)
![Architecture](12.png)

4. **PATCH (Modify partial Data)**
![Architecture](13.png)
![Architecture](14.png)

5. **DELETE (Delete Data)** (刪除資料)
![Architecture](15.png)
![Architecture](16.png)

# Reference
[JSON Server 使用](https://ithelp.ithome.com.tw/articles/10212106)