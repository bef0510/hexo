---
title: 結構型模式 - 外觀模式
date: 2019-06-05 11:12:22
tags:
 - Structural
categories: 
 - Design Pattern
---

# 外觀模式
為子系統中的一組接口提供一個統一的高層接口，使得子系統更容易使用
![Architecture](1.png)

#### 建立 子類別
    public class Audioelectronics
    {
        public void On() =>
            Console.WriteLine("Audioelectronics is On");

        public void Off() =>
            Console.WriteLine("Audioelectronics is Off");
    }

    public class PowerWindow
    {
        public void Up() =>
            Console.WriteLine("PowerWindow is Up");

        public void Down() =>
            Console.WriteLine("PowerWindow is Down");
    }

    public class SideviewMirror
    {
        public void Fold() =>
            Console.WriteLine("SideviewMirror is Fold");

        public void Unfold() =>
            Console.WriteLine("SideviewMirror is Unfold");
    }


    public class AirConditioning
    {
        public void On() => 
            Console.WriteLine("AirConditioning is On");

        public void Off() => 
            Console.WriteLine("AirConditioning is Off");

        public void SetTemperature(int temperature) => 
            Console.WriteLine($"temperature is {temperature}");
    }

#### 建立 類別
    public class ControlPanel
    {
        private readonly Audioelectronics audioelectronics;
        private readonly PowerWindow powerWindow;
        private readonly SideviewMirror sideviewMirror;
        private readonly AirConditioning airConditioning;

        public ControlPanel(Audioelectronics audioelectronics, PowerWindow powerWindow,
            SideviewMirror sideviewMirror, AirConditioning airConditioning)
        {
            this.audioelectronics = audioelectronics;
            this.powerWindow = powerWindow;
            this.sideviewMirror = sideviewMirror;
            this.airConditioning = airConditioning;
        }

        public void TurnOnEngine()
        {
            Console.WriteLine("starting...");

            audioelectronics.On();
            powerWindow.Down();
            sideviewMirror.Unfold();
            airConditioning.On();
            airConditioning.SetTemperature(23);

            Console.WriteLine("Engine is started");
        }

        public void TurnOffEngine()
        {
            Console.WriteLine("Shutting down...");

            audioelectronics.Off();
            powerWindow.Up();
            sideviewMirror.Fold();
            airConditioning.Off();

            Console.WriteLine("Engine is off");
        }
    }

#### 引用方式
    class Program
    {
        static void Main(string[] args)
        {
            ControlPanel controlPanel = new ControlPanel(new Audioelectronics(), new PowerWindow(), 
                new SideviewMirror(), new AirConditioning());

            controlPanel.TurnOnEngine();

            controlPanel.TurnOffEngine();

            Console.ReadKey();
        }
    }

#### 適用場景
1. 為一個複雜的子系統對外提供一個簡單的接口
2. 提供子系統的獨立性

#### 優點
1. 降低與子類別的耦合度，實現子類別與客戶之間的松耦合關係
2. 降低原有系統的複雜度和系統中的編譯依賴性，並簡化了系統在不同平台之間的移植過程

#### 缺點
1. 不能很好地限制客戶使用子系統類，如果對客戶訪問子系統類做太多的限制則減少了可變性和靈活性

# Reference
[維基百科-外觀模式](https://zh.wikipedia.org/wiki/%E5%A4%96%E8%A7%80%E6%A8%A1%E5%BC%8F)
[表象模式(Facade Pattern)](https://dotblogs.com.tw/pin0513/2010/11/01/18721)