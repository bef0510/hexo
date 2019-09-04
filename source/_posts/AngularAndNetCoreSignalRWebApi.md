---
title: Angular + .Net Core SignalR WebApi
date: 2019-08-23 09:57:45
tags:
 - Angular Basic
 - .Net Core
 - SignalR
 - Postman
categories: 
 - .Net Core
---

# Angular + .Net Core SignalR WebApi
    需先建置 SignalR Interface
[Angular + .Net Core SignalR Interface](/2019/08/22/AngularAndNetCoreSignalRInterface/)

#### .Net Core
1. 增加 **Message.cs**
![Architecture](1.png)

2. **Message.cs**
~~~ bash
namespace HubServerBasic.Models
{
    public class Message
    {
        public string Message1 { get; set; }
        public string Message2 { get; set; }
    }
}
~~~

3. 增加 **MessageController.cs**
![Architecture](2.png)

4. **MessageController.cs**
~~~ bash
using System;
using HubServerBasic.Hubs;
using HubServerBasic.Models;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.SignalR;

namespace HubServerBasic.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class MessageController : ControllerBase
    {
        private IHubContext<MessageHub, IMessageClient> _hubContext;

        public MessageController(IHubContext<MessageHub, IMessageClient> hubContext)
        {
            _hubContext = hubContext;
        }

        [HttpPost("sendAll")]
        public string SendAll([FromBody]Message msg)
        {
            string retMessage;

            try
            {
                _hubContext.Clients.All.ReceiveMessage(msg.Message1, msg.Message2);
                retMessage = "Success";
            }
            catch (Exception e)
            {
                retMessage = e.ToString();
            }

            return retMessage;
        }

        [HttpPost("sendSpecificClient")]
        public string SendSpecificClient([FromBody]Message msg)
        {
            string retMessage;

            try
            {
                _hubContext.Clients.Client(msg.Message2).ReceiveMessage(msg.Message1);
                retMessage = "Success";
            }
            catch (Exception e)
            {
                retMessage = e.ToString();
            }

            return retMessage;
        }
    }
}
~~~

5. 執行
![Architecture](3.gif)

# Reference
[使用 ASP.NET Core SignalR 中樞](https://docs.microsoft.com/zh-tw/aspnet/core/signalr/hubs?view=aspnetcore-2.2)
[How to notify your Angular 7 app using SignalR](https://rukshan.dev/2019/05/how-to-notify-your-angular-7-app-using-signalr)