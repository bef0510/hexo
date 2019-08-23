---
title: .Net Core 2 WebApi + Postman
date: 2019-08-23 15:05:51
tags:
 - .Net Core
 - Postman
categories: 
 - .Net Core
---

## Basic
#### .Net Core part
1. 開啟 Web Api 專案
![Architecture](1.png)

2. 修改 **Startup.cs**
![Architecture](2.png)
~~~ bash
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;

namespace WebApi
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

            app.UseMvc(routes =>
            {
                routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
            });

            app.UseHttpsRedirection();
        }
    }
}
~~~

3. 新增 **HomeController.cs**
~~~ bash
using Microsoft.AspNetCore.Mvc;

namespace WebApi.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class HomeController : ControllerBase
    {
        public IActionResult Get()
        {
            return Ok("Get Success");
        }

        [HttpPost]
        public IActionResult Post()
        {
            return Ok("Post Success");
        }

        [HttpPut]
        public IActionResult Put()
        {
            return Ok("Put Success");
        }

        [HttpDelete]
        public IActionResult Delete()
        {
            return Ok("Delete Success");
        }
    }
}
~~~

#### Postman part
1. **Close SSL certificate verification**
![Architecture](3.png)

2. **Get**
![Architecture](4.png)

3. **Post**
![Architecture](5.png)

4. **Put**
![Architecture](6.png)

5. **Delete**
![Architecture](7.png)

## Route Attribute
#### .Net Core part
1. 新增 **Message.cs**
![Architecture](8.png)
~~~ bash
namespace WebApi.Models
{
    public class Message
    {
        public string Message1 { get; set; }
        public string Message2 { get; set; }
    }
}
~~~

2. 新增 **RouteAttributeController.cs**
~~~ bash
using Microsoft.AspNetCore.Mvc;
using WebApi.Models;

namespace WebApi.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class RouteAttributeController : ControllerBase
    {
        [Route("Index")]
        public IActionResult MyIndex()
        {
            return Ok("Index Success");
        }

        [Route("Index/{id:int}/{firstName")]
        public IActionResult MyIndex(int id)
        {
            return Ok($"Index {id} Success");
        }

        [Route("About")]
        [HttpPost]
        public IActionResult MyAbout()
        {
            return Ok("About Success");
        }

        [Route("About/{id:int}")]
        [HttpPost]
        public IActionResult MyAbout(int id)
        {
            return Ok($"About {id} Success");
        }

        [Route("ContactSingle")]
        [HttpPost]
        public IActionResult MyContact([FromBody]string message)
        {
            return Ok($"Contact {message} Success");
        }

        [Route("ContactClass")]
        [HttpPost]
        public IActionResult MyContact([FromBody]Message msg)
        {
            return Ok($"Contact {msg.Message1} {msg.Message2} Success");
        }
    }
}
~~~

#### Postman part
1. **Get Route("Index")**
![Architecture](9.png)

2. **Get [Route("Index/{id:int}")]**
![Architecture](10.png)

3. **Post [Route("About")]**
![Architecture](11.png)

4. **Post [Route("About/{id:int}")]**
![Architecture](12.png)

5. **Post [Route("ContactSingle")] FromBody**
![Architecture](13.png)

6. **Post [Route("ContactClass")] FromBody**
![Architecture](14.png)

## Http Verb
#### .Net Core part
1. 新增 **HttpVerbController.cs**
~~~ bash
using System.Collections.Generic;
using System.Linq;
using Microsoft.AspNetCore.Mvc;
using WebApi.Models;

namespace WebApi.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class HttpVerbController : ControllerBase
    {
        private List<Message> messages = new List<Message>
        {
            new Message { Message1 = "Message11", Message2 = "Message12" },
            new Message { Message1 = "Message21", Message2 = "Message22" },
            new Message { Message1 = "Message31", Message2 = "Message32" },
            new Message { Message1 = "Message41", Message2 = "Message42" },
            new Message { Message1 = "Message51", Message2 = "Message52" }
        };

        [HttpGet("getMessage")]
        public IActionResult GetListMessages()
        {
            return Ok(this.messages);
        }

        [HttpGet("getMessage/{id}")]
        public IActionResult GetListMessages(int id)
        {
            return Ok(this.messages.Where(o => o.Message2.Contains(id.ToString())).ToList());
        }

        [HttpPost("postMessage")]
        public IActionResult PostListMessages()
        {
            return Ok(this.messages);
        }

        [HttpPost("postMessage/{id}")]
        public IActionResult PostListMessages(int id)
        {
            return Ok(this.messages.Where(o => o.Message2.Contains(id.ToString())).ToList());
        }

        [HttpPost("postMessageSingleBody")]
        public IActionResult PostListMessages([FromBody]string message)
        {
            return Ok(this.messages.Where(o => o.Message2 == message).ToList());
        }

        [HttpPost("postMessageClassBody")]
        public IActionResult PostListMessages([FromBody]Message msg)
        {
            return Ok(this.messages.Where(o => o.Message2 == msg.Message2).ToList());
        }
    }
}
~~~

#### Postman part
1. **Get [HttpGet("getMessage")]**
![Architecture](15.png)

2. **Get [HttpGet("getMessage/{id}")]**
![Architecture](16.png)

3. **Post [HttpPost("postMessage")]**
![Architecture](17.png)

4. **Post [HttpPost("postMessage/{id}")]**
![Architecture](18.png)

5. **Post [HttpPost("postMessageSingleBody")] FromBody**
![Architecture](19.png)

6. **Post [HttpPost("postMessageClassBody")] FromBody**
![Architecture](20.png)

## Http Response Type
#### .Net Core part
1. 新增 **ResponseTypeController.cs**
~~~ bash
using Microsoft.AspNetCore.Mvc;

namespace WebApi.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class ResponseTypeController : ControllerBase
    {
        [HttpGet("getResponse/{id}")]
        public IActionResult GetResponse(int id)
        {
            switch (id)
            {
                case 1:
                    return Ok("Success");
                case 2:
                    return BadRequest("BadRequest");
                default:
                    return NotFound("NotFound");
            }
        }
    }
}
~~~

#### Postman part
1. **Status 200 Ok**
![Architecture](21.png)

2. **Status 400 Bad Request**
![Architecture](22.png)

3. **Status 404 Not Found**
![Architecture](23.png)

# Reference
[ASP.NET Core 中的路由至控制器動作](https://docs.microsoft.com/zh-tw/aspnet/core/mvc/controllers/routing?view=aspnetcore-2.2)
[使用 ASP.NET Core 建立 Web API](https://docs.microsoft.com/zh-tw/aspnet/core/web-api/?view=aspnetcore-2.2)