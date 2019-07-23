---
title: Angular Reactive Forms Dynamic Add FormControls
date: 2019-07-23 11:43:01
tags:
 - Angular Basic
 - Angular Form
categories: 
 - Angular
---

# Reactive Forms Dynamic Add FormControls
1. **index.html import bootstrap**
2. **app.modules.ts import ReactiveFormsModule**

#### All Code
1. **index.html**
~~~ bash
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>ReactiveForm</title>
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
import { ReactiveFormsModule } from '@angular/forms';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { EmailComponent } from './email/email.component';

@NgModule({
  declarations: [
    AppComponent,
    EmailComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    ReactiveFormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
~~~

3. **app.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, FormArray } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  registrationForm: FormGroup;

  get email() {
    return this.registrationForm.get("email");
  }

  get alternateEmails() {
    return this.registrationForm.get("alternateEmails") as FormArray;
  }

  constructor(private fb: FormBuilder) {}

  ngOnInit() {
    this.registrationForm = this.fb.group({
      email: [""],
      alternateEmails: this.fb.array([])
    });
  }

  addAlternateEmail() {
    this.alternateEmails.push(this.fb.control(""));
  }
}
~~~

4. **app.component.html**
~~~ bash
<div class="container-fluid">
  <h2> Registration Form </h2>

  {{ registrationForm.value | json }}

  <form [formGroup]="registrationForm" (ngSubmit)="onSubmit()">

    <div class="form-group">
      <label> Email </label>
      <button type="button" class="btn btn-secondary btn-sm m-2" (click)="addAlternateEmail()"> Add e-mail </button>
      <input [class.is-invalid]="email.invalid && email.touched" formControlName="email" type="text" class="form-control" />

      <div formArrayName="alternateEmails" *ngFor="let email of alternateEmails.controls; let i=index">
          <input type="text" class="form-control my-1" [formControlName]="i" />
      </div>
    </div>

  </form>
</div>
~~~

# Reference
[Angular Forms Tutorial - 26 - Dynamic Form Controls](https://www.youtube.com/watch?v=4nJJQMxZkF0&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=58)