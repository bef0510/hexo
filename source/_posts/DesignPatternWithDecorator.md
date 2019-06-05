---
title: 結構型模式 - 裝飾模式
date: 2019-06-05 09:33:24
tags:
 - Structural
categories: 
 - Design Pattern
---

# 裝飾模式
動態的對某個對象進行擴展，裝飾器是除了繼承之外的另外一種為對象擴展功能的方法
![Architecture](1.png)

#### 建立一個 抽象類別 `SetMenu`
    public abstract class SetMenu
    {
        public virtual string Description { get; protected set; } = "Unknown";

        public abstract double Cost();
    }

#### 建立一個 抽象類別 `CondimentDecorator` 繼承 `SetMenu`
    public abstract class CondimentDecorator : SetMenu
    {
        public abstract override string Description { get; }
    }

#### 建立一個 類別 `Hanberger` 繼承 `SetMenu`
    public class Hanberger : SetMenu
    {
        public Hanberger()
        {
            base.Description = "Hanberger";
        }

        public override double Cost()
        {
            return 50;
        }
    }

#### 建立一個 類別 `Bago` 繼承 `SetMenu`
    public class Bago : SetMenu
    {
        public Bago()
        {
            base.Description = "Bago";
        }

        public override double Cost()
        {
            return 40;
        }
    }

#### 建立一個 類別 `FrenchFries` 繼承 `CondimentDecorator`
    public class FrenchFries : CondimentDecorator
    {
        private readonly SetMenu setMenu;

        public FrenchFries(SetMenu setMenu)
        {
            this.setMenu = setMenu;
        }

        public override string Description => this.setMenu.Description;

        public override double Cost()
        {
            return 30 + this.setMenu.Cost();
        }
    }

#### 建立一個 類別 `HashBrowns` 繼承 `CondimentDecorator`
    public class HashBrowns : CondimentDecorator
    {
        private readonly SetMenu setMenu;

        public HashBrowns(SetMenu setMenu)
        {
            this.setMenu = setMenu;
        }

        public override string Description => this.setMenu.Description;

        public override double Cost()
        {
            return 35 + this.setMenu.Cost();
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            SetMenu set1 = new Hanberger();
            Console.WriteLine($"{set1.Description} $ {set1.Cost()}");

            SetMenu set2 = new Bago();
            set2 = new FrenchFries(set2);
            set2 = new HashBrowns(set2);
            Console.WriteLine($"{set2.Description} $ {set2.Cost()}");

            Console.ReadKey();
        }
    }

#### 適用場景
1. 增加由一些基本功能的排列組合而產生的非常大量的功能
2. 擴展一個類的功能或給一個類增加附加責任

#### 優點
1. 有很好的可擴展性
2. 使用不同的具體裝飾的排列組合，可以做出很多不同行為的組合

#### 缺點
1. 會出現許多類別，如過度使用程式會變得很複雜

# Reference
[裝飾者模式筆記](https://dotblogs.com.tw/pin0513/archive/2010/01/04/12779.aspx)
[使用C# (.NET Core) 实现装饰模式 (Decorator Pattern) 并介绍 .NET/Core的Stream](https://www.cnblogs.com/cgzl/p/8697949.html)