---
title: 創建型模式 - 原型模式
date: 2019-05-15 10:24:45
tags:
 - C#
 - Creational
categories: 
 - Design Pattern
---

# 原型模式
通過「複製」一個已經存在的實例來返回新的實例,而不是新建實例。被複製的實例就是我們所稱的「原型」，這個原型是可定製的
![Architecture](1.png)

原型模式其實就是從一個物件再建立另外一個可訂製的物件，而且不須知道任何建立的細節。 
其中有區分為「淺複製」與「深複製」 
`淺複製`：複製所有欄位，參考型別只複製參考。 
`深複製`：建立新的物件。

### 淺複製
#### 建立一個 類別 `Feature`
    public class Feature
    {
        public string Grade { get; set; }
        public string ChargeMode { get; set; }
    }

#### 建立一個 類別 `Robot` 繼承 `ICloneable`
    public class Robot : ICloneable
    {
        private string name;
        private string weight;
        private string color;

        Feature feature;

        public Robot(string name)
        {
            this.name = name;
            this.feature = new Feature();
        }

        public void SetRobotInfo(string weight, string color)
        {
            this.weight = weight;
            this.color = color;
        }

        public void SetRobotFeature(string grade, string chargeMode)
        {
            this.feature.Grade = grade;
            this.feature.ChargeMode = chargeMode;
        }

        public void Display()
        {
            Console.WriteLine("{0} {1} {2}", this.name, this.weight, this.color);
            Console.WriteLine("RobotFeature：{0} {1}", this.feature.Grade, this.feature.ChargeMode);
        }

        public object Clone()
        {
            return this.MemberwiseClone();
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            Robot r1 = new Robot("Mykie");
            r1.SetRobotInfo("28", "Red");
            r1.SetRobotFeature("Professional", "Wireless");

            Robot r2 = (Robot)r1.Clone();
            r2.SetRobotInfo("27", "Gold");

            Robot r3 = (Robot)r1.Clone();
            r3.SetRobotInfo("29", "Blue");

            r1.Display();
            r2.Display();
            r3.Display();
            Console.ReadKey();
        }
    }

### 深複製
#### 建立一個 類別 (Feature)
    public class Feature : ICloneable
    {
        public string Grade { get; set; }
        public string ChargeMode { get; set; }

        public object Clone()
        {
            return (object)this.MemberwiseClone();
        }
    }

#### 建立一個 類別 (Robot) 繼承 ICloneable
    public class Robot : ICloneable
    {
        private string name;
        private string weight;
        private string color;

        Feature feature;

        public Robot(string name)
        {
            this.name = name;
            this.feature = new Feature();
        }

        public Robot(Feature feature)
        {
            this.feature = (Feature)feature.Clone();
        }

        public void SetRobotInfo(string weight, string color)
        {
            this.weight = weight;
            this.color = color;
        }

        public void SetRobotFeature(string grade, string chargeMode)
        {
            this.feature.Grade = grade;
            this.feature.ChargeMode = chargeMode;
        }

        public void Display()
        {
            Console.WriteLine("{0} {1} {2}", this.name, this.weight, this.color);
            Console.WriteLine("RobotFeature：{0} {1}", this.feature.Grade, this.feature.ChargeMode);
        }

        public object Clone()
        {
            Robot obj = new Robot(this.feature);
            obj.name = this.name;
            obj.weight = this.weight;
            obj.color = this.color;
            return obj;
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            Robot r1 = new Robot("Mykie");
            r1.SetRobotInfo("28", "Red");
            r1.SetRobotFeature("Professional", "Wireless");

            Robot r2 = (Robot)r1.Clone();
            r2.SetRobotInfo("27", "Gold");
            r2.SetRobotFeature("Personal", "Wired");

            Robot r3 = (Robot)r1.Clone();
            r3.SetRobotInfo("29", "Blue");
            r3.SetRobotFeature("Professional", "Wired");

            r1.Display();
            r2.Display();
            r3.Display();
            Console.ReadKey();
        }
    }

#### 適用場景
1. 當要實例化的類別是在運行時刻指定時，例如，通過動態裝載
2. 當一個類的實例只能有幾個不同狀態組合中的一種時

#### 優點
1. 原型模式向客戶隱藏了創建新實例的複雜性
2. 原型模式允許動態增加或較少產品類
3. 原型模式簡化了實例的創建結構，工廠方法模式需要有一個與產品類等級結構相同的等級結構，而原型模式不需要這樣
4. 產品類不需要事先確定產品的等級結構，因為原型模式適用於任何的等級結構

#### 缺點
1. 每個類必須配備一個克隆方法
2. 配備克隆方法需要對類的功能進行通盤考慮，這對於全新的類不是很難，但對於已有的類不一定很容易，特別當一個類引用不支持串行化的間接對象，或者引用含有循環結構的時候

# Reference
[維基百科-原型模式](https://zh.wikipedia.org/wiki/%E5%8E%9F%E5%9E%8B%E6%A8%A1%E5%BC%8F)
[C#设计模式之五原型模式（Prototype Pattern）【创建型】](https://www.cnblogs.com/PatrickLiu/p/7640873.html)
[C#设计模式(6)——原型模式（Prototype Pattern）](https://www.cnblogs.com/zhili/p/PrototypePattern.html)