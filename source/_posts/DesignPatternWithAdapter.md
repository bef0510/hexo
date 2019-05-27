---
title: 結構型模式 - 適配器模式
date: 2019-05-15 17:47:53
tags:
 - Structural
categories: 
 - Design Pattern
---

# 適配器模式
將一個類別的介面轉接成用戶所期待的。一個適配使得因介面不相容而不能在一起工作的類別能在一起工作，做法是將類別自己的介面包裹在一個已存在的類別中
![Architecture](1.jpg)

#### 建立一個 抽象類別 `Target`
    public abstract class Target
    {
        public abstract void Request();
    }

#### 建立一個 類別 `Adaptee`
    public class Adaptee
    {
        public void SepecificRequest() => Console.WriteLine("執行要適配器的特殊請求方法");
    }

#### 建立一個 類別 `Adapter` 繼承 `Target`
    public class Adapter : Target
    {
        private Adaptee adaptee;

        public override void Request()
        {
            if(this.adaptee == null)
            {
                this.adaptee = new Adaptee();
            }
            this.adaptee.SepecificRequest();
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            Target target = new Adapter();
            target.Request();

            Console.ReadKey();
        }
    }

#### 適用場景
1. 系統需要复用現有類，而該類的接口不符合系統的需求
2. 想要建立一個可重複使用的類，用於與一些彼此之間沒有太大關聯的一些類，包括一些可能在將來引進的類一起工作

#### 優點
1. 可以在不修改原有代碼的基礎上來復用現有類，很好地符合“開閉原則”（這點是兩種實現方式都具有的）
2. 採用"對象組合"的方式，更符合松耦合

#### 缺點
1. 使得重定義適配者的行為較困難，這就需要生成適配者的子類並且使得適配器引用這個子類而不是引用適配者本身

# Reference
[維基百科-配接器模式](https://zh.wikipedia.org/wiki/%E9%80%82%E9%85%8D%E5%99%A8%E6%A8%A1%E5%BC%8F)
[C#设计模式(7)——适配器模式（Adapter Pattern）](https://www.cnblogs.com/zhili/p/AdapterPattern.html)
[C# 适配器模式和适配器模式实例（两个应用实例）](https://blog.csdn.net/EmmaGood/article/details/7835961)