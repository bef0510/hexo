---
title: Angular Input Ouput Decorator
date: 2019-07-16 17:27:19
tags:
 - Angular Basic
categories: 
 - Angular
---

# Input Ouput Decorator
![Architecture](1.png)

#### Input Decorator
1. **app.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = "app";
  public name = "Hello World";
}
~~~

2. **app.component.html**
~~~ bash
<div style="text-align:center">
  <h1>
    Welcome to {{ title }}!
  </h1>
</div>
<app-test [parentData]="name" [alias]="name"></app-test>
~~~

3. **test.component.ts**
~~~ bash
import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css']
})
export class TestComponent implements OnInit {

  @Input() public parentData;
  @Input('alias') public name;
  
  constructor() { }

  ngOnInit() {
  }
}
~~~

4. **test.component.html**
~~~ bash
<h2> {{ "Hello " + parentData }} </h2>
<h2> {{ "Hello " + name }} </h2>
~~~

#### Ouput Decorator
1. **app.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = "app";
  
  greet() {
    alert("Hello World");
  }

  greetParam(name: string) {
    alert("Hello " + name);
  }
}
~~~

2. **app.component.html**
~~~ bash
<h1>Parent Component</h1>
<app-child (greetEvent)="greet()"></app-child>
<app-child (greetEventParam)="greetParam($event)"></app-child>
~~~

3. **child.component.ts**
~~~ bash
import { Component, OnInit, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent implements OnInit {

  @Output() greetEvent = new EventEmitter();
  @Output() greetEventParam = new EventEmitter();
  name= "Codevolution";
  constructor() { }

  ngOnInit() {

  }

  callParentGreet() {
    this.greetEvent.emit();
    this.greetEventParam.emit(this.name);
  }
}
~~~

4. **child.component.html**
~~~ bash
<h2>Child Component</h2>
<button (click)="callParentGreet()">Greet</button>
~~~

# Reference
[Angular Component Interaction - 13 - Output Decorator](https://www.youtube.com/watch?v=QQ4gYIeZQrs&list=PLC3y8-rFHvwgKhaLU8GTyF-5Bb8qT-wzV&index=13)