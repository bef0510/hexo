---
title: 行為型模式 - 中介者模式 
date: 2019-06-10 11:23:27
tags:
 - C#
 - Behavioral
categories: 
 - Design Pattern
---

# 中介者模式
用一个中介对象来封装一系列的对象交互
![Architecture](1.png)

#### 建立 抽象類別
    public abstract class Colleague
    {
        protected Mediator mediator;
        public Colleague(Mediator mediator)
        {
            this.mediator = mediator;
        }

        public abstract void Send(string message);
        public abstract void Receive(string message);
    }

    public abstract class Mediator
    {
        public abstract void SendMessage(Colleague sender, string message);
    }

#### 建立 類別 繼承 `Colleague`
    public class ConcreteColleague1 : Colleague
    {
        public ConcreteColleague1(Mediator mediator) : base(mediator) { }

        public override void Send(string message) => this.mediator.SendMessage(this, message);

        public override void Receive(string message) => Console.WriteLine($"Colleague1 gets message: {message}");
    }

    public class ConcreteColleague2 : Colleague
    {
        public ConcreteColleague2(Mediator mediator) : base(mediator) { }

        public override void Send(string message) => this.mediator.SendMessage(this, message);

        public override void Receive(string message) => Console.WriteLine($"Colleague2 gets message: {message}");
    }

#### 建立 類別 繼承 `Mediator`
    public class ConcreteMediator : Mediator
    {
        private ConcreteColleague1 colleague1;
        private ConcreteColleague2 colleague2;
        public ConcreteColleague1 Colleague1
        {
            set { this.colleague1 = value; }
        }

        public ConcreteColleague2 Colleague2
        {
            set { this.colleague2 = value; }
        }

        public override void SendMessage(Colleague sender, string message)
        {
            if (sender == this.colleague1)
            {
                this.colleague1.Receive(message);
            }
            else if (sender == this.colleague2)
            {
                this.colleague2.Receive(message);
            }
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            ConcreteMediator mediator = new ConcreteMediator();

            ConcreteColleague1 colleague1 = new ConcreteColleague1(mediator);
            ConcreteColleague2 colleague2 = new ConcreteColleague2(mediator);

            mediator.Colleague1 = colleague1;
            mediator.Colleague2 = colleague2;

            colleague1.Send("Do the first half");
            colleague2.Send("Do the second half");

            Console.ReadKey();
        }
    }

#### 適用場景
1. 一組對象定義良好但是使用複雜的通信方式。產生的相互依賴關係結構混亂且難以理解
2. 想定制一個分佈在多個類中的行為，而又不想生成太多的子類

#### 優點
1. 提供系統的靈活性，使得各個同事對象獨立而易於复用

#### 缺點
1. 如果同事對象之間的交互非常多，而且比較複雜，當這些複雜性全都集中到中介者的時候，會導致中介者對象變的十分複雜，而且難於維護和管理

# Reference
[維基百科-中介者模式](https://zh.wikipedia.org/wiki/%E4%B8%AD%E4%BB%8B%E8%80%85%E6%A8%A1%E5%BC%8F)
[Mediator（中介者）](https://www.cnblogs.com/gaochundong/p/design_pattern_mediator.html)