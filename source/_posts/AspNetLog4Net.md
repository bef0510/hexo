---
title: log4net
date: 2019-06-18 14:17:02
tags:
 - log4net
categories: 
 - ASP.Net
 - C#
---

# log4net

#### **Nuget** 載入 `log4net`
![Architecture](1.png)

#### 加入 **log4net.config**
    <?xml version="1.0" encoding="utf-8" ?>
    <log4net>
        <appender name="DefaultAppender" type="log4net.Appender.RollingFileAppender">
            <file value="LogFiles/Root1/"/>　<!--存放log路徑-->
            <staticLogFileName value="false"/>
            <appendToFile value="true"/>
            <rollingStyle value="Date"/>
            <datePattern value="yyyy-MM-dd.lo\g"/>　<!--log檔的命名-->
            <layout type="log4net.Layout.PatternLayout">
            <!--內容格式-->
            <conversionPattern value="%-5p %date{yyyy/MM/dd HH:mm:ss} %-20c{1} %-20M %m%n" />
            </layout>
        </appender>
        <root>
            <level value="ALL"/>
            <appender-ref ref="DefaultAppender"/>
        </root>
    </log4net>

#### **Application_Start** 加入 **config** 路徑
![Architecture](2.png)

### **AOP** 方式寫入 **Log**
#### 加入 **LogAttribute.cs**
    public class LogAttribute : ActionFilterAttribute
    {
        public override void OnActionExecuting(HttpActionContext actionContext)
        {
            var instanceType = actionContext.ControllerContext.Controller.GetType();
            ILog log = LogManager.GetLogger("Web");
            string method = $"{instanceType.FullName}.{actionContext.ActionDescriptor.ActionName}";
            string attributeMethod = $"{method}.OnActionExecuting";

            log.Debug($"Method：{method}，AttributeMethod：{attributeMethod}，Message：Debug");
        }
    }

#### **Action Attribute** 引用
![Architecture](3.png)

# Reference
[[.NET] 輕鬆上手Log4net](https://yohey66.wordpress.com/2018/08/07/net-%E8%BC%95%E9%AC%86%E4%B8%8A%E6%89%8Blog4net/)