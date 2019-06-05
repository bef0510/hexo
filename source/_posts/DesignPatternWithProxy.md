---
title: 結構型模式 - 代理模式
date: 2019-06-05 15:32:48
tags:
 - Structural
categories: 
 - Design Pattern
---

# 代理模式
為其他對象提供一種代理以控制對這個對象的訪問
![Architecture](1.png)

#### 建立一個 介面 `IGiveGift`
    public interface IGiveGift
    {
        void GiveDolls();
        void GiveMoneyFlowers();
        void GiveChocolate();
    }

#### 建立一個 類別 `Favorite`
    public class Favorite
    {
        public string Name { get; set; }
    }

#### 建立一個 類別 `Pursuit` 繼承 `IGiveGift`
    public class Pursuit : IGiveGift
    {
        Favorite favorite;

        public Pursuit(Favorite favorite)
        {
            this.favorite = favorite;
        }

        public void GiveDolls() => Console.WriteLine($"送 {favorite.Name} 洋娃娃");
        public void GiveMoneyFlowers() => Console.WriteLine($"送 {favorite.Name} 錢摺的花");
        public void GiveChocolate() => Console.WriteLine($"送 {favorite.Name} 巧克力");
    }

#### 建立一個 代理類別 `FriendProxy` 繼承 `IGiveGift`
    public class FriendProxy : IGiveGift
    {
        Pursuit pursuit;

        public FriendProxy(Favorite favorite)
        {
            pursuit = new Pursuit(favorite);
        }

        public void GiveDolls() => pursuit.GiveDolls();
        public void GiveMoneyFlowers() => pursuit.GiveMoneyFlowers();
        public void GiveChocolate() => pursuit.GiveChocolate();
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            FriendProxy proxy = new FriendProxy(new Favorite() { Name = "SomeOne"});

            proxy.GiveDolls();
            proxy.GiveMoneyFlowers();
            proxy.GiveChocolate();

            Console.ReadKey();
        }
    }

#### 適用場景
1. 需要為一個對象再不同的地址空間提供局部的代表時

#### 優點
1. 協調調用者和被調用者，降低了系統的耦合度
2. 代理對象作為客戶端和目標對象之間的中介，起到了保護目標對象的作用

#### 缺點
1. 由於在客戶端和真實主題之間增加了代理對象，因此會造成請求的處理速度變慢
2. 實現代理模式需要額外的工作，從而增加了系統實現的複雜度

# Reference
[維基百科-代理模式](https://zh.wikipedia.org/wiki/%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F)
[[設計模式] 代理模式 (Proxy Pattern)](https://dotblogs.com.tw/atowngit/2010/03/09/13956)
[C#设计模式(13)——代理模式（Proxy Pattern）](https://www.cnblogs.com/zhili/p/ProxyPattern.html)
