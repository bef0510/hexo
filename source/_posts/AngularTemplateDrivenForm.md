---
title: Angular Template Driven Form (TDF)
date: 2019-07-18 17:09:32
tags:
 - Angular Basic
 - Angular Form
categories: 
 - Angular
---

# Template Driven Form
1. **ngForm**
2. **ngModel**
3. **ngModelGroup**
4. **State**
![Architecture](1.png)
5. **ngModel properties**
![Architecture](2.png)
6. **index.html import bootstrap**
7. **app.modules.ts import FormsModule**

#### Basic
1. **index.html**
~~~ bash
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>TemplateDrivenform</title>
  <base href="/">

  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
</head>
<body>
  <app-root></app-root>
</body>
</html>
~~~

2. **app.module.ts**
~~~ bash
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
~~~

3. **app.component.ts**
~~~ bash
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  topics = ["Angular", "React", "Vue"];
}
~~~

4. **app.component.html**
~~~ bash
<div class="container-fluid">
  <h1> Bootcamp Enrollment Form </h1>
  <form #userForm="ngForm">

    {{ userForm.value | json }}
    
    <div ngModelGroup="address">
      <div class="form-group">
        <label> Street </label>
        <input type="text" class="form-control" name="street" ngModel />
      </div>

      <div class="form-group">
        <label> City </label>
        <input type="text" class="form-control" name="city" ngModel />
      </div>

      <div class="form-group">
        <label> State </label>
        <input type="text" class="form-control" name="state" ngModel />
      </div>

      <div class="form-group">
        <label> Postal Code </label>
        <input type="text" class="form-control" name="postalCode" ngModel />
      </div>
    </div>
    
    <div class="form-group">
      <label> Name </label>
      <input type="text" class="form-control" name="userName" ngModel />
    </div>

    <div class="form-group">
      <label> Email </label>
      <input type="email" class="form-control" name="email" ngModel />
    </div>

    <div class="form-group">
      <label> Phone </label>
      <input type="tel" class="form-control" name="phone" ngModel />
    </div>

    <div class="form-group">
      <select class="custom-select" name="topic" ngModel>
        <option value="">I am interested in</option>
        <option *ngFor="let topic of topics"> {{ topic }} </option>
      </select>
    </div>

    <div class="mb-3">
      <label> Time preference </label>
      <div class="form-check">
        <input type="radio" class="form-check-input" name="timePreference" value="morning" ngModel>
        <label class="form-check-label"> Morning (9AM - 12PM) </label>
      </div>

      <div class="form-check">
        <input type="radio" class="form-check-input" name="timePreference" value="evening" ngModel>
        <label class="form-check-label"> Evening (5PM - 8PM) </label>
      </div>
    </div>

    <div class="mb-3">
      <div class="form-check">
        <input type="checkbox" class="form-check-input" name="subscribe" ngModel>
        <label class="form-check-label"> Send me promotional offers </label>
      </div>
    </div>

  </form>
</div>
~~~

#### One way binding
1. **Create model**
`ng g class user --spec false`

2. **app.component.ts**
~~~ bash
import { Component } from '@angular/core';
import { User } from './user';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  topics = ["Angular", "React", "Vue"];

  userModel = new User("", "rob@test.com", 5556665566, "default", "morning", true);
}
~~~

3. **app.component.html**
~~~ bash
<div class="container-fluid">
  <h1> Bootcamp Enrollment Form </h1>
  <form #userForm="ngForm">
  
    {{ userForm.value | json }}
    <hr />
    {{ userModel | json }}
    
    <div class="form-group">
      <label> Name </label>
      <input type="text" class="form-control" name="userName" [ngModel]="userModel.name" />
    </div>

    <div class="form-group">
      <label> Email </label>
      <input type="email" class="form-control" name="email" [ngModel]="userModel.email" />
    </div>

    <div class="form-group">
      <label> Phone </label>
      <input type="tel" class="form-control" name="phone" [ngModel]="userModel.phone" />
    </div>

    <div class="form-group">
      <select class="custom-select" name="topic" [ngModel]="userModel.topic">
        <option value="">I am interested in</option>
        <option *ngFor="let topic of topics"> {{ topic }} </option>
      </select>
    </div>

    <div class="mb-3">
      <label> Time preference </label>
      <div class="form-check">
        <input type="radio" class="form-check-input" name="timePreference" value="morning" [ngModel]="userModel.timePreference">
        <label class="form-check-label"> Morning (9AM - 12PM) </label>
      </div>
      <div class="form-check">
        <input type="radio" class="form-check-input" name="timePreference" value="evening" [ngModel]="userModel.timePreference">
        <label class="form-check-label"> Evening (5PM - 8PM) </label>
      </div>
    </div>

    <div class="mb-3">
      <div class="form-check">
        <input type="checkbox" class="form-check-input" name="subscribe" [ngModel]="userModel.subscribe">
        <label class="form-check-label"> Send me promotional offers </label>
      </div>
    </div>

  </form>
</div>
~~~

4. **user.ts**
~~~ bash
export class User {
    constructor (
        public name: string,
        public email: string,
        public phone: number,
        public topic: string,
        public timePreference: string,
        public subscribe: boolean
    ) {}
}
~~~

#### Two way binding
1. **app.component.ts**
~~~ bash
import { Component } from '@angular/core';
import { User } from './user';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  topics = ["Angular", "React", "Vue"];

  userModel = new User("", "rob@test.com", 5556665566, "default", "morning", true);
}
~~~

2. **app.component.html**
~~~ bash
<div class="container-fluid">
  <h1> Bootcamp Enrollment Form </h1>
  <form #userForm="ngForm">

    {{ userForm.value | json }}
    <hr />
    {{ userModel | json }}

    <div class="form-group">
      <label> Name </label>
      <input type="text" class="form-control" name="userName" [(ngModel)]="userModel.name" />
    </div>
     
    <div class="form-group">
      <label> Email </label>
      <input type="email" class="form-control" name="email" [(ngModel)]="userModel.email" />
    </div>
      
    <div class="form-group">
      <label> Phone </label>
      <input type="tel" class="form-control" name="phone" [(ngModel)]="userModel.phone" />
    </div>
      
    <div class="form-group">
      <select class="custom-select" name="topic" [(ngModel)]="userModel.topic">
        <option value="default">I am interested in</option>
        <option *ngFor="let topic of topics"> {{ topic }} </option>
      </select>
    </div>
      
    <div class="mb-3">
      <label> Time preference </label>
      <div class="form-check">
        <input type="radio" class="form-check-input" name="timePreference" value="morning" [(ngModel)]="userModel.timePreference">
        <label class="form-check-label"> Morning (9AM - 12PM) </label>
      </div>
      <div class="form-check">
        <input type="radio" class="form-check-input" name="timePreference" value="evening" [(ngModel)]="userModel.timePreference">
        <label class="form-check-label"> Evening (5PM - 8PM) </label>
      </div>
    </div>
      
    <div class="mb-3">
      <div class="form-check">
        <input type="checkbox" class="form-check-input" name="subscribe" [(ngModel)]="userModel.subscribe">
        <label class="form-check-label"> Send me promotional offers </label>
      </div>
    </div>
  
  </form>
</div>
~~~

# Reference
[Angular Forms Tutorial - 7 - Tracking state and validity](https://www.youtube.com/watch?v=WyWJwR0FJV0&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=39)