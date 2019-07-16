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

## Two-wan Binding [()]
#### Two-wan Binding 1
1. app.component.ts
~~~ bash
export class AppComponent {
  userName: string;
}
~~~

2. app.component.html
~~~ bash
<div>
  <input type="text" [ngModel]="userName" (ngModelChange)="userName=$event">
  <p>Welcome {{ userName }}</p>
</div>
~~~

#### Two-wan Binding 2
1. app.component.ts
~~~ bash
export class AppComponent {
  nickName: string;

  greetNickName(updatedValue) {
    this.nickName = updatedValue;

    if(updatedValue == 'Nickname'){
      alert('Welcome back Nickname');
    }
  }
}
~~~

2. app.component.html
~~~ bash
<div>
  <input type="text" [ngModel]="nickName" (ngModelChange)="greetNickName($event)">
  <p>Welcome {{ nickName }}</p>
</div>
~~~

#### Two-wan Binding 3
1. app.component.ts
~~~ bash
export class AppComponent {
  name: string;
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