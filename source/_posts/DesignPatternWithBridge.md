---
title: 結構型模式 - 橋接模式
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

#### 定義一個 抽象類別 廠牌 `CarBase`

    public abstract class CarBase
    {
        public abstract void GetCar(IColor color);
    }

#### 定義一個 介面 提供 取得顏色方法 `IColor`

    public interface IColor
    {
        string GetColor();
    }

#### 實作 各顏色，繼承 `IColor`

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

#### 實作 各廠牌，繼承 `CarBase`

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

#### 引用方式

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

#### 適用場景
1. 系統需要在抽象化和具體化之間增加更多的靈活性，避免在兩個層次之間建立靜態的繼承關係，通過橋接模式可以使它們在抽象層建立一個關聯關係
2. "抽象部分" 和 "實現部分" 可以以繼承的方式獨立擴展而互不影響，在程序運行時可以動態將一個抽象化子類的對象和一個實現化子類的對象進行組合，即系統需要對抽象化角色和實現化角色進行動態耦合
3. 一個類存在兩個（或多個）獨立變化的維度，且這兩個（或多個）維度都需要獨立進行擴展
4. 對於那些不希望使用繼承或因為多層繼承導致系統類的個數急劇增加的系統，橋接模式尤為適用

#### 優點:
1. 分離抽象接口及其實現部分。
2. 橋接模式有時類似於多繼承方案，但是多繼承方案違背了類的單一職責原則（即一個類只有一個變化的原因），復用性比較差，而且多繼承結構中類的個數非常龐大，橋接模式是比多繼承方案更好的解決方法。
3. 橋接模式提高了系統的可擴充性，在兩個變化維度中任意擴展一個維度，都不需要修改原有系統。
4. 實現細節對客戶透明，可以對用戶隱藏實現細節

#### 缺點
1. 橋接模式的引入會增加系統的理解與設計難度，由於聚合關聯hＨ關係建立在抽象層，要求開發者針對抽象進

# Reference
[維基百科-橋接模式](https://zh.wikipedia.org/wiki/%E6%A9%8B%E6%8E%A5%E6%A8%A1%E5%BC%8F)
[大話設計模式-橋接模式](https://kknews.cc/tech/8m2kv9g.html)