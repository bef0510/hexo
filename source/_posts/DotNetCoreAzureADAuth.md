---
title: .Net Core 3 AzureAD with JWT Auth
date: 2019-11-26 16:48:38
 - .Net Core
 - Azure
categories: 
 - .Net Core
---

## .Net Core 3 AzureAD with JWT Auth

1. **appsettings.json**
~~~ bash
"AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "TenantId": "8135e33d-36db-4906-af5e-6a7df298ed96",
    "ClientId": "310c879d-fa10-4d7e-be1b-3efe6a38a04c"
  },
  ...
~~~

2. **Startup.cs**
~~~ bash
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.AspNetCore.Authentication.AzureAD.UI;
using Microsoft.AspNetCore.Authentication.JwtBearer;

namespace testWebapi
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
        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.UseHttpsRedirection();

            app.UseStaticFiles();       // 必須在 app.UseRouting() 之前被呼叫

            app.UseRouting();

            app.UseCors();              // 必須在所有會用到的 CORS 的中介層之前被呼叫

            app.UseAuthentication();

            app.UseAuthorization();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllers();
            });
        }
    }
}
~~~

3. **WeatherForecastController.cs**
~~~ bash
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Logging;

namespace testWebapi.Controllers
{
    [Authorize]
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

        [HttpGet]
        public IEnumerable<WeatherForecast> Get()
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
    }
}
~~~

4. **Postman**
`Fail no token`
![Architecture](1.png)

`Success with token`
![Architecture](2.png)

# Reference
[Using JwtBearer Authentication in an API-only ASP.NET Core Project](https://wildermuth.com/2018/04/10/Using-JwtBearer-Authentication-in-an-API-only-ASP-NET-Core-Project)