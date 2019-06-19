---
title: C# 自訂擴充方法
date: 2019-06-19 14:08:01
tags:
 - Extension
 - C#
categories: 
 - ASP.Net
---

# 自訂擴充方法
1. 需宣告一個 **static class**
2. 第一個參數需加上 **this** 關鍵字

#### 建立自訂類別
    public static class StringExtension
    {
        public static string Reverse(this string s)
        {
            List<string> characters = s.Select(c => c.ToString()).ToList();

            characters.Reverse();

            return string.Join("", characters.ToArray());
        }

        public static bool IsNumeric(this string s)
        {
            double output;

            return double.TryParse(s, out output);
        }

        public static string ToRocDateString(this DateTime date, char separator)
            => $"{(date.Year - 1911)}{separator}{date.Month}{separator}{date.Day}";

        public static string GetName(this Account account)
            => account.Name;
    }

    public class Account
    {
        public string Name { get; set; }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("123456789".Reverse());

            Console.WriteLine("100".IsNumeric());

            Console.WriteLine(DateTime.Now.ToRocDateString('/'));

            Account account = new Account { Name = "Joey" };

            Console.WriteLine(account.GetName());

            Console.ReadKey();
        }
    }

# Reference
[擴充方法 (C# 程式設計手冊)](https://docs.microsoft.com/zh-tw/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)