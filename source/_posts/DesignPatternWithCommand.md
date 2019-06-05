---
title: 行為型模式 - 命令模式
date: 2019-06-05 17:47:15
tags:
 - Behavioral
categories: 
 - Design Pattern
---

# 命令模式
將一個請求封裝為一個對象，從而使我們可用不同的請求對客戶進行參數化;對請求排隊或者記錄請求日誌，以及支持可撤銷的操作命令模式是一種對象行為型模式
![Architecture](1.png)

#### 建立一個 類別 `Barbecuer`
    // Receiver
    public class Barbecuer
    {
        public void BakeMutton() => Console.WriteLine("烤羊肉！");

        public void BakeChickenWing() => Console.WriteLine("烤雞翅！");
    }

#### 建立一個 抽象類別 `Command`
    // Command
    public abstract class Command
    {
        protected Barbecuer receiver;

        public Command(Barbecuer receiver)
        {
            this.receiver = receiver;
        }

        public abstract void ExecuteCommand();
    }

#### 建立 類別 繼承 `Command`
    // ConcreteCommand
    public class BakeMuttonCommand : Command
    {
        public BakeMuttonCommand(Barbecuer receiver) : base(receiver) { }

        public override void ExecuteCommand() => this.receiver.BakeMutton();
    }

    public class BakeChickenWingCommand : Command
    {
        public BakeChickenWingCommand(Barbecuer receiver) : base(receiver) { }

        public override void ExecuteCommand() => this.receiver.BakeMutton();
    }

#### 建立一個 類別 `Waiter`
    // Invoker
    public class Waiter
    {
        private IList<Command> orders = new List<Command>();

        public void SetOrder(Command command)
        {
            if (command.ToString() == "CommandPattern.BakeChickenWingCommand")
            {
                Console.WriteLine("服務員：雞翅沒有了，請點別的燒烤。");
            }
            else
            {
                this.orders.Add(command);
                Console.WriteLine($"增加訂單：{command.ToString()}，時間：{DateTime.Now.ToString()}");
            }
        }

        public void CancelOrder(Command command)
        {
            this.orders.Remove(command);
            Console.WriteLine($"取消訂單：{command.ToString()}，時間：{DateTime.Now.ToString()}");
        }

        public void Notify()
        {
            foreach(Command cmd in orders)
            {
                cmd.ExecuteCommand();
            }
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            Barbecuer boy = new Barbecuer();
            Command bakeMuttonCommand1 = new BakeMuttonCommand(boy);
            Command bakeMuttonCommand2 = new BakeMuttonCommand(boy);
            Command bakeChikenWingCommand1 = new BakeChickenWingCommand(boy);

            Waiter girl = new Waiter();

            girl.SetOrder(bakeMuttonCommand1);
            girl.SetOrder(bakeMuttonCommand2);
            girl.SetOrder(bakeChikenWingCommand1);

            girl.Notify();

            Console.ReadKey();
        }
    }

#### 適用場景
1. 系統需要將請求調用者和請求接收者解耦，使得調用者和接收者不直接交互
2. 系統需要支持命令的撤銷 **Undo** 操作和恢復 **Redo** 操作
3. 系統需要將一組操作組合在一起，即支持宏命令

#### 優點
1. 可以方便地實現對請求的 **Undo** 和 **Redo**
2. 新的命令可以很容易地加入到系統中

#### 缺點
1. 用命令模式可能會導致某些系統有過多的具體命令類。因為針對每一個命令都需要設計一個具體命令類，因此某些系統可能需要大量具體命令類，這將影響命令模式的使用

# Reference
[維基百科-命令模式](https://zh.wikipedia.org/wiki/%E5%91%BD%E4%BB%A4%E6%A8%A1%E5%BC%8F)
[深入浅出设计模式——命令模式（Command Pattern）](https://blog.csdn.net/iamoldpan/article/details/78911846)