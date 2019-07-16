---
title: Angular Template Reference
date: 2019-07-16 15:18:53
tags:
 - Angular Basic
categories: 
 - Angular
---

# Template Reference

#### Basic
1. **test.component.ts**
~~~ bash
@Component({
  selector: 'app-test',
  template: `
    <h2>
      Welcome {{ name }}
    </h2>

    <input #myInput type="text" />

    <button (click)="logMessage(myInput)">Log Html</button>

    <button (click)="logMessageValue(myInput.value)">Log Value</button>
  `,
  styles: []
})
export class TestComponent implements OnInit {

  public name = "Hello World";
  public greeting = "";
  public greetingEvent = "";

  constructor() { }

  ngOnInit() {
  }

  logMessage(value) {
    console.log(value);
  }

  logMessageValue(value) {
    console.log(value);
  }
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
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
<app-child #child [loggedIn]="userLoggedIn"></app-child>
{{ child.name }}
<button (click)="child.greetVishwas()">Greet</button>
~~~

3. **child.component.ts**
~~~ bash
import { Component, Input, OnChanges, SimpleChanges } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent implements OnChanges {

  @Input() loggedIn: boolean;
  message: string;
  name = "Vishwas";

  constructor() { }

  ngOnChanges(changes: SimpleChanges) {
    console.log(changes);
    const loggedInValue = changes['loggedIn'];
    if (loggedInValue.currentValue == true) {
      this.message = "Welcome back Vishwas!";
    } else {
      this.message = "Please log in";
    }
  }

  greetVishwas() {
    alert("Hey " + this.name);
  }

}
~~~

4. **child.component.html**
~~~ bash
<h2>Child Component</h2>
{{ message }}
~~~

# Reference
[Angular 8 Tutorial - 10 - Template Reference Variables](https://www.youtube.com/watch?v=Oo0-r_YhoJs&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=10)