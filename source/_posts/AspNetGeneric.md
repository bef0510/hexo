---
title: C# 泛型
date: 2019-06-21 11:26:41
tags:
 - C#
 - Generic
categories: 
 - ASP.Net
---

# 泛型

#### 泛型類別
    public class Generic<T>
    {
        public T Name;
        public T NickName;
    }

    class Program
    {
        static void Main(string[] args)
        {
            Generic<string> generic = new Generic<string>();
            generic.Name = "Patrick";
            generic.NickName = "Pat";

            Console.WriteLine($"Name：{generic.Name}");
            Console.WriteLine($"NickName：{generic.NickName}");

            Console.ReadKey();
        }
    }

#### 泛型方法 使用型別參數宣告的方法
    public class Generic
    {
        public void Dump<T>(T s)
        {
            Console.WriteLine(s.ToString());
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Generic generic = new Generic();

            generic.Dump("Patrick");

            Console.ReadKey();
        }
    }

#### 泛型方法 + 條件約束
    public class Generic
    {
        public T Max<T>(T param1, T param2) where T : IComparable<T>
        {
            T result = param2;

            if (param1.CompareTo(param2) > 0)
            {
                result = param1;
            }

            return result;
        }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            Generic generic = new Generic();

            int resultInt = generic.Max<int>(10, 12);

            // 推論型別 省略 <float>
            float resultFloat = generic.Max(3.6F, 3.5F);

            // 推論型別 使用 var
            var resultObject = generic.Max("Gen", "Eric");

            Console.WriteLine(resultInt);
            Console.WriteLine(resultFloat);
            Console.WriteLine(resultObject);

            Console.ReadKey();
        }
    }

#### 泛型方法 + 條件約束 + 擴充方法
    public class Student
    {
        public string Name { get; set; }
        public int Age { get; set; }
        public DateTime Time { get; set; }
    }

    public static class StringExtension
    {
        public static void Dump<T>(this T s) => Console.WriteLine(s.ToString());

        public static void Dump<T>(this T s, string formate)
        {
            if (s.GetType() == typeof(DateTime))
            {
                DateTimeOffset.Parse(s.ToString()).DateTime.ToString(formate).Dump();
            }
        }

        public static void ObjectDump<T>(this T obj) where T : class, new()
        {
            Type type = obj.GetType();
            PropertyInfo[] props = type.GetProperties();
            foreach(var prop in props)
            {
                if(prop.GetIndexParameters().Length == 0)
                {
                    $"({prop.PropertyType.Name}) {prop.Name}：{prop.GetValue(obj)}".Dump();
                }
                else
                {
                    $"({prop.PropertyType.Name}) {prop.Name}：<Indexed>".Dump();
                }
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string name = "Patrick";
            int age = 26;
            DateTime time = DateTime.Now;
            Student student = new Student() { Name = name, Age = age, Time = time };

            name.Dump();
            age.Dump();
            time.Dump("yyyy-MM-dd HH:mm:ss");
            student.ObjectDump();

            Console.ReadKey();
        }
    }

#### 泛型委派
    class Program
    {
        public delegate T Payment<T, T1, T2>(T1 momey1, T2 momey2);

        static string Bill(int momey1, int momey2)
        {
            string result = "超商繳款";

            int total = momey1 + momey2;

            if (total > 30000)
            {
                result = "銀行繳款";
            }
            return result;
        }

        static bool Bill(float momey1, float momey2)
        {
            return (momey1 + momey2) > 30000;
        }

        static void Main(string[] args)
        {
            Payment<string, int, int> paymentType = new Payment<string, int, int>(Bill);
            string resultString = paymentType(10000, 16000);

            Payment<bool, float, float> isByBank = new Payment<bool, float, float>(Bill);
            bool resultBool = isByBank(16000.0F, 20000.0F);

            Console.WriteLine(resultString);
            Console.WriteLine(resultBool);

            Console.ReadKey();
        }
    }

#### 泛型委派 + 泛型類別
    class Stack<T>
    {
        T[] items;
        int index;

        public delegate void StackDelegate(T[] items, int index = -1);
    }

    class Program
    {
        public static void DoWork(float[] items, int index)
        {
            if (index >= 0)
            {
                if(index >= items.Length)
                {
                    index = items.Length - 1;
                }
                Console.WriteLine(items[index]);
            }
            else
            {
                foreach (float item in items)
                {
                    Console.WriteLine(item);
                }
            }
        }

        static void Main(string[] args)
        {
            float[] items = new float[] { 4.5F, 4.8F };
            Stack<float> s = new Stack<float>();
            Stack<float>.StackDelegate d = DoWork;
            d(items);

            Console.ReadKey();
        }
    }

# Reference
[泛型 (C# 程式設計手冊)](https://docs.microsoft.com/zh-tw/dotnet/csharp/programming-guide/generics/)
[型別參數的條件約束 (C# 程式設計手冊)](https://docs.microsoft.com/zh-tw/dotnet/csharp/programming-guide/generics/constraints-on-type-parameters)
[PropertyInfo.GetValue Method](https://docs.microsoft.com/zh-tw/dotnet/api/system.reflection.propertyinfo.getvalue?view=netframework-4.8)