---
title: Angular + .Net Core JWT
date: 2019-08-19 10:44:33
tags:
 - Angular
 - .Net Core
 - JWT
categories: 
 - .Net Core
---

# Angular + .Net Core JWT

#### .Net Core
1. 開啟 **VS**
![Architecture](1.png)
![Architecture](2.png)
![Architecture](3.png)

2. 增加 **Models**
![Architecture](4.png)

**AppSettings.cs**
~~~ bash
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

namespace CoreJWT.Models
{
    public class AppSettings
    {
        public string Secret { get; set; }
    }
}
~~~

**User.cs**
~~~ bash
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

namespace CoreJWT.Models
{
    public class User
    {
        public int Id { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Username { get; set; }
        public string Password { get; set; }
        public string JwtToken { get; set; }
    }
}
~~~

3. 增加 **UsersController.cs**
![Architecture](5.png)
![Architecture](6.png)
![Architecture](7.png)
~~~ bash
using CoreJWT.Models;
using CoreJWT.Services;
using Microsoft.AspNetCore.Mvc;

namespace CoreJWT.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class UsersController : ControllerBase
    {
        private IUserService userService;

        public UsersController(IUserService userService)
        {
            this.userService = userService;
        }

        [HttpPost("authenticate")]
        public IActionResult Authenticate([FromBody]User userParam)
        {
            var user = this.userService.Authenticate(userParam.Username, userParam.Password);

            if (user == null)
                return BadRequest(new { message = "Username or password is incorrect" });

            return Ok(user);
        }

        [HttpGet("getAllUser")]
        public IActionResult GetAllUser()
        {
            var user = this.userService.GetAll();

            return Ok(user);
        }

        [HttpDelete("{id}")]
        public IActionResult DeleteUser(int id)
        {
            var user = this.userService.DeleteUser(id);

            return Ok(user);
        }


        [HttpPost("register")]
        public IActionResult Register([FromBody]User userParam)
        {
            var user = this.userService.Register(userParam);

            return Ok(user);
        }
    }
}
~~~

4. 增加 **service**
![Architecture](8.png)

**IUserService.cs**
~~~ bash
using CoreJWT.Models;
using System.Collections.Generic;

namespace CoreJWT.Services
{
    public interface IUserService
    {
        User Authenticate(string username, string password);
        IEnumerable<User> GetAll();
        IEnumerable<User> DeleteUser(int id);
        IEnumerable<User> Register(User user);
    }
}
~~~

**UserService.cs**
~~~ bash
using CoreJWT.Models;
using Microsoft.Extensions.Options;
using Microsoft.IdentityModel.Tokens;
using System;
using System.Collections.Generic;
using System.IdentityModel.Tokens.Jwt;
using System.Linq;
using System.Security.Claims;
using System.Text;

namespace CoreJWT.Services
{
    public class UserService : IUserService
    {
        private List<User> users = new List<User>
        {
            new User { Id = 1, FirstName = "Test1", LastName = "User1", Username = "test1", Password = "test1" },
            new User { Id = 2, FirstName = "Test2", LastName = "User2", Username = "test2", Password = "test2" },
            new User { Id = 3, FirstName = "Test3", LastName = "User3", Username = "test3", Password = "test3" },
            new User { Id = 4, FirstName = "Test4", LastName = "User4", Username = "test4", Password = "test4" },
            new User { Id = 5, FirstName = "Test5", LastName = "User5", Username = "test5", Password = "test5" }
        };

        private readonly AppSettings appSettings;

        public UserService(IOptions<AppSettings> appSettings)
        {
            this.appSettings = appSettings.Value;
        }

        public User Authenticate(string username, string password)
        {
            var user = this.users.SingleOrDefault(x => x.Username == username && x.Password == password);

            if (user == null)
            {
                return null;
            }

            // authentication successful so generate jwt token
            var tokenHandler = new JwtSecurityTokenHandler();
            var key = Encoding.ASCII.GetBytes(this.appSettings.Secret);
            var tokenDescriptor = new SecurityTokenDescriptor
            {
                Subject = new ClaimsIdentity(new Claim[]
                {
                    new Claim(ClaimTypes.Name, user.Id.ToString())
                }),
                Expires = DateTime.UtcNow.AddDays(7),
                SigningCredentials = new SigningCredentials(new SymmetricSecurityKey(key), SecurityAlgorithms.HmacSha256Signature)
            };
            var token = tokenHandler.CreateToken(tokenDescriptor);
            user.JwtToken = tokenHandler.WriteToken(token);

            // remove password before returning
            user.Password = null;

            return user;
        }

        public IEnumerable<User> GetAll()
        {
            return this.users.Select(x =>
            {
                x.Password = null;
                return x;
            });
        }

        public IEnumerable<User> DeleteUser(int id)
        {
            return this.users.Where(o => o.Id != id).Select(x =>
            {
                x.Password = null;
                return x;
            });
        }


        public IEnumerable<User> Register(User user)
        {
            var _users = this.users.ToList();

            _users.Add(user);

            return _users;
        }

    }
}
~~~

5. 修改 **appsettings.json**
![Architecture](9.png)

~~~ bash
{
  "AppSettings": {
    "Secret": "THIS IS USED TO SIGN AND VERIFY JWT TOKENS, REPLACE IT WITH YOUR OWN SECRET, IT CAN BE ANY STRING"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
~~~

6. 修改 **Startup.cs**
![Architecture](10.png)
![Architecture](11.png)

~~~ bash
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using CoreJWT.Models;
using CoreJWT.Services;
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.HttpsPolicy;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.SpaServices.AngularCli;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Options;
using Microsoft.IdentityModel.Tokens;

namespace CoreJWT
{
    public class Startup
    {
        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }

        public IConfiguration Configuration { get; }

        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
            services.AddSpaStaticFiles(c => { c.RootPath = "ClientApp/dist"; });
            // configure strongly typed settings objects
            var appSettingsSection = Configuration.GetSection("AppSettings");
            services.Configure<AppSettings>(appSettingsSection);

            // configure jwt authentication
            var appSettings = appSettingsSection.Get<AppSettings>();
            var key = Encoding.ASCII.GetBytes(appSettings.Secret);
            services.AddAuthentication(x =>
            {
                x.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
                x.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
            }).AddJwtBearer(x =>
            {
                x.RequireHttpsMetadata = false;
                x.SaveToken = true;
                x.TokenValidationParameters = new TokenValidationParameters
                {
                    ValidateIssuerSigningKey = true,
                    IssuerSigningKey = new SymmetricSecurityKey(key),
                    ValidateIssuer = false,
                    ValidateAudience = false
                };
            });

            // configure DI for application services
            services.AddScoped<IUserService, UserService>();

        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Error");
            }

            // global cors policy
            app.UseCors(x => x
                .AllowAnyOrigin()
                .AllowAnyMethod()
                .AllowAnyHeader());

            app.UseMvc(routes =>
            {
                routes.MapRoute(
                    name: "default",
                    template: "api/{controller}/{action=Index}/{id?}");
            });

            app.UseSpa(spa =>
            {
                spa.Options.SourcePath = "ClientApp";
                if (env.IsDevelopment())
                {
                    spa.UseAngularCliServer(npmScript: "start");
                }
            });

            app.UseAuthentication();
            app.UseHttpsRedirection();
        }
    }
}
~~~

#### Postman call web api
![Architecture](12.png)

### Angular
1. 新增專案 **cmd** → **ng new ClientApp**
![Architecture](13.png)
![Architecture](14.png)

2. 開啟 **VS Code**
![Architecture](15.png)
![Architecture](16.png)

3. 修改 **index.html**
~~~ bash
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>ClientApp</title>
  <base href="/">

  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">

   <!-- bootstrap css -->
   <link href="//cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.3.1/css/bootstrap.min.css" rel="stylesheet" />

   <style>
       a { cursor: pointer; }
   </style>
   
</head>
<body>
  <app-root></app-root>
</body>
</html>
~~~

4. 修改 **app-routing.module.cs**
~~~ bash
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { AuthGuard } from './_helpers';
import { HomeComponent } from './home/home.component';
import { LoginComponent } from './login/login.component';
import { RegisterComponent } from './register/register.component';

const routes: Routes = [
	{ path: '', component: HomeComponent, canActivate: [AuthGuard] },
	{ path: 'login', component: LoginComponent },
	{ path: 'register', component: RegisterComponent },

	// otherwise redirect to home
	{ path: '**', redirectTo: '' }
];

@NgModule({
	imports: [RouterModule.forRoot(routes)],
	exports: [RouterModule]
})
export class AppRoutingModule { }
~~~

5. 修改 **app.module.ts**
~~~ bash
import { BrowserModule } from '@angular/platform-browser';
import { ReactiveFormsModule } from '@angular/forms';
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HomeComponent } from './home/home.component';
import { LoginComponent } from './login/login.component';
import { AlertComponent } from './_components/alert.component';
import { RegisterComponent } from './register/register.component';

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    LoginComponent,
    AlertComponent,
    RegisterComponent
  ],
  imports: [
    BrowserModule,
    ReactiveFormsModule,
    HttpClientModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
~~~

6. 修改 **app.component.html**
~~~ bash
<!-- nav -->
<nav class="navbar navbar-expand navbar-dark bg-dark" *ngIf="currentUser">
  <div class="navbar-nav">
      <a class="nav-item nav-link" routerLink="/">Home</a>
      <a class="nav-item nav-link" (click)="logout()">Logout</a>
  </div>
</nav>

<!-- main app container -->
<div class="jumbotron">
  <div class="container">
      <div class="row">
          <div class="col-sm-6 offset-sm-3">
              <app-alert></app-alert>
              <router-outlet></router-outlet>
          </div>
      </div>
  </div>
</div>

<!-- credits -->
<div class="text-center">
  <p>
      <a href="https://angular.io/cli/generate" target="_top">Angular 8</a>
  </p>
</div>
~~~

7. 修改 **app.component.ts**
~~~ bash
import { Component } from '@angular/core';
import { User } from './_models';
import { AuthenticationService } from './_service';
import { Router } from '@angular/router';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  currentUser: User;

  constructor(
    private router: Router,
    private authenticationService: AuthenticationService
  ) { 
    
  }
}
~~~

8. 新增 **config.ts**
![Architecture](20.png)
~~~ bash
export const config = {
    apiUrl: 'https://localhost:44354/api'
  };
~~~

#### Create component login
1. **ng g c login --spec false**
2. 修改 **login.component.html**
~~~ bash
<h2>Login</h2>
<form [formGroup]="loginForm" (ngSubmit)="onSubmit()">
    <div class="form-group">
        <label for="username">Username</label>
        <input type="text" formControlName="username" class="form-control" [ngClass]="{ 'is-invalid': submitted && f.username.errors }" />
        <div *ngIf="submitted && f.username.errors" class="invalid-feedback">
            <div *ngIf="f.username.errors.required">Username is required</div>
        </div>
    </div>
    <div class="form-group">
        <label for="password">Password</label>
        <input type="password" formControlName="password" class="form-control" [ngClass]="{ 'is-invalid': submitted && f.password.errors }" />
        <div *ngIf="submitted && f.password.errors" class="invalid-feedback">
            <div *ngIf="f.password.errors.required">Password is required</div>
        </div>
    </div>
    <div class="form-group">
        <button [disabled]="loading" class="btn btn-primary">
            <span *ngIf="loading" class="spinner-border spinner-border-sm mr-1"></span>
            Login
        </button>
        <a routerLink="/register" class="btn btn-link">Register</a>
    </div>
</form>
~~~

3. 修改 **login.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, Router } from '@angular/router';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { first } from 'rxjs/operators';

import { AuthenticationService, AlertService } from '../_service';

@Component({
	selector: 'app-login',
	templateUrl: './login.component.html',
	styleUrls: ['./login.component.css']
})
export class LoginComponent implements OnInit {

	loginForm: FormGroup;
	loading = false;
	submitted = false;
	returnUrl: string;

	constructor(
		private formBuilder: FormBuilder,
		private route: ActivatedRoute,
		private router: Router,
		private authenticationService: AuthenticationService,
		private alertService: AlertService
	) {
		// redirect to home if already logged in
		if (this.authenticationService.currentUserValue) {
			this.router.navigate(['/']);
		}
	}

	ngOnInit() {
		this.loginForm = this.formBuilder.group({
			username: ['', Validators.required],
			password: ['', Validators.required]
		});

		// get return url from route parameters or default to '/'
		this.returnUrl = this.route.snapshot.queryParams['returnUrl'] || '/';
	}

	// convenience getter for easy access to form fields
	get f() { return this.loginForm.controls; }

	onSubmit() {
		this.submitted = true;

		// reset alerts on submit
		this.alertService.clear();

		// stop here if form is invalid
        if (this.loginForm.invalid) {
            return;
		}
		
		this.loading = true;
		this.authenticationService.login(this.f.username.value, this.f.password.value)
			.pipe(first())
			.subscribe(
				data => {
					this.router.navigate([this.returnUrl]);
				},
				error => {
					this.alertService.error(error.error.message);
					this.loading = false;
				});
	}

}
~~~

#### Create component home
1. **ng g c home --spec false**
2. 修改 **home.component.html**
~~~ bash
<h1>Hi {{currentUser.firstName}}!</h1>
<p>You are logged in with Angular 8!!</p>
<h3>All registered users:</h3>
<ul>
    <li *ngFor="let user of users">
        {{user.username}} ({{user.firstName}} {{user.lastName}})
        - <a (click)="deleteUser(user.id)" class="text-danger">Delete</a>
    </li>
</ul>
~~~

3. 修改 **home.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';
import { User } from '../_models';
import { AuthenticationService, UsersService } from '../_service';
import { first } from 'rxjs/operators';

@Component({
	selector: 'app-home',
	templateUrl: './home.component.html',
	styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {

	currentUser: User;
	users = [];

	constructor(
		private authenticationService: AuthenticationService,
		private usersService: UsersService
	) {
		this.currentUser = this.authenticationService.currentUserValue;
	}

	ngOnInit() {
		this.loadAllUsers();
	}

	deleteUser(id: number) {
		console.log(123,id)
        this.usersService.delete(id)
            .pipe(first())
            .subscribe(users => {
				this.users = users
			});
    }

	private loadAllUsers() {
        this.usersService.getAll()
            .pipe(first())
            .subscribe(users => this.users = users);
    }
}
~~~

#### Create component register
1. **ng g c register --spec false**
2. 修改 **register.component.html**
~~~ bash
<h2>Register</h2>
<form [formGroup]="registerForm" (ngSubmit)="onSubmit()">
    <div class="form-group">
        <label for="firstName">First Name</label>
        <input type="text" formControlName="firstName" class="form-control" [ngClass]="{ 'is-invalid': submitted && f.firstName.errors }" />
        <div *ngIf="submitted && f.firstName.errors" class="invalid-feedback">
            <div *ngIf="f.firstName.errors.required">First Name is required</div>
        </div>
    </div>
    <div class="form-group">
        <label for="lastName">Last Name</label>
        <input type="text" formControlName="lastName" class="form-control" [ngClass]="{ 'is-invalid': submitted && f.lastName.errors }" />
        <div *ngIf="submitted && f.lastName.errors" class="invalid-feedback">
            <div *ngIf="f.lastName.errors.required">Last Name is required</div>
        </div>
    </div>
    <div class="form-group">
        <label for="username">Username</label>
        <input type="text" formControlName="username" class="form-control" [ngClass]="{ 'is-invalid': submitted && f.username.errors }" />
        <div *ngIf="submitted && f.username.errors" class="invalid-feedback">
            <div *ngIf="f.username.errors.required">Username is required</div>
        </div>
    </div>
    <div class="form-group">
        <label for="password">Password</label>
        <input type="password" formControlName="password" class="form-control" [ngClass]="{ 'is-invalid': submitted && f.password.errors }" />
        <div *ngIf="submitted && f.password.errors" class="invalid-feedback">
            <div *ngIf="f.password.errors.required">Password is required</div>
            <div *ngIf="f.password.errors.minlength">Password must be at least 6 characters</div>
        </div>
    </div>
    <div class="form-group">
        <button [disabled]="loading" class="btn btn-primary">
            <span *ngIf="loading" class="spinner-border spinner-border-sm mr-1"></span>
            Register
        </button>
        <a routerLink="/login" class="btn btn-link">Cancel</a>
    </div>
</form>
~~~

3. 修改 **register.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';
import { FormGroup, FormBuilder, Validators } from '@angular/forms';
import { AlertService, UsersService } from '../_service';
import { first } from 'rxjs/operators';
import { Router } from '@angular/router';

@Component({
	selector: 'app-register',
	templateUrl: './register.component.html',
	styleUrls: ['./register.component.css']
})
export class RegisterComponent implements OnInit {
	[x: string]: any;

	registerForm: FormGroup;
	loading = false;
	submitted = false;

	constructor(
		private formBuilder: FormBuilder,
		private router: Router,
		private userService: UsersService,
		private alertService: AlertService
	) { }

	ngOnInit() {
		this.registerForm = this.formBuilder.group({
			firstName: ['', Validators.required],
			lastName: ['', Validators.required],
			username: ['', Validators.required],
			password: ['', [Validators.required, Validators.minLength(6)]]
		});
	}

	// convenience getter for easy access to form fields
	get f() { return this.registerForm.controls; }

	onSubmit() {
		this.submitted = true;
  
		// reset alerts on submit
		this.alertService.clear();
		
		// stop here if form is invalid
		if (this.registerForm.invalid) {
			return;
		}

		this.loading = true;
		this.userService.register(this.registerForm.value)
		.pipe(first())
		.subscribe(
			data => {
				this.alertService.success('Registration successful', true);
                this.router.navigate(['/login']);
			},
			error => {
				this.alertService.error(error.error.message);
                this.loading = false;
			});
	}
}
~~~

#### Create _components
1. 新增 **folder naming _components**
![Architecture](17.png)

2. 新增 **file naming alert.component.css**

3. 新增 **file naming alert.component.html**
~~~ bash
<div *ngIf="message" [ngClass]="message.cssClass">{{message.text}}</div>
~~~

4. 新增 **file naming alert.component.ts**
~~~ bash
import { Component, OnInit, OnDestroy } from '@angular/core';
import { Subscription } from 'rxjs';

import { AlertService } from '../_service';

@Component({
	selector: 'app-alert',
	templateUrl: './alert.component.html',
	styleUrls: ['./alert.component.css']
})
export class AlertComponent implements OnInit, OnDestroy {

	private subscription: Subscription;
	message: any;

	constructor(private alertService: AlertService) { }

	ngOnInit() {
		this.subscription = this.alertService.getAlert()
			.subscribe(message => {

				switch (message && message.type) {
					case 'success':
						message.cssClass = 'alert alert-success';
						break;
					case 'error':
						message.cssClass = 'alert alert-danger';
						break;
				}

				this.message = message;
			});
	}

	ngOnDestroy() {
        this.subscription.unsubscribe();
    }
}
~~~

#### Create _helpers
1. 新增 **folder naming _helpers**
![Architecture](18.png)

2. 新增 **file auth.guard.ts**
~~~ bash
import { Injectable } from '@angular/core';
import { CanActivate, Router, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';

import { AuthenticationService } from '../_service';

@Injectable({
	providedIn: 'root'
})
export class AuthGuard implements CanActivate {
	constructor(
		private router: Router,
		private authenticationService: AuthenticationService
	) { }

	canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot) {
		const currentUser = this.authenticationService.currentUserValue;
		
		if (currentUser) {
            // authorised so return true
            return true;
		}
		
		this.router.navigate(['/login'], { queryParams: { returnUrl: state.url } });
		return false;
	}
}
~~~

3. 新增 **file index.ts**
~~~ bash
export * from './auth.guard';
~~~

#### Create _models
1. 新增 **folder naming _models**
![Architecture](19.png)

2. 新增 **file user.ts**
~~~ bash
export class User {
    id: string;
    firstName: string;
    lastName: string;
    username: string;
    password: string;
    jwtToken: string;
}
~~~

3. 新增 **file index.ts**
~~~ bash
export * from './user';
~~~

#### Create _service
1. 新增 **folder naming _service**
![Architecture](20.png)

2. 新增 **file alert.service.ts**
~~~ bash
import { Injectable } from '@angular/core';
import { Observable, Subject } from 'rxjs';
import { Router, NavigationStart } from '@angular/router';

@Injectable({
	providedIn: 'root'
})
export class AlertService {

	private subject = new Subject<any>();
	private keepAfterRouteChange = false;

	constructor(private router: Router) {
		// clear alert messages on route change unless 'keepAfterRouteChange' flag is true
		this.router.events.subscribe(event => {
			if (event instanceof NavigationStart) {
				if (this.keepAfterRouteChange) {
					// only keep for a single route change
					this.keepAfterRouteChange = false;
				} else {
					// clear alert message
					this.clear();
				}
			}
		});
	}

	getAlert(): Observable<any> {
		return this.subject.asObservable();
	}

	success(message: string, keepAfterRouteChange = false) {
        this.keepAfterRouteChange = keepAfterRouteChange;
        this.subject.next({ type: 'success', text: message });
	}
	
	error(message: string, keepAfterRouteChange = false) {
		this.keepAfterRouteChange = keepAfterRouteChange;
		this.subject.next({ type: 'error', text: message });
	}

	clear() {
		// clear by calling subject.next() without parameters
		this.subject.next();
	}
}
~~~

3. 新增 **file authentication.service.ts**
~~~ bash
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { BehaviorSubject, Observable } from 'rxjs';
import { map } from 'rxjs/operators';

import { User } from '../_models';
import { config } from '../config';

@Injectable({
	providedIn: 'root'
})
export class AuthenticationService {

	public currentUser: Observable<User>;
	private currentUserSubject: BehaviorSubject<User>;

	constructor(private http: HttpClient) {
		this.currentUserSubject = new BehaviorSubject<User>(JSON.parse(localStorage.getItem('currentUser')));
		this.currentUser = this.currentUserSubject.asObservable();
	}

	public get currentUserValue(): User {
		return this.currentUserSubject.value;
	}

	login(username, password) {
		var url = `${config.apiUrl}/Users/authenticate`;

		return this.http.post<any>(url, { username, password })
			.pipe(map(user => {

				// store user details and jwt token in local storage to keep user logged in between page refreshes
				localStorage.setItem('currentUser', JSON.stringify(user));

				this.currentUserSubject.next(user);

				return user;
			}));
	}
}
~~~

4. 新增 **file users.service.ts**
~~~ bash
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { config } from '../config';
import { User } from '../_models';

@Injectable({
	providedIn: 'root'
})
export class UsersService {

	constructor(private http: HttpClient) { }

	getAll() {
		var url = `${config.apiUrl}/Users/getAllUser`;
		return this.http.get<User[]>(url);
	}

	register(user: User) {
		var url = `${config.apiUrl}/Users/register`;
        return this.http.post(url, user);
    }

	delete(id: number) {
		var url = `${config.apiUrl}/Users/${id}`;
        return this.http.delete<User[]>(url);
    }
}
~~~

5. 新增 **file index.ts**
~~~ bash
export * from './alert.service';
export * from './authentication.service';
export * from './users.service';
~~~

# Reference
[ASP.NET Core Authentication with JWT and Angular](https://code-maze.com/authentication-aspnetcore-jwt-1/)