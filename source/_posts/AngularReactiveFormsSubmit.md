---
title: Angular Reactive Forms Submit
date: 2019-07-23 11:50:06
tags:
 - Angular Basic
 - Angular Form
categories: 
 - Angular
---

# Reactive Forms Submit
1. **[Create Express Json Server](/2019/07/19/AngularBuildExpressJsonServer/)**
2. **index.html import bootstrap**
3. **app.modules.ts import FormsModule**
4. **app.modules.ts import HttpClientModule**
5. **Create registration.service.ts**
`ng g s registration --spec false`
6. **Create user-name.validator.ts**
7. **Create password.validator.ts**

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
import { HttpClientModule } from '@angular/common/http';
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
    ReactiveFormsModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
~~~

3. **registration.service.ts**
~~~ bash
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
@Injectable({
  providedIn: 'root'
})
export class RegistrationService {

  url = "http://localhost:3020/enroll";

  constructor(private http: HttpClient) { }

  register(userData) {
    return this.http.post<any>(this.url, userData);
  }
}
~~~

4. **app.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';
import { FormBuilder, Validators, FormGroup, FormArray } from '@angular/forms';
import { forbiddenNameValidator } from './shared/user-name.validator';
import { passwordValidator } from './shared/password.validator';
import { RegistrationService } from './registration.service';

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

  get alternateEmails() {
    return this.registrationForm.get("alternateEmails") as FormArray;
  }

  constructor(private fb: FormBuilder, private registrationService: RegistrationService) {}

  ngOnInit() {
    this.registrationForm = this.fb.group({
      id: [],
      userName: ["", [Validators.required, Validators.minLength(3), forbiddenNameValidator(/password/)]],
      email: [""],
      subscribe: [""],
      password: [""],
      confirmPassword: [""],
      address: this.fb.group({
        city: [""],
        state: [""],
        postalCode: [""]
      }),
      alternateEmails: this.fb.array([])
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

  addAlternateEmail() {
    this.alternateEmails.push(this.fb.control(""));
  }

  onSubmit() {
    this.registrationService.register(this.registrationForm.value)
      .subscribe(
        response => console.log("Success!", response),
        error => console.log("Error!", error),
      );
  }
}
~~~

5. **user-name.validator.ts**
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

6. **password.validator.ts**
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

7. **app.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';
import { FormBuilder, Validators, FormGroup, FormArray } from '@angular/forms';
import { forbiddenNameValidator } from './shared/user-name.validator';
import { passwordValidator } from './shared/password.validator';
import { RegistrationService } from './registration.service';

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

  get alternateEmails() {
    return this.registrationForm.get("alternateEmails") as FormArray;
  }

  constructor(private fb: FormBuilder, private registrationService: RegistrationService) {}

  ngOnInit() {
    this.registrationForm = this.fb.group({
      id: [],
      userName: ["", [Validators.required, Validators.minLength(3), forbiddenNameValidator(/password/)]],
      email: [""],
      subscribe: [""],
      password: [""],
      confirmPassword: [""],
      address: this.fb.group({
        city: [""],
        state: [""],
        postalCode: [""]
      }),
      alternateEmails: this.fb.array([])
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

  addAlternateEmail() {
    this.alternateEmails.push(this.fb.control(""));
  }

  onSubmit() {
    this.registrationService.register(this.registrationForm.value)
      .subscribe(
        response => console.log("Success!", response),
        error => console.log("Error!", error),
      );
  }
}
~~~

8. **app.component.html**
~~~ bash
<div class="container-fluid">
  <h2> Registration Form </h2>

  {{ registrationForm.value | json }}

  <form [formGroup]="registrationForm" (ngSubmit)="onSubmit()">
      <div class="form-group">
          <label> id </label>
          <input formControlName="id" type="text" class="form-control" />
        </div>

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
      <button type="button" class="btn btn-secondary btn-sm m-2" (click)="addAlternateEmail()"> Add e-mail </button>
      <input [class.is-invalid]="email.invalid && email.touched" formControlName="email" type="text" class="form-control" />
       <!--Custom Validation-->
       <small [class.d-none]="email.valid || email.untouched" class="text-danger"> Email is required </small>
       <div formArrayName="alternateEmails" *ngFor="let email of alternateEmails.controls; let i=index">
         <input type="text" class="form-control my-1" [formControlName]="i" />
       </div>
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

    <button [disabled]="!registrationForm.valid" class="btn btn-primary" type="submit"> Register </button>
    <button (click)="loadAPIData()" class="btn btn-secondary ml-2" type="button"> Load API Data </button>
    
  </form>
</div>
~~~

# Reference
[Angular Forms Tutorial - 27 - Submitting Form Data](https://www.youtube.com/watch?v=bHcQx4hCF_0&list=PLC3y8-rFHvwhBRAgFinJR8KHIrCdTkZcZ&index=59)