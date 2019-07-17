---
title: Angular Service
date: 2019-07-17 17:19:00
tags:
 - Angular Basic
categories: 
 - Angular
---

# Service
1. **Share data**
2. **Implement application logic**
3. **External Interaction**
![Architecture](1.png)
4. **Create a new service command**
    `ng generate service user`
    `ng generate service user --spec false`
    `ng g s user --spec false`

#### Service
1. **app.module.ts**
~~~ bash
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { EmployeeListComponent } from './employee-list/employee-list.component';
import { EmployeeDetailComponent } from './employee-detail/employee-detail.component';
import { EmployeeService } from './employee.service';

@NgModule({
  declarations: [
    AppComponent,
    EmployeeListComponent,
    EmployeeDetailComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule
  ],
  providers: [EmployeeService],
  bootstrap: [AppComponent]
})
export class AppModule { }
~~~

2. **employee.service.ts**
~~~ bash
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class EmployeeService {

  constructor() { }

  getEmployees() {
    return [
      { "id" : 1, "name" : "Andrew", "age" : 30 },
      { "id" : 2, "name" : "Brandon", "age" : 25 },
      { "id" : 3, "name" : "Christina", "age" : 26 },
      { "id" : 4, "name" : "Elena", "age" : 28 }
    ];
  }
}
~~~

3. **app.component.html**
~~~ bash
<div style="text-align:center">
    <h1>
      Welcome to {{ title }}!
    </h1>
  </div>
<app-employee-list></app-employee-list>
<app-employee-detail></app-employee-detail>
~~~

4. **employee-list.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';
import { EmployeeService } from '../employee.service';

@Component({
  selector: 'app-employee-list',
  template: `
    <h2>Employee List</h2>
    <ul *ngFor="let employee of employees">
      <li> {{ employee.name }} </li>
    </ul>
  `,
  styleUrls: []
})
export class EmployeeListComponent implements OnInit {

  public employees = [];

  constructor(private employeeService: EmployeeService) { }

  ngOnInit() {
    this.employees = this.employeeService.getEmployees();
  }
}
~~~

5. **employee-detail.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';
import { EmployeeService } from '../employee.service';

@Component({
  selector: 'app-employee-detail',
  template: `
    <h2>Employee Detail</h2>
    <ul *ngFor="let employee of employees">
      <li> {{ employee.id }}. {{ employee.name }} - {{ employee.age }} </li>
    </ul>
  `,
  styleUrls: []
})
export class EmployeeDetailComponent implements OnInit {

  public employees = [];

  constructor(private employeeService: EmployeeService) { }

  ngOnInit() {
    this.employees = this.employeeService.getEmployees();
  }
}
~~~

# Reference
[Angular 8 Tutorial - 19 - Using a Service](https://www.youtube.com/watch?v=69VeYoKzL6I&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=19)