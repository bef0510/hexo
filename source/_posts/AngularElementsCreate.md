---
title: Angular Custom Elements Add
date: 2019-09-04 08:40:12
tags:
 - Angular Elements
categories: 
 - Angular
---

# Angular Custom Elements Add

1. **Create a new project**
`ng new createCustomElements`

2. **Create component**
`ng g c activeUsers --spec false`

3. **active-users.component.html**
~~~ bash
<h1>{{title}}</h1>
 <h2>You are welcome, {{user}}!</h2>
 <h4>Active Users - </h4>
 <div>
    <table *ngFor="let user of activeUsers; let i = index;" width="40%">
      <tr><td width="5%">#{{i +1}}</td><td width="30%">{{user.name}}</td></tr>
    </table>
 </div>
~~~

4. **active-users.component.ts**
~~~ bash
import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'app-active-users',
  templateUrl: './active-users.component.html',
  styleUrls: ['./active-users.component.css']
})
export class ActiveUsersComponent implements OnInit {

  public activeUsers :any;
  public title :any;
  @Input() user: string;

  constructor() {
    this.title ='Angular - Create Custom Elements';
   }

  ngOnInit() {
    this.activeUsers = [
      {"name" :"Anil Singh"},
      {"name" :"Alok Singh"},
      {"name" :"Dilip Singh"},
      {"name" :"Sunil Singh"},
      {"name" :"Aradhya Singh"},
      {"name" :"Reena Singh"}
];
  }
}
~~~

5. **Download angular elements**
`ng add @angular/elements`

6. **Delete app.component files**
![Architecture](1.png)
![Architecture](2.png)

7. **app.module.ts**
~~~ bash
import { BrowserModule } from '@angular/platform-browser';
import { NgModule, Injector } from '@angular/core';
import { createCustomElement } from '@angular/elements';

import { ActiveUsersComponent } from './active-users/active-users.component';

@NgModule({
  declarations: [
    ActiveUsersComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  entryComponents: [
    ActiveUsersComponent
  ]
})
export class AppModule {
  constructor(private injector: Injector) { }

  ngDoBootstrap() {
    const el = createCustomElement(ActiveUsersComponent, { injector: this.injector });
    customElements.define('app-activeuser', el);
  }
}
~~~

8. **index.html**
~~~ bash
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>CreateCustomElements</title>
  <base href="/">

  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>
  <app-activeuser user="Joey Tribbiani"></app-activeuser>
</body>
</html>
~~~

9. **Add file build-elements.js**
~~~ bash
const fs = require('fs-extra');
const concat = require('concat');
(async function build() {
  const files = [
    './dist/createCustomElements/runtime-es2015.js',
    './dist/createCustomElements/polyfills-es2015.js',
    './dist/createCustomElements/scripts.js',
    './dist/createCustomElements/main-es2015.js',
  ]
  await fs.ensureDir('elements')
  await concat(files, 'elements/elements.js');
  await fs.copyFile('./dist/createCustomElements/styles.css', 'elements/styles.css')
  await fs.copy('./dist/createCustomElements/assets/', 'elements/assets/' )
})()
~~~

10. **package.json**
~~~ bash
***
"scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e",
    "build:elements": "ng build --prod --output-hashing none && node build-elements.js"
  },
***
~~~

11. **Download fs-extra and concat**
`npm install fs-extra concat --save-dev`

12. **build element**
`npm run build:elements`

13. **Generate file**
![Architecture](3.png)

# Reference
[Angular 7|8 Elements Tutorial by Example: Building Web Components](https://www.techiediaries.com/angular-elements-web-components/)
[Create Custom Element Angular 6 - Step by Step with Example](https://www.code-sample.com/2018/07/create-custom-element-angular-6.html)