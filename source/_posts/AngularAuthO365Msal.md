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
  redirectUri: 'http://localhost:4200/',
  authority: 'https://login.microsoftonline.com/8135e33d-36db-4906-af5e-6a7df298ed96',
  interactionMode: 'popUp',
  graphEndpoint: 'https://graph.microsoft.com/v1.0/me',
  graphScopes: ['user.read']
};
~~~

3. **Add auth.service.ts**
~~~ bash
import { Injectable } from '@angular/core';
import * as Msal from 'msal';
import { environment } from '../environments/environment';
import { from } from 'rxjs';
import { map, catchError, mergeMap } from 'rxjs/operators';
import { CacheLocation } from 'msal';
import { HttpClient, HttpHeaders } from '@angular/common/http';

@Injectable({
    providedIn: 'root'
})
export class AuthService {

    private clientApplication: Msal.UserAgentApplication;

    constructor(
        private http: HttpClient
    ) {
        this.clientApplication = this.createApplication();
        this.clientApplication.handleRedirectCallback((error, response) => {
            console.log('response', response)
            console.log('error', error)
        });
    }

    private createApplication() {
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

    login() {
        const acquireTokenSilent = () =>
            from(
                this.clientApplication.acquireTokenSilent({
                    scopes: environment.graphScopes
                })
            );

        const acquireTokenPopup = () =>
            from(
                this.clientApplication.acquireTokenPopup({
                    scopes: environment.graphScopes,
                })
            ).pipe(
                catchError(error => {
                    window.alert('Error acquiring the popup:\n' + error);
                    return 'EMPTY';
                })
            );

        console.log(798, environment.graphScopes)
        from(this.clientApplication.loginPopup({
            scopes: environment.graphScopes
        })
        )
            .pipe(
                mergeMap(token => {
                    console.log('token', token)
                    return acquireTokenSilent().pipe(
                        catchError(err => acquireTokenPopup())
                    );
                })
            )
            .subscribe(
                (accessToken: any) => {
                    console.log('rawIdToken', accessToken.idToken.rawIdToken);
                    console.log('accessToken.accessToken', accessToken.accessToken);
                    localStorage.rawIdToken = accessToken.idToken.rawIdToken;
                    localStorage.accessToken = accessToken;
                },
                error => {
                    console.log(789, error);
                    window.alert('Error during login:\n' + error);
                }
            );
    }

    logout() {
        this.clientApplication.logout();
        delete localStorage.token;
        delete localStorage.user;
    }

    getInfo() {
        let token = localStorage.getItem('token');

        const header = new HttpHeaders().append(
            'Authorization',
            `Bearer ${localStorage.token}`
        );

        let url = 'https://graph.microsoft.com/v1.0/me';
        this.http.get(url, {
            headers: header,
            responseType: 'text'
        })
        .subscribe(data => console.log(data));
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
    this.authService.login();
  }

  onSignOut() {
    this.authService.logout();
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
1. ****
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