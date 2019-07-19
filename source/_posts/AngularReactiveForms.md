---
title: Angular Reactive Forms
date: 2019-07-19 16:01:08
tags:
 - Angular Basic
 - Angular Form
categories: 
 - Angular
---

# Reactive Forms
1. **Code and the logic resides in the component class**
2. **No two way binding**
3. **Well suited for complex scenarios**
4. **Dynamic form fields**
5. **Custom validation**
6. **Dynamic validation**
7. **Unit test**
8. **Form**
    8.1 **FormGroup**
    8.2 **FormControl**
    ![Architecture](1.png)
9. **index.html import bootstrap**
10. **app.modules.ts import ReactiveFormsModule**

## Basic
1. **strict set value**
~~~ bash
this.registrationForm.setValue({
    userName: "Bruce",
    password: "test",
    confirmPassword: "test",
    address: {
        city: "City",
        state: "State",
        postalCode: "123456"
    }
});
~~~

2. **Patch set value**
~~~ bash
this.registrationForm.setValue({
    userName: "Bruce",
    password: "test",
    confirmPassword: "test"
});
~~~


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

@NgModule({
  declarations: [
    AppComponent
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
import { Component } from '@angular/core';
import { FormGroup, FormControl } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {

  registrationForm = new FormGroup({
    userName: new FormControl("Joey"),
    password: new FormControl(""),
    confirmPassword: new FormControl(""),
    address: new FormGroup({
      city: new FormControl(""),
      state: new FormControl(""),
      postalCode: new FormControl("")
    })
  });

  loadAPIData() {
    // this.registrationForm.setValue({
    //   userName: "Bruce",
    //   password: "test",
    //   confirmPassword: "test",
    //   address: {
    //     city: "City",
    //     state: "State",
    //     postalCode: "123456"
    //   }
    // });
    this.registrationForm.patchValue({
        userName: "Bruce",
        password: "test",
        confirmPassword: "test"
      });
  }
}

~~~

4. **app.component.html**
~~~ bash
<div class="container-fluid">
  <h2> Registration Form </h2>

  {{ registrationForm.value | json }}

  <form [formGroup]="registrationForm">
    <div class="form-group">
      <label> Username </label>
      <input formControlName="userName" type="text" class="form-control" />
    </div>

    <div class="form-group">
      <label> Password </label>
      <input formControlName="password" type="password" class="form-control" />
    </div>

    <div class="form-group">
      <label> Confirm Password </label>
      <input formControlName="confirmPassword" type="password" class="form-control" />
    </div>

    <div formGroupName="address">
      <div class="form-group">
        <label> City </label>
        <input formControlName="city" type="text" class="form-control">
      </div>

      <div class="form-group">
        <label> State </label>
        <input formControlName="state" type="text" class="form-control">
      </div>

      <div class="form-group">
        <label> Postal Code </label>
        <input formControlName="postalCode" type="text" class="form-control">
      </div>
    </div>

    <button (click)="loadAPIData()" class="btn btn-secondary ml-2" type="button"> Load API Data </button>

  </form>
</div>
~~~

#### FormBuilder Service
1. **less Instantiation new FormControl()**

2. **app.component.ts**
~~~ bash
import { Component } from '@angular/core';
import { FormBuilder } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {

  constructor(private fb: FormBuilder) {}

  registrationForm = this.fb.group({
    userName: ["Bruce"],
    password: [""],
    confirmPassword: [""],
    address: this.fb.group({
      city: [""],
      state: [""],
      postalCode: [""]
    })
  });

  loadAPIData() {
    this.registrationForm.patchValue({
        userName: "Bruce",
        password: "test",
        confirmPassword: "test"
      });
  }
}
~~~

# Reference
[Angular Forms Tutorial - 21 - FormBuilder Service](https://www.youtube.com/watch?v=3_dFnULt3FA&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=53)