---
title: Angular Auth O365 V1.0 using adal
date: 2019-10-22 11:19:16
tags:
 - Angular Auth
categories: 
 - Angular
---

# Auth O365 V1.0 using adal
1. **Install**
`npm i microsoft-adal-angular6 --save`
![Architecture](1.png)

2. **Modify file environment.ts**
**src\environments\environment.ts**
~~~ bash
export const environment = {
  production: false,
  tenant: '8135e33d-36db-4906-af5e-6a7df298ed96',
  clientId: '310c879d-fa10-4d7e-be1b-3efe6a38a04c',
  extraQueryParameter: 'nux=1', // This is optional
  endpoints: {
    "http://localhost:4200/": '310c879d-fa10-4d7e-be1b-3efe6a38a04c' // Note, this is an object, you could add several things here
  }
};
~~~

3. **Add user.ts**
~~~ bash
export class User {
    id: number;
    username: string;
    password: string;
    firstName: string;
    lastName: string;
    authdata?: string;
}
~~~

4. **app.component.html**
~~~ bash
<button (click)="onSignIn()">Sign In</button>
<button (click)="onSignOut()">Sign Out</button>
<button (click)="onGetInfo()">Get Info</button>
~~~

5. **app.component.ts**
~~~ bash
import { Component } from '@angular/core';
import { MsAdalAngular6Service } from 'microsoft-adal-angular6';
import { HttpClient } from '@angular/common/http';
import { map} from 'rxjs/operators';
import { BehaviorSubject, Observable } from 'rxjs';
import { User } from './user';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'adal8';
  private currentUserSubject: BehaviorSubject<User>;
  public currentUser: Observable<User>;
  
  constructor(private adalSvc: MsAdalAngular6Service, private http: HttpClient){

      this.currentUserSubject = new BehaviorSubject<User>(JSON.parse(localStorage.getItem('currentUser')));

      this.currentUser = this.currentUserSubject.asObservable();

      this.adalSvc.acquireToken('https://graph.microsoft.com').subscribe(
        next => {
          this.currentUser = next;
          localStorage.setItem('currentUser', JSON.stringify(next));

        },
        error => {
          this.onSignIn();
        }
      )
    }
  
  onSignIn() {
    this.adalSvc.login();
  }

  onSignOut() {
    this.adalSvc.logout();
  }

  onGetInfo() {
    let aa = this.adalSvc.userInfo;

    const api = `https://172.17.98.63:8096/ForAuth/testAuth/11`;
    this.http.get<ApiResult[]>(api).pipe(
      map((result: any) => {
        console.log(123, result)

        if (result.statusCode === '200') {
          return JSON.parse(result.datas);
        }
      })
    ).subscribe((result) => {
      console.log(789, result)
    });
  }
}

export interface ApiResult {
  statusCode: string;
  statusMessage: string;
  datas: any;
}
~~~

6. **app.module.ts**
~~~ bash
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { HttpClientModule } from '@angular/common/http';
import { HTTP_INTERCEPTORS } from '@angular/common/http';
import { MsAdalAngular6Module } from 'microsoft-adal-angular6';


import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { environment } from '../environments/environment';
//import { InsertAuthTokenInterceptor } from './insert-auth-token-interceptor';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule,
    MsAdalAngular6Module.forRoot({
      tenant: environment.tenant,
      clientId: environment.clientId,
      redirectUri: window.location.origin,
      endpoints: environment.endpoints,
      navigateToLoginRequestUrl: false,
      extraQueryParameter: environment.extraQueryParameter,
      cacheLocation: 'localStorage'
    }),
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
~~~

7. **Run server**
![Architecture](2.gif)

## With header token
1. **Add insert-auth-token-interceptor.ts**
~~~ bash
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpHandler, HttpRequest } from '@angular/common/http';
import { mergeMap } from 'rxjs/operators';
import { MsAdalAngular6Service } from 'microsoft-adal-angular6';
 
@Injectable({
    providedIn: 'root'
  })
export class InsertAuthTokenInterceptor implements HttpInterceptor {
 
    constructor(private adal: MsAdalAngular6Service) { }
 
    public intercept(req: HttpRequest<any>, next: HttpHandler) {

        // get api url from adal config
        const resource = this.adal.GetResourceForEndpoint(req.url);
        if (!resource || !this.adal.isAuthenticated) {
            return next.handle(req);
        }
 
        // merge the bearer token into the existing headers
        return this.adal.acquireToken(resource).pipe(
            mergeMap((token: string) => {
                console.log('token', token)
                const authorizedRequest = req.clone({
                    headers: req.headers.set('api', `Bearer ${token}`),
                });
                return next.handle(authorizedRequest);
        }));
    }
}
~~~

2. **app.module.ts**
~~~ bash
import { InsertAuthTokenInterceptor } from './insert-auth-token-interceptor';

@NgModule({
  ...
  providers: [  {
      provide: HTTP_INTERCEPTORS,
      useClass: InsertAuthTokenInterceptor,
      multi: true,
    }],
  ...
})
export class AppModule { }
~~~

3. **GetInfo**
![Architecture](3.png)

# Reference
[microsoft-adal-angular6](https://www.npmjs.com/package/microsoft-adal-angular6)