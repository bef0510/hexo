---
title: Angular Auth O365 V2.0 using msal
date: 2019-10-22 12:10:55
tags:
 - Angular Auth
categories: 
 - Angular
---

# Auth O365 V2.0 using msal
1. **Install**
`npm i msal --save`
![Architecture](1.png)

2. **Modify file environment.ts**
**src\environments\environment.ts**
~~~ bash
export const environment = {
  production: false,
  clientID: '310c879d-fa10-4d7e-be1b-3efe6a38a04c',
  redirectUri: 'http://172.17.98.158:5500/',
  authority: 'https://login.microsoftonline.com/8135e33d-36db-4906-af5e-6a7df298ed96',
  interactionMode: 'popUp',
  graphEndpoint: 'https://graph.microsoft.com/v1.0/me',
  graphScopes: ['api://310c879d-fa10-4d7e-be1b-3efe6a38a04c/access_as_user']
};
~~~

3. **Add auth.service.ts**
~~~ bash
import { Injectable, ɵConsole } from '@angular/core';
import * as Msal from 'msal';
import { environment } from '../../environments/environment';
import { from, Observable, of } from 'rxjs';
import { catchError, mergeMap, map } from 'rxjs/operators';
import { CacheLocation } from 'msal';
import { HttpClient } from '@angular/common/http';
@Injectable({
  providedIn: 'root'
})
export class AuthService {

  private clientApplication: Msal.UserAgentApplication;
  scope = { scopes: environment.graphScopes };

  constructor(private http: HttpClient) {
    this.clientApplication = this.createApplication();
    this.clientApplication.handleRedirectCallback((error, response) => {
      if (error) {
        console.log(error);
      } else {
        if (response.tokenType === "access_token") {
          console.log('response.tokenType', response.tokenType)
        } else {
          console.log("token type is:" + response.tokenType);
        }
      }
    });
  }

  onLoginPopup(): Observable<string | Msal.AuthResponse> {
    const loginPop = from(this.clientApplication.loginPopup(this.scope));

    return loginPop.pipe(
      mergeMap(token => {
        console.log('token', token)
        return this.onRefresh();
      })
    )
  }

  onLoginRedirect() {
    this.clientApplication.loginRedirect(this.scope);
  }

  onGetAccount(): Msal.Account {
    return this.clientApplication.getAccount();
  }
  
  onRefresh(): Observable<string | Msal.AuthResponse> {
    return this.acquireTokenSilent().pipe(
      catchError(err => this.acquireTokenPopup())
    );
  }

  onLogout() {
    this.clientApplication.logout();
    localStorage.clear();
  }

  onIsExpire(): boolean {
    const expiration = Math.round(new Date(localStorage.expiresOn).getTime() / 1000);
    const now = Math.round(new Date().getTime() / 1000);
    const leftTime = expiration - now;
    return leftTime < 0;
  }

  onSaveToken(accessToken: any): Observable<boolean> {
    localStorage.accessToken = accessToken.accessToken;
    localStorage.expiration = accessToken.idToken.expiration;
    localStorage.expiresOn = accessToken.expiresOn;

    return of(true);
  }


  getInfo() {

    let url = 'https://172.17.98.63:8096/ForAuth/testUserisAuth';
    let url1 = 'https://graph.microsoft.com/v1.0/me';
 
    this.http.get(url)
      .subscribe(data => console.log(data));
  }

  private createApplication(): Msal.UserAgentApplication {
    const msalConfig = {
      auth: {
        clientId: environment.clientID,
        redirectUri: environment.redirectUri,
        authority: environment.authority
      },
      cache: {
        storeAuthStateInCookie: true,
        cacheLocation: ('localStorage' as CacheLocation),
      }
    };

    return new Msal.UserAgentApplication(msalConfig);
  }


  private acquireTokenSilent(): Observable<Msal.AuthResponse> {
    return from(
      this.clientApplication.acquireTokenSilent(this.scope)
    );
  }

  private acquireTokenPopup(): Observable<string | Msal.AuthResponse> {
    return from(
      this.clientApplication.acquireTokenPopup(this.scope)
    ).pipe(
      catchError(error => {
        window.alert('Error acquiring the popup:\n' + error);
        return '';
      })
    );
  }
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
import { AuthService } from './auth.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'msal8';

  constructor(private authService: AuthService){}

  onSignIn() {
    if (localStorage.loginType === CONST_INFO.popupType) {
      this.authService.onLoginPopup()
        .subscribe((accessToken: any) => {

          this.authService.onSaveToken(accessToken);

          this.isLogin = true;
        }, (error: any) => {
          window.alert('Error during login:\n' + error);
        });
    } else if (localStorage.loginType === CONST_INFO.redirectType) {
      this.authService.onLoginRedirect();
    }
  }

  onSignOut() {
    this.authService.onLogout();
  }

  onGetInfo() {
   this.authService.getInfo();
  }
}
~~~

6. **app.module.ts**
~~~ bash
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
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
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
~~~

7. **Run server**
![Architecture](2.gif)


## Using Interceptor
1. **Add auth-interceptor.ts**
~~~ bash
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent, HttpEventType } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
    providedIn: 'root'
  })
export class AuthInterceptor implements HttpInterceptor {

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<HttpEventType.Response>> {
    const authReq = req.clone({
      setHeaders: { Authorization: `Bearer ${localStorage.rawIdToken}` }
    });
    return next.handle(authReq);
  }
}
~~~

# Reference
[msal](https://www.npmjs.com/package/msal)
[Application configuration options](https://docs.microsoft.com/bs-latn-ba/azure/active-directory/develop/msal-client-application-configuration)