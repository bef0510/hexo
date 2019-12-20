---
title: .Net Core 3 Dapper + 泛型 取 Sql 資料
date: 2019-12-20 14:52:33
tags:
 - .Net Core
 - Console
 - Dapper
 - Generic
categories: 
 - .Net Core
---

## .Net Core 3 Dapper + 泛型 取 Sql 資料

#### 下載套件
1. 組合鍵 **Ctrl+Shift+P**
2. 下載 **System.Data.SqlClient**
3. 下載 **Dapper**
4. 下載 **Microsoft.Extensions.Configuration.Json**

#### 新專案
1. **Create appsettings.json**
~~~ bash
{
    "ConnectionStrings": {
        "DefaultConnection": "Data Source=172.17.102.113;Initial Catalog=NewPortal;User ID=test555;Password=test555"
    },
    "Message": "It's appsettings.json"
}
~~~

2. **Create SqlParamterModel.cs under folder Models**
~~~ bash
namespace textConsole.Models
{
    public class SqlParamterModel
    {
        public string Name { get; set; }
        public object Value { get; set; }

        public SqlParamterModel(string name, object value)
		{
			this.Name = name;
			this.Value = value;
		}
    }
}
~~~

3. **Create EmployeeModel.cs under folder Models**
~~~ bash
namespace textConsole.Models
{
    public class EmployeeModel
    {
        public string DHID { get; set; }
        public string DHGUID { get; set; }
        public string DiscussName { get; set; }
        public string DiscussRemark { get; set; }
        public string DiscussAdmin { get; set; }
        public string DiscussOrder { get; set; }
        public string DiscussParentDHID { get; set; }
        public string DiscussType { get; set; }
        public string DiscussURL { get; set; }
        public string DiscussIconPath { get; set; }
        public string DiscussSite { get; set; }
        public string DiscussAttachSizeLimit { get; set; }
        public string AddDate { get; set; }
        public string AddEmpNo { get; set; }
        public string ModDate { get; set; }
        public string ModEmpNo { get; set; }
        public string ActiveStatus { get; set; }
        public string DiscussDisplayType { get; set; }
    }
}
~~~

4. **Create AppSettingHelper.cs under folder Extensions**
~~~ bash
using System.IO;
using Microsoft.Extensions.Configuration;

namespace textConsole.Extensions
{
    public static class AppSettingHelper
    {
        public static string DefaultConnection => ReadFromAppSettings().GetConnectionString("DefaultConnection");

        public static IConfigurationRoot ReadFromAppSettings()
        {
            return new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json", optional:true,reloadOnChange: true)
                .Build();
        }
    }
}
~~~

5. **Create SqlHelper.cs under folder Extensions**
~~~ bash
using System;
using System.Collections.Generic;
using System.Data.SqlClient;
using System.Threading.Tasks;
using Dapper;
using textConsole.Models;

namespace textConsole.Extensions
{
    public static class SqlHelper
    {
        public static async Task<IEnumerable<T>> Execute<T>(string connString, string queryString)
        {
            return await SqlHelper.Execute<T>(connString, queryString, new SqlParamterModel[] { });
        }

        public static async Task<IEnumerable<T>> Execute<T>(string connString, string queryString, SqlParamterModel[] parameters)
        {
            using (var conn = new SqlConnection(connString))
            {
                DynamicParameters dynamicParameters = new DynamicParameters();

                foreach (var item in parameters)
                {
                    dynamicParameters.Add(item.Name, item.Value);
                }

                try
                {
                    return await conn.QueryAsync<T>(queryString, dynamicParameters);
                }
                catch(SqlException ex)
                {
                    Console.WriteLine(ex.StackTrace);
                    return null;
                }
                finally
                {
                    conn.Close();
                }
            }
        }
    }
}
~~~

6. **Create EmployeeRepository.cs under folder Repositories**
~~~ bash
using System.Collections.Generic;
using System.Threading.Tasks;
using textConsole.Extensions;
using textConsole.Models;

namespace textConsole.Repositories
{
    public class EmployeeRepository
    {
        public async Task<IEnumerable<EmployeeModel>> GetEmployee()
        {
            string connString = AppSettingHelper.DefaultConnection;
            string queryString = "SELECT top 10 * FROM BBS_Discuss";
   
            return await SqlHelper.Execute<EmployeeModel>(connString, queryString);
        }

        public async Task<IEnumerable<EmployeeModel>> GetEmployeeByDiscuss(string type, int parent)
        {
            string connString = AppSettingHelper.DefaultConnection;
            string queryString = "SELECT top 10 * FROM BBS_Discuss WHERE DiscussType = @DiscussType AND DiscussParentDHID = @DiscussParentDHID";
   
            return await SqlHelper.Execute<EmployeeModel>(connString, queryString,
                new SqlParamterModel[]{
                    new SqlParamterModel("DiscussType", type),
                    new SqlParamterModel("DiscussParentDHID", parent)
                }
            );
        }
    }
}
~~~

7. **Modify Program.cs**
~~~ bash
using System;
using System.Linq;
using System.Text.Json;
using System.Threading.Tasks;
using textConsole.Repositories;

namespace textConsole
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var allEmp = await new EmployeeRepository().GetEmployee();

            Console.WriteLine(JsonSerializer.Serialize(allEmp));

            var aEmp = await new EmployeeRepository().GetEmployeeByDiscuss("D", 28);

            Console.WriteLine(JsonSerializer.Serialize(aEmp));
            Console.WriteLine(aEmp.ToList().Count);
        }
    }
}
~~~

# Reference
[Dapper In .NET Core – Part 1 – The What/Why/Who](https://dotnetcoretutorials.com/2019/08/03/dapper-in-net-core-part-1-the-what-why-who/)