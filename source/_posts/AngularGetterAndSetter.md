---
title: Angular Getter and Setter
date: 2019-07-16 09:54:59
tags:
 - Angular Basic
categories: 
 - Angular
---

# Getter and Setter

#### Basic
1. **app.component.ts**
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

2. **app.component.html**
~~~ bash
<div>
  <input type="text" [(ngModel)]="customerName">
  <p>Welcome {{ customerName }}</p>
</div>
~~~

#### @Input
1. **app.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  userLoggedIn = true;
}
~~~

2. **app.component.html**
~~~ bash
<h1>Parent Component</h1>
<div>
    <button (click)="userLoggedIn=true">Login</button>
    <button (click)="userLoggedIn=false">Logout</button>
    {{ userLoggedIn }}
</div>
<app-child [loggedIn]="userLoggedIn"></app-child>
~~~

3. **child.component.ts**
~~~ bash
import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent implements OnInit {

  private _loggedIn: boolean;
  message: string;

  get loggIn(): boolean {
    return this._loggedIn;
  }

  @Input()
  set loggedIn(value: boolean) {
    this._loggedIn = value;
    if(value == true){
      this.message = "Welcome back Vishwas";
    } else{
      this.message = "Please log in";
    }
  }

  constructor() { }

  ngOnInit() {
  }

}
~~~

4. **child.component.html**
~~~ bash
<h2>Child Component</h2>
{{ message }}
~~~

# Reference
[Angular Component Interaction - 5 - Getters and Setters](https://www.youtube.com/watch?v=132VfEETYvs&list=PLC3y8-rFHvwgKhaLU8GTyF-5Bb8qT-wzV&index=5)