---
title: 行為型模式 - 備忘錄模式 
date: 2019-06-10 12:19:59
tags:
 - C#
 - Behavioral
categories: 
 - Design Pattern
---

# 備忘錄模式
在不破壞封裝的前提下，捕獲一個物件的內部狀態，並在該物件之外儲存這個狀態，這樣以後就可以把該物件恢復到原先的狀態
![Architecture](1.png)

#### 建立一個 類別 `Unit`
    public class Unit
    {
        public string ID { get; set; }
        public string Name { get; set; }
    }

#### 建立一個 類別 `OriginalObject`
    public class OriginalObject
    {
        public List<Unit> Units { get; set; }

        public OriginalObject(List<Unit> units)
        {
            this.Units = units;
        }

        public void SetMemento(Memento memento)
        {
            this.Units = memento.Units;
        }

        public Memento CreateMemento()
        {
            return new Memento(new List<Unit>(this.Units));
        }

        public void ShowUnits()
        {
            Console.WriteLine($"================Total：{Units.Count}=============================");
            foreach (Unit unit in Units)
            {
                Console.WriteLine($"部門代號：{unit.ID}，部門名稱：{unit.Name}");
            }
        }
    }

#### 建立一個 類別 `Memento`
    public class Memento
    {
        public List<Unit> Units;

        public Memento(List<Unit> units)
        {
            this.Units = units;
        }
    }

#### 建立一個 類別 `CareTaker`
    public class CareTaker
    {
        public Memento Memento { get; set; }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            List<Unit> Units = new List<Unit>()
            {
                new Unit() {ID="HR",Name = "人事" },
                new Unit() {ID="RD",Name = "研發" },
                new Unit() {ID="Sales",Name = "銷售" }
            };
            OriginalObject original = new OriginalObject(Units);

            Memento firstMemento = original.CreateMemento();
            CareTaker caretaker = new CareTaker();
            caretaker.Memento = firstMemento;
            original.ShowUnits();

            original.Units.RemoveAt(1);
            original.ShowUnits();

            original.SetMemento(caretaker.Memento);
            original.ShowUnits();

            Console.ReadKey();
        }
    }

#### 適用場景
1. 如果系統需要提供回滾操作時，使用備忘錄模式非常合適。例如文本編輯器的Ctrl + Z鍵撤銷操作的實現，數據庫中事務操作

#### 優點
1. 如果某個操作錯誤地破壞了數據的完整性，此時可以使用備忘錄模式將數據恢復成原來正確的數據

#### 缺點
1. 在實際的系統中，可能需要維護多個備份，需要額外的資源，這樣對資源的消耗比較嚴重

# Reference
[Memento pattern](https://en.wikipedia.org/wiki/Memento_pattern)
[C#设计模式(23)——备忘录模式（Memento Pattern）](https://www.cnblogs.com/zhili/p/MementoPattern.html)