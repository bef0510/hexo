---
title: .Net Core 3 + Swagger
date: 2019-11-29 09:35:58
tags:
 - .Net Core
 - Swagger
categories: 
 - .Net Core
---

## .Net Core 3 + Swagger
1. **Nuget Install Swashbuckle.AspNetCore**
![Architecture](1.png)

#### Basic
1. **Modify Startup.cs**
~~~ bash
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();

    // Register the Swagger generator, defining 1 or more Swagger documents.
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc(name: "v1", new OpenApiInfo { Title = "test api", Version = "v1" });
    });
}

//.AddSecurityDefinition(string name, OpenApiSecurityScheme securityScheme)
// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    // Enable middleware to serve generated Swagger as a JSON endpoint.
    app.UseSwagger();

    // Enable middleware to serve swagger-ui (HTML, JS, CSS, etc.),
    // specifying the Swagger JSON endpoint.
    app.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint(url: "/swagger/v1/swagger.json", name: "v1");
    });

    app.UseHttpsRedirection();

    app.UseStaticFiles();       // 必須在 app.UseRouting() 之前被呼叫

    app.UseRouting();

    app.UseCors();              // 必須在所有會用到的 CORS 的中介層之前被呼叫

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
~~~
![Architecture](2.png)

#### Add Header
1. **Modify Startup.cs**
~~~ bash
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                var azureadoptions = new AzureADOptions();
                Configuration.Bind("AzureAD", azureadoptions);
                options.Authority = $"{azureadoptions.Instance}{azureadoptions.TenantId}/v2.0";
                options.TokenValidationParameters = new Microsoft.IdentityModel.Tokens.TokenValidationParameters
                {
                    ValidAudience = $"{azureadoptions.ClientId}",
                    ValidIssuer = $"{azureadoptions.Instance}{azureadoptions.TenantId}/v2.0"
                };
            });

    services.AddControllers();

    // Register the Swagger generator, defining 1 or more Swagger documents.
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc(name: "v1", new OpenApiInfo { Title = "test api", Version = "v1" });
        
        c.AddSecurityDefinition("Bearer", new OpenApiSecurityScheme
        {
            Description = "JWT Authorization header using the Bearer scheme.",
            Name = "Authorization",
            In = ParameterLocation.Header,
            Type = SecuritySchemeType.ApiKey,
            Scheme = "Bearer"
        });

        c.AddSecurityRequirement(new OpenApiSecurityRequirement()
        {
            {
                new OpenApiSecurityScheme
                {
                    Reference = new OpenApiReference
                    {
                        Type = ReferenceType.SecurityScheme,
                        Id = "Bearer"
                    },
                    Scheme = "oauth2",
                    Name = "Bearer",
                    In = ParameterLocation.Header,
                },
                new List<string>()
            }
        });
    });
}
~~~

2. **Modify Controller.cs**
~~~ bash
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Logging;

namespace testWebapi.Controllers
{
    [ApiController]
    [Route("[controller]")]
    public class WeatherForecastController : ControllerBase
    {
        private static readonly string[] Summaries = new[]
        {
            "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
        };

        private readonly ILogger<WeatherForecastController> _logger;

        public WeatherForecastController(ILogger<WeatherForecastController> logger)
        {
            _logger = logger;
        }

        /// <summary>
        /// 查詢
        /// </summary>
        /// <param name="orderCode">單號</param>
        /// <param name="orderName">名稱</param>
        /// <returns></returns>
        [HttpGet("{orderCode}/{orderName}")]
        public IEnumerable<WeatherForecast> Get(string orderCode, string orderName)
        {
            var rng = new Random();
            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = DateTime.Now.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = Summaries[rng.Next(Summaries.Length)]
            })
            .ToArray();
        }

        [HttpPost]
        public OrderModel Post([FromBody]OrderModel model)
        {
            return model;
        }
    }

    public class OrderModel
    {
        /// <summary>
        /// 訂單編號
        /// </summary>
        public string OrderCode { get; set; }

        /// <summary>
        /// 訂單金額
        /// </summary>
        public decimal OrderAmount { get; set; }

    }
}
~~~
![Architecture](3.png)

# Reference
[Swashbuckle 與 ASP.NET Core 使用者入門](https://docs.microsoft.com/zh-tw/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-3.0&tabs=visual-studio)