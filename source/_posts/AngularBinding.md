---
title: Angular Binding
date: 2019-07-15 17:52:53
tags:
 - Angular Basic
categories: 
 - Angular
---

# Binding
1. Interpolation
2. Property Binding
3. Event Binding
4. Two-wan Binding
![Architecture](1.png)

#### Interpolation { { } }
1. app.component.ts
~~~ bash
export class AppComponent {
  pageTitle = 'helloworld';
}
~~~
2. app.component.html
~~~ bash
<h1>{{ pageTitle }}</h1>
~~~

#### Property Binding []
1. app.component.ts
~~~ bash
export class AppComponent {
  pageTitle = 'helloworld';
  imgUrl = 'https://picsum.photos/200';
}
~~~
2. app.component.html
~~~ bash
<div>
  <img [src]="imgUrl" alt="Image" />
</div>
~~~

#### Event Binding ()
1. app.component.ts
~~~ bash
export class AppComponent {
  pageTitle = 'helloworld';
  imgUrl = 'https://picsum.photos/200';
  count = 0;

  incrementCount() {
    this.count += 1;
  }
}
~~~
2. app.component.html
~~~ bash
<div>
  <button (click)="incrementCount()">Click</button>
  <p>Clicked {{ count }} times</p>
</div>
~~~

#### Two-wan Binding [()]
1. app.component.ts
~~~ bash
export class AppComponent {
  pageTitle = 'helloworld';
  imgUrl = 'https://picsum.photos/200';
  count = 0;
  name: string;

  incrementCount() {
    this.count += 1;
  }
}
~~~
2. app.component.html
~~~ bash
<div>
  <input type="text" [(ngModel)]="name">
  <p>Welcome {{ name }}</p>
</div>
~~~

# Reference
[Angular Component Interaction - 3 - Binding](https://www.youtube.com/watch?v=rH-xfNvpm_o&list=PLC3y8-rFHvwgKhaLU8GTyF-5Bb8qT-wzV&index=3)