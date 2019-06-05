---
title: 行為型模式 - 責任鏈模式
date: 2019-06-05 16:31:04
tags:
 - Behavioral
categories: 
 - Design Pattern
---

# 責任鏈模式
避免請求發送者與接收者耦合在一起，讓多個對象都有可能接受請求，將這些對象連接成一條鏈，並且沿著這條鏈傳遞請求，知道有對象處理它為止
![Architecture](1.png)

#### 建立一個 類別 `LeaveRequest`
    public class LeaveRequest
    {
        public string Name { get; set; }

        public int Days { get; set; }

        public LeaveRequest(string name, int days)
        {
            Name = name;
            Days = days;
        }
    }

#### 建立一個 抽象類別 `Approver`
    public abstract class Approver
    {
        public Approver NextApprover { get; set; }
        public string ApproverName { get; set; }
        public Approver(string name)
        {
            ApproverName = name;
        }

        public abstract void ProcessReQuest(LeaveRequest request);
    }

#### 建立一個 類別 `Manager` 繼承 `Approver`
    public class Manager : Approver
    {
        public Manager(string approverName) : base(approverName) { }

        public override void ProcessReQuest(LeaveRequest request)
        {
            if (request.Days <= 3)
            {
                Console.WriteLine($"{this.ApproverName} Manager approve {request.Name}'s leave");
            }
            else if (NextApprover != null)
            {
                Console.WriteLine($"{this.ApproverName} Manager approve {request.Name}'s leave");
                NextApprover.ProcessReQuest(request);
            }
        }
    }

#### 建立一個 類別 `Director` 繼承 `Approver`
    public class Director : Approver
    {
        public Director(string approverName) : base(approverName) { }

        public override void ProcessReQuest(LeaveRequest request)
        {
            if (request.Days > 3 && request.Days <= 5)
            {
                Console.WriteLine($"{this.ApproverName} Director approve {request.Name}'s leave");
            }
            else if (NextApprover != null)
            {
                Console.WriteLine($"{this.ApproverName} Director approve {request.Name}'s leave");
                NextApprover.ProcessReQuest(request);
            }
        }
    }

#### 建立一個 類別 `CEO` 繼承 `Approver`
    public class CEO : Approver
    {
        public CEO(string approverName) : base(approverName) { }

        public override void ProcessReQuest(LeaveRequest request)
        {
            if (request.Days > 5 && request.Days <= 7)
            {
                Console.WriteLine($"{this.ApproverName} CEO approve {request.Name}'s leave");
            }
            else
            {
                Console.WriteLine($"{this.ApproverName} CEO approve {request.Name}'s leave");
                Console.WriteLine($"Need running process");
            }
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            LeaveRequest ericLeave = new LeaveRequest("Rachel", 2);
            LeaveRequest joeyLeave = new LeaveRequest("Joey", 4);
            LeaveRequest monicaLeave = new LeaveRequest("Monica", 6);

            Approver manager = new Manager("Ross");
            Approver director = new Director("Chandler");
            Approver ceo = new CEO("Phoebe");

            manager.NextApprover = director;
            director.NextApprover = ceo;

            manager.ProcessReQuest(ericLeave);
            manager.ProcessReQuest(joeyLeave);
            manager.ProcessReQuest(monicaLeave);

            Console.ReadKey();
        }
    }

#### 適用場景
1. 一個系統的審批需要多個對象才能完成處理的情況下
2. 代碼中存在多個的 **if-else** 語句的情況下，此時可以考慮使用責任鏈模式來對代碼進行重構

#### 優點
1. 將職責分類隔離處理，降低耦合，符合開閉原則，不需修改原有代碼，新增一個責任類即可

#### 缺點
1. 在找到正確的處理對象之前，所有的條件判定都要執行一遍，當責任鏈過長時，可能會引起性能的問題

# Reference
[維基百科-責任鏈模式](https://zh.wikipedia.org/wiki/%E8%B4%A3%E4%BB%BB%E9%93%BE%E6%A8%A1%E5%BC%8F)
[C#设计模式之二十职责链模式（Chain of Responsibility Pattern）【行为型】](https://www.cnblogs.com/PatrickLiu/p/8109100.html)