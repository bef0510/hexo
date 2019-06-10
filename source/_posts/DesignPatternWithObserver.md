---
title: 行為型模式 - 觀察者模式
date: 2019-06-10 14:57:47
tags:
 - Behavioral
categories: 
 - Design Pattern
---

# 觀察者模式
一對多的依賴關系，讓多個觀察者對象同時監聽某一個主題對象
![Architecture](1.png)

#### 建立 介面
    public interface IObserver
    {
        void Update(string message);
    }
    public interface IObserverable
    {
        void Add(IObserver observer);
        void Remove(IObserver observer);
        void NotifyObservers(string content);
    }

#### 建立一個 類別 `Subject` 繼承 `IObserverable`
    public class Subject : IObserverable
    {
        List<IObserver> observers;
        public string Name = "Youtube";

        public Subject()
        {
            this.observers = new List<IObserver>();
        }

        public void Add(IObserver observer)
        {
            this.observers.Add(observer);
        }

        public void Remove(IObserver observer)
        {
            if (this.observers.IndexOf(observer) >=0)
            {
                this.observers.Remove(observer);
            }
        }

        public void NotifyObservers(string content)
        {
            foreach(IObserver observer in this.observers)
            {
                observer.Update(content);
            }
        }
    }

#### 建立一個 類別 `Customer` 繼承 `IObserver`
    public class Customer : IObserver
    {
        public string Name { get; set; }

        public Customer(string name)
        {
            this.Name = name;
        }

        public void Update(string message)
        {
            Console.WriteLine($"{this.Name} used channel from {message}");
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            Subject subject = new Subject();
            IObserver customer = new Customer("Joey");
            subject.Add(customer);

            IObserver customer1 = new Customer("Ross");
            subject.Add(customer1);
            subject.NotifyObservers("Youtube");

            Console.ReadKey();
        }
    }

#### 適用場景
1. 當對一個對象的改變需要同時改變其他對象，而又不知道具體有多少對象有待改變的情況下
2. 當一個對象必須通知其他對象，而又不能假定其他對象是誰的情況下

#### 優點
1. 可以有各種各樣不同的表示層
2. 抽象主題只依賴於抽象觀察者

#### 缺點
1. 如果某些觀察者的響應方法被阻塞，整個通知過程即被阻塞，其它觀察者不能及時被通知

# Reference
[維基百科-觀察者模式](https://zh.wikipedia.org/wiki/%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F)