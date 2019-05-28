---
title: C# 委派演變
date: 2019-05-28 10:14:46
tags:
 - delegate
categories: 
 - ASP.Net
---

# C# 委派
1. 使用 delegate 定義委派名稱、傳入參數及回傳值。
2. 定義委派型別的 signature 的方法。
3. 建立委派物件，並指定委派方法。
4. 透過委派物件執行委派方法。

#### 建立一個 類別 `GetEmployee`
    // 步驟 1: 定義委派型別
    public delegate bool Predicate(Employee employee);

    public class GetEmployee
    {
        private ArrayList Employees;

        public GetEmployee()
        {
            Employees = new ArrayList
            {
                new Employee() { Name = "Joey", Age = 31 },
                new Employee() { Name = "Chandler", Age = 41 },
                new Employee() { Name = "Monica", Age = 28 },
                new Employee() { Name = "Phoebe", Age = 26 }
            };
        }

        public ArrayList IsGreater(Predicate p)
        {
            ArrayList employees = new ArrayList();
            for (int i = 0; i< Employees.Count; i++)
            {
                Employee e = (Employee)Employees[i];

                // 步驟 4: 執行委派
                bool isMatch = p.Invoke(e);
                if (isMatch)
                {
                    employees.Add(e);
                }
            }

            return employees;
        }
    }

    public class Employee
    {

        private string name;

        public string Name
        {
            get { return this.name; }
            set { this.name = value; }
        }

        private int age;

        public int Age
        {
            get { return this.age; }
            set { this.age = value; }
        }
    }

#### C# 1.0 具名委派
    class Program
    {
        static void Main(string[] args)
        {
            GetEmployee employees = new GetEmployee();

            // 步驟 3: 建立委派物件（具名委派）
            Predicate predicate = new Predicate(FindGreater);

            ArrayList e = employees.IsGreater(predicate);

            for (int i = 0; i < e.Count; i++)
            {
                Console.WriteLine($"{((Employee)e[i]).Name} / {((Employee)e[i]).Age}");
            }

            Console.ReadKey();
        }

        // 步驟 2: 撰寫符合委派型別所宣告的委派方法
        static bool FindGreater(Employee e)
        {
            return e.Age > 30;
        }
    }

#### C# 2.0 匿名委派
    class Program
    {
        static void Main(string[] args)
        {
            GetEmployee employees = new GetEmployee();

            // 步驟 3: 建立委派物件（匿名委派），合併 步驟 2
            Predicate predicate = delegate (Employee emp)
            {
                return emp.Age > 30;
            };

            ArrayList e = employees.IsGreater(predicate);

            for (int i = 0; i < e.Count; i++)
            {
                Console.WriteLine($"{((Employee)e[i]).Name} / {((Employee)e[i]).Age}");
            }

            Console.ReadKey();
        }
    }

#### C# 3.0 Lambda
    class Program
    {
        static void Main(string[] args)
        {
            GetEmployee employees = new GetEmployee();

            // 步驟 3: 建立委派物件（Lambda），合併 步驟 2，簡化 delegate
            Predicate predicate = (Employee emp) =>
            {
                return emp.Age > 30;
            };

            ArrayList e = employees.IsGreater(predicate);

            for (int i = 0; i < e.Count; i++)
            {
                Console.WriteLine($"{((Employee)e[i]).Name} / {((Employee)e[i]).Age}");
            }

            Console.ReadKey();
        }
    }

# Reference
[C# 筆記：重訪委派－從 C# 1.0 到 2.0 到 3.0](https://www.huanlintalk.com/2009/01/delegate-revisited-csharp-1-to-2-to-3.html)