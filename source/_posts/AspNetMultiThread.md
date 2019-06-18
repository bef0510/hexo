---
title: C# 多執行緒 演變
date: 2019-05-28 14:50:21
tags:
 - Thread
categories: 
 - ASP.Net
 - C#
---

# C# 多執行緒
#### C# 1.0 具名
    public class MultiThread10
    {
        private int itemCount = 0;
        private object locker = new object();
        public void Run()
        {
            Thread t = new Thread(Work);
            t.Start(300);
           
            Thread t1 = new Thread(Work);
            t1.Start(100);
        }

        private void Work(object delay)
        {
            lock (locker)
            {
                this.itemCount++;

                Thread.Sleep((int)delay);

                Console.WriteLine($"C# 1.0 Items in cart：{itemCount}");
            }
        }
    }

#### C# 2.0 匿名
    public class MultiThread20
    {
        private int itemCount = 0;
        private object locker = new object();

        public void Run()
        {
            Thread t = new Thread(delegate (object delay)
            {
                lock (locker)
                {
                    this.itemCount++;

                    Thread.Sleep((int)delay);

                    Console.WriteLine($"C# 2.0 Items in cart：{itemCount}");
                }
            });

            t.Start(300);

            Thread t1 = new Thread(delegate (object delay)
            {
                lock (locker)
                {
                    this.itemCount++;

                    Thread.Sleep((int)delay);

                    Console.WriteLine($"C# 2.0 Items in cart：{itemCount}");
                }
            });

            t1.Start(100);
        }
    }

#### C# 3.0 Lambda
    public class MultiThread30
    {
        private int itemCount = 0;
        private object locker = new object();

        public void Run()
        {
            Thread t = new Thread((object delay) =>
            {
                lock (locker)
                {
                    this.itemCount++;

                    Thread.Sleep((int)delay);

                    Console.WriteLine($"C# 3.0 Items in cart：{itemCount}");
                }
            });

            t.Start(300);

            Thread t1 = new Thread((object delay) =>
            {
                lock (locker)
                {
                    this.itemCount++;

                    Thread.Sleep((int)delay);

                    Console.WriteLine($"C# 3.0 Items in cart：{itemCount}");
                }
            });

            t1.Start(100);
        }
    }

#### C# 4.0 Task
    public class MultiThread40
    {
        private int itemCount = 0;
        private object locker = new object();

        public void Run()
        {
            Task t = new Task(() =>
            {
                lock (locker)
                {
                    this.itemCount++;

                    Thread.Sleep(300);

                    Console.WriteLine($"C# 4.0 Items in cart：{itemCount}");
                }
            });

            t.Start();

            Task t1 = new Task(() =>
            {
                lock (locker)
                {
                    this.itemCount++;

                    Thread.Sleep(100);

                    Console.WriteLine($"C# 4.0 Items in cart：{itemCount}");
                }
            });

            t1.Start();
        }
    }

#### C# 4.0 Async/Await
    public class MultiThreadAync
    {
        private int itemCount = 0;
        private object locker = new object();

        public void Run()
        {
            T1();
            T2();
        }

        public async void T1()
        {
            await Task.Delay(3000);

            lock (locker)
            {
                this.itemCount++;

                Console.WriteLine($"C# Aync Items in cart：{itemCount}");
            }
        }

        public async void T2()
        {
            await Task.Delay(1000);

            lock (locker)
            {
                this.itemCount++;

                Console.WriteLine($"C# Aync Items in cart：{itemCount}");
            }
        }
    }

# Reference
[C# 學習筆記：多執行緒 (2) - 分道揚鑣](https://www.huanlintalk.com/2013/05/csharp-notes-multithreading-2.html)