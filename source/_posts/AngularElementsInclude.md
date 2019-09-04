---
title: Angular Custom Elements Include
date: 2019-09-04 09:24:17
tags:
 - Angular Elements
categories: 
 - Angular
---

# Angular Custom Elements Include

## Basic
1. **Copy js and css file**
![Architecture](1.png)

2. **Create new folder and add index.html**
![Architecture](2.png)
~~~ bash
<!doctype html>
<html lang="en">
    <head>
        <title>Angular Elements Demo</title>
    </head>
    <body>
        <app-activeuser user="Joey Tribbiani"></app-activeuser>
        <script src="elements.js"></script>
    </body>
</html>
~~~

3. **Open from browser**
![Architecture](3.png)

## Include with Angular
1. **Create js file under assets**
![Architecture](4.png)

2. **index.html**
~~~ bash
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>IncludeCustomElements</title>
  <base href="/">

  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>
  <app-root></app-root>
  <app-activeuser user="Joey Tribbiani"></app-activeuser>
  <script src="./assets/js/elements.js"></script>
</body>
</html>
~~~

3. **Open from browser**
![Architecture](5.png)

## Include with Angular lazy
1. **Create js file under assets**
![Architecture](4.png)

2. **Download extensions elements**
`npm i @angular-extensions/elements`

3. **Create component**
`ng g c feature --spec false`

4. **feature.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-feature',
  template: `
  <app-activeuser user="Joey Tribbiani" *axLazyElement="elementUrl"></app-activeuser>
  `
})
export class FeatureComponent {
  elementUrl = 'http://localhost:4200/assets/js/elements.js';
}
~~~

5. **app.component.html**
~~~ bash
<app-feature></app-feature>
<router-outlet></router-outlet>
~~~

6. **app.module.ts**
~~~ bash
import { BrowserModule } from '@angular/platform-browser';
import { NgModule, CUSTOM_ELEMENTS_SCHEMA  } from '@angular/core';
import { LazyElementsModule } from '@angular-extensions/elements';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { FeatureComponent } from './feature/feature.component';

@NgModule({
  schemas: [CUSTOM_ELEMENTS_SCHEMA],
  declarations: [
    AppComponent,
    FeatureComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    LazyElementsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
~~~

7. **Open from browser**
![Architecture](6.png)

# Reference
[@angular-extensions/elements](https://angular-extensions.github.io/elements/#/docs/getting-started)