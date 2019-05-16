---
title: 創建型模式 - 抽象工廠模式
date: 2019-05-14 15:47:28
tags:
 - Creational
categories: 
 - Design Pattern
---

# 抽象工廠模式
抽象工廠模式的實質是「提供介面，建立一系列相關或獨立的物件，而不指定這些物件的具體類
![Architecture](1.png)

#### 建立一個 原料 介面 `IBubbleTeaIngredientFactory`
    public interface IIngredient
    {
        string Name { get; }
    }

    public interface ITea : IIngredient { }

    public interface ITapioca : IIngredient { }

    public interface ISugar : IIngredient { }

    public interface IBubbleTeaIngredientFactory
    {
        ITea CreateTea();
        ITapioca CreateTapioca();
        ISugar CreateSugar();
    }

#### 建立類別 實作原料
    public class BlackTea : ITea
    {
        public string Name => "Black Tea";
    }

    public class GreenTea : ITea
    {
        public string Name => "Green Tea";
    }

    public class SucroseSugar : ISugar
    {
        public string Name => "Sucrose Sugar";
    }

    public class FructoseSugar : ISugar
    {
        public string Name => "Fructose Sugar";
    }

    public class CassavaTapioca : ITapioca
    {
        public string Name => "Cassava Tapioca";
    }

    public class PotatoTapioca : ITapioca
    {
        public string Name => "Potato Tapioca";
    }

#### 建立一個抽象類別 `BubbleTea`
    public abstract class BubbleTea
    {
        public string Name { get; set; }
        public ITea Tea { get; protected set; }
        public ISugar Sugar { get; protected set; }
        public ITapioca Tapioca { get; protected set; }

        public abstract void Prepare();

        public void Shake() => Console.WriteLine("Shake for 5 times");

        public void Bag() => Console.WriteLine("Reusable shopping bag");
    }

#### 建立一個類別 實作各式 `BubbleTea`
    public class PearlMilkTea : BubbleTea
    {
        private readonly IBubbleTeaIngredientFactory bubbleTeaIngredientFactory;

        public PearlMilkTea(IBubbleTeaIngredientFactory bubbleTeaIngredientFactory)
        {
            this.bubbleTeaIngredientFactory = bubbleTeaIngredientFactory;
        }

        public override void Prepare()
        {
            Console.WriteLine($"Preparing: {Name}");
            Tea = this.bubbleTeaIngredientFactory.CreateTea();
            Sugar = this.bubbleTeaIngredientFactory.CreateSugar();
            Tapioca = this.bubbleTeaIngredientFactory.CreateTapioca();
            Console.WriteLine($"    {Tea.Name}");
            Console.WriteLine($"    {Sugar.Name}");
            Console.WriteLine($"    {Tapioca.Name}");
        }
    }

    public class CaramelMilkTea : BubbleTea
    {
        private readonly IBubbleTeaIngredientFactory bubbleTeaIngredientFactory;

        public CaramelMilkTea(IBubbleTeaIngredientFactory bubbleTeaIngredientFactory)
        {
            this.bubbleTeaIngredientFactory = bubbleTeaIngredientFactory;
        }

        public override void Prepare()
        {
            Console.WriteLine($"Preparing: {Name}");
            Tea = this.bubbleTeaIngredientFactory.CreateTea();
            Sugar = this.bubbleTeaIngredientFactory.CreateSugar();
            Tapioca = this.bubbleTeaIngredientFactory.CreateTapioca();
            Console.WriteLine($"    {Tea.Name}");
            Console.WriteLine($"    {Sugar.Name}");
            Console.WriteLine($"    {Tapioca.Name}");
        }
    }

#### 建立一個類別 繼承 原料，實作 各地原料 `IBubbleTeaIngredientFactory`
    public class TaiPeiBubbleTeaIngredientFactory : IBubbleTeaIngredientFactory
    {
        public ITea CreateTea()
        {
            return new BlackTea();
        }

        public ISugar CreateSugar()
        {
            return new SucroseSugar();
        }

        public ITapioca CreateTapioca()
        {
            return new CassavaTapioca();
        }
    }

    public class HuaLienBubbleTeaIngredientFactory : IBubbleTeaIngredientFactory
    {
        public ITea CreateTea()
        {
            return new GreenTea();
        }

        public ISugar CreateSugar()
        {
            return new FructoseSugar();
        }

        public ITapioca CreateTapioca()
        {
            return new PotatoTapioca();
        }
    }

#### 建立一個抽象類別 `BubbleTeaStore`
    public abstract class BubbleTeaStore
    {
        public BubbleTea OrderBubbleTea(string type)
        {
            var bubbleTea = CreateBubbleTea(type);
            bubbleTea.Prepare();
            bubbleTea.Shake();
            bubbleTea.Bag();
            return bubbleTea;
        }

        protected abstract BubbleTea CreateBubbleTea(string type);
    }

#### 建立一個類別 實作 各地商店 `BubbleTeaStore`
    public class TaiPeiBubbleTeaStore : BubbleTeaStore
    {
        protected override BubbleTea CreateBubbleTea(string type)
        {
            var factory = new TaiPeiBubbleTeaIngredientFactory();
            BubbleTea bubbleTea = null;
            switch (type)
            {
                case "pearl":
                    bubbleTea = new PearlMilkTea(factory);
                    bubbleTea.Name = "TaiPei Bubble Tea";
                    break;
                case "caramel":
                    bubbleTea = new CaramelMilkTea(factory);
                    bubbleTea.Name = "TaiPei Caramel Milk Tea";
                    break;
            }
            return bubbleTea;
        }
    }

    public class HuaLienBubbleTeaStore : BubbleTeaStore
    {
        protected override BubbleTea CreateBubbleTea(string type)
        {
            var factory = new HuaLienBubbleTeaIngredientFactory();
            BubbleTea bubbleTea = null;
            switch (type)
            {
                case "pearl":
                    bubbleTea = new PearlMilkTea(factory);
                    bubbleTea.Name = "HuaLien Bubble Tea";
                    break;
                case "caramel":
                    bubbleTea = new CaramelMilkTea(factory);
                    bubbleTea.Name = "HuaLien Caramel Milk Tea";
                    break;
            }
            return bubbleTea;
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            var tpStore = new TaiPeiBubbleTeaStore();
            var hlStore = new HuaLienBubbleTeaStore();

            var softDrinks = tpStore.OrderBubbleTea("pearl");

            Console.WriteLine("-----------------------------------------------------------");

            var softDrinks2 = hlStore.OrderBubbleTea("caramel");

            Console.ReadKey();
        }
    }

#### 適用場景
1. 一個系統要獨立於它的產品的建立、組合和表示時
2. 一個系統要由多個產品系列中的一個來組態時
3. 需要強調一系列相關的產品物件的設計以便進行聯合使用時
4. 提供一個產品類別庫，而只想顯示它們的介面而不是實現時

#### 優點
1. 具體產品從客戶程式碼中被分離出來
2. 容易改變產品的系列
3. 將一個系列的產品族統一到一起建立

#### 缺點
1. 在產品族中擴充新的產品是很困難的，它需要修改抽象工廠的介面

# Reference
[維基百科-抽象工廠](https://zh.wikipedia.org/wiki/%E6%8A%BD%E8%B1%A1%E5%B7%A5%E5%8E%82)
[抽象工厂设计模式](https://www.cnblogs.com/cgzl/p/8776868.html)