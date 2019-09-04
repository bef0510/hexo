---
title: Angular Build Express Node Server
date: 2019-07-18 16:08:48
tags:
 - Angular Basic
 - Express Server
categories: 
 - Angular
---

# Build Express Server
1. **Create Folder**

2. **Init**
`npm init –yes`
![Architecture](1.png)

3. **Install**
`npm install --save express body-parser cors`
![Architecture](2.png)

4. **Create server.js**
![Architecture](3.png)
~~~ bash
const express = require("express");
const bodyParser = require("body-parser");
const cors = require("cors");

const PORT = 3000;

const app = express();

app.use(bodyParser.json());

app.use(cors());

app.get("/", function(req, res) {
    res.send("Hello from server");
});

app.post("/enroll", function(req, res) {
    console.log(req.body);
    res.status(200).send({"message": "Data received"});
});

app.listen(PORT, function() {
    console.log("Server running on localhost:" + PORT);
});
~~~

5. **Run Server**
`node server`
![Architecture](4.png)

6. **Input url at browser**
![Architecture](5.png)

7. **Using postman**
**postman**
![Architecture](6.png)
**Express server terminal**
![Architecture](7.png)

# Reference
[Angular Forms Tutorial - 13 - Express Server to Receive Form Data](https://www.youtube.com/watch?v=jwA-9XXybdM&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=45)