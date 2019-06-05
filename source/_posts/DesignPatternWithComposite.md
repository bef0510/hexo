---
title: 結構型模式 - 組合模式
date: 2019-05-27 09:36:00
tags:
 - Structural
categories: 
 - Design Pattern
---

# 組合模式
樹狀結構的物件，每個物件有相同的介面
![Architecture](1.png)

#### 建立一個 抽象類別 `Organization`
    public abstract class Organization
    {
        protected string name;
        protected string desc;
        protected List<Organization> organizations = new List<Organization>();

        public Organization(string name, string desc)
        {
            this.name = name;
            this.desc = desc;
        }

        public virtual void AddDepartment(Organization organization) => this.organizations.Add(organization);

        public virtual void RemoveDepartment(Organization organization) => this.organizations.Remove(organization);

        public virtual List<Organization> GetAllDepartment(Organization organization) => this.organizations;

        public abstract string GetString();
    }

#### 建立一個 類別 `Company` 繼承 `Organization`
    public class Company : Organization
    {
        public Company(string name, string desc) : base(name, desc)
        {
        }

        public override string GetString()
        {
            StringBuilder sb = new StringBuilder($"\n【公司】：{this.name} 說明： {this.desc} \n");

            foreach (var organization in this.organizations)
            {
                sb.Append(organization.GetString()).Append("\n");
            }

            return sb.ToString();
        }
    }

#### 建立一個 類別 `Dept` 繼承 `Organization`
    public class Dept : Organization
    {
        public Dept(string name, string desc) : base(name, desc)
        {
        }

        public override string GetString() => $" - 部門：{this.name} 說明： {this.desc}";
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            Organization company = new Company("HQ", "總公司");

            Organization hrDept = new Dept("HR", "人力資源部");
            Organization accountDept = new Dept("Account", "會計部");
            Organization financeDept = new Dept("Finance", "財務部");

            company.AddDepartment(hrDept);
            company.AddDepartment(accountDept);
            company.AddDepartment(financeDept);

            Console.WriteLine(company.GetString());

            Console.ReadKey();
        }
    }

#### 適用場景
1. 需分層管理多物件時

#### 優點
1. 容易新增組件
2. 介面與功能分離，更具移植性

#### 缺點
1. 不能依靠編譯期的類型約束來實現，必須在運行期間動態檢測
2. 元件更名不易

# Reference
[MBA智库百科-組合模式](https://wiki.mbalib.com/wiki/%E7%BB%84%E5%90%88%E6%A8%A1%E5%BC%8F)
[設計模式（C#）——組合模式](https://www.itread01.com/content/1546887006.html)