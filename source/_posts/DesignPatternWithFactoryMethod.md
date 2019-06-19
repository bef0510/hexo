---
title: 創建型模式 - 工廠方法模式
date: 2019-05-13 16:28:42
tags:
 - C#
 - Creational
categories: 
 - Design Pattern
---

# 工廠方法模式
工廠方法模式定義了一個創建對象的接口，但是讓子類來決定具體創建的是哪一個對象。工廠方法讓一個類延遲實例化，直到子類的出現
![Architecture](1.png)

#### 建立一個抽象類別 產品 `BubbleTea`
    public abstract class BubbleTea
    {
        public string Name { get; protected set; }
        public string Tea { get; protected set; }
        public string Sugar { get; protected set; }
        public string Tapioca { get; protected set; }

        public List<string> Toppings = new List<string>();

        public void Prepare()
        {
            Console.WriteLine($"Preparing: {Name}");
            Console.WriteLine($"Tea: {Tea}");
            Console.WriteLine($"Sugar: {Sugar}");
            Console.WriteLine($"Tapioca: {Tapioca}");
            Console.WriteLine("Adding toppings: ");
            Toppings.ForEach(x => Console.WriteLine($"  {x}"));
        }

        public void Shake() => Console.WriteLine("Shake for 5 times");

        public void Bag() => Console.WriteLine("Reusable shopping bag");
    }

#### 建立 類別 產品 `TPPearlMilkTea` 繼承 `BubbleTea`
    public class TPPearlMilkTea : BubbleTea
    {
        public TPPearlMilkTea()
        {
            Name = "TaiPei Bubble Tea";
            Sugar = "Sucrose Sugar";
            Tapioca = "Cassava Tapioca";
            Toppings.Add("Coconut Jelly");
            Toppings.Add("Grass Jelly");
        }
    }

    public class TPCaramelMilkTea : BubbleTea
    {
        public TPCaramelMilkTea()
        {
            Name = "TaiPei Caramel Milk Tea";
            Sugar = "Fructose Sugar";
            Tapioca = "Potato Tapioca";
        }
    }

    public class HLPearlMilkTea : BubbleTea
    {
        public HLPearlMilkTea()
        {
            Name = "HuaLien Bubble Tea";
            Sugar = "Sucrose Sugar";
            Tapioca = "Cassava Tapioca";
            Toppings.Add("Coconut Jelly");
            Toppings.Add("Grass Jelly");
        }
    }

    public class HLCaramelMilkTea : BubbleTea
    {
        public HLCaramelMilkTea()
        {
            Name = "HuaLien Caramel Milk Tea";
            Sugar = "Fructose Sugar";
            Tapioca = "Potato Tapioca";
        }
    }

#### 建立一個抽象類別 商店 `BubbleTeaStore`
    public abstract class BubbleTeaStore
    {
        public BubbleTea OrderBubbleTea(string type)
        {
            BubbleTea bubbleTea = CreateBubbleTea(type);
            bubbleTea.Prepare();
            bubbleTea.Shake();
            bubbleTea.Bag();
            return bubbleTea;
        }
        protected abstract BubbleTea CreateBubbleTea(string type);
    }

#### 建立 類別 各地區商店 `TaiPeiBubbleTeaStore` 繼承 `BubbleTeaStore`
    public class TaiPeiBubbleTeaStore : BubbleTeaStore
    {
        protected override BubbleTea CreateBubbleTea(string type)
        {
            BubbleTea bubbleTea = null;
            switch (type)
            {
                case "pearl":
                    bubbleTea = new TPPearlMilkTea();
                    break;
                case "caramel":
                    bubbleTea = new TPCaramelMilkTea();
                    break;
            }
            return bubbleTea;
        }
    }

    public class HuaLienBubbleTeaStore : BubbleTeaStore
    {
        protected override BubbleTea CreateBubbleTea(string type)
        {
            BubbleTea bubbleTea = null;
            switch (type)
            {
                case "pearl":
                    bubbleTea = new HLPearlMilkTea();
                    break;
                case "caramel":
                    bubbleTea = new HLCaramelMilkTea();
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
            Console.WriteLine($"Ordered a {softDrinks.Name} in TaiPei");

            Console.WriteLine("-----------------------------------------------------------");

            var softDrinks2 = hlStore.OrderBubbleTea("caramel");
            Console.WriteLine($"Ordered a {softDrinks2.Name} in HuaLien");

            Console.ReadKey();
        }
    }

#### 適用場景
1. 客戶端不需要知道具體產品類的類名，只需要知道所對應的工廠即可
2. 對於抽象工廠類只需要提供一個創建產品的接口，而由其子類來確定具體要創建的對象，利用面向對象的多態性和裡氏代換原則，在程序運行時，子類對象將覆蓋父類對象，從而使得系統更容易擴展
3. 將創建對象的任務委託給多個工廠子類中的某一個，客戶端在使用時可以無須關心是哪一個工廠子類創建產品子類，需要時再動態指定，可將具體工廠類的類名存儲在配置文件或數據庫中

#### 優點
1. 新增一種產品時，只需要增加相應的具體產品類和相應的工廠子類即可
2. 每個具體工廠類只負責創建對應的產品
3. 不使用靜態工廠方法，可以形成基於繼承的等級結構

#### 缺點
1. 添加新產品時，除了增加新產品類外，還要提供與之對應的具體工廠類，系統類的個數將成對增加，在一定程度上增加了系統的複雜度;同時，有更多的類需要編譯和運行，會給系統帶來一些額外的開銷
2. 由於考慮到系統的可擴展性，需要引入抽象層，在客戶端代碼中均使用抽象層進行定義，增加了系統的抽象性和理解難度，且在實現時可能需要用到DOM，反射等技術，增加了系統的實現難度
3. 雖然保證了工廠方法內的對修改關閉，但對於使用工廠方法的類，如果要更換另外一種產品，仍然需要修改實例化的具體工廠類
4. 一個具體工廠只能創建一種具體產品

# Reference
[維基百科-工廠方法](https://zh.wikipedia.org/wiki/%E5%B7%A5%E5%8E%82%E6%96%B9%E6%B3%95)
[和工厂方法设计模式](https://www.cnblogs.com/cgzl/p/8760250.html)
[設計模式 - 工廠方法及抽象工廠](https://blog.techbridge.cc/2017/05/22/factory-method-and-abstract-factory/)