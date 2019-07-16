---
title: Angular Getter and Setter
date: 2019-07-16 09:54:59
tags:
 - Angular Basic
categories: 
 - Angular
---

# Getter and Setter
1. app.component.ts
~~~ bash
export class AppComponent {
  private _customerName: string;

  get customerName(): string {
    return this._customerName;
  }

  set customerName(value:string) {
    this._customerName = value;

    if(value == 'CustomerName'){
      alert('Hello CustomerName');
    }
  }
}
~~~

2. app.component.html
~~~ bash
<div>
  <input type="text" [(ngModel)]="customerName">
  <p>Welcome {{ customerName }}</p>
</div>
~~~

# Reference
[Angular Component Interaction - 5 - Getters and Setters](https://www.youtube.com/watch?v=132VfEETYvs&list=PLC3y8-rFHvwgKhaLU8GTyF-5Bb8qT-wzV&index=5)