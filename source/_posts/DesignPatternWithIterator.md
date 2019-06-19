---
title: 行為型模式 - 迭代器模式
date: 2019-06-06 17:15:52
tags:
 - C#
 - Behavioral
categories: 
 - Design Pattern
---

# 迭代器模式
提供一種方法順序訪問一個聚合對象中的各種元素，而又不暴露該對象的內部表示
![Architecture](1.png)

#### 建立 介面 `Iterator` 及 `IListCollection`
    public interface Iterator
    {
        bool MoveNext();

        object GetCurrent();

        void Next();

        void Reset();
    }

    public interface IListCollection
    {
        Iterator GetIterator();
    }

#### 建立一個 類別 `ConcreteList` 繼承 `IListCollection`
    public class ConcreteList : IListCollection
    {
        int[] collection;

        public ConcreteList()
        {
            this.collection = new int[] { 1, 2, 3, 4, 5, 6 };
        }

        public Iterator GetIterator() => new ConcreteIterator(this);

        public int Length => this.collection.Length;

        public int GetElement(int index) => this.collection[index];
    }

#### 建立一個 類別 `ConcreteIterator` 繼承 `Iterator`
    public class ConcreteIterator : Iterator
    {
        private ConcreteList list;
        private int index;

        public ConcreteIterator(ConcreteList list)
        {
            this.list = list;
            this.index = 0;
        }

        public bool MoveNext()
        {
            if (this.index < this.list.Length)
            {
                return true;
            }
            return false;
        }

        public object GetCurrent() => this.list.GetElement(this.index);

        public void Reset() => this.index = 0;

        public void Next()
        {
            if (this.index < this.list.Length)
            {
                this.index++;
            }
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            Iterator iterator;
            IListCollection list = new ConcreteList();
            iterator = list.GetIterator();
            int i = 0;

            while (iterator.MoveNext())
            {
                i = (int)iterator.GetCurrent();
                Console.WriteLine(i.ToString());
                iterator.Next();
            }

            Console.ReadKey();
        }
    }

#### 適用場景
1. 訪問一個聚合對象的內容而無需暴露它的內部表示
2. 支持對聚合對象的多種遍歷

#### 優點
1. 迭代器模式使得訪問一個聚合對象的內容而無需暴露它的內部表示
2. 迭代器模式為遍歷不同的集合結構提供了一個統一的接口，從而支持同樣的算法在不同的集合結構上進行操作

#### 缺點
1. 迭代器模式在遍歷的同時更改迭代器所在的集合結構會導致出現異常。所以使用的foreach語句只能在對集合進行遍歷，不能在遍歷的同時更改集合中的元素

# Reference
[維基百科-疊代器模式](https://zh.wikipedia.org/wiki/%E8%BF%AD%E4%BB%A3%E5%99%A8%E6%A8%A1%E5%BC%8F)
[C#设计模式-迭代器模式](https://blog.csdn.net/heyangyi_19940703/article/details/51330510)