---
title: Angular + .Net Core SignalR Interface
date: 2019-08-22 17:37:20
tags:
 - Angular
 - .Net Core
 - SignalR
categories: 
 - .Net Core
---

# Angular + .Net Core SignalR Interface
#### .Net Core
1. 增加 **Hubs**
![Architecture](1.png)

2. **IMessageClient.cs**
~~~ bash
using System.Threading.Tasks;

namespace HubServerBasic.Hubs
{
    public interface IMessageClient
    {
        Task ReceiveMessage(string message1, string message2);
        Task ReceiveMessage(string message);
    }
}
~~~

2. **MessageHub.cs**
~~~ bash
using Microsoft.AspNetCore.SignalR;
using System.Threading.Tasks;

namespace HubServerBasic.Hubs
{
    public class MessageHub : Hub<IMessageClient>
    {
        public string GetConnectionId()
        {
            return Context.ConnectionId;
        }

        public Task SendMessageToAll(string message1, string message2)
        {
            return Clients.All.ReceiveMessage(message1, message2);
        }

        public Task SendMessageToCaller(string message)
        {
            return Clients.Caller.ReceiveMessage(message);
        }

        public Task SendMessageToSpecificClient(string user, string message)
        {
            return Clients.Client(user).ReceiveMessage(message);
        }
    }
}
~~~

3. **Startup.cs**
~~~ bash
using HubServerBasic.Hubs;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.SpaServices.AngularCli;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;

namespace HubServerBasic
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
            services.AddSignalR();
            services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
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
                // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
                app.UseHsts();
            }

            app.UseSignalR(routes =>
            {
                routes.MapHub<MessageHub>("/hubs/message");
            });

            app.UseMvc();

            app.UseSpa(spa =>
            {
                spa.Options.SourcePath = "ClientApp";
                if (env.IsDevelopment())
                {
                    spa.UseAngularCliServer(npmScript: "start");
                }
            });

            app.UseHttpsRedirection();
        }
    }
}
~~~

#### Angular
1. 修改 **app.component.html**
~~~ bash
<input type="text" [(ngModel)]="message">
<br />
<input type="text" [(ngModel)]="connID">
<button (click)="sendAll()">Send All</button>
&nbsp;&nbsp;&nbsp;&nbsp;
<button (click)="sendCaller()">Send Caller</button>
&nbsp;&nbsp;&nbsp;&nbsp;
<button (click)="sendSpecificClient()">Send Specific Client</button>
&nbsp;&nbsp;&nbsp;&nbsp;
<button (click)="getConnID()">Get Connection ID</button>
&nbsp;&nbsp;&nbsp;&nbsp;
<p *ngFor="let m of messages">{{m}}</p>
~~~

2. 修改 **app.component.ts**
~~~ bash
import { Component, OnInit } from '@angular/core';
import { HubConnection, HubConnectionBuilder } from '@aspnet/signalr';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {

  public hubConnection: HubConnection;
  public messages: string[] = [];
  public message: string;
  public connID: string;

  constructor() { }

  ngOnInit(): void {

    this.hubConnection = new HubConnectionBuilder()
      .withUrl("https://localhost:44371/hubs/message")
      .build();
     
    this.hubConnection.on("ReceiveMessage", (user, message) => {
      this.messages.push(user);
    });

    this.hubConnection.start();
  }

  sendAll() {
    this.hubConnection.invoke("SendMessageToAll", this.message, this.message);
    this.message = "";
  }

  sendCaller() {
    this.hubConnection.invoke("SendMessageToCaller", this.message);
    this.message = "";
  }

  sendSpecificClient() {
    this.hubConnection.invoke("SendMessageToSpecificClient", this.connID, this.message);
    this.message = "";
  }

  getConnID() {
    this.hubConnection.invoke('getConnectionId')
    .then(function (connectionId) {
        console.log("connectionId", connectionId)
    });
  }
}
~~~

3. 修改 **app.module.ts**
~~~ bash
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { FormsModule } from '@angular/forms';

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

# Reference
[使用 ASP.NET Core SignalR 中樞](https://docs.microsoft.com/zh-tw/aspnet/core/signalr/hubs?view=aspnetcore-2.2)
[How to notify your Angular 7 app using SignalR](https://rukshan.dev/2019/05/how-to-notify-your-angular-7-app-using-signalr)