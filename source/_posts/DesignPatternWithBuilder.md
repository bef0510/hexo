---
title: 創建型模式 - 建造者模式
date: 2019-05-14 17:18:49
tags:
 - C#
 - Creational
categories: 
 - Design Pattern
---

# 建造者模式
是一種對象構建模式。它可以將複雜對象的建造過程抽象出來（抽象類別），使這個抽象過程的不同實現方法可以構造出不同表現（屬性）的對象
![Architecture](1.jpg)

#### 建立一個 產品角色 `Product`
    public class Pizza
    {
        private List<string> parts = new List<string>();

        public void Add(string part) => this.parts.Add(part);

        public void Show()
        {
            this.parts.ForEach(x => Console.WriteLine($"材料 {x} 備好了！"));

            Console.WriteLine("可以烤了！");
        }
    }

#### 建立一個 抽象建造者 `Builder`
    public abstract class PizzaBuilder
    {
        public abstract void BuildDough();
        public abstract void BuildSauce();
        public abstract void BuildTopping();
        public abstract Pizza GetPizza();
    }

#### 建立一個 具體建造者 `ConcreteBuilder` 繼承 `Builder`
    public class HawaiianPizzaBuilder : PizzaBuilder
    {
        Pizza pizza = new Pizza();
        public override void BuildDough() => this.pizza.Add("Dough cross");

        public override void BuildSauce() => this.pizza.Add("Sauce mild");

        public override void BuildTopping() => this.pizza.Add("Topping ham + pineapple");

        public override Pizza GetPizza() => this.pizza;
    }

    public class SpicyPizzaBuilder : PizzaBuilder
    {
        Pizza pizza = new Pizza();
        public override void BuildDough() => this.pizza.Add("Dough pan baked");

        public override void BuildSauce() => this.pizza.Add("Sauce hot");

        public override void BuildTopping() => this.pizza.Add("Topping pepperoni + salami");

        public override Pizza GetPizza() => this.pizza;
    }

#### 建立一個 指揮者 `Director`
    public class Waiter
    {
        public void ConstructPizza(PizzaBuilder pizzaBuilder)
        {
            pizzaBuilder.BuildDough();
            pizzaBuilder.BuildSauce();
            pizzaBuilder.BuildTopping();
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            Waiter waiter = new Waiter();
            PizzaBuilder hawaiian_pizzabuilder = new HawaiianPizzaBuilder();
            PizzaBuilder spicy_pizzabuilder = new SpicyPizzaBuilder();

            waiter.ConstructPizza(hawaiian_pizzabuilder);
            waiter.ConstructPizza(spicy_pizzabuilder);

            Pizza pizza = hawaiian_pizzabuilder.GetPizza();
            pizza.Show();

            Console.WriteLine("======================");

            Pizza pizza2 = spicy_pizzabuilder.GetPizza();
            pizza2.Show();

            Console.ReadKey();
        }
    }

#### 適用場景
1. 當創建複雜對象的算法應該獨立於該對象的組成部分以及它們的裝配方式時
2. 當構造過程必須允許被構造的對象有不同的表示時

#### 優點
1. 將產品本身與產品創建過程進行解耦，可以使用相同的創建過程來得到不同的產品。也就說細節依賴抽象
2. 將複雜產品的創建步驟分解在不同的方法中，使得創建過程更加清晰
3. 增加新的具體建造者無需修改原有類庫的代碼，易於拓展，符合"開閉原則"

#### 缺點
1. 建造者模式所創建的產品一般具有較多的共同點，其組成部分相似;如果產品之間的差異性很大，則不適合使用建造者模式，因此其使用範圍受到一定的限制
2. 如果產品的內部變化複雜，可能會導致需要定義很多具體建造者類來實現這種變化，導致系統變得很龐大

# Reference
[維基百科-生成器模式](https://zh.wikipedia.org/wiki/%E7%94%9F%E6%88%90%E5%99%A8%E6%A8%A1%E5%BC%8F)
[建造者模式(Builder Pattern)](https://www.bookstack.cn/read/Design-Pattern/lesson14-README.md)
[建造者模式（Builder Pattern）- 最易懂的设计模式解析](https://blog.csdn.net/carson_ho/article/details/54910597)