---
title: Angular Structural Directives
date: 2019-07-16 15:48:31
tags:
 - Angular Basic
categories: 
 - Angular
---

# Structural Directives
1. **ngIf**
2. **nfSwitch**
3. **ngFor**

#### ngIf Directives
1. **test.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-test',
  template: `
    <!--Basic-->
    <h2 *ngIf="displayName"> HelloWorld Basic </h2>
    
    <!--If else-->
    <h2 *ngIf="displayName; else elseBlock"> HelloWorld If else </h2>
    <ng-template #elseBlock>
      <h2> Name is hidden </h2>
    </ng-template>

    <div *ngIf="displayName; then thenBlock; else eBlock"></div>
    <ng-template #thenBlock>
      <h2> True </h2>
    </ng-template>

    <ng-template #eBlock>
      <h2> Flase </h2>
    </ng-template>

    <button (click)="onChangeStatus()">Click</button>
  `,
  styles: []
})
export class TestComponent implements OnInit {

  displayName = true;

  constructor() { }

  ngOnInit() {
  }

  onChangeStatus() {
    this.displayName = !this.displayName;
  }
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### nfSwitch Directives
1. **test.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-test',
  template: `
    <label *ngFor="let c of colorList">
      <input type="radio" [(ngModel)]="color" [value]="c.id" /> {{ c.name }}
    </label>
 
    <div [ngSwitch]="color">
      <div *ngSwitchCase="'red'"> You picked red color </div>
      <div *ngSwitchCase="'blue'"> You picked blue color </div>
      <div *ngSwitchCase="'green'"> You picked green color </div>
      <div *ngSwitchDefault> Pick again </div>
    </div>
  `,
  styles: []
})
export class TestComponent implements OnInit {

  colorList:any = [
    { id: "red", name: "Red" },
    { id: "blue", name: "Blue" },
    { id: "green", name: "Green" },
    { id: "", name: "Empty" }
  ];
  public color = "";

  constructor() { }

  ngOnInit() {
  }

  setColor(value) {
    this.color = value;
  }
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

#### ngFor Directives
1. **test.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-test',
  template: `
    -----------Basic--------------
    <div *ngFor="let color of colors">
      <h2> {{ color }} </h2>
    </div>

    -----------index--------------
    <div *ngFor="let color of colors; index as i">
      <h2> {{ i }} {{ color }} </h2>
    </div>

    -----------first--------------
    <div *ngFor="let color of colors; first as f">
      <h2> {{ f }} {{ color }} </h2>
    </div>

    -----------last--------------
    <div *ngFor="let color of colors; last as l">
      <h2> {{ l }} {{ color }} </h2>
    </div>

    -----------odd--------------
    <div *ngFor="let color of colors; odd as o">
      <h2> {{ o }} {{ color }} </h2>
    </div>

    -----------even--------------
    <div *ngFor="let color of colors; even as e">
      <h2> {{ e }} {{ color }} </h2>
    </div>
  `,
  styles: []
})
export class TestComponent implements OnInit {

  public colors = ["red", "blue", "green", "yellow"];

  constructor() { }

  ngOnInit() {
  }
}
~~~

2. **app.component.html**
~~~ bash
<app-test></app-test>
~~~

# Reference
[Angular 8 Tutorial - 12 - ngIf Directive](https://www.youtube.com/watch?v=nWst87nQmZQ&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=12)
[Angular 8 Tutorial - 13 - ngSwitch Directive](https://www.youtube.com/watch?v=WiDn2y1Ktws&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=13)
[Angular 8 Tutorial - 14 - ngFor Directive](https://www.youtube.com/watch?v=Du3p6QYGs3A&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=14)