---
title: Angular Interpolation 使用 template
date: 2019-07-16 10:17:02
tags:
 - Angular Basic
categories: 
 - Angular
---

# Interpolation 使用 template
1. **template** 下無法直接使用 **javascript**，要透過 **ts** 給變數
2. 無法使用 `a = 2 + 2 運算式`，只能顯示

#### Interpolation
1. **test.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-test',
  template: `
    <h2>
      Welcome {{ name }}
    </h2>
    <h2> {{ 2 + 2 }} </h2>
    <h2> {{ "Welcome " + name }} </h2>
    <h2> {{ name.length }} </h2>
    <h2> {{ name.toUpperCase() }} </h2>
    <h2> {{ greetUser() }} </h2>
    <h2> {{ siteUrl }} </h2>
  `,
  styleUrls: []
})
export class TestComponent implements OnInit {

  public name = "Hello World";
  public siteUrl = window.location.href;

  constructor() { }

  ngOnInit() {
  }

  greetUser() {
    return "Hi! " + this.name;
  }
}
~~~

2. **app.component.html**
~~~ bash
<div style="text-align:center">
  <h1>
    From AppComponent
  </h1>
</div>
<app-test></app-test>
~~~

# Reference
[Angular 8 Tutorial - 5 - Interpolation](https://www.youtube.com/watch?v=2a6OfacW_-I&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=5)