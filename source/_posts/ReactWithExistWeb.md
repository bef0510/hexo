---
title: React 加入現有網站
date: 2019-08-19 10:44:33
tags:
 - React
categories: 
 - .React
---

# React 加入現有網站

#### VS Code Extension
1. 加入 **Live Server**
![Architecture](01.png)

2. 加入 **ES7 React/Redux/GraphQL/React-Native snippetsr**
![Architecture](02.png)

#### Create React
1. 建立資料庫
![Architecture](03.png)

2. 開啟 **VS Code**
`code .`
![Architecture](04.png)

3. 新增 **index.html**
![Architecture](05.png)

4. 增加基本 **Html** 資訊
`Key ! 後加 Tab`
![Architecture](06.png)
![Architecture](07.png)

5. 加入 **Root DOM**
~~~ bash
<div id="app"></div>
~~~

6. 加入 **React CDN**
~~~ bash
<script crossorigin src="https://unpkg.com/react@17/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
~~~

7. 加入 **Babel CDN**
~~~ bash
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
~~~

8. **Babel js** 加入函式
~~~ bash
<script type="text/babel">
        // functional component
        const App = () => {
            return (
                <div>
                    <p>App</p>
                </div>
            )
        }

        ReactDOM.render(
            <App/>,
            document.querySelector('#app')
        )
    </script>
~~~

9. 完整程式
~~~ bash
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>First React</title>
</head>
<body>
    <div id="app"></div>

    <script crossorigin src="https://unpkg.com/react@17/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>

    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <script type="text/babel">
        // functional component
        const App = () => {
            return (
                <div>
                    <p>App</p>
                </div>
            )
        }

        ReactDOM.render(
            <App/>,
            document.querySelector('#app')
        )
    </script>
</body>
</html>
~~~

10. 運行程式
`點擊 Go Live`
![Architecture](08.png)

# Reference
[ReactJS入門教學課程04-React Component (元件/零件)](https://www.youtube.com/watch?v=8_o0EZKFdos)