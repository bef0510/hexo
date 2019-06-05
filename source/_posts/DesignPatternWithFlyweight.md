---
title: 結構型模式 - 享元模式
date: 2019-06-05 14:51:14
tags:
 - Structural
categories: 
 - Design Pattern
---

# 享元模式
通過共用以便有效的支援大量小顆粒物件
![Architecture](1.png)

#### 建立 類別
    public abstract class Card
    {
        public abstract string ShowCard(string num);
    }

    public class Spade : Card
    {
        public override string ShowCard(string num) => $"Spade  ：{num}";
    
    }

    public class Heart : Card
    {
        public override string ShowCard(string num) => $"Heart  ：{num}";
    }

    public class Diamond : Card
    {
        public override string ShowCard(string num) => $"Diamond：{num}";
    }

    public class Club : Card
    {
        public override string ShowCard(string num) => $"Club   ：{num}";
    }

    public class Poker
    {
        public Card Card { get; set; }
        public string Number { get; set; }
    }

#### 建立 工廠模式
    public class PokerFactory
    {
        public static Dictionary<int, Card> pokers = new Dictionary<int, Card>();

        public static Card GetPoker(int color)
        {
            if (pokers.ContainsKey(color))
            {
                return pokers[color];
            }else
            {
                Card card;
                switch (color)
                {
                    case 0:
                        card = new Spade();
                        break;
                    case 1:
                        card = new Heart();
                        break;
                    case 2:
                        card = new Diamond();
                        break;
                    case 3:
                        card = new Club();
                        break;
                    default:
                        card = new Spade();
                        break;
                }
                pokers.Add(color, card);
                return pokers[color];
            }
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            Card card;
            string poker = string.Empty;
            Random random = new Random();

            for (int i = 0; i < 5; i++)
            {
                int colorRandom = random.Next(4) + 1;
                card = PokerFactory.GetPoker(colorRandom);

                if (card != null)
                {
                    int numRandom = random.Next(13) + 1;
                    switch (numRandom)
                    {
                        case 1:
                            poker = card.ShowCard("A");
                            break;
                        case 11:
                            poker = card.ShowCard("J");
                            break;
                        case 12:
                            poker = card.ShowCard("Q");
                            break;
                        case 13:
                            poker = card.ShowCard("K");
                            break;
                        default:
                            poker = card.ShowCard(numRandom.ToString());
                            break;
                    }

                    Console.WriteLine(poker);
                }
            }

            Console.ReadKey();
        }
    }

#### 適用場景
1. 系統有大量相同或者相似的對象，消耗大量內存

#### 優點
1. 減少對象的創建，降低系統的內存，使效率提高

#### 缺點
1. 使得系統變得複雜，需要分離出內部狀態和外部狀態，這使得程序的邏輯複雜化

# Reference
[一起学设计模式 - 享元模式](https://segmentfault.com/a/1190000012037596)
