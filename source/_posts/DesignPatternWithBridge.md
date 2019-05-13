---
title: 設計模式 - 橋接模式
date: 2019-05-10 15:56:02
tags:
 - Structural
categories: 
 - Design Pattern
---

# 橋接模式
將一個抽象與實現解耦，以便兩者可以獨立的變化

    各種車子，有不同顏色
    廠牌：BMW、BENZ、Toyota...
    顏色：紅、藍、黑
![Architecture](2.png)

定義一個 抽象類別 讓各廠牌繼承

    public abstract class CarBase
    {
        public abstract void GetCar(IColor color);
    }

定義一個 介面 提供 取得顏色方法

    public interface IColor
    {
        string GetColor();
    }

實作 各顏色，繼承 `IColor`

    public class BlueColor : IColor
    {
        public string GetColor()
        {
            return "Blue";
        }
    }

    public class RedColor : IColor
    {
        public string GetColor()
        {
            return "Red";
        }
    }

實作 各廠牌，繼承 `CarBase`

    public class Toyota : CarBase
    {
        public override void GetCar(IColor color)
        {
            Console.WriteLine($"It's Toyota, color is {color.GetColor()}");
        }
    }

    public class BMW : CarBase
    {
        public override void GetCar(IColor color)
        {
            Console.WriteLine($"It's BMW, color is {color.GetColor()}");
        }
    }

呼叫方式

    class Program
    {
        static void Main(string[] args)
        {
            Toyota t = new Toyota();
            t.GetCar(new BlueColor());
            t.GetCar(new RedColor());

            BMW b = new BMW();
            b.GetCar(new BlueColor());
            b.GetCar(new RedColor());

            Console.ReadKey();
        }
    }

#### 優點:

- 分離抽象接口及其實現部分。

- 橋接模式有時類似於多繼承方案，但是多繼承方案違背了類的單一職責原則（即一個類只有一個變化的原因），復用性比較差，而且多繼承結構中類的個數非常龐大，橋接模式是比多繼承方案更好的解決方法。

- 橋接模式提高了系統的可擴充性，在兩個變化維度中任意擴展一個維度，都不需要修改原有系統。

- 實現細節對客戶透明，可以對用戶隱藏實現細節

#### 缺點

- 橋接模式的引入會增加系統的理解與設計難度，由於聚合關聯hＨ關係建立在抽象層，要求開發者針對抽象進

# Reference
[大話設計模式-橋接模式](https://kknews.cc/tech/8m2kv9g.html)