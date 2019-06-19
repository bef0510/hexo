---
title: 行為型模式 - 解釋器模式
date: 2019-06-06 16:01:16
tags:
 - C#
 - Behavioral
categories: 
 - Design Pattern
---

# 解釋器模式
定義一個語言的文法，並且建立一個解釋器來解釋該語言中的句子
![Architecture](1.png)

#### 建立一個 類別 `Context`
    public class Context
    {
        public string Input { get; set; }
        public int Output { get; set; }

        public Context(string input)
        {
            this.Input = input;
        }
    }

#### 建立一個 抽象類別 `Expression`
    public abstract class Expression
    {
        public void Interpret(Context context)
        {
            if (context.Input.Length == 0)
            {
                return;
            }
                
            if (context.Input.StartsWith(Nine()))
            {
                context.Output += (9 * Multiplier());
                context.Input = context.Input.Substring(2);
            }
            else if (context.Input.StartsWith(Four()))
            {
                context.Output += (4 * Multiplier());
                context.Input = context.Input.Substring(2);
            }
            else if (context.Input.StartsWith(Five()))
            {
                context.Output += (5 * Multiplier());
                context.Input = context.Input.Substring(1);
            }

            while (context.Input.StartsWith(One()))
            {
                context.Output += (1 * Multiplier());
                context.Input = context.Input.Substring(1);
            }
        }

        public abstract string One();
        public abstract string Four();
        public abstract string Five();
        public abstract string Nine();
        public abstract int Multiplier();
    }

#### 建立一個 類別 `ThousandExpression` 繼承 `Expression`
    public class ThousandExpression : Expression
    {
        public override string One() { return "M"; }
        public override string Four() { return " "; }
        public override string Five() { return " "; }
        public override string Nine() { return " "; }
        public override int Multiplier() { return 1000; }
    }

#### 建立一個 類別 `HundredExpression` 繼承 `Expression`
    public class HundredExpression : Expression
    {
        public override string One() { return "C"; }
        public override string Four() { return "CD"; }
        public override string Five() { return "D"; }
        public override string Nine() { return "CM"; }
        public override int Multiplier() { return 100; }
    }

#### 建立一個 類別 `TenExpression` 繼承 `Expression`
    public class TenExpression : Expression
    {
        public override string One() { return "X"; }
        public override string Four() { return "XL"; }
        public override string Five() { return "L"; }
        public override string Nine() { return "XC"; }
        public override int Multiplier() { return 10; }
    }

#### 建立一個 類別 `OneExpression` 繼承 `Expression`
    public class OneExpression : Expression
    {
        public override string One() { return "I"; }
        public override string Four() { return "IV"; }
        public override string Five() { return "V"; }
        public override string Nine() { return "IX"; }
        public override int Multiplier() { return 1; }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            string roman = "MCMXXVIII";
            Context context = new Context(roman);

            List<Expression> tree = new List<Expression>();
            tree.Add(new ThousandExpression());
            tree.Add(new HundredExpression());
            tree.Add(new TenExpression());
            tree.Add(new OneExpression());

            foreach (Expression exp in tree)
            {
                exp.Interpret(context);
            }

            Console.WriteLine($"{roman} = {context.Output}");

            Console.ReadKey();
        }
    }

#### 適用場景
1. 一些重複發生的問題，比如加減乘除四則運算，數字、中英文轉換

#### 優點
1. 易於改變和擴展文法
2. 每一條文法規則都可以表示為一個類，因此可以方便地實現一個簡單的語言

#### 缺點
1. 對於複雜文法難以維護
2. 執行效率較低

# Reference
[Interpreter pattern](https://en.wikipedia.org/wiki/Interpreter_pattern)
[Interpreter](https://www.dofactory.com/net/interpreter-design-pattern)