---
title: Angular Template Driven Form Validation
date: 2019-07-19 09:17:09
tags:
 - Angular Basic
 - Angular Form
categories: 
 - Angular
---

# Template Driven Form Validation
1. **[Create Express Server](/2019/07/18/AngularBuildExpressServer/)**
2. **index.html import bootstrap**
3. **app.modules.ts import FormsModule**
4. **app.modules.ts import HttpClientModule**

#### Form Validate and submit
1. **form** 標籤下的 **novalidate** 屬性，是為了要防止瀏覽器自己執行 **native** 的驗證
~~~ bash
<form #userForm="ngForm" (ngSubmit)="onSubmit(userForm)" novalidate>
</form>
~~~

#### Text Validate
1. **Html**
~~~ bash
<input type="text" required #name="ngModel" [class.is-invalid]="name.invalid && name.touched" class="form-control" name="userName" [(ngModel)]="userModel.name" />
<!--Error Messages-->
<small class="text-danger" [class.d-none]="name.valid || name.untouched"> Name is required </small>

<input type="tel" required #phone="ngModel" pattern="^\d{10}$" [class.is-invalid]="phone.invalid && phone.touched" class="form-control" name="phone" [(ngModel)]="userModel.phone" />
<!--Error Messages-->
<!--<small class="text-danger" [class.d-none]="phone.valid || phone.untouched"> Phone number is required and must be 10 digits </small>-->
<div *ngIf="phone.errors && (phone.invalid || phone.touched)">
    <small class="text-danger" *ngIf="phone.errors.required"> Phone number is required </small>
    <small class="text-danger" *ngIf="phone.errors.pattern"> Phone number must be 10 digits </small>
</div>
~~~

#### Select Option Validate
1. **Html**
~~~ bash
<select (blur)="validateTopic(topic.value)" (change)="validateTopic(topic.value)" required #topic="ngModel" [class.is-invalid]="topicHasError && topic.touched" class="custom-select" name="topic" [(ngModel)]="userModel.topic">
    <option value="default">I am interested in</option>
    <option *ngFor="let topic of topics"> {{ topic }} </option>
</select>
<!--Error Messages-->
<small class="text-danger" [class.d-none]="!topicHasError || topic.untouched"> Please choose a topic </small>
~~~

2. **.ts**
~~~ bash
export class AppComponent {
  topics = ["Angular", "React", "Vue"];
  topicHasError = true;

  validateTopic(value) {
    this.topicHasError = false;

    if (value === "default") {
      this.topicHasError = true;
    }
  }
}
~~~

#### Button Submit
1. **Html**
**Need to be combined with the above verification**
~~~ bash
<button [disabled]="userForm.form.invalid || topicHasError" class="btn btn-primary" type="submit"> Submit form </button>
~~~

# All Html
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
import { HttpClientModule } from '@angular/common/http';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
~~~

3. **user.ts**
**Create model**
`ng g class user --spec false`
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

4. **enrollment.service.ts**
**Create service**
`ng g s enrollment --spec false`
~~~ bash
import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { User } from './user';
import { catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class EnrollmentService {

  url = "http://localhost:3000/enroll";

  constructor(private http: HttpClient) { }

  enroll(user: User) {
    return this.http.post<any>(this.url, user)
            .pipe(catchError(this.errorHandler));
  }

  errorHandler(error: HttpErrorResponse) {
    return throwError(error);
  }
}
~~~

5. **app.component.ts**
~~~ bash
import { Component } from '@angular/core';
import { User } from './user';
import { EnrollmentService } from './enrollment.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  topics = ["Angular", "React", "Vue"];
  topicHasError = true;
  submitted = false;
  errorMsg = "";

  userModel = new User("", "rob@test.com", 5556665566, "default", "morning", true);

  constructor(private enrollmentService: EnrollmentService) {}

  validateTopic(value) {
    this.topicHasError = false;

    if (value === "default") {
      this.topicHasError = true;
    }
  }

  onSubmit(userForm) {
    console.log(userForm)
    this.submitted = true;
    this.enrollmentService.enroll(this.userModel)
      .subscribe(
        data => console.log("Success!", data),
        error => this.errorMsg = error.statusText
      );
  }
}
~~~

6. **app.component.html**
~~~ bash
<div class="container-fluid">
  <h1> Bootcamp Enrollment Form </h1>
  <div class="alert alert-danger" *ngIf="errorMsg">
    {{ errorMsg }}
  </div>
  
  <form #userForm="ngForm" *ngIf="!submitted" (ngSubmit)="onSubmit(userForm)" novalidate>
  
    <div class="form-group">
      <label> Name </label>
      <input type="text" required #name="ngModel"
        [class.is-invalid]="name.invalid && name.touched" class="form-control" 
        name="userName" [(ngModel)]="userModel.name" />
      <!--Error Messages-->
      <small class="text-danger" [class.d-none]="name.valid || name.untouched"> Name is required </small>
    </div>
   
    <div class="form-group">
      <label> Email </label>
      <input type="email" class="form-control" name="email" [(ngModel)]="userModel.email" />
    </div>
    
    <div class="form-group">
      <label> Phone </label>
      <input type="tel" required #phone="ngModel" pattern="^\d{10}$"
        [class.is-invalid]="phone.invalid && phone.touched" class="form-control" name="phone" [(ngModel)]="userModel.phone" />
      <!--Error Messages-->
      <!--<small class="text-danger" [class.d-none]="phone.valid || phone.untouched"> Phone number is required and must be 10 digits </small>-->
      <div *ngIf="phone.errors && (phone.invalid || phone.touched)">
        <small class="text-danger" *ngIf="phone.errors.required"> Phone number is required </small>
        <small class="text-danger" *ngIf="phone.errors.pattern"> Phone number must be 10 digits </small>
      </div>
    </div>
    
    <div class="form-group">
      <select (blur)="validateTopic(topic.value)" (change)="validateTopic(topic.value)" required #topic="ngModel" 
        [class.is-invalid]="topicHasError && topic.touched" class="custom-select" name="topic" [(ngModel)]="userModel.topic">
        <option value="default">I am interested in</option>
        <option *ngFor="let topic of topics"> {{ topic }} </option>
      </select>
      <!--Error Messages-->
      <small class="text-danger" [class.d-none]="!topicHasError || topic.untouched"> Please choose a topic </small>
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
    
    <button [disabled]="userForm.form.invalid || topicHasError" class="btn btn-primary" type="submit"> Submit form </button>
    
  </form>
</div>
~~~

# Reference
[Angular Forms Tutorial - 14 - Error Handling](https://www.youtube.com/watch?v=_sJHvvgfE9o&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=46)