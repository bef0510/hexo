---
title: C# Delegate、Action、Func、Predicate
date: 2019-06-10 15:24:24
tags:
 - delegate
categories: 
 - ASP.Net
 - C#
---

# 委派 Delegate、Action、Func、Predicate

### 使用 **Delegate**
1. 使用 delegate
2. 定義委派型別的 signature 的方法

#### **Delegate**
    class Program
    {
        private delegate void Payment();

        static void Main(string[] args)
        {
            Payment payment = new Payment(Bill);
            payment();

            Console.ReadKey();
        }

        public static void Bill()
        {
            Console.WriteLine("繳款方式：便利商店");
        }
    }

# Action

### 改用 **Action**，無參數
1. 省去定義方法
2. `Action` 封裝沒有參數且沒有傳回值的方法

#### **Action**
    class Program
    {
        static void Main(string[] args)
        {
            Action payment = new Action(Bill);
            payment();

            Console.ReadKey();
        }

        public static void Bill() => Console.WriteLine("繳款方式：便利商店");
    }

### 改用 **Action**，有參數
1. `Action<T>` 封裝至多 16 個參數的方法，並且不會傳回值

#### **Action**
    class Program
    {
        static void Main(string[] args)
        {
            Action<string> payment = new Action<string>(Bill);
            payment("便利商店");

            Console.ReadKey();
        }

        public static void Bill(string place) => Console.WriteLine($"繳款方式：{place}");
    }

#### **Action** 匿名委派
    class Program
    {
        static void Main(string[] args)
        {
            Action<string> payment = delegate (string place)
            {
                Console.WriteLine($"繳款方式：{place}");
            };
            payment("便利商店");

            Console.ReadKey();
        }
    }

#### **Action** Lambda
    class Program
    {
        static void Main(string[] args)
        {
            Action<string> payment = (string place) => Console.WriteLine($"繳款方式：{place}");

            payment("便利商店");

            Console.ReadKey();
        }
    }

# Func
### 改用 **Func**，無參數
1. `Func<TResult>` 封裝沒有參數並傳回 TResult 參數所指定之型別值的方法

#### **Func**
    class Program
    {
        static void Main(string[] args)
        {
            Func<string> payment = new Func<string>(Bill);
            Console.WriteLine(payment());

            Console.ReadKey();
        }

        public static string Bill()
        {
            return $"繳款方式：便利商店";
        }
    }

### 改用 **Func**，有參數
1. `Func<T, TResult>` 封裝具至多 16 個參數的方法，並傳回由 TResult 參數指定之類型的值

#### **Func**
    class Program
    {
        static void Main(string[] args)
        {
            Func<string, string> payment = new Func<string, string>(Bill);
            Console.WriteLine(payment("CVS"));

            Console.ReadKey();
        }

        public static string Bill(string type)
        {
            string place = string.Empty;
            switch (type)
            {
                case "CVS":
                    place = "便利商店";
                    break;
                case "POST":
                    place = "郵局轉帳";
                    break;
                case "BANK":
                    place = "銀行轉帳";
                    break;
                default:
                    place = "逾期未繳";
                    break;
            }
            return $"繳款方式：{place}";
        }
    }

#### **Func** 匿名委派
    class Program
    {
        static void Main(string[] args)
        {
            Func<string, string> payment = delegate (string type)
            {
                string place = string.Empty;
                switch (type)
                {
                    case "CVS":
                        place = "便利商店";
                        break;
                    case "POST":
                        place = "郵局轉帳";
                        break;
                    case "BANK":
                        place = "銀行轉帳";
                        break;
                    default:
                        place = "逾期未繳";
                        break;
                }
                return $"繳款方式：{place}";
            };
            Console.WriteLine(payment("CVS"));

            Console.ReadKey();
        }
    }

#### **Func** Lambda
    class Program
    {
        static void Main(string[] args)
        {
            Func<string, string> payment = (string type) =>
            {
                string place = string.Empty;
                switch (type)
                {
                    case "CVS":
                        place = "便利商店";
                        break;
                    case "POST":
                        place = "郵局轉帳";
                        break;
                    case "BANK":
                        place = "銀行轉帳";
                        break;
                    default:
                        place = "逾期未繳";
                        break;
                }
                return $"繳款方式：{place}";
            };
            Console.WriteLine(payment("CVS"));

            Console.ReadKey();
        }
    }

### **Predicate**
1. 等同於 `Func<T, bool>`

#### **Predicate**
    class Program
    {
        static void Main(string[] args)
        {
            Predicate<string> payment = new Predicate<string>(Bill);
            string isWay = payment("CVS") ? "是" : "否";
            Console.WriteLine($"是否有此繳費方式：{isWay}");

            Console.ReadKey();
        }

        public static bool Bill(string type)
        {
            List<string> lists = new List<string>() { "CVS", "POST", "BANK" };
            
            return lists.Where(o=>o == type).ToList().Count > 0;
        }
    }

#### **Predicate** Lambda
    class Program
    {
        static void Main(string[] args)
        {
            Predicate<string> payment = (string type) =>
            {
                List<string> lists = new List<string>() { "CVS", "POST", "BANK" };

                return lists.Where(o => o == type).ToList().Count > 0;
            };
            string isWay = payment("CVS") ? "是" : "否";

            Console.WriteLine($"是否有此繳費方式：{isWay}");

            Console.ReadKey();
        }
    }

# Reference
[delegate (C# 參考)](https://docs.microsoft.com/zh-tw/dotnet/csharp/language-reference/keywords/delegate)
[Action<T> Delegate](https://docs.microsoft.com/zh-tw/dotnet/api/system.action-1?view=netframework-4.8)
[Func<T,TResult> Delegate](https://docs.microsoft.com/zh-tw/dotnet/api/system.func-2?view=netframework-4.8)
[Predicate<T> Delegate](https://docs.microsoft.com/zh-tw/dotnet/api/system.predicate-1?view=netframework-4.8)