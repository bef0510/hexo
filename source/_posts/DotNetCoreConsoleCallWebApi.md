---
title: .Net Core 3 Console Call WebApi
date: 2019-11-29 17:40:58
tags:
 - .Net Core
 - Console
categories: 
 - .Net Core
---

## .Net Core 3 Console Call WebApi
1. **Create Console**
~~~ bash
dotnet new console
~~~

2. **Program.cs**
~~~ bash
using System;
using System.Net.Http;
using System.Threading.Tasks;

namespace textConsole
{
    class Program
    {
        static async Task Main(string[] args)
        {
            await GetWebApi();
        }

        static async Task GetWebApi()
        {
            var client = new HttpClient(
                // 防止 The SSL connection could not be established, see inner exception.
                new HttpClientHandler
                {
                    ServerCertificateCustomValidationCallback = HttpClientHandler.DangerousAcceptAnyServerCertificateValidator
                }
            );

            try
            {
                const string url = "https://localhost:5001/WeatherForecast/1/1";

                // 加上 Header
                client.DefaultRequestHeaders.Add("Authorization", "Bearer 1eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IkJCOENlRlZxeWFHckdOdWVoSklpTDRkZmp6dyJ9.eyJhdWQiOiIzMTBjODc5ZC1mYTEwLTRkN2UtYmUxYi0zZWZlNmEzOGEwNGMiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vODEzNWUzM2QtMzZkYi00OTA2LWFmNWUtNmE3ZGYyOThlZDk2L3YyLjAiLCJpYXQiOjE1NzUwMTg0MzcsIm5iZiI6MTU3NTAxODQzNywiZXhwIjoxNTc1MDIyMzM3LCJhaW8iOiJBVFFBeS84TkFBQUFFM0xnbDZYemFXSUp0bSt4UjkxTWYvUDlwaTFpbVJjZXZFdWpYdkUwSVZkWW95UUs3T2wxTUsxakwrUlJDY3h1IiwiYXpwIjoiMzEwYzg3OWQtZmExMC00ZDdlLWJlMWItM2VmZTZhMzhhMDRjIiwiYXpwYWNyIjoiMCIsIm9pZCI6ImE4MTI3ZjYxLTJhMjMtNDg3ZC05NjcyLWMxM2Y1MmUwZTFiZiIsInByZWZlcnJlZF91c2VybmFtZSI6IkRhcnJlbi5XYW5nQGRsaW5rY29ycC5jb20iLCJzY3AiOiJhY2Nlc3NfYXNfdXNlciIsInN1YiI6IjlXYXpXSldZb08wanY4NVVieDBuSGhUX2xMRzlDZVRXU200b2thMEFHY0kiLCJ0aWQiOiI4MTM1ZTMzZC0zNmRiLTQ5MDYtYWY1ZS02YTdkZjI5OGVkOTYiLCJ1dGkiOiJRWG5KTTlPZjJFR0FoOUJ6cWc0VkFRIiwidmVyIjoiMi4wIiwibWFpbG5pY2tuYW1lIjoiMDg3MjgifQ.J-2Ur5FAe8U4LzjFys56AZZE8VG4Ms43PAasaq-Vs3orwa6mUwzwWvLcEhuYiBQhNupi3BZGpd9OPDubjdFqrZEo6vlNW7VzUPpL2TUQ777SsdsKtcDdII2MUGV8Qhxukt9xMBnljR-WhInt-fqJdAW4ttNUZrF4oX5hCis-ERvhIMPekQV14hJjSvM9bl83HuIi5V4XhSSoUKpxPoGSGzn17l3T7PeqY4lJTEiKTQhBJExPlZvqGL3nEL1QD54JUAl5sigKrlH1h21KJaX7h6eX-ix1K26w0DrLWWq3QR92zjER1b2oGBIpd7hWiWlhaViAv4pFgtDlwXOP52hkcA");
                
                HttpResponseMessage response = await client.GetAsync(url);

                if (response.IsSuccessStatusCode)
                {
                    response.EnsureSuccessStatusCode();
                    string responseBody = await response.Content.ReadAsStringAsync();

                    Console.WriteLine(responseBody);
                }
                else
                {
                    string responseBody = await response.Content.ReadAsStringAsync();

                    Console.WriteLine(responseBody);
                }
            }
            catch (HttpRequestException ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    }
}
~~~

# Reference
[C# HttpClient tutorial](http://zetcode.com/csharp/httpclient/)