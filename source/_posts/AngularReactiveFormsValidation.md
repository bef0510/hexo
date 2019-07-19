---
title: Angular Reactive Forms Validation
date: 2019-07-19 16:42:35
tags:
 - Angular Basic
 - Angular Form
categories: 
 - Angular
---

# Reactive Forms Validation
1. **index.html import bootstrap**
2. **app.modules.ts import ReactiveFormsModule**

#### Basic validation
1. **.ts**
~~~ bash
get userName() {
    return this.registrationForm.get("userName");
}

registrationForm = this.fb.group({
    //userName: ["", Validators.required],
    userName: ["", [Validators.required, Validators.minLength(3)]],
});
~~~

2. **.html**
~~~ bash
<input [class.is-invalid]="userName.invalid && userName.touched" formControlName="userName" type="text" class="form-control" />

<!--Error Messages (not using get userName)-->
<!--<small [class.d-none]="registrationForm.get('userName').valid || registrationForm.get('userName').untouched" class="text-danger"> Username is required </small>-->

<!--Error Messages-->
<div *ngIf="userName.invalid && userName.touched">
    <small *ngIf="userName.errors?.required" class="text-danger"> Username is required </small>
    <small *ngIf="userName.errors?.minlength" class="text-danger"> Username must be at least 3 characters </small>
</div>
~~~

#### Custom validation
1. **Create user-name.validator.ts**
2. **.ts**
~~~ bash
get userName() {
    return this.registrationForm.get("userName");
}

constructor(private fb: FormBuilder) {}

registrationForm = this.fb.group({
    //userName: ["", [Validators.required, Validators.minLength(3), forbiddenNameValidator]],
    userName: ["", [Validators.required, Validators.minLength(3), forbiddenNameValidator(/password/)]],
});
~~~

3. **.html**
~~~ bash
<div *ngIf="userName.invalid && userName.touched">
    <!--Custom Validation-->
    <small *ngIf="userName.errors?.forbiddenName" class="text-danger"> {{ userName.errors?.forbiddenName.value }} Username not allowed </small>
</div>
~~~

#### Cross Field validation
1. **Create password.validator.ts**
2. **.ts**
~~~ bash
get userName() {
    return this.registrationForm.get("userName");
}

constructor(private fb: FormBuilder) {}

registrationForm = this.fb.group({
    userName: ["", [Validators.required, Validators.minLength(3), forbiddenNameValidator(/password/)]],
}, { validator: passwordValidator});
~~~

3. **.html**
~~~ bash
<div class="form-group">
    <!--Custom Validation-->
    <small class="text-danger" *ngIf="registrationForm.errors?.misMatch"> Passwords do not match </small>
</div>
~~~

#### Conditional validation
![Architecture](1.gif)
1. **.ts**
~~~ bash
this.registrationForm.get("subscribe").valueChanges
    .subscribe(checkedValue => {
        const email = this.registrationForm.get("email");
        if (checkedValue) {
            email.setValidators(Validators.required);
        } else {
             email.clearValidators();
        }

        email.updateValueAndValidity();

    });
~~~

2. **.html**
~~~ bash
<div class="form-group">
    <label> Email </label>
    <input [class.is-invalid]="email.invalid && email.touched" formControlName="email" type="text" class="form-control" />
    <!--Custom Validation-->
    <small [class.d-none]="email.valid || email.untouched" class="text-danger"> Email is required </small>
</div>

<div class="form-check mb-3">
    <input formControlName="subscribe" type="checkbox" class="form-check-input" />
    <label class="form-check-label"> Send me promotional offers </label>
</div>
~~~

#### All Code
1. **Folder Struct**
![Architecture](2.png)

2. **index.html**
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

3. **app.module.ts**
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

4. **app.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';
import { FormBuilder, Validators, FormGroup } from '@angular/forms';
import { forbiddenNameValidator } from './shared/user-name.validator';
import { passwordValidator } from './shared/password.validator';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  registrationForm: FormGroup;

  get userName() {
    return this.registrationForm.get("userName");
  }

  get email() {
    return this.registrationForm.get("email");
  }

  constructor(private fb: FormBuilder) {}

  ngOnInit() {
    this.registrationForm = this.fb.group({
      userName: ["", [Validators.required, Validators.minLength(3), forbiddenNameValidator(/password/)]],
      email: [""],
      subscribe: [""],
      password: [""],
      confirmPassword: [""],
      address: this.fb.group({
        city: [""],
        state: [""],
        postalCode: [""]
      })
    }, { validator: passwordValidator});

    this.registrationForm.get("subscribe").valueChanges
      .subscribe(checkedValue => {
        const email = this.registrationForm.get("email");
        if (checkedValue) {
          email.setValidators(Validators.required);
        } else {
          email.clearValidators();
        }

        email.updateValueAndValidity();

      });
  }
}
~~~

5. **app.component.html**
~~~ bash
<div class="container-fluid">
  <h2> Registration Form </h2>

  {{ registrationForm.value | json }}

  <form [formGroup]="registrationForm">
    <div class="form-group">
      <label> Username </label>
      <input [class.is-invalid]="userName.invalid && userName.touched"
                                  formControlName="userName" type="text" class="form-control" />
      <!--Error Messages-->
      <!--<small [class.d-none]="registrationForm.get('userName').valid ||
                              registrationForm.get('userName').untouched" class="text-danger"> Username is required </small>-->
      <div *ngIf="userName.invalid && userName.touched">
        <small *ngIf="userName.errors?.required" class="text-danger"> Username is required </small>
        <small *ngIf="userName.errors?.minlength" class="text-danger"> Username must be at least 3 characters </small>
        <!--Custom Validation-->
        <small *ngIf="userName.errors?.forbiddenName" class="text-danger"> {{ userName.errors?.forbiddenName.value }} Username not allowed </small>
      </div>
    </div>

    <div class="form-group">
      <label> Email </label>
      <input [class.is-invalid]="email.invalid && email.touched" formControlName="email" type="text" class="form-control" />
       <!--Custom Validation-->
       <small [class.d-none]="email.valid || email.untouched" class="text-danger"> Email is required </small>
    </div>

    <div class="form-check mb-3">
      <input formControlName="subscribe" type="checkbox" class="form-check-input" />
      <label class="form-check-label"> Send me promotional offers </label>
    </div>

    <div class="form-group">
      <label> Password </label>
      <input formControlName="password" type="password" class="form-control" />
    </div>

    <div class="form-group">
      <label> Confirm Password </label>
      <input [class.id-invalid]="registrationForm.errors?.misMatch" formControlName="confirmPassword" type="password" class="form-control" />
      <!--Custom Validation-->
      <small class="text-danger" *ngIf="registrationForm.errors?.misMatch"> Passwords do not match </small>
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

    <button class="btn btn-primary" type="submit"> Register </button>
    <button (click)="loadAPIData()" class="btn btn-secondary ml-2" type="button"> Load API Data </button>
    
  </form>
</div>
~~~

6. **user-name.validator.ts**
~~~ bash
import { AbstractControl, ValidatorFn } from "@angular/forms";

// basic
// export function forbiddenNameValidator(control: AbstractControl): { [key: string]: any } | null {
//     const forbidden = /admin/.test(control.value);
//     return forbidden ? { "forbiddenName": { value: control.value }} : null;
// }

// factory function
export function forbiddenNameValidator(forbiddenName: RegExp): ValidatorFn {
    return (control: AbstractControl): { [key: string]: any } | null => {
        const forbidden = forbiddenName.test(control.value);
        return forbidden ? { "forbiddenName": { value: control.value }} : null;
    }
}
~~~

7. **password.validator.ts**
~~~ bash
import { AbstractControl } from "@angular/forms";

export function passwordValidator(control: AbstractControl): { [key: string]: boolean } | null {
    const password = control.get("password");
    const confirmPassword = control.get("confirmPassword");
    if(password.pristine || confirmPassword.pristine) {
        return null;
    }
    return password && confirmPassword && password.value !== confirmPassword.value ? { "misMatch": true } : null;
}
~~~

# Reference
[Angular Forms Tutorial - 25 - Conditional Validation](https://www.youtube.com/watch?v=7y6GV3EeMHE&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=57)