---
title: MVC 跨網域要求 CORS
date: 2019-06-18 10:42:29
tags:
 - Ajax
 - CORS
categories: 
 - ASP.Net
 - MVC
 - C#
---

# Allow CORS

**Nuget** 載入 `Microsoft.AspNet.WebApi.Cors`
![Architecture](1.png)

**Enable CORS**
![Architecture](2.png)

#### 全域
![Architecture](3.png)

### 區域
#### Controller
![Architecture](4.png)

#### Action
![Architecture](5.png)

**Disable CORS**
### 區域
#### Controller
![Architecture](6.png)

#### Action
![Architecture](7.png)

### Custom Handle 
#### Add CorsHandle.cs
    public class CorsHandle : Attribute, ICorsPolicyProvider
    {
        private CorsPolicy corsPolicy;

        public CorsHandle()
        {
            this.corsPolicy = new CorsPolicy
            {
                AllowAnyHeader = true,
                AllowAnyMethod = true
            };

            this.corsPolicy.Origins.Add("http://localhost");
            this.corsPolicy.Origins.Add("http://www.google.com");
        }

        public Task<CorsPolicy> GetCorsPolicyAsync(HttpRequestMessage request, CancellationToken cancellationToken)
            => Task.FromResult(this.corsPolicy);
    }

#### Add CorsOnActionHandle.cs
    public class CorsOnActionHandle : ActionFilterAttribute
    {
        public override void OnActionExecuting(HttpActionContext actionContext)
        {
            List<string> allowDomain = new List<string>()
            {
                "http://localhost",
                "http://www.google.com"
            };

            string origin = actionContext.Request.Headers.GetValues("Origin").FirstOrDefault();

            bool isAllow = allowDomain.Contains(origin);

            if (!isAllow)
            {
                UnauthorizedObject result = new UnauthorizedObject()
                {
                    code = "401",
                    message = "domain is not allow"
                };

                actionContext.Response = actionContext.Request.CreateResponse(HttpStatusCode.Unauthorized, result);
            }
        }

        public class UnauthorizedObject
        {
            public string code { get; set; }
            public string message { get; set; }
        }
    }

#### Modify Controller or Action filter
![Architecture](8.png)

# Reference
[Enable cross-origin requests in ASP.NET Web API 2](https://docs.microsoft.com/en-us/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api)