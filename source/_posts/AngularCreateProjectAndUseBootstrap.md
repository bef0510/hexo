---
title: Angular 建置，使用 Bootstrap
date: 2019-04-30 15:03:57
tags:
 - Angular Basic
 - Bootstrap
 - VS Core
categories: 
 - Angular
---

# 準備環境
[Node.js](https://nodejs.org/en/) 下載並安裝
[Visual Studio Code](https://code.visualstudio.com/docs/?dv=win) 下載並安裝
~~~ bash
$ npm install -g @angular/cli
~~~

### 建立專案
用 **VS Code** 開啟檔案，預設 **Port** `http://localhost:4200/`
~~~ bash
$ ng new my-angular
$ cd my-angular
$ ng serve -o
~~~

### Bootstrap & jQuery
透過 **npm** 載入 **Bootstrap** 及 **jQuery**
~~~ bash
$ npm install bootstrap@3 jquery --save
~~~

`angular.json` 加入 **styles** 及 **scripts** 設定

    "styles": [
        "src/styles.css",
        "node_modules/bootstrap/dist/css/bootstrap.min.css"
    ],
    "scripts": [
        "node_modules/jquery/dist/jquery.min.js",
        "node_modules/bootstrap/dist/js/bootstrap.min.js"
    ],

啟動 **Server**，確認 **Bootstrap** 有引用
~~~ bash
$ ng s
~~~
![ImportBootstrap](1.png)

`app.component.html` 加入 **Bootstrap button**

    <li>
        <button class="btn btn-primary">Bootstrap Styled Button</button>
    </li>
~~~ bash
$ ng s
~~~
![BootstrapButton](2.png)

# Reference
[Angular CLI](https://cli.angular.io/)