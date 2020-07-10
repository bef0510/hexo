---
title: Solr Basic Install With Win 10
date: 2020-01-17 15:50:49
tags:
 - Solr
categories: 
 - Text Search
---

# Solr Basic Install

1. [Install Java SDK](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

2. [Java 安裝教學](https://bonze.tw/windows%E5%AE%89%E8%A3%9Djava%E5%8F%8A%E8%A8%AD%E5%AE%9A%E7%92%B0%E5%A2%83/)

3. [Download Solr](https://www.apache.org/dyn/closer.lua/lucene/solr/8.4.1/solr-8.4.1.zip)
![Architecture](1.png)

4. **Start solr**
![Architecture](2.png)
~~~ bash
$ solr.cmd start
~~~
![Architecture](3.png)

5. **Open browser**
![Architecture](4.png)

6. **Create New Collection**
~~~ bash
$ solr create -c collection1
~~~
![Architecture](5.png)
![Architecture](6.png)

7. **Stop solr**
~~~ bash
$ solr stop -all
~~~

# Reference
[【Apache Solr全文檢索】安裝和運行](http://mrblueman.pixnet.net/blog/post/98907396-%5Bapache-solr%E5%85%A8%E6%96%87%E6%AA%A2%E7%B4%A2%5D-%E5%AE%89%E8%A3%9D%E5%92%8C%E9%81%8B%E8%A1%8C)