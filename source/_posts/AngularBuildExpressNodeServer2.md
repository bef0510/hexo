---
title: AngularBuildExpressNodeServer2
date: 2019-11-07 09:15:17
tags:
 - Angular Basic
 - Express Server
categories: 
 - Angular
---

# Build Express Server
1. **Create Folder**
![Architecture](1.png)

2. **Install connect**
~~~ bash
npm install connect
~~~

3. **Install express**
~~~ bash
npm install express@3.0.0
~~~

4. **Add File app.js**
~~~ bash
var express = require("express"),
app = express.createServer(express.logger());

app.get("/", function(req, res) {
    res.sendfile(__dirname + '/index.html', function(err) {
        if (err) res.send(404);
    });
});

app.get(/(.*)\.(jpg|gif|png|ico|css|js|txt)/i, function(req, res) {
    res.sendfile(__dirname + "/" + req.params[0] + "." + req.params[1], function(err) {
        if (err) res.send(404);
    });
});

port = process.env.PORT || 3000;
app.listen(port, function() {
    console.log("Listening on " + port);
});
~~~

5. **Add File index.html**
~~~ bash
<h1>Hello NodeJS</h1>
~~~

6. **Run Server**
~~~ bash
node app.js
~~~

# Reference
[[NodeJS] 第一次 node 就上手](https://blog.hinablue.me/nodejs-first-look/)