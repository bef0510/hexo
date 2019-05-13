---
title: 設計模式 - 單例模式
date: 2019-05-13 10:03:03
tags:
 - Creational
categories: 
 - Design Pattern
---

# 單例模式
在應用這個模式時，單例對象的類必須保證只有一個實例存在。許多時候整個系統只需要擁有一個的全局對象，這樣有利於我們協調系統整體的行為

#### 單一執行緒做法
    public class Singleton
    {
        private static Singleton instance = null;

        private Singleton() { }

        public static Singleton Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new Singleton();
                }
                return instance;
            }
        }

        public static void Hello()
        {
            Console.WriteLine("Hello World!");
        }

        public void DoSomething()
        {
            Console.WriteLine("Do something!");
        }
    }

#### 多執行緒做法
    public class Singleton
    {
        private static Singleton instance = null;
        private static readonly object padlock = new object();

        private Singleton() { }

        public static Singleton Instance
        {
            get
            {
                lock (padlock)
                {
                    if(instance == null)
                    {
                        instance = new Singleton();
                    }
                    return instance;
                }
            }
        }

        public static void Hello()
        {
            Console.WriteLine("Hello World!");
        }

        public void DoSomething()
        {
            Console.WriteLine("Do something!");
        }
    }

#### .NET 4.0 前做法
    public class Singleton
    {
        private class Nested
        {
            internal static readonly Singleton instance = new Singleton();

            static Nested() { }
        }
        private Singleton() { }

        public static Singleton Instance
        {
            get
            {
                return Nested.instance;
            }
        }

        public static void Hello()
        {
            Console.WriteLine("Hello World!");
        }

        public void DoSomething()
        {
            Console.WriteLine("Do something!");
        }
    }

#### Lazy<T> 泛型做法
    public class Singleton
    {
        private static readonly Lazy<Singleton> LazyInstance = new Lazy<Singleton>(() => new Singleton());

        private Singleton() { }

        public static Singleton Instance
        {
            get
            {
                return LazyInstance.Value;
            }
        }

        public static void Hello()
        {
            Console.WriteLine("Hello World!");
        }

        public void DoSomething()
        {
            Console.WriteLine("Do something!");
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            var instance = Singleton.Instance;
            Singleton.Hello();
            instance.DoSomething();
            
            Console.ReadKey();
        }
    }

#### 適用場景
1. 需要生成唯一序列的環境
2. 需要頻繁實例化然後銷毀的對象
3. 創建對象時耗時過多或者耗資源過多，但又經常用到的對象
4. 方便資源相互通信的環境

#### 優點
1. 實現了對唯一實例訪問的可控
2. 對於一些需要頻繁創建和銷毀的對象來說可以提高系統的性能

#### 缺點
1. 不適用於變化頻繁的對象
2. 開發人員必須記住自己不能使用新的關鍵字實例化對象

# Reference
[維基百科-單例模式](https://zh.wikipedia.org/wiki/%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F)
[C# in Depth](https://csharpindepth.com/Articles/Singleton)